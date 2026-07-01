# Week 6 — Apple DEP-2026: Configuration Profiles + DDM + VPP Management

**Dates:** Aug 8 – Aug 14, 2026
**Target hours:** ~4.5h (30% of weekly target)
**Focus:** MDM configuration profile payloads, Declarative Device Management (DDM), VPP app management in practice, DEP-2026 full review
**Cert:** Apple Certified IT Professional — Deployment and Management (DEP-2026)

---

## Daily Session Template

1. **Review** (5 min) — Anki cards from Weeks 1–5
2. **Read** (varies) — Apple training material or topic notes
3. **Self-check** (10 min) — 3–4 exam-style questions
4. **Anki update** (5 min) — new cards for today

---

## Hands-On Lab (Kambi's real Intune + ABM tenant, this week)

1. In Kambi's Intune console, open an existing Mac configuration profile (any one already deployed) — identify which payload types it uses (Restrictions, Wi-Fi, Certificate, etc.) and check whether any setting is supervised-only.
2. Check the 2nd MacBook Pro's ABM/Intune status again. If it has deployed since last week: screenshot or note the real enrollment flow you observed (activation → ABM check → Intune contact → supervision) — this is the single best reinforcement of this week's DDM/ADE content, better than another self-check question.
3. If you have visibility into it, check whether Kambi's Intune tenant shows any DDM-related settings for Mac/iOS. Intune's DDM support is partial compared to some MDMs — note whatever you actually find as of now, that's a real, current answer, not a guess from a training doc.

---

## Saturday Aug 8 — Configuration Profile Payloads in Depth

**Hours:** 1.5h

### Topics
- **Configuration profile format:**
  - XML-based `.mobileconfig` file; contains one or more payload dictionaries
  - Each payload: `PayloadType` (what it configures), `PayloadVersion`, `PayloadUUID`, `PayloadIdentifier`
  - Profile-level keys: `PayloadDisplayName` (name shown to user), `PayloadDescription`, `PayloadOrganization`, `PayloadRemovalDisallowed` (true = user cannot remove — only on supervised devices)
  - Signing: profiles can be signed (with cert from trusted CA) → green lock icon when installing; unsigned = warning; MDM-pushed profiles don't require signing (MDM channel is already trusted)

- **Common payload types and what they configure:**
  - **Passcode:** minimum length, complexity, max failed attempts, max inactivity, require alphanumeric; applies to device lock PIN/passcode
  - **Restrictions:** disable camera, AirDrop, iCloud backup, App Store, Safari, explicit content, screen recording, AirPlay; many restrictions are supervised-only
  - **Wi-Fi:** SSID, security type (WPA2/WPA3/WPA2 Enterprise), EAP settings, proxy; delivered silently via MDM
  - **VPN:** VPN type (IKEv2, Cisco AnyConnect, etc.), per-app VPN (supervised — VPN activates only for specific apps)
  - **Certificate (PKCS #12 / SCEP):** install CA certificates or identity certificates; used with Wi-Fi/VPN enterprise auth
  - **Email (Exchange ActiveSync):** auto-configure corporate email account; user only enters password
  - **Web Clip:** adds a shortcut icon to the home screen pointing to a URL (like a web app bookmark)
  - **Single App Mode (SAM):** supervised only; locks device to one specific app; used for kiosks and assessments
  - **DNS proxy / Content Filter:** supervised only; enforce web filtering via third-party MDM or built-in content filter

- **Supervised-only payloads/restrictions (key ones for exam):**
  - Single App Mode, per-app VPN, content filter, global HTTP proxy, device name, app removal restriction, iMessage disable, Activation Lock bypass, AirPrint, wallpaper

- **Profile delivery lifecycle:**
  - MDM push → device downloads profile → device applies silently (supervised) or shows install prompt (unsupervised)
  - Profile update: MDM pushes updated version → old profile replaced silently (supervised)
  - Profile removal by MDM: MDM sends remove command → profile deleted; settings revert
  - Multiple profiles: if two profiles configure the same payload type → most restrictive setting wins (not a conflict/error like Intune — Apple uses most-restrictive logic)

- **Apple Configurator 2:**
  - Mac app for USB-based device configuration (supervision, profile deployment, app install, OS restore)
  - Use cases: supervising devices not in ABM, staging devices in bulk before deployment, factory restore
  - Blueprint: saved configuration (profiles + apps + OS version) to apply to multiple devices at once
  - Automated enrollment: Configurator can add devices to ABM for ADE enrollment

### Self-Check
1. A configuration profile has `PayloadRemovalDisallowed = true`. The device is not supervised. A user tries to remove the profile. What happens?
2. Two MDM-pushed Wi-Fi profiles configure the same SSID with different passwords. Which password does the device use?
3. An IT admin wants to restrict users from disabling Wi-Fi on company iPads. The iPads are not supervised. Can this restriction be applied?
4. A VPN profile is configured as per-app VPN. Only the Salesforce app is listed. A user opens Safari. Does the VPN activate for Safari?
5. An admin signs a configuration profile with a certificate issued by an untrusted CA. What does the user see when installing it manually?

### Anki Cards to Build
- `PayloadRemovalDisallowed = true`: only prevents removal on supervised devices — ignored on unsupervised
- Two profiles, same payload type: most restrictive setting wins (Apple) vs Error state (Intune) — know the difference
- Supervised-only: Single App Mode, per-app VPN, content filter, global HTTP proxy, device name via MDM
- Profile signing: signed with trusted cert = green lock; unsigned = warning; MDM-pushed = no signing required
- Apple Configurator 2: USB-based supervision, staging, OS restore; can add devices to ABM via automated enrollment

---

## Sunday Aug 9 — REST

No study. Full rest day.

---

## Monday Aug 10 — Declarative Device Management (DDM)

**Hours:** 1.5h

### Topics
- **Legacy MDM model (command-and-response):**
  - Server sends command → device executes → device reports result
  - Server must poll device for status — no proactive reporting
  - Scale problem: large device fleets mean constant polling load on MDM server

- **DDM model (declarative):**
  - Server sends declarations (desired state) → device stores them locally → device manages its own state autonomously
  - Device proactively reports status changes to the server via a status channel — no polling required
  - If device state drifts from declaration: device self-corrects without waiting for server command
  - Faster compliance detection: device detects its own non-compliance and reports immediately
  - Reduces MDM server load significantly at scale

- **DDM components:**
  - **Declarations:** what the device should do; four types:
    - **Activation declarations:** enable a configuration on the device (e.g., apply a Wi-Fi config)
    - **Configuration declarations:** the actual settings (Wi-Fi, passcode, etc.) referenced by activations
    - **Asset declarations:** payloads delivered to device (credentials, certificates)
    - **Management declarations:** control MDM enrollment behavior
  - **Status channel:** device-to-server channel; device reports current state, installed profiles, errors, OS version, etc.
  - **Status subscriptions:** MDM server subscribes to specific status items; device reports when those items change

- **DDM requirements:**
  - macOS 13 (Ventura)+, iOS 16+, iPadOS 16+, tvOS 16+
  - MDM server must support DDM (Jamf Pro 10.40+, Microsoft Intune — partial support)
  - Coexists with legacy MDM: DDM and legacy MDM commands work on the same device; DDM handles what it can, legacy handles the rest

- **Practical DDM use case:**
  - Example: DDM passcode declaration deployed to 1000 iPhones
  - Legacy: MDM polls each device periodically to check passcode compliance
  - DDM: each device monitors its own passcode state; if user removes passcode → device immediately reports to MDM server → MDM can take action
  - Result: near-real-time compliance visibility without polling

- **DDM vs Intune equivalent:**
  - DDM status channel ≈ Intune endpoint analytics + compliance state reporting
  - DDM declarations ≈ Intune configuration profiles, but device applies autonomously

### Self-Check
1. An MDM server uses legacy MDM to check passcode compliance across 5000 iPhones. The admin switches to DDM for passcode declarations. How does the compliance check process change?
2. A DDM activation declaration references a configuration declaration. The configuration declaration defines a Wi-Fi payload. What must be true before the Wi-Fi settings are applied to the device?
3. DDM is deployed to macOS 12 (Monterey) Macs. Will DDM declarations apply?
4. A device's passcode is removed by the user after DDM is enrolled. How does the MDM server know?
5. Can DDM and legacy MDM commands coexist on the same device?

### Anki Cards to Build
- Legacy MDM: server-initiated command-response; server polls for status
- DDM: server sends declarations; device is autonomous; device proactively reports via status channel
- DDM activation: references a configuration declaration; device applies the configuration when activation is active
- DDM requires: macOS 13+ / iOS 16+ / MDM server support
- DDM status channel: device-initiated; MDM subscribes to status items; no polling required
- DDM + legacy MDM: coexist on same device; DDM handles declared items, legacy handles remainder

---

## Tuesday Aug 11 — VPP Management in Practice

**Hours:** 1h

### Topics
- **Apps and Books (VPP) full workflow:**
  1. Sign into ABM with Apple ID that has payment method
  2. Browse App Store in ABM → purchase quantity of licenses
  3. Assign licenses to MDM server (Jamf/Intune) in ABM
  4. MDM server syncs with ABM → licenses visible in MDM app catalog
  5. MDM distributes apps to users (user-based) or devices (device-based)

- **License types (recap + depth):**
  - User-based: license tied to Managed Apple ID; app installs on all devices where user is signed in; license follows user if device reassigned
  - Device-based: license tied to device record; no Apple ID required on device; silent install via MDM; ideal for shared/kiosk/classroom devices
  - Switching types: can reassign licenses between users and devices in MDM

- **App updates via VPP:**
  - MDM can auto-update apps from VPP: Jamf → enable automatic updates; Intune → update available notification in Company Portal or force update
  - Supervised devices: MDM can update apps silently without user approval
  - Unsupervised: user sees update prompt in App Store or Company Portal

- **Custom apps (B2B / in-house):**
  - Apps built by the org or a vendor distributed privately (not on public App Store)
  - Distributed via ABM as custom app: vendor uploads to App Store Connect → distributes to specific ABM org → org assigns via VPP like any other app
  - Managed distribution: no public listing; only the org's ABM account sees the app

- **Books via VPP:**
  - Same workflow as apps; buy book licenses in ABM → assign to MDM → distribute to users
  - Mostly used for managed curriculum in education deployments

- **License revocation and reclamation:**
  - User leaves org: revoke license from user in MDM → license returns to org pool → assign to new user
  - Device retired: revoke device-based licenses → licenses return to pool
  - Time to reassign: typically within minutes after revocation syncs to ABM

- **Exam traps:**
  - User-based licenses: Managed Apple ID MUST be signed in on device — if no Managed Apple ID, user-based license cannot install
  - Device-based licenses: no Apple ID required — correct for any shared/kiosk scenario
  - ABM ≠ direct app deployment: ABM assigns licenses to MDM server; MDM does the actual deployment

### Self-Check
1. A school deploys 60 iPads as shared classroom devices. Students do not have Managed Apple IDs configured. The school purchased 60 licenses for an educational app via ABM. Which license type must they use, and why?
2. A user leaves the company. Their iPhone has 3 VPP apps installed under user-based licensing. An admin revokes the licenses in MDM. What happens to the apps on the phone?
3. An org wants to distribute an internal tool that is not on the public App Store. What is the distribution mechanism via ABM?
4. A supervised iPad has a VPP app. MDM pushes an update to the app. Does the user see an "Update available" prompt?
5. An admin assigns 50 app licenses to an MDM server in ABM. The MDM syncs. Where in the MDM workflow does the admin distribute the app to actual devices?

### Anki Cards to Build
- Device-based VPP: no Apple ID required; silent install via MDM; best for shared/kiosk devices
- User-based VPP: requires Managed Apple ID signed in on device; follows user across devices
- VPP workflow: buy in ABM → assign to MDM server → MDM distributes to users/devices
- License revocation: returns to org pool; apps removed from device on next MDM sync
- Custom app (B2B): private App Store distribution via ABM; only assigned ABM orgs can see/install
- Supervised app update: MDM pushes silently — no user prompt required

---

## Wednesday Aug 12 — DEP-2026 Full Review

**Hours:** 0.5h | Office

### Topics to confirm cold recall (no notes):

**ABM:** roles (Administrator / Device Enrollment Manager / Content Manager / People Manager / MDM Server Admin / Viewer), device assignment → MDM server, Managed Apple IDs + federation, Apps and Books (user-based vs device-based)

**ADE:** zero-touch flow, enrollment profile (supervised flag, setup assistant screens), ADE re-enrollment after wipe, skip Setup Assistant screens

**Supervision:** supervised-only capabilities (Silent App Mode, Lost Mode, per-app VPN, content filter, device name, restrict app removal), user CANNOT remove MDM profile, supervision persists through wipe

**Enrollment types:** User Enrollment (BYOD, Managed Apple ID required, no serial in MDM, selective wipe only), Device Enrollment (corporate, manual, user can remove MDM profile), ADE (zero-touch, supervised, user CANNOT remove MDM profile)

**Configuration profiles:** payload types, most-restrictive logic (not conflict), supervised-only payloads, `PayloadRemovalDisallowed` only works on supervised

**DDM:** declarations vs commands, status channel (device-initiated), requires macOS 13+ / iOS 16+, coexists with legacy MDM

**VPP:** user-based (Managed Apple ID required) vs device-based (no Apple ID), revocation, custom apps, supervised silent update

### 3 Self-Check Questions
1. An admin creates an MDM enrollment profile with supervised = Yes and assigns it in ABM. A MacBook is wiped via macOS Recovery. After the wipe, the device boots and connects to the internet. It does NOT re-enroll in MDM. What is the most likely cause?
2. A content filter payload is pushed via MDM to an iPhone. The iPhone is enrolled via User Enrollment (BYOD). Does the content filter apply?
3. A DDM configuration declaration defines a passcode policy. The device is running iOS 15.7. The MDM server supports DDM. Will the declaration apply?

---

## Thursday Aug 13 — Debrief Gaps + Anki

**Hours:** 0.5h | Office

### Tasks
1. Revisit wrong answers from Wed review
2. Anki review: all Apple Weeks 1–6 new cards
3. 3 cross-topic scenario questions:
   - An org uses ADE with supervised = Yes. A user is offboarded. IT retires the device in MDM (Intune/Jamf). What steps should IT take in ABM before re-issuing the device to a new user?
   - An iPad is enrolled via ADE. The enrollment profile does not have `supervised = Yes`. Which capabilities are unavailable via MDM?
   - An admin pushes a DDM passcode declaration AND a legacy MDM passcode configuration profile to the same device. Which takes precedence?

---

## Friday Aug 14 — Final Anki Review + DEP-2026 Coverage Check

**Hours:** 0.5h | Office — Anki only

### Tasks
1. Full Anki review: all Apple Weeks 1–6 decks
2. SUP-2026 + DEP-2026 complete coverage check:
   - **SUP-2026 (Device Support):** Hardware ID, ports, Apple Silicon vs Intel (W1) ✅ | Startup keys, Recovery, Safe Mode, DFU, reinstall (W2) ✅ | Apple Diagnostics, Console.app, FileVault intro (W3) ✅ | FileVault management, T2/Secure Enclave, Diagnostics codes (W4) ✅ | **Exam:** should already be booked or taken by now (flexible, targeted after Week 4)
   - **DEP-2026 (Deployment & Management):** ABM, ADE, supervision, enrollment types (W5) ✅ | Config profile payloads, DDM, VPP (W6) ✅ | **Exam:** Week 9
3. Week 7 is DEP-2026 final review only (no new content, no exam). MD-102 exam is Week 8. DEP-2026 exam is Week 9.
4. Confirm SUP-2026 result if taken; if not yet taken, don't let it drift — book it now.
