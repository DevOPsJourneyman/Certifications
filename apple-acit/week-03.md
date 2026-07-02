# Week 3 — Apple SUP-2026: Mac Support — Accounts, FileVault, Startup + Recovery, Diagnostics

**Dates:** Jul 18 – Jul 24, 2026
**Target hours:** ~7h
**Focus:** Mac user account types, password reset, FileVault, startup modes, macOS Recovery, erase/reinstall, Apple Diagnostics, Activity Monitor, Console, sysdiagnose
**Cert:** Apple Certified Support Professional — Apple Device Support Exam (SUP-2026)
**Course modules:** Setup, Backup, and Recovery for Mac; Managing User Accounts and FileVault; Diagnosing Issues on Mac

> This week condenses the old plan's Mac recovery/diagnostics/FileVault weeks — that content was on-syllabus, it just can't take 3 weeks anymore. No skip rule; there is no slack in the v2 schedule.

**Lab device:** spare Apple Silicon MacBook Pro. This is the week to use it hard — Recovery, Safe Mode, Diagnostics, FileVault are all safe on a spare unit.

---

## Daily Session Template

1. **Review** (5 min) — Anki cards from Weeks 1–2
2. **Read** (varies) — course article(s) + Check Your Understanding questions
3. **Self-check** (10 min) — 3–4 exam-style questions
4. **Anki update** (5 min) — add cards for today's key facts

---

## Hands-On Lab (spare MacBook Pro, this week)

1. **Recovery:** hold Power → Startup Options → Options → Continue. Explore Disk Utility, Startup Security Utility, Terminal, Reinstall option. Don't erase. Restart normally.
2. **Safe Mode:** Startup Options → hold Shift → Continue in Safe Mode. Confirm "Safe Boot" at login, restart normally.
3. **Apple Diagnostics:** power off → hold D at power-on. Record result/reference code and duration.
4. **FileVault:** System Settings → Privacy & Security → FileVault → Turn On. Save the personal recovery key durably (not on the device). Then `fdesetup status` and `fdesetup list` in Terminal — watch progress, confirm enabled users.
5. **Accounts:** create a second local Standard account. Confirm it can't unlock FileVault at boot until an enabled user has unlocked. Note the difference in System Settings → Users & Groups between Admin and Standard.
6. **Console + Activity Monitor:** open Console → find one crash report, identify process + timestamp. Activity Monitor → sort by CPU and Memory; note memory pressure graph.
7. **sysdiagnose on Mac:** `sudo sysdiagnose` in Terminal → note output path (/private/var/tmp/).

---

## Saturday Jul 18 — User Accounts, Password Reset, FileVault

**Hours:** 2.5h

### Topics
- **Account types on Mac:** Administrator, Standard, Sharing Only, Guest, root (disabled by default) — what each can/can't do
- **Login password reset paths:**
  1. Admin resets another user in Users & Groups
  2. Apple Account reset at login (if enabled)
  3. FileVault personal recovery key at login
  4. Recovery mode `resetpassword` utility
  - FileVault caveat: resetting a password by force can orphan the login keychain — user's saved passwords lost; new login keychain created
- **FileVault:** XTS-AES encryption; Apple Silicon storage always hardware-encrypted — FileVault adds pre-boot authentication; multi-user unlock (enabled users only); personal vs institutional recovery keys; MDM escrow
- `fdesetup status` / `fdesetup list` / `diskutil apfs list`
- Lost password + lost recovery key = data unrecoverable

### Self-Check
1. Rank the four password-reset paths a support tech should try for a locked-out FileVault Mac user.
2. After a forced password reset the user complains Safari lost all saved passwords. Why?
3. A Standard user tries to install software system-wide. What happens?
4. Three accounts on a FileVault Mac; one is unlock-enabled. Another user powers it on. What do they see?
5. What must an org configure before FileVault enablement to guarantee remote decryption capability?

### Anki Cards to Build
- Account types: Admin / Standard / Sharing Only / Guest / root (off by default)
- Password reset paths: admin reset → Apple Account → FileVault recovery key → Recovery `resetpassword`
- Forced reset breaks login keychain — saved passwords lost, new keychain created
- FileVault on Apple Silicon = pre-boot auth layer on always-encrypted storage
- MDM escrow must be configured BEFORE FileVault enable

---

## Sunday Jul 19 — REST

No study. Full rest day.

---

## Monday Jul 20 — Startup, Recovery, Erase + Reinstall

**Hours:** 2h

### Topics
- **Apple Silicon startup:** hold Power → Startup Options; Safe Mode via Shift; fallback to Internet Recovery if local Recovery damaged; DFU restore via another Mac + Apple Configurator (firmware-level, last resort)
- **Intel differences (exam-knowledge only):** Cmd+R local Recovery, Cmd+Opt+R Internet Recovery, Shift Safe Boot, D Diagnostics
- **Startup Security Utility:** Full / Reduced / Permissive security; external boot policy
- **macOS Recovery contents:** Reinstall macOS, Disk Utility (First Aid, erase), Terminal, Startup Security Utility, Share Disk
- **Erase options:** Erase All Content and Settings (System Settings — Apple Silicon; fast, keeps macOS, wipes data/settings via Erase Assistant) vs full Disk Utility erase + reinstall
- **Reinstall over existing macOS:** preserves user data + apps
- **macOS Recovery logs:** available in Recovery for diagnosing failed recovery/reinstall
- Redeployment context: Erase Assistant is the standard path to return a Mac to out-of-box for the next user

### Self-Check
1. Erase All Content and Settings vs Disk Utility erase + reinstall: when is each the right tool, and which is faster?
2. An Apple Silicon Mac's local Recovery won't load. What happens next, automatically?
3. A Mac must be handed to a new employee today. Which erase path and why?
4. Reinstalling macOS from Recovery over the existing installation — what happens to user data?
5. What tool and second device does a firmware-level (DFU) restore require?

### Anki Cards to Build
- Erase All Content and Settings: Apple Silicon Erase Assistant — fast wipe, macOS stays, ideal for redeployment
- Local Recovery damaged → automatic Internet Recovery fallback (Apple Silicon)
- Reinstall over existing macOS = user data + apps preserved
- DFU restore: Apple Configurator + second Mac + cable — firmware level
- Startup Security Utility: Full (default) / Reduced / Permissive

---

## Tuesday Jul 21 — Diagnostics: Apple Diagnostics, Activity Monitor, Console, System Information, sysdiagnose

**Hours:** 1h

### Topics
- **Apple Diagnostics:** hold D at power-on (Apple Silicon); tests logic board, RAM, storage controller, GPU, battery, wireless; reference codes (NDD storage, VFD video, PPT power, MEM memory); clean result ≠ no hardware fault; run twice to confirm, escalate to Apple with code
- **Activity Monitor:** CPU/Memory/Energy/Disk/Network tabs; memory pressure (green/yellow/red) beats "memory used" as health signal; identifying runaway processes; Force Quit
- **Console:** live log stream + Crash Reports; `.crash` app crash, `.spin` hang, `.panic` kernel panic; crash paths `~/Library/Logs/DiagnosticReports/` (user) and `/Library/Logs/DiagnosticReports/` (system); kernel panic from third-party kext → Safe Mode isolates
- **System Information:** hardware/software/network inventory — first stop for spec questions
- **sysdiagnose (Mac):** `sudo sysdiagnose` → comprehensive log bundle for AppleCare/engineering escalation

### Self-Check
1. Memory pressure is red but "memory used" seems normal to the user. What do you tell them, and what's the fix path?
2. Issue disappears in Safe Mode. What's the diagnostic conclusion and next step?
3. `.spin` file vs `.crash` file — what user complaint matches each?
4. Apple Diagnostics returns no issues; user still gets random shutdowns. Next steps?
5. AppleCare asks for a sysdiagnose. Command and output location?

### Anki Cards to Build
- Memory pressure colour > memory-used number — red = RAM genuinely short
- Safe Mode success → third-party kext/login item suspect → remove/re-add
- `.crash` = crash | `.spin` = hang | `.panic` = kernel panic
- Diagnostics codes: NDD storage / VFD video / PPT power / MEM memory
- `sudo sysdiagnose` → /private/var/tmp/

---

## Wednesday Jul 22 — Review + Self-Check

**Hours:** 0.5h | Office

1. Anki review: Week 3 cards
2. 3 questions:
   - Kernel panic on every boot except Safe Mode. Diagnosis walk-through?
   - A departing user's FileVault Mac needs redeploying and IT has no recovery key escrowed. Options?
   - Startup Options screen won't appear at all on an Apple Silicon Mac. Escalation path?

---

## Thursday Jul 23 — Review + Self-Check

**Hours:** 0.5h | Office

1. Revisit gaps + course Check Your Understanding questions for Mac modules
2. 3 questions:
   - User forgot login password; FileVault on; Apple Account reset not enabled. Which path recovers access, and what's the keychain side effect?
   - Which Activity Monitor tab and metric identify an app draining a MacBook battery?
   - When would you choose DFU restore over Recovery reinstall?

---

## Friday Jul 24 — Anki Review

**Hours:** 0.5h | Office — Anki only

1. Full review: Apple Weeks 1–3 decks
2. Flag weak areas — likely: password-reset path order, erase-option selection
3. Confirm labs done: Recovery walk, Safe Mode, Diagnostics run, FileVault enabled + fdesetup, second account, Console/Activity Monitor, sysdiagnose
