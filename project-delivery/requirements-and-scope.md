# Business brief and requirements

I used this fictional stock movement reporting pilot to show how I turn an operational problem into a delivery scope with testable requirements. The figures and site codes are synthetic.

## Business brief

The inventory operations team needs to explain the difference between opening and closing stock for a reporting period. Staff currently compare a manual stock extract with a report and spend time tracing differences back to receipts, issues or adjustments. My pilot outcome is a report an analyst can reconcile to the agreed extract, with a visible data as-of time and site-level access.

| Item | Pilot agreement |
| --- | --- |
| Users | Inventory analysts review the figures. The inventory business owner approves the KPI and acceptance evidence. |
| Data | One agreed manual extract for SITE-A, supplied by the source owner, with a site-level opening snapshot, movement rows and a reference closing balance for the reporting period. |
| Data model | One movement row per `(site_id, event_id)`. Opening and reference closing balances are held separately at site and reporting-period grain. Reuse the same definitions for subsequent extracts. |
| Reporting | Display opening stock, receipts, issues, signed adjustments, calculated closing stock, reference closing stock and reconciliation difference. Show the as-of time and freshness state. |
| Access | Site-based report access. Use a synthetic SITE-B in the test environment to prove the access rule. The proposed live pilot covers SITE-A. |
| Boundaries | Direct transfers are out of scope. Automated source integration and additional sites are deferred through CR-01. |

## Definitions and controls

- Receipts and issues are non-negative quantities. Closing stock equals opening stock plus receipts, minus issues, plus the sum of signed adjustments.
- A negative adjustment reduces stock. DEC-01 records the decision to preserve the source sign through ingestion, the common model and the report.
- The source owner confirms how timestamps, units and reporting-period boundaries are represented. The data engineering lead validates the extract against that contract, including required fields, supported movement types and event-key uniqueness. A rejected batch is recorded for correction before refresh.
- The analyst compares the closing balance with the reference closing amount from the same extract, site and period. With an opening balance of 1,240, receipts of 180, issues of 95 and an adjustment of −5, the expected closing balance is 1,320 units.
- The report displays the source data as-of time separately from the report refresh time. For the rehearsal, the manual extract is due by 08:00 UTC on service days. When that cutoff passes without an accepted extract, the report shows the previous accepted source timestamp and a stale indicator. A report refresh does not reset source freshness.

## Requirements register

| ID | Requirement | Accountable owner | Acceptance evidence |
| --- | --- | --- | --- |
| REQ-01 | Load the agreed extract to the common event grain after validating its required fields, types and keys. | Data engineering lead | A valid fixture produces one row per event key. A duplicate event key or unsupported movement type triggers validation failure and retains the previous accepted dataset. |
| REQ-02 | Reconcile opening, movements and closing to the agreed extract at the same site and period. | Inventory business owner | Record expected and actual component totals. Confirm the signed −5 adjustment and zero closing difference against 1,320 units after the DEF-01 retest. |
| REQ-03 | Enforce site-based access in the report and underlying semantic layer. | Platform owner | Show allowed SITE-A detail and totals for an authorised viewer. Check that a viewer without access cannot retrieve SITE-A rows or totals. Use synthetic SITE-B to verify site isolation in the test environment. |
| REQ-04 | Display a clear data as-of time and stale-source state. | BI lead | Compare the displayed as-of time with the accepted extract. Pass the 08:00 UTC cutoff without an accepted extract and confirm the report flags it even after a successful report refresh. |
| REQ-05 | Hand over refresh, monitoring, escalation and recovery to operations. | Operations owner | Rehearse source failure and recovery using a known-good extract and report version. Record owner, recovery steps and post-restore reconciliation. |

I keep the acceptance evidence linked to the requirement ID and the version tested. A new site, source or transfer rule can affect grain, reconciliation, security and support, so it goes through [change control](raid-and-change-control.md) before I update the baseline.

[Delivery plan and RACI](delivery-plan-and-raci.md) · [UAT and acceptance](uat-and-acceptance.md) · [Case-study overview](README.md)
