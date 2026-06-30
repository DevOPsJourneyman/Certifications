# Week 7 — MD-102 Exam Week

**Dates:** Aug 15 – Aug 21, 2026
**Target hours:** ~5h (reduced load — exam week)
**Focus:** Final retrieval practice, exam day logistics, MD-102 exam, post-exam debrief
**Exam day (target):** Tuesday Aug 18, 2026

> **No new content this week.** Everything is review, retrieval, and logistics. If you're tempted to study a new concept on Sunday — don't. Trust the 6 weeks.

---

## Exam Format Reminder

- **Questions:** 65 (mix of multiple choice, case study, drag-and-drop, performance-based)
- **Time:** 120 minutes → ~1 min 50 sec per question
- **Passing score:** 700 / 1000
- **Performance-based questions:** click through a simulated Intune/Entra interface — no keyboard shortcuts
- **Strategy:** answer all questions, flag uncertain ones, return at end; never leave a blank

---

## Daily Session Template (exam week — short)

1. **Review** (5 min) — Anki "Again" + "Hard" cards only
2. **Retrieval** (varies) — cold recall without notes, then verify
3. **Logistics** (as needed) — confirm booking, ID, test environment

---

## Saturday Aug 15 — Final Retrieval Session

**Hours:** 2h | Last big block — retrieval only, no reading

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

## Monday Aug 17 — Final Confidence Check + Exam Logistics

**Hours:** 1.5h | Day before exam — light touch only

### Session 1: Top Exam Traps (30 min)
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

### Session 2: Final Anki + Exam Logistics (30 min)

**Anki:** Review all "Again" cards from the past week. Nothing else.

**Exam logistics checklist:**
- [ ] Exam booking confirmed — Pearson VUE confirmation email received
- [ ] Exam format: Pearson OnVUE (online proctored) or test centre?
  - **Online:** quiet room, no second monitor, webcam required, ID visible to camera, clear desk
  - **Test centre:** arrive 15 min early, government-issued photo ID, no personal items at desk
- [ ] ID ready: government-issued photo ID (passport or driving licence)
- [ ] If online: test system check at [home.pearsonvue.com/op/StandaloneSystemCheck-APT](https://home.pearsonvue.com/op/StandaloneSystemCheck-APT)
- [ ] Start time confirmed — set two alarms
- [ ] Know the break policy: no scheduled breaks for 120-min exam; raise hand (test centre) or pause request (online) if urgent

### Evening before exam
- Stop studying by 8pm
- Light walk or activity — no screens for 30 min before bed
- Sleep target: 7+ hours

---

## Tuesday Aug 18 — MD-102 EXAM DAY

**Target time:** Check your booking confirmation

### Morning checklist (2 hours before exam)
- [ ] Eat — light meal, nothing heavy
- [ ] Review only: top 3 exam traps per domain (printed or written on paper — discard before exam)
- [ ] No new learning, no practice tests
- [ ] Online: join Pearson OnVUE 30 min early; camera/mic check; clear desk; ID ready
- [ ] Test centre: arrive 15 min early; leave phone/watch in car or locker

### Exam strategy
1. Read every question fully before selecting an answer — scenario questions have critical details in the last sentence
2. Performance-based questions (lab simulations): click through carefully; no keyboard shortcuts work
3. Flag any question you're unsure about — answer your best guess, flag, return at end
4. Time check: at question 32, you should have ~60 min remaining; at question 50, ~35 min remaining
5. Never leave a blank — all unanswered questions score 0; a guess has positive expected value

### After the exam
- Score reported immediately on screen (pass/fail + scaled score)
- Official certificate email: 1–5 business days from Microsoft
- If passed: screenshot the score report before it closes
- If failed: note the domain score breakdown — retake eligibility: 24-hour wait after first fail, 14 days after second fail

---

## Wednesday Aug 19 — Post-Exam Debrief

**Hours:** 0.5h | Low effort — reflection only

### Tasks
1. Record result: pass/fail, scaled score, per-domain breakdown (if shown)
2. Update GitHub profile README: change MD-102 status from 🔄 to ✅ (if passed) or add retake note
3. If passed — note 3 things that went well during prep
4. If failed — note which domains scored lowest; plan targeted review before retake
5. Either way: take the rest of the day off. Exam week is physically and mentally draining.

---

## Thursday Aug 20 — Transition Day

**Hours:** 0h active study

Apple exams: SUP-2026 (Aug 19) and DEP-2026 (Aug 20) may already be done by this point.

Take stock of where you are:
- MD-102: done (pass or retake plan in place)
- SUP-2026: done
- DEP-2026: done or today

If DEP-2026 is today — follow the Apple week-07 exam day checklist.

---

## Friday Aug 21 — Wrap-Up

**Hours:** 0.5h | Reflection + next steps

### Tasks
1. Update GitHub Certifications repo README with final status for all certs
2. Update GitHub profile README with completed badges
3. Reflect on 7-week plan:
   - What worked well?
   - What would you change if doing it again?
4. If any cert failed: plan retake timeline (earliest retake dates, target prep time)
5. Post in GitHub profile or LinkedIn if passed — recruiter visibility was the original goal

---

## Week 7 Summary Card

| Day | Hours | Activity |
|-----|-------|----------|
| Sat Aug 15 | 2h | Cold recall across all 4 domains — find last weak spots |
| Sun Aug 16 | 0h | REST |
| Mon Aug 17 | 1.5h | Top exam traps review + full logistics check |
| Tue Aug 18 | — | **MD-102 EXAM DAY** |
| Wed Aug 19 | 0.5h | Post-exam debrief, score record, README update |
| Thu Aug 20 | 0h | Transition / DEP-2026 exam day (see Apple week-07) |
| Fri Aug 21 | 0.5h | Full wrap-up + next steps |
| **Total** | **~4.5h** | |
