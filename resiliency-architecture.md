---

copyright:
  years: 2023
lastupdated: "2023-12-15"

subcollection: pattern-vpc-vsi-multizone-resiliency

keywords:

---

# Architecture decisions for resiliency
{: #resiliency-architecture}

The following sections summarize the resiliency architecture decisions for workloads deployed on IBM Cloud VPC infrastructure.

## Architecture decisions for high availability
{: #high-availability}

| Architecture decision | Requirement | Alternative | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| High Availability Deployment | * Ensure availability of resources if outages occur. \n * Support SLA targets for application availability. | - Single zone, single region \n - Multi zone, single region \n - Multi-zone, multi region | Multi-zone, single region | Recommended approach for core business applications and production level workloads with stringent resiliency requirements \n - Provides protection from zone outage \n - Supports 99.99% availability for active-active application architecture with virtual servers that are deployed across 3 zones \n  - Supports business continuity policies with country boundaries or geo-data residence constraints |
{: caption="Table 1. High availability architecture decisions" caption-side="bottom"}

## Architecture decisions for backup and restore 
{: #backup-and-restore}

| Architecture decision | Requirement | Alternative | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| DB server backup  | Backup DB server images to enable recovery of databases if unplanned outages occur. | - IBM Cloud Backup \n - IBM Storage Protect | IBM Storage Protect | IBM Storage Protect is used to backup DB servers in DB tier since this is also the recommended tool for transaction consistent backups for the data volumes. |
| DB content backup | Create transaction-consistent database backups to enable recovery of database tier if unplanned outages occur. | - IBM Storage Protect \n - DB Backup Tool \n - BYO Tool | IBM Storage Protect | IBM Storage Protect can create application consistent backups of the database. It supports Oracle, IBM Db2, MongoDB, Microsoft SQL Server, SAP HANA. |
| File Backup | Create file system backups according to business processes. | - IBM Storage Protect \n - BYO Backup Tool | IBM Storage Protect | IBM Storage Protect supports backup and recovery of specific files. |
| Backup Automation | Schedule regular database backups based on RPO requirements to enable data recovery if unplanned outages occur. | - IBM Cloud Backup \n - IBM Storage Protect \n - BYO Backup Tool | IBM Storage Protect | IBM Storage Protect supports scheduling regular backups and defining backup policies to manage the creation and deletion of backups. |
{: caption="Table 2. Backup and restore architecture decisions" caption-side="bottom"}
