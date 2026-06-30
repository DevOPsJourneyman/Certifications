# Week 4 — MD-102 Domain 3 Part 1: Device Configuration + LAPS

**Dates:** Jul 25 – Jul 31, 2026
**Target hours:** ~10.5h (70% of weekly target)
**Focus:** Device configuration profiles, settings catalog, ADMX templates, OMA-URI, LAPS, remote actions
**Exam weight:** D3 = ~25–30% of MD-102

---

## Daily Session Template

1. **Review** (10 min) — Anki cards from previous session
2. **Read** (varies) — topic notes + MS Learn module
3. **Lab** (varies) — hands-on in dev tenant
4. **Self-check** (15 min) — 4–5 exam-style questions; mark failures
5. **Anki update** (10 min) — add cards for today's exam gotchas

---

## Saturday Jul 25 — Device Configuration Profiles

**Hours:** 3.5h | Big block — theory + lab

### Topics
- **Profile types in Intune:**
  - **Settings catalog:** modern approach — browse all available settings by category; replaces device restrictions template
  - **Administrative templates (ADMX-backed):** Group Policy equivalent settings for Windows; ingests ADMX files from on-prem if needed
  - **Device restrictions:** legacy template; maps to common lockdown settings (camera, screenshots, app install)
  - **Custom (OMA-URI):** raw MDM configuration; used when a setting isn't in settings catalog or ADMX
  - **Endpoint protection:** Windows Defender, BitLocker, firewall, Smart Screen — all in one profile
  - **Email, VPN, Wi-Fi, Certificate profiles:** delivery of connection credentials to managed devices
- **Profile assignment:**
  - Assign to: user groups or device groups
  - User group assignment: applies when the user signs in (follows the user, not device)
  - Device group assignment: applies as soon as device enrolls (ideal for shared devices)
  - Exclude groups: exclusion takes precedence over include
- **Conflict resolution:**
  - Same setting configured in two profiles → **Error/Conflict** state — neither value applies to that setting
  - Conflict does NOT affect other settings in the same profile — only the conflicting setting
  - Resolution: remove duplicate setting from one profile; use Intune reports to identify conflict source
- **Profile applicability:**
  - OS version filter: target only Win 10 22H2, or Win 11 23H2+ etc.
  - Scope tags: RBAC boundary — admins only see profiles with matching scope tags

### MS Learn Modules
- [Configure device profiles in Intune](https://learn.microsoft.com/en-us/training/modules/configure-device-profiles/)
- [Create device configuration profiles](https://learn.microsoft.com/en-us/training/modules/create-device-configuration-profiles/)

### Lab Tasks
1. Create a Settings Catalog profile → Windows → target: Windows 10/11
   - Browser: configure Microsoft Edge homepage URL, disable InPrivate mode
   - Assign to All Devices
2. Create an ADMX-backed Administrative Templates profile:
   - Computer config → Windows components → OneDrive → Silently move Windows known folders to OneDrive = Enabled
   - Assign to a pilot device group
3. Create a custom OMA-URI profile:
   - OMA-URI: `./Device/Vendor/MSFT/Policy/Config/Browser/AllowSmartScreen`
   - Data type: Integer, Value: 1
   - (Note: Settings Catalog covers this too — this is just OMA-URI practice)
4. Intune → Devices → Configuration → select your Settings Catalog profile → Device status — verify assignment
5. Intune → Devices → Configuration → select your ADMX profile → Per-setting status — check for conflict state

### Self-Check
1. A settings catalog profile and a device restrictions profile both configure the same setting. What is the resulting state for that setting on the device?
2. A Wi-Fi profile is assigned to a user group. A device is enrolled but no user has signed in. Will the Wi-Fi profile apply?
3. OMA-URI `./Device/Vendor/MSFT/...` targets the device. `./User/Vendor/MSFT/...` targets what?
4. An admin needs to push a Group Policy setting that is not yet in the Settings Catalog. Which profile type should they use?
5. A device is in both an "Include" group and an "Exclude" group for a configuration profile. Which takes precedence?

### Anki Cards to Build
- Settings catalog: modern, browse-all-settings approach — replaces device restrictions
- ADMX templates: GPO equivalent in Intune — use for settings not yet in catalog
- OMA-URI `./Device/` vs `./User/`: device-scope vs user-scope MDM URI
- Conflict state: same setting in two profiles → Error — neither value applied to that setting
- Exclude > Include: exclusion always wins in Intune assignment

---

## Sunday Jul 26 — REST

No study. Full rest day.

---

## Monday Jul 27 — LAPS + Remote Actions

**Hours:** 3.5h | Big block — theory + lab

### Topics
- **Windows LAPS (Local Administrator Password Solution):**
  - Manages local administrator account passwords on Windows devices
  - Intune-managed LAPS: stores password in Entra ID (not on-prem AD)
  - Requirements: Windows 11 22H2+ (built-in), or Windows 10/11 with KB5025221+ (April 2023 CU)
  - Password rotation: on-demand (manual rotate in Intune) or automatic at configurable interval
  - Who can view LAPS password: Global Admin, Intune Admin, or custom role with "Read BitLocker key" permission
  - LAPS manages: local administrator accounts only — NOT domain accounts
  - Configuration: Endpoint security → Account protection → LAPS policy
- **Remote device actions in Intune:**
  - **Retire:** removes MDM enrollment + company data, leaves personal data — BYOD offboarding
  - **Wipe:** factory reset — removes everything including OS; optional: retain enrollment state for Autopilot
  - **Fresh Start (Windows):** reinstalls Windows, removes OEM bloatware, keeps user data + MDM enrollment
  - **Delete:** removes device record from Intune — does NOT wipe the device
  - **Sync:** forces device to check in with Intune immediately
  - **Restart:** remote restart (requires device online)
  - **Collect diagnostics:** pulls logs from device to Intune portal (Windows only)
  - **Rotate BitLocker key:** generates new recovery key and escrows to Intune/Entra
  - **Locate device (iOS/macOS):** GPS location from Managed Device
  - **Lost mode (iOS):** locks device, shows message, plays sound — only Supervised devices

### MS Learn Modules
- [Manage device security with endpoint security policies](https://learn.microsoft.com/en-us/training/modules/manage-device-security-endpoint-security-policies/)
- [Monitor device configuration profiles](https://learn.microsoft.com/en-us/training/modules/monitor-device-configuration-profiles/)

### Lab Tasks
1. Endpoint security → Account protection → LAPS policy → Windows 10 and later:
   - Backup directory: Azure Active Directory
   - Password age (days): 30
   - Administrator account name: specify (or use default built-in admin)
   - Assign to pilot device group
2. View LAPS password: Devices → select enrolled device → Local administrator password
3. Practice remote actions on a test device:
   - Sync → observe check-in time update
   - Collect diagnostics → download the zip
   - Restart (if device is available)
4. Review retire vs wipe: Devices → select device → Retire vs Wipe — note the confirmation dialog difference
5. Rotate BitLocker key: Devices → select device → Rotate BitLocker keys (if BitLocker is enabled)

### Self-Check
1. A device is unenrolled from Intune using "Retire." Is the device wiped?
2. A LAPS policy is assigned to a Windows 10 21H2 device. The device does not have KB5025221 installed. Will LAPS work?
3. Who can view the LAPS password in Intune by default (minimum built-in role)?
4. "Delete" is performed on an Intune device record. The device still has Windows installed. What happens?
5. A corporate iOS device is lost. It is a Supervised device enrolled via ADE. What remote action can you take to lock it, display a message, and play a sound?

### Anki Cards to Build
- LAPS requires: Windows 11 22H2+ built-in OR Windows 10/11 + KB5025221 (April 2023 CU)
- LAPS password stored in: Entra ID (Intune-managed) or on-prem AD (legacy LAPS)
- Retire: removes company data + MDM; leaves personal data (BYOD wipe)
- Wipe: full factory reset; optional retain enrollment state
- Delete: removes Intune record only — device not wiped
- Fresh Start: reinstall Windows, remove OEM bloatware, keep user data + MDM

---

## Tuesday Jul 28 — Endpoint Security Policies + Monitoring

**Hours:** 2.5h | WFH — focused topic

### Topics
- **Endpoint security policy types:**
  - Antivirus: Defender AV settings — real-time protection, cloud protection, exclusions, PUA protection
  - Disk encryption: BitLocker for Windows, FileVault for macOS — configure and escrow recovery keys
  - Firewall: Windows Defender Firewall rules — inbound/outbound, per-profile (domain/private/public)
  - Endpoint detection and response (EDR): MDE onboarding settings
  - Attack surface reduction (ASR): ASR rules, controlled folder access, network protection
  - Account protection: LAPS, Windows Hello for Business enrollment
- **BitLocker configuration via Intune:**
  - Require BitLocker: via compliance policy (reports compliance) vs via endpoint protection profile (configures BitLocker)
  - Escrow recovery key: endpoint protection profile → BitLocker → configure recovery key backup to Entra ID
  - Silent enablement: pre-provision BitLocker via Autopilot + endpoint protection profile
  - View recovery key: Devices → select device → Recovery keys → BitLocker
- **Intune monitoring and reports:**
  - Reports → Endpoint security → reports by policy type (Antivirus, Firewall, BitLocker)
  - Devices → Monitor → Device compliance (per-device state)
  - Devices → Configuration → select profile → Device status (per-device assignment result)
  - Reports → Windows → Feature update failures (specific to WUfB)

### MS Learn Modules
- [Manage endpoint security in Microsoft Intune](https://learn.microsoft.com/en-us/training/modules/manage-endpoint-security/)

### Lab Tasks
1. Endpoint security → Disk encryption → Create BitLocker policy:
   - Require device encryption: Yes
   - Recovery key backup: Require (backup to Entra ID)
   - Startup PIN: require (or allow)
   - Assign to pilot device group
2. Endpoint security → Antivirus → create policy: real-time protection = On, cloud protection = Enabled, PUA protection = Enabled
3. Reports → Endpoint security → BitLocker → check any devices showing "Not encrypted"
4. Devices → select a device → Recovery keys → view BitLocker key (note: requires appropriate role)
5. Endpoint security → Firewall → create policy → Windows firewall: Domain network = On, inbound = Block, outbound = Allow

### Self-Check
1. BitLocker compliance policy reports a device as non-compliant. Does this configure BitLocker encryption on the device?
2. A device has two Intune antivirus policies assigning conflicting values to "Cloud protection level." What is the device's state for that setting?
3. Where in Intune does an admin view a device's BitLocker recovery key?
4. ASR rule "Block executable content from email and webmail client" is set to Audit mode. A user opens a malicious attachment. What happens?
5. A user reports an Intune policy isn't applying. You want to see if the profile was assigned and what the result was. Where do you check?

### Anki Cards to Build
- BitLocker compliance policy: reports compliance state — does NOT configure BitLocker
- BitLocker endpoint protection profile: configures and enables BitLocker
- Recovery key view: Devices → device → Recovery keys → BitLocker
- ASR Audit mode: logs activity to Event Viewer/MDE — does NOT block
- Conflict in endpoint security policies: same as config profiles — Error state for that setting

---

## Wednesday Jul 29 — D3 Gap Fill + Self-Check

**Hours:** 2h | Office

### Topics
- Revisit any self-check failures from Sat–Tue
- Settings catalog vs ADMX vs OMA-URI decision tree
- Remote action use cases: when to use Retire vs Wipe vs Fresh Start vs Delete

### Practice Questions (mixed D3 Part 1)
1. An admin removes a setting from a settings catalog profile that was previously in conflict. When does the conflict resolve — immediately, or at next device check-in?
2. A Windows 10 device has LAPS configured. The local admin password is 30 days old. The LAPS policy sets password age = 14 days. What happens at next Intune check-in?
3. A device shows "Not evaluated" for a compliance policy. What does this state mean?
4. You want to configure a custom Microsoft Edge policy that is not yet available in the Settings Catalog. Which profile type do you use and where do you get the ADMX file?
5. An admin performs "Wipe" with "Retain enrollment state" enabled. The device reboots into OOBE. What happens at the OOBE screen?

---

## Thursday Jul 30 — Anki + Practice Gaps

**Hours:** 2h | Office

### Tasks
1. Full Anki review: D1 + D2 + D3 Part 1 decks
2. 5-question self-quiz: mixed D1+D2+D3 — cover all domains studied so far
3. Flag any D3 topics that need more coverage (carry to Week 5 D3 Part 2 review)

---

## Friday Jul 31 — Anki Review + Week Close

**Hours:** 1h | Office — Anki only

### Tasks
1. Full review D3 Part 1 Anki deck
2. Note weak cards — carry to Week 5
3. Confirm lab tasks done: BitLocker policy, LAPS policy, settings catalog profile, remote action practice
