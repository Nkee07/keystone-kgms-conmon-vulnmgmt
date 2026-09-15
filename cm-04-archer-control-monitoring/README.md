# CM-04: Control Monitoring in RSA Archer
### Keystone Grants Management System (KGMS) | Control library, control state, evidence repository and the monitoring calendar

> ### PORTFOLIO EXERCISE, SIMULATED SYSTEM
> **Keystone Grants Management System (KGMS) does not exist.** The Federal Workforce Development Agency (FWDA) is a fictional federal
> agency invented for this portfolio. Every organisation, person, system
> identifier, network address, finding, date and signature on this page is
> fabricated for training purposes, with one exception: the author, Nkeiru Sarah Adesida, is a real person. Where her name appears in a scenario role, that role is fictional, and she has never worked for this agency, which does not exist. Nothing here is drawn from any real
> employer, client or government system, and no real document, artifact or
> data has been reproduced. This file is coursework, not a government record.

---

**Document:** Control Monitoring and Evidence Management Model  
**Simulated system:** Keystone Grants Management System (KGMS) | `FWDA-KGMS-2026-MOD`  
**Framework and standards:** NIST SP 800-53 Rev 5, CA-7 Continuous Monitoring, ISO/IEC 27001:2022 Clause 9.1  
**Author:** Nkeiru Sarah Adesida  
**Document date:** 31 August 2026  
**Status:** Complete

**Navigation:** [<- CM-03: POA&M Lifecycle in ServiceNow](../cm-03-poam-lifecycle-servicenow/README.md) &nbsp;|&nbsp; [Repository home](../README.md) &nbsp;|&nbsp; [CM-05: Security Control Assessment Procedures ->](../cm-05-security-control-assessment/README.md)

---

## Purpose

RSA Archer is where control state and evidence live. Scanning finds technical weaknesses
and ServiceNow moves remediation work, but neither answers the question an auditor actually
asks: **prove this control operated throughout the year.**

Plain language version: imagine a fire safety folder for a building. It holds the list of
every safety measure, who is responsible for each one, when each was last checked, and the
signed checking records going back years. When the inspector arrives you hand them the
folder instead of spending three weeks searching email.

---

## Section 1: What this system is for

| Purpose | Why the other tools cannot do it |
|---|---|
| Hold the control library: every applicable control, its owner, its test frequency | A scanner has no concept of a control. ServiceNow tracks work items, not control state |
| Hold the evidence, attached to the control it evidences | Evidence in a shared folder is evidence that cannot be found at audit |
| Record control test results over time | A scan report shows one moment. A control needs a history |
| Show which controls are currently degraded | A list of open tickets does not tell you which controls are affected |
| Map one control to several frameworks at once | Reporting the same control separately for NIST and ISO produces two versions of the truth |
| Retain records for the retention period | Evidence has to survive staff turnover, and it usually does not |

---

## Section 2: Control library structure

| Field | Purpose |
|---|---|
| Control identifier | NIST SP 800-53 Rev 5 identifier, for example AU-6 |
| Control name | The Rev 5 title. Using a Rev 4 title here is how stale catalogues get discovered |
| Framework mappings | The Annex A controls this maps to, so one test result reports to both frameworks |
| Responsibility | Provider, Customer or Shared. Shared controls carry the written split |
| Control owner | The named person accountable for the control operating, distinct from the person who tests it |
| Implementation statement | How this system implements it, synchronised with the System Security Plan |
| Test procedure | The SP 800-53A derived procedure, see CM-05 |
| Test frequency | From the monitoring calendar in CM-01 |
| Last test date and result | Satisfied, Other than satisfied, or Not tested this cycle |
| Current state | Effective, Degraded, or Not effective |
| Linked open items | POA&M items and scan findings currently affecting this control |
| Evidence | Artifacts attached to the control record, with a retention date |

### The distinction that matters: control owner versus tester

The control owner is accountable for the control working. The tester checks whether it
does. If they are the same person, the test is a self assessment and its value drops
sharply, because nobody marks their own homework badly. In this scenario the Cloud
Operations Lead owns configuration controls and the ISSO tests them; the ISSO owns
documentation controls and the internal audit function tests those.

---

## Section 3: Control state model

| State | Definition | Example on this system |
|---|---|---|
| Effective | Tested in the current cycle, satisfied, evidence retained | AC-3 Access Enforcement. Nine roles enforced server side, tested, evidence on file |
| Degraded | The control operates but with a known gap, tracked as an open item | AU-6. Daily review evidenced, weekly correlated review evidenced 2 of 4 weeks. POA-004 open |
| Not effective | The control does not achieve its objective | None currently. IA-2(1) was Not effective from assessment until 24 July 2026 |
| Not tested this cycle | Due but not yet tested in the current cycle. Not the same as effective | Rotates as the monthly test subset moves |

### Why Not tested is its own state

Treating an untested control as effective is the most common way a control dashboard
becomes dishonest. It produces a green board where green means "we have not looked",
and that board is then shown to an AO who reads it as assurance. Not tested is
uncomfortable to display, which is exactly why it must be displayed.

---

## Section 4: Control state at 31 August 2026

| Control | Name | State | Reason |
|---|---|---|---|
| AC-2 | Account Management | Effective | Tested July 2026. 25 account sample traced to approved requests |
| AC-3 | Access Enforcement | Effective | Tested June 2026 |
| AC-6(5) | Privileged Accounts | Effective | Remediated March 2026, group membership now under identity provider control. Retested August 2026 |
| AT-2 | Literacy Training and Awareness | Effective | 418 of 420 current, 2 inside grace |
| AU-6 | Audit Record Review, Analysis and Reporting | **Degraded** | Weekly correlated review evidenced 2 of 4 weeks in July. POA-004 open, automation due 31 August 2026 |
| AU-11 | Audit Record Retention | Effective | All 7 log groups corrected March 2026, verified August 2026 |
| CA-7 | Continuous Monitoring | Effective | This programme. Monthly reports delivered on time since authorisation |
| CM-6 | Configuration Settings | Effective | Continuous compliance active since July 2026. Two consecutive clean reports |
| CM-8 | System Component Inventory | **Degraded** | Container images added July 2026, but the pipeline does not yet register components automatically. POA-010 open |
| CP-4 | Contingency Plan Testing | **Degraded** | Plan updated 31 July 2026. Functional restore test not yet performed. POA-005 open, due 15 September 2026 |
| IA-2(1) | Multifactor Authentication | Effective | Reviewer portal federated 24 July 2026. All 65 reviewer accounts enrolled |
| IA-5 | Authenticator Management | Effective | Policy v2.0 published 15 July 2026, now matches configured behaviour |
| PS-4 | Personnel Termination | **Degraded** | Accounts disabled within one business day in all cases. Checklist evidence incomplete. POA-009 open |
| RA-5 | Vulnerability Monitoring and Scanning | **Degraded** | Two database instances outside scan scope in July. POA-011 open, due 14 August 2026 |
| SC-28 | Protection of Information at Rest | **Degraded** | Secondary region customer managed key created 30 July 2026. Snapshot re-encryption pending. POA-007 open |
| SI-2 | Flaw Remediation | **Degraded** | Transitive dependency vulnerability CVE-2021-44228 open. POA-012, due 30 August 2026 |
| SI-10 | Information Input Validation | Effective | Allow list validation plus output encoding. Retested by the assessor March 2026 |
| SR-3 | Supply Chain Controls and Processes | **Degraded** | Procedure finalised 24 July 2026, awaiting System Owner approval. POA-006 open |

| State | Count |
|---|---|
| Effective | 10 |
| Degraded | 8 |
| Not effective | 0 |
| **Controls shown** | **18** |

This is a reported subset of the controls in the library, chosen because each one either
produced a finding or is referenced elsewhere in the portfolio. Eight degraded controls
out of eighteen shown is not a flattering picture, and it is the accurate one: every open
POA&M item degrades at least one control, so a system with eight open items cannot have a
fully green control board. A dashboard showing all green while eight POA&M items are open
is a dashboard that is not connected to the POA&M.

---

## Section 5: Evidence repository

| Evidence type | Source | Frequency | Retention | Satisfies |
|---|---|---|---|---|
| Vulnerability scan reports | Tenable Nessus | Weekly and monthly | 3 years | RA-5, SI-2, A 8.8 |
| Configuration compliance reports | Platform compliance service | Continuous, snapshot monthly | 3 years | CM-6, A 8.9 |
| Access review attestations | Export plus supervisor signature | Quarterly | 3 years | AC-2, AC-6, A 5.18, A 8.2 |
| Weekly privileged activity review records | Correlated query output | Weekly | 3 years | AU-6, A 8.16 |
| Training completion reports | Agency training platform | Monthly | 3 years | AT-2, A 6.3 |
| Separation checklists | Human capital workflow | Per separation | 7 years | PS-4, A 6.5 |
| Change records and impact analyses | Change management | Per change | 3 years | CM-3, A 8.32 |
| Incident records | Security operations centre | Per incident | 7 years | IR-4 to IR-6, A 5.24 to 5.27 |
| POA&M closure evidence | ServiceNow, validated by the ISSO | Per closure | 3 years, or life of the authorisation | POA&M management, CA-5 |
| Payment reconciliation verification | Finance function confirmation | Monthly | 3 years | Underpins the integrity categorisation |
| Control test results | This programme | Monthly rolling | Life of the authorisation | CA-2, CA-7, Clause 9.1 |
| Assessment and audit reports | Assessor and internal audit | Annual | Life of the authorisation plus 3 years | CA-2, Clause 9.2 |

### Two rules about evidence

**Evidence is captured when the control runs, not when somebody asks for it.** Evidence
reconstructed at audit time is an assertion with a date on it. The correct time to attach
the weekly review record is the day of the review.

**Evidence must prove the control operated, not that the tool exists.** A screenshot of a
scanner's configuration page proves a scanner is configured. It does not prove a scan ran
last Monday. The scan report with its timestamp and target list does.

---

## Section 6: Monitoring calendar in practice

The monthly rolling test model: one twelfth of the control set is tested each month, so
every control is tested annually without a single annual crunch that nobody has capacity
for.

| Month | Control families in the test subset | Note |
|---|---|---|
| June 2026 | AC, IA | First cycle after authorisation. Started with access control because it produced the highest finding |
| July 2026 | AU, SI | AU-6 found degraded, consistent with POA-004 |
| August 2026 | CM, SR | CM-8 and SR-3 found degraded, consistent with POA-010 and POA-006 |
| September 2026 | CP, CA | Will include the functional restore test under POA-005 |
| October 2026 | SC, MP | Will include verification of the secondary region key under POA-007 |
| November 2026 | PS, AT, PL | Will include the separation checklist workflow change under POA-009 |
| December 2026 | RA, SA | Annual risk assessment refresh falls here |
| January to May 2027 | IR, PE, MA, remaining enhancements | Balance of the catalogue ahead of the annual assessment |

### Previously failed controls are always in scope

Every control that failed a previous assessment is retested every cycle, not once a year
in its turn. A closed finding is the single most likely finding to recur, because the
conditions that produced it usually still exist. On this system, CM-6 is retested monthly
rather than annually for that reason: configuration drift was found once and drift is a
recurring condition, not a one off event.

---

**Navigation:** [<- CM-03: POA&M Lifecycle in ServiceNow](../cm-03-poam-lifecycle-servicenow/README.md) &nbsp;|&nbsp; [Repository home](../README.md) &nbsp;|&nbsp; [CM-05: Security Control Assessment Procedures ->](../cm-05-security-control-assessment/README.md)

---

*Author: Nkeiru Sarah Adesida, Information System Security Officer (ISSO), portfolio scenario*  
*Simulated system: Keystone Grants Management System (KGMS) | FWDA-KGMS-2026-MOD*  
*This document is a portfolio exercise built on a fictional organisation. It is not a government record.*  
*[GitHub portfolio](https://github.com/Nkee07) | [LinkedIn](https://www.linkedin.com/in/nkeiru-adesida-grc)*
