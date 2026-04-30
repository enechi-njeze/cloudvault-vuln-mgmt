# Vulnerability Risk Classification

**System:** CloudVault Federal Health Exchange (FHX)
**Repository:** cloudvault-vuln-mgmt
**Folder:** risk-classification
**Framework Reference:** CVSS v3.1 / NIST NVD / CISA KEV / FedRAMP ConMon
**Document ID:** FHX-VM-RCL-001
**Prepared By:** Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, Security+, CHP, CSCS, CISSP (in progress)
**Role:** ISSO / ISSM, CloudVault Federal Health Exchange
**Last Updated:** April 2025
**Status:** Active

---

## Purpose

Not all vulnerabilities are equal. A CVSS 7.5 vulnerability on an internet-facing API that handles PHI is a far greater operational risk than the same CVSS score on an internal logging component with no external access. Effective vulnerability risk classification adjusts for environmental context, asset criticality, exposure, and exploitability to produce an accurate picture of true risk rather than relying solely on the base CVSS score.

This folder documents the FHX vulnerability risk classification methodology: how each discovered vulnerability is scored, contextualized, and prioritized for remediation.

---

## Documents in This Folder

| Document | Description | Reference | Status |
|---|---|---|---|
| risk-classification-methodology.md | Full FHX vulnerability risk classification methodology | CVSS v3.1, NIST NVD | Active |
| environmental-scoring-guide.md | Guide for calculating CVSS environmental scores for FHX assets | CVSS v3.1 Specification | Active |
| risk-tier-definitions.md | FHX risk tier definitions with remediation SLAs | FedRAMP ConMon | Active |
| cisa-kev-tracking.md | Tracking of CISA Known Exploited Vulnerabilities (KEV) affecting FHX | CISA KEV Catalog | Active |

---

## Risk Classification Methodology

### Step 1: Base CVSS Score

All vulnerabilities are assigned a base CVSS v3.1 score from the National Vulnerability Database (NVD) or the scanner's built-in scoring. The base score represents the intrinsic characteristics of the vulnerability independent of environment.

### Step 2: Environmental Score Adjustment

The FHX ISSO adjusts each base score using CVSS v3.1 environmental metrics for the specific affected asset:

**Confidentiality Requirement (CR):** HIGH for all PHI-processing assets, MEDIUM for management assets, LOW for monitoring-only assets.

**Integrity Requirement (IR):** HIGH for all production data stores and application logic, MEDIUM for supporting services.

**Availability Requirement (AR):** HIGH for all patient-facing and agency-facing services, MEDIUM for internal management systems.

**Modified Attack Vector:** If the affected asset is in a private subnet with no internet access, the attack vector may be downgraded (e.g., from Network to Adjacent).

**Modified Privileges Required:** If the affected asset requires MFA and privileged access to exploit, the exploitability metrics are adjusted.

### Step 3: CISA KEV Check

Every vulnerability is checked against the CISA Known Exploited Vulnerabilities (KEV) catalog. Vulnerabilities in the CISA KEV catalog are automatically escalated to the next higher risk tier, regardless of CVSS score, because active exploitation in the wild has been confirmed.

### Step 4: FHX Risk Tier Assignment

| Risk Tier | Criteria | Remediation SLA | Escalation |
|---|---|---|---|
| Tier 1 (Critical) | Adjusted CVSS 9.0-10.0 OR CISA KEV with CVSS 7.0+ | 72 hours | ISSO + CISO + AO notification within 1 hour |
| Tier 2 (High) | Adjusted CVSS 7.0-8.9 OR CISA KEV with CVSS under 7.0 | 7 days | ISSO notification within 4 hours |
| Tier 3 (Medium) | Adjusted CVSS 4.0-6.9 (no KEV listing) | 30 days | Monthly remediation review |
| Tier 4 (Low) | Adjusted CVSS 0.1-3.9 | 90 days | Quarterly remediation review |
| Tier 5 (Informational) | CVSS 0.0 or configuration recommendation | 180 days (best effort) | Annual review |

### Step 5: Exploitation Context Assessment

For Tier 1 and Tier 2 vulnerabilities, the ISSO conducts an additional exploitation context assessment:

- Is exploit code publicly available? (Exploit-DB, Metasploit, PoC-in-GitHub)
- Has exploitation been observed in similar environments (healthcare, federal government)?
- Does the vulnerability chain with other vulnerabilities to create a higher-risk attack path?
- Are compensating controls in place that reduce the likelihood of exploitation?

This assessment informs whether to escalate the remediation SLA (if exploitation is imminent) or accept a brief extension (if compensating controls are demonstrably effective).

---

## CISA KEV Tracking

The CISA KEV catalog is checked daily by automated feed. When a new KEV entry matches an installed software version in FHX, the ISSO is notified within 30 minutes via PagerDuty. As of April 30, 2025, no open FHX vulnerabilities appear in the CISA KEV catalog.

**Historical KEV Remediations (FY2025):**

| CVE | CISA KEV Addition | FHX Impact | Remediation Date | Time to Remediate |
|---|---|---|---|---|
| CVE-2024-21626 | January 2025 | Yes: runC version in use | January 2025 (within 24 hours) | 22 hours |
| CVE-2024-1086 | February 2025 | Yes: Linux kernel version in use | February 2025 (within 48 hours) | 41 hours |

---

## Risk Classification Examples

| CVE | Base CVSS | Asset | CR/IR/AR | Adjusted CVSS | KEV | FHX Risk Tier | SLA |
|---|---|---|---|---|---|---|---|
| CVE-2024-21626 | 8.6 | ECS containers (PHI processing) | HIGH/HIGH/HIGH | 9.4 | Yes | Tier 1 (Critical) | 72 hours |
| CVE-2024-1086 | 7.8 | EC2 (Management VPC) | MEDIUM/HIGH/MEDIUM | 8.1 | Yes | Tier 1 (Critical, KEV escalation) | 72 hours |
| CVE-2024-XXXX | 6.5 | CloudWatch log analysis Lambda | LOW/LOW/LOW | 4.2 | No | Tier 3 (Medium) | 30 days |
| CVE-2024-XXXX | 5.3 | Non-PHI S3 bucket encryption issue | MEDIUM/HIGH/LOW | 5.8 | No | Tier 3 (Medium) | 30 days |

---

## Related Documents

- [Asset Inventory](../inventory/README.md)
- [Scan Reports](../scan-reports/README.md)
- [Remediation Tracking](../remediation-tracking/README.md)
- [Vulnerability Metrics](../metrics/README.md)
- [FedRAMP POA&M](https://github.com/enechi-njeze/cloudvault-fedramp-ato/blob/main/README.md)
- [NIST RMF Risk Assessment](https://github.com/enechi-njeze/cloudvault-nist-rmf/blob/main/step4-assess/README.md)

---

*Prepared by Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, CompTIA Security+ SY0-701, CHP, CSCS, CISSP (in progress)*
*ISSO / ISSM, CloudVault Federal Health Exchange (FHX)*
*[LinkedIn](https://www.linkedin.com/in/enechi-njeze) | [Portfolio](https://github.com/enechi-njeze)*
