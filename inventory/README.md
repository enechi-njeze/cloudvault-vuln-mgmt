# Asset Inventory

**System:** CloudVault Federal Health Exchange (FHX)
**Repository:** cloudvault-vuln-mgmt
**Folder:** inventory
**Framework Reference:** NIST SP 800-53 CM-8 / FedRAMP ConMon / CIS Controls v8 Control 1-2
**Document ID:** FHX-VM-INV-001
**Prepared By:** Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, Security+, CHP, CSCS, CISSP (in progress)
**Role:** ISSO / ISSM, CloudVault Federal Health Exchange
**Last Updated:** April 2025
**Status:** Active

---

## Purpose

You cannot protect what you cannot see. Asset inventory is the foundation of every vulnerability management program. Without an accurate, complete, and continuously updated inventory of all system components, a vulnerability management team cannot ensure that all assets are scanned, all vulnerabilities are discovered, and all remediation actions are verified.

For CloudVault FHX, the asset inventory must capture all components within the FedRAMP authorization boundary and all components that connect to or can impact the security of in-boundary systems. The inventory is maintained using AWS Config, Amazon Inspector, and the FHX Configuration Management Database (CMDB), and is reconciled quarterly.

---

## Documents in This Folder

| Document | Description | Reference | Status |
|---|---|---|---|
| asset-inventory-master.md | Complete FHX asset inventory across all AWS GovCloud services | NIST SP 800-53 CM-8 | Active |
| asset-inventory-methodology.md | Inventory discovery methodology and reconciliation procedures | FedRAMP ConMon | Active |
| asset-classification.md | Asset classification by criticality, data sensitivity, and internet exposure | CIS Controls v8 | Active |
| inventory-reconciliation-q1-2025.md | Q1 2025 inventory reconciliation report | FedRAMP ConMon | Active |

---

## Asset Inventory Summary

CloudVault FHX runs entirely on AWS GovCloud (us-gov-west-1 primary, us-gov-east-1 for DR/replication). All assets are Infrastructure-as-Code managed via Terraform; the Terraform state file serves as the authoritative asset inventory baseline.

### Compute Assets

| Asset Category | Service | Count | Environment | Internet-Facing | PHI |
|---|---|---|---|---|---|
| Application containers | Amazon ECS Fargate | 12 task definitions | Production | No (behind ALB) | Yes |
| API management | Amazon API Gateway | 4 APIs | Production | Yes (WAF-protected) | Yes |
| Batch processing | AWS Lambda | 23 functions | Production | No | Yes |
| Data processing pipeline | Amazon ECS Fargate | 6 task definitions | Production | No | Yes |
| Bastion/jump host | EC2 (t3.small) | 1 | Management VPC | No (SSM only) | No |

### Data Assets

| Asset Category | Service | Count | PHI | Encrypted | Backup |
|---|---|---|---|---|---|
| Patient records database | RDS Aurora (PostgreSQL) | 1 cluster (2 instances) | Yes | AES-256 KMS | Daily + PITR |
| Clinical document storage | S3 | 3 buckets | Yes | SSE-KMS | Cross-region replication |
| Clinical index | DynamoDB | 2 tables | Yes | AWS Managed Key | PITR |
| Secrets and credentials | AWS Secrets Manager | 47 secrets | No (credentials, not PHI) | KMS | Replicated |
| Audit logs | CloudWatch Logs, S3 | Multiple log groups and 2 buckets | Pseudonymized | AES-256 | S3 lifecycle to Glacier |
| Long-term archive | S3 Glacier | 1 vault | Yes | SSE-KMS | Vault Lock |

### Network Assets

| Asset Category | Service | Count | Purpose |
|---|---|---|---|
| Virtual Private Clouds | AWS VPC | 4 VPCs | CDE, Application, Management, Shared Services |
| Load balancers | AWS ALB | 3 | Patient portal, FHIR API, management |
| Web Application Firewall | AWS WAF | 2 ACLs | Internet-facing endpoints |
| Transit Gateway | AWS Transit Gateway | 1 | Inter-VPC routing |
| PrivateLink endpoints | AWS PrivateLink | 6 | S3, DynamoDB, STS, SSM, Secrets Manager, HealthPay |
| VPN connections | AWS Site-to-Site VPN | 3 | Agency partner connectivity (3 agencies with high data volume) |

### Security and Management Assets

| Asset Category | Service | Count | Purpose |
|---|---|---|---|
| Identity management | AWS IAM | 1 account | Role-based access control |
| Threat detection | Amazon GuardDuty | 1 (all regions) | Continuous threat detection |
| Config compliance | AWS Config | 1 recorder | 47 rules enforced |
| Security posture | AWS Security Hub | 1 hub | Aggregated security findings |
| Vulnerability scanning | Amazon Inspector | 1 (all compute) | Continuous vulnerability assessment |
| Key management | AWS KMS | 8 CMKs | Data encryption keys by data type |
| Certificate management | AWS Certificate Manager | 6 certificates | TLS for all FHX endpoints |
| Logging | AWS CloudTrail | 1 trail (all regions) | API audit logging |

---

## Total Asset Count

| Category | Count | Scan Coverage |
|---|---|---|
| Compute (containers, functions) | 41 task definitions / functions | 100% (Amazon Inspector) |
| Data stores | 8 services / storage types | 100% (Inspector + Config) |
| Network assets | 19 components | 100% (Config rules) |
| Security assets | 11 services | 100% (Security Hub) |
| **Total asset types** | **79** | **100%** |

---

## Inventory Discovery and Reconciliation

AWS Config records all resource configurations and changes continuously. The FHX CMDB is reconciled quarterly against the AWS Config resource inventory to identify: (1) new resources not yet in the CMDB, (2) resources in the CMDB that no longer exist in AWS (decommissioned), and (3) resources whose configuration has drifted from the baseline. Reconciliation findings are reviewed by the ISSO and any unauthorized new resources trigger a P1 incident.

---

## Related Documents

- [Scan Reports](../scan-reports/README.md)
- [Risk Classification](../risk-classification/README.md)
- [Remediation Tracking](../remediation-tracking/README.md)
- [FedRAMP SSP Component Inventory](https://github.com/enechi-njeze/cloudvault-fedramp-ato/blob/main/README.md)
- [NIST RMF Prepare Step](https://github.com/enechi-njeze/cloudvault-nist-rmf/blob/main/step0-prepare/README.md)

---

*Prepared by Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, CompTIA Security+ SY0-701, CHP, CSCS, CISSP (in progress)*
*ISSO / ISSM, CloudVault Federal Health Exchange (FHX)*
*[LinkedIn](https://www.linkedin.com/in/enechi-njeze) | [Portfolio](https://github.com/enechi-njeze)*
