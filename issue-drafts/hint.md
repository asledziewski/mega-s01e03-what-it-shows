# booking-v3 date hint conflicts with the tenant date reader — 33 reports

Source: support ticket #4471, KB export dated 2026-10-04. Count: **33 distinct report IDs**, across 3 tenants, received 2026-04-17 through 2026-09-22.

For day-first tenants using booking-v3 forms, the form displays MM/DD/YYYY but reads the input in the tenant’s day-first order. A user following the hint can enter 04/07/2026 for 7 April and get 4 July. Expected: the hint and the actual reader agree for the tenant. A parser fix alone does not fix this form contract. Affected tenants: alder.physio, copper.studio, pale.harbour.

## Evidence and counting

Counts execute the supplied causes.ts against the tenant directory and deduplicate by report ID. The raw export contains 427 rows but 412 distinct reports; 15 overlapping rows are not extra reports. Intake tags in this group: {"parser":15,"hint":18}. These labels do not determine the cause.

These reports must have their own ticket, rather than being treated as duplicates of the parser issue solely because their subjects match.

## Verification

- Reproduce booking-v3 with the listed day-first tenants and input following its displayed hint.
- Align the hint with the reader and verify the saved date and invite match the displayed date.
- Verify booking-v2 remains correct independently of the shared parser fix.

## Report IDs

`R-41230`, `R-41241`, `R-41251`, `R-41259`, `R-41262`, `R-41275`, `R-41294`, `R-41305`, `R-41308`, `R-41366`, `R-41372`, `R-41384`, `R-41387`, `R-41396`, `R-41400`, `R-41406`, `R-41415`, `R-41416`, `R-41418`, `R-41428`, `R-41439`, `R-41441`, `R-41449`, `R-41453`, `R-41456`, `R-41460`, `R-41472`, `R-41485`, `R-41493`, `R-41539`, `R-41572`, `R-41582`, `R-41584`
