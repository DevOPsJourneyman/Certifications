# Week 1 — MD-102 Domain 1: Deploy Windows Client

**Dates:** Jul 4 – Jul 10, 2026
**Target hours:** ~10.5h (70% of weekly target)
**Focus:** Windows deployment methods, Autopilot, Intune enrollment, WUfB, co-management
**Exam weight:** ~25–30% of MD-102

---

## Daily Session Template

1. **Review** (10 min) — Anki cards from previous session
2. **Read** (varies) — topic notes + MS Learn module
3. **Lab** (varies) — hands-on in dev tenant
4. **Self-check** (15 min) — 4–5 exam-style questions; mark failures
5. **Anki update** (10 min) — add cards for today's exam gotchas

---

## Saturday Jul 4 — Deployment Methods + Autopilot

**Hours:** 3.5h | Big block — theory + lab

### Topics
- Deployment method comparison: MDT, WDS, OSD (ConfigMgr), Windows Autopilot
- Autopilot modes: user-driven (AAD join + hybrid), self-deploying, pre-provisioning (white glove)
- OOBE flow + Enrollment Status Page (ESP)
- Autopilot device registration: hardware hash, CSV import, OEM partner upload
- Deployment profiles: skip EULA, skip privacy settings, skip account setup

### MS Learn Modules
- [Deploy Windows with Microsoft Intune](https://learn.microsoft.com/en-us/training/modules/deploy-windows-with-intune/)
- [Set up Windows Autopilot](https://learn.microsoft.com/en-us/training/modules/set-up-windows-autopilot/)

### Lab Tasks
1. **HP laptop #1 (real hardware):** reset to OOBE if needed, then extract the real hardware hash — `Install-Script Get-WindowsAutoPilotInfo; Get-WindowsAutoPilotInfo -OutputFile hash.csv` — and import it into Devices → Enrollment → Windows Autopilot
2. Create Autopilot deployment profile: user-driven, AAD join, skip EULA = Yes, skip privacy = Yes — assign to the HP device
3. Create Enrollment Status Page profile: block device use until required apps installed
4. **Run the enrollment for real:** boot the HP through OOBE → watch Autopilot pull the profile → observe every ESP phase (device prep / device setup / account setup). This end-to-end run is worth more than any number of console screenshots — the exam's performance-based questions assume you've seen it.
5. Verify profile assignment in Devices → Enrollment → Deployment profiles; find the enrolled HP under Devices → Windows

### Self-Check
1. A technician runs pre-provisioning (white glove). What does the **technician phase** accomplish before the device reaches the end user?
2. Which Autopilot mode requires **no user interaction** and is suited for shared kiosks?
3. An Autopilot profile is assigned to a device group. The device registers at OOBE but skips Autopilot and goes to standard setup. What is the most likely cause?
4. MDT vs Autopilot: which requires a Windows PE boot image?
5. What PowerShell command generates the hardware hash for Autopilot registration?

### Anki Cards to Build
- Autopilot modes + use cases (user-driven / self-deploying / pre-provisioning)
- ESP: what it blocks, when it releases
- White glove: technician phase vs user phase
- Hardware hash: `Get-WindowsAutoPilotInfo`

---

## Sunday Jul 5 — REST

No study. Full rest day.

---

## Monday Jul 6 — Intune Enrollment + Hybrid Join

**Hours:** 3.5h | Big block — theory + lab

### Topics
- Enrollment types: BYOD (MAM-WE, user-initiated MDM) vs corporate-owned (Autopilot, bulk, DEM)
- Enrollment restrictions: platform, OS version min/max, personally-owned block
- Device Enrollment Manager (DEM): use case, 1000-device limit, limitations (no Autopilot, no Apple DEP)
- Bulk enrollment: Windows Configuration Designer, provisioning packages
- Autopilot hybrid Entra join: prerequisites (Intune Connector for AD, line of sight to DC)
- Device cleanup rules + stale device management

### MS Learn Modules
- [Enroll devices with Microsoft Intune](https://learn.microsoft.com/en-us/training/modules/enroll-devices-with-microsoft-intune/)
- [Configure device enrollment](https://learn.microsoft.com/en-us/training/modules/configure-device-enrollment/)

### Lab Tasks
1. Create enrollment restriction: block personally-owned Windows devices, allow corporate only
2. Create second restriction at lower priority: allow all platforms for a pilot group
3. Review DEM accounts — note what features are unavailable for DEM-enrolled devices
4. Create Hybrid Entra join Autopilot profile (note: Intune Connector prerequisite)
5. Device cleanup: Devices → Device cleanup rules → review 90-day inactive threshold (read-only)

### Self-Check
1. A DEM account tries to enroll a macOS device via Apple Business Manager. Will this work? Why or why not?
2. Enrollment restrictions have two types: device type and **\_\_\_\_\_** restrictions.
3. A user-initiated MDM enrollment registers a personal device. What is the primary compliance check Intune uses to gate access?
4. Hybrid Entra join via Autopilot fails with "Domain Join failed." What is the first component to verify?
5. Bulk enrollment uses a provisioning package created in which tool?

### Anki Cards to Build
- DEM limitations: no Autopilot, no Apple DEP, no CA targeting
- Enrollment restriction priority: lower number = higher priority
- Hybrid join prerequisite: Intune Connector for AD + DC reachable at enrollment time
- MAM-WE: no MDM enrollment, app-level policy only

---

## Tuesday Jul 7 — WUfB Update Rings

**Hours:** 2.5h | WFH — focused topic

### Topics
- WUfB: quality updates vs feature updates, deferral periods
- Quality max deferral: 30 days | Feature max deferral: 365 days
- Update rings: semi-annual channel, deadline-driven installs, active hours
- Delivery Optimization (DO) download modes: 0=HTTP only, 1=LAN peer, 2=group, 3=internet peer, 99=simple, 100=bypass
- WUfB reporting: Windows Update compliance report, feature update report
- WUfB vs Windows Autopatch (Autopatch = Microsoft-managed rings)

### MS Learn Modules
- [Manage Windows updates with Intune](https://learn.microsoft.com/en-us/training/modules/manage-windows-updates-with-intune/)

### Lab Tasks
1. Create update ring: quality deferral 7d, feature deferral 30d, active hours 8am–10pm, deadline 5d, restart warning 15 min
2. Assign ring to "Pilot-Devices" group
3. Create second ring for broad deployment: quality 14d, feature 60d
4. Check compliance: Devices → Monitor → Windows Update compliance report
5. Set Delivery Optimization download mode to LAN (1) via config profile

### Self-Check
1. Quality update maximum deferral days in WUfB?
2. Feature update maximum deferral days in WUfB?
3. A device has active hours 8am–10pm. An update requires restart. When will the restart occur?
4. Delivery Optimization download mode **1** = ?
5. Windows Autopatch differs from manually configured WUfB rings in what key operational way?

### Anki Cards to Build
- Quality deferral: max 30 days
- Feature deferral: max 365 days
- DO download modes: 0=HTTP only, 1=LAN, 2=group, 3=internet peer, 99=simple, 100=bypass
- Autopatch = Microsoft-managed rings, no manual ring config required

---

## Wednesday Jul 8 — Co-Management

**Hours:** 2h | Office — focused topic

### Topics
- Co-management: Intune + ConfigMgr managing same device simultaneously
- Cloud attach vs co-management: cloud attach = CMG + tenant attach only; co-management = workload sharing
- Co-management workloads (7): Compliance Policies, Conditional Access, Resource Access, Endpoint Protection, Device Configuration, Office Click-to-Run, Windows Update Policies
- Pilot collections: workload moved to Intune only for pilot group
- Prerequisites: ConfigMgr 1710+, Windows 10 1709+, Entra hybrid join or AAD join

### MS Learn Modules
- [Implement co-management](https://learn.microsoft.com/en-us/mem/configmgr/comanage/overview)

### Lab Tasks (conceptual — no ConfigMgr in dev tenant)
1. Draw co-management workload table: list all 7 workloads + default owner (ConfigMgr)
2. Note prerequisites and cloud attach vs co-management distinction
3. Read the co-management overview doc — focus on enrollment path and workload slider behavior

### Self-Check
1. Co-management requires which minimum ConfigMgr version?
2. Which co-management workload controls Windows Update policy ownership?
3. A pilot collection has the "Compliance Policies" workload set to Intune. Devices not in the pilot collection use which tool?
4. Cloud attach (tenant attach) is a prerequisite for co-management: True or False?
5. A device is co-managed. The admin moves the "Endpoint Protection" workload slider to Intune. What happens to ConfigMgr's AV policies on that device?

### Anki Cards to Build
- Co-management prerequisites (ConfigMgr 1710+, Win10 1709+)
- 7 workloads: all start at ConfigMgr, can slide to Intune per workload
- Cloud attach ≠ co-management (cloud attach is a subset)

---

## Thursday Jul 9 — D1 Gap Fill + Mixed Practice

**Hours:** 2h | Office — review + self-test

### Topics
- Revisit any self-check failures from Sat–Wed
- Autopilot pre-provisioning deep-dive if needed
- WUfB deadline + grace period mechanics
- Co-management enrollment path (ConfigMgr auto-enroll existing clients to Intune)

### Lab Tasks
1. Pull any outstanding lab tasks from earlier in the week
2. Intune → Devices → review enrolled devices — note enrollment type shown
3. Check update ring compliance report — any devices showing "In grace period"?

### Self-Check (mixed D1)
1. An Autopilot deployment profile has "Skip EULA" = No. What does the OOBE show at first boot?
2. A WUfB update ring has quality deferral 14d and deadline 5d. An update releases on Day 1. When must the device install it at the latest?
3. DEM account limit: how many devices can a single DEM account enroll?
4. What is the Intune Connector for AD used for in Autopilot hybrid join?
5. Delivery Optimization mode 3 = internet peer. Who provides the content in this mode?

### Anki Cards to Build
- WUfB math: deferral start = update release date; deadline starts after deferral ends
- DEM limit: 1000 devices per DEM account

---

## Friday Jul 10 — Anki Review + Week Close

**Hours:** 1h | Office — Anki only, no new content

### Tasks
1. Review all D1 Anki cards built this week — mark hard cards for re-review
2. Note shaky topics — flag for Week 3 catchup buffer
3. Confirm dev tenant lab tasks completed: update ring, enrollment restriction, Autopilot profile
