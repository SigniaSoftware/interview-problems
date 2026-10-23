# Personas

## Stop-loss analyst

Works for the insurer and watches every group.

- Uploads each month's claim files, and wants to know straight away which rows failed and why.
- Wants a list of open alerts, newest first, and a view of each group's members ranked by how close they are to their
  deductible.
- Acknowledges an alert with a note (for example, "case manager assigned").
- Needs to trust the numbers: when a member's spend changes, they want to see which claims and versions make it up.

## Broker

Works for a brokerage and looks after a few employer groups on the insurer's portal.

- Sees spend, excess and alerts for **their own groups only**. They must never see another brokerage's groups.
- Read-only: they can't upload files or acknowledge alerts.

## Suggested seed logins

| Role | Email | Groups |
|---|---|---|
| Analyst | analyst@example.test | all |
| Broker | broker.a@example.test | Group A |
| Broker | broker.b@example.test | Group B |
