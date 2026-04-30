# Vulnerability Scan Reports

**System:** CloudVault Federal Health Exchange (FHX)
**Repository:** cloudvault-vuln-mgmt
**Folder:** scan-reports
**Framework Reference:** NIST SP 800-53 RA-5 / FedRAMP ConMon / PCI-DSS v4.0 Req 11.3
**Document ID:** FHX-VM-SCN-001
**Prepared By:** Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, Security+, CHP, CSCS, CISSP (in progress)
**Role:** ISSO / ISSM, CloudVault Federal Health Exchange
**Last Updated:** April 2025
**Status:** Active

---

## Purpose

This folder is the archive of all vulnerability scan reports for CloudVault FHX. Each report documents the findings from a specific scan, the tools used, the scope covered, and the disposition of each finding. Scan reports are the primary evidentiary record that the ISSO, AO, and FedRAMP PMO use to assess the security posture of the system between authorization events.

---

## Documents in This Folder

| Document | Description | Tool | Date | Status |
|---|---|---|---|---|
| amazon-inspector-q1-2025.md | Q1 2025 Amazon Inspector infrastructure scan results | Amazon Inspector | January 2025 | Active |
| security-hub-q1-2025.md | Q1 2025 AWS Security Hub consolidated findings | AWS Security Hub | January 2025 | Active |
| sast-q1-2025.md | Q1 2025 static code analysis results (Checkmarx) | Checkmarx | March 2025 | Active |
| dast-q1-2025.md | Q1 2025 dynamic application scan results (OWASP ZAP) | OWASP ZAP | March 2025 | Active |
| pentest-report-fy2025.md | FY2025 annual penetration test executive summary | ClearPath Security Assessors LLC | February 2025 | Active |
| dependency-scan-q1-2025.md | Q1 2025 dependency and SCA scan results | OWASP Dependency-Check | March 2025 | Active |

---

## Scan Results Summary: Q1 2025

### Amazon Inspector Infrastructure Scan

Amazon Inspector continuously scans all FHX EC2 instances, container images, and Lambda functions for known CVEs using the NVD and vendor vulnerability feeds.

| Severity | Q1 Start | Discovered | Remediated | Accepted Risk | Q1 End |
|---|---|---|---|---|---|
| Critical | 0 | 0 | 0 | 0 | 0 |
| High | 1 | 2 | 3 | 0 | 0 |
| Medium | 5 | 12 | 14 | 0 | 3 |
| Low | 18 | 31 | 35 | 0 | 14 |
| Informational | 42 | 67 | 52 | 0 | 57 |

**Key Q1 Findings Remediated:**
- CVE-2024-21626 (runC container escape): Patched within 24 hours of discovery; critical severity
- CVE-2024-1086 (Linux kernel privilege escalation): Patched within 48 hours; high severity
- Three medium-severity OpenSSL vulnerabilities: Patched within 7 days as part of monthly patch cycle

### AWS Security Hub Findings

AWS Security Hub aggregates findings from Inspector, GuardDuty, Macie, Config, and IAM Access Analyzer.

| Standard | Total Findings | Critical | High | Medium | Passing Controls |
|---|---|---|---|---|---|
| AWS Foundational Security Best Practices | 8 | 0 | 1 | 7 | 98.2% |
| NIST SP 800-53 Rev 5 | 4 | 0 | 0 | 4 | 99.1% |
| PCI-DSS v4.0 | 3 | 0 | 0 | 3 | 99.4% |
| HIPAA | 0 | 0 | 0 | 0 | 100% |
| CIS AWS Foundations Benchmark | 6 | 0 | 0 | 6 | 97.8% |

All findings are tracked in the remediation tracker and assigned to the DevOps Lead (Trevor L. Abrams) for remediation within applicable SLAs.

### SAST Results Summary (Q1)

The Checkmarx SAST scan runs on every pull request and full branch scan runs weekly.

| Severity | Q1 Findings | Remediated Before Merge | Remediated Post-Merge | Open |
|---|---|---|---|---|
| Critical | 0 | 0 | 0 | 0 |
| High | 2 | 2 | 0 | 0 |
| Medium | 7 | 5 | 2 | 0 |
| Low | 14 | 8 | 6 | 0 |

FHX CI/CD pipeline blocks merges to main if any Critical or High SAST findings exist.

### Penetration Test Summary (FY2025)

The FY2025 annual penetration test was conducted by ClearPath Security Assessors LLC in February 2025. The test scope included: external network penetration testing, internal network testing, web application testing, API testing, and CDE segmentation validation.

| Finding | Severity | Status |
|---|---|---|
| PT-2025-001: TLS session renegotiation enabled on Payment API | Medium | Remediated March 2025 |
| PT-2025-002: Missing HTTP security headers on patient portal | Low | Remediated March 2025 |
| PT-2025-003: Verbose error messages in API edge cases | Informational | In Progress (May 2025) |

**Overall Assessment:** No critical or high findings. Segmentation controls validated. FHX security posture rated "Strong" by ClearPath.

---

## Related Documents

- [Asset Inventory](../inventory/README.md)
- [Risk Classification](../risk-classification/README.md)
- [Remediation Tracking](../remediation-tracking/README.md)
- [Validation Scans](../validation/README.md)
- [PCI-DSS Scan Results](https://github.com/enechi-njeze/cloudvault-pci-dss/blob/main/scan-results/README.md)

---

*Prepared by Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, CompTIA Security+ SY0-701, CHP, CSCS, CISSP (in progress)*
*ISSO / ISSM, CloudVault Federal Health Exchange (FHX)*
*[LinkedIn](https://www.linkedin.com/in/enechi-njeze) | [Portfolio](https://github.com/enechi-njeze)*
