---

copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-vpc-vsi-multizone-resiliency

keywords:

---

# Architecture decisions for security
{: #security-architecture}

The following are security architecture decisions for the web app multi-zone resiliency pattern.

## Architecture decisions for data encryption
{: #data-encryption}

| Architecture decision | Requirement | Alternative | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Encryption approach | Encrypt all data to protect it from unauthorized disclosure. | - Encryption with provider keys \n - Encryption with customer-managed keys | Encryption with customer-managed keys | Encrypting data with customer-managed keys is recommended since it provides the added security and customer control that is needed to meet regulatory compliance requirements. |
| Data encryption at rest of application | Encrypt all application data to protect it from unauthorized disclosure. | - Storage encryption with customer-managed keys \n - App-level encryption with customer-managed keys | Storage Encryption with customer-managed keys | Application data can be stored on File Storage for VPC, Block Storage for VPC, or Cloud Object Storage that support encryption with customer-managed keys by selecting a Key Management Service (KMS) for the respective storage service. |
| Data encryption at rest of backups | Encrypt all backup data to protect it from unauthorized disclosure. | Storage encryption with customer-managed keys App-level encryption | Storage encryption with customer-managed keys | Backup data can be stored in Cloud Object Storage or Block Storage for VPC storage that support encryption with customer-managed keys by selecting a KMS. |
| Data encryption at rest of logs | Encrypt all operational and audit logs at rest to protect them from unauthorized disclosure. | Storage encryption with customer-managed keys App-level encryption | Storage encryption with customer-managed keys | All operational and audit logs are stored in Cloud Object Storage that supports encryption with customer-managed keys by selecting a KMS. |
| Data encryption in transit of web app | Encrypt all application data in transit to protect it from unauthorized disclosure. | Application-level encryption with TLS | Application-level encryption with TLS | Web app uses HTTPS protocol to encrypt data transmissions. |
| Data encryption in transit of DB tier | Encrypt all application data in transit to protect it from unauthorized disclosure. | Application-level encryption with TLS | Application-level encryption with TLS | The database application uses TLS to encrypt data in transit. |
{: caption="Data encryption architecture decisions" caption-side="bottom"}

## Architecture decisions for key management
{: #kms}

| Architecture decision | Requirement | Alternative | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Key Lifecycle Management and HSM | Encrypt data at rest and in transit by using customer-managed keys to protect them from unauthorized access.  | Key Protect \n Hyper Protect Crypto Services (HPCS) | Key Protect | Key Protect is recommended for applications that need to comply with regulations requiring encryption of data with customer-managed keys. Key Protect provides key management services by using a shared (multi-tenant) FIPS 140-2 Level 3 certified hardware security modules (HSMs). |
| | | | Hyper Protect Crypto Services (HPCS) | HPCS is recommended for financial services and highly regulated industry applications. HPCS provides Key Management Services with the highest level of security and control that is offered by any cloud provider in the industry. It uses a dedicated (single-tenant) FIPS 140-2 Level 4 certified Hardware Security Module and supports customer-managed master keys, giving the customer exclusive control of the entire key hierarchy. |
| Certificate Management | Protect secrets through their entire lifecycle and secure them using access control measures | Secrets Manager \n BYO Certificate Manager | Secrets Manager | IBM Secrets Manager creates, leases, and centrally manages secrets that are used by IBM Cloud Services or customer applications. Secrets are stored in a dedicated instance of Secrets Manager and can be encrypted by using any of IBM Cloud Key Management Services. |
{: caption="Key management architecture decisions" caption-side="bottom"}

## Architecture decisions for identity and access management
{: #iam}

| Architecture decision | Requirement | Alternative | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Privileged Access Management        | - Ensure that all operator actions are run securely through a bastion host \n - Implement session recording to track all activities and note any potential threats \n - Manage access to resources and track commands issued | - BYO Bastion Host \n - BYO Bastion Host with Privileged Access Management (PAM) SW                  | BYO Bastion Host with Privileged Access Management (PAM) SW  | The Bastion Host is a Virtual Server instance that is provisioned through SSH over a private network to securely access resources within IBM Cloud’s private network. Using PAM software is recommended to perform session recording and to help track and manage all access.                      |
{: caption="Identity and access management architecture decisions" caption-side="bottom"}

## Architecture decisions for application security
{: #app-security}

| Architecture decision | Requirement | Alternative | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| DDoS | - Enforce information flow policies and protect the boundaries of the application. \n - Protect against or limit the effects of denial-of-service attacks. | IBM Cloud Internet Services (CIS) | IBM Cloud Internet Services (CIS) | IBM Cloud Internet Services provide Distributed Denial of Service (DDoS) to protect applications that are exposed to the public network. |
| Web Application Firewall | Protect web applications from application layer attacks. | - IBM Cloud Internet Services (CIS) \n - BYO Firewall on Virtual Server for VPC | IBM Cloud Internet Services (CIS) | IBM Cloud Internet Services provides Web Application Firewall security features to protect applications that are exposed to the public network. |
{: caption="Application security architecture decisions" caption-side="bottom"}
