# Week 4 — Apple SUP-2026: Networking + Security + Updates/Storage/Continuity → Practice Exam Gate

**Dates:** Jul 25 – Jul 31, 2026
**Target hours:** ~7h
**Focus:** Network configuration + troubleshooting, Mac app security (Gatekeeper/notarization/XProtect/SIP), privacy + sharing, software updates, storage, Continuity — then the paid practice exam as the booking gate
**Cert:** Apple Certified Support Professional — Apple Device Support Exam (SUP-2026)
**Course modules:** Managing Network Connections; Managing Privacy, Security, and Sharing; Software Updates, Device Storage, and Continuity

**Lab device:** spare Apple Silicon MacBook Pro + iPhone if available.

---

## Daily Session Template

1. **Review** (5 min) — Anki cards from Weeks 1–3
2. **Read** (varies) — course article(s) + Check Your Understanding questions
3. **Self-check** (10 min) — 3–4 exam-style questions
4. **Anki update** (5 min) — add cards for today's key facts

---

## Hands-On Lab (this week)

1. System Settings → Network — identify interfaces and their order; create a new network **location**, switch to it, switch back (Apple menu → Location on older macOS; Network settings on Tahoe)
2. Set service order manually — drag Wi-Fi below Ethernet (or USB-adapter) and confirm which interface routes traffic (`route get default` in Terminal)
3. Terminal network tools: `ping`, `traceroute`, `dig apple.com`, `networkQuality` — run each, read output
4. Gatekeeper walk: download a non-App-Store app → note the first-open prompt; System Settings → Privacy & Security → "Open Anyway" flow; `spctl --status` read-only check
5. Privacy: System Settings → Privacy & Security — review Location Services, Camera, Mic, Full Disk Access lists; find which apps hold Full Disk Access
6. Storage: System Settings → General → Storage — read the category breakdown + recommendations; compare `df -h` in Terminal
7. Continuity: with iPhone nearby, test Handoff (Safari page) and Universal Clipboard between iPhone and Mac; note requirements (same Apple Account, Bluetooth, Wi-Fi on)

---

## Saturday Jul 25 — Networking on Apple Devices

**Hours:** 2.5h

### Topics
- **Network settings (macOS):** interfaces, service order (top active service wins for default route), locations (saved config sets), DHCP vs manual, DNS configuration
- **Wi-Fi troubleshooting flow:** signal/interference → correct SSID/password → IP address sanity (169.254.x.x = DHCP failure) → DNS test → captive portal awareness → forget/rejoin network
- **iOS network settings:** Wi-Fi private address (MAC randomisation — can break MAC-filtered corporate networks), Reset Network Settings as escalation
- **VPN:** built-in client types, VPN configuration via app or profile; managed devices get VPN by profile — user symptom: VPN settings greyed/managed
- **Test tools:** ping (reachability), traceroute (path), dig/nslookup (DNS), networkQuality (built-in speed test, macOS)
- Captive portals, proxy basics, personal hotspot as diagnostic alternative path

### Self-Check
1. A Mac has Wi-Fi and Ethernet both connected; traffic must prefer Ethernet. Where is this controlled?
2. A device shows IP 169.254.20.5. What failed?
3. Corporate Wi-Fi uses MAC filtering; a user's iPhone connects at home but not at work. What iOS 26 feature is the likely culprit?
4. Which tool answers "is this a DNS problem?" fastest, and what would you run?
5. A user's Mac loads some sites but not others on hotel Wi-Fi. First suspicion?

### Anki Cards to Build
- Service order: topmost active service = default route
- 169.254.x.x = self-assigned = DHCP failure
- iOS Private Wi-Fi Address = per-network MAC randomisation — breaks MAC filtering
- ping = reachability | traceroute = path | dig = DNS | networkQuality = throughput (macOS)
- Network locations = saved configuration sets, switchable per site

---

## Sunday Jul 26 — REST

No study. Full rest day.

---

## Monday Jul 27 — Security + Privacy: Gatekeeper, Notarization, XProtect, SIP, Privacy Controls, Sharing

**Hours:** 2h

### Topics
- **Gatekeeper:** verifies downloaded apps are signed/notarized before first launch; user override path (Privacy & Security → Open Anyway); quarantine attribute
- **Notarization:** Apple's automated malware scan for developer-distributed (non-App-Store) apps — notarized ≠ App-Store-reviewed
- **XProtect:** built-in antimalware — signature detection, blocking, remediation; updates silently via Apple
- **System Integrity Protection (SIP):** protects system files/processes even from root; disabled only via Recovery (`csrutil`) — support red flag if found off
- **Privacy controls (TCC):** per-app permissions — camera, mic, screen recording, Full Disk Access, Accessibility; managed devices can have these pre-granted/denied by MDM
- **Location Services:** system + per-app control, precise vs approximate
- **Sharing settings (Mac):** Screen Sharing, File Sharing (SMB), Remote Login (SSH), AirDrop scope (Off/Contacts/Everyone 10 min)
- **Stolen Device Protection + Lockdown Mode** (recap from Week 2 — cross-platform security picture)

### Self-Check
1. A user downloads an app from a vendor site; macOS says it can't verify it's free of malware. What's the safe override path, and what should a tech check first?
2. Notarized app vs Mac App Store app — what's the difference in Apple's review?
3. What protects /System from modification by a root process, and how would it ever be disabled?
4. An app can't see files in Documents despite being installed. Which setting gates this?
5. A user must receive an AirDrop file from a visitor not in their contacts. What setting change, and what should they do after?

### Anki Cards to Build
- Gatekeeper: signed + notarized check at first launch; Open Anyway = user override
- Notarization = automated malware scan, NOT App Store review
- XProtect: built-in AV — detect, block, remediate; silent updates
- SIP: system files protected even from root; csrutil in Recovery only
- TCC/Privacy: per-app Camera/Mic/Full Disk Access/Screen Recording grants
- AirDrop: Off / Contacts Only / Everyone for 10 Minutes

---

## Tuesday Jul 28 — Software Updates + Storage + Continuity

**Hours:** 1h

### Topics
- **Software updates:** macOS (System Settings → General → Software Update), iOS; automatic updates; Rapid Security Responses; orgs defer/enforce via MDM — user symptom: "Update managed by your organisation"
- **Storage management:** built-in recommendations (Store in iCloud, Optimise Storage, Empty Bin automatically); storage categories; APFS awareness — snapshots can make "free space" look wrong; purgeable space
- **Continuity family:** Handoff, Universal Clipboard, AirDrop, AirPlay, Sidecar (iPad as display), Continuity Camera (iPhone as webcam), iPhone Mirroring, Instant Hotspot — shared requirements: same Apple Account, Bluetooth + Wi-Fi on, proximity
- Continuity troubleshooting: account mismatch is the #1 cause; then Bluetooth/Wi-Fi state, then feature-specific support matrix

### Self-Check
1. Handoff doesn't work between a user's iPhone and Mac. What are the first two checks?
2. A Mac reports 40 GB free but a 20 GB copy fails for space. What APFS concept explains it?
3. What does "Optimise Storage" offload, and when does it re-download?
4. An org needs updates held for 2 weeks of testing. Where is that enforced — device or MDM?
5. Name the Continuity feature that turns iPhone into a Mac webcam.

### Anki Cards to Build
- Continuity requirements: same Apple Account + Bluetooth + Wi-Fi + proximity
- Continuity #1 failure cause: different Apple Accounts on the two devices
- Purgeable/snapshot space: free-space number can overstate usable space
- MDM can defer OS updates — user sees "managed by organisation"
- Continuity Camera = iPhone as Mac webcam; Sidecar = iPad as display

---

## Wednesday Jul 29 — Full SUP Review (all 9 areas)

**Hours:** 0.5h | Office

Cold-recall sweep — one question per area, no notes:
1. Apple Account: two-factor trusted elements?
2. iCloud: what's excluded from iCloud Backup and why?
3. iPhone/iPad: encrypted local backup adds what?
4. Mac accounts: password-reset path order?
5. Startup/recovery: Erase All Content and Settings vs Disk Utility erase?
6. Diagnostics: memory pressure red means?
7. Networking: 169.254.x.x means?
8. Security: notarization vs App Store review?
9. Updates/Continuity: Handoff requirements?

Failures → targeted Anki + article re-read tonight.

---

## Thursday Jul 30 — SUP-2026 PRACTICE EXAM (paid — the booking gate)

**Hours:** 2.5h | Uninterrupted block

### Logistics
- Sign in with Apple Account at **ACRS** (Apple Certification Records System) → Practice Exams → Apple Device Support Practice Exam → submit application → continue to **Pearson VUE** → pay → start
- ~80 scored questions, 120 minutes, one sitting, no notes
- Pass mark: **75%, not rounded**

### Gate rule
- **≥75% (pass):** book the real SUP-2026 exam via Pearson VUE for Week 5 (target Aug 3–7). Real exam = Pearson OnVUE online proctoring or test centre.
- **65–74%:** book for late Week 5/early Week 6; patch the weak areas the score report shows — do NOT let it slide past Week 6, DEP needs the runway.
- **<65%:** flag it in session notes immediately — schedule triage needed with MD-102 also in flight. Retake costs money and has a 7-day wait, so patch gaps hard before rebooking.

---

## Friday Jul 31 — Debrief + Anki

**Hours:** 0.5h | Office

1. Log practice exam score + per-area weak spots
2. Anki cards for every question you were unsure about
3. Confirm SUP-2026 exam booked (or triage plan written)
4. Week 5 starts DEP-2026 content regardless — SUP final polish rides along as Anki-only
