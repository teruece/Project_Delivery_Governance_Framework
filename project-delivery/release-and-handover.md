# Release and support handover

I use this runbook to turn acceptance evidence into a release decision and give the support owner a way to recover the service. The steps below describe a proposed rollout for a fictional SITE-A stock-movement report. The worked example shows a sandbox release hold and retest.

## Release conditions

| Control | Evidence I would bring to the release review | Decision role |
| --- | --- | --- |
| Approved pilot scope | SITE-A only, the agreed manual extract and dated scope acceptance. Automated ingestion and additional sites stay in deferred CR-01. | Inventory business owner confirms scope. |
| Reconciliation and acceptance | Technical QA and business UAT records for the candidate. DEF-01 retest with the signed adjustment and correct closing of 1,320 units. | Inventory business owner accepts results for the agreed scope. |
| Access and freshness | Viewer tests for site access and verified stale behaviour when the extract is unavailable or rejected. | Platform owner confirms access. BI lead confirms freshness behaviour. |
| Support readiness | Named service owner and backup, expected delivery window, monitoring route, known-good artefact and data snapshot, and successful recovery rehearsal. | Operations owner accepts support readiness. |
| Go or no-go | Scope, evidence, open conditions, cutover window and recovery ownership reviewed together. | The inventory business owner acts as release authority for this pilot. The delivery lead coordinates the review and records the decision. |

In the example, the initial sandbox balance was 1,325 rather than 1,320 because the −5 adjustment was omitted. I would hold release, log DEF-01 and rerun the same tests on a version with DEC-01 applied. A successful retest resolves the reconciliation blocker. Access, freshness and recovery evidence still need to clear their gates.

## Cutover and verification

1. Record the candidate report and model versions, accepted extract ID and source as-of timestamp. Capture the last verified report and model artefacts and the matching accepted data snapshot.
2. Confirm the release decision and the support owner’s availability. In the agreed window, stage the compatible model and report without distributing them to pilot users. Configure the intended pilot access rules and validate the current accepted extract.
3. Run smoke checks with a test Viewer identity that has the intended SITE-A assignment. Confirm the report opens, SITE-A filters and totals are correct, the agreed closing reconciles to the source, and the report displays the correct source as-of time and freshness status.
4. Check the report as an unassigned identity. Keep the candidate unavailable to the pilot audience if access or another release condition fails.
5. Publish to the agreed pilot audience when verification passes. Record the deployed version and send the handover update. Monitor the next scheduled source delivery during the staffed support window.

## Support runbook

The manual extract is due by 08:00 UTC on service days. An incoming extract becomes the current snapshot after ingestion and reconciliation pass. If the deadline passes without an accepted current extract, the Viewer sees the last accepted source as-of time and a stale status. The freshness check needs to run even when no file loads.

| Trigger | First action | Escalation and evidence |
| --- | --- | --- |
| Extract missing by 08:00 UTC | Mark freshness stale and keep the last accepted snapshot available. | Source owner confirms delivery or expected recovery. Record the missed window and last accepted source timestamp. |
| Extract fails contract or reconciliation | Reject the candidate file without loading it into the accepted data snapshot. Retain the rejection reason and request a corrected extract. | Data engineering investigates the validation failure. The inventory business owner confirms source balances if needed. |
| Incorrect balance after release | Hold access to affected outputs or restore the known-good version. Compare the accepted extract, shared model and report at the same site and period. | BI lead and data engineering lead confirm impact and repair evidence. Record affected version and scope. |
| Unexpected cross-site access | Contain exposure by restricting report access and notify the platform owner promptly. | Review intended assignments, applicable access rules and evidence. Retest as the affected Viewer roles before restoring access. |
| Report or model cannot refresh | Keep the last verified snapshot visible with its correct freshness state where safe. Compare artefact versions and restore if the candidate caused the failure. | Operations records the version, source as-of time, error and first affected refresh. |

## Rollback and communications

The operations owner initiates the recovery runbook with the platform owner and release authority when a critical smoke check fails. Pause publication and refresh activity for the affected rollout. Restore the compatible report and model version with the last accepted data snapshot and access configuration. Verify the restored source timestamp, balance and Viewer access before reopening the report. If the restored source is stale, leave the stale status visible. Record the rollback reason, time, restored versions and checks. Resume the rollout only after the relevant gate is reviewed again.

For the release notice, I include the pilot audience, report purpose, source as-of time and support route. For a hold or rollback, I include affected users, what they can safely use, current freshness, the incident owner and the next update time. The service desk receives the runbook and release record so support can follow the same decisions.

[Project overview](README.md) · [Acceptance evidence](uat-and-acceptance.md) · [Delivery roles](delivery-plan-and-raci.md) · [Control log](raid-and-change-control.md)
