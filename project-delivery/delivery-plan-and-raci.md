# Delivery plan and RACI

I planned the fictional SITE-A stock movement pilot so a business owner could see what they were approving at each stage. The sequence, evidence and ownership below are examples for a delivery rehearsal.

## Stage plan

| Stage | Work and dependencies | Exit evidence | Decision owner |
| --- | --- | --- | --- |
| 1. Define the pilot | Agree the business question, SITE-A scope, reporting grain, manual extract and known exclusions. Confirm who owns the extract and KPI. | Business brief, requirements baseline, initial RAID log and reviewer roles. | Inventory business owner |
| 2. Design and build | Confirm the source contract and agreed cadence. Design the common movement model, stock calculations, site access, freshness indicator and operating workflow. Build against the agreed extract shape. | Versioned design and report, source contract, KPI definitions, and support draft. | BI lead |
| 3. Complete technical QA | Test event grain, supported values, signed adjustments, closing balances, access isolation and freshness. Record expected and actual results against the requirement IDs. | Technical QA evidence, defect register, retest evidence and version identifiers. DEF-01 must be retested after the adjustment fix. | BI lead |
| 4. Run business UAT | Use representative viewer accounts and the sample extract. Ask the inventory analyst to reconcile the report and confirm that the results support the operational decision. | Written acceptance against the traceable requirements and a record of exceptions or conditions. | Inventory business owner |
| 5. Rehearse release and handover | Confirm the deployment and refresh order, prior good versions, who can pause the release, and who will support the feed. Rehearse recovery and publish the handover pack for review. | Release checklist, recovery test, handover record and go/no-go recommendation. | Operations owner for readiness. Inventory business owner for the release decision. |

## Example responsibility matrix

**R** carries out the activity, **A** owns the approval, **C** advises before a decision, and **I** receives the outcome. The delivery lead keeps the records current and escalates issues through the agreed governance route.

| Activity | Delivery lead | Inventory business owner | Inventory analyst | Source owner | Data engineering lead | BI lead | QA reviewer | Platform owner | Operations owner |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Confirm scope, KPI and acceptance criteria | R | A | C | C | C | C | C | I | C |
| Agree the manual extract contract and validate ingestion (REQ-01) | C | I | I | R | A/R | C | C | I | C |
| Reconcile metrics against the source (REQ-02) | C | A | R | C | C | R | R | I | I |
| Define site entitlements and prove access isolation (REQ-03) | I | C | C | I | I | R | R | A | C |
| Build and verify data as-of and staleness indicators (REQ-04) | C | C | C | C | R | A/R | R | I | C |
| Run business UAT and decide acceptance | R | A | R | C | C | C | C | C | C |
| Rehearse recovery and accept operational handover (REQ-05) | C | I | I | C | R | R | R | C | A |
| Convene go/no-go and record the decision | R | A | I | I | C | C | C | C | C |

## Delivery cadence and exception handling

- At the delivery review, check the next gate, changes to dependencies, unresolved defects and owners for each action. Keep status tied to the evidence still needed.
- During technical QA and UAT, maintain a short defect review. In this example the prototype closes at 1,325 units because it omits the −5 adjustment. DEF-01 stays open until a retest proves the adjusted 1,320-unit result and confirms the other movement components still reconcile.
- If the agreed extract is late or fails validation, record the issue, hold the refreshed output and ask the source owner for a corrected delivery. After the 08:00 UTC service-day cutoff, the existing report shows the last accepted source timestamp with a stale indicator.
- When automated integration or additional sites are requested, assess testing effort, source readiness, access and support. CR-01 keeps those changes out of the pilot baseline pending a separate estimate and approval.
- At go/no-go, link the accepted dataset, tested report version, UAT evidence and recovery result. If a blocking condition is open, record the owner and what must be verified before reconsidering release.

[Business brief and requirements](requirements-and-scope.md) · [RAID and change control](raid-and-change-control.md) · [Release and handover](release-and-handover.md)
