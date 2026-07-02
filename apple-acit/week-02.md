# Week 2 — Apple SUP-2026: iPhone + iPad Support

**Dates:** Jul 11 – Jul 17, 2026
**Target hours:** ~7h
**Focus:** iPhone/iPad backup + restore, update/reset options, cellular + hotspot, Activation Lock, Stolen Device Protection, Lockdown Mode, sysdiagnose
**Cert:** Apple Certified Support Professional — Apple Device Support Exam (SUP-2026)
**Course modules:** Setup, Backup, and Recovery for iPhone or iPad; Managing Cellular Connections; device security features

**Lab devices:** personal iPhone if available (all tasks non-destructive). No iPhone → exam-knowledge; study the course article screenshots carefully — the exam tests Settings paths.

---

## Daily Session Template

1. **Review** (5 min) — Anki cards from Week 1
2. **Read** (varies) — course article(s) + Check Your Understanding questions
3. **Self-check** (10 min) — 3–4 exam-style questions
4. **Anki update** (5 min) — add cards for today's key facts

---

## Hands-On Lab (this week, iPhone if available)

1. Settings → [Your Name] → iCloud → iCloud Backup — check state, last backup, backup size breakdown per app
2. Connect iPhone to the spare MacBook Pro → Finder → device sidebar — observe local backup option (encrypted checkbox), Manage Backups
3. Settings → General → Transfer or Reset iPhone — read every option (Reset vs Erase All Content and Settings) without tapping through
4. Settings → Personal Hotspot — enable, join from the MacBook, then troubleshoot-walk: what would you check if a client couldn't see the hotspot?
5. Generate a sysdiagnose on iOS: hold Vol+ + Vol− + Power ~1.5s (device vibrates) → Settings → Privacy & Security → Analytics & Improvements → Analytics Data → sysdiagnose file appears after a few minutes

---

## Saturday Jul 11 — Backup + Restore (iCloud and Mac)

**Hours:** 2.5h

### Topics
- **iCloud Backup:** automatic when locked + charging + Wi-Fi (or 5G on supported models); includes app data, settings, Home Screen, purchase history; excludes already-synced iCloud data
- **Backup to Mac (Finder) / PC (Apple Devices app):** full local backup; **encrypted backup** required to include Health, Keychain, saved passwords, call history — password set by user, unrecoverable if lost
- **Restore paths:** during Setup Assistant (iCloud / computer backup / direct transfer) or via Finder restore
- **Restore vs Update in Finder:** Update reinstalls iOS keeping data; Restore erases then reinstalls
- **Recovery mode (iPhone):** device with corrupted iOS — connect to computer + button sequence → Finder offers Update/Restore
- **DFU on iPhone:** deepest restore, firmware level — last resort before service
- Organizations: MDM can restrict which backup methods are available

### Self-Check
1. A user's encrypted local backup includes which data categories that an unencrypted one omits?
2. iCloud Backup runs automatically when which three conditions are met?
3. A user forgot their encrypted-backup password. Can Apple or the tech recover the backup?
4. In Finder, what's the difference between Update and Restore for a malfunctioning iPhone?
5. A user restores an iCloud backup and their photos are missing. They used iCloud Photos. Where are the photos?

### Anki Cards to Build
- Encrypted local backup adds: Health, Keychain/passwords, call history, Wi-Fi settings
- iCloud Backup auto-runs: locked + power + Wi-Fi
- Encrypted-backup password lost = backup unusable (no recovery)
- Finder Update = keep data; Restore = erase + reinstall
- iCloud Photos data isn't inside iCloud Backup — it re-syncs after restore

---

## Sunday Jul 12 — REST

No study. Full rest day.

---

## Monday Jul 13 — Update + Reset Options, Cellular + Hotspot

**Hours:** 2h

### Topics
- **Software updates (iOS/iPadOS):** Settings → General → Software Update; automatic updates; Rapid Security Responses; beta enrollment; MDM can defer updates (org context)
- **Reset menu** (Settings → General → Transfer or Reset): Reset All Settings (keeps data), Reset Network Settings, Reset Keyboard Dictionary, Reset Home Screen, Reset Location & Privacy — vs **Erase All Content and Settings** (full wipe, triggers Activation Lock check)
- **Cellular:** physical SIM vs eSIM; eSIM transfer between iPhones; Dual SIM; carrier settings updates; cellular data options (roaming, 5G modes); APN via profile in managed environments
- **Personal Hotspot:** requirements (carrier plan support), connection methods (Wi-Fi, Bluetooth, USB), Instant Hotspot via Continuity, common failures: carrier plan, iCloud account mismatch for Instant Hotspot, Wi-Fi radio state

### Self-Check
1. A user's iPhone has flaky Wi-Fi on every network. Data must be preserved. Which reset is appropriate?
2. Reset All Settings vs Erase All Content and Settings — what survives each?
3. A user gets a new iPhone and wants their number moved without a physical SIM. What feature does this?
4. Personal Hotspot doesn't appear in Settings at all. Most likely cause?
5. What is a carrier settings update and when does it prompt?

### Anki Cards to Build
- Reset Network Settings: wipes Wi-Fi passwords, VPN, cellular prefs — keeps user data
- Reset All Settings: all settings reset, no data loss; Erase All Content = full wipe
- eSIM: digital SIM, transferable device-to-device, multiple profiles storable
- Hotspot missing from Settings = carrier plan doesn't include it
- Instant Hotspot: Continuity feature — same Apple Account on both devices

---

## Tuesday Jul 14 — Device Security: Activation Lock, Stolen Device Protection, Lost Mode, Lockdown Mode

**Hours:** 1h

### Topics
- **Activation Lock:** enabled automatically with Find My; ties device to Apple Account; survives erase — blocks reactivation without owner credentials; org devices: MDM/ABM can clear Activation Lock (supervised) — support scenario: second-hand device locked to previous owner
- **Stolen Device Protection (iPhone):** when away from familiar locations, sensitive actions require biometrics with no passcode fallback + security delay for critical changes (e.g., Apple Account password change)
- **Managed Lost Mode:** MDM marks supervised device lost — locks, shows message/phone, can report location; user-level Lost Mode via Find My for personal devices
- **Lockdown Mode:** extreme optional protection against targeted spyware — blocks most attachments, link previews, some web tech; support impact: users may report "broken" features that Lockdown Mode intentionally disables

### Self-Check
1. A user buys a second-hand iPhone that asks for the previous owner's Apple Account after erase. What feature is this and what are the resolution paths?
2. Stolen Device Protection is on. A thief knows the passcode and tries to change the Apple Account password at a coffee shop. What stops them?
3. What's the difference between Lost Mode (Find My) and Managed Lost Mode (MDM)?
4. A user in Lockdown Mode reports image attachments not arriving in Messages. Fault or feature?

### Anki Cards to Build
- Activation Lock: auto with Find My; survives erase; cleared by owner sign-out, proof-of-purchase via Apple, or MDM (supervised)
- Stolen Device Protection: biometric-only + security delay away from familiar locations
- Managed Lost Mode: MDM, supervised — lock + message + location
- Lockdown Mode: optional extreme protection — intentionally degrades features

---

## Wednesday Jul 15 — Sysdiagnose + iOS Troubleshooting Flow

**Hours:** 0.5h | Office

1. Topics: sysdiagnose capture on iOS (button chord → Analytics Data), when support asks for it; general iOS troubleshooting order: restart → update → settings reset → restore
2. 3 questions:
   - What's the button combination to trigger a sysdiagnose on iPhone, and where does the file land?
   - A user's iPhone app crashes repeatedly. Order the escalation steps from least to most invasive.
   - Which reset would you try before a full erase for persistent cellular issues?

---

## Thursday Jul 16 — Review + Self-Check

**Hours:** 0.5h | Office

1. Revisit gaps + course Check Your Understanding questions for iPhone/iPad modules
2. 3 questions:
   - Encrypted vs unencrypted backup: a user switching iPhones wants Health data moved via local backup. What must they do?
   - An employee leaves and their supervised iPad is Activation Locked to their personal Apple Account. What can IT do?
   - eSIM transfer fails during Quick Start. What are two manual fallback paths?

---

## Friday Jul 17 — Anki Review

**Hours:** 0.5h | Office — Anki only

1. Full review: Apple Weeks 1–2 decks
2. Flag weak areas — likely: reset-option distinctions, Activation Lock resolution paths
3. Confirm labs done: backup inspection, hotspot test, sysdiagnose capture
