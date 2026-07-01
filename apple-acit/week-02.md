# Week 2 — Apple SUP-2026: Recovery Modes + macOS Reinstall

**Dates:** Jul 11 – Jul 17, 2026
**Target hours:** ~4.5h (30% of weekly target)
**Focus:** Recovery modes, Safe Mode, DFU, macOS reinstall via Recovery
**Cert:** Apple Certified Support Professional — Device Support (SUP-2026)

---

## Daily Session Template

1. **Review** (5 min) — Anki cards from Week 1
2. **Read** (varies) — Apple training material or topic notes
3. **Self-check** (10 min) — 3–4 exam-style questions
4. **Anki update** (5 min) — add cards for today's key facts

---

## Hands-On Lab (real device, this week)

On the 2nd MacBook Pro (spare unit, not Newton):

1. Boot into macOS Recovery for real (hold Power → Startup Options → Options → Continue) — explore Disk Utility, Startup Security Utility, Firmware Password Utility. Don't erase or reinstall — just navigate and confirm what each screen shows, then restart normally.
2. Boot into Safe Mode (Startup Options → hold Shift → Continue in Safe Mode) — confirm it boots, note the login screen says "Safe Boot" in the top corner, then restart normally.
3. Optional/only if comfortable: try `nvram boot-args=-v` from Terminal (in a normal boot) then reboot to see verbose-style output on Apple Silicon — revert with `nvram -d boot-args` afterward.
4. Skip DFU mode hands-on this week unless you have a second Mac + cable free to be the "host" — DFU requires Apple Configurator 2 on another machine and is higher-risk to practice casually. Fine to leave as exam-knowledge only for now.

---

## Saturday Jul 11 — macOS Recovery Modes

**Hours:** 1.5h

### Topics
- **macOS Recovery (Apple Silicon):**
  - Hold Power button → Startup Options → Options → Continue
  - Contains: Reinstall macOS, Disk Utility, Safari, Terminal, Startup Security Utility, Firmware Password Utility
  - Recovery OS stored on internal storage (not internet-dependent for local recovery)
- **macOS Recovery (Intel):**
  - Cmd+R: local Recovery (same version of macOS as installed)
  - Cmd+Opt+R: Internet Recovery → latest compatible macOS
  - Shift+Cmd+Opt+R: Internet Recovery → version shipped with Mac or closest available
- **Recovery Partition:** hidden APFS volume on internal SSD — not visible in Finder or Disk Utility
- **Startup Security Utility (in Recovery):** Full Security / Reduced Security / Permissive Security; controls allowed boot OS
- **Reinstall macOS from Recovery:** preserves user data and apps when reinstalling over existing installation

### Self-Check
1. A user's macOS installation is corrupted. They want to reinstall macOS without losing user data. What tool and option do they use?
2. On Intel: Cmd+Opt+R vs Shift+Cmd+Opt+R — what is the difference in macOS version installed?
3. Startup Security Utility is located where? How is it accessed?
4. A MacBook Pro M2's recovery partition is deleted. Can the user still boot to Recovery?
5. After using Recovery to reinstall macOS, what happens to user documents and applications?

### Anki Cards to Build
- Apple Silicon Recovery: hold Power → Startup Options → Options
- Intel Recovery shortcuts: Cmd+R (local), Cmd+Opt+R (latest compatible), Shift+Cmd+Opt+R (shipped version)
- Recovery partition: hidden APFS volume — always present unless manually deleted
- Startup Security Utility: Full (default, requires Apple-signed OS) / Reduced / Permissive
- macOS reinstall from Recovery: preserves user data if installed over existing macOS

---

## Sunday Jul 12 — REST

No study. Full rest day.

---

## Monday Jul 13 — Safe Mode + Verbose Boot + Single User Mode

**Hours:** 1.5h

### Topics
- **Safe Mode (Intel):** hold Shift at startup → loads minimal kernel extensions, runs fsck, clears font/kernel caches
- **Safe Mode (Apple Silicon):** Startup Options → hold Shift → click Continue in Safe Mode
- **What Safe Boot does:** disables third-party kexts, login items, startup items; checks startup disk with fsck
- **Verbose mode (Intel):** Cmd+V at startup — shows boot log in real time; useful for diagnosing boot failures
- **Verbose mode (Apple Silicon):** not available the same way; use Recovery Terminal or `nvram boot-args=-v`
- **Single User Mode (Intel only):** Cmd+S → drops to BSD shell with root access; not available on Apple Silicon
- **Diagnosing with Safe Mode:** if issue disappears in Safe Mode → likely third-party kext or login item

### Self-Check
1. A user reports the Mac is slow and crashes after login. You boot into Safe Mode and the issue does not occur. What is the most likely cause?
2. How is Safe Mode initiated on an Apple Silicon Mac (step by step)?
3. Single User Mode is available on Apple Silicon: True or False?
4. Safe Mode runs `fsck` on the startup disk. What does `fsck` do?
5. Verbose mode on Intel: what information does it display that normal startup hides?

### Anki Cards to Build
- Safe Mode Intel: hold Shift at startup
- Safe Mode Apple Silicon: hold Shift in Startup Options
- Safe Mode: disables third-party kexts + login items + startup items; runs fsck
- Single User Mode (Cmd+S): Intel only — not on Apple Silicon
- Verbose mode: Cmd+V (Intel) / nvram boot-args=-v (Apple Silicon workaround)

---

## Tuesday Jul 14 — DFU Mode + macOS Reinstall via Internet Recovery

**Hours:** 1h

### Topics
- **DFU (Device Firmware Update) mode:**
  - Apple Silicon: hold Power + Volume Down → continue holding → Apple logo appears → release Volume Down (keep holding Power)
  - Intel: specific key sequence per model; used when normal recovery is inaccessible
  - DFU restores firmware/OS via Apple Configurator 2 (connected Mac required)
  - Use case: Mac with corrupted firmware, stuck bootloop, or unresponsive to Recovery
- **Internet Recovery:**
  - Loads Recovery OS from Apple servers — fallback when local Recovery partition is missing or damaged
  - Requires Wi-Fi or Ethernet; may take 10–30 min to load
  - Intel: Cmd+Opt+R (latest compatible) or Shift+Cmd+Opt+R (shipped version)
  - Apple Silicon: if local Recovery is unavailable, Startup Options automatically falls back to Internet Recovery
- **Reinstall macOS (clean install):** Disk Utility → Erase volume → then Reinstall macOS — removes all data
- **Reinstall macOS (upgrade/repair):** Reinstall without erasing — preserves user data

### Self-Check
1. A MacBook Pro M3 has a corrupted firmware and won't respond to holding Power at startup. What recovery method requires another Mac and a cable?
2. Internet Recovery on Intel: what is the difference in macOS version between Cmd+Opt+R and Shift+Cmd+Opt+R?
3. To perform a clean erase-and-reinstall via Recovery, what is the correct order of steps?
4. DFU mode on Apple Silicon requires which external tool to restore?
5. An Apple Silicon Mac's local Recovery partition is missing. A user boots it. What happens?

### Anki Cards to Build
- DFU mode: firmware-level restore via Apple Configurator 2 — requires another Mac
- DFU use case: corrupted firmware / unresponsive to Recovery
- Internet Recovery fallback: automatic on Apple Silicon if local Recovery missing
- Clean reinstall: Disk Utility Erase → then Reinstall macOS
- Repair reinstall: Reinstall macOS without erasing (preserves user data)

---

## Wednesday Jul 15 — Review + Self-Check

**Hours:** 0.5h | Office

### Tasks
1. Anki review: all Apple Week 2 cards built so far
2. 3 mixed questions:
   - A user can't get past the Apple logo on an Intel Mac. List 3 things you would try in order.
   - On Apple Silicon, what is the equivalent of Intel's Cmd+R (local Recovery)?
   - When would you use DFU mode instead of Recovery mode?

---

## Thursday Jul 16 — Review + Self-Check

**Hours:** 0.5h | Office

### Tasks
1. Revisit gaps from Wed
2. 3 more questions:
   - After booting into Safe Mode and the issue disappears, what is your next diagnostic step?
   - macOS Recovery → Terminal. A user erased the wrong volume. Is recovery possible?
   - Explain the Startup Security Utility settings (Full / Reduced / Permissive) and when each applies.

---

## Friday Jul 17 — Anki Review

**Hours:** 0.5h | Office — Anki only

### Tasks
1. Full review of Apple Week 1 + Week 2 Anki decks
2. Flag weak areas (likely: DFU steps, Intel vs Apple Silicon recovery key differences)
3. Note topics to carry into Week 3 catchup if needed
