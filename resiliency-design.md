---

copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-vpc-vsi-multizone-resiliency

keywords:

---

# Resiliency design
{: #resiliency-design}

## High availability design
{: #high-availability-design}

The web app multi-zone resiliency pattern deploys a 3-tier web architecture in a region following the multi-zone, single-region deployment described in [IBM VPC resiliency](/docs/vpc-resiliency).

The web tier and application tier are deployed in two availability zones. Each tier is deployed across VPC Virtual Server Instances (VSIs) in a VPC Instance Group for Autoscaling. A public VPC Application Load Balancer (ALB) routes web requests to healthy virtual instances in the app tier. A private VPC ALB routes traffic to healthy virtual servers in the app tier.

The web and app tiers are subject to the 99.9% IBM Cloud infrastructure [SLA](https://www.ibm.com/support/customer/csol/terms/?id=i126-9268&lc=en#detail-document){: external} for a region. For 99.99% infrastructure SLA within the region, deploy the web and app servers across 3 availability zones.

High availability at the database tier is typically achieved with an active-standby architecture that includes:

- A primary database and one or more standby database replicas
- Data replication between the primary and standby replicas
- A mechanism to failover from the primary database to the standby replica and back

In the web app multi-zone resiliency pattern, the database tier is deployed on Virtual Server Instances across two availability zones in the same region, following an active-standby architecture that uses database-specific replication and failover configuration settings.

This database deployment architecture is subject to the 99.9% IBM Cloud infrastructure [SLA](https://www.ibm.com/support/customer/csol/terms/?id=i126-9268&lc=en#detail-document){: external} for a region. For 99.99% infrastructure SLA within the region, the database must be deployed on virtual servers across three availability zones within the region and use clustering and replication configurations that are database-specific and are beyond the scope of this document.

An alternative to deploying the database in VPC Virtual Server Instances is to use [IBM Cloud Databases](/docs/cloud-databases) instances in a multi-zone region (MZR). IBM Cloud Databases provides highly available and scalable managed SQL and no-SQL databases with 99.99% SLA and low operational cost.

## Backup and restore design
{: #backup-design}

The web app multi-zone resiliency pattern uses backup and restore to protect database contents from accidental deletion or corruption and provide short-term storage for historical data.

Do the following to create transaction-consistent database backups that can be quickly restored in the same region where the Web Application is deployed:

- Use database-aware backup tools, such as [IBM Storage Protect](https://cloud.ibm.com/catalog/content/SPonIBMCloud-20c54034-d319-48c0-beb6-0b4adc54265c-global){: external}, to create and manage database backups.

- Schedule regular automated database backups based on business Recovery Point Objective requirements.

- Schedule regular file-level backups if needed to meet application and business process requirements.

Do the following to enable recovery of the database contents in an alternative site if a regional outage occurs:
- Create backups of the database servers by using [IBM Storage Protect](https://cloud.ibm.com/catalog/content/SPonIBMCloud-20c54034-d319-48c0-beb6-0b4adc54265c-global){: external} to enable recovery of the database application in an alternative site if a regional outage occurs.

- Store database backups in [cross-region Cloud Object Storage](/docs/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-geo).
