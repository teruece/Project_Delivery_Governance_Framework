# RAID, decisions and change control

I use the registers to make the next delivery decision visible. In this simulated pilot, the first stock report showed a closing balance of 1,325 units. The agreed extract reconciled to 1,320. I recorded the mismatch as a defect, documented the sign convention, and kept the decision about future scope separate.

## RAID register

In a weekly delivery review, I would ask each owner for the evidence that supports the current status, the next action, and the decision it affects. I would bring forward anything that could block testing or release. The entries below use role names and milestones so the same format can be reused.

| ID | Type | What could affect delivery | Owner | Next action or evidence | Simulated status |
| --- | --- | --- | --- | --- | --- |
| RISK-01 | Risk | The manual extract arrives after 08:00 UTC on a service day. A viewer could use an old balance as current. | BI lead | Use UAT-04 to check the stale-source indicator and the timestamp of the last accepted extract. See REQ-04. | Control planned |
| ASSUMP-01 | Assumption | Each event identifier is unique within its site. | Data engineering lead | Confirm the extract contract with the source owner. Use UAT-01 to profile the composite key `(site_id, event_id)` and reject a file with duplicate keys. See REQ-01. | To verify |
| DEF-01 | Issue | The initial closing balance is 1,325 units, five above the source. The report omits a signed adjustment of −5. | BI lead | Apply DEC-01 and rerun the reconciliation for REQ-02. See QA-02-A, QA-02-B and UAT-02. | Fixed in the simulated retest |
| DEP-01 | Dependency | The platform owner must provide test identities for the access checks. | Platform owner | Run UAT-03 with sandbox fixtures for both sites to demonstrate that the pilot user can see SITE-A only. See REQ-03. | Required before acceptance |
| DEP-02 | Dependency | The operations owner must be able to support and recover the manual intake. | Operations owner | Use UAT-05 to rehearse recovery. Record the last accepted extract, post-restore checks and escalation route. See REQ-05. | Required before release |

## Decision and defect evidence

**DEC-01 · Preserve the adjustment sign.** I documented the inventory rule for review by the inventory business owner. Closing units equal opening units plus receipts minus issues plus signed adjustments. An adjustment can increase or decrease stock. Applying an absolute value would reverse the effect of a negative adjustment. The reporting model keeps adjustment events at the same event grain as the other movements.

**DEF-01 · Reconcile closing units.** In the example, SITE-A opens at 1,240 units, receives 180, issues 95, and has a −5 adjustment. The initial total was `1,240 + 180 − 95 = 1,325`. The corrected total is `1,240 + 180 − 95 + (−5) = 1,320`. The simulated retest reconciles to the agreed extract, a difference of zero. I would retain the source snapshot identifier, report version, filters, and expected and observed balances with the [worked QA evidence](uat-and-acceptance.md#worked-qa-evidence-record). A live release decision would still depend on the remaining acceptance and release checks.

## Change request CR-01

| Field | Example entry |
| --- | --- |
| Request | Automate the source extract and onboard additional sites. |
| Baseline | SITE-A uses one agreed manual extract and the common stock movement model. |
| Affected requirements | REQ-01 for intake and keys, REQ-02 for reconciliation, REQ-03 for access, REQ-04 for freshness, and REQ-05 for support and recovery. |
| Impact to assess | Source interfaces, mapping differences, site-level permissions, revised timeliness checks, recovery arrangements, regression coverage and delivery capacity. |
| Decision route | The data engineering lead and platform owner assess feasibility. The delivery lead brings cost, capacity and dependency impacts to the inventory business owner for a scope decision. |
| Simulated disposition | Deferred pending separate scope and estimates. The pilot continues on the agreed manual intake. |
| Revisit when | The source owner can provide representative extracts and access conditions for the additional sites. |

I kept DEF-01 within the agreed scope because reconciliation was already an acceptance requirement. CR-01 would change the intake method and the population covered by the report. If approved, I would update the scope, owners and acceptance plan before scheduling that work.

## Reuse these fields

| Record | Fields to carry forward |
| --- | --- |
| RAID or defect | ID, type, description, impact, owner, next action, target milestone, status, latest evidence and escalation. |
| Decision | ID, decision owner, options, selected rule, rationale, affected requirements and evidence. |
| Change request | ID, requester role, baseline and requested change, benefits, impacts, decision owner, disposition and revisit trigger. |

[Project delivery case study](README.md) · [Requirements and scope](requirements-and-scope.md) · [Delivery plan and RACI](delivery-plan-and-raci.md) · [UAT and acceptance](uat-and-acceptance.md) · [Release and handover](release-and-handover.md)
