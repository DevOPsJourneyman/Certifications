# Week 6 — MD-102 Full Review + Timed Practice Exam

**Dates:** Aug 8 – Aug 14, 2026
**Target hours:** ~10.5h (70% of weekly target)
**Focus:** Domain-by-domain review, Anki consolidation, full 65-question timed practice exam, targeted debrief
**Exam weight:** All 4 domains — D1 ~25% | D2 ~25% | D3 ~25–30% | D4 ~15–20%

---

## Exam Format Reminder

- **Questions:** 65 (mix of multiple choice, case study, drag-and-drop)
- **Time:** 120 minutes → ~1 min 50 sec per question
- **Passing score:** 700 / 1000
- **Lab simulations (Performanced-based):** some questions require clicking through an Intune/Entra interface in a browser simulation — no typing shortcuts
- **Strategy:** answer all questions, flag uncertain ones, return at end; don't leave blanks

---

## Daily Session Template

1. **Review** (10 min) — Anki cards from previous session
2. **Read/Practice** (varies) — topic review or practice questions
3. **Self-check / debrief** (15 min) — identify weak areas
4. **Anki update** (10 min) — reinforce weak cards

---

## Saturday Aug 8 — D1 + D2 Full Review

**Hours:** 3.5h | Big block — structured review

### Domain 1 Review: Deploy Windows Client (~25%)

Key concepts to confirm you can recall without notes:

**Windows Autopilot:**
- Autopilot profiles: User-Driven (user completes OOBE), Self-Deploying (kiosk/shared, no user interaction, requires TPM 2.0), Pre-provisioning (Technician phase + User phase)
- Autopilot device registration: hardware hash CSV import, OEM direct registration, Intune auto-capture at first enrollment
- Deployment profile: Deployment mode, join type (Entra join vs Hybrid Entra join), OOBE settings (hide EULA, privacy, keyboard, account)
- Autopilot reset: reinstate Autopilot state without full OS reinstall; preserves Entra join + Intune enrollment
- Enrollment Status Page (ESP): blocks device use until Intune policies apply; configured per profile

**Windows Enrollment:**
- Entra join: cloud-only; Intune MDM auto-enrollment; no on-prem domain join
- Hybrid Entra join: device in on-prem AD AND Entra ID; requires Entra Connect sync; Intune enrollment via GPO or co-management
- Workplace join (Entra register): personal device; no MDM auto-enrollment; used for BYOD access to resources
- Bulk enrollment: Provisioning Package (PPKG) via Windows Configuration Designer; or Autopilot CSV import

**Windows Update for Business (WUfB):**
- Update rings: defer quality updates (0–30 days), defer feature updates (0–365 days), pause updates (35 days max)
- Servicing channels: General Availability Channel (semi-annual feature updates), Long-Term Servicing Channel (LTSC — no feature updates, IoT/kiosks only)
- Feature update policies: target a specific Windows 11 version; combined with update ring deferral
- Delivery Optimization: peer-to-peer update caching; configured via Intune profile; DO Group ID groups peers

**Co-management:**
- Requires: Hybrid Entra join + Configuration Manager client + Intune enrollment
- Workload slider: move workloads from ConfigMgr to Intune (compliance, device config, endpoint protection, etc.)
- Pilot collections: move a subset of devices to Intune workload before full rollout

### Domain 2 Review: Identity & Compliance (~25%)

**Entra ID join types (summary):**
| State | AD joined | Entra joined | Entra registered | MDM auto |
|-------|-----------|--------------|------------------|----------|
| On-prem domain | ✅ | ❌ | ❌ | GPO/co-mgmt |
| Entra join | ❌ | ✅ | ✅ | ✅ |
| Hybrid Entra join | ✅ | ✅ | ✅ | GPO/co-mgmt |
| Entra register (BYOD) | ❌ | ❌ | ✅ | ❌ (manual) |

**Conditional Access:**
- Conditions: user/group, cloud app, device platform, location (IP range / named location), client app, device state
- Grant controls: MFA, compliant device, Hybrid Entra joined, approved client app, terms of use
- Session controls: sign-in frequency, persistent browser session, app-enforced restrictions
- Named locations: IP ranges or countries; can be marked trusted

**Windows Hello for Business (WHfB):**
- Replaces password with PIN or biometrics; credentials stored in TPM — never transmitted
- Provisioning: user completes MFA → creates PIN/biometric → key registered in Entra ID
- On-prem vs cloud trust: Cloud Kerberos Trust (hybrid — recommended modern approach); Certificate Trust (requires PKI); Key Trust (hybrid, legacy)
- Admin policy: configure via Intune (Windows Hello for Business profile) or GPO (hybrid)

**SSPR (Self-Service Password Reset):**
- Authentication methods: mobile app notification, mobile app code, email, mobile phone, office phone, security questions
- Number of methods required: 1 or 2 (configurable)
- SSPR writeback: for hybrid environments — resets sync back to on-prem AD via Entra Connect

**Microsoft Defender for Endpoint (MDE) — D2 context:**
- Onboarding: via Intune, SCCM, Group Policy, local script
- Alert severity: Informational, Low, Medium, High
- Automated investigation and remediation (AIR): automatically investigates alerts and remediates if configured

### MS Learn Review Modules
- [Review and revisit MD-102 learning path](https://learn.microsoft.com/en-us/certifications/exams/md-102/)
- Scan weak flashcards for D1 + D2 only today

### Practice Questions (D1 + D2 mixed)
1. An Autopilot device shows "Profile assigned" but enrollment fails at the ESP stage. The ESP is configured to block device use until all apps are installed. A required app fails to install. What does the user see?
2. A hybrid Entra join device is in Configuration Manager. Co-management is enabled with the compliance workload moved to Intune. A ConfigMgr compliance rule is also active. Which policy applies?
3. A user is in both a CA "require MFA" policy (all users) and a CA "require compliant device" policy (a named group). Both apply. The user's device is compliant. Does the user need MFA?
4. WHfB is configured via Intune. A user has not completed MFA registration. Can the user complete WHfB provisioning?
5. SSPR writeback is enabled. A remote user resets their Entra ID password via SSPR. The on-prem AD is unreachable (VPN down). What happens to the on-prem password?

---

## Sunday Aug 9 — REST

No study. Full rest day.

---

## Monday Aug 10 — D3 + D4 Full Review

**Hours:** 3.5h | Big block — structured review

### Domain 3 Review: Manage, Maintain, and Protect Devices (~25–30%)

**Device configuration profiles:**
- Settings catalog vs ADMX templates vs device restrictions vs custom OMA-URI
- Conflict resolution: same setting in two profiles → Error state for that setting; other settings unaffected
- Assignment: user group (follows user) vs device group (follows device, applies at enrollment)
- Profile applicability filter: scope tags (RBAC), OS version filters

**Compliance policies:**
- States: Compliant, Not Compliant, In Grace Period, Not Evaluated, Error
- Grace period: days before non-compliant mark; CA still evaluates during grace period
- Actions for non-compliance: mark not compliant → email → push notification → remote lock → retire
- Device-based CA: "Require device to be marked compliant" — Intune-enrolled + compliant state required

**LAPS:** Win 11 22H2+ built-in or Win 10/11 + KB5025221; Entra ID backup; Intune Admin role can view password

**Remote actions:** Retire (remove MDM + company data), Wipe (factory reset), Fresh Start (reinstall Windows, keep user data), Delete (removes Intune record only — no device wipe), Sync, Restart, Collect diagnostics, Rotate BitLocker key, Lost Mode (supervised iOS)

**Endpoint security policies:** Antivirus (Defender AV), Disk encryption (BitLocker/FileVault), Firewall, EDR, ASR, Account protection (LAPS, WHfB)

**BitLocker:** compliance policy reports state only; endpoint protection profile configures BitLocker; recovery key: Devices → device → Recovery keys

**MDE integration:** onboarding via EDR policy; threat level used in compliance condition; security settings management = Defender settings from MDE portal without Intune enrollment

**Endpoint analytics:** startup score, app reliability, restart frequency, resource performance; requires Intune enrollment + telemetry

### Domain 4 Review: Manage Apps (~15–20%)

**Win32 app deployment:**
- IntuneWinAppUtil: `-c` (source) `-s` (setup file) `-o` (output) → .intunewin
- Detection: MSI product code / File or folder / Registry / PowerShell script (exit 0 = detected)
- Install context: System (installs as SYSTEM, no user needed) vs User (per-user)
- Assignment types: Required / Available (Company Portal) / Uninstall
- Requirements rule: checked BEFORE install attempt — if not met, Intune skips
- Supersedence: v2 replaces v1; Dependency: App B installs before App A

**M365 Apps:**
- Update channels: Current Channel (monthly, latest), Monthly Enterprise (monthly, 1-month lag), Semi-Annual Enterprise (twice/year)
- Deployed via Intune → Apps → Windows → Microsoft 365 Apps (Windows 10 and later)

**MAM / App Protection Policies:**
- MAM-WE (without enrollment): BYOD data protection without MDM
- APP data transfer settings: restrict Save As, restrict copy/paste to managed apps only
- Selective wipe: removes org data from managed app, not personal data
- WIP: legacy; Allow Override = user can copy but logged; Block = hard block; being replaced by Windows MAM
- Conditional launch: block jailbroken/rooted, min OS version, max PIN attempts

### Practice Questions (D3 + D4 mixed)
1. A device has an Intune compliance policy that requires BitLocker. The device is not encrypted. The CA policy requires compliance to access SharePoint. The user tries to access SharePoint. What is the CA decision?
2. An admin assigns a Win32 app as "Available" to All Users. A user on a non-enrolled personal device opens Company Portal. Will the app appear for install?
3. An App Protection Policy sets "Send org data to other apps" = Policy managed apps only. A user opens a Word document from SharePoint (managed app) and copies text. They open their personal Gmail app (unmanaged). Can they paste?
4. An ASR rule is in Audit mode. A user runs a script that the ASR rule would normally block. What is the outcome?
5. An endpoint protection profile enables BitLocker with recovery key escrow to Entra ID. The device already has BitLocker enabled with a key stored only locally. What does Intune do?

---

## Tuesday Aug 11 — Gap Fill + Weak Area Targeting

**Hours:** 2.5h | WFH — focused review

### Tasks
1. Pull Anki deck: sort by "Again" and "Hard" cards — study bottom 30 weak cards across all domains
2. Review any self-check failures from Sat + Mon
3. 5 cross-domain scenario questions:
   1. A company has Autopilot + ESP configured with "Block device use until enrolled" = Yes. The ESP profile assigns 5 required apps. App 4 fails to install (network issue). What does the user see at OOBE?
   2. Co-management is enabled with the Device Configuration workload set to Intune. A ConfigMgr admin pushes a configuration item to a co-managed device. Does it apply?
   3. A Win32 app is deployed with a PowerShell detection script. The script exits with code 1. Intune retries the install. The app is actually installed but the detection script is wrong. What is the result?
   4. A CA policy requires MFA and a compliant device for Office 365. A user is on an Autopilot-enrolled device (Entra joined, compliant). They sign in on a new browser session. Do they need MFA?
   5. An admin configures a Wi-Fi profile assigned to an Entra user group. A new device enrolls but no user has signed in yet. Will the Wi-Fi profile apply?
4. Flag exam booking status — confirm exam is scheduled for Aug 18 or 19

---

## Wednesday Aug 12 — Full Timed Practice Exam

**Hours:** 2.5h | Office (or home — uninterrupted block required)

### Instructions
- Use MeasureUp, Whizlabs, Microsoft official practice assessment, or equivalent
- 65 questions, 120 minutes — set a timer, no pausing
- Exam rules: no notes, no open tabs, simulated exam conditions
- Flag uncertain questions during exam — return at end
- Record your score and which domains had the most failures

**After the exam:**
1. Note overall score (target: ≥700 / 1000 or 70%+)
2. Per-domain breakdown — which domain had the highest failure rate?
3. List the top 5 questions you were most unsure about — look them up immediately after
4. Tag as "pre-exam baseline" — compare against any practice run from Week 2

**If score ≥ 75%:** Proceed to targeted review of failed questions only (Thursday)
**If score 60–74%:** Extended debrief — Thursday full domain review of weakest domain
**If score < 60%:** Reassess — consider delaying exam by 1 week; shift Week 7 by one week

---

## Thursday Aug 13 — Practice Exam Debrief

**Hours:** 2h | Office

### Tasks
1. Go through every wrong answer from Wednesday's exam — understand WHY the correct answer is correct
2. For each wrong answer: add or update an Anki card
3. Focus areas by most-missed domain:
   - D1 weak: Autopilot profile settings, Hybrid Entra join + co-management pre-requisites
   - D2 weak: CA conditions + grant types, WHfB trust models, SSPR writeback
   - D3 weak: compliance states + grace period, BitLocker policy vs compliance policy distinction
   - D4 weak: Win32 detection rule types, MAM data transfer settings, WIP modes
4. Re-attempt the 10 hardest questions from the practice exam without looking at answers

---

## Friday Aug 14 — Final Anki Review + Exam Week Prep

**Hours:** 1h | Office — Anki + logistics

### Tasks
1. Full Anki review: all 4 domains — prioritize "Again" cards from the week
2. Confirm exam booking: MD-102 scheduled for Aug 18 or 19
3. Final logistics:
   - Exam centre address / Pearson OnVUE online requirements (quiet room, webcam, ID)
   - Review ID requirements: government-issued photo ID required
   - Arrive 15 min early (test centre) or connect 30 min early (online proctored)
4. D1–D4 high-confidence check — for each domain, confirm the top 3 exam traps you know cold:
   - D1: Autopilot Self-Deploying requires TPM 2.0; Fresh Start keeps user data; co-management requires Hybrid Entra join
   - D2: CA grant "require compliant device" needs Intune-managed device; Workplace join alone is insufficient; WHfB credentials stored in TPM — never transmitted
   - D3: Compliance policy reports only — CA does the blocking; BitLocker compliance ≠ BitLocker config; LAPS requires KB5025221 on Win 10/11
   - D4: Requirements rule checked BEFORE install; detection exit 0 = detected; MAM selective wipe removes org data only; WIP Allow Override logs but doesn't block
