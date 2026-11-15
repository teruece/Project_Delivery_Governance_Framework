# Project delivery update

This example shows how I would brief the business owner, delivery team and support lead after the stock movement pilot retest. It uses simulated evidence.

| Area | Update |
| --- | --- |
| Overall | **Amber · Release decision pending.** The reconciliation retest passed. Business acceptance and the operations rehearsal remain on the release path. |
| Pilot scope | One site, one manual source extract and a shared movement model. |
| Evidence | The initial closing stock figure was 1,325 against 1,320 in the source. Applying the signed adjustment brought the sandbox report to 1,320. The case and its retest are recorded under `DEF-01`. |
| Decisions needed | Ask the inventory business owner to review the retest evidence. Ask the platform owner to confirm the site-access results and the operations owner to confirm recovery readiness. |
| Next scope decision | Keep automated extraction and additional sites in `CR-01` until the source owner and delivery team have assessed the impact and effort. |

## Escalation rule

I would move this to red and request a no-go decision if reconciliation or site access failed again, or if nobody could take ownership of a failed refresh. I would record which requirement failed, the owner, when the next evidence will be available, and who decides whether the release can resume.

See the [RAID and change records](raid-and-change-control.md), [UAT evidence](uat-and-acceptance.md) and [release and handover checklist](release-and-handover.md).
