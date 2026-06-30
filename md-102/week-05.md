# Week 5 — MD-102 Domain 3 Part 2 + Domain 4: Compliance, MDE, App Management

**Dates:** Aug 1 – Aug 7, 2026
**Target hours:** ~10.5h (70% of weekly target)
**Focus:** Compliance policies, MDE integration, Win32 app packaging, MAM / App Protection Policies, M365 Apps deployment
**Exam weight:** D3 = ~25–30% | D4 = ~15–20% of MD-102

---

## Daily Session Template

1. **Review** (10 min) — Anki cards from previous session
2. **Read** (varies) — topic notes + MS Learn module
3. **Lab** (varies) — hands-on in dev tenant
4. **Self-check** (15 min) — 4–5 exam-style questions; mark failures
5. **Anki update** (10 min) — add cards for today's exam gotchas

---

## Saturday Aug 1 — Compliance Policies in Depth

**Hours:** 3.5h | Big block — theory + lab

### Topics
- **What compliance policies do:**
  - Define rules a device must meet to be considered compliant (OS version, encryption, PIN, Defender AV state)
  - Compliance policy alone does NOT enforce settings — it only reports state
  - To block non-compliant devices: pair compliance policy with a Conditional Access policy
- **Compliance states:**
  - **Compliant:** device meets all policy requirements
  - **Not compliant:** device fails one or more requirements; actions for non-compliance apply
  - **In grace period:** device is non-compliant but within the configured grace period — CA still grants access
  - **Not evaluated:** policy not yet applied (device just enrolled, or no policy assigned to device)
  - **Error:** policy couldn't be evaluated (agent issue, check-in failure)
- **Grace period:**
  - Configured per compliance policy (0–730 days)
  - During grace period: device shows "In grace period" but Conditional Access treats it as **non-compliant** — access decisions depend on CA policy configuration
  - After grace period expires: device is marked Not Compliant; CA blocks access if configured
- **Actions for non-compliance (in order of severity):**
  1. Mark device non-compliant (immediate or delayed)
  2. Send email to end user (configurable template)
  3. Send push notification to Company Portal
  4. Remotely lock device
  5. Retire device (remove company data)
- **Per-platform compliance settings:**
  - Windows: Bitlocker required, Secure Boot, Code Integrity, OS version min/max, Defender AV state, Firewall state, TPM required
  - iOS/iPadOS: minimum OS version, passcode required, device not jailbroken
  - macOS: FileVault required, Gatekeeper, minimum OS version, firewall
  - Android: minimum OS version, device not rooted, SafetyNet attestation
- **Conditional Access + compliance:**
  - Device-based CA condition: "Require device to be marked compliant"
  - Only devices managed by Intune AND marked compliant → granted access
  - Hybrid Entra join + compliant = most secure combined requirement
  - CA policy applies even to Entra-registered (BYOD) devices if compliance policy assigned to user

### MS Learn Modules
- [Create compliance policies in Intune](https://learn.microsoft.com/en-us/training/modules/create-compliance-policies/)
- [Protect access to resources using Intune](https://learn.microsoft.com/en-us/training/modules/protect-access-to-resources-intune/)

### Lab Tasks
1. Intune → Compliance → Policies → Create → Windows 10 and later:
   - BitLocker required: Yes
   - Minimum OS version: 10.0.19041 (Windows 10 2004)
   - Microsoft Defender Antimalware: Required
   - Grace period: 3 days
   - Actions for non-compliance: Mark not compliant (day 0), Send email to user (day 1)
   - Assign to All Devices
2. Devices → Compliance → select a device → check compliance state and per-setting breakdown
3. Reports → Device compliance → Non-compliant devices — review which settings are failing
4. Entra ID → Security → Conditional Access → New policy:
   - Users: a test user or group
   - Cloud apps: Office 365
   - Conditions: Device state → Require device to be marked as compliant
   - Grant: Block access (or Allow + compliant required)
   - Note: set Report-only mode first to avoid locking yourself out
5. Test: sign in as that test user on a non-compliant device (or simulate) → observe CA block message

### Self-Check
1. A device has a compliance policy assigned. The device has been enrolled for 5 minutes. What compliance state does it show?
2. A compliance policy has a grace period of 7 days. The device has been non-compliant for 3 days. A CA policy requires compliance to access Exchange. Can the user access email?
3. A compliance policy requires BitLocker encryption. A device is not encrypted. Does Intune automatically enable BitLocker?
4. A Windows device passes all compliance policy checks. The CA policy requires the device to be Entra joined AND compliant. The device is only Intune enrolled (Workplace joined). Does CA grant access?
5. Actions for non-compliance are configured as: Day 0 = mark not compliant, Day 3 = send email, Day 7 = remotely lock. A device becomes non-compliant. When is the device locked?

### Anki Cards to Build
- Compliance policy: reports state only — does NOT enforce settings
- Grace period: non-compliant device gets grace period days before CA blocks; CA still evaluates during grace period
- Not evaluated: device just enrolled or no policy assigned yet
- Actions for non-compliance: mark not compliant → email → push notification → remote lock → retire
- Require compliant device (CA): Intune-managed + compliant state required — Workplace-joined alone insufficient
- BitLocker compliance: compliance policy marks non-compliant if not encrypted — does NOT enable BitLocker

---

## Sunday Aug 2 — REST

No study. Full rest day.

---

## Monday Aug 3 — Win32 App Deployment + M365 Apps

**Hours:** 3.5h | Big block — theory + lab

### Topics
- **Win32 app deployment overview:**
  - For apps not available in the Microsoft Store — EXE or MSI installers packaged into .intunewin format
  - IntuneWinAppUtil.exe: Microsoft packaging tool; creates .intunewin from source files
  - Command: `IntuneWinAppUtil.exe -c <source folder> -s <setup file> -o <output folder>`
  - Upload .intunewin file to Intune → configure install/uninstall commands, detection rules, requirements
- **Install and uninstall commands:**
  - MSI: `msiexec /i "App.msi" /qn` (silent install); uninstall: `msiexec /x {product-code} /qn`
  - EXE: depends on installer; use switch for silent (e.g., `/S`, `/quiet`, `/silent`)
  - Install context: **System context** (installs as SYSTEM, runs before user login — recommended for corporate apps) vs **User context** (installs per-user)
- **Detection rules** (Intune uses these to know if app is installed):
  - **MSI product code:** automatic detection using MSI GUID — simplest option for MSIs
  - **File or folder:** check for specific path and optionally version or date
  - **Registry:** check for specific registry key or value
  - **Custom script (PowerShell):** script returns exit 0 (detected) or exit non-zero (not detected)
- **Requirements rules:**
  - OS version, architecture (32-bit/64-bit), disk space, RAM, custom PowerShell script
  - Device must meet requirements before Intune attempts install
- **Supersedence and dependencies:**
  - Dependency: App A requires App B first — Intune installs B before A
  - Supersedence: App v2 replaces App v1 — Intune uninstalls v1 and installs v2
- **Assignment types:**
  - Required: force-installs on assigned devices/users
  - Available (enrolled devices): shows in Company Portal for self-service install
  - Uninstall: removes the app from assigned devices
- **M365 Apps for Enterprise:**
  - Deployed via Intune → Apps → Windows → Add → Microsoft 365 Apps (Windows 10 and later)
  - Configure: which apps (Word, Excel, etc.), architecture (64-bit preferred), update channel, languages
  - Update channels: **Current Channel** (monthly, latest features), **Monthly Enterprise Channel** (monthly, one month behind CC — predictable), **Semi-Annual Enterprise Channel** (twice a year — most stable, enterprise standard)
  - Update management: Intune can control which channel; Office Update deadlines can be set via Intune
  - Activation: Microsoft 365 license — assigned in M365 admin center; Intune deploys the software

### MS Learn Modules
- [Deploy apps by using Intune](https://learn.microsoft.com/en-us/training/modules/deploy-apps-intune/)
- [Deploy Microsoft 365 Apps with Intune](https://learn.microsoft.com/en-us/training/modules/deploy-microsoft-365-apps-intune/)

### Lab Tasks
1. Download IntuneWinAppUtil.exe → package a simple installer (e.g., Notepad++ MSI or any test MSI):
   - `IntuneWinAppUtil.exe -c "C:\source\notepadplusplus" -s "npp.installer.x64.exe" -o "C:\output"`
2. Intune → Apps → Windows → Add → Windows app (Win32) → upload .intunewin:
   - Install command: `npp.installer.x64.exe /S`
   - Uninstall command: use MSI product code or EXE uninstall switch
   - Detection rule: File — `C:\Program Files\Notepad++\notepad++.exe`
   - Assignment: Available for enrolled devices → All Users
3. Intune → Apps → Windows → Add → Microsoft 365 Apps (Windows 10 and later):
   - Select: Word, Excel, Outlook, Teams (or exclude Teams if using Teams standalone)
   - Architecture: 64-bit
   - Update channel: Monthly Enterprise Channel
   - Assign: Required → All Devices
4. Apps → Monitor → App install status → check deployment result for the Win32 app
5. Verify detection rule: on a device, manually install the app → trigger Intune sync → confirm "Installed" status

### Self-Check
1. A Win32 app has a registry detection rule pointing to `HKLM:\Software\MyApp`. The app installs to the user profile only. Will the registry detection rule fire for a machine-context install?
2. An EXE installer requires a dependency app to be installed first. How is this configured in Intune?
3. A company wants Office to receive monthly updates but needs one month's lag before rollout to test compatibility. Which update channel should they select?
4. The Intune admin uploads a Win32 app and assigns it as "Required" to All Devices. The app requires 4 GB free disk space, but the device only has 2 GB. What happens?
5. A Win32 app install fails on a device. Where in Intune would you find the installation error code?

### Anki Cards to Build
- IntuneWinAppUtil: `-c` (source) `-s` (setup file) `-o` (output) → produces .intunewin
- Win32 detection: MSI product code / File or folder / Registry / Custom PowerShell script
- Requirements rule: Intune checks BEFORE attempting install — if not met, install skipped
- Dependency: App B installed before App A automatically by Intune
- Supersedence: App v2 → uninstall v1 + install v2 in one assignment
- Current Channel: monthly, latest | Monthly Enterprise: monthly, 1-month lag | Semi-Annual: twice/year

---

## Tuesday Aug 4 — MAM + App Protection Policies

**Hours:** 2.5h | WFH — focused topic

### Topics
- **MAM (Mobile Application Management) overview:**
  - Protects org data within apps — no full device enrollment required
  - Separate from device enrollment: can apply to personal phones (BYOD) without MDM
  - Supported platforms: iOS/iPadOS, Android, Windows 11 (newer feature)
- **App Protection Policies (APP):**
  - Controls what the managed app can do with org data:
    - **Data transfer:** restrict cut/copy/paste between managed and unmanaged apps; restrict "Save As" to approved locations (SharePoint, OneDrive)
    - **Encryption:** encrypt org data within the app at rest
    - **PIN/access requirements:** require PIN or biometrics to open the managed app
    - **Conditional launch:** block compromised/rooted devices, minimum OS version, minimum app version
  - Applied to: Microsoft apps (Outlook, Teams, Edge, Word, etc.) and ISV apps with Intune SDK or App Wrapping Tool
- **Managed vs unmanaged apps:**
  - Managed app: APP assigned, data encrypted and transfer-restricted
  - Unmanaged app: no APP; org data copied to unmanaged app = data leakage risk
  - Transfer control: "Send org data to other apps" → Policy managed apps only (blocks paste to unmanaged apps)
- **Enrollment vs MAM-only:**
  - Enrolled + APP: full MDM control + data protection
  - Unenrolled + APP (MAM-WE — Without Enrollment): data protection only; no device compliance, no remote wipe of device
  - Selective wipe via APP: removes org data (account data, emails, files) from the app without wiping personal data
- **Windows MAM:**
  - For Windows 11: protect Edge for Business and other managed apps on unmanaged Windows devices
  - Requires Entra ID registration (not full MDM enrollment)
- **WIP (Windows Information Protection):**
  - Legacy Windows data protection — separates work data from personal data at the file level
  - Being deprecated; replaced by Windows MAM policies
  - Exam tip: WIP still tested; key modes: Block, Allow Override, Silent, Off

### MS Learn Modules
- [Protect apps and data on devices not enrolled in Intune](https://learn.microsoft.com/en-us/training/modules/protect-apps-not-enrolled/)
- [Manage and protect apps](https://learn.microsoft.com/en-us/training/modules/manage-protect-apps/)

### Lab Tasks
1. Intune → Apps → App protection policies → Create → iOS/iPadOS:
   - Target apps: Microsoft Outlook
   - Data protection: Save copies of org data → block; Allowed locations: SharePoint, OneDrive for Business
   - Copy/paste: Policy managed apps only
   - PIN required: Yes (4 digits)
   - Max PIN attempts: 5 (then reset)
   - Assign to: All Users (or a test group)
2. Intune → Apps → App protection policies → Create → Android:
   - Same settings as above for Outlook Android
   - Conditional launch: Max OS version = 15 (or current Android latest)
3. Test MAM on a test iOS device (if available): sign into Outlook → attempt to paste content into a personal app → verify paste is blocked
4. Apps → Monitor → App protection status → check if your policy shows "Checked in" devices
5. Review selective wipe: Devices → select a device → App selective wipe (or Apps → Monitor → App protection)

### Self-Check
1. A user has Outlook on their personal iPhone (not enrolled in MDM). An App Protection Policy is assigned to the user targeting Outlook. The user copies an email body and tries to paste it into their personal Notes app. What happens?
2. An App Protection Policy requires a PIN to open Outlook. A user sets up fingerprint instead of a PIN. Is this allowed?
3. A BYOD device with MAM (no MDM) is lost. An admin performs "selective wipe" via Intune. What is removed?
4. WIP mode is set to "Allow Override." A user tries to copy work data to a personal app. What happens?
5. A Windows 11 unmanaged device needs MAM protection for Microsoft Edge. What is required for this device before MAM policies apply?

### Anki Cards to Build
- MAM-WE (without enrollment): data protection without device MDM — BYOD scenario
- Selective wipe via APP: removes org data from the app — personal data untouched
- APP data transfer: "Policy managed apps only" → blocks paste to unmanaged apps
- Conditional launch: min OS version, block rooted/jailbroken, max PIN attempts
- WIP Allow Override: user can copy work data to personal app but action is logged
- WIP deprecated: replacing with Windows MAM policies on Windows 11

---

## Wednesday Aug 5 — MDE Integration + Endpoint Analytics

**Hours:** 2h | Office

### Topics
- **Microsoft Defender for Endpoint (MDE) integration with Intune:**
  - MDE = advanced threat detection, investigation, and response platform
  - Intune integration: Intune Security Center connector → enables device compliance signals from MDE
  - MDE onboarding via Intune: Endpoint security → Endpoint detection and response → Create policy → onboard Windows 10/11 devices
  - Once onboarded: Intune compliance policy can use "Require threat level" as a requirement:
    - No threat, Low, Medium, High — device reports its threat level to Intune; CA blocks if above threshold
  - Security settings management: manage MDE AV + firewall settings directly from Defender portal without Intune enrollment (for non-enrolled devices)
- **Endpoint analytics:**
  - Reports → Endpoint analytics — measures user experience metrics:
    - **Startup score:** time from power-on to desktop; device-level and org-wide baseline comparison
    - **App reliability score:** crash and hang rates per app
    - **Resource performance:** CPU/RAM usage patterns
    - **Restart frequency:** planned (update reboots) vs unplanned (crash reboots)
  - Requires: devices enrolled in Intune + telemetry sent to Intune (enabled via diagnostic settings)
  - Use: identify devices with poor startup times for hardware refresh prioritization
- **Windows Update compliance monitoring:**
  - Reports → Windows → Feature update failures — shows devices failing to install a feature update and the error code
  - Reports → Windows → Windows 10 and later update rings — deployment ring status across devices
  - Device details: Devices → select device → Update status — shows installed Windows version and compliance with update ring

### Lab Tasks
1. Endpoint security → Endpoint detection and response → Create policy → Windows 10 and later → onboarding
   - Setting: Onboard → Next → Assign to pilot group
2. Reports → Endpoint analytics: review startup score and app reliability score for any enrolled devices
3. Compliance → Compliance policies → select your Windows policy → Add setting: Require Microsoft Defender Advanced Threat Protection threat level: Low or above = non-compliant
4. Reports → Windows → Feature update failures: review any errors on enrolled devices (even if 0 failures — note the report exists and what columns it shows)

### Self-Check
1. A device is onboarded to MDE and has an active threat at "High" level. The compliance policy sets max allowed threat level to "Low." What compliance state does the device show?
2. Endpoint analytics startup score is 42 (low). What data does this most directly help an admin decide?
3. MDE Security settings management allows managing AV settings from the Defender portal. Does this require Intune device enrollment?
4. An admin wants to see which Windows devices failed to install the latest feature update and the error code. Where in Intune do they look?
5. Intune Endpoint analytics requires what prerequisite on devices to report startup scores?

### Anki Cards to Build
- MDE compliance condition: "Require machine risk score ≤ Low/Medium/High" — blocks high-risk devices via CA
- MDE onboarding via Intune: Endpoint security → Endpoint detection and response policy
- Endpoint analytics: Reports → Endpoint analytics; requires Intune enrollment + telemetry
- Security settings management: MDE-managed AV/firewall settings without Intune enrollment
- Feature update failures: Reports → Windows → Feature update failures — error codes per device

---

## Thursday Aug 6 — Mixed D3 Part 2 + D4 Gaps

**Hours:** 2h | Office

### Tasks
1. Revisit self-check failures from Mon–Wed (compliance states, detection rules, MAM data transfer)
2. 5 mixed practice questions:
   1. A Win32 app detection rule uses a custom PowerShell script. The script exits with code 0. What does Intune conclude?
   2. An App Protection Policy is assigned to a user on an enrolled iPhone. The user removes the Intune MDM profile. Does the APP policy still apply?
   3. A compliance policy requires BitLocker AND minimum OS 10.0.22621. A device has BitLocker but runs 10.0.19041. What is its compliance state?
   4. M365 Apps is deployed via Intune with Monthly Enterprise Channel. Microsoft releases a new Office update. Within what typical timeframe does the Monthly Enterprise Channel receive it?
   5. An admin sets supersedence from App v1 to App v2 in Intune. The device has App v1 installed. What actions does Intune take?
3. Flag any D4 topics needing deeper review → carry to Week 6 review session

---

## Friday Aug 7 — Anki + Week 5 Close

**Hours:** 1h | Office — Anki only

### Tasks
1. Full Anki review: D3 Part 2 + D4 new cards (compliance, MAM, Win32, MDE, M365 Apps)
2. Full review D3 Part 1 weak cards from Week 4 (config profiles, LAPS, BitLocker)
3. Week 5 coverage check:
   - ✅ Compliance policies: states, grace period, actions, CA integration
   - ✅ Win32 app packaging: IntuneWinAppUtil, detection rules, requirements, assignment types
   - ✅ M365 Apps deployment: channels, architecture, update management
   - ✅ MAM + App Protection Policies: BYOD data protection, selective wipe, WIP
   - ✅ MDE integration: onboarding, threat level compliance, endpoint analytics
4. Book MD-102 exam if not already done — target: Aug 18–19
