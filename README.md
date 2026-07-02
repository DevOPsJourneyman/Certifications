# Certifications

Structured study notes and progress tracking for active certification pursuits.

## Status

| Cert | Domain | Progress | Target |
|------|--------|----------|--------|
| MD-102 Endpoint Administrator | Microsoft | 🔄 In progress | Exam: Week 8 |
| SUP-2026 (Apple Device Support) | Apple | 🔄 In progress | Exam: Week 5 (gated by Week 4 practice exam) |
| DEP-2026 (Apple Deployment & Management) | Apple | 🔄 In progress | Exam: Week 9 |

Goal: all 3 passed by end of August 2026. **Non-negotiable — the schedule flexes hours, not dates.**

## Structure

- [`md-102/`](./md-102/) — Microsoft Intune Endpoint Administrator (weeks 01–08)
- [`apple-acit/`](./apple-acit/) — Apple SUP-2026 (Device Support) + DEP-2026 (Deployment & Management) (weeks 01–09)

## Study Schedule

9-week plan, started 2026-07-04. Week files in each cert folder are the source of truth — this table is a pointer.

**v2 (2026-07-02):** SUP-2026 weeks 1–4 restructured around the real exam syllabus (iPhone + iPad + Mac + Apple Account + iCloud, on iOS 26/iPadOS 26/macOS Tahoe). Weekly load raised to ~17.5h: MD-102 ~10.5h + Apple ~7h. Baseline: MD-102 official practice assessment ≤10/50 on 2026-07-02 — the gap is detail-level facts (limits, which-policy-when), which the Anki "numbers deck" targets.

| Week | Dates | MD-102 (~10.5h) | Apple (~7h) |
|------|-------|--------|-------|
| 1 | Jul 4–10 | D1: Deploy Windows | SUP: device setup, Apple Account, iCloud, Managed Apple Accounts |
| 2 | Jul 11–17 | D2: Identity & Compliance | SUP: iPhone/iPad — backup/restore, reset, cellular, Activation Lock, Lockdown Mode |
| 3 | Jul 18–24 | Catchup/review buffer | SUP: Mac — accounts, FileVault, startup/Recovery, diagnostics |
| 4 | Jul 25–31 | D3 Part 1: config profiles, LAPS | SUP: networking, security, updates/storage/Continuity — **practice exam Thu (booking gate)** |
| 5 | Aug 1–7 | D3 Part 2 + D4: compliance, MDE, apps | DEP: ABM, ADE, supervision, enrollment types — **SUP-2026 exam this week** |
| 6 | Aug 8–14 | Full review + timed practice exam | DEP: config profile payloads, DDM, VPP |
| 7 | Aug 15–21 | Final review + retrieval | DEP final review + retrieval |
| 8 | date TBD | **MD-102 exam** | — |
| 9 | date TBD | — | **DEP-2026 exam** |

Weeks 8–9 dates aren't fixed yet — pick a window that keeps the ~1-week gap between the last two exams while staying as close to end-of-August as practical.

## Lab environment

- **Windows/Intune:** dev tenant (personal) + **2–3 spare HP laptops** for real enrollment labs — hardware-hash Autopilot import, ESP observation, compliance/CA testing, Win32 app deployment on physical hardware
- **Apple hardware:** 2× Apple Silicon MacBook Pro (M1/M2). One is a production AI server — not used for hands-on OS-level labs (no reboot/recovery/DFU testing on it). The other is a spare unit, already assigned in Kambi's Apple Business Manager (ABM), awaiting deployment — used for SUP-2026 Mac labs (Recovery, FileVault, Diagnostics) and to observe the real DEP-2026 ADE enrollment flow live when it deploys.
- **iPhone/iPad:** personal iPhone if available for SUP-2026 iOS labs (all non-destructive); otherwise iOS content is exam-knowledge via course articles
- **Apple MDM:** Kambi manages Macs and iPads via Microsoft Intune (not Jamf) and has an active requirement to DEP/ADE-enroll all Mac devices. DEP-2026 conceptual content is cross-checked against this live tenant — read-only observation, no destructive testing against production-assigned devices.

## Exam delivery

- **MD-102:** Pearson VUE (OnVUE online proctored or test centre)
- **SUP-2026 + DEP-2026:** Pearson VUE / OnVUE — Apple moved off Kryterion; practice exams are paid, via ACRS → Pearson VUE
