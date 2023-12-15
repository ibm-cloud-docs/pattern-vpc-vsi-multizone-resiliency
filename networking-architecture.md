---

copyright:
  years: 2023
lastupdated: "2023-12-15"

subcollection: pattern-vpc-vsi-multizone-resiliency

keywords:

---

# Architecture decisions for networking
{: #networking-architecture}

The following tables summarize the networking architecture decisions for the web app multi-zone resiliency pattern.

## Architecture decisions for enterprise connectivity
{: #enterprise-connectivity}

The following are architecture decisions for enterprise connectivity for this design.

| Architecture decision | Requirement | Alternative | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Connectivity for management | Provide secure, encrypted connectivity to the cloud’s private network for management purposes. | * Client VPN for VPC \n * VPN for VPC | Client VPN for VPC | Client VPN for VPC provides client-to-site connectivity, which allows remote devices to securely connect to the VPC network that uses an OpenVPN software client. |
{: caption="Table 1. Enterprise connectivity architecture decisions" caption-side="bottom"}

## Architecture decisions for network segmentation and isolation
{: #network-segmentation-isolation}

The following are network segmentation and isolation architecture decisions for this design.

| Architecture decision | Requirement | Alternative | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Web App Deployment | * Deploy workloads in an isolated environment and enforce information flow policies. \n * Provide isolated security zones between app tiers	- Virtual Private Clouds (VPCs) | * Virtual Private Clouds (VPCs) \n * Subnets \n * Security Groups (SGs) \n * ACLs | VPCs, subnets, Security Groups (SGs) and ACLs | VPCs provide secure, virtual networks for web apps that are logically isolated from other public cloud tenants. \n \n Subnets provide a range of private IP addresses for each web app tier within a zone. \n \n Security Groups and ACLs are used as firewalls to limit access to virtual servers and web app tiers. |
{: caption="Table 2. Network segmentation and isolation architecture decisions" caption-side="bottom"}

## Architecture decisions for cloud native connectivity
{: #cloud-native-connectivity}

The following are cloud native connectivity architecture decisions for this design.

| Architecture decision | Requirement | Alternative | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Connectivity to Cloud Services | Provide secure connection to Cloud Services | * VPC Gateway + Virtual Private Endpoints (VPE) \n * Private Cloud Service endpoints \n * Public Cloud Service Endpoints | VPC Gateway + Virtual Private Endpoints (VPE) | VPC Gateway + Virtual Private Endpoints enable connectivity to IBM Cloud services by using private IP addresses allocated from a VPC subnet. |
| VPC to VPC Connectivity | Connect two or more VPCs over a private network | * Global Transit Gateway \n * Local Transit Gateway (TGW) | Local Transit Gateway (TGW) | The Local Transit Gateway enables connectivity between the Management and Workload VPCs. |
{: caption="Table 3. Cloud native connectivity architecture decisions" caption-side="bottom"}

## Architecture decisions for load balancing
{: #network-segmentation-isolation}

The following are load balancing architecture decisions for this design.

| Architecture decision | Requirement | Alternative | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Application Load Balancer | Route web user http/https requests | * VPC ALB \n * VPC NLB | VPC ALB | The VPC ALB is recommended for web-based workloads. \n * Provides layer 4 and layer 7 load balancing \n * Supports HTTP, HTTPS, and TCP requests \n * Supports SSL offloading |
| Local Load Balancing: Web Tier | Distribute requests across zones for high availability | * Public VPC ALB \n * Private VPC ALB \n * Public VPC NLB \n * Private VPC NLB | Public ALB | The Public VPC ALB distributes traffic among virtual servers within the web tier. |
| Local Load Balancing: App Tier | Distribute requests across zones for high availability | * Public VPC ALB \n * Private VPC ALB \n * Public VPC NLB \n * Private VPC NLB | Private ALB | The Private VPC ALB distributes traffic among virtual servers within the app tier. |
{: caption="Table 4. Load balancing architecture decisions" caption-side="bottom"}

## Architecture decisions for domain name system
{: #dns}

The following are domain name system architecture decisions for this design.

| Architecture decision | Requirement | Alternative | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Public DNS | Provide DNS resolution to support the use of hostnames instead of IP addresses for applications | * IBM Cloud Internet Services (CIS) \n * IBM Cloud DNS	IBM Cloud Internet Services (CIS) | IBM Cloud DNS	IBM Cloud Internet Services (CIS) | IBM Cloud Internet Services support provisioning and configuring DNS records for public DNS resolution and can be integrated with the public VPC ALBs for the web tier. |
| Private DNS | Provide DNS resolution within IBM Cloud's private network | IBM Cloud DNS | IBM Cloud DNS | IBM Cloud DNS manages private DNS records, resolves domain names from IBM Cloud's private network, and can be integrated with the private VPC ALBs used for the application's back end. |
{: caption="Table 5. Domain name system (DNS) architecture decisions" caption-side="bottom"}
