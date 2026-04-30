# Remediation Tracking

**System:** CloudVault Federal Health Exchange (FHX)
**Repository:** cloudvault-vuln-mgmt
**Folder:** remediation-tracking
**Framework Reference:** NIST SP 800-53 CA-5 (POA&M) / FedRAMP ConMon / NIST SP 800-40
**Document ID:** FHX-VM-REM-001
**Prepared By:** Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, Security+, CHP, CSCS, CISSP (in progress)
**Role:** ISSO / ISSM, CloudVault Federal Health Exchange
**Last Updated:** April 2025
**Status:** Active

---

## Purpose

Vulnerability remediation tracking is the operational heart of the vulnerability management program. Discovering vulnerabilities creates no value unless those vulnerabilities are tracked from identification through remediation and validation. The FHX remediation tracking program uses the FedRAMP Plan of Action and Milestones (POA&M) as the authoritative tracking document for all open vulnerabilities and security findings, supplemented by an operational remediation tracker for day-to-day management.

This folder documents the FHX POA&M structure, the remediation workflow, and the current remediation status across all open findings.

---

## Documents in This Folder

| Document | Description | Reference | Status |
|---|---|---|---|
| poam-fy2025-q1.md | Q1 FY2025 POA&M with all open findings | FedRAMP ConMon CA-5 | Active |
| remediation-workflow.md | Step-by-step remediation workflow from assignment to closure | NIST SP 800-40 | Active |
| remediation-tracker-current.md | Current operational remediation tracker with status by finding | FedRAMP ConMon | Active |
| false-positive-log.md | Log of findings assessed as false positives with justification | FedRAMP ConMon | Active |
| risk-acceptance-log.md | Log of findings with formally accepted risk and justification | NIST SP 800-53 CA-5 | Active |

---

## POA&M Structure

The FHX POA&M follows the FedRAMP POA&M template format. Each entry contains:

- **POA&M ID:** Unique identifier (e.g., FHX-POAM-2025-001)
- **Control:** NIST SP 800-53 control affected (e.g., RA-5)
- **Weakness Name:** Brief description of the vulnerability or finding
- **Description:** Detailed description including CVE if applicable
- **Source:** How the finding was discovered (Amazon Inspector, Security Hub, penetration test, SAR finding)
- **Status:** Open, In Remediation, Completed, Risk Accepted, False Positive
- **Date Discovered:** When the finding was first identified
- **Scheduled Completion Date:** Target remediation date per SLA
- **Milestones:** Specific tasks and their completion status
- **Responsible Party:** Name and role of the remediation owner
- **Resources Required:** Time, tools, funding, or personnel needed

---

## Current POA&M Status (Q1 2025)

| POA&M ID | Finding | Severity | Source | Status | Owner | Target Date |
|---|---|---|---|---|---|---|
| FHX-POAM-2025-001 | Medium: AWS Foundational Best Practices: S3 bucket public access not fully restricted on logging bucket | Medium | Security Hub | In Remediation | Trevor L. Abrams | May 2025 |
| FHX-POAM-2025-002 | Medium: AWS Config Rule: root account MFA not enforced on secondary account | Medium | AWS Config | In Remediation | Enechi P.C. Njeze | May 2025 |
| FHX-POAM-2025-003 | Medium: Verbose error messages in Payment API edge cases (PT-2025-003) | Medium | Penetration Test | In Remediation | Trevor L. Abrams | May 2025 |
| FHX-POAM-2025-004 | Low: CIS Benchmark: CloudTrail log file validation not enabled on one trail | Low | Security Hub | In Remediation | Trevor L. Abrams | June 2025 |
| FHX-POAM-2025-005 | Low: Dependency: lodash 4.17.20 (low-severity prototype pollution) | Low | Dependency-Check | In Remediation | Trevor L. Abrams | June 2025 |

**Closed in Q1 2025:** 15 findings (2 High, 8 Medium, 5 Low)
**Total Open as of April 30, 2025:** 5 (0 Critical, 0 High, 3 Medium, 2 Low)

---

## Remediation Workflow

**Step 1: Assignment.** When a new finding is added to the POA&M, the ISSO assigns it to the responsible party (typically DevOps Lead Trevor L. Abrams for infrastructure/application findings, or ISSO for process/policy findings) and sets the remediation target date based on the risk tier SLA.

**Step 2: Remediation Planning.** For complex findings, the responsible party creates a remediation plan with specific milestones, testing steps, and a deployment window. The plan is reviewed by the ISSO before work begins.

**Step 3: Implementation.** Remediation is implemented through the standard change management process. All vulnerability remediations require an approved Change Request and follow the FHX SDLC (for application patches) or infrastructure change procedure (for infrastructure changes). Emergency patching follows the emergency change procedure.

**Step 4: Validation.** After remediation, a validation scan is run to confirm the vulnerability no longer exists. See [validation/README.md](../validation/README.md) for validation procedures.

**Step 5: POA&M Closure.** Once validation confirms remediation, the ISSO updates the POA&M entry status to "Completed" with the actual completion date and validation scan reference.

---

## Risk Acceptance Policy

When a vulnerability cannot be remediated within the standard SLA due to technical constraints, operational impact, or dependency on a third party, the ISSO may pursue formal risk acceptance. Risk acceptance requires:

1. Written justification explaining why the vulnerability cannot be remediated within SLA
2. Assessment of compensating controls that reduce the risk to an acceptable level
3. ISSO and System Owner (Dr. Patricia Owens) sign-off
4. AO (Deputy Director Henry Kline) notification and concurrence for High or Critical vulnerabilities
5. Scheduled re-evaluation date (maximum 180 days for Medium, 90 days for High)

No Critical or High vulnerabilities currently have accepted risk in FHX.

---

## Related Documents

- [Asset Inventory](../inventory/README.md)
- [Scan Reports](../scan-reports/README.md)
- [Risk Classification](../risk-classification/README.md)
- [Validation Scans](../validation/README.md)
- [Vulnerability Metrics](../metrics/README.md)
- [FedRAMP Continuous Monitoring](https://github.com/enechi-njeze/cloudvault-nist-rmf/blob/main/step6-monitor/README.md)

---

*Prepared by Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, CompTIA Security+ SY0-701, CHP, CSCS, CISSP (in progress)*
*ISSO / ISSM, CloudVault Federal Health Exchange (FHX)*
*[LinkedIn](https://www.linkedin.com/in/enechi-njeze) | [Portfolio](https://github.com/enechi-njeze)*
