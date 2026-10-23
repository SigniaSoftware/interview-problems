# Stop-loss rules

## Background

Some employers pay their staff's medical claims themselves (they are "self-funded"). To protect against one very
expensive member, they buy **specific stop-loss** insurance from an insurer:

- Each policy year has a **specific deductible**, for example $50,000 per member.
- Once one member's claims in a policy year go past that amount, the insurer pays the part above it (the **excess**).

The insurer wants to see trouble coming before it lands. A claims administrator (a "TPA") sends a claims file for each
group every month. Your app loads these files, tracks each member's spend, and raises alerts as members get close to
their deductible.

## Reference files (`data/`)

- `policies.csv`: each group's policy years and the specific deductible for each year. A group can have more than one
  policy year, and the deductible can change between years.
- `lasers.csv`: members who have their own, higher deductible for one policy year (a "laser"). It replaces the
  group's deductible for that member in that year.
- `members.csv`: every member, and the group they belong to.

Loading these at startup as seed data is fine. They don't need an upload screen. They also list groups whose claim
files you don't have yet. That's expected.

## Claim files (`data/claims/`)

- Each file holds one group's claims. The file name starts with the group ID (`GA_…` is Group A).
- Each TPA uses its own layout. Group A's TPA and Group B's TPA send different columns and formats, so look at both.
- We upload files in file-name order. A corrected or re-sent file can arrive later than the files around it.

### Claim versions

A claim can show up in more than one file, because TPAs adjust and reverse claims after they first pay them.

1. A claim is identified by its claim ID **within its group**.
2. Every row is one version of a claim. The version number says which is newest (higher is newer).
3. **The newest version replaces all earlier ones.** Don't add versions together.
4. A reversed or voided version means that claim now pays $0, whatever amount the row shows.
5. If a version arrives that is older than one already loaded, it changes nothing.
6. The same version loaded again with the same values (for example, a re-sent file) changes nothing. The same version
   with **different** values is an error, so report it.

### Bad rows

A row that can't be used is reported with its file, its row number and the reason, and is otherwise skipped. Examples:
an unknown member, a member from another group, or an amount or date that can't be read. Summary rows (such as
totals) are not claims.

## Spend

- A claim counts toward the policy year that contains its **service date** (when the care happened), not the date it
  was paid.
- A claim whose service date falls in none of the group's policy years doesn't count toward any spend.
- A member's spend for a policy year = the total of the current versions of their claims in that year.
- Excess = spend minus deductible, if above zero.

## Alerts

- Raise an alert when a member's spend in a policy year reaches **50%, 75% and 100%** of their deductible.
- Check after each file is loaded. One file can cross several thresholds, which raises one alert for each.
- Each threshold alerts **at most once** per member and policy year. If spend drops later (a reversal, say), the alert
  stays. If spend rises past the threshold again, no new alert.
- Alert lifecycle: `open` → `acknowledged` (analyst, with a note).
