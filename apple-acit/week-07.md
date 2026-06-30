# Week 7 — Apple Exam Week: SUP-2026 + DEP-2026

**Dates:** Aug 15 – Aug 21, 2026
**Target hours:** ~3h (reduced load — exam week)
**Focus:** Final retrieval practice for both Apple certs, exam days, post-exam debrief
**Exam days (target):** SUP-2026 → Wednesday Aug 19 | DEP-2026 → Thursday Aug 20

> **No new content this week.** Both Apple certs in one week — keep sessions short and focused. Trust the 6 weeks of prep.

---

## Exam Format Reminder

### SUP-2026 — Apple Certified Support Professional (ACSP)
- **Format:** Multiple choice + scenario-based questions
- **Proctor:** Kryterion online proctoring or Apple-authorised test centre
- **Preparation:** Apple IT Training platform — complete all SUP-2026 modules

### DEP-2026 — Apple Certified IT Professional (ACIT)
- **Format:** Multiple choice + scenario-based questions
- **Proctor:** Kryterion online proctoring or Apple-authorised test centre
- **Preparation:** Apple IT Training platform — complete all DEP-2026 modules

---

## Saturday Aug 15 — Final Apple Retrieval Session

**Hours:** 1.5h | Last big block — retrieval only, no reading

### SUP-2026 Cold Recall: Device Support

Answer from memory first, then verify:

**Hardware:**
1. How do you identify Apple Silicon vs Intel on a Mac without opening it?
2. MagSafe port generations: which MacBooks use MagSafe 1, 2, and 3?
3. What happens when you hold T at startup on an Intel Mac with Thunderbolt? On Apple Silicon with another Mac connected?
4. Name 5 startup key combinations for Intel Mac and their functions.
5. Which Apple Silicon Macs support Target Disk Mode via USB-C?

**Startup Modes & Recovery:**
1. What is the difference between macOS Recovery (Cmd+R) and Internet Recovery (Option+Cmd+R / Shift+Option+Cmd+R)?
2. Safe Mode on Apple Silicon: how do you enter it? What does it disable?
3. DFU mode vs Recovery mode: what is the fundamental difference in what software is visible to the host computer?
4. When would you use Apple Configurator 2 to restore a Mac in DFU mode?
5. How do you reinstall macOS without erasing user data? What is the correct Recovery option?

**Diagnostics:**
1. Apple Diagnostics: how to run it on Intel vs Apple Silicon?
2. What does a reference code starting with "ADP" indicate?
3. Console.app: what is the difference between the All Messages view and a specific subsystem filter?
4. `log show` vs `log stream`: when would you use each?
5. What is the `ping` equivalent check for a Mac that boots but cannot reach the internet?

**FileVault:**
1. FileVault recovery key types: what is the difference between a Personal Recovery Key and an Institutional Recovery Key?
2. `fdesetup status`: what does it output when FileVault is enabled and encryption is in progress?
3. `fdesetup list`: what does it show?
4. A user is not on the FileVault enabled users list. The Mac is encrypted. What happens when they try to boot the Mac?
5. How does MDM manage FileVault differently on Intel vs Apple Silicon (T2 chip)?

### DEP-2026 Cold Recall: Deployment & Management

**ABM + ADE:**
1. ABM roles: list all 6 roles and one key permission each.
2. ADE zero-touch flow: describe it in 5 steps from factory-new device to enrolled desktop.
3. What MDM enrollment profile setting makes a Mac supervised? What does supervision unlock?
4. Can a device enrolled via ADE be removed from MDM by the user? What happens if the device is wiped?
5. How does Configurator add devices to ABM? What workflow does it follow?

**Enrollment Types:**
1. User Enrollment: what identifier does the MDM server see instead of serial number? Why?
2. Device Enrollment: can the user remove the MDM profile? What type of devices is it designed for?
3. ADE enrollment: what two things make it different from Device Enrollment?

**Configuration Profiles:**
1. `PayloadRemovalDisallowed = true` on an unsupervised device: what happens when the user tries to remove the profile?
2. Two MDM profiles push the same payload type (e.g., passcode). Which setting wins?
3. Name 4 supervised-only payload types or restrictions.
4. What file format is a configuration profile? What is the root structure?

**DDM:**
1. What are the 4 declaration types in DDM?
2. What is the status channel and which direction does it communicate?
3. Minimum OS requirements for DDM?
4. Can DDM and legacy MDM commands coexist on the same device?

**VPP:**
1. User-based vs device-based licensing: which requires a Managed Apple ID on device?
2. VPP workflow: describe 4 steps from purchasing in ABM to app appearing on device.
3. A license is revoked from a user. What happens to the app on their device?
4. What is a custom app (B2B) and how is it distributed via ABM?

### Post-session
- Anki: review all "Again" + "Hard" cards from Apple weeks 1–6
- Flag any cold-recall failures — revisit on Mon

---

## Sunday Aug 16 — REST

No study. Full rest day, no exceptions.

---

## Monday Aug 17 — Final Exam Traps + Logistics

**Hours:** 1h | Day before first Apple exam — light only

### SUP-2026 Top Exam Traps (15 min)

- **Apple Silicon Target Disk Mode:** not available via keyboard — use System Settings > General > Startup Disk (or Finder on the host Mac via USB-C)
- **DFU mode:** device appears completely dark; no Apple logo, no progress bar; Finder/iTunes shows device in DFU mode
- **Safe Mode:** on Apple Silicon, hold power button until "Loading startup options" → click disk, hold Shift → Safe Mode; on Intel, hold Shift at power-on
- **Internet Recovery:** downloads macOS directly from Apple servers; slower than local recovery; fallback when local Recovery volume is corrupted
- **FileVault multi-user unlocking:** only users added to FileVault can unlock at startup; adding a new admin after encryption does NOT automatically add them
- **T2 firmware password:** cannot be reset by user, Apple Configurator, or MDM — requires Apple Authorized Service
- **`fdesetup removeuser`:** removes a user from the FileVault unlock list; does NOT decrypt the drive
- **Apple Diagnostics reference codes:** 4-character codes starting with specific letters indicate subsystem (ADP = memory, VFD = video/GPU, etc.)

### DEP-2026 Top Exam Traps (15 min)

- **`PayloadRemovalDisallowed`:** only enforced on supervised devices — on unsupervised, user CAN remove the profile despite the flag
- **User Enrollment:** no serial number in MDM, no full wipe possible, no device-based CA compliance — designed for BYOD with privacy separation
- **Two profiles, same payload type:** most restrictive setting wins — NOT an error state (unlike Intune, which shows Error for conflicting profiles)
- **DDM minimum OS:** macOS 13 / iOS 16 — devices running older OS fall back to legacy MDM silently
- **Device-based VPP:** no Apple ID required on device — correct answer for any shared or kiosk scenario
- **ADE + supervised:** supervision persists through OS wipe — a wiped ADE device re-enrolls automatically when connected to internet
- **Configurator add to ABM:** uses "Add to Organization's Apple Business Manager" workflow inside Configurator 2 — requires Configurator to be authorized in ABM first
- **ABM Content Manager role:** can buy and assign apps/books in ABM but cannot manage devices or enroll MDM servers

### Exam Logistics Checklist (both exams)

- [ ] SUP-2026 booking confirmed — Kryterion email received
- [ ] DEP-2026 booking confirmed — Kryterion email received
- [ ] Exam format: online proctored (Kryterion) or test centre?
  - **Online:** quiet room, webcam on, ID visible, clear desk, no notes
  - **Test centre:** arrive 15 min early, government-issued photo ID
- [ ] ID ready: government-issued photo ID
- [ ] If online: Kryterion system check completed
- [ ] Start times confirmed for both exams — set alarms

---

## Tuesday Aug 18 — Final Day Before SUP-2026

**Hours:** 0.5h | Only if you feel you need it — otherwise rest

### Optional: 10-minute SUP-2026 mental run-through

Walk through this sequence in your head, no notes:
1. A user calls: "My Mac won't boot past the Apple logo." — What do you check first? (Safe Mode → Verbose Mode → Recovery)
2. A MacBook is stuck in a boot loop after a failed update. — Recovery options in order of invasiveness?
3. A user reports the login screen never appears — FileVault might be the cause. — How do you check?

If any answer felt uncertain → 10 min of targeted Anki, then stop.

Evening: no study after 8pm. Normal sleep.

---

## Wednesday Aug 19 — SUP-2026 EXAM DAY

**Target time:** Check your booking confirmation

### Morning checklist (2 hours before exam)
- [ ] Light meal — nothing heavy
- [ ] Skim top traps only (the list from Monday — 5 min max)
- [ ] Online: join Kryterion 30 min early; camera/mic check; ID ready; clear desk
- [ ] Test centre: arrive 15 min early

### Exam strategy
1. Scenario questions: read fully — the situation in the question stem often rules out 2 of 4 answers
2. Hardware questions: visualise the physical Mac (ports, form factor) before answering
3. Flag uncertain questions — answer your best guess, return at end
4. No blank answers

### After the exam
- Record result: pass/fail + score
- Short break — DEP-2026 is tomorrow

---

## Thursday Aug 20 — DEP-2026 EXAM DAY

**Target time:** Check your booking confirmation

### Morning checklist
- [ ] Same as SUP-2026 morning checklist
- [ ] Skim DEP-2026 top traps (5 min — the Monday list)
- [ ] No new reading

### Exam strategy
- Same as SUP-2026
- Watch for questions that contrast Intune vs Apple MDM behaviour (conflict resolution, compliance states, enrollment types) — these are common distractors when you've studied both

### After the exam
- Record result: pass/fail + score for DEP-2026

---

## Friday Aug 21 — Full Certification Wrap-Up

**Hours:** 0.5h | Reflection + updates

### Tasks

1. **Record all three results:**
   - MD-102: pass/fail + score (taken Tue Aug 18)
   - SUP-2026: pass/fail + score (taken Wed Aug 19)
   - DEP-2026: pass/fail + score (taken Thu Aug 20)

2. **Update GitHub:**
   - `DevOPsJourneyman/Certifications` README: update status for each cert
   - Profile README (`DevOPsJourneyman/DevOPsJourneyman`): update cert table with final status
   - Change 🔄 → ✅ for passed certs; add retake note for any fails

3. **LinkedIn (optional but recommended for recruiter visibility):**
   - Add MD-102, SUP-2026, DEP-2026 to Certifications section with issue date
   - One-line post if all three passed — IT engineering recruiters watch for these

4. **Reflect on the 6-week plan:**
   - What worked? (daily breakdown, Apple daily split, practice exam Week 6)
   - What would you cut or change if doing it again?
   - Where did the 70/30 split feel wrong?

5. **If any cert failed:**
   - Check domain breakdown in score report
   - Earliest retake: verify with exam provider (Pearson for MD-102, Kryterion for Apple)
   - Plan targeted 2-week review; do NOT restart from scratch — patch the gaps

---

## Week 7 Summary Card

| Day | Hours | Activity |
|-----|-------|----------|
| Sat Aug 15 | 1.5h | Cold recall — SUP-2026 + DEP-2026 full retrieval |
| Sun Aug 16 | 0h | REST |
| Mon Aug 17 | 1h | Top exam traps + full logistics check |
| Tue Aug 18 | 0h | (MD-102 exam day — see md-102/week-07.md) |
| Wed Aug 19 | — | **SUP-2026 EXAM DAY** |
| Thu Aug 20 | — | **DEP-2026 EXAM DAY** |
| Fri Aug 21 | 0.5h | Full wrap-up + GitHub/LinkedIn updates |
| **Total** | **~3h** | |

---

## Coverage Confirmation (all 6 Apple weeks)

| Week | Cert | Topics |
|------|------|--------|
| 1 | SUP-2026 | Hardware ID, Apple Silicon vs Intel, ports, startup keys |
| 2 | SUP-2026 | Recovery modes, Safe Mode, DFU, macOS reinstall |
| 3 | SUP-2026 | Apple Diagnostics, Console.app, FileVault intro |
| 4 | SUP-2026 | FileVault management, T2/Secure Enclave, Diagnostics codes |
| 5 | DEP-2026 | ABM, ADE, supervision, enrollment types, config profiles |
| 6 | DEP-2026 | Config profile payloads, DDM, VPP management |
| 7 | Both | Final retrieval + exam days |
