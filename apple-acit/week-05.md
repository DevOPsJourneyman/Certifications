# Week 5 — Apple DEP-2026: ABM + ADE + Supervision + MDM Enrollment

**Dates:** Aug 1 – Aug 7, 2026
**Target hours:** ~4.5h (30% of weekly target)
**Focus:** Apple Business Manager, Automated Device Enrollment, supervision model, MDM enrollment types
**Cert:** Apple Certified IT Professional — Deployment and Management (DEP-2026)

---

## Daily Session Template

1. **Review** (5 min) — Anki cards from Weeks 1–4
2. **Read** (varies) — Apple training material or topic notes
3. **Self-check** (10 min) — 3–4 exam-style questions
4. **Anki update** (5 min) — new cards for today

---

## Hands-On Lab (Kambi's real Intune + ABM tenant, this week)

This is DEP-2026's actual lab — Kambi already runs Apple devices through Intune + ABM. Read-only observation on production; don't change assignment or settings on devices you don't own the test for.

1. In Kambi's Apple Business Manager portal: find your own account's role. Note which role(s) exist in the org (Administrator, Device Enrollment Manager, Content Manager, People Manager, MDM Server Admin, Viewer) and who holds Administrator.
2. In Kambi's Intune console: find the Apple MDM Push certificate (Devices → Enrollment → Apple → MDM Push certificate) and the Enrollment Program Token (ABM/ASM token, if you have access) — this is the link between Intune and ABM.
3. Find the 2nd MacBook Pro's device record in ABM. Confirm its current status ("Awaiting deployment") and which MDM server (Intune) it's assigned to. This device is your live example of "assigned before first power-on" — the exact zero-touch scenario the exam describes.
4. If Managed Apple IDs are in use at Kambi, check whether they're federated with Entra ID — note it either way, it's a real answer to a common exam question.

---

## Saturday Aug 1 — Apple Business Manager (ABM)

**Hours:** 1.5h

### Topics
- **ABM overview:**
  - Web-based Apple portal (business.apple.com) for org-level management of devices, users, apps, and books
  - Required for: Automated Device Enrollment (ADE), Managed Apple IDs, Volume-purchased apps (Apps and Books)
  - Cannot manage devices directly from ABM — ABM + MDM server (e.g., Jamf, Intune) work together
- **ABM roles:**
  - **Administrator:** full access — manage all settings, users, MDM servers, payments
  - **Device Enrollment Manager:** assign devices to MDM servers only
  - **Content Manager:** purchase and assign apps and books only
  - **People Manager:** create and manage Managed Apple IDs
  - **MDM Server Admin:** manage MDM server settings only
  - **Viewer:** read-only access
- **Device assignment in ABM:**
  - Devices appear in ABM when purchased directly from Apple, Apple Authorized Resellers, or added via Apple Configurator 2
  - Assign device to an MDM server in ABM: device will enroll in that MDM at next activation
  - Reassignment: move device to a different MDM server — takes effect at next activation/wipe
  - Devices can be assigned before they are ever turned on (zero-touch setup for IT)
- **Managed Apple IDs:**
  - Created and owned by the organization — not linked to personal Apple IDs
  - Allow use of Apple services (iCloud Drive, FaceTime, etc.) with work accounts
  - Federated authentication: Managed Apple IDs can be linked to Microsoft Entra ID or Google Workspace for SSO
  - Limitations vs personal Apple IDs: no Apple Pay, no App Store purchases, no iMessage (iMessage available on some configurations)
- **Apps and Books (Volume Purchase Program / VPP):**
  - Buy app licenses in volume from App Store → assign licenses to users or devices
  - User-based licensing: app syncs to user's Managed Apple ID; follows the user across devices
  - Device-based licensing: app pushed silently to device; no Apple ID required on device; used for shared/kiosk devices
  - Revoke and reassign: license can be revoked from one user/device and reassigned to another

### Self-Check
1. An org purchases 200 iPad licenses for a productivity app via Apps and Books. They want the app to install silently on shared devices without any Apple ID being signed in. Which licensing type should they use?
2. A new hire's MacBook Pro was purchased directly from Apple and assigned to the org's ABM. Before the laptop is ever powered on, can the IT team configure which MDM server it will enroll in?
3. An admin with the "Content Manager" role in ABM tries to assign a device to an MDM server. Will they succeed?
4. An employee leaves. The org revokes their Managed Apple ID. What happens to app licenses assigned to that user?
5. A Managed Apple ID is federated with Microsoft Entra ID. What does this enable for the user?

### Anki Cards to Build
- ABM: web portal for devices + managed IDs + apps/books; pairs with MDM (Jamf/Intune)
- Device-based VPP: app pushed silently to device — no Apple ID required; ideal for shared devices
- User-based VPP: follows user's Managed Apple ID across devices
- ABM Device Enrollment Manager role: assign devices to MDM servers only (no app/content access)
- Managed Apple ID: org-owned; no Apple Pay; no personal App Store; federate with Entra ID / Google
- Zero-touch ABM: device assigned to MDM in ABM before first power-on → auto-enrolls at activation

---

## Sunday Aug 2 — REST

No study. Full rest day.

---

## Monday Aug 3 — Automated Device Enrollment (ADE) + Supervision

**Hours:** 1.5h

### Topics
- **ADE (Automated Device Enrollment) — formerly DEP:**
  - Zero-touch enrollment: device activates → contacts Apple servers → checks ABM assignment → contacts MDM → downloads enrollment profile → enrolls automatically
  - Requires: device purchased through Apple/authorized reseller AND assigned to MDM in ABM
  - User experience: OOBE simplified; org can skip certain setup screens (language, region, Terms of Service)
  - MDM profile: pushed during ADE; defines supervision state, MDM configuration, which setup screens to skip
  - Skip Setup Assistant screens: IT can suppress screens (Biometric ID, iCloud sign-in, Siri, etc.) to streamline setup
- **Supervision:**
  - Supervision = enhanced MDM control mode for org-owned devices
  - How to supervise:
    - Via ADE: set "Supervised" in the MDM enrollment profile pushed through ABM
    - Via Apple Configurator 2: erase and restore device into supervised state (used for devices not in ABM)
  - Supervised-only MDM capabilities:
    - Silent app installation (no user prompt)
    - Single App Mode (lock to one app — kiosk)
    - Lost Mode (lock + display message + play sound — ADE supervised iOS)
    - Device name setting via MDM
    - Activation Lock bypass
    - Restrict app removal
    - Content Filter (web filtering via MDM)
    - Global HTTP proxy
    - Per-app VPN
  - User CANNOT remove MDM profile on a supervised device
  - Supervision state is permanent until device is wiped
- **Enrollment profile (MDM):**
  - Defined in the MDM server (e.g., Jamf or Intune)
  - Synced to ABM → linked to devices
  - Key settings: supervised (yes/no), department, support phone/email, MDM server URL, setup assistant screens to skip
  - Assignment: enrollment profile assigned to device or group in MDM, then synced to ABM
- **ADE re-enrollment:**
  - If device is wiped after ADE enrollment: device re-contacts ABM at activation → re-enrolls in same MDM automatically
  - This is the "persistent" enrollment — wipe doesn't remove it; must be released in ABM to remove

### Self-Check
1. An iPad is enrolled via ADE and set to Supervised. A user goes to Settings → General → VPN & Device Management and tries to remove the MDM profile. What happens?
2. An IT admin wants to push an app silently to 500 iPads without any user prompt. What two requirements must be met?
3. An org has 10 Mac laptops that were not purchased via Apple or an authorized reseller (e.g., bought on Amazon). Can these be added to ABM for ADE enrollment?
4. A supervised iPad is wiped (factory reset). Does the device re-enroll in MDM automatically?
5. What is the primary tool to supervise a device that was not purchased through ABM?

### Anki Cards to Build
- ADE flow: power on → Apple activation servers → ABM assignment check → MDM contact → enrollment profile → enroll
- Supervised via ADE: set in MDM enrollment profile; requires ABM assignment
- Supervised via Apple Configurator 2: for devices not in ABM; requires erase + restore
- Supervised: user CANNOT remove MDM profile; supervision persists through wipe (re-enrolls)
- Silent app install: supervised + device-based VPP OR supervised + required assignment in MDM
- Lost Mode: supervised ADE iOS only; locks device, shows message, plays sound

---

## Tuesday Aug 4 — MDM Enrollment Types + Configuration Profiles

**Hours:** 1h

### Topics
- **Three MDM enrollment types for Apple:**
  - **User Enrollment:** BYOD; uses Managed Apple ID; limited MDM access; cryptographic separation of work/personal data; user CAN remove MDM profile; MDM sees: device model, OS version, managed apps only — NOT serial number, personal apps
  - **Device Enrollment:** corporate-owned but no ABM; manual enrollment; user CAN remove MDM profile; MDM sees: full device inventory
  - **Automated Device Enrollment (ADE):** zero-touch, ABM required; supervised; user CANNOT remove MDM profile; MDM sees: full device inventory + supervision commands available
- **Configuration profiles:**
  - XML-based files (`.mobileconfig`) that deliver settings to managed Apple devices
  - Profile types: Wi-Fi, VPN, Email, Passcode, Restrictions, Certificate, Web Clip, SCEP, Single Sign-On
  - Delivery methods: MDM (pushed remotely), Apple Configurator 2 (USB/local), manual (email/web — user installs)
  - MDM-installed profiles: cannot be removed by user (on supervised devices); can be updated/removed remotely
  - User-installed profiles: user can remove; used for BYOD (User Enrollment) scenarios
- **Profile removal behavior:**
  - Supervised device: MDM profiles cannot be removed by user
  - Unsupervised (Device Enrollment or User Enrollment): user can go to Settings → VPN & Device Management → remove profile
  - MDM can remove profiles remotely regardless of supervision state
- **iCloud and data separation (User Enrollment):**
  - User Enrollment creates a separate cryptographic APFS volume for managed data
  - MDM cannot wipe personal data — only managed data (selective wipe)
  - Personal apps, photos, personal iCloud account: invisible to MDM in User Enrollment

### Self-Check
1. An iPhone is enrolled via User Enrollment. The MDM admin runs a full device wipe command. What happens?
2. A Wi-Fi profile is pushed via MDM to a supervised iPhone. The user goes to Settings and tries to delete the Wi-Fi profile. Can they?
3. What is the key difference between Device Enrollment and ADE in terms of what the user can do with the MDM profile?
4. An IT admin wants to know the serial number of a BYOD iPhone enrolled via User Enrollment. Is this visible in MDM?
5. A `.mobileconfig` profile is sent to a user via email. The user opens it and installs it. Who can remove this profile?

### Anki Cards to Build
- User Enrollment: BYOD; Managed Apple ID required; no serial number in MDM; no full wipe; user can remove MDM
- Device Enrollment: corporate; manual; full inventory; user CAN remove MDM profile
- ADE: zero-touch; supervised; user CANNOT remove MDM; full inventory; supervision commands
- User Enrollment selective wipe: removes managed APFS volume (work data) — personal data untouched
- MDM-pushed profiles (supervised): user cannot remove; MDM can remove remotely
- User-installed profiles: user can remove in Settings → VPN & Device Management

---

## Wednesday Aug 5 — Review + Self-Check

**Hours:** 0.5h | Office

### Tasks
1. Anki review: all Apple Week 5 new cards
2. 3 questions:
   - An org uses ABM with Jamf Pro as their MDM. A MacBook Pro is assigned to Jamf in ABM. The user wipes the Mac via Recovery Mode (erase all content). What happens when the Mac boots up?
   - An admin wants to set up a supervised kiosk iPad locked to one app. Which MDM feature does this use, and what enrollment type is required?
   - A Managed Apple ID is created in ABM. What identity federation option allows employees to use their corporate password for this ID?

---

## Thursday Aug 6 — Review + Self-Check

**Hours:** 0.5h | Office

### Tasks
1. Revisit gaps from previous days
2. 3 questions:
   - An IT admin creates an enrollment profile in Jamf/Intune that sets supervised = Yes. The device is assigned to the MDM server in ABM. At enrollment, is the device automatically supervised?
   - An enterprise deploys 1000 iPhones. Half are shared devices (no persistent user), half are personal-use corporate phones. Which VPP licensing model suits each group?
   - A Content Manager in ABM wants to add a new MDM server to the org's ABM account. Are they able to do this?

---

## Friday Aug 7 — Anki Review + DEP-2026 Progress Check

**Hours:** 0.5h | Office — Anki only

### Tasks
1. Full review: Apple Weeks 1–5 Anki decks
2. DEP-2026 coverage check:
   - ✅ ABM: portal roles, device assignment, managed Apple IDs, Apps and Books (Week 5)
   - ✅ ADE: zero-touch flow, enrollment profile, setup assistant skipping (Week 5)
   - ✅ Supervision: how to supervise, supervised-only capabilities, MDM profile removal (Week 5)
   - ✅ Enrollment types: User Enrollment vs Device Enrollment vs ADE (Week 5)
   - ✅ Configuration profiles: types, delivery, removal behavior (Week 5)
   - 🔜 Week 6: Config profiles deep-dive, DDM, VPP management, final review
3. If the 2nd MacBook Pro has deployed this week, note what actually happened vs the ADE flow on paper — that comparison is worth more than another self-check.
