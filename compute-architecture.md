---

copyright:
  years: 2023
lastupdated: "2023-12-15"

subcollection: pattern-vpc-vsi-multizone-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Compute architecture decisions
{: #compute-architecture}

The following are computer architecture decisions for the web app multi-zone resiliency pattern.

| Architecture Decision | Requirement | Alternative | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Compute Web, App, DB Tiers                                     | Provide properly isolated compute resources with adequate compute capacity for the applications | - Virtual Servers for VPC \n - Virtual Servers for VPC-Dedicated Hosts                                     | Virtual Servers for VPC                          | x86 compute within isolated VPC network that can be quickly provisioned and scaled based on load requirements. |
| Virtual Servers for VPC Profile for Web Tier                                           |                                                                                                   | - Balanced \n - Compute \n - Memory                                        |  Compute Profile                  | Compute profile is recommended since this tier tends to be higher in traffic workload and more CPU-intensive. Select the vCPU and memory configuration based on application requirements. |
| Virtual Servers for VPC Profile for App Tier                                           |                                                                                                   | - Balanced \n - Compute \n - Memory                                        | Memory                            | Memory profile is recommended since this tier tends to be more memory-intensive and involves more caching. |
| Virtual Servers for VPC Profile for Database Tier                                      |                                                                                                   | - Balanced \n - Compute \n - Memory                                        | Balance                           | For databases deployed on virtual servers select a balance profile. |
| Virtual Servers Placement Group Configuration Web, App Tiers   | Provide compute resources that are highly available and can handle increased workload demands   | - Host Spread \n - Power Spread                                         | Power Spread                      | Use “power spread” for the placement group to ensure the virtual servers are placed on different computer hosts with different power and network supplies. |
| Virtual Servers High Availability Configuration Web, App Tiers |                                                                                                   | - Multiple instances-single zone \n - Multiple instances-multiple zones | Multiple instances-multiple zones | Deploy virtual servers across multiple availability zones to protect the application from zone outages. |
{: caption="Table 1. Compute architecture decisions" caption-side="bottom"}
