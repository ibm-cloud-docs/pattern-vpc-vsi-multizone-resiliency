---

copyright:
  years: 2023
lastupdated: "2023-12-15"

subcollection: pattern-vpc-vsi-multizone-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Overview
{: #overview}

The web app multi-zone resiliency pattern provides a solution design for a 3-tier web architecture deployment that meets high availability requirements for enterprise workloads. It uses cloud platform capabilities to deploy resilient applications on [IBM Cloud Virtual Servers for VPC](/docs/vpc?topic=vpc-getting-started&interface=ui). It is not intended for applications that are deployed on Power Virtual Servers (PowerVS) and does not cover application or database high availability design.

This pattern is recommended for applications with 99.9% availability requirements.
- It can support up to 99.99% application availability designs when the application is spread across 3 availability zones.
- It can be used to support business continuity policies or regulatory requirements with country boundaries or geo-data residence constraints.
- It does not support out-of-region disaster recovery. To address disaster recovery policies or business continuity policies with geo or distance compliance requirements, see the [web app multi-zone resiliency pattern](/docs/pattern-vpc-vsi-cross-region-resiliency).

The web app multi-zone resiliency pattern is intended to:

- Accelerate and simplify solution design by providing a standard IBM Cloud deployment architecture reference following the [IBM Architecture Framework](/docs/architecture-framework).

- Provide a prescriptive, end-to-end enterprise-class solution design, with diagrams, component architecture decisions along with rationale for cloud component selection to meet enterprise requirements.

- Ensure that requirements can be met from a performance, system availability, and security perspective.
