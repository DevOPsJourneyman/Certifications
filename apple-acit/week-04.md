# Week 4 — Apple SUP-2026: FileVault Deep-Dive + T2 / Secure Enclave + Apple Diagnostics

**Dates:** Jul 25 – Jul 31, 2026
**Target hours:** ~4.5h (30% of weekly target)
**Focus:** FileVault management, T2 chip security model, Secure Enclave, Apple Diagnostics reference codes
**Cert:** Apple Certified Support Professional — Device Support (SUP-2026)

---

## Daily Session Template

1. **Review** (5 min) — Anki cards from Weeks 1–3
2. **Read** (varies) — Apple training material or topic notes
3. **Self-check** (10 min) — 3–4 exam-style questions
4. **Anki update** (5 min) — new cards for today

---

## Saturday Jul 25 — FileVault Management + Recovery Key Escrow

**Hours:** 1.5h

### Topics
- **Enabling FileVault:**
  - System Settings → Privacy & Security → FileVault → Turn On
  - Multi-user Mac: each user who should unlock FileVault must be enabled individually (Enable User button)
  - Users not enabled: cannot unlock at startup — must wait for an enabled user to log in first
- **Recovery key options:**
  - **Personal recovery key:** 24-character alphanumeric key; user must store it securely
  - **Institutional recovery key:** created via `security` command or Apple Configurator; stored by admin; allows org to decrypt any device
  - **MDM escrow:** MDM solution can receive and store the personal recovery key automatically at enablement
- **FileVault status commands:**
  - `fdesetup status` → shows FileVault on/off + encryption progress
  - `fdesetup list` → lists all users enabled to unlock FileVault
  - `fdesetup remove -user username` → removes a user's ability to unlock
  - `diskutil apfs list` → shows APFS volumes and encryption state
- **Disabling FileVault:**
  - System Settings → FileVault → Turn Off
  - Decryption happens in background — fully usable during decryption
  - Requires admin authentication to disable
- **FileVault and MDM:**
  - MDM (Jamf, Intune) can: enforce FileVault, escrow recovery key, defer enabling until user logs in, generate new recovery key
  - Intune: endpoint protection profile → FileVault → require encryption, escrow key to Intune

### Self-Check
1. A Mac has 3 user accounts. FileVault is enabled and only 1 user is set to unlock the disk. A second user turns on the Mac. What happens at startup?
2. An admin wants to decrypt a managed Mac remotely without user involvement. What must have been set up before encryption?
3. `fdesetup list` outputs only one username. What does this tell you?
4. A user enabled FileVault and stored the personal recovery key. They have since forgotten their login password and lost the recovery key. What are the options?
5. How do you check FileVault encryption progress from Terminal?

### Anki Cards to Build
- Multi-user FileVault: only enabled users can unlock at startup — others must wait
- `fdesetup status` → on/off + progress | `fdesetup list` → unlock-capable users
- MDM escrow: MDM receives recovery key at enable time — must be configured BEFORE enable
- Disabled FileVault user: cannot unlock disk, but can still log in after an enabled user has unlocked
- Lost password + lost recovery key = data unrecoverable

---

## Sunday Jul 26 — REST

No study. Full rest day.

---

## Monday Jul 27 — T2 Chip Security Model + Secure Enclave

**Hours:** 1.5h

### Topics
- **T2 chip (Intel Macs, 2018+):**
  - Combines: secure boot controller, encrypted storage controller, Touch ID security, System Management Controller (SMC), audio controller
  - Secure Boot: ensures only Apple-signed macOS boots; three levels: Full (default), Reduced, Permissive
  - Full Security: only current signed macOS version allowed; requires internet to verify at every boot (or cached certificate)
  - Reduced Security: allows older or non-Apple-signed OS kernels
  - Permissive Security: allows any software — required for booting Linux or certain third-party kexts
  - USB/Thunderbolt boot restriction: by default, T2 Macs cannot boot from external volumes; enable in Startup Security Utility
  - Firmware password: stored in T2; prevents boot from external media, Recovery Mode, and target disk mode without password
  - Reset firmware password on T2 Mac: requires Apple service (service provider or Apple Store) — cannot be reset by user or MDM
- **Secure Enclave (Apple Silicon and iPhone/iPad):**
  - Dedicated coprocessor within the SoC (not a separate chip like T2)
  - Stores: cryptographic keys for Touch ID/Face ID, Apple Pay, Keychain encryption
  - Processes biometric data locally — biometric template never sent to Apple or apps
  - Keys in Secure Enclave: cannot be extracted even with physical access to the device
  - Data Protection: ties file encryption keys to Secure Enclave keys — no Secure Enclave = no data access
- **T2 vs Secure Enclave comparison:**
  - T2: separate chip (Intel Macs), handles encrypted storage + secure boot + SMC + audio
  - Secure Enclave: coprocessor within SoC (Apple Silicon), handles key storage + biometrics only
  - Apple Silicon: no T2 chip — Secure Enclave is built into M-series SoC; storage always encrypted via hardware

### Self-Check
1. A T2 Mac has Full Security enabled. An admin wants to boot from an external USB drive. What two changes must be made in Startup Security Utility?
2. A user forgets the firmware password on their T2 MacBook Pro. The user cannot contact Apple. Can the firmware password be reset without Apple service?
3. What is the primary difference between T2 chip and Secure Enclave in terms of their physical implementation?
4. An M2 Mac has no T2 chip. How is the internal SSD encrypted?
5. Secure Enclave stores biometric data (fingerprint image). Can this image be extracted by a forensic tool?

### Anki Cards to Build
- T2 chip: Intel only — secure boot + encrypted storage + SMC + Touch ID + audio controller
- T2 Secure Boot levels: Full (default) / Reduced / Permissive
- T2 firmware password: requires Apple service to reset — no user/MDM reset
- Secure Enclave: in-SoC coprocessor — key storage + biometrics (no storage controller)
- Apple Silicon: no T2; Secure Enclave in M-series SoC; storage always encrypted via hardware
- External boot on T2: disabled by default; enable in Startup Security Utility + change boot policy

---

## Tuesday Jul 28 — Apple Diagnostics Reference Codes

**Hours:** 1h

### Topics
- **Reference code format:** 3–4 letter prefix (component) + 3-digit number (specific fault)
- **Common component prefixes:**
  - `ADP` — general hardware
  - `NDR` or `NDD` — storage (NVMe/SSD)
  - `VFD` — video/graphics
  - `PPT` — power/battery
  - `MEM` — memory (RAM)
  - `WL` — Wi-Fi
  - `BT` — Bluetooth
  - `FAN` — fan/thermal
  - `PFM` — performance (may indicate thermal throttling)
- **No issues found:** Apple Diagnostics shows "No issues found" — system passes
- **After a positive code:**
  1. Note the exact code
  2. Run diagnostics again to confirm (transient errors can occur)
  3. Share code with Apple Genius or Apple Authorized Service Provider (AASP)
  4. Do NOT attempt to repair based on code alone — diagnostic code is a starting point, not a repair instruction
- **Limitations of Apple Diagnostics:**
  - Cannot diagnose all hardware faults (e.g. intermittent logic board issues may not trigger a code)
  - Does not diagnose software, third-party peripherals, or firmware
  - A clean result does NOT rule out hardware failure if symptoms persist

### Self-Check
1. Apple Diagnostics returns code `NDR001`. What component area does this indicate?
2. A user's MacBook Pro M3 runs diagnostics and gets "No issues found." The user still reports random restarts. Does "No issues found" rule out hardware failure?
3. An admin receives a reference code from a user. What is the recommended next step — repair the device or share the code with Apple service?
4. Apple Diagnostics detects a fault with the GPU. The code starts with which 3-letter prefix?
5. A MacBook Air runs Apple Diagnostics and returns a `WL` code. What hardware component is implicated?

### Anki Cards to Build
- `NDD`/`NDR` = storage (SSD/NVMe) | `VFD` = video | `PPT` = power/battery | `MEM` = memory
- `WL` = Wi-Fi | `BT` = Bluetooth | `FAN` = fan/thermal | `PFM` = performance/throttling
- "No issues found" ≠ hardware is definitely healthy — intermittent faults can evade diagnostics
- After positive code: note code → run again to confirm → share with Apple service

---

## Wednesday Jul 29 — Review + Self-Check

**Hours:** 0.5h | Office

### Tasks
1. Anki review: all Apple Week 4 cards
2. 3 questions:
   - FileVault is required by MDM policy. A user declines to enable it at login prompt. What happens on an MDM-enforced deployment?
   - A T2 Mac is set to Permissive Security. What risks does this introduce?
   - Apple Diagnostics returns `PPT001`. What component and what symptom might the user report?

---

## Thursday Jul 30 — Review + Self-Check

**Hours:** 0.5h | Office

### Tasks
1. Revisit gaps from Wed
2. 3 questions:
   - `fdesetup status` returns "FileVault is On. Encryption in progress: 45% complete." Can the user still use the Mac?
   - A forensic investigator obtains an M2 MacBook with no login credentials. The storage is removed. Can the data be decrypted on another machine?
   - What is the T2 chip feature that restricts booting from external volumes?

---

## Friday Jul 31 — Anki Review + SUP-2026 Progress Check

**Hours:** 0.5h | Office — Anki only

### Tasks
1. Full review: Apple Weeks 1–4 Anki decks
2. SUP-2026 coverage check so far:
   - ✅ Hardware ID, ports, Apple Silicon vs Intel (Week 1)
   - ✅ Startup keys, Recovery modes, Safe Mode, DFU, macOS reinstall (Week 2)
   - ✅ Apple Diagnostics overview, Console.app, crash reports, FileVault intro (Week 3)
   - ✅ FileVault management, T2/Secure Enclave, Diagnostics reference codes (Week 4)
   - 🔜 Week 5: DEP-2026 — ABM, ADE, supervision, MDM enrollment (new cert track)
3. Flag any SUP-2026 topics not yet covered — note for Week 5 or 6 review
