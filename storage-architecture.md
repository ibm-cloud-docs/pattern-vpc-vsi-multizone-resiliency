---

copyright:
  years: 2023
lastupdated: "2023-12-15"

subcollection: pattern-vpc-vsi-multizone-resiliency

keywords:

---

# Architecture decisions for storage
{: #storage-architecture}

The following are storage architecture decisions for the web app multi-zone resiliency pattern.

| Architecture decision | Requirement | Alternative | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Main Storage for web or app tier         | Provide highly available storage that meets the application performance requirements. | - Block storage for VPC \n - File storage for VPC \n - Object storage  | Object Storage                | Object Storage is recommended to store large volumes of static content or unstructured data.                                                                                                                      |
| Main storage for database tier         | Provide highly available storage that meets the database performance requirements.    | - Block storage for VPC \n - Managed DB (DBaaS)                        | Block Storage for VPC | Block storage provides the best performance for databases.                                                                                                           |
| Logs storage (Audit, Operational) | Provide highly available storage for logs                                             | - Block Storage for VPC \n - Object Storage                      | Object Storage                | Object Storage provides high available storage and is integrated with operational and audit logging cloud services.                                                         |
| Backup storage                     | Provide highly available storage for backups                                          | - Block Storage for VPC \n - Object Storage                      | Object Storage                | Object Storage provides high available storage for data backups. |
{: caption="Table 1. Storage architecture decisions" caption-side="bottom"}
