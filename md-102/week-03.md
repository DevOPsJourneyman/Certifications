# Week 3 — MD-102 Domain 3 Part 1: Manage and Protect Devices

**Dates:** Jul 11 – Jul 17, 2026  
**Target hours:** ~21h  
**Focus:** Multi-platform enrollment, configuration profiles, LAPS, endpoint security policies, Intune reporting  
**Exam weight:** D3 = ~25% of MD-102

---

## Saturday Jul 11 — Multi-Platform Enrollment: macOS, iOS, Android
**Hours:** 5h | Big block — theory + lab

### Topics

**macOS enrollment:**
- Apple Business Manager (ABM) / Apple School Manager (ASM): device registration for ADE (Automated Device Enrollment)
- ADE (formerly DEP): MDM profile pushed at activation, supervised, cannot be removed by user
- User-initiated enrollment: downloads enrollment profile from Company Portal; unsupervised; user can remove
- Intune Connector for Apple: token renewal required annually (ABM sync token)
- Platform SSO (macOS 13+): Entra ID credentials used at macOS login screen
- macOS enrollment profile: MDM push certificate required (Apple APNs cert, renew annually)

**iOS / iPadOS enrollment:**
- ADE (supervised): corporate-owned; MDM profile locked; strongest management
- User Enrollment: BYOD; uses Managed Apple ID; limited management scope (no full wipe, no device inventory)
- Web-based enrollment (iOS 16+): no Company Portal app needed
- VPP (Volume Purchase Program) / Apple Business Manager: app licensing without Apple ID per user
- Enrollment type determines supervision level → supervision required for many restrictions

**Android enrollment:**
- Android Enterprise modes:
  - **Fully Managed (COBO):** corporate-owned, single user, full device management, no personal use
  - **Dedicated (COSU):** corporate-owned, kiosk/shared device, locked to one or few apps
  - **Work Profile (BYOD):** personal device, separate managed work profile, selective wipe only
  - **Corporate-Owned Work Profile (COPE):** corporate-owned + personal use allowed, work profile + device management
- Device Admin (legacy): avoid — limited capabilities, Google deprecated; still appears on exam
- Zero-touch enrollment: Android equivalent of Autopilot for Fully Managed devices
- Android Enterprise requires: Google account binding in Intune (Managed Google Play)

### MS Learn Modules
- [Set up Apple device management](https://learn.microsoft.com/en-us/training/modules/set-up-apple-device-management/)
- [Set up Android device management](https://learn.microsoft.com/en-us/training/modules/set-up-android-device-management/)

### Lab Tasks
1. Intune → Devices → Enrollment → Apple → APNs certificate: review expiry date (do NOT let this expire in prod)
2. Intune → Enrollment → Apple → Enrollment program tokens → review ABM token sync status
3. Create an iOS ADE enrollment profile:
   - Setup Assistant: skip location, skip privacy, skip Siri
   - Supervised: Yes
   - Locked enrollment: Yes
   - Assign to device group
4. Intune → Enrollment → Android → Managed Google Play: review binding status
5. Create an Android Work Profile enrollment restriction — allow personally-owned work profile only (block fully managed for personal devices)

### Self-Check
1. A macOS device was enrolled via user-initiated enrollment. IT wants to push a kernel extension policy. Will this work? Why or why not?
2. iOS ADE vs User Enrollment: which allows IT to perform a remote **full device wipe**?
3. Android Work Profile enrollment: which data does Intune selective wipe remove?
4. The APNs certificate for iOS expires tomorrow. IT doesn't renew it. What happens to existing enrolled iOS devices?
5. Android Fully Managed vs Dedicated: key difference in intended use case?

### Anki Cards to Build
- ABM/ADE: supervised + locked = cannot remove MDM, full management
- iOS User Enrollment: Managed Apple ID required, limited scope (no full wipe, no device inventory)
- Android Work Profile: work data wiped, personal data untouched on selective wipe
- APNs cert expiry: existing devices lose MDM communication; must renew before expiry
- Zero-touch enrollment: Android Enterprise = Autopilot equivalent

---

## Sunday Jul 12 — REST

No study. Full rest day.

---

## Monday Jul 13 — Configuration Profiles: Windows + macOS
**Hours:** 5h | Big block — theory + lab

### Topics

**Windows configuration profiles:**
- **Settings catalog** (preferred): granular settings from all Windows CSPs in one interface; searchable; replaces most templates
- **Administrative Templates (ADMX-backed):** GPO-equivalent settings; ingested via Intune for third-party ADMX too
- **Templates (legacy):** device restrictions, endpoint protection, etc.; being replaced by settings catalog
- **OMA-URI (custom):** raw CSP paths for settings not yet in catalog; format: `./Vendor/MSFT/Policy/Config/<Area>/<Setting>`
- Profile assignment: **device groups** = policy applies at enrollment regardless of user; **user groups** = applies when user signs in
- Conflict resolution: if two profiles set the same setting to different values → **Conflict** state; neither value applies
- Profile types for exam: device restrictions, endpoint protection, identity protection, kiosk, shared multi-user device

**macOS configuration profiles:**
- Preference domains: com.apple.Safari, com.microsoft.Intune, etc.
- Intune macOS templates: device restrictions, endpoint protection, extensions, system preferences
- Custom profiles: upload .mobileconfig XML; used for settings Intune UI doesn't expose
- FileVault (disk encryption): enforce via endpoint security → disk encryption policy (not device config)
- Kernel extensions (KEXT) + System Extensions: requires supervised + user-approved (or ADE-enrolled)

**Shared device config:**
- Shared PC mode (Windows): multiple users, account cleanup, sleep/hibernation settings
- Shared iPad: multiple Managed Apple IDs on one device (education focus, but on exam)

### MS Learn Modules
- [Configure device profiles](https://learn.microsoft.com/en-us/training/modules/configure-device-profiles/)
- [Monitor device profiles](https://learn.microsoft.com/en-us/training/modules/monitor-device-profiles/)

### Lab Tasks
1. Intune → Devices → Configuration → Create profile → Windows 10 and later → Settings catalog:
   - Search "BitLocker" → enable OS drive encryption, require startup PIN
   - Search "SmartScreen" → enable for apps + Edge
   - Assign to device group
2. Create a second profile: Administrative Templates → configure "Prevent access to registry editing tools" = Enabled
3. Test conflict: assign both profiles to same group; set a setting (e.g., "Allow Telemetry") to different values in each → verify Conflict state in monitor
4. Create a macOS profile: device restrictions → disable iCloud backup, disable AirDrop
5. Review: Devices → Monitor → Assignment failures — note error code format

### Self-Check
1. Settings catalog vs Administrative Templates in Intune: which is the preferred method for new policy creation?
2. A Windows device has two configuration profiles both targeting it. Profile A sets "Allow Telemetry" = 0. Profile B sets "Allow Telemetry" = 1. What is the resulting state?
3. A macOS device needs a custom .mobileconfig deployed. What profile type is used in Intune?
4. A configuration profile is assigned to a **user group**. A device has no user signed in (kiosk). Will the profile apply?
5. OMA-URI path format: `./Vendor/MSFT/Policy/Config/Browser/AllowBrowser` — what does `./Vendor/MSFT` indicate?

### Anki Cards to Build
- Settings catalog = preferred; Administrative Templates = GPO parity (ADMX-backed)
- Conflict: two profiles, same setting, different values → neither applies, both show Conflict
- Device group assignment: applies regardless of signed-in user
- User group assignment: applies only when that user signs in; doesn't apply on shared/kiosk devices
- OMA-URI: raw CSP path for unlisted settings; `./Device/` = device context; `./User/` = user context

---

## Tuesday Jul 14 — LAPS + Endpoint Security Policies
**Hours:** 3.5h | WFH — focused topic

### Topics

**Microsoft LAPS (cloud-native):**
- Manages local administrator account password on AAD Joined + Hybrid AAD Joined Windows devices
- Password stored in Entra ID device object; rotated automatically on next login after expiry
- Prerequisites: Windows 10 20H2+ or Windows 11, LAPS CSP enabled via Intune
- Who can read LAPS password: Global Admin, Intune Admin, or custom role with `deviceManagementManagedDevices/laps` permission
- Manual rotation: Intune → device → Rotate local admin password (immediate rotation)
- Legacy LAPS (on-prem AD): separate — stores password in AD attribute; different from cloud LAPS
- Exam trap: cloud LAPS ≠ legacy LAPS; both exist simultaneously, serve different join types

**Endpoint security policies (Intune):**
- Antivirus: Defender AV settings (real-time protection, cloud-delivered protection, sample submission, exclusions, scan schedule)
- Disk encryption: BitLocker (Windows) — OS drive, fixed drive, removable drive; recovery key escrow to Entra ID
- Firewall: Windows Defender Firewall per network profile (domain, private, public); custom firewall rules
- Endpoint detection and response (EDR): onboarding package, MDE connectivity settings (covered Week 2)
- Account protection: LAPS config (password length, complexity, age), credential guard, local user group membership
- Attack surface reduction: ASR rules — always Audit → Block; never enable Block without baseline first
- **Reusable settings groups:** define exclusions once, reference in multiple Antivirus policies (reduces duplication)
- Security baselines vs endpoint security vs config profiles: all can manage Defender settings — conflict = whichever wins per CSP

**BitLocker specifics (exam-heavy):**
- Recovery key escrow to Entra ID: required before encryption allowed (configurable)
- Silent encryption: TPM-only, no user prompt; requires AAD joined + modern standby or TPM 2.0
- Startup PIN: user must enter PIN at boot; adds pre-boot auth
- Fixed drive encryption: separate policy from OS drive; can require OS drive encrypted first

### MS Learn Modules
- [Configure Microsoft LAPS](https://learn.microsoft.com/en-us/training/modules/implement-endpoint-privilege-management/)
- [Configure endpoint security in Intune](https://learn.microsoft.com/en-us/training/modules/configure-endpoint-security/)

### Lab Tasks
1. Intune → Endpoint security → Account protection → Create policy → Local admin password solution (LAPS):
   - Password age: 14 days
   - Password complexity: Letters + numbers + special
   - Administrator account name: leave blank (manages built-in admin)
   - Assign to Windows device group
2. Intune → Endpoint security → Disk encryption → Create policy → BitLocker:
   - OS drive: require encryption, recovery key to Entra ID, block write without encryption
   - Fixed drives: require encryption
   - Assign to same group
3. View LAPS password (if device enrolled): Devices → select device → Local admin password
4. Intune → Endpoint security → Antivirus → Create Defender AV policy:
   - Enable cloud-delivered protection: Yes
   - Enable real-time protection: Yes
   - Sample submission: send safe samples
5. Review reusable settings groups: Endpoint security → Reusable settings → create an exclusion group for a fake path (e.g., C:\BuildTools\)

### Self-Check
1. Cloud LAPS stores the local admin password where?
2. A device needs silent BitLocker encryption. What two hardware requirements must be met?
3. LAPS password rotation is triggered by admin from Intune. When does the new password take effect?
4. Two Intune policies both configure Windows Defender Real-time protection: an Endpoint Security Antivirus policy and an MDM Security Baseline. The values differ. What is the resolution?
5. BitLocker recovery key is not escrowed to Entra ID yet. The admin configured LAPS to require escrow before encryption. What does the device show in Intune?

### Anki Cards to Build
- Cloud LAPS: password in Entra device object; requires Win 10 20H2+ or Win 11
- Silent BitLocker: TPM 2.0 + AAD joined (or modern standby)
- LAPS rotation: immediate in portal, but password changes at **next device login**
- Reusable settings groups: define AV exclusions once, reuse across policies
- Endpoint security + config profile conflict on same CSP: conflict state, neither applies

---

## Wednesday Jul 15 — D3 Part 1 Review + Week 2 Carry-Forward
**Hours:** 2.5h | Office — consolidation + gap fill

### Tasks
1. Review Week 2 carry-forward items (anything scored ≤ 3 in D2 checklist)
2. Run Anki deck: all Week 3 cards built Mon–Tue
3. Review self-check failures from Sat + Mon + Tue
4. If time: read [Intune reports overview](https://learn.microsoft.com/en-us/mem/intune/fundamentals/reports)

### D3 Part 1 Carry-Forward Traps
Common exam scenarios to verify you know cold:
- macOS KEXT policy requires supervised device — user-enrolled macOS cannot receive it
- Android Work Profile: IT manages work profile, user manages personal side; no full device wipe
- LAPS: manual rotation in Intune changes password at next login, not immediately
- Configuration profile conflict: BOTH profiles show conflict, NEITHER setting applies
- ADE supervision: locked enrollment prevents profile removal; user-initiated does not

### Self-Check (D3 Part 1 mixed)
1. A macOS device enrolled via ADE has a Kernel Extension policy. A macOS device enrolled via user-initiated enrollment also has the same policy assigned. Which device successfully receives the KEXT policy?
2. Android Dedicated (COSU) vs Fully Managed (COBO): which is designed for kiosk/shared use?
3. A Windows Settings Catalog profile and an Administrative Template profile both set the same registry key to different values. What Intune state do both profiles show?
4. Cloud LAPS password age is set to 30 days. The password was last set 35 days ago. A technician logs into the device. What happens?
5. BitLocker silent encryption fails on a device. The device has TPM 2.0 but is Hybrid AAD Joined and on a domain network. What is likely missing?

---

## Thursday Jul 16 — Intune Reporting + Device Health Monitoring
**Hours:** 2.5h | Office — operational knowledge

### Topics
- **Intune report types:**
  - Operational: real-time, unfiltered; e.g., "Noncompliant devices" — immediate current state
  - Analytical: aggregated, historical trend; e.g., "Device compliance org" — trends over time
  - Specialty: focused on specific scenarios; e.g., "Feature update failures"
- **Key reports for exam:**
  - Device compliance: per-device compliance state, per-setting breakdown
  - Assignment failures: which profiles failed to apply and why
  - Windows Update compliance: update rings deferral/install status
  - Feature update report: upgrade deployment status
  - App install report: per-app success/failure/pending
- **Device health attestation:** TPM-based hardware health report; used in CA compliance evaluation
- **Endpoint analytics:** boot time, restart frequency, app reliability, hardware scores
- **Log collection:** Devices → Collect diagnostics → uploads diagnostic logs to Intune portal (no agent needed)
- **Intune Data Warehouse:** export Intune data to Power BI for custom dashboards (ODATA endpoint)

### Lab Tasks
1. Intune → Reports → Device compliance → Org report → review compliance percentage by platform
2. Intune → Devices → Monitor → Assignment failures → identify any profile or app failures
3. Intune → Reports → Endpoint analytics → Startup performance (if data available) — note boot score
4. Select a device → Collect diagnostics → verify upload to portal
5. Intune → Reports → Windows updates → Feature update report → review success vs failed vs in progress

### Self-Check
1. Which Intune report type shows real-time noncompliant devices (not aggregated)?
2. A profile shows "Error" in assignment status. Where do you go in Intune to find the specific error code?
3. Endpoint analytics requires which agent or process on devices to collect boot data?
4. Intune Data Warehouse — what is its primary use case?
5. A device collects diagnostics successfully. Where do the logs appear in Intune?

### Anki Cards to Build
- Operational report = real-time current state; Analytical = trend/aggregated
- Assignment failures report: specific error codes per device per profile
- Endpoint analytics: requires Intune-managed + Intune Management Extension (IME) or ConfigMgr sensor
- Collect diagnostics: no extra agent; output in Devices → device → Diagnostics tab

---

## Friday Jul 17 — D3 Part 1 Final Review + Anki Run
**Hours:** 2.5h | Office — close week, prep Week 4

### Tasks
1. Full Anki deck run: all Week 3 cards
2. Review Week 3 self-check failures
3. Preview Week 4: D3 Part 2 (remote actions, device categories) + D4 (app deployment, MAM, M365 Apps)
4. Schedule: Week 4 ends with full 65-question timed practice exam (Thu Jul 23)

### D3 Part 1 Confidence Checklist

| Topic | Score |
|-------|-------|
| macOS enrollment: ADE vs user-initiated | / 5 |
| iOS: ADE vs User Enrollment scope differences | / 5 |
| Android Enterprise modes (COBO/COSU/Work Profile/COPE) | / 5 |
| Windows config profiles: catalog vs templates vs OMA-URI | / 5 |
| Profile conflict resolution | / 5 |
| Device group vs user group assignment behavior | / 5 |
| Cloud LAPS: prerequisites + password location + rotation | / 5 |
| BitLocker silent encryption requirements | / 5 |
| Endpoint security policy types | / 5 |
| Intune report types: operational vs analytical | / 5 |

Anything ≤ 3: carry into Week 4 buffer (Wed Jul 22).

---

## Week 3 Resources

| Resource | URL / Notes |
|----------|-------------|
| MS Learn D3 path | [Manage devices with Intune](https://learn.microsoft.com/en-us/training/paths/endpoint-manager-fundamentals/) |
| Apple Platform Deployment guide | learn.microsoft.com/en-us/mem/intune/enrollment/device-enrollment |
| Android Enterprise docs | learn.microsoft.com/en-us/mem/intune/enrollment/android-enroll |
| LAPS overview | learn.microsoft.com/en-us/windows-server/identity/laps/laps-overview |
| John Savill — Intune deep dive | YouTube: "John Savill Microsoft Intune" |
| Newton topic file | `~/study-trainer/topics/md102-d3-manage-protect-devices.md` |

---

## Week 3 Summary (fill in on Jul 17)

- Hours logged: ___
- Anki cards built: ___
- D3 Part 1 weakest area: ___
- Carry-forward to Week 4 buffer: ___
