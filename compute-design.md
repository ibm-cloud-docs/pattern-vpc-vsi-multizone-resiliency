---

copyright:
  years: 2023
lastupdated: "2023-12-15"

subcollection: pattern-vpc-vsi-multizone-resiliency

keywords:

---

# Compute design
{: #compute-design}

The web app multi-zone resiliency pattern uses compute options that are highly available, properly isolated, provide adequate capacity for the applications, and can handle increased workload demands.

The Virtual Servers for the web tier, application tier, and database tier are provisioned as follows:

-   **VPC Virtual Server Profile**: select compute and memory profiles for each tier, based on tier capacity and performance requirements.

-   **Placement Groups**: create [Placement Groups](https://cloud.ibm.com/docs/vpc?topic=vpc-about-placement-groups-for-vpc&interface=ui) for virtual servers to avoid single point of failures. Select “[power spread](https://cloud.ibm.com/docs/vpc?topic=vpc-about-placement-groups-for-vpc&interface=ui#power-spread-placement-groups-for-vpc)” to provision virtual servers in the Placement Group across different hosts to protect against host failures and on separate power and network domains to protect against power and network failures.

-   **VPC Autoscale**: create [Instance Groups](https://cloud.ibm.com/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=ui) for autoscaling virtual servers in the Web and App tiers to adjust compute resources to handle dynamic changes in load conditions.
    -   Add the instance group to the Placement Group.
    -   Add a load balancer to the instance group to balance incoming requests across instances and configure specific health checks. \n

-   **Multi-zone deployment**: deploy Virtual Servers across multiple availability zones in the same region to protect the application from zone outages. Use a VPC Application Load Balancer (ALB) to front end the virtual servers in the web and app tiers. Use the Virtual Servers auto-scale instance group as the backend pool for the ALB to automatically adjust the server pool based on auto-scale policies.
