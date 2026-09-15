# CM-01: Continuous Monitoring Strategy
### Keystone Grants Management System (KGMS) | What is monitored, how often, and the reasoning behind each frequency

> ### PORTFOLIO EXERCISE, SIMULATED SYSTEM
> **Keystone Grants Management System (KGMS) does not exist.** The Federal Workforce Development Agency (FWDA) is a fictional federal
> agency invented for this portfolio. Every organisation, person, system
> identifier, network address, finding, date and signature on this page is
> fabricated for training purposes. Nothing here is drawn from any real
> employer, client or government system, and no real document, artifact or
> data has been reproduced. This file is coursework, not a government record.

---

**Document:** Information Security Continuous Monitoring Strategy v1.0  
**Simulated system:** Keystone Grants Management System (KGMS) | `FWDA-KGMS-2026-MOD`  
**Framework and standards:** NIST SP 800-137, NIST SP 800-53 Rev 5 CA-7  
**Author:** Nkeiru Sarah Adesida  
**Document date:** 29 May 2026  
**Status:** Complete

**Navigation:** [Repository home](../README.md) &nbsp;|&nbsp; [CM-02: Vulnerability Management Programme ->](../cm-02-vulnerability-management/README.md)

---

## Purpose

Continuous monitoring is the part of the framework that decides whether an authorisation
means anything six months after it was signed.

Plain language version: passing the safety inspection once proves the building was safe
on one day. Continuous monitoring is the system for knowing it is still safe today, and
for telling the truth about it on a schedule rather than when somebody asks.

NIST SP 800-137 is the federal guidance for this. Its central idea is that monitoring
frequency should be derived from how fast things change and how much you care, not
chosen because a number sounded reasonable.

---

## Section 1: Strategy

| Element | Decision for this system |
|---|---|
| Objective | Maintain awareness of security posture, vulnerabilities and threats sufficient for the Authorizing Official to keep making an informed risk decision |
| Scope | Every component inside the authorisation boundary, plus the agency processes that operate the controls |
| Primary reference | NIST SP 800-137 |
| Controls implemented | CA-7 Continuous Monitoring, RA-5 Vulnerability Monitoring and Scanning, SI-2 Flaw Remediation, SI-4 System Monitoring, CM-3 Configuration Change Control, AU-6 Audit Record Review |
| Reporting | Monthly to the AO by the 15th of the following month, and immediately on escalation triggers |
| Tooling | Tenable Nessus for detection, ServiceNow for workflow, RSA Archer for control state and evidence |
| Strategy owner | Nkeiru Sarah Adesida, ISSO |
| Approved by | Daniel K. Osei, CISO, 29 May 2026 |

## Section 2: The three tier model

SP 800-137 describes monitoring at three organisational tiers. Confusing them produces
reports that nobody can act on, because the AO is sent a list of unpatched packages
while the engineer is sent a governance summary.

| Tier | Level | What is monitored here | Who consumes it |
|---|---|---|---|
| Tier 1 | Organisation | Agency wide policy currency, risk tolerance, threat landscape changes affecting grant systems | Agency CISO, Audit Committee |
| Tier 2 | Mission and business process | Grants programme risk: award cycle availability, integrity of the disbursement path, privacy obligations | System Owner, Senior Agency Official for Privacy |
| Tier 3 | Information system | This system: vulnerabilities, configuration drift, account state, audit record completeness, POA&M movement | ISSO, Cloud Operations, Security Operations, and the AO in summary |

Most of this repository is Tier 3, which is where a GRC analyst works. The discipline
worth naming is **translating upward**: the AO does not need CVE identifiers, they need
to know whether the conditions attached to their authorisation are being met. CM-06
covers that translation.

---

## Section 3: How each frequency was chosen

This is the section usually reduced to a table of numbers with no reasoning. Each
frequency below states why it is what it is, because a frequency that cannot be justified
will be changed by the first person who finds it inconvenient.

| Activity | Frequency | Method | Owner | Deliverable | Why this frequency |
|---|---|---|---|---|---|
| Authenticated infrastructure vulnerability scan | Weekly, Monday 02:00 | Tenable Nessus, credentialed | Samuel P. Hargrave | Scan report, findings to ServiceNow | Weekly is the shortest cadence that keeps mean time to detection under the 30 day service level with margin |
| Authenticated web application scan | Monthly, second Tuesday | Nessus web application scanning | Priya N. Raghunathan | Scan report | Monthly because the application release cadence is monthly; scanning more often than you change is measurement without information |
| Container image scan | Every build | Pipeline integrated, blocking on Critical | Priya N. Raghunathan | Build gate result | Per build, because a vulnerable image that reaches a registry will be deployed |
| Configuration baseline compliance | Continuous | Platform compliance rules | Samuel P. Hargrave | Drift exception ticket | Continuous, because the assessment showed drift was only ever found at assessment |
| Privileged account activity review | Weekly | Correlated query across three planes | Derrick A. Whitmore | Weekly review record | Weekly, because the pattern being looked for spans days and a daily view cannot see it |
| Alert queue triage | Daily | Detection rules on 7 log groups | Derrick A. Whitmore | Ticket record | Daily, matched to security operations centre staffing |
| Access review | Quarterly | Export plus line by line supervisor attestation | Nkeiru Sarah Adesida | Signed attestation in RSA Archer | Quarterly, because monthly attestation produces rubber stamping and annual leaves stale access for too long |
| POA&M status review | Monthly | Evidence validation against every open item | Nkeiru Sarah Adesida | POA&M status update | Monthly, aligned to the AO reporting requirement |
| Control test, rolling subset | Monthly, 1/12 of controls | SP 800-53A procedures | Nkeiru Sarah Adesida | Test results in RSA Archer | Monthly rolling, so every control is tested annually without an annual crunch |
| Significant change review | Per change | Security impact analysis before deployment | Nkeiru Sarah Adesida | Impact analysis record | Per change, because after the fact review cannot prevent anything |
| Payment reconciliation verification | Monthly | Confirm the daily control ran every business day | Nkeiru Sarah Adesida | Verification record | Monthly. This control carries the integrity categorisation, so it is verified separately rather than assumed |
| Interconnection agreement review | Annual | Confirm agreements current | Marcus T. Delacroix | Review memorandum | Annual, matched to the agreement review cycle |
| Contingency plan functional test | Annual | Restore the database tier into an isolated subnet | Marcus T. Delacroix | After action report with measured recovery time | Annual, and functional rather than tabletop because a tabletop cannot measure a recovery time |
| Incident response exercise | Annual | Tabletop plus one functional element | Derrick A. Whitmore | Exercise report | Annual |
| Security literacy and awareness training | Annual per user, tracked monthly | Agency training platform | Yvonne C. Castellanos | Completion report | Annual per user with monthly tracking, because tracking annually means finding lapses annually |
| Full control reassessment | Annual | One third of controls plus every previously failed control | Rebecca J. Tran | Assessment report | Annual, with previously failed controls always in scope because a closed finding is the most likely one to recur |

## Section 4: Frequency by change rate and impact

The general rule applied above: **monitoring frequency should track the rate at which the
thing being monitored can change, bounded by how much the change would matter.**

| Monitored item | How fast can it change | Impact if missed | Resulting frequency |
|---|---|---|---|
| Published vulnerabilities in installed software | Daily, outside agency control | High. Exploitation of a published vulnerability is the most frequently realised risk | Weekly scanning, with a 30 day remediation service level |
| Configuration state | Any time a change is deployed or a manual action is taken | Moderate to High. Drift disabled audit logging on one host at assessment | Continuous, automated |
| Application code | Monthly release cadence | High. The application is the shortest path to the data | Per build in the pipeline, plus monthly scanning |
| User access rights | On joiners, movers and leavers, so continuously in small increments | Moderate. Stale access accumulates rather than spiking | Automated deprovisioning on separation, plus quarterly attestation |
| Audit record completeness | Only on configuration change | High if missed, because it is invisible until needed | Monthly control test, plus continuous retention policy enforcement |
| Recovery capability | Rarely, but silently | High. An untested backup is a belief | Annual functional test |
| The payment reconciliation control | Could be changed by the finance function for non security reasons | Very high. The system's integrity categorisation depends on it | Monthly verification, plus a change control gate requiring AO approval |

### The one that matters most and is easiest to miss

The last row. The system's impact categorisation was reduced from High to Moderate
integrity on the strength of a daily reconciliation performed by the finance function.
That control is operated by people who do not report to security and who could reasonably
change its cadence for efficiency reasons. Monitoring it monthly, and gating changes to
it, is the only thing standing between the agency and an invalid categorisation it does
not know is invalid.

---

## Section 5: Escalation

| Trigger | Action | Timeframe | Who decides |
|---|---|---|---|
| Critical or High severity vulnerability on an internet reachable component | Notify the CISO and ISSO, assess for emergency change | Same business day | Samuel P. Hargrave |
| Suspected incident | Report to the security operations centre, follow the incident response plan | Within one hour | Derrick A. Whitmore |
| POA&M target date at risk | Deviation request to the AO with justification and a revised date, before the original date passes | Nkeiru Sarah Adesida | Patricia L. Ambrose |
| Significant change deployed without prior impact analysis | Report to the CISO, perform retrospective analysis, record a control deficiency | One business day from discovery | Nkeiru Sarah Adesida |
| Payment reconciliation fails for more than one business day | Notify the AO directly. The basis of the integrity categorisation is affected | Same business day | Patricia L. Ambrose |
| Scan coverage gap discovered | Raise a POA&M item, correct scope, and rescan before reporting the period | Before the monthly report | Nkeiru Sarah Adesida |

### Why deviation requests go in before the date, not after

A POA&M date that passes unmet and is then explained is a missed commitment. The same
date, flagged two weeks early with a reason and a revised date, is risk management. The
work is identical; the difference is entirely whether the AO learns about it before or
after their authorisation condition was breached. This is the single most useful habit in
the job and it costs nothing.

---

## Machine readable version

The monitoring calendar is published as
[artifacts/kgms-conmon-calendar.csv](../artifacts/kgms-conmon-calendar.csv).

---

**Navigation:** [Repository home](../README.md) &nbsp;|&nbsp; [CM-02: Vulnerability Management Programme ->](../cm-02-vulnerability-management/README.md)

---

*Author: Nkeiru Sarah Adesida, Information System Security Officer (ISSO), portfolio scenario*  
*Simulated system: Keystone Grants Management System (KGMS) | FWDA-KGMS-2026-MOD*  
*This document is a portfolio exercise built on a fictional organisation. It is not a government record.*  
*[GitHub portfolio](https://github.com/Nkee07) | [LinkedIn](https://www.linkedin.com/in/nkeiru-adesida-grc)*
