# Week 7 — MD-102 Final Review

**Dates:** Aug 15 – Aug 21, 2026
**Target hours:** ~3h
**Focus:** Final retrieval practice and exam-trap review. No exam this week — MD-102 exam is Week 8.

> No new content this week. Everything is review and retrieval. Exam day, logistics, and post-exam debrief live in `week-08.md`.

---

## Exam Format Reminder

- **Questions:** 65 (mix of multiple choice, case study, drag-and-drop, performance-based)
- **Time:** 120 minutes → ~1 min 50 sec per question
- **Passing score:** 700 / 1000
- **Performance-based questions:** click through a simulated Intune/Entra interface — no keyboard shortcuts
- **Strategy:** answer all questions, flag uncertain ones, return at end; never leave a blank

---

## Saturday Aug 15 — Final Retrieval Session

**Hours:** 2h | Retrieval only, no reading

### Instructions
- No open notes during this session — answer from memory first, then verify
- Goal: find the last few weak spots. Not a review marathon.

### D1 Cold Recall: Deploy Windows Client

Without notes, answer each question. Write down your answer, then check:

1. What are the three Autopilot deployment modes and what distinguishes each?
2. What are the pre-requisites for Windows Autopilot Self-Deploying mode?
3. What is the difference between Entra join and Hybrid Entra join? Which requires Entra Connect?
4. What is the ESP and what happens when a required app fails to install?
5. What does Autopilot Reset do vs a factory reset?
6. WUfB: what is the max deferral for quality updates? Feature updates?
7. What is a co-management workload slider? Name 3 workloads that can be moved from ConfigMgr to Intune.
8. What provisioning options exist for bulk Windows enrollment (non-Autopilot)?

### D2 Cold Recall: Identity & Compliance

1. What are the four Entra ID device states and which ones allow MDM auto-enrollment?
2. CA conditions vs grant controls — give 2 examples of each.
3. WHfB: where are credentials stored? Can they be transmitted over the network?
4. What is Cloud Kerberos Trust and why is it the recommended WHfB model for hybrid environments?
5. SSPR: what is writeback and when does it fail?
6. MDE onboarding: name 3 valid methods.

### D3 Cold Recall: Manage, Maintain, Protect Devices

1. Compliance policy states: list all 5 and explain what triggers "In Grace Period."
2. What is the difference between BitLocker compliance policy and BitLocker configuration profile?
3. LAPS: what OS version is required for built-in LAPS? Where is the backup stored in a cloud-only scenario?
4. Remote actions: distinguish between Retire, Wipe, Delete, and Fresh Start.
5. Conflict resolution: two Intune configuration profiles set the same setting to different values. What is the device state for that setting?
6. MDE Security Settings Management: what does it allow that standard Intune-only deployment does not?

### D4 Cold Recall: Manage Apps

1. Win32 app detection: list the 4 detection rule types. What exit code means "detected"?
2. Requirements rule: when is it evaluated relative to the install attempt?
3. M365 Apps update channels: which channel gets updates monthly with a 1-month lag?
4. MAM selective wipe: what data is removed? What stays?
5. WIP modes: what does "Allow Override" do vs "Block"?
6. Win32 app assignment: what is the difference between Required and Available?

### Post-session
- Add any cold-recall failures as Anki cards immediately
- Flag any domain where more than 2 questions stumped you — revisit Monday

---

## Sunday Aug 16 — REST

No study. Full rest day, no exceptions.

---

## Monday Aug 17 — Top Exam Traps

**Hours:** 1h | Light touch only

Run through these without looking anything up. These are the highest-yield "gotcha" items across all domains:

**D1 Traps:**
- Self-Deploying Autopilot: requires TPM 2.0 — no TPM = enrollment fails
- Hybrid Entra join: requires Entra Connect — if Connect is not deployed, HEJ is impossible
- WUfB max pause: 35 days — after that, device forces update
- Co-management pre-requisite: Hybrid Entra join + ConfigMgr client — Entra join alone is not enough
- Workplace join (Entra register): personal device; NO MDM auto-enrollment on its own

**D2 Traps:**
- CA "require compliant device": requires Intune-managed AND compliant — Entra-registered BYOD alone is insufficient
- WHfB: credentials stored in TPM, never transmitted — correct answer when any question asks about credential security
- Cloud Kerberos Trust: does NOT require certificate infrastructure (unlike Certificate Trust)
- SSPR writeback: if on-prem AD unreachable, SSPR can reset Entra ID password but on-prem password stays unchanged until AD sync resumes

**D3 Traps:**
- Compliance policy alone does NOT block access — CA blocks access using compliance state as a condition
- BitLocker recovery key location: Devices → [device] → Recovery keys (NOT under BitLocker configuration profile)
- LAPS on Win 10/11: requires KB5025221; not built-in like on Win 11 22H2+
- Fresh Start: reinstalls Windows but **keeps personal data** — common distractor vs Wipe (factory reset, removes all)
- Device-based CA "require compliant device": device must be Intune-enrolled — MDM-registered only is not enough

**D4 Traps:**
- Requirements rule failure: app does NOT install, no error to user, Intune just skips silently
- Detection script: exit 0 = app detected (installed), any non-zero = not detected
- MAM selective wipe: removes org data only, personal data intact — correct answer for all BYOD MAM scenarios
- WIP Allow Override: user can override the block BUT action is logged — not a silent bypass
- Available app: user must have Company Portal installed and manually install — not auto-deployed

### Anki + logistics
- Review all "Again" cards from the past week. Nothing else.
- If MD-102 isn't booked yet, book it now — see `week-08.md` for the logistics checklist.

---

## Rest of the week

No new content, no exam this week. Use any remaining short sessions for weak-card Anki review only. Exam day, morning checklist, and post-exam debrief are all in `week-08.md`.
