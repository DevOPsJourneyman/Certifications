# Week 4 — MD-102 Domain 3 Part 2 + Domain 4: Apps + Full Practice Exam

**Dates:** Jul 18 – Jul 24, 2026  
**Target hours:** ~21h  
**Focus:** Remote actions, device categories, Win32/MSI/Store app deployment, MAM, M365 Apps, full timed practice exam  
**Exam weight:** D3 remainder + D4 (~15–20% for D4); full exam simulation Thu

---

## Saturday Jul 18 — D3 Part 2: Remote Actions + Device Categories + Inventory
**Hours:** 5h | Big block — theory + lab

### Topics

**Remote device actions (exam-heavy — know all of them):**

| Action | What it does | Data preserved? | Use case |
|--------|-------------|----------------|----------|
| Retire | Removes MDM, removes corp data, leaves personal data | Personal yes, corp no | BYOD offboarding |
| Wipe | Factory reset, removes everything | Nothing | Lost/stolen corporate device |
| Fresh start | Reinstalls Windows, removes OEM bloat | User data optional | Refresh without reprovision |
| Autopilot reset | Returns to provisioning state | AAD join + Intune enrollment kept | Reassign to new user |
| Delete | Removes device from Intune (not the device itself) | N/A — Intune record only | Stale record cleanup |
| Sync | Forces immediate policy check-in | N/A | Policy not applying, test |
| Restart | Remote OS restart | N/A | Apply pending updates |
| Remote lock | Locks screen, PIN required to unlock | All data intact | Lost device, still trusted |
| Reset passcode | iOS/Android: clears device PIN | All data intact | User forgot PIN |
| Rotate BitLocker key | Generates new recovery key, uploads to Entra ID | All data intact | Key rotation compliance |
| Collect diagnostics | Uploads logs to Intune portal | All data intact | Troubleshoot remotely |
| Locate device | GPS location (supervised iOS + Android fully managed) | N/A | Lost device |

**Device categories:**
- Admin defines categories (e.g., "Corporate", "Kiosk", "Field Device")
- User selects category during Company Portal enrollment (configurable to skip prompt)
- Dynamic AAD group rule: `(device.deviceCategory -eq "Corporate")`
- Use: automatically assign policies/apps to devices based on category at enrollment

**Device inventory + hardware details:**
- Intune → Devices → select device: hardware, discovered apps, managed apps, compliance, configuration, update status
- Hardware inventory: model, serial, TPM version, storage, RAM, OS build, last check-in
- Discovered apps: all apps installed (from Intune managed + user installed)
- Managed apps: only Intune-deployed apps

**Bulk device actions:**
- Wipe, retire, sync, delete: can be applied to multiple devices at once via filter/export
- Bulk enrollment: Windows Configuration Designer (provisioning packages)

### MS Learn Modules
- [Perform remote actions on devices](https://learn.microsoft.com/en-us/training/modules/perform-actions-for-device-management/)
- [Monitor devices with Intune](https://learn.microsoft.com/en-us/training/modules/monitor-devices-with-intune/)

### Lab Tasks
1. Intune → Devices → select any enrolled device:
   - Review available remote actions (note which are greyed out based on platform/join type)
   - Click Sync → observe "Sync initiated" status
   - Review Hardware tab: TPM version, storage, last check-in time
2. Intune → Device categories → Create a category: "Corporate-Windows"
3. Create a dynamic AAD group: rule `(device.deviceCategory -eq "Corporate-Windows")`
4. Intune → Devices → All devices → Filter → select 2+ devices → Sync (bulk action)
5. Review "Discovered apps" on a device — note an app you didn't deploy via Intune

### Self-Check
1. **Wipe vs Retire vs Fresh start vs Autopilot reset** — match each to its use case:
   - A. Reassign a corporate laptop to a new employee without re-enrolling
   - B. Remove a former employee's personal phone from company access
   - C. A corporate laptop is stolen; wipe everything
   - D. Corporate laptop has too much OEM bloat; clean reinstall keeping Windows Hello
2. Device category is set to "Field Device" during enrollment. How does Intune use this automatically?
3. A device is deleted from Intune but not retired. The user turns the device on. What happens at next sync?
4. Remote lock is triggered on an AAD Joined Windows device. What does the user see?
5. Rotate BitLocker key action — where does the new recovery key appear after rotation?

### Anki Cards to Build
- Retire vs Wipe: personal data distinction
- Fresh start: OEM bloat gone, user data optional, AAD join kept
- Autopilot reset: MDM enrollment kept, returned to provisioning (OOBE)
- Device category → dynamic group → policy assignment chain
- Delete ≠ Wipe: delete removes Intune record; device can re-enroll

---

## Sunday Jul 19 — REST

No study. Full rest day.

---

## Monday Jul 20 — D4: Win32 App Deployment + MSI + Store Apps
**Hours:** 5h | Big block — theory + lab

### Topics

**Win32 app deployment (exam-heavy):**
- **IntuneWinAppUtil.exe**: packages app folder → `.intunewin` format; required for Win32 upload
- Install command: `setup.exe /S` or `msiexec /i app.msi /qn` — depends on installer
- Uninstall command: must be specified; different from install command
- Detection rules:
  - **File/folder**: path + file exists, or version comparison
  - **Registry**: key exists, or value comparison
  - **MSI product code**: auto-detected for MSI installers
  - **Custom script**: PowerShell returns exit code 0 = detected
- Requirement rules: minimum OS version, disk space, RAM, custom script
- **Install context**: System (runs as SYSTEM, installs for all users) vs User (runs as logged-in user)
- **Dependency**: define prerequisite apps that must install first (auto-installed if not present)
- **Supersedence**: replace or update older version of same app; old version uninstalled if configured
- Assignment types:
  - Required: mandatory install (or uninstall)
  - Available for enrolled devices: optional from Company Portal
  - Available with enrollment: MAM users see in Company Portal without MDM
- Delivery Optimization: Win32 apps cached via DO when content is available
- **Intune Management Extension (IME)**: required agent for Win32 apps, PowerShell scripts, and Win32 dependencies; auto-installed when Win32 app or PowerShell script is assigned

**MSI / Line-of-Business (LOB) apps:**
- Direct upload: .msi, .msix, .msixbundle, .appx, .appxbundle
- MSI: Intune extracts ProductCode automatically for detection
- Limited compared to Win32: no dependency, no supersedence, no requirement rules
- Install context: determined by MSI installer property

**Microsoft Store apps (new):**
- Powered by WinGet; Intune queries new Microsoft Store directly
- Search by name, add to Intune, assign — no packaging needed
- Architecture: 32-bit, 64-bit, or ARM auto-selected by Windows
- Updates: managed by Windows; not via Intune update ring

**App categories and filters:**
- App categories: tag apps for Company Portal display grouping
- Assignment filters: assign to subset of group (e.g., only Win 11 devices from an "All Devices" group)

### MS Learn Modules
- [Deploy apps using Intune](https://learn.microsoft.com/en-us/training/modules/deploy-apps-by-using-intune/)
- [Add and assign Win32 apps](https://learn.microsoft.com/en-us/training/modules/deploy-apps-by-using-intune/)

### Lab Tasks
1. Download IntuneWinAppUtil.exe from GitHub → package a sample installer:
   - Source: any folder with a setup.exe
   - Command: `IntuneWinAppUtil.exe -c <sourcefolder> -s setup.exe -o <outputfolder>`
2. Intune → Apps → Windows → Add → Windows app (Win32) → upload .intunewin:
   - Install command: `setup.exe /S`
   - Detection: registry key or file path
   - Assign as Required to a test device group
3. Add a Microsoft Store app: search "7-Zip" → assign as Available
4. Review an existing app: Apps → Windows → select app → Device install status
5. Intune → Apps → App install status report → review failure reasons (error codes)

### Self-Check
1. Win32 app fails to install. Detection rule uses "MSI product code." The MSI was recently updated and the product code changed. What happens to Intune's detection?
2. Intune Management Extension (IME) is not installed on a device. An admin assigns a Win32 app. What happens?
3. Win32 app assignment: Available vs Required — what is the user experience difference?
4. Supersedence in Win32 apps: what happens to the previous version when supersedence is configured with "Uninstall previous version"?
5. An app has a dependency configured. The primary app is assigned as Required. The dependency is only available for the same device group. What does Intune do when installing the primary app?

### Anki Cards to Build
- IntuneWinAppUtil.exe: must package before Win32 upload; .intunewin format
- IME: required for Win32 + PowerShell scripts; auto-installs on assignment
- Detection rule: custom script = exit 0 means detected; exit 1 = not detected
- Dependency: auto-installed before primary app, even if not separately assigned
- Supersedence: uninstall old version + install new; or update only

---

## Tuesday Jul 21 — D4: MAM + App Protection Policies
**Hours:** 3.5h | WFH — focused topic

### Topics

**App Protection Policies (APP) / MAM:**
- **MAM-WE** (without enrollment): no device MDM required; app-level policy only; BYOD use case
- **MAM with enrollment**: enrolled device + APP = strongest control; corporate-owned
- APP settings categories:
  - **Data relocation:** cut/copy/paste (block between managed/unmanaged apps), save to (block save to personal storage), receive data from (which apps can share data in)
  - **Access requirements:** PIN, biometric, work account credentials required to open app
  - **Conditional launch:** offline grace period, min app version, min OS version, block jailbroken/rooted, max sign-in attempts
- Managed apps (APP-aware): Outlook, Teams, Edge, Office apps, OneDrive — have Intune SDK built in
- **Selective wipe:** removes corp app data + APP policy; leaves personal data and unmanaged apps untouched
- App status reports: Device install status, User install status, App protection status

**Windows Information Protection (WIP) — legacy but on exam:**
- Protects corp data on Windows 10 devices (not Windows 11, officially deprecated)
- **Enlightened apps**: WIP-aware; can switch between personal and corp context (Office, Edge)
- **Unenlightened apps**: treated as personal; corp data cannot be pasted in
- WIP modes: Off → Silent (audit only) → Allow overrides → Block (strictest)
- WIP is NOT the same as MAM APP; WIP is for Windows, APP is for iOS/Android primarily
- Status: deprecated, but appears in MD-102 exam questions

**App configuration policies:**
- Push app settings without user interaction (e.g., pre-configure Outlook email account)
- Two types: managed devices (MDM-enrolled) vs managed apps (APP-targeted, no enrollment needed)
- Managed apps type: uses App Config channel (Intune SDK); applies even without MDM

### MS Learn Modules
- [Configure app protection policies](https://learn.microsoft.com/en-us/training/modules/configure-app-protection-policies/)
- [Implement mobile application management](https://learn.microsoft.com/en-us/training/modules/implement-mobile-application-management/)

### Lab Tasks
1. Intune → Apps → App protection policies → Create policy → iOS/iPadOS:
   - Apps: Microsoft Outlook
   - Data relocation: block "Send org data to other apps" = Policy managed apps only
   - Access: require PIN (4 digits), allow biometric
   - Conditional launch: offline grace = 720 hours (30 days); block jailbroken
   - Assign to user group (NOT device group — APP assigns to users)
2. Create a Windows APP policy (WIP — for exam familiarity):
   - Mode: Allow overrides
   - Protected apps: add Office apps
   - Assign to user group
3. App configuration policy: Intune → Apps → App configuration policies → managed apps → Outlook:
   - Pre-configure account: use Azure AD username token
4. Review app protection status: Apps → Monitor → App protection status → select user → view per-app compliance
5. Note: selective wipe — Devices → select device → Apps → select managed app → Selective wipe

### Self-Check
1. A user has Outlook on their personal iPhone with an MAM APP policy (no MDM). They leave the company. What does selective wipe remove?
2. APP is assigned to a user group. The same user has two devices: an enrolled corporate iPhone and a personal Android (not enrolled). Does APP apply to both?
3. WIP mode "Allow overrides" — what can users do that "Block" mode prevents?
4. App configuration policy: "managed apps" type vs "managed devices" type — which requires MDM enrollment?
5. An APP conditional launch rule: "Min OS version = 15.0 — Block access." A user's iPhone runs iOS 14.8. What happens when they open Outlook?

### Anki Cards to Build
- APP assigns to **users**, not devices (unlike most Intune policies)
- Selective wipe: org data + APP removed; personal apps + photos untouched
- MAM-WE: no MDM needed; app must have Intune SDK (managed app)
- WIP: Windows-only, deprecated, still on exam; Enlightened = WIP-aware app
- Conditional launch: Offline grace = days before app blocks access without connectivity

---

## Wednesday Jul 22 — D4: M365 Apps + Office Cloud Policy + Carry-Forward
**Hours:** 2.5h | Office — M365 Apps + Week 3 gaps

### Topics

**Microsoft 365 Apps deployment via Intune:**
- Intune has built-in Microsoft 365 Apps suite (no .intunewin needed)
- Configuration: select suite (Word, Excel, PowerPoint, Outlook, Teams, etc.), architecture (64-bit preferred), update channel
- **Update channels:**
  - Current Channel: monthly feature updates, latest features fastest
  - Monthly Enterprise Channel: monthly updates, one-month validation lag; predictable
  - Semi-Annual Enterprise Channel: every 6 months; most stable; enterprise default
- Assignment: Required (auto-install) or Available (user installs from Company Portal)
- App configuration: Intune Office policies override local settings for managed apps

**Office Cloud Policy service:**
- Apply Office policy settings to users without Group Policy (no domain required)
- Policies follow the user across any device (cloud-delivered, not device-specific)
- Scope: Microsoft 365 Apps for enterprise (not Office LTSC or consumer)
- Access: Microsoft 365 Apps admin center → Office cloud policy

**Licensing via Intune / Entra:**
- Microsoft 365 Apps license assigned to user in Entra ID admin center
- No license = app installs but runs in reduced functionality mode (read-only)

**Carry-forward from Week 3 (if any):**
- Review D3 items scored ≤ 3 in confidence checklist
- Close Anki gaps

### Lab Tasks
1. Intune → Apps → Windows → Add → Microsoft 365 Apps for Windows 10 and later:
   - Suite: select Office apps (exclude Project/Visio)
   - Architecture: 64-bit
   - Update channel: Monthly Enterprise
   - Assign as Required to device group
2. Review Office Cloud Policy: navigate to config.office.com → Office customization tool → policies
3. Check M365 Apps update channel in tenant: Intune → Apps → select M365 suite → Edit → update channel

### Self-Check
1. M365 Apps update channel for most enterprises wanting monthly updates with stability validation?
2. Office Cloud Policy applies to which scope: device or user?
3. A user has M365 Apps installed via Intune. Their license is removed from Entra ID. What happens to the installed apps?
4. M365 Apps via Intune vs Win32 Win32 deployment via IntuneWinAppUtil: which is simpler and does NOT require packaging?
5. Carry-forward: A macOS device enrolled via ADE has a configuration profile assigned. The profile requires the device be supervised. The device is ADE-enrolled. Will the profile apply?

### Anki Cards to Build
- M365 Apps update channels: Current (fastest) → Monthly Enterprise (stable monthly) → Semi-Annual (stable 6-monthly)
- Office Cloud Policy: user-based, cloud-delivered, no GPO needed, no domain required
- License removed → apps reduce to read-only (not uninstalled immediately)

---

## Thursday Jul 23 — FULL TIMED PRACTICE EXAM (All Domains)
**Hours:** 2.5h | Office — exam simulation

### Rules
- 65 questions, 120 minutes (actual MD-102 exam: 40–60 questions, 120 min; adjust if using MeasureUp)
- No notes, no browser, no looking anything up
- Write down question numbers you were uncertain about (not just wrong — uncertain too)

### Exam Source Options (pick one)
1. **MeasureUp MD-102** (best fidelity; paid) — most like real exam
2. **Microsoft free practice assessment** at learn.microsoft.com — free, shorter, scenario-based
3. **Whizlabs MD-102** — cheaper paid option

### Domain Coverage Target

| Domain | % of MD-102 | Questions in 65q set |
|--------|-------------|---------------------|
| D1: Deploy Windows | 25–30% | ~18 |
| D2: Manage Identity & Compliance | 25–30% | ~18 |
| D3: Manage/Protect Devices | 25% | ~16 |
| D4: Manage Applications | 15–20% | ~13 |

### Score Log

| Domain | # Correct / # Total | % |
|--------|---------------------|---|
| D1 (deploy/enrollment/WUfB) | / | |
| D2 (identity/CA/compliance/MDE) | / | |
| D3 (profiles/LAPS/remote actions) | / | |
| D4 (Win32/MAM/M365 Apps) | / | |
| **Total** | **/ 65** | |

**Pass target:** ≥ 70% (MD-102 passing score is 700/1000)  
**Strong target:** ≥ 80%

### After the Exam
- Do NOT look up answers during the exam
- After time is up: log all wrong answers + uncertain answers
- Save for Friday review

---

## Friday Jul 24 — Review Practice Exam + Book Real Exam
**Hours:** 2.5h | Office — debrief + book

### Tasks
1. For every wrong answer: write the correct rule + why you missed it + Anki card
2. For uncertain answers (right or wrong): add Anki card to reinforce
3. Identify weakest domain from score log → flag for Phase 3 focus (if Apple weeks don't absorb)
4. Full Anki deck run: all cards from Weeks 1–4

### Book Exam Dates (deadline: today)
- **MD-102 exam:** target exam week of Aug 4–8 (after 6-week plan completes Aug 3)
- **Apple Device Support (SUP-2026):** target same window or 1 week after MD-102
- Book at Pearson VUE: [home.pearsonvue.com/microsoft](https://home.pearsonvue.com/microsoft)
- Allow 3–5 business days buffer between booking and exam

### Week 4 Confidence Checklist

| Topic | Score |
|-------|-------|
| Remote actions: Retire/Wipe/Fresh start/Autopilot reset | / 5 |
| Device categories + dynamic group rules | / 5 |
| Win32: IntuneWinAppUtil, detection rules, IME | / 5 |
| Win32: Dependency vs Supersedence | / 5 |
| APP: data relocation + access + conditional launch | / 5 |
| MAM-WE vs MAM with enrollment | / 5 |
| WIP: modes + enlightened apps (legacy, exam-present) | / 5 |
| M365 Apps: update channels | / 5 |
| Office Cloud Policy: scope + delivery | / 5 |
| App configuration: managed device vs managed app type | / 5 |

---

## Week 4 Resources

| Resource | URL / Notes |
|----------|-------------|
| IntuneWinAppUtil (GitHub) | github.com/microsoft/Microsoft-Win32-Content-Prep-Tool |
| Win32 app deployment docs | learn.microsoft.com/en-us/mem/intune/apps/apps-win32-app-management |
| MAM / APP overview | learn.microsoft.com/en-us/mem/intune/apps/app-protection-policy |
| M365 Apps deployment guide | learn.microsoft.com/en-us/deployoffice/ |
| Office Cloud Policy | config.office.com |
| Pearson VUE exam booking | home.pearsonvue.com/microsoft |
| Newton topic file | `~/study-trainer/topics/md102-d4-manage-applications.md` |
| MeasureUp MD-102 | measureup.com (paid; most accurate) |

---

## Week 4 Summary (fill in on Jul 24)

- Hours logged: ___
- Anki cards built: ___
- Full practice exam score: ___ / 65 (___%)
- Weakest domain: ___
- MD-102 exam booked: Yes / No — date: ___
- Apple exam booked: Yes / No — date: ___
