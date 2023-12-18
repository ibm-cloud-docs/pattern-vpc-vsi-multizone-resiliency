---

copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-vpc-vsi-multizone-resiliency

keywords:

---

# Architecture decisions for storage
{: #storage-architecture}

The following are storage architecture decisions for the web app multi-zone resiliency pattern.

| Architecture decision | Requirement | Alternative | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Main Storage for web and app tiers         | Provide highly available storage that meets the application performance requirements. | - Block storage for VPC \n - File storage for VPC \n - Cloud Object Storage  | Cloud Object Storage                | Cloud Object Storage is recommended to store large volumes of static content or unstructured data.                                                                                                                      |
| Main storage for database tier         | Provide highly available storage that meets the database performance requirements.    | - Block storage for VPC \n - Managed DB (DBaaS)                        | Block Storage for VPC | Block storage provides the best performance for databases.                                                                                                           |
| Logs storage: Audit and operational logs | Provide highly available storage for logs                                             | - Block Storage for VPC \n - Cloud Object Storage                      | Cloud Object Storage                | Cloud Object Storage provides high available storage and is integrated with operational and audit logging cloud services.                                                         |
| Backup storage                     | Provide highly available storage for backups                                          | - Block Storage for VPC \n - Cloud Object Storage                      | Cloud Object Storage                | Cloud Object Storage provides high available storage for data backups. |
{: caption="Table 1. Architecture decisions for storage" caption-side="bottom"}
