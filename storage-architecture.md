---

copyright:
  years: 2023
lastupdated: "2023-12-15"

subcollection: pattern-vpc-vsi-multizone-resiliency

keywords:

---

# Storage architecture decisions
{: #storage-architecture}

The following are storage architecture decisions for the web app multi-zone resiliency pattern.

| Architecture Decision | Requirement | Alternative | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Main Storage for Web, App Tier         | Provide highly available storage that meets the application performance requirements. | - Block storage for VPC \n - File storage for VPC \n - Object storage  | Object Storage                | Use Object Storage to store large volumes of static content or unstructured data.                                                                                                                      |
| Main Storage for Database Tier         | Provide highly available storage that meets the database performance requirements.    | - Block storage for VPC \n - Managed DB (DBaaS)                        | Block Storage for VPC | For databases deployed on virtual servers use block storage for best performance.                                                                                                           |
| Logs Storage  (Audit, Operational) | Provide highly available storage for logs                                             | - Block Storage for VPC \n - Object Storage                      | Object Storage                | Object Storage provides high available storage and is integrated with operational and audit logging cloud services.                                                         |
| Backup Storage                     | Provide highly available storage for backups                                          | - Block Storage for VPC \n - Object Storage                      | Object Storage                | Object Storage provides high available storage for data backups. |
{: caption="Table 1. Storage architecture decisions" caption-side="bottom"}
