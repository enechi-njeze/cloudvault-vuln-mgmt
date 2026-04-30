# cloudvault-vuln-mgmt

![Framework](https://img.shields.io/badge/Framework-NIST%20SP%20800--40-blue)
![FedRAMP](https://img.shields.io/badge/FedRAMP-High-red)
![Impact](https://img.shields.io/badge/Impact-HIGH-darkred)
![Status](https://img.shields.io/badge/Status-In%20Progress-yellow)
![Cert](https://img.shields.io/badge/Cert-CISA-green)

## System Overview

**System Name:** CloudVault Federal Health Exchange (FHX)
**System Owner:** Dr. Patricia Owens
**ISSO / ISSM:** Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, Security+, CHP, CSCS, CISSP (in progress)
**AO:** Deputy Director Henry Kline
**CISO:** Angela Torres
**DevOps Lead:** Trevor L. Abrams
**Classification:** UNCLASSIFIED // CUI
**Impact Level:** HIGH (FIPS 199) | FedRAMP High (421 controls)
**Cloud Platform:** AWS GovCloud (US-West)
**Last Updated:** April 2025

---

## Purpose

Vulnerability management is one of the most operationally intensive and consequential GRC functions for any federal cloud system. A well-run vulnerability management program continuously identifies, classifies, prioritizes, remediates, and validates security vulnerabilities across the entire asset inventory before they can be exploited by threat actors targeting federal health data.

This repository documents the CloudVault FHX vulnerability management program: the asset inventory, scan report archive, risk classification methodology, remediation tracking procedures, validation processes, and performance metrics. The program operates in alignment with NIST SP 800-40 Rev 4 (Guide to Enterprise Patch Management Planning) and satisfies FedRAMP RA-5 (Vulnerability Scanning) and SI-2 (Flaw Remediation) control requirements.

---

## Repository Structure

```
cloudvault-vuln-mgmt/
├── inventory/              # Asset inventory: all FHX components subject to scanning
├── scan-reports/           # Vulnerability scan reports archive (quarterly)
├── risk-classification/    # Risk classification methodology and scoring model
├── remediation-tracking/   # POA&M and remediation tracking by vulnerability
├── validation/             # Validation scan results confirming remediation
├── metrics/                # Vulnerability management KPIs and trend reporting
├── diagrams/               # Vulnerability management workflow diagrams
└── README.md               # This file
```

---

## Deliverables

| Document | Folder | Framework Reference | Status |
|---|---|---|---|
| Asset Inventory | inventory/ | NIST SP 800-40 / FedRAMP RA-5 | In Progress |
| Quarterly Scan Reports | scan-reports/ | FedRAMP RA-5, PCI-DSS Req 11 | In Progress |
| Risk Classification Methodology | risk-classification/ | CVSS v3.1, NIST NVD | In Progress |
| POA&M and Remediation Tracker | remediation-tracking/ | FedRAMP POA&M / NIST SP 800-53 CA-5 | In Progress |
| Validation Scan Results | validation/ | FedRAMP RA-5(c) / PCI-DSS Req 11.3 | In Progress |
| Vulnerability Management Metrics | metrics/ | FedRAMP ConMon / CISA KPIs | In Progress |
| VM Workflow Diagrams | diagrams/ | NIST SP 800-40 | In Progress |

---

## Vulnerability Management Program Overview

### Scanning Schedule

FHX conducts vulnerability scanning according to the following schedule, which meets or exceeds FedRAMP High continuous monitoring requirements:

| Scan Type | Frequency | Tool | Scope | Owner |
|---|---|---|---|---|
| Infrastructure vulnerability scan | Monthly | Amazon Inspector | All EC2, ECS, Lambda, RDS in FHX accounts | Trevor L. Abrams |
| Container image scan | Per build (CI/CD) | ECR Image Scanning (Clair) | All container images in ECR | Trevor L. Abrams |
| Web application scan (DAST) | Per release | OWASP ZAP | All internet-facing FHX web applications | Trevor L. Abrams |
| Static code analysis (SAST) | Per commit | Checkmarx | All FHX application code | Trevor L. Abrams |
| Dependency and SCA scan | Per build | OWASP Dependency-Check | All third-party libraries and dependencies | Trevor L. Abrams |
| External ASV scan (PCI CDE) | Quarterly | TrustScan Federal LLC | CDE-facing internet endpoints | Enechi P.C. Njeze |
| Annual penetration test | Annual | ClearPath Security Assessors LLC | Full FHX system boundary + CDE segmentation | Enechi P.C. Njeze |
| AWS Config compliance check | Continuous | AWS Config | All AWS resource configurations | Trevor L. Abrams |
| CIS Benchmark compliance scan | Monthly | AWS Security Hub | All EC2 and container images | Trevor L. Abrams |

### Remediation SLAs

FedRAMP High requires remediation of critical and high vulnerabilities within specific timeframes. FHX policy is stricter:

| Severity | CVSS Score | FedRAMP SLA | FHX Policy | Escalation |
|---|---|---|---|---|
| Critical | 9.0-10.0 | 30 days | 72 hours | Automatic PagerDuty alert to ISSO and CISO |
| High | 7.0-8.9 | 60 days | 7 days | ISSO notification within 4 hours of detection |
| Medium | 4.0-6.9 | 180 days | 30 days | Monthly remediation review |
| Low | 0.1-3.9 | 365 days | 90 days | Quarterly remediation review |
| Informational | 0.0 | Best effort | Best effort | Tracked but not mandated |

### Laws, Regulations, and Standards

| Standard | Requirement | FHX Implementation |
|---|---|---|
| NIST SP 800-40 Rev 4 | Enterprise patch management planning | Full program aligned to NIST 800-40 lifecycle |
| NIST SP 800-53 Rev 5 RA-5 | Vulnerability scanning | Monthly scans, immediate critical remediation |
| NIST SP 800-53 Rev 5 SI-2 | Flaw remediation | POA&M-tracked remediation with SLAs |
| FedRAMP High Continuous Monitoring | Monthly scanning, 30-day critical SLA | FHX scans more frequently than required |
| PCI-DSS v4.0 Req 11.3 | Quarterly internal and external scans | Quarterly scans plus continuous CI/CD scanning |
| CIS Controls v8 Control 7 | Continuous vulnerability management | Amazon Inspector provides near-real-time detection |

---

## Certifications

| Certification | Holder | Relevance |
|---|---|---|
| CGRC (Certified in Governance, Risk, and Compliance) | Enechi P.C. Njeze | Vulnerability management governance and federal authorization |
| CISA (Certified Information Systems Auditor) | Enechi P.C. Njeze | Vulnerability assessment methodology and testing |
| CISM (Certified Information Security Manager) | Enechi P.C. Njeze | Vulnerability management program oversight |
| PMP (Project Management Professional) | Enechi P.C. Njeze | Remediation project tracking and stakeholder management |
| PMI-RMP (Risk Management Professional) | Enechi P.C. Njeze | Vulnerability risk scoring and prioritization |
| CompTIA Security+ SY0-701 | Enechi P.C. Njeze | Technical vulnerability assessment and scanning |
| CHP (Certified HIPAA Professional) | Enechi P.C. Njeze | PHI-related vulnerability risk assessment |
| CSCS (Certified Security Compliance Specialist) | Enechi P.C. Njeze | Multi-framework vulnerability management integration |
| CISSP (in progress) | Enechi P.C. Njeze | Broad security architecture and vulnerability management |

---

## Related Repositories

- [cloudvault-fedramp-ato](https://github.com/enechi-njeze/cloudvault-fedramp-ato): FedRAMP ATO (RA-5, SI-2 controls)
- [cloudvault-nist-rmf](https://github.com/enechi-njeze/cloudvault-nist-rmf): NIST RMF (continuous monitoring)
- [cloudvault-pci-dss](https://github.com/enechi-njeze/cloudvault-pci-dss): PCI-DSS (Requirement 11 scans)
- [cloudvault-risk-governance](https://github.com/enechi-njeze/cloudvault-risk-governance): Risk register
- [cloudvault-sox-itgc](https://github.com/enechi-njeze/cloudvault-sox-itgc): Patch management controls (ITGC CO-004)

---

*Prepared by Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, CompTIA Security+ SY0-701, CHP, CSCS, CISSP (in progress)*
*ISSO / ISSM, CloudVault Federal Health Exchange (FHX)*
*[LinkedIn](https://www.linkedin.com/in/enechi-njeze) | [Portfolio](https://github.com/enechi-njeze)*
