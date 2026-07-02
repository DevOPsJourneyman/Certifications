# Week 1 — Apple SUP-2026: Device Setup + Apple Account + iCloud

**Dates:** Jul 4 – Jul 10, 2026
**Target hours:** ~7h
**Focus:** iPhone/iPad/Mac setup, Apple Account, iCloud features, Managed Apple Accounts, device management intro
**Cert:** Apple Certified Support Professional — Apple Device Support Exam (SUP-2026)
**Course:** [Apple Device Support](https://it-training.apple.com/tutorials/support) — modules: Getting Started, Device Setup, Apple Account + iCloud, Introduction to Device Management

> Plan v2 (2026-07-02): restructured to match the real SUP-2026 syllabus — the exam covers iPhone, iPad, Mac, Apple Account, and iCloud on iOS 26/iPadOS 26/macOS Tahoe, not just Mac hardware/recovery. Complete the **Check Your Understanding** questions in each course article — they're free and mirror the exam style.

**Lab devices:** 2nd Apple Silicon MacBook Pro (spare — not Newton). iPhone/iPad labs: use a personal iPhone if available (all tasks non-destructive); otherwise treat as exam-knowledge and lean harder on course screenshots.

---

## Daily Session Template

1. **Review** (5 min) — Anki cards from previous session
2. **Read** (varies) — course article(s) + Check Your Understanding questions
3. **Self-check** (10 min) — 3–4 exam-style questions
4. **Anki update** (5 min) — add cards for today's key facts

---

## Hands-On Lab (this week)

1. On the spare MacBook Pro: walk Setup Assistant awareness — System Settings → sign in view, note what an Apple Account unlocks (iCloud, FaceTime, App Store) vs what works without one
2. System Settings → [Your Name] → iCloud — list every iCloud feature toggle present (Drive, Photos, Keychain, FileVault recovery, Find My, Private Relay) and note which are on
3. `system_profiler SPHardwareDataType` — confirm Model Identifier + serial; find the same info in System Information GUI
4. If iPhone available: Settings → [Your Name] → iCloud — compare the same feature list on iOS; check iCloud Backup state and last backup time
5. In Kambi context: note whether your work account is a Managed Apple Account and whether it's federated with Entra ID (you'll verify in the ABM portal in Week 5)

---

## Saturday Jul 4 — Platform Fundamentals + Device Setup

**Hours:** 2.5h

### Topics
- Apple platforms overview: iOS 26, iPadOS 26, macOS Tahoe — shared design, Continuity family
- iPhone/iPad setup flow: language/region, Quick Start (proximity setup from old device), Wi-Fi, Face ID/Touch ID, passcode, restore options (iCloud backup, Mac/PC backup, direct transfer), Apple Account sign-in, Siri, Screen Time
- Mac setup flow: Setup Assistant, Migration Assistant (from Mac, Time Machine, PC), local account creation, Apple Account sign-in, FileVault prompt, Touch ID
- Setup differences for organization-owned devices: Automated Device Enrollment intercepts Setup Assistant (Remote Management screen) — detail lives in DEP-2026, know it exists for SUP
- Identifying devices: Model Identifier, serial number, System Information / Settings → General → About

### Self-Check
1. A user gets a new iPhone and holds it near their old one. What setup feature activates, and what does it transfer?
2. Which three restore sources can a user choose from during iPhone setup?
3. During Mac setup, Migration Assistant can pull data from which three sources?
4. A company iPad shows a "Remote Management" screen during setup. What does this indicate?
5. Where on iPhone do you find the serial number without the physical device label?

### Anki Cards to Build
- Quick Start: proximity setup — transfers settings + data from old device or iCloud
- iPhone restore options at setup: iCloud backup / Mac or PC backup / direct device transfer
- Migration Assistant sources: another Mac, Time Machine backup, Windows PC
- Remote Management screen at setup = device assigned to MDM via Automated Device Enrollment
- Settings → General → About = serial, model, OS version (iOS); System Information (macOS)

---

## Sunday Jul 5 — REST

No study. Full rest day.

---

## Monday Jul 6 — Apple Account + iCloud

**Hours:** 2h

### Topics
- **Apple Account** (formerly Apple ID): one account for App Store, iCloud, FaceTime, iMessage, Find My
  - Trusted devices + trusted phone numbers; two-factor authentication (on by default, cannot practically disable)
  - Account recovery: recovery contacts, recovery key, account recovery waiting period
- **iCloud core features:** iCloud Drive, Photos, Mail, Keychain (password sync + passkeys), Find My, Private Relay (iCloud+), Hide My Email (iCloud+), custom email domain (iCloud+)
- **iCloud storage:** 5 GB free; iCloud+ paid tiers; what counts against storage (backups, Photos, Drive) vs what doesn't (purchased media)
- **iCloud Backup (iOS/iPadOS):** what's included (app data, settings, Home Screen) vs excluded (already-synced data like Photos-in-iCloud, mail)
- **Find My:** device locating, Lost Mode, remote erase, Activation Lock tie-in (deep-dive Week 2)
- **Keychain:** iCloud Keychain sync across devices; passkeys; Keychain Access app on Mac

### Self-Check
1. A user enables iCloud Photos and iCloud Backup on iPhone. Are photos included in the iCloud backup? Why or why not?
2. What two "trusted" elements does Apple Account two-factor authentication rely on?
3. A user forgets their Apple Account password and has no trusted device access. Name two recovery mechanisms they may have set up in advance.
4. Which iCloud features require a paid iCloud+ subscription? Name two.
5. Where does a Mac user inspect saved passwords and certificates locally?

### Anki Cards to Build
- iCloud free tier: 5 GB — backups + Photos + Drive count against it
- iCloud Backup excludes data already synced to iCloud (Photos, Contacts, Mail)
- 2FA: trusted devices + trusted phone numbers
- Account recovery: recovery contact / recovery key / waiting-period recovery
- iCloud+ only: Private Relay, Hide My Email, custom email domain
- Keychain Access (Mac) = local view; iCloud Keychain = cross-device sync + passkeys

---

## Tuesday Jul 7 — Managed Apple Accounts + Device Management Intro

**Hours:** 1h

### Topics
- **Managed Apple Accounts:** created/owned by org via Apple Business Manager; separate iCloud storage; no Apple Pay; can coexist with personal Apple Account on the same device (work/personal separation)
- Federated authentication: Managed Apple Accounts linked to Microsoft Entra ID or Google Workspace — corporate password signs into Apple services
- **Device management intro (SUP level):** what MDM is, what a profile is, supervised vs unsupervised at a support level — user-facing symptoms: Settings → General → VPN & Device Management, "this device is supervised" banner
- Support boundary: what a helpdesk tech can fix vs what needs the MDM admin (e.g. user can't remove a supervised MDM profile — that's by design, not a fault)

### Self-Check
1. A user has a personal Apple Account and a Managed Apple Account on the same iPhone. Whose iCloud storage does work data use?
2. Name two things a Managed Apple Account cannot do that a personal one can.
3. A Kambi user signs into their Managed Apple Account with their Entra ID password. What makes this possible?
4. A user complains they can't delete a profile in Settings. The device shows a "supervised" banner. Is this a defect?

### Anki Cards to Build
- Managed Apple Account: org-owned, ABM-created, separate iCloud storage, no Apple Pay
- Federation: Managed Apple Account ↔ Entra ID / Google Workspace — corporate credentials
- Supervised banner + immovable profile = expected MDM behaviour, not a fault
- Settings → General → VPN & Device Management = where users see profiles

---

## Wednesday Jul 8 — Review + Self-Check

**Hours:** 0.5h | Office

1. Anki review: all Week 1 cards
2. 3 mixed questions:
   - A user's iPhone says "iCloud storage full" but they bought 200 GB via iCloud+. What's the first thing to check?
   - Quick Start vs restoring from iCloud backup: when would each be the right recommendation?
   - What's the difference between an Apple Account and a Managed Apple Account for App Store purchases?

---

## Thursday Jul 9 — Review + Self-Check

**Hours:** 0.5h | Office

1. Revisit gaps from Wed + course Check Your Understanding questions for this week's modules
2. 3 more questions:
   - A user lost their trusted iPhone and doesn't know their Apple Account password. Walk through their recovery options.
   - Which setup screens differ between a personal iPad and an org-owned ADE-assigned iPad?
   - What does Find My require to have been enabled before a device is lost?

---

## Friday Jul 10 — Anki Review

**Hours:** 0.5h | Office — Anki only

1. Full review of Apple Week 1 deck
2. Flag weak areas — likely: iCloud backup inclusions/exclusions, account recovery paths
3. Confirm labs done: iCloud feature walk on Mac (+ iPhone if available)
