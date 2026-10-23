# Timesheet Check

## Why

The company's pay cycle closes on the 24th of each month. On the first working day after it closes, someone checks by hand whether everyone logged all their hours for that cycle. It takes a morning, and people get chased late.

The last cycle closed on Saturday, 24 October 2026. It's Monday morning.

## What to build

A small web app that answers one question: **did everyone log all their hours in the pay cycle that just closed?**

1. **Mock data.** There is no real timesheet system to connect to. Generate a team of employees with random names, and the hours each of them logged per day from 24 August to 24 October 2026.
2. **Team overview.** One row per employee: name, hours logged, hours expected, and whether they are complete.
3. **Summary.** How many people are complete, and how many still have hours missing.
4. **Employee detail.** Click an employee to see the days they logged and the days they didn't.
5. **Previous cycle.** Let me switch to the earlier cycle in the data too.

## Rules

- A full-time employee works **8 hours per working day**.
- Working days are **Monday to Friday**.
- **Public holidays in Portugal** are not working days. For example, 5 October (Implantação da República).

## Not needed

- Login or user accounts
- A real database
- Editing or approving timesheets
- Exporting to Excel or PDF

## Done when

You can open the app in a browser and see at a glance who still owes hours for the cycle that just closed.
