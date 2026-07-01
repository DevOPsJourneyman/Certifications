# Certifications

Structured study notes and progress tracking for active certification pursuits.

## Status

| Cert | Domain | Progress | Target |
|------|--------|----------|--------|
| MD-102 Endpoint Administrator | Microsoft | 🔄 In progress | Exam: Week 8 |
| SUP-2026 (Apple Device Support) | Apple | 🔄 In progress | Exam: flexible, booked once Week 4 content is solid |
| DEP-2026 (Apple Deployment & Management) | Apple | 🔄 In progress | Exam: Week 9 |

Goal: all 3 passed by end of August 2026.

## Structure

- [`md-102/`](./md-102/) — Microsoft Intune Endpoint Administrator (weeks 01–08)
- [`apple-acit/`](./apple-acit/) — Apple SUP-2026 (Device Support) + DEP-2026 (Deployment & Management) (weeks 01–09)

## Study Schedule

9-week plan, started 2026-07-04. Week files in each cert folder are the source of truth — this table is a pointer.

| Week | Dates | MD-102 | Apple |
|------|-------|--------|-------|
| 1 | Jul 4–10 | D1: Deploy Windows | SUP-2026: Hardware, ports, Apple Silicon vs Intel |
| 2 | Jul 11–17 | D2: Identity & Compliance | SUP-2026: Recovery modes, Safe Mode, DFU |
| 3 | Jul 18–24 | Catchup/review buffer | SUP-2026: Diagnostics, Console.app, FileVault intro |
| 4 | Jul 25–31 | D3 Part 1: config profiles, LAPS | SUP-2026: FileVault mgmt, T2/Secure Enclave — **SUP-2026 exam booked here or shortly after** |
| 5 | Aug 1–7 | D3 Part 2 + D4: compliance, MDE, apps | DEP-2026: ABM, ADE, supervision, enrollment types |
| 6 | Aug 8–14 | Full review + timed practice exam | DEP-2026: config profile payloads, DDM, VPP |
| 7 | Aug 15–21 | Final review + retrieval | DEP-2026 final review + retrieval |
| 8 | date TBD | **MD-102 exam** | — |
| 9 | date TBD | — | **DEP-2026 exam** |

Weeks 8–9 dates aren't fixed yet — pick a window that keeps the ~1-week gap between the last two exams while staying as close to end-of-August as practical.

## Lab environment

- **Windows/Intune:** dev tenant (personal)
- **Apple hardware:** 2× Apple Silicon MacBook Pro (M1/M2). One is a production AI server — not used for hands-on OS-level labs (no reboot/recovery/DFU testing on it). The other is a spare unit, already assigned in Kambi's Apple Business Manager (ABM), awaiting deployment — used for SUP-2026 hardware/recovery labs and to observe the real DEP-2026 ADE enrollment flow live when it deploys.
- **Apple MDM:** Kambi manages Macs and iPads via Microsoft Intune (not Jamf) and has an active requirement to DEP/ADE-enroll all Mac devices. DEP-2026 conceptual content is cross-checked against this live tenant — read-only observation, no destructive testing against production-assigned devices.
