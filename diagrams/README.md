# Vulnerability Management Diagrams and Process Flows

**System:** CloudVault Federal Health Exchange (FHX)
**Framework Reference:** NIST SP 800-137, FedRAMP Continuous Monitoring Strategy Guide
**Document ID:** FHX-VULN-DIAG-001
**Prepared By:** Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, Security+, CHP, CSCS, CISSP (in progress)
**Role:** ISSO / ISSM, CloudVault FHX
**Review Cycle:** Annual (or upon significant architecture change)
**Classification:** Internal Use Only

---

## Purpose

This folder contains the process flow diagrams, workflow maps, and architectural reference diagrams that support the CloudVault FHX vulnerability management program. Visual documentation serves a critical role in a mature GRC practice: it communicates complex workflows to diverse audiences including engineers, auditors, executive leadership, and FedRAMP assessors.

During a FedRAMP assessment or FISMA audit, diagrams in this folder help the 3PAO (ClearPath Security Assessors LLC) verify that the described vulnerability management processes are real, repeatable, and correctly implemented. They also support control inheritance claims under CA-7 (Continuous Monitoring) and RA-5 (Vulnerability Monitoring and Scanning).

Each diagram in this folder is accompanied by a textual description that explains the flow, identifies the responsible parties, and cross-references the related policy or procedure document.

---

## Documents in This Folder

| Document | Description | Reference | Status |
|---|---|---|---|
| FHX-VULN-DIAG-001.md | This document (diagrams index and descriptions) | NIST SP 800-137 | Current |
| vuln-lifecycle-flow.md | End-to-end vulnerability lifecycle from discovery to verified closure | FedRAMP ConMon Guide | Current |
| scan-architecture-diagram.md | Scanning tool architecture: Tenable Nessus, Amazon Inspector, Qualys VMDR placement in AWS GovCloud | NIST SP 800-137 | Current |
| remediation-workflow.md | Remediation triage and assignment workflow from ISSO to DevOps to verification | FedRAMP ConMon | Current |
| conmon-reporting-flow.md | Monthly continuous monitoring report generation and submission workflow | FedRAMP ConMon Guide | Current |
| poam-lifecycle-diagram.md | POA&M item lifecycle from creation through AO acceptance to closure | FedRAMP POA&M Template | Current |
| escalation-matrix.md | Escalation paths for Critical and High findings exceeding SLA thresholds | FHX IRP | Current |
| tool-integration-diagram.md | Integration map: scanner outputs to SIEM, ticketing, and POA&M register | FHX SSP | Current |

---

## Vulnerability Lifecycle Flow

The vulnerability lifecycle at CloudVault FHX follows a six-stage process aligned to NIST SP 800-137 and the FedRAMP Continuous Monitoring Strategy Guide. Every vulnerability, regardless of severity, travels this path from initial discovery to verified closure.

**Stage 1: Discovery**
Automated scanners (Tenable Nessus, Amazon Inspector, Qualys VMDR) run on a continuous and scheduled basis. Authenticated scans run monthly at minimum; unauthenticated network scans run weekly. Container image scans run at build time via CI/CD pipeline integration. Scanner output is aggregated in the centralized vulnerability management platform.

**Stage 2: Triage and Classification**
The DevOps security team reviews raw scanner output within 24 hours of scan completion. Each finding is assigned a severity tier (Critical, High, Moderate, Low) based on CVSS base score and FHX contextual factors. Findings are deduplicated across scanners using CVE identifier matching. False positive candidates are flagged for ISSO review.

**Stage 3: Assignment**
Validated findings are assigned to the responsible remediation owner via the ticketing system (Jira). Ticket creation is automated through scanner API integration. SLA countdown begins at ticket creation. ISSO receives daily digest of newly created tickets.

**Stage 4: Remediation**
Remediation owners patch, upgrade, reconfigure, or apply compensating controls within the SLA window. For Critical findings, the DevOps Lead Trevor Abrams personally reviews and approves the remediation plan within 24 hours of ticket creation. Emergency patching uses the hot-patch pipeline with change management exception documentation.

**Stage 5: Verification**
After remediation is marked complete by the owner, the ISSO or designated verifier performs independent validation: re-scan to confirm the vulnerability is absent, review of patch version, or testing of the remediated configuration. Validation must be completed before the ticket is closed. See the validation/ folder for detailed verification procedures.

**Stage 6: Closure and Reporting**
Verified closures are recorded in the vulnerability management platform and the monthly ConMon report. Closed POA&M items receive ISSO sign-off and are submitted to the FedRAMP PMO in the next monthly report cycle. Trend data feeds the metrics dashboard documented in the metrics/ folder.

---

## Scan Architecture Overview

CloudVault FHX operates a multi-tool scanning architecture within AWS GovCloud (us-gov-west-1 and us-gov-east-1). The architecture is designed for complete asset coverage with no single point of scanning failure.

**Tenable Nessus Professional**
Deployed as two scanner instances within the FHX VPC. One scanner covers the primary compute tier (EC2 instances, RDS, EKS worker nodes). The second scanner covers management workstations and network devices. Nessus performs authenticated credentialed scans monthly and unauthenticated network sweeps weekly. Scan credentials are stored in AWS Secrets Manager with rotation every 90 days.

**Amazon Inspector**
Native AWS service enabled across all accounts in the FHX AWS Organization. Inspector provides continuous vulnerability assessment for EC2 instances and Lambda functions using the AWS Systems Manager (SSM) agent. Inspector findings are streamed to AWS Security Hub and then forwarded to the centralized SIEM (Splunk Enterprise Security) via EventBridge.

**Qualys VMDR**
Qualys Cloud Agent deployed on all EC2 instances that run CMS-facing application workloads. Qualys provides continuous real-time assessment between Nessus monthly scans. Qualys findings are ingested via API into the vulnerability management platform for deduplication and unified reporting.

**CI/CD Integration**
Container image scanning is performed at build time using Amazon Inspector image scanning within the CI/CD pipeline (AWS CodePipeline). Images with Critical or High findings are blocked from deployment pending ISSO review. This gate prevents new vulnerabilities from entering the environment via container deployments.

---

## Remediation Workflow

The remediation workflow maps the handoff path from ISSO triage through DevOps execution to ISSO verification.

| Step | Actor | Action | SLA |
|---|---|---|---|
| 1 | Scanner / Inspector | Automated finding generated and logged | Immediate |
| 2 | DevOps Security | Triage finding: classify, deduplicate, flag FP candidates | Within 24 hours |
| 3 | ISSO | Review FP candidates, approve or reject | Within 48 hours |
| 4 | ISSO | Create Jira ticket, assign to remediation owner, set SLA due date | Within 48 hours |
| 5 | Remediation Owner | Acknowledge ticket, review finding, develop remediation plan | Within 72 hours (Critical) |
| 6 | DevOps Lead | Approve remediation plan for Critical/High findings | Within 96 hours (Critical) |
| 7 | Remediation Owner | Apply patch, configuration change, or compensating control | Per SLA by severity |
| 8 | Remediation Owner | Update Jira ticket to "Remediation Complete" with evidence | Same day as remediation |
| 9 | ISSO / Verifier | Independent validation: re-scan, log review, or config check | Within 5 days of Step 8 |
| 10 | ISSO | Close ticket, update POA&M register, include in ConMon report | Within 2 days of Step 9 |

---

## Continuous Monitoring Reporting Flow

The monthly ConMon report is the primary mechanism for communicating vulnerability management posture to the FedRAMP PMO and Authorizing Official Henry Kline.

**Week 1 of Month:**
- All scanners complete monthly authenticated scan cycle
- DevOps security team completes triage of all findings from current month
- ISSO reviews triage results and updates vulnerability inventory

**Week 2 of Month:**
- ISSO drafts ConMon report sections 1-4 (Executive Summary, Scan Coverage, Vulnerability Inventory, New Findings)
- DevOps Lead provides remediation status updates for all open High and Critical tickets
- Privacy Officer Sandra Voss reviews for any PHI/PII-adjacent findings

**Week 3 of Month:**
- ISSO completes ConMon report sections 5-10 (Closures, SLA Compliance, POA&M Status, FP Log, Exceptions, Outlook)
- CISO Angela Torres reviews draft report
- System Owner Dr. Patricia Owens provides system change notifications for inclusion

**Last Business Day of Month:**
- ISSO finalizes ConMon report
- Report submitted to FedRAMP PMO via the FedRAMP Secure Repository
- Copy submitted to AO Henry Kline for awareness
- Report archived in continuous monitoring records

---

## POA&M Lifecycle

A Plan of Action and Milestones (POA&M) item is created for any vulnerability that cannot be remediated within its SLA window or that requires AO risk acceptance. The POA&M lifecycle at FHX follows the FedRAMP POA&M template and NIST SP 800-18 guidance.

| Phase | Trigger | Actor | Action |
|---|---|---|---|
| Creation | Finding exceeds 75% of SLA window without closure | ISSO | Create POA&M entry with finding details, scheduled completion date, milestones |
| Review | POA&M entry created | ISSO, System Owner | Review compensating controls, validate risk acceptance rationale |
| AO Submission | Monthly ConMon report cycle | ISSO | Include POA&M in ConMon report submitted to AO |
| AO Acceptance | AO reviews ConMon report | AO Henry Kline | Accept risk, request accelerated remediation, or reject with escalation |
| Milestone Tracking | Each monthly ConMon cycle | ISSO | Update POA&M with milestone completion status |
| Closure | Remediation verified | ISSO | Close POA&M item, document evidence, include in ConMon report |
| Audit | Annual assessment | 3PAO (ClearPath) | Review POA&M aging, closure evidence, AO acceptance documentation |

---

## Escalation Matrix

When vulnerabilities approach or exceed SLA thresholds, a defined escalation path ensures leadership visibility and accelerated response.

| Condition | Escalation Level | Notified Parties | Required Action | Timeframe |
|---|---|---|---|---|
| Critical finding open for 10 days (SLA: 15 days) | Level 1 | ISSO, DevOps Lead | Daily status update, accelerated patch plan | Immediate |
| Critical finding open for 14 days (SLA breach imminent) | Level 2 | CISO Angela Torres, System Owner Dr. Owens | Executive briefing, resource reallocation authorized | Same day |
| Critical finding breaches 15-day SLA | Level 3 | AO Henry Kline, CISO | POA&M created, written risk acceptance required, out-of-cycle ConMon report | Within 24 hours |
| High finding open for 25 days (SLA: 30 days) | Level 1 | ISSO, DevOps Lead | Status update, remediation plan review | Within 48 hours |
| High finding breaches 30-day SLA | Level 2 | CISO, System Owner | POA&M created, compensating control required | Within 48 hours |
| Scan coverage drops below 95% | Level 2 | ISSO, DevOps Lead, CISO | Emergency scan initiation, root cause analysis | Within 24 hours |
| 3 or more Critical findings open simultaneously | Level 2 | CISO, AO | Special briefing, incident response pre-assessment | Within 48 hours |

---

## Tool Integration Architecture

The FHX vulnerability management tools integrate with the broader security operations infrastructure to ensure findings flow seamlessly from discovery to tracking to reporting.

| Source Tool | Integration | Destination | Purpose | Frequency |
|---|---|---|---|---|
| Tenable Nessus | API export | Vulnerability Management Platform | Centralized finding aggregation | Post-scan (monthly) |
| Amazon Inspector | EventBridge to Security Hub | SIEM (Splunk Enterprise Security) | Real-time alerting for Critical findings | Continuous |
| Qualys VMDR | API export | Vulnerability Management Platform | Continuous assessment data | Daily |
| Vulnerability Management Platform | Webhook | Jira Service Management | Automated ticket creation per validated finding | Within 24 hours of validation |
| Jira | API | POA&M Register | SLA tracking and POA&M population | Daily sync |
| Jira | API | ConMon Report Template | Monthly report data population | Monthly |
| SIEM (Splunk) | Dashboard | ISSO Metrics Dashboard | Real-time vulnerability posture view | Real-time |
| ConMon Report | Manual upload | FedRAMP Secure Repository | FedRAMP PMO submission | Monthly |

---

## Related Documents

- [Scan Reports](../scan-reports/README.md)
- [Remediation Tracking](../remediation-tracking/README.md)
- [Risk Classification](../risk-classification/README.md)
- [Validation Records](../validation/README.md)
- [Vulnerability Metrics](../metrics/README.md)
- [FedRAMP ConMon](https://github.com/enechi-njeze/cloudvault-nist-rmf/blob/main/step6-monitor/README.md)
- [NIST RMF Step 6](https://github.com/enechi-njeze/cloudvault-nist-rmf/blob/main/step6-monitor/README.md)

---

*Prepared by:* **Enechi P.C. Njeze**, CGRC, CISA, CISM, PMP, PMI-RMP, CompTIA Security+ SY0-701, CHP, CSCS, CISSP (in progress)
*Role:* ISSO / ISSM, CloudVault Federal Health Exchange
*LinkedIn:* [linkedin.com/in/enechi-njeze](https://linkedin.com/in/enechi-njeze)
*Portfolio:* [github.com/enechi-njeze](https://github.com/enechi-njeze)
