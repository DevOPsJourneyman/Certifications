# Week 3 — Apple SUP-2026: Diagnostics + FileVault Intro

**Dates:** Jul 18 – Jul 24, 2026
**Target hours:** ~4.5h (30% of weekly target)
**Focus:** Apple Diagnostics, Console.app crash reports, FileVault fundamentals
**Cert:** Apple Certified Support Professional — Device Support (SUP-2026)

---

## Skip Rule (mirrors MD-102 Week 3)

If Week 2 Apple self-checks scored well and hardware/recovery topics feel solid → treat this week as light review + new FileVault intro only. Full catchup is optional.

---

## Daily Session Template

1. **Review** (5 min) — Anki cards from Weeks 1–2
2. **Read** (varies) — Apple training material or topic notes
3. **Self-check** (10 min) — 3–4 exam-style questions
4. **Anki update** (5 min) — new cards for today

---

## Hands-On Lab (real device, this week)

On the 2nd MacBook Pro:

1. Run Apple Diagnostics for real — power off, then hold D at power-on. Record the actual result: "No issues found" or a reference code. Note how long it takes.
2. Open Console.app (`/Applications/Utilities/Console.app`). Browse "All Messages" and the "Crash Reports" section for a minute — find one real log entry and identify its process name and timestamp.
3. In Terminal, run `fdesetup status` — read-only check, note current FileVault state (likely off if this is a freshly-assigned device). Don't enable/disable yet — that's Week 4's lab.

---

## Saturday Jul 18 — Apple Diagnostics

**Hours:** 1.5h

### Topics
- **Apple Diagnostics (formerly Apple Hardware Test):**
  - Intel: hold **D** at startup (local) or **Opt+D** (internet version — downloads from Apple)
  - Apple Silicon: hold **D** during power-on → runs before Startup Options appear
  - What it tests: logic board, memory, storage controller, GPU, battery, Bluetooth, Wi-Fi
  - Does NOT test: third-party peripherals, software issues, Thunderbolt external devices
  - Output: reference code + progress bar; codes begin with letters (e.g. ADP, NDR, VFD, PPT)
- **Reading reference codes:**
  - Format: 3-letter prefix (component), rest = specific fault (e.g. `NDD001` = storage issue)
  - After diagnostics: option to contact Apple, see result code, or restart
  - Next step after positive code: run again to confirm; then escalate to Apple service
- **When to run:** user reports hardware failure symptoms (random crashes, no display, not charging, kernel panics) — rule out software first (Safe Mode), then run diagnostics

### Self-Check
1. How do you launch Apple Diagnostics on an Intel Mac? On an Apple Silicon Mac?
2. Apple Diagnostics reports a reference code starting with "NDD." What component area does this indicate?
3. A user's MacBook randomly powers off under load. You boot into Safe Mode — it runs fine. Should you run Apple Diagnostics? Why or why not?
4. Apple Diagnostics tests third-party Thunderbolt storage: True or False?
5. After Apple Diagnostics completes with no issues, but the user still reports problems. What is the next diagnostic step?

### Anki Cards to Build
- Apple Diagnostics Intel: hold D (local) / Opt+D (internet)
- Apple Diagnostics Apple Silicon: hold D at power-on (before Startup Options)
- Reference code format: 3-letter component prefix (NDD = storage, VFD = video, PPT = power)
- Diagnostics scope: logic board, RAM, storage controller, GPU, battery, BT, Wi-Fi — NOT third-party peripherals
- Positive result (no fault found) + ongoing issue → escalate to Console.app or service

---

## Sunday Jul 19 — REST

No study. Full rest day.

---

## Monday Jul 20 — Console.app + Crash Reports

**Hours:** 1.5h

### Topics
- **Console.app:** macOS log viewer — real-time log stream + historical logs
  - Location: /Applications/Utilities/Console.app
  - Key areas: All Messages, Crash Reports, Diagnostic Reports
  - Log levels: Default, Info, Debug (Debug not shown by default — enable via Action menu)
  - Filtering: search by process name, message content, subsystem, category
- **Crash reports:**
  - Location: `~/Library/Logs/DiagnosticReports/` (user crashes) + `/Library/Logs/DiagnosticReports/` (system)
  - File types: `.crash` (app crash), `.ips` (iOS/macOS structured crash), `.spin` (hang report)
  - Key fields in a crash report: Exception Type, Exception Codes, Crashed Thread, backtrace
  - Common exception types: `EXC_BAD_ACCESS` (memory error), `SIGABRT` (app called abort), `SIGKILL` (system killed process)
- **Kernel panics:**
  - Written to: `/Library/Logs/DiagnosticReports/` as `.panic` files
  - Also visible in: Console.app → Crash Reports section
  - Key info in panic log: which kext or process triggered, macOS build, hardware details
  - Common causes: third-party kext incompatibility, hardware fault, memory issue

### Self-Check
1. A user reports an app crashes every time they open a specific document. Where do you find the crash log for a user-space application?
2. What is the difference between a `.crash` file and a `.spin` file in DiagnosticReports?
3. A kernel panic log shows a third-party kext in the backtrace. What is the likely cause, and what is the first remediation step?
4. Console.app by default shows Debug log level: True or False?
5. Exception type `EXC_BAD_ACCESS` in a crash report indicates what category of error?

### Anki Cards to Build
- Crash logs path: `~/Library/Logs/DiagnosticReports/` (user) + `/Library/Logs/DiagnosticReports/` (system)
- `.crash` = app crash | `.spin` = hang (not responding) | `.panic` = kernel panic
- `EXC_BAD_ACCESS` = memory access violation (null pointer, buffer overrun)
- `SIGABRT` = app called abort() — usually assertion failure or uncaught exception
- Kernel panic cause: bad kext → boot to Safe Mode (no third-party kexts) to confirm

---

## Tuesday Jul 21 — FileVault Fundamentals

**Hours:** 1h

### Topics
- **FileVault:** macOS full-disk encryption
  - Encrypts the entire APFS volume using XTS-AES-128 encryption
  - Encryption happens at the storage layer — transparent to the user after unlock at login
  - Requires user to sign in at boot to unlock — no auto-login with FileVault enabled
- **FileVault + hardware:**
  - **Intel Mac with T2 chip:** T2 handles encryption natively (always encrypted at hardware level); FileVault adds pre-boot authentication requirement
  - **Apple Silicon:** storage is always encrypted (hardware, Secure Enclave); FileVault adds the pre-boot authentication layer only
  - **Intel Mac without T2:** software encryption via FileVault — slower, no hardware acceleration
- **Recovery keys:**
  - **Personal recovery key:** 24-character key generated at FileVault setup; user must save it
  - **Institutional recovery key:** set by admin via MDM or command line; allows org to decrypt any device
  - If both are lost: data is unrecoverable — Apple cannot bypass FileVault
- **Enabling FileVault:**
  - System Settings → Privacy & Security → FileVault → Turn On
  - Encryption happens in background — Mac fully usable during this process
  - First restart after enable: user must log in to begin encryption

### Self-Check
1. FileVault is enabled on an Intel Mac without a T2 chip. What handles the encryption — hardware or software?
2. A user enables FileVault and loses both their recovery key and forgets their login password. What happens to the data?
3. What encryption standard does FileVault use?
4. On an Apple Silicon Mac, is the storage encrypted even if FileVault is disabled?
5. An MDM admin wants to decrypt a managed Mac without knowing the user's password. What must have been configured before encryption?

### Anki Cards to Build
- FileVault encryption: XTS-AES-128
- Intel + T2: hardware encrypted always; FileVault adds pre-boot auth requirement only
- Apple Silicon: hardware encrypted always (Secure Enclave); FileVault = pre-boot auth layer
- Intel (no T2): software encryption via FileVault (slower)
- Lost recovery key + lost password = data unrecoverable (no Apple backdoor)
- Institutional recovery key: configured via MDM before FileVault enable

---

## Wednesday Jul 22 — Review + Self-Check

**Hours:** 0.5h | Office

### Tasks
1. Anki review: all Apple Week 3 cards + weak Week 1/2 cards
2. 3 questions:
   - A kernel panic happens every time a specific USB-C hub is connected. How would you diagnose this?
   - Apple Diagnostics shows code VFD001. What component area is affected?
   - FileVault is enabled on a Mac. The hard drive is physically removed and placed in another Mac. Can the data be read?

---

## Thursday Jul 23 — Review + Self-Check

**Hours:** 0.5h | Office

### Tasks
1. Revisit gaps from Wed
2. 3 questions:
   - Where in Console.app do you find kernel panic logs?
   - A user reports their M2 MacBook Air feels hot and the battery drains fast. No app crashes. What diagnostic tools would you use first?
   - Institutional recovery key vs personal recovery key: when would you use each?

---

## Friday Jul 24 — Anki Review

**Hours:** 0.5h | Office — Anki only

### Tasks
1. Full review: Apple Weeks 1–3 Anki decks
2. Flag: startup keys (Week 1), recovery modes (Week 2), diagnostic codes (Week 3)
3. Note: FileVault deep-dive continues in Week 4 — just intro today
