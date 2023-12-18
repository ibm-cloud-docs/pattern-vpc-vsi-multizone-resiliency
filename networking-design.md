---

copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-vpc-vsi-multizone-resiliency

keywords:

---

# Networking design
{: #networking-design}

The web app multi-zone resiliency pattern uses IBM Cloud Virtual Private Cloud infrastructure and network services to segment the application tiers and support the application deployment across multiple availability zones.

- Deploy the web app within a Virtual Private Cloud (VPC) provisioned across multiple availability zones within a region to provide workload isolation within the public cloud.
   - Place each web app tier in a separate subnet in each availability zone.
   - Use security groups and ACLs as firewalls to limit access to virtual server instances for operational purposes and to control network traffic to each web app tier.

- Create an instance group for the VPC Virtual Server instances in the web and app tiers to enable auto-scaling by using a VPC Load Balancer.

- Use VPC Application Load Balancers (ALB) to distribute incoming requests among the virtual server instances within the web and app tiers.
   - Configure load balancing policies, rules, and check health settings to distribute the requests across the available virtual servers.
   - Integrate the VPC ALB with the Virtual Servers Instance Group for the tier to auto-scale this backend pool based on load requirements.
   - Use a Public Application Load Balancer for the web tier in the front end and a Private Application Load Balancer for the application tier in the backend.
   - Use the assigned FQDNs to send traffic to the ALBs to avoid connectivity problems.
   - Configure the Application Load Balancers for high availability by selecting the subnets in each availability zone where the virtual servers are deployed. The ALB automatically provisions load balancing appliances for each subnet zone.

- Configure the Cloud Internet Service (CIS) as a proxy to the public VPC Load Balancers for the web tier to use CIS capabilities such as Web Application Firewall (WAF) and DDoS protection to secure the Web Application.
   - Use the IBM Cloud Secrets Manager service to manage the Transport Layer Security (TLS) certificate for all incoming HTTPS requests.
   - Make the SSL certificates available to the VPC Application Load Balancers to configure HTTPS encryption.

- Use CIS to provision and configure DNS records for public DNS resolution and IBM Cloud DNS to manage DNS records and resolve domain names from IBM Cloud's private network.
