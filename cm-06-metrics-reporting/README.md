# CM-06: Metrics and Reporting
### Keystone Grants Management System (KGMS) | What to measure, what not to, and the monthly report to the AO

> ### PORTFOLIO EXERCISE, SIMULATED SYSTEM
> **Keystone Grants Management System (KGMS) does not exist.** The Federal Workforce Development Agency (FWDA) is a fictional federal
> agency invented for this portfolio. Every organisation, person, system
> identifier, network address, finding, date and signature on this page is
> fabricated for training purposes, with one exception: the author, Nkeiru Sarah Adesida, is a real person. Where her name appears in a scenario role, that role is fictional, and she has never worked for this agency, which does not exist. Nothing here is drawn from any real
> employer, client or government system, and no real document, artifact or
> data has been reproduced. This file is coursework, not a government record.

---

**Document:** Security Metrics and Reporting Model  
**Simulated system:** Keystone Grants Management System (KGMS) | `FWDA-KGMS-2026-MOD`  
**Framework and standards:** NIST SP 800-55, NIST SP 800-137, FedRAMP continuous monitoring reporting  
**Author:** Nkeiru Sarah Adesida  
**Document date:** 31 August 2026  
**Status:** Complete

**Navigation:** [<- CM-05: Security Control Assessment Procedures](../cm-05-security-control-assessment/README.md) &nbsp;|&nbsp; [Repository home](../README.md)

---

## Purpose

Metrics are how a monitoring programme talks to the people who make decisions. Badly chosen
metrics are worse than none, because they produce confident action on the wrong thing.

Plain language version: you can count almost anything. The question is which numbers
actually help somebody decide. "We closed 40 tickets" is a number. "The one thing you told
us to fix by 31 July was fixed on 24 July" is a decision.

---

## Section 1: What makes a metric useful

NIST SP 800-55 is the federal guidance on security measurement. The practical test used
here has four parts, and a metric has to pass all four:

| Test | Question | A metric that fails it |
|---|---|---|
| Actionable | If this number moves, does somebody do something different? | Total number of vulnerabilities detected. It moves constantly and nobody acts on the total |
| Attributable | Is there a named person who can move it? | Overall security posture score. Nobody owns it, so nobody moves it |
| Comparable | Does it mean the same thing this month as last month? | Open findings count, if deduplication is inconsistent. The number changes because counting changed |
| Honest | Can it be improved without improving security? | Percentage of findings closed. Closing easy Low findings raises it while a High sits open |

### The metric this programme deliberately does not report

**Total vulnerability count.** It rises when scan coverage improves, which is a good thing
that looks like a bad thing. When the database tier was moved to authenticated scanning the
count jumped by 14 overnight, because the scanner could finally see. A programme judged on
total count has a direct incentive to scan less thoroughly, so the metric punishes the
correct behaviour.

What is reported instead: **coverage** (is everything being scanned) and **service level
adherence** (is what we find being fixed on time). Those two cannot be improved by looking
less hard.

---

## Section 2: The metric set

| Metric | Definition | Target | August 2026 | Why it earns its place |
|---|---|---|---|---|
| Scan coverage | Components scanned as a percentage of components in inventory | 100 percent | 96 percent, 2 database instances outside scope, POA-011 | Every other vulnerability metric is meaningless below 100 percent here |
| Inventory accuracy | Components in inventory as a percentage of components discovered | 100 percent | 100 percent since 22 July 2026 | Coverage depends on it, so it is measured separately rather than assumed |
| Service level adherence | Findings remediated within the service level, as a percentage of findings due | 100 percent | 100 percent | The direct measure of whether remediation works |
| Findings past service level | Count of findings open beyond their deadline | 0 | 0 | A count, not a percentage. One overdue Critical matters regardless of the denominator |
| Mean time to remediate, Critical and High | Average days from detection to verified closure | Under 20 days | 11 days | Shows margin against the 30 day deadline, or the absence of it |
| POA&M movement | Opening, closed, new, closing balance | Closing at or below opening | 10 open, 4 closed, 2 new, 8 closing | A single count hides whether the list is shrinking |
| Authorisation conditions met | Conditions the AO attached that are being met | All | All. SAR-001 closed 7 days early | The only metric the AO strictly needs |
| Control test completion | Controls tested this cycle against those scheduled | 100 percent | 100 percent | Untested is not effective, so completion is tracked separately from results |
| Degraded controls | Controls with a known gap and an open item | Trending down | 8 | Connects the control board to the POA&M so they cannot disagree |
| Evidence completeness | Scheduled evidence artifacts actually produced | 100 percent | 92 percent, weekly review records 2 of 4 in July | The metric that catches controls performed without a record |
| Reconciliation control adherence | Business days the daily payment reconciliation ran | 100 percent | 100 percent, 22 of 22 in July | The integrity categorisation depends on this control |
| Reporting timeliness | Monthly report delivered by the 15th | 100 percent | 100 percent since authorisation | A late report is an unmonitored month |

---

## Section 3: Reporting to three audiences

The same underlying data, three different reports. Sending the wrong one is a common and
costly mistake: an AO sent a list of CVE identifiers will stop reading, and an engineer sent
a governance summary cannot act.

| Audience | Cadence | Content | Deliberately excluded |
|---|---|---|---|
| Authorizing Official | Monthly | Whether the conditions attached to the authorisation are being met; POA&M movement with opening and closing balance; anything new that changes the risk picture; any significant change; incidents; one honest statement of what is not going well | CVE identifiers, host names, scanner output, per finding detail |
| System Owner and CISO | Monthly | The AO report plus control state, degraded controls with reasons, metric trends, resource constraints blocking remediation | Individual scan findings below High |
| Component owners and engineers | Weekly | Their own open findings with deadlines, the affected hosts, the specific fix, and the evidence required for closure | Programme level metrics, which are not theirs to move |

### The one paragraph the AO actually reads

The monthly report opens with the conditions. For July 2026 that paragraph was:

> The condition attached to this authorisation required SAR-001, single factor
> authentication for 65 contracted peer reviewers, to be closed by 31 July 2026. It was
> closed on 24 July 2026 and the evidence has been validated. All other POA&M items remain
> within their target dates. Two new items were raised from July scanning, one High, both
> within service level. No incidents. The daily payment reconciliation, on which the
> integrity categorisation depends, ran on all 22 business days.

Everything else in the report supports that paragraph. If the AO reads nothing else, they
have what they need to know whether their decision still holds.

---

## Section 4: Reporting what is going badly

A monthly report where everything is green every month trains the reader to stop reading
it, and then the one month that matters is also unread. Each report carries an explicit
statement of what is not going well.

For July 2026:

> **What is not going well.** Evidence completeness is at 92 percent because the weekly
> correlated privileged activity review produced records for two of four weeks. The
> automation that would make this artifact reliable is not yet in production and is due 31
> August. This is the control we would most need after an incident, and it is currently the
> least reliably evidenced. It is also the second period in which it has been reported as
> incomplete.

Three things that paragraph does that a green dashboard cannot: it names the specific gap,
it states why the gap matters rather than just its status, and it admits this is a repeat
rather than letting the reader assume it is new.

---

## Section 5: Trend, May to August 2026

| Metric | May | June | July | August | Direction |
|---|---|---|---|---|---|
| POA&M items open | 10 | 9 | 8 | 6 | Improving |
| Of which High | 1 | 1 | 0 | 1 | One new High raised in August, POA-012 |
| Findings past service level | 0 | 0 | 0 | 0 | Held |
| Scan coverage, percent | 100 | 100 | 96 | 96 | Regressed in July when two instances were provisioned outside scope |
| Evidence completeness, percent | 85 | 88 | 92 | 96 | Improving |
| Degraded controls | 10 | 10 | 9 | 8 | Improving |
| Mean time to remediate High, days | n/a | 14 | 9 | 11 | Within the 20 day target |
| Reporting delivered on time | Yes | Yes | Yes | Yes | Held |

### Reading the two honest lines in that table

**Scan coverage regressed from 100 to 96 percent in July** and has not recovered in August,
because two database instances were provisioned outside the scan target group and the fix,
binding scan scope to inventory, is not yet complete. Reporting it as 96 rather than
rounding up is the point: a coverage gap is the one metric that makes every other
vulnerability number unreliable.

**Open High findings went from 0 in July back to 1 in August.** POA-012, the Log4j
transitive dependency. A report that showed only the improving POA&M total would have hidden
that a new High appeared, which is the single fact in this table most likely to change what
the AO does.

---

## Section 6: Annual cycle

| Activity | Timing | Output |
|---|---|---|
| Monthly continuous monitoring report | By the 15th of the following month | Report to the AO |
| Quarterly access review | March, June, September, December | Signed attestations |
| Quarterly management review | Following each quarter close | Decisions record |
| Annual risk assessment refresh | December | Updated risk register |
| Annual contingency plan functional test | September | After action report with measured recovery time |
| Annual incident response exercise | October | Exercise report |
| Annual control assessment | February and March | Assessment report, one third of controls plus every previously failed control |
| Annual interconnection agreement review | August | Review memorandum |
| Authorisation renewal | By 14 May 2029 | Reauthorisation package |

---

## Where this sits in the portfolio

| Repository | What it covers |
|---|---|
| [keystone-kgms-fedramp-rmf](https://github.com/Nkee07/keystone-kgms-fedramp-rmf) | The authorisation this programme maintains |
| [keystone-kgms-iso27001](https://github.com/Nkee07/keystone-kgms-iso27001) | The same system under ISO/IEC 27001:2022, where this activity satisfies Clause 9.1 |

---

**Navigation:** [<- CM-05: Security Control Assessment Procedures](../cm-05-security-control-assessment/README.md) &nbsp;|&nbsp; [Repository home](../README.md)

---

*Author: Nkeiru Sarah Adesida, Information System Security Officer (ISSO), portfolio scenario*  
*Simulated system: Keystone Grants Management System (KGMS) | FWDA-KGMS-2026-MOD*  
*This document is a portfolio exercise built on a fictional organisation. It is not a government record.*  
*[GitHub portfolio](https://github.com/Nkee07) | [LinkedIn](https://www.linkedin.com/in/nkeiru-adesida-grc)*
