# UAT and acceptance evidence

I trace each business requirement to a test with an expected result, an evidence record and an owner. This worked example uses a synthetic SITE-A pilot. It shows how a five-unit reconciliation failure would stop a release and what I would ask the business owner to review after the fix.

## Test scope and setup

- Freeze the candidate report and shared data model version for the test run.
- Keep a read-only copy of the agreed manual extract and record its as-of timestamp.
- Run technical QA first. Then ask business testers to use the published sandbox report with their assigned Viewer identities.
- Use SITE-B only in the sandbox access fixture. SITE-A is the sole pilot site.
- Record expected and actual results separately. Keep the source snapshot, validation output and relevant report filters with each evidence record.

## Acceptance matrix

The expected results below are based on the synthetic pilot contract. Use a separate acceptance record for the observed result of each test run.

| Test | Requirement | Setup or user action | Expected result | Evidence owner |
| --- | --- | --- | --- | --- |
| UAT-01 | REQ-01 | Load the valid SITE-A extract, then test two invalid fixtures separately. One repeats a `(site_id, event_id)` pair and the other uses a movement type outside the agreed scope. | The valid file has one row per event key. Each invalid file is rejected for the relevant reason. The accepted snapshot and its timestamp stay in place. | Data engineering lead |
| UAT-02 | REQ-02 | Select SITE-A and the agreed reporting period. Compare the source balances and stock movements with the model and report using the same filters. | Opening 1,240 + receipts 180 − issues 95 + signed adjustment −5 = closing 1,320. The report and source difference is zero units. | Inventory business owner |
| UAT-03 | REQ-03 | Open the sandbox report as a SITE-A Viewer, a SITE-B Viewer and a Viewer with no site assignment. Try the site filter, totals and available drillthrough and export paths. | Each scoped group sees its assigned site only. The unassigned Viewer sees no site data. Totals and any permitted export follow the same access rules. | Platform owner |
| UAT-04 | REQ-04 | View an accepted extract for a service day, then simulate a missed 08:00 UTC publication and an invalid replacement file. Run a successful report refresh without accepting new source data. | The report identifies the accepted source as-of time. After the deadline, without an accepted current extract, the freshness state remains stale even after the report refresh succeeds. A rejected replacement leaves the previous figures and source timestamp in place. | BI lead |
| UAT-05 | REQ-05 | Rehearse a failed post-deployment source check. Restore the known-good reporting version and last accepted data snapshot, then run the smoke checks. | The report loads under the intended identity, displays the expected balance, shows the restored source timestamp and applies the correct freshness state. Operations can follow the [handover runbook](release-and-handover.md). | Operations owner |

## Worked QA evidence record

The table illustrates the defect and retest record a team would keep. The runs and results are synthetic.

| Run | Candidate and fixture | Expected source closing | Illustrative report closing | Disposition |
| --- | --- | ---: | ---: | --- |
| QA-02-A | Initial model, SITE-A reconciliation fixture | 1,320 | 1,325 | Fail. Hold the candidate and raise DEF-01. |
| QA-02-B | Corrected model, same fixture and report filters | 1,320 | 1,320 | Pass for reconciliation. Confirm the remaining release evidence before requesting approval. |

The initial candidate omitted the −5 adjustment. Under DEC-01 the model preserves the sign supplied in the agreed extract. The retest checks both the closing balance and the adjustment line so a compensating error cannot produce a false pass on the total.

## Acceptance record

| Field to capture | What I would record |
| --- | --- |
| Test run | Run reference, environment, candidate version, fixture or accepted extract ID and test date |
| Verification | Expected and actual results, test identity, filter selection, evidence location and disposition |
| Defects and conditions | Linked defect ID, impact, owner, workaround if supported, and retest outcome |
| Business acceptance | Inventory business owner, approved report scope, outstanding conditions and approval timestamp |
| Release authority | Inventory business owner as release authority, go or no-go decision, and evidence checked across QA, UAT, access and recovery |

I would hold deployment for an unresolved source mismatch, an access leak, a misleading freshness indicator or a failed recovery rehearsal. I would raise any proposed exception with the accountable owner and capture its rationale and expiry in the [control log](raid-and-change-control.md). The approval fields above are examples for a future project.

[Project overview](README.md) · [Requirements and scope](requirements-and-scope.md) · [Delivery roles and gates](delivery-plan-and-raci.md)
