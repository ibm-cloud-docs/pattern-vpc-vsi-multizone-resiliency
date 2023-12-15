---

copyright:
  years: 2023
lastupdated: "2023-12-14"

keywords: # Not typically populated

subcollection: pattern-vpc-vsi-multizone-resiliency

authors:
  - name: Carol Hernandez
    url: https://linkedin.com/in/carolbhernandez
  - name: DoYoung Im
    url: https://www.linkedin.com/in/doyoung-im-b13bab20/

# The release that the reference architecture describes
version: 1.0

# Use if the reference architecture has deployable code.
# Value is the URL to land the user in the IBM Cloud catalog details page for the deployable architecture.
# See https://test.cloud.ibm.com/docs/get-coding?topic=get-coding-deploy-button
deployment-url:

docs: https://cloud.ibm.com/docs/pattern-vpc-vsi-multizone-resiliency

# use-case from 'code' column in
# https://github.ibm.com/digital/taxonomy/blob/main/topics/topics_flat_list.csv
use-case: VirtualPrivateCloud

content-type: reference-architecture

---

{{site.data.keyword.attribute-definition-list}}

# Web app multi-zone resiliency
{: #web-app-multi-zone}
{: toc-content-type="reference-architecture"}
{: toc-use-case="VirtualPrivateCloud"}
{: toc-version="1.0"}

The web app multi-zone resiliency architecture deploys a 3-tier web application on Virtual Servers for VPC using compute, storage, and network cloud resources as well as other Cloud Services provisioned across multiple availability zones within a single region.

## Architecture diagram
{: #architecture-diagram}

![Web app multi-zone resiliency solution architecture](web-app-multi-zone-architecture.png) [Web app multi-zone resiliency solution architecture]
{: caption="Figure 1. Web app multi-zone resiliency solution architecture" caption-side="bottom"}

The Web, Application, and Database tiers are deployed on Virtual Servers for VPC (VSIs) across two availability zones within the Workload Virtual Private Cloud (VPC).
- The virtual servers in the Web and App tiers are placed within [Placement Groups](https://cloud.ibm.com/docs/vpc?topic=vpc-about-placement-groups-for-vpc&interface=ui) for host failure protection and are part of [Instance Groups](https://cloud.ibm.com/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=ui) for autoscaling. A [VPC Application Load Balancer](https://cloud.ibm.com/docs/vpc?topic=vpc-load-balancers) is used to route traffic to healthy application servers.
- The database servers are deployed in active-standby mode. Data replication across availability zones is handled by the database software based on database specific high availability configuration options.
- IBM Storage Protect is used to create database backups to enable data recovery.

All data is encrypted using customer-provided keys managed by [Key Protect](https://cloud.ibm.com/docs/key-protect?topic=key-protect-about).
- All storage is encrypted at rest using storage encryption with customer-provided keys managed by Key Protect. Key Protect is provisioned in the primary region and configured with failover units in the second region.
- Data is encrypted in transit using TLS encryption. A [Secrets Manager](https://cloud.ibm.com/catalog/services/secrets-manager) instance is deployed in each region to store and manage SSL/TLS certificates.
- The [Cloud Internet Services (CIS)](https://cloud.ibm.com/docs/cis?topic=cis-getting-started) is deployed as a proxy to the public VPC Application Load Balancer that front ends the web tier to provide Distributed Denial of Service (DDoS) protection and Web Application Firewall protection to the web servers exposed to the internet.

## Design scope
{: #design-scope}

Following the [Architecture Framework](https://cloud.ibm.com/docs/architecture-framework?topic=architecture-framework-intro)\*, the web app multi-zone resiliency architecture covers design considerations and architecture decisions for the following aspects and domains:

- **Compute:** Virtual Servers

- **Storage:** Primary Storage, Backup Storage

- **Networking:** Enterprise Connectivity, Segmentation and Isolation, Cloud Native Connectivity, Load Balancing, DNS

- **Security:** Data Security, Identity and Access Management, Application Security, Infrastructure and Endpoint Security

- **Resiliency:** High Availability, Backup and Restore,

- **Service Management:** Monitoring, Logging, Auditing, Alerting

 ![Web app multi-zone resiliency architecture design scope](heat-map-vpc-multi-zone.svg){: caption="Figure 1. Web app multi-zone resiliency architecture design scope" caption-side="bottom"}

\*The Architecture Framework provides a consistent approach to design cloud solutions by addressing requirements across a set of "aspects" and "domains", which are technology-agnostic architectural areas that need to be considered for any enterprise solution. See [Introduction to the Architecture Framework](https://cloud.ibm.com/docs/architecture-framework?topic=architecture-framework-intro) for more details.

## Requirements
{: #requirements}

| Aspects | Requirements |
| -------------- | -------------- |
| Compute            | Provide properly isolated compute resources with adequate compute capacity for the applications. |
| Storage            | Provide storage that meets the application and database performance requirements. |
| Networking         | Deploy workloads in isolated environment and enforce information flow policies. \n Provide secure, encrypted connectivity to the cloud’s private network for management purposes. \n Distribute incoming application requests across available compute resources. \n Support failover of application to alternate site in the event of planned or unplanned outages \n Provide public and private DNS resolution to support use of hostnames instead of IP addresses. |
| Security           | Ensure all operator actions are executed securely through a bastion host. \n Protect the boundaries of the application against denial-of-service and application-layer attacks. \n Encrypt all application data in transit and at rest to protect from unauthorized disclosure. \n Encrypt all backup data to protect from unauthorized disclosure. \n Encrypt all security data (operational and audit logs) to protect from unauthorized disclosure. \n Encrypt all data using customer managed keys to meet regulatory compliance requirements for additional security and customer control. \n Protect secrets through their entire lifecycle and secure them using access control measures. |
| Resiliency         | Support application availability targets and business continuity policies. \n Ensure availability of the application in the event of planned and unplanned outages. \n Provide highly available compute, storage, network, and other cloud services to handle application load and performance requirements. \n Backup application data to enable recovery in the event of unplanned outages. \n Provide highly available storage for security data (logs) and backup data. |
| Service Management | Monitor system and application health metrics and logs to detect issues that might impact the availability of the application. \n Generate alerts/notifications about issues that might impact the availability of applications to trigger appropriate responses to minimize down time. \n Monitor audit logs to track changes and detect potential security problems. \n Provide a mechanism to identify and send notifications about issues found in audit logs. |
{: caption="Table 1. Web app multi-zone resiliency requirements" caption-side="bottom"}

## Components
{: #components}

| Aspects | Solution Components | How the component is used |
| -------------- | -------------- | -------------- |
| Compute            | [Virtual Servers for VPC](https://cloud.ibm.com/vpc-ext/provision/vs)                                                                                                                                                                   | Web, App, and database servers                                                                                                        |
| Storage            | [Block Storage for VPC](https://cloud.ibm.com/docs/openshift?topic=openshift-vpc-block)                                                                                                                                      | Database servers storage                                                                                                              |
|                    | [Object Storage (COS)](https://cloud.ibm.com/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage)                                                                                      | Web app static content, backups, logs (application, operational and audit logs)                                                       |
| Networking         | [VPC Virtual Private Network (VPN) Client](https://cloud.ibm.com/docs/iaas-vpn?topic=iaas-vpn-getting-started)                                                                                                           | Remote access to manage resources in private network                                                                                  |
|                    | [Virtual Private Clouds (VPCs), Subnets, Security Groups (SGs), ACLs](https://cloud.ibm.com/docs/vpc?topic=vpc-getting-started)                                                                                          | VPCs for workload isolation Subnets, SGs, and ACLs for restricted access to web, app, and database tiers                              |
|                    | [Local Transit Gateway (TGW)](https://cloud.ibm.com/docs/transit-gateway?topic=transit-gateway-getting-started)                                                                                                                | . |
|                    | [Virtual Private Gateway & Virtual Private Endpoint (VPE)](https://cloud.ibm.com/docs/vpc?topic=vpc-about-vpe)                                                                                                           | Private network access to Cloud Services, e.g. Key Protect, COS, etc.                                                                 |
|                    | [VPC Application Load Balancer](https://cloud.ibm.com/docs/vpc?topic=vpc-load-balancers)                                                                                                                                 | Application Load Balancing for web and app tiers                                                                                      |
|                    | [Public Gateway](https://cloud.ibm.com/docs/vpc?topic=vpc-about-networking-for-vpc&interface=cli#public-gateway-for-external-connectivity)                                                                               | Web app access to the internet                                                                                                        |
|                    | [Cloud Internet Services (CIS)](https://cloud.ibm.com/docs/cis?topic=cis-getting-started)                                                                                                                                | Public DNS resolution.                                                                         |
|                    | [DNS Services](https://cloud.ibm.com/docs/dns-svcs?topic=dns-svcs-about-dns-services)                                                                                                                                    | Private DNS resolution                                                                                                                |
| Security           | [IAM](https://cloud.ibm.com/docs/account?topic=account-cloudaccess)                                                                                                                                                      | IBM Cloud Identity & Access Management                                                                                                |
|                    | [BYO Bastion Host on VPC VSI with PAM SW](https://cloud.ibm.com/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-bastion-tutorial-teleport)                            | Remote access with Privileged Access Management                                                                                       |
|                    | [Cloud Internet Services (CIS)](https://cloud.ibm.com/docs/cis?topic=cis-getting-started)                                                                                                                                | DDoS protection and Web App Firewall                                                                                                  |
|                    | [Key Protect](https://cloud.ibm.com/docs/key-protect?topic=key-protect-about)                                                                                                                                            | Key Management Service                                                                                                                |
|                    | [Secrets Manager](https://cloud.ibm.com/catalog/services/secrets-manager)                                                                                                                                                | Certificate and Secrets Management                                                                                                    |
| Resiliency         | [Placement Groups](https://cloud.ibm.com/docs/vpc?topic=vpc-about-placement-groups-for-vpc&interface=ui) and [Instance Groups](https://cloud.ibm.com/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=ui) | To avoid single points of failure and adjust capacity based on load changes                                                           |
|                    | VPC VSIs, VPC Block across multiple zones in one region                                                                                                                                                                 | Web, app, database high availability deployment                                                                            |
|                    | [IBM Storage Protect](https://cloud.ibm.com/catalog/content/SPonIBMCloud-20c54034-d319-48c0-beb6-0b4adc54265c-global)                                                                                                    | Database backups                                                                                                                      |
|                    | [Cross-Region COS Buckets](https://cloud.ibm.com/docs/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints-geo)                                                                                    | Backup storage                                                                                                                        |
| Service Management | [IBM Cloud Monitoring](https://cloud.ibm.com/docs/monitoring?topic=monitoring-about-monitor)                                                                                                                             | Apps and operational monitoring.                                                                                                      |
|                    | [IBM Log Analysis](https://cloud.ibm.com/docs/log-analysis?topic=log-analysis-getting-started)                                                                                                                           | Apps and operational logs                                                                                                             |
|                    | [IBM Cloud Activity Tracker](https://cloud.ibm.com/docs/activity-tracker?topic=activity-tracker-getting-started)                                                                                                         | Audit logs                                                                                                                            |
{: caption="Table 2. Web app multi-zone resiliency components" caption-side="bottom"}
