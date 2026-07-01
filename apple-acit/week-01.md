# Week 1 — Apple SUP-2026: Hardware & Fundamentals

**Dates:** Jul 4 – Jul 10, 2026
**Target hours:** ~4.5h (30% of weekly target)
**Focus:** Mac hardware identification, ports, Apple Silicon vs Intel, startup keys
**Cert:** Apple Certified Support Professional — Device Support (SUP-2026)

**Lab device:** 2nd Apple Silicon MacBook Pro (not Newton — that's the production AI server, don't reboot/recover/DFU it). Both available test units are Apple Silicon — Intel-only content below (T2 chip, Boot Camp, Cmd+S) has no hands-on unit; treat as exam-knowledge only.

---

## Daily Session Template

1. **Read** (varies) — Apple training material or topic notes
2. **Self-check** (10 min) — 3–4 exam-style questions
3. **Anki update** (5 min) — add cards for today's key facts

---

## Hands-On Lab (real device, this week)

1. On the 2nd MacBook Pro: System Information → Overview → confirm Chip (should read Apple M1 or M2)
2. Terminal: `system_profiler SPHardwareDataType` — cross-check Model Identifier + serial format against notes
3. Physically inspect the ports on the device — count/identify Thunderbolt vs USB-C vs any other connector, then confirm against System Information → the exact Thunderbolt/USB spec it reports
4. Non-destructively: hold Power to reach Startup Options — observe the screen, do NOT select anything that erases or reinstalls — just confirm what's on it (Continue, Options, shutdown), then power off normally

---

## Saturday Jul 4 — Apple Silicon vs Intel + Hardware Identification

**Hours:** 1.5h

### Topics
- Apple Silicon (M-series): unified memory architecture, Neural Engine, no Boot Camp, Rosetta 2 for x86 app translation
- Intel Mac: separate CPU/GPU, Boot Camp supported, EFI firmware, T2 chip (2018+ models)
- Identifying Mac model: System Information app, `system_profiler SPHardwareDataType`, About This Mac → More Info
- Mac hardware identifiers: Model Identifier (e.g. MacBookPro18,1), Serial number (post-2021: randomised 10-char)
- T2 chip (Intel): secure boot controller + encrypted storage controller + Touch ID processor
- Secure Enclave (Apple Silicon): built into SoC, replaces T2 for key storage and Touch ID/Face ID

### Resources
- [Apple Training: macOS Support Essentials](https://training.apple.com/uk/en/mac-support-essentials)
- [About Apple Silicon](https://support.apple.com/en-gb/102869)

### Self-Check
1. A user has a 2020 Intel MacBook Pro with a T2 chip. What two storage-related security functions does the T2 provide?
2. Rosetta 2 translates apps built for **\_\_\_\_\_** architecture to run on Apple Silicon.
3. Which command-line tool shows Model Identifier and serial number?
4. Boot Camp is supported on: Apple Silicon / Intel / both?
5. An M2 MacBook Air has a T2 chip: True or False?

### Anki Cards to Build
- Apple Silicon: no Boot Camp, Rosetta 2, unified memory, no T2 chip (Secure Enclave built into SoC)
- T2 chip: Intel only — secure boot + encrypted storage + Touch ID controller
- `system_profiler SPHardwareDataType` → hardware ID from Terminal
- Model Identifier format: ProductName + generation number (e.g. MacBookPro18,1)

---

## Sunday Jul 5 — REST

No study. Full rest day.

---

## Monday Jul 6 — Ports, Connectors + Peripheral Support

**Hours:** 1.5h

### Topics
- Thunderbolt 3 vs Thunderbolt 4 vs USB 4: all use USB-C connector; Thunderbolt 4 = 40Gbps, mandatory minimum specs
- MagSafe 3: MacBook Pro 14"/16" and MacBook Air M2+; charging only, no data
- SDXC slot: MacBook Pro 14" and 16" (M2+); supports UHS-II cards
- HDMI 2.1: MacBook Pro 14" and 16" (2021+); supports 8K output
- MacBook Air M2/M3: 2× Thunderbolt / USB 4 ports, no HDMI, no SD slot, no MagSafe on earlier models
- Mac mini M4: 3× Thunderbolt 4, 2× USB-A, 1× HDMI, ethernet, SD slot
- System Information: find exact Thunderbolt/USB spec, Wi-Fi standard, Bluetooth version per model

### Self-Check
1. A user connects a Thunderbolt 4 external SSD to a MacBook Air M2. The port is USB-C. Will it work at Thunderbolt 4 speeds?
2. MagSafe 3 supports data transfer: True or False?
3. A user needs to read a UHS-II SD card at full speed using a built-in slot. Which MacBook model supports this?
4. Where in macOS does a technician verify the exact Wi-Fi standard (Wi-Fi 6E) for a given Mac?

### Anki Cards to Build
- MacBook Air M2/M3: 2× USB-C/TB4, no HDMI, no SD slot
- MacBook Pro 14/16: HDMI 2.1 + SDXC UHS-II + MagSafe 3 + 3× Thunderbolt 4
- MagSafe 3: charging only
- Thunderbolt 4 = 40Gbps; USB 4 = 40Gbps (but Thunderbolt spec is stricter minimum)

---

## Tuesday Jul 7 — Startup Keys + Boot Options

**Hours:** 1h

### Topics
- **Apple Silicon startup:** hold Power button → Startup Options screen (no keyboard shortcuts at power-on)
- **Intel Mac startup keys:**
  - Option (⌥): Startup Manager — choose boot volume
  - Cmd+R: macOS Recovery (local)
  - Cmd+Opt+R: Internet Recovery — latest compatible macOS
  - Shift: Safe Boot — loads minimal kernel extensions
  - Cmd+S: Single User Mode (Intel only — removed in Apple Silicon)
  - D or Opt+D: Apple Diagnostics
  - T: Target Disk Mode (Intel) — expose internal drive as external volume
- **Apple Silicon startup differences:** Safe Boot = hold Shift after clicking Continue in Startup Options; Target Disk Mode = share disk via Finder / System Information

### Self-Check
1. On an Apple Silicon Mac, how does a user access Startup Options?
2. An Intel Mac user holds Cmd+R at startup. What environment loads?
3. Cmd+Opt+R on an Intel Mac: which macOS version does Internet Recovery install?
4. Is Single User Mode (Cmd+S) available on Apple Silicon?
5. Which startup key on Intel loads the minimal safe environment by bypassing non-essential kexts?

### Anki Cards to Build
- Apple Silicon: hold Power → Startup Options (no Cmd+R etc.)
- Cmd+R = local Recovery | Cmd+Opt+R = Internet Recovery (latest compatible)
- Option key = Startup Manager (boot volume picker) — Intel only shortcut
- Safe Boot: Shift (Intel) / hold Shift in Startup Options (Apple Silicon)
- Single User Mode (Cmd+S): Intel only — not available on Apple Silicon

---

## Wednesday Jul 8 — Review + Self-Check

**Hours:** 0.5h | Office

### Tasks
1. Anki review: all Apple Week 1 cards built so far
2. 3 mixed questions:
   - What is the difference between the T2 chip and Secure Enclave on Apple Silicon?
   - An M3 MacBook Pro is frozen. How does a technician force a restart when keyboard shortcuts are unresponsive?
   - A user wants to boot an Apple Silicon Mac from an external USB drive. Walk through the steps.

---

## Thursday Jul 9 — Review + Self-Check

**Hours:** 0.5h | Office

### Tasks
1. Revisit gaps from Wed — topic notes or Apple support articles
2. 3 more questions:
   - Name 2 hardware differences between MacBook Pro 14" M3 and MacBook Air M3.
   - An Intel Mac has a firmware password set. The user forgets it. How is it reset?
   - Which tool identifies a Mac's exact Model Identifier from within macOS?

---

## Friday Jul 10 — Anki Review

**Hours:** 0.5h | Office — Anki only

### Tasks
1. Full review of Apple Week 1 Anki deck
2. Flag weak areas — likely: Intel vs Apple Silicon startup differences
3. Note anything to carry into Week 2 or Week 3 catchup
