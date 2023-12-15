---

copyright:
  years: 2023
lastupdated: "2023-12-15"

subcollection: pattern-vpc-vsi-multizone-resiliency

keywords:

---

# Resiliency architecture decisions
{: #resiliency-architecture}

The following sections summarize the resiliency architecture decisions for workloads deployed on IBM Cloud VPC infrastructure.

## High Availability Architecture Decisions
{: #high-availability}

| Architecture Decision | Requirement | Alternative | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| High Availability Deployment | * Ensure availability of resources in the event of outages. \n * Support SLA targets for application availability. | Single zone, single region \n Multi zone, single region \n Multi-zone, multi region | Multi-zone, single region | Recommended approach for core business applications and production level workloads with stringent resiliency requirements \n - Provides protection from zone outage \n - Supports 99.99% availability for active-active application architecture with virtual servers deployed across 3 zones \n  - Supports business continuity policies with country boundaries or geo data residence constraints |
{: caption="Table 1. High availability architecture decisions" caption-side="bottom"}

## Backup and Restore Architecture Decisions
{: #backup-and-restore}

| Architecture Decision | Requirement | Alternative | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| DB server backup  | Backup DB server images to enable recovery of databases in the event of unplanned outages | - IBM Cloud Backup \n - IBM Storage Protect | IBM Storage Protect | IBM Storage Protect is used to backup DB servers in DB tier since this is also the recommended tool for transaction consistent backups for the data volumes. |
| DB backup | Create transaction consistent database backups to enable recovery of database tier in the event of unplanned outages | - IBM Storage Protect \n - DB Backup Tool \n - BYO Tool | IBM Storage Protect | IBM Storage Protect con create application consistent backups of the database. \n It supports Oracle, IBM Db2, MongoDB, Microsoft SQL Server, SAP HANA. |
| File Backup | Create file system backups for according to business processes | - IBM Storage Protect \n - BYO Backup Tool | IBM Storage Protect | IBM Storage Protect supports backup and recovery of specific files. |
| Backup Automation | Schedule regular database backups based on RPO requirements to enable data recovery in the event of unplanned outages | - IBM Cloud Backup \n - IBM Storage Protect \n - BYO Backup Tool | IBM Storage Protect | IBM Storage Protect supports scheduling regular backups and defining backup policies to manage the creation and deletion of backups. |
{: caption="Table 2. Backup and restore architecture decisions" caption-side="bottom"}
