# Project delivery case study

I use this case to show how I take a BI request from an agreed business outcome through requirements, delivery, acceptance and handover. The documents use a fictional stock movement report. The sites, figures, test evidence and release decisions are illustrative.

## Business outcome

The inventory team needs a daily view of opening stock, movement and closing stock so it can investigate differences against its source extract. I scoped the first release to one pilot site and one agreed manual input. I kept automated extraction and wider rollout in a separate change request so the team could agree the data contract and prove the report first.

The case uses a common movement model at one row per site and event. It distinguishes source freshness from the time the report refreshed, and uses site-level access rules. A synthetic second site appears in the test workspace to exercise the access rule.

## A decision I would defend in review

The first report returned 1,325 closing units against an expected 1,320. The difference came from a missing negative inventory adjustment. I recorded the sign convention, treated the mismatch as a release blocker and linked the fix to the business acceptance test. The simulated retest reconciles to zero difference. Production would still require the release owner to review the evidence and authorise the change.

| Movement | Pilot example |
| --- | ---: |
| Opening stock | 1,240 |
| Receipts | +180 |
| Issues | −95 |
| Inventory adjustment | −5 |
| Expected closing stock | **1,320** |
| Initial report | 1,325 |
| Corrected report in the simulated retest | **1,320** |

## Delivery documents

| Document | What I show |
| --- | --- |
| [Requirements and scope](requirements-and-scope.md) | Business outcome, pilot boundaries, technical contract and who owns each requirement. |
| [Delivery plan and RACI](delivery-plan-and-raci.md) | Work sequencing, dependencies, handoffs and responsibility for each gate. |
| [RAID, decisions and change control](raid-and-change-control.md) | How I log risk, separate a defect from new scope and trace both to evidence. |
| [UAT and acceptance](uat-and-acceptance.md) | Expected results, access tests, exact reconciliation, defect retest and sign-off. |
| [Release and handover](release-and-handover.md) | Go/no-go criteria, refresh checks, recovery steps and ownership after launch. |
| [Stakeholder update](stakeholder-update.md) | A short example of how I communicate the release position and decisions needed. |
