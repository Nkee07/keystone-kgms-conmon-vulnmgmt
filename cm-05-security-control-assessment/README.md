# CM-05: Security Control Assessment Procedures
### Keystone Grants Management System (KGMS) | Objectives, methods, sampling decisions and 10 worked test cases

> ### PORTFOLIO EXERCISE, SIMULATED SYSTEM
> **Keystone Grants Management System (KGMS) does not exist.** The Federal Workforce Development Agency (FWDA) is a fictional federal
> agency invented for this portfolio. Every organisation, person, system
> identifier, network address, finding, date and signature on this page is
> fabricated for training purposes. Nothing here is drawn from any real
> employer, client or government system, and no real document, artifact or
> data has been reproduced. This file is coursework, not a government record.

---

**Document:** Security Control Assessment Procedures and Test Cases  
**Simulated system:** Keystone Grants Management System (KGMS) | `FWDA-KGMS-2026-MOD`  
**Framework and standards:** NIST SP 800-53A Rev 5, NIST SP 800-53 Rev 5, CA-2  
**Author:** Nkeiru Sarah Adesida  
**Document date:** 31 August 2026  
**Status:** Complete

**Navigation:** [<- CM-04: Control Monitoring in RSA Archer](../cm-04-archer-control-monitoring/README.md) &nbsp;|&nbsp; [Repository home](../README.md) &nbsp;|&nbsp; [CM-06: Metrics and Reporting ->](../cm-06-metrics-reporting/README.md)

---

## Purpose

A security control assessment determines whether a control actually does what the
documentation claims. This document is the procedures, built from NIST SP 800-53A Rev 5,
and ten worked test cases from the assessment of this system.

Plain language version: somebody wrote down how the system is protected. This is the
method for finding out whether that is true, by reading records, asking people, and
poking the system to see what it really does.

---

## Section 1: The three methods

SP 800-53A defines three assessment methods. The reason most controls need more than one is
worth stating plainly, because it is the whole craft of assessment:

| Method | What it establishes | What it cannot establish |
|---|---|---|
| Examine | What the records and configuration say | Whether the documented thing actually happens, or happened last Tuesday |
| Interview | What the people who operate the control believe happens | Whether their belief matches the system's behaviour |
| Test | What the system actually does when exercised | Why it does that, or whether the process around it is sound |

**The findings are where the three answers disagree.** Three examples from this system:

- **AU-6.** Interview said the weekly review happens. Examine found records for 6 of 12
  weeks. The interview was honest and the records were incomplete, so the finding is about
  evidence, not about effort.
- **IA-2(1).** The documentation said multifactor authentication was required. Test found
  that a password alone was accepted for the reviewer population. Examine alone would have
  passed this control.
- **SR-3.** Examine found a procedure. Interview found that the development team it applied
  to had never seen it. A document nobody follows is not a control.

---

## Section 2: Assessment objects and sampling

| Decision | Rule applied | Reason |
|---|---|---|
| When to sample | Population is large and homogeneous, and one miss does not invalidate the conclusion | Efficiency without loss of confidence |
| When to test the full population | Population is small, or a single exception invalidates the control | Privileged accounts, input fields, compute instances |
| How to stratify a sample | Weight toward the highest risk stratum | An unapproved privileged account matters more than an unapproved read only account |
| How to sample a time series | Consecutive periods, not scattered ones | A gap in cadence is visible in consecutive weeks and averages away in scattered ones |

### The three full population decisions on this system, and why

- **Privileged accounts, 16 of 16.** One unapproved administrator invalidates the control.
  A sample that misses it produces a Satisfied result on a failed control.
- **Input fields, 31 of 31.** The control is as strong as its weakest field. A sample of 10
  fields that all pass tells you nothing about the other 21, and the finding was in a field
  a proportional sample would probably have missed.
- **Compute instances, 11 of 11.** Configuration drift is not uniformly distributed. It
  clusters on the hosts that were touched manually during an incident, which is precisely
  where a random sample is least likely to look.

---

## Section 3: Test case register

| Test case | Control | Methods | Expected result | Result |
|---|---|---|---|---|
| TC-AC-02-01 | AC-2 | Examine, Interview | Every sampled account has a completed request naming requester, approv... | Satisfied |
| TC-AC-02-02 | AC-2 | Examine, Test | Account disabled within one business day of the separation notificatio... | Satisfied |
| TC-AC-06-01 | AC-6(5) | Examine, Test | Every member has a documented approval, and membership matches the app... | Other than satisfied at initial assessment |
| TC-AU-06-01 | AU-6 | Examine, Interview | A retained review record exists for each of the 12 weeks, showing peri... | Other than satisfied |
| TC-CM-06-01 | CM-6 | Test | Each instance matches the approved baseline with no deviation | Other than satisfied |
| TC-IA-02-01 | IA-2(1) | Test | Single factor authentication is rejected for every population | Other than satisfied |
| TC-RA-05-01 | RA-5 | Examine, Test | Scans run weekly, authenticated, with target list equal to the invento... | Other than satisfied |
| TC-SC-28-01 | SC-28 | Test, Examine | All encrypted, with a customer managed key under agency key policy | Other than satisfied |
| TC-SI-10-01 | SI-10 | Test | No field accepts or stores input that executes when rendered | Other than satisfied |
| TC-SR-03-01 | SR-3 | Examine, Interview | An approved procedure exists and is applied to component acquisition | Other than satisfied |

---

## Section 4: Test case detail

### TC-AC-02-01: AC-2 Account Management

| Element | Detail |
|---|---|
| Assessment objective | Determine whether accounts are created only on approved request, and whether the approval is recorded |
| Methods | Examine, Interview |
| Assessment objects | 25 accounts sampled across 3 populations; the access request record for each |
| Expected result | Every sampled account has a completed request naming requester, approver, role and justification |
| Actual result | Satisfied. 25 of 25 traced |
| Sampling decision | Sample stratified toward privileged accounts rather than proportional, because that is where an unapproved account matters |


### TC-AC-02-02: AC-2 Account Management

| Element | Detail |
|---|---|
| Assessment objective | Determine whether accounts are disabled promptly on separation |
| Methods | Examine, Test |
| Assessment objects | All 9 separations in the period; identity provider audit log |
| Expected result | Account disabled within one business day of the separation notification |
| Actual result | Satisfied. 9 of 9 within one business day |
| Sampling decision | Tested against the identity provider log rather than the separation checklist, because the log is system generated and the checklist is human completed |


### TC-AC-06-01: AC-6(5) Privileged Accounts

| Element | Detail |
|---|---|
| Assessment objective | Determine whether privileged group membership is limited to approved individuals |
| Methods | Examine, Test |
| Assessment objects | All 16 privileged accounts across 3 groups, full population |
| Expected result | Every member has a documented approval, and membership matches the approved list exactly |
| Actual result | Other than satisfied at initial assessment. 3 of 8 application administrators had no approval record. Remediated and retested |
| Sampling decision | Full population, not a sample. A sample that misses the one unapproved administrator tells you nothing |


### TC-AU-06-01: AU-6 Audit Record Review

| Element | Detail |
|---|---|
| Assessment objective | Determine whether audit records are reviewed at the defined frequency and whether the review is evidenced |
| Methods | Examine, Interview |
| Assessment objects | 12 consecutive weeks of weekly correlated review records |
| Expected result | A retained review record exists for each of the 12 weeks, showing period, reviewer, queries run and outcome |
| Actual result | Other than satisfied. Evidence for 6 of 12 weeks. Interviews indicated the review was largely performed but produced no artifact when nothing was found |
| Sampling decision | 12 consecutive weeks rather than 12 scattered weeks, so a break in cadence is visible rather than averaged away |


### TC-CM-06-01: CM-6 Configuration Settings

| Element | Detail |
|---|---|
| Assessment objective | Determine whether running configuration matches the approved baseline |
| Methods | Test |
| Assessment objects | All 11 compute instances, full population |
| Expected result | Each instance matches the approved baseline with no deviation |
| Actual result | Other than satisfied. 4 of 11 deviated. One was missing audit rules for privileged command execution, which also degraded AU family assurance on that host |
| Sampling decision | Automated comparison against the baseline rather than manual inspection, and full population because drift is not uniformly distributed |


### TC-IA-02-01: IA-2(1) Multifactor Authentication

| Element | Detail |
|---|---|
| Assessment objective | Determine whether multifactor authentication is enforced for all user populations |
| Methods | Test |
| Assessment objects | All 3 user populations; a test authentication attempt per population |
| Expected result | Single factor authentication is rejected for every population |
| Actual result | Other than satisfied. Agency population enforced via PIV card. Reviewer population accepted a password alone |
| Sampling decision | Tested by attempting authentication rather than by reading the configuration, because configuration intent and enforced behaviour are different things |


### TC-RA-05-01: RA-5 Vulnerability Monitoring and Scanning

| Element | Detail |
|---|---|
| Assessment objective | Determine whether scanning is performed at the defined frequency, with credentials, across the full component inventory |
| Methods | Examine, Test |
| Assessment objects | 13 weeks of scan reports; the component inventory; the scan target group |
| Expected result | Scans run weekly, authenticated, with target list equal to the inventory |
| Actual result | Other than satisfied. Database tier scanned unauthenticated. Recurrence found July 2026: 2 instances outside scope |
| Sampling decision | Compared the scan target list against the inventory rather than only confirming scans ran. A scan that runs on the wrong list still runs |


### TC-SC-28-01: SC-28 Protection of Information at Rest

| Element | Detail |
|---|---|
| Assessment objective | Determine whether information at rest is encrypted under a customer managed key |
| Methods | Test, Examine |
| Assessment objects | All storage volumes, database storage, object stores and snapshots in both regions |
| Expected result | All encrypted, with a customer managed key under agency key policy |
| Actual result | Other than satisfied. Primary region satisfied. Secondary region snapshots used the default service managed key |
| Sampling decision | Both regions in scope. A single region test would have passed and missed a full copy of the data |


### TC-SI-10-01: SI-10 Information Input Validation

| Element | Detail |
|---|---|
| Assessment objective | Determine whether user supplied input is validated and safely rendered |
| Methods | Test |
| Assessment objects | All 31 input fields plus the file upload path, full population |
| Expected result | No field accepts or stores input that executes when rendered |
| Actual result | Other than satisfied. Stored cross site scripting in the rich text project narrative field. Remediated and retested with the original payload plus 4 variants |
| Sampling decision | Full population and multiple payload variants. Input validation is only as strong as the weakest field, so sampling measures the wrong thing |


### TC-SR-03-01: SR-3 Supply Chain Controls and Processes

| Element | Detail |
|---|---|
| Assessment objective | Determine whether documented supply chain processes exist and are approved |
| Methods | Examine, Interview |
| Assessment objects | Supply chain procedure; the container image pipeline; the third party component list |
| Expected result | An approved procedure exists and is applied to component acquisition |
| Actual result | Other than satisfied. Draft existed, unapproved, and the development team was unaware of it |
| Sampling decision | Interviewed the development team as well as examining the document, which is how the awareness gap surfaced |


---

## Section 5: Writing a finding that is useful

A finding that says "AU-6 not satisfied" is true and useless. A useful finding has five
parts, and the two most often missing are the last two.

| Part | Why | Example, SAR-006 |
|---|---|---|
| The control and its objective | Anchors the finding to a requirement rather than an opinion | AU-6, audit records are reviewed at the defined frequency and the review is evidenced |
| What was tested and how | Lets the reader judge whether the conclusion is supported | 12 consecutive weeks of weekly correlated review records examined, plus interview with the Security Operations Manager |
| What was found | The fact | Evidence existed for 6 of the 12 weeks |
| **Root cause** | Without it the fix addresses the symptom | The review was performed from a live dashboard and escalated only anomalies, so a week with no anomaly produced no artifact |
| **Risk, in business terms** | Lets the AO make a decision rather than read a status | If this system were compromised, the record needed to establish what a privileged account did would not exist for half the period |

### Why root cause is the part that earns the salary

Without root cause, the remediation for SAR-006 is "perform the weekly review and keep
records", which is a reminder. With root cause, the remediation is "generate the review
artifact automatically whether or not anything is found", which removes the failure mode.

The same root cause also explains SAR-013, the missing separation checklists: a human
performed step with no system forcing an artifact. Two findings, one cause, one fix. Nobody
finds that by writing up findings in isolation.

---

**Navigation:** [<- CM-04: Control Monitoring in RSA Archer](../cm-04-archer-control-monitoring/README.md) &nbsp;|&nbsp; [Repository home](../README.md) &nbsp;|&nbsp; [CM-06: Metrics and Reporting ->](../cm-06-metrics-reporting/README.md)

---

*Author: Nkeiru Sarah Adesida, Information System Security Officer (ISSO), portfolio scenario*  
*Simulated system: Keystone Grants Management System (KGMS) | FWDA-KGMS-2026-MOD*  
*This document is a portfolio exercise built on a fictional organisation. It is not a government record.*  
*[GitHub portfolio](https://github.com/Nkee07) | [LinkedIn](https://www.linkedin.com/in/nkeiru-adesida-grc)*
