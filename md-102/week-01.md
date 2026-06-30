# Week 1 — MD-102 Domain 1: Deploy Windows Client

**Dates:** Jun 27 – Jul 3, 2026  
**Target hours:** ~21h  
**Focus:** Windows deployment methods, Autopilot, Intune enrollment, WUfB, co-management  
**Exam weight:** ~25–30% of MD-102

---

## Daily Session Template

1. **Review** (10 min) — Anki cards from previous session
2. **Read** (varies) — topic notes + MS Learn module
3. **Lab** (varies) — hands-on in dev tenant or Newton VM
4. **Self-check** (15 min) — 4–5 exam-style questions; mark failures
5. **Anki update** (10 min) — add cards for today's exam gotchas

---

## Saturday Jun 27 — Deployment Methods + Autopilot Fundamentals
**Hours:** 5h | Big block — theory + lab

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
1. In dev tenant → Devices → Enrollment → Windows Autopilot → import a device (use PowerShell `Get-WindowsAutoPilotInfo` output or CSV with fake hash for lab)
2. Create an Autopilot deployment profile: user-driven, AAD join, skip EULA = Yes, skip privacy = Yes
3. Create an Enrollment Status Page profile: block device use until required apps installed
4. Review: Devices → Enrollment → Deployment profiles — verify assignment

### Self-Check
1. A technician runs pre-provisioning (white glove). What does the **technician phase** accomplish before the device reaches the end user?
2. Which Autopilot mode requires **no user interaction** and is suited for shared kiosks?
3. An Autopilot profile is assigned to a device group. The device registers at OOBE but skips Autopilot and goes to standard setup. What is the most likely cause?
4. MDT vs Autopilot: which requires a Windows PE boot image?
5. What is the maximum number of devices per Autopilot group tag filter in Intune?

### Anki Cards to Build
- Autopilot modes + use cases
- ESP: what it blocks, when it releases
- White glove: technician phase vs user phase
- Hardware hash: how obtained (`Get-WindowsAutoPilotInfo`)

---

## Sunday Jun 28 — REST

No study. Full rest day.

---

## Monday Jun 29 — Intune Enrollment + Autopilot Hybrid Join
**Hours:** 5h | Big block — theory + lab

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
2. Create a second restriction at lower priority: allow all platforms for a pilot group
3. Review DEM accounts — note what features are unavailable for DEM-enrolled devices
4. In Autopilot → Profiles → create a Hybrid Azure AD join profile (note: Intune Connector prerequisite)
5. Device cleanup: Devices → Device cleanup rules → set 90-day inactive threshold (review only, don't enable in prod tenant)

### Self-Check
1. A DEM account tries to enroll a macOS device via Apple Business Manager. Will this work? Why or why not?
2. Enrollment restrictions have two types: device type and **\_\_\_\_\_** restrictions.
3. A user-initiated MDM enrollment registers a personal device. What is the primary compliance check Intune uses to gate access (hint: MAM vs MDM)?
4. Hybrid Entra join via Autopilot fails with "Domain Join failed." What is the first component to verify?
5. Bulk enrollment uses a provisioning package created in which tool?

### Anki Cards to Build
- DEM limitations (no Autopilot, no Apple DEP, no conditional access targeting)
- Enrollment restriction priority: lower number = higher priority
- Hybrid join prerequisite: Intune Connector for AD + DC reachable
- MAM-WE: no MDM enroll, app-level policy only

---

## Tuesday Jun 30 — WUfB Update Rings ← TODAY
**Hours:** 3.5h | WFH session — focused topic

### Topics
- Windows Update for Business (WUfB): quality updates vs feature updates
- Deferral periods: quality max 30 days, feature max 365 days
- Update rings: semi-annual channel, deadline-driven installs, active hours
- Delivery Optimization (DO): download mode (HTTP only, LAN peer, internet peer, group, simple, bypass)
- WUfB reporting: Windows Update compliance report, feature update report
- WUfB vs Windows Autopatch (Autopatch = managed WUfB rings)

### MS Learn Modules
- [Manage Windows updates with Intune](https://learn.microsoft.com/en-us/training/modules/manage-windows-updates-with-intune/)
- [Configure Windows Update for Business](https://learn.microsoft.com/en-us/training/modules/configure-windows-update-for-business/)

### Lab Tasks
1. Intune → Devices → Windows → Update rings → create ring:
   - Quality update deferral: 7 days
   - Feature update deferral: 30 days
   - Active hours: 8am–10pm
   - Deadline: 5 days after deferral
   - Auto-restart: 15 min warning
2. Assign ring to a device group (e.g., "Pilot-Devices")
3. Check compliance: Devices → Monitor → Windows Update compliance report
4. Create second ring for broad deployment: quality = 14d, feature = 60d
5. Review Delivery Optimization settings: set download mode to LAN (1)

### Self-Check
1. Quality update maximum deferral days in WUfB?
2. Feature update maximum deferral days in WUfB?
3. A device in an update ring has active hours set 8am–10pm. Windows needs to restart to apply an update. When will the restart occur?
4. Delivery Optimization download mode **1** = ?
5. Windows Autopatch differs from manually configured WUfB rings in what key operational way?

### Anki Cards to Build
- Quality deferral: max 30 days
- Feature deferral: max 365 days
- DO download modes: 0=HTTP only, 1=LAN, 2=group, 3=internet peer, 99=simple, 100=bypass
- Autopatch = Microsoft-managed update rings, no manual ring config required

---

## Wednesday Jul 1 — Co-Management
**Hours:** 2.5h | Office — focused review + notes

### Topics
- Co-management: Intune + ConfigMgr managing same device simultaneously
- Cloud attach (tenant attach): prerequisite for co-management
- Co-management workload sliders:
  - Compliance policies, Conditional Access, Resource Access, Endpoint Protection, Device Configuration, Office Click-to-Run, Windows Update policies
- Pilot collections: workload moved to Intune only for pilot group
- Co-management enrollment path: ConfigMgr auto-enrolls existing clients to Intune

### MS Learn Modules
- [Implement co-management](https://learn.microsoft.com/en-us/training/modules/implement-co-management/)

### Lab Tasks (conceptual — no ConfigMgr in dev tenant)
1. Draw the co-management workload table: list all 7 workloads + which tool owns each (default = ConfigMgr)
2. Note prerequisites: ConfigMgr 1710+, clients on Windows 10 1709+, Entra ID hybrid join or AAD join
3. Read: [Co-management overview](https://learn.microsoft.com/en-us/mem/configmgr/comanage/overview)
4. Note the cloud attach vs co-management distinction: cloud attach = CMG + tenant attach only; co-management = workload sharing

### Self-Check
1. Co-management requires which minimum ConfigMgr version?
2. A device is co-managed. The Compliance policies workload is set to Intune. Where does the compliance policy come from?
3. Which co-management workload, if moved to Intune, affects Conditional Access decision-making immediately?
4. Can a co-managed device have its Windows Update workload split between ConfigMgr for pilot and Intune for production? (Yes/No + why)
5. Cloud attach (tenant attach) allows what capabilities WITHOUT full co-management?

### Anki Cards to Build
- Co-management workloads list (7)
- Pilot collection: workload moved to Intune for subset only
- Cloud attach ≠ co-management (no workload sharing, just visibility + remote actions)
- ConfigMgr min version: 1710

---

## Thursday Jul 2 — D1 Review + Exam-Style Practice
**Hours:** 2.5h | Office — consolidation

### Tasks
1. Review self-check failures from Mon–Wed (check your notes)
2. Close Anki gaps: review all cards built this week, edit weak ones
3. Run 15-question D1 practice set (MeasureUp or Microsoft practice assessment)
4. For each wrong answer: write the correct rule as an Anki card

### Focus Areas (common D1 exam traps)
- ESP: difference between "block until required apps installed" vs "track only"
- Autopilot: device must be registered in Intune **before** it reaches OOBE — not at OOBE
- WUfB: deferral + deadline stack (device waits deferral days, then has deadline days to comply)
- DEM: no targeting by user (no CA, no user-based compliance), 1000 device cap
- Hybrid join: requires line of sight to DC during OOBE — VPN or on-prem network

### Self-Check (D1 mixed)
1. An Autopilot device fails at the ESP with "Something went wrong." The error code is 0x800705B4 (timeout). What does this typically indicate?
2. A WUfB ring has quality deferral = 10 days, deadline = 2 days. An update is released on Day 0. When is the device forced to restart?
3. Enrollment restriction blocks personally-owned Android devices. A user tries to enroll their corporate-owned Android via work profile. Will this succeed?
4. DEM account enrolls 1001 devices. What happens to device 1001?
5. An Autopilot profile is set to "Hybrid Azure AD join." The Intune Connector for AD is installed but the device fails to join. What is the most common next check?

---

## Friday Jul 3 — D1 Final Review + Anki Deck Run
**Hours:** 2.5h | Office — close the week

### Tasks
1. Full Anki deck review — all cards from Week 1
2. Review MD-102 exam blueprint D1 weight: 25–30%
3. Check MS Learn D1 learning path completion status
4. Write 3–5 sentences summarising D1 for yourself: what you know cold, what still feels fuzzy
5. Preview Week 2 topics: Entra join types, Conditional Access, compliance policies

### D1 Confidence Checklist
Rate each 1–5 (1 = need more work, 5 = exam-ready):

| Topic | Score |
|-------|-------|
| Autopilot modes + when to use each | / 5 |
| Autopilot hybrid join prerequisites | / 5 |
| ESP configuration + error handling | / 5 |
| Enrollment restrictions + priority | / 5 |
| DEM use case + limitations | / 5 |
| WUfB ring config + deferral math | / 5 |
| Delivery Optimization download modes | / 5 |
| Co-management workloads + sliders | / 5 |
| Cloud attach vs co-management | / 5 |

Anything scored ≤ 3: add to Week 2 buffer session.

---

## Week 1 Resources

| Resource | URL / Notes |
|----------|-------------|
| MS Learn MD-102 path | [MD-102 Learning Path](https://learn.microsoft.com/en-us/certifications/exams/md-102) |
| John Savill — MD-102 Study Cram | Search YouTube: "John Savill MD-102" |
| MeasureUp practice tests | measureup.com — MD-102 (paid, most accurate) |
| Whizlabs | whizlabs.com — cheaper alternative |
| Newton topic file | `~/study-trainer/topics/md102-d1-deploy-windows.md` |
| Dev tenant | M365 dev tenant — enrolled via MSDN |

---

## Week 1 Summary (fill in on Jul 3)

- Hours logged: ___
- Anki cards built: ___
- Practice assessment score: ___
- D1 weakest area: ___
- Carry-forward to Week 2 buffer: ___
