# MD-102 Study Plan

The `week-01.md`–`week-08.md` files are the source of truth for daily content, labs, and self-checks. This page is an index only — don't duplicate schedule details here, it drifts.

| Week | Focus |
|------|-------|
| 1 | D1: Deploy Windows Client (Autopilot, enrollment, WUfB, co-management) |
| 2 | D2: Identity & Compliance (Entra join types, CA, WHfB, SSPR, MDE) |
| 3 | Catchup/review buffer (skip rule: ≥75% on D1+D2 self-checks → jump to Week 4) |
| 4 | D3 Part 1: config profiles, LAPS, remote actions, BitLocker |
| 5 | D3 Part 2 + D4: compliance policies, MDE integration, Win32, MAM, M365 Apps |
| 6 | Full D1–D4 review + full timed practice exam |
| 7 | Final review + retrieval — no exam this week |
| 8 | **MD-102 exam** — logistics, exam day, debrief |

## Daily rhythm

Newton (Hermes) sends a morning nudge with today's focus topic. Midweek check: if < 50% of week objectives touched → drift alert.

## The Numbers Deck (baseline fix)

2026-07-02 baseline: ≤10/50 on the official practice assessment — losses were detail facts, not concepts. Keep a dedicated Anki deck of every hard number and "which policy for X" pair; drill it daily before topic study:

- DEM account: **1,000** devices max; no Autopilot, no Apple ADE, no CA targeting
- WUfB deferral: quality **30d** max, feature **365d** max, pause **35d** max
- Compliance grace period: **0–730 days**; no policy assigned = **compliant** by default
- LAPS: Win 11 22H2+ built-in, else **KB5025221**; password readable by Intune Admin+
- Co-management: **7** workloads; ConfigMgr **1710+**, Win10 **1709+**, Hybrid-join required
- Exam: **65 q / 120 min / 700-1000 pass**; retake waits: 24h after 1st fail, 14d after 2nd
- Which-policy pairs: report state → compliance policy; enforce setting → config profile; block access → CA; configure BitLocker → endpoint security disk-encryption profile; app data on BYOD → APP/MAM-WE

Take the free MS practice assessment again end of Week 1 and every Friday after — it's free and unlimited; the score trend is the drift alarm.
