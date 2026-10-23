# Engineering take-home: stop-loss claims monitor

Build a small app for a fictional health insurer that sells stop-loss cover to self-funded employers. Analysts upload
monthly claim files, the app keeps a running total of each member's spend for the policy year, and it raises alerts
as members get close to their deductible. `STOP_LOSS_RULES.md` explains the business in two minutes. No insurance
background needed.

**Use AI.** Claude Code, Cursor, Copilot, agents, whatever you work with. We expect you to, and we want to see how.

**Time box:** the core below should take about 10–12 hours with AI tools.

## What's in this pack

- `STOP_LOSS_RULES.md`: claim versions, spend, policy years and alerts.
- `PERSONAS.md`: who uses the app, and suggested seed logins.
- `data/`: policies, lasers, members, and monthly claim files for two groups from two different claims administrators.

## Core: this is the whole assignment

A submission that does all of this well is a complete, strong submission.

1. **Login with two roles**, analyst and broker. A broker sees only their own groups, and the API enforces that, not
   just the UI.
2. **Claim file upload** for both layouts in `data/claims/`. Show which rows failed and why. Uploading the same file
   twice must not change any totals. We will also load files you haven't seen.
3. **Spend per member per policy year**, following the claim-version rules, with the deductible, % used and excess.
4. **Alerts** at 50/75/100%, an analyst view of open alerts, and acknowledging one with a note.
5. **A relational database** with migrations.
6. **Tests** for the claim-version and alert rules, and at least one access-control test (broker A can't see
   Group B).
7. **`docker compose up`** (or one documented command) runs everything with seed data and the logins.
8. **CI** (GitHub Actions or similar) that would build the app and run your tests.
9. **Infrastructure as code** for one cloud target (Terraform/OpenTofu/CDK) that would deploy it.

> **Important: about items 8 and 9.** We don't expect you to write the CI or infrastructure code by hand, and you
> don't need to run either of them. No pipeline runs, no `plan` or `apply`, no cloud account, no money spent. Let
> your AI tools write them. What we do expect is that you can **reason about them**: if we discuss your submission, we'll ask what
> they do, why you made the choices you did, and what you would change before running them for real.

## Optional extras: none are expected

Only if you finish the core with time left. Pick whichever you think matter most and tell us why in `DECISIONS.md`.
Doing none of these is fine.

- A member drill-down: every claim and version behind their spend.
- Audit history: who uploaded which file, and who acknowledged which alert.
- An email or notification outbox for new alerts (record the messages, don't actually send them).
- A 200,000-row claim file loads in reasonable time (generate your own test file).
- An end-to-end browser test of the main flow.
- Two analysts uploading files for the same group at the same moment can't corrupt the totals.

## Questions and judgment calls

Some rules are less precise than they could be. That's normal for real work, and it's not a trick. For anything
unclear, make a sensible call and write it down in `DECISIONS.md`. There's no single right answer we're looking
for; we care that the call is reasonable and stated.

If you run short of time, stop adding features. A core that works, plus an honest list of what you skipped or
simplified, scores better than more features that half work.

## Submit

A git repo. Keep your real commit history, don't squash. It should contain:

1. `README.md`: how to run it, an architecture sketch, and what works.
2. `DECISIONS.md`: your key design decisions, plus a **shortcut list**. For every corner you cut, say why that is
   acceptable for now, what could go wrong, what would tell you it's time to fix it, and roughly what the fix costs.
   We value an honest list far more than a short one.
3. `AI.md`: how you set up and drove your tools (rules files, agent config, prompts, workflow), three moments where the
   AI got something wrong and how you caught it, what you chose *not* to hand to the AI and why, and roughly how many
   hours you spent.
4. `PRODUCTION.md` (1 page max): what changes to run this for 50 insurers, 5M members and about 100M claim rows a year.
   Tenancy, security of health data, observability, scaling, deployment.
5. Optional: a 5-minute screen recording walking through it.

All data is synthetic. Nothing you build will be used in our product.
