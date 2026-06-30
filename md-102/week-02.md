# Week 2 — MD-102 Domain 2: Identity & Compliance

**Dates:** Jul 4 – Jul 10, 2026  
**Target hours:** ~21h  
**Focus:** Entra ID join types, Conditional Access, compliance policies, MFA/SSPR, MDE, WHfB  
**Exam weight:** ~25–30% of MD-102 (same as D1 — most-tested domain)

---

## Saturday Jul 4 — Entra Join Types + Conditional Access Foundations
**Hours:** 5h | Big block — theory + lab

### Topics
- Entra ID device join types:
  - **Azure AD Registered** — personal/BYOD; MAM or user-initiated MDM; no org credentials at OS level
  - **Azure AD Joined** — cloud-only org device; no on-prem domain; MDM enrollment automatic
  - **Hybrid Azure AD Joined** — on-prem AD joined + Entra registered; needs AAD Connect + sync
- Device identity in CA: "Device must be compliant" vs "Hybrid Azure AD joined" as grant control
- Conditional Access fundamentals:
  - Assignments: users/groups, cloud apps, conditions (sign-in risk, device platform, location, client app)
  - Grant controls: block, require MFA, require compliant device, require HAADJ, require approved app
  - Session controls: sign-in frequency, persistent browser session, app enforced restrictions
- Named locations: IP ranges, countries — used to exclude trusted networks from MFA requirement
- Report-only mode: evaluate policy impact before enforcing

### MS Learn Modules
- [Manage device identities in Azure AD](https://learn.microsoft.com/en-us/training/modules/manage-device-identities-in-azure-ad/)
- [Implement Conditional Access](https://learn.microsoft.com/en-us/training/modules/implement-conditional-access/)

### Lab Tasks
1. Entra admin center → Devices → Overview — identify join type of a device in your tenant
2. Create a Named Location: your home IP range (or office subnet), mark as trusted
3. Create CA policy #1 — **Require MFA for all users, all cloud apps**, exclude named location (trusted IP)
   - Set to Report-only first; review Sign-in logs to verify expected impact
4. Create CA policy #2 — **Require compliant device for O365 apps**, exclude break-glass account
5. Review: Sign-ins → filter by "Conditional Access" = "Success/Failure" — examine applied policies

### Self-Check
1. A personal Windows laptop is Azure AD **Registered**. A CA policy requires "compliant device." Will the user pass or be blocked?
2. What is the difference between "Require compliant device" and "Require Hybrid Azure AD joined" in a CA grant control?
3. A CA policy has no conditions set (all users, all apps, no location filter). What applies?
4. Report-only mode in CA — what does it do to existing user sign-ins?
5. A CA policy blocks access if sign-in risk is **High**. The user's Entra ID P2 license is expired. Will the risk-based condition still evaluate?

### Anki Cards to Build
- Join type comparison table (Registered / Joined / Hybrid)
- CA grant controls full list
- Named location: IP-based vs country-based
- Report-only: logs evaluation but does NOT enforce
- Entra ID P2 required for: Identity Protection risk-based CA, PIM

---

## Sunday Jul 5 — REST

No study. Full rest day.

---

## Monday Jul 6 — Compliance Policies + Identity Protection + WHfB
**Hours:** 5h | Big block — theory + lab

### Topics
- **Intune compliance policies:**
  - Settings evaluated: OS version min/max, password, encryption, Defender state, jailbreak
  - Compliance states: Compliant, Not compliant, Not evaluated, In grace period, In error
  - Grace period: device marked non-compliant but CA still grants access during period
  - Actions for noncompliance: send email, remotely lock, retire, push notification (in order of severity)
  - Default compliance: device with **no compliance policy assigned** = **compliant** (configurable)
- **Windows Hello for Business (WHfB):**
  - Trust models: Cloud Trust (preferred, requires Kerberos Cloud Trust extension), Key Trust (requires DC patch KB5014754), Certificate Trust (most complex, requires PKI)
  - Cloud Trust: easiest to deploy, no PKI, no DC patches, uses TGT from Entra ID
  - PIN: not password — doesn't leave device, backed by TPM
  - WHfB vs FIDO2: both phishing-resistant; WHfB tied to device, FIDO2 tied to security key
- **Microsoft Entra Identity Protection:**
  - Sign-in risk: real-time detection (anonymous IP, atypical travel, malicious IP, unfamiliar sign-in)
  - User risk: accumulated risk (leaked credentials, high sign-in risk pattern)
  - Policies: sign-in risk policy → require MFA; user risk policy → require password change
  - Risk remediation: admin dismiss, user self-remediate via SSPR, mark as safe
- **SSPR (Self-Service Password Reset):**
  - Methods: authenticator app, phone, email, security questions (weakest, not recommended)
  - Min methods required: 1 for reset (configurable), 2 recommended

### MS Learn Modules
- [Configure device compliance policies](https://learn.microsoft.com/en-us/training/modules/configure-device-compliance/)
- [Implement Windows Hello for Business](https://learn.microsoft.com/en-us/training/modules/implement-windows-hello-for-business/)
- [Implement Azure AD Identity Protection](https://learn.microsoft.com/en-us/training/modules/implement-azure-ad-identity-protection/)

### Lab Tasks
1. Intune → Compliance → Create policy → Windows 10 and later:
   - Min OS: 10.0.19041 (2004)
   - Password required: Yes, min 8 chars, complex
   - Encryption: Require BitLocker
   - Defender real-time protection: Require
   - Assign to "All Devices" group
2. Set noncompliance actions: Day 0 = mark non-compliant; Day 3 = email user; Day 7 = remotely lock
3. Entra admin center → Security → Identity Protection → Sign-in risk policy:
   - Risk level: Medium and above
   - Action: Require MFA
   - Set to Report-only
4. SSPR: Entra → Password reset → enable for pilot group, require 2 methods
5. WHfB: Intune → Devices → Enrollment → Windows Hello for Business → Enable, require TPM, 6-digit PIN min

### Self-Check
1. A device has no compliance policy assigned in Intune. A CA policy requires "compliant device." Can the user access resources?
2. Compliance policy grace period = 3 days. A device becomes non-compliant on Day 1. When does CA start blocking?
3. WHfB Cloud Trust vs Key Trust: which requires **no additional domain controller patches**?
4. Identity Protection: a user has a "High" user risk. The admin configures user risk policy to "require password change." The user resets their password via SSPR. What happens to their risk level?
5. SSPR is enabled for a group. A user not in that group forgets their password. They can use SSPR: True or False?

### Anki Cards to Build
- Compliance default: no policy = compliant (not non-compliant)
- Grace period: CA still grants access during period
- WHfB Cloud Trust prerequisites: Kerberos Cloud Trust extension (no PKI, no DC patch)
- Identity Protection P2 license required
- User risk remediation: password change clears risk after successful SSPR

---

## Tuesday Jul 7 — MDE Onboarding + Security Baselines
**Hours:** 3.5h | WFH — focused topic

### Topics
- **Microsoft Defender for Endpoint (MDE):**
  - Onboarding methods via Intune: configuration profile (preferred), onboarding package, security baseline
  - MDE plan tiers: MDE Plan 1 (AV, ASR, device control) vs Plan 2 (EDR, threat analytics, automated investigation)
  - Endpoint detection and response (EDR) in block mode: blocks threats even when non-Microsoft AV is primary
  - Attack Surface Reduction (ASR) rules: block Office macros from spawning child processes, block executable content from email, etc.
  - MDE security baseline: Intune-managed settings from Microsoft security benchmark
  - Threat and Vulnerability Management (TVM): device exposure score, software inventory
- **Intune security baselines:**
  - Pre-built policy templates from Microsoft security benchmark
  - Available for: Windows (MDM Security Baseline, Defender for Endpoint baseline, Edge baseline)
  - Baseline vs custom policy: baseline = opinionated defaults; custom = granular control
  - Conflicts: if baseline and config profile set same setting differently, **conflict** state — neither applies

### MS Learn Modules
- [Onboard devices with Microsoft Intune](https://learn.microsoft.com/en-us/training/modules/onboard-devices-with-microsoft-endpoint-manager/)
- [Configure Microsoft Defender for Endpoint](https://learn.microsoft.com/en-us/training/modules/configure-endpoint-security/)

### Lab Tasks
1. Intune → Endpoint security → Security baselines → MDM Security Baseline → Create profile
   - Review key settings: BitLocker, Windows Defender, SmartScreen, credential guard
   - Assign to pilot device group
2. MDE onboarding (if MDE is enabled in tenant):
   - Endpoint security → Endpoint detection and response → Create policy → Windows
   - Onboarding package: set connectivity type, sample sharing
3. Review ASR rules in Intune → Endpoint security → Attack surface reduction
   - Note: "Block Office applications from creating executable content" — Audit first, then enforce
4. Threat and Vulnerability: Microsoft Defender portal → TVM → view Exposure Score baseline

### Self-Check
1. MDE is onboarded via Intune using a configuration profile. The device shows "Onboarding failed." What Intune report should you check first?
2. EDR in block mode: when is this useful vs standard EDR?
3. A security baseline and a device configuration profile both configure Windows Defender Real-time protection, but with different values. What is the result?
4. ASR rule "Block executable content from email client and webmail" — what mode should you use in production before switching to Block?
5. MDE Plan 1 vs Plan 2: which plan includes automated investigation and response (AIR)?

### Anki Cards to Build
- MDE Plan 1 vs Plan 2 feature table
- EDR in block mode: works even when non-MS AV is primary
- Baseline + config profile conflict = conflict state, neither applies
- ASR: always Audit → Block, never Block first in production

---

## Wednesday Jul 8 — D2 Review + SSPR/MFA Deep Dive
**Hours:** 2.5h | Office — consolidation + fill gaps

### Topics
- **MFA methods** (strength order, highest to lowest):
  - FIDO2 security key (phishing-resistant)
  - Windows Hello for Business (phishing-resistant)
  - Microsoft Authenticator app — passwordless phone sign-in or TOTP
  - OATH hardware token
  - SMS / phone call (weakest, vulnerable to SIM swap)
- **Authentication strengths** (CA control): require specific MFA method (e.g., phishing-resistant MFA only)
- **MFA registration campaign**: prompts users to register without requiring it via CA

### Tasks
1. Review self-check failures from Sat–Tue (mark cards)
2. Entra → Security → Authentication methods → review enabled methods in tenant
3. CA → Authentication strengths — review built-in "Phishing-resistant MFA" strength definition
4. Run through Anki D2 deck — all cards built Mon–Tue
5. Write 10 scenario questions and self-test (no looking at notes)

### Self-Check (D2 mixed)
1. A CA policy requires "Phishing-resistant MFA." A user has MS Authenticator (TOTP) registered but no FIDO2 key. Will they pass?
2. MFA registration campaign vs CA policy requiring MFA: what is the functional difference?
3. A user risk is "Medium." The user risk policy requires password change at Medium+. The user changes password via admin reset (not SSPR). Will user risk be cleared?
4. Entra ID P2 is required for which three features relevant to MD-102?
5. A Hybrid AAD Joined device is compliant per Intune. CA policy requires "compliant device OR HAADJ." The device passes. Which control is satisfied?

### Anki Cards to Build
- MFA method strength ranking
- Authentication strength: phishing-resistant = FIDO2 or WHfB only
- User risk: password change via SSPR clears risk; admin reset does NOT

---

## Thursday Jul 9 — Practice Assessment D1+D2 (Timed)
**Hours:** 2.5h | Office — exam simulation

### Tasks
1. **30-question timed assessment** — 45 minutes, no notes, no browser
   - Source: MeasureUp MD-102 or Microsoft's free [practice assessment](https://learn.microsoft.com/en-us/certifications/practice-assessments-for-microsoft-certifications)
   - Focus on D1 + D2 scenarios (deployment, enrollment, CA, compliance, WUfB)
2. After timer: review all questions — note each wrong answer with reason
3. Score target: ≥ 70% (below 70% = more review before Week 3)

### What to Expect
- Scenario-based questions: "A company has X. Admin does Y. What happens?"
- Common traps:
  - Default compliance state (no policy = compliant, not non-compliant)
  - Autopilot: device must be pre-registered
  - CA: ALL conditions must match for policy to apply
  - Grace period: CA still grants, Intune marks non-compliant
  - DEM: no user-targeted CA, no Apple DEP
  - WHfB: Cloud Trust = no DC patch needed

### Score Log

| Category | # Correct / # Total | % |
|----------|---------------------|---|
| D1 (deploy/enrollment) | / | |
| D2 (identity/compliance/CA) | / | |
| **Total** | **/ 30** | |

---

## Friday Jul 10 — Review Practice Results + Gap Fill
**Hours:** 2.5h | Office — close the week, prep Week 3

### Tasks
1. For every wrong question from Thu: write the correct rule + Anki card
2. Review MS Learn: any module with < 80% quiz score — re-read key sections
3. Full Anki deck run: D1 + D2 (all cards)
4. Preview Week 3 topics: D3 Part 1 — Intune enrollment for macOS/iOS, config profiles, LAPS, endpoint security

### D2 Confidence Checklist

| Topic | Score |
|-------|-------|
| Entra join types (Registered/Joined/Hybrid) | / 5 |
| CA: assignments + grant controls + session controls | / 5 |
| Named locations + trusted networks | / 5 |
| Compliance policy settings + grace period | / 5 |
| Actions for noncompliance (order + timing) | / 5 |
| WHfB trust models (Cloud vs Key vs Cert) | / 5 |
| Identity Protection: sign-in risk vs user risk | / 5 |
| SSPR: methods + minimum required | / 5 |
| MDE: Plan 1 vs 2, ASR, EDR in block mode | / 5 |
| Security baseline conflicts | / 5 |

Anything ≤ 3: carry into Week 3 buffer (Wed Jul 15).

---

## Week 2 Resources

| Resource | URL / Notes |
|----------|-------------|
| MS Learn D2 path | [Configure Azure AD device identity](https://learn.microsoft.com/en-us/training/modules/manage-device-identities-in-azure-ad/) |
| Microsoft free practice assessment | [MD-102 Practice Assessment](https://learn.microsoft.com/en-us/certifications/practice-assessments-for-microsoft-certifications) |
| John Savill — Conditional Access | YouTube: "John Savill Conditional Access deep dive" |
| Entra ID Conditional Access docs | learn.microsoft.com/en-us/entra/identity/conditional-access/ |
| MDE onboarding docs | learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/ |
| Newton topic file | `~/study-trainer/topics/md102-d2-identity-compliance.md` |

---

## Week 2 Summary (fill in on Jul 10)

- Hours logged: ___
- Anki cards built: ___
- Practice assessment score: ___ / 30 (___%)
- D2 weakest area: ___
- Carry-forward to Week 3 buffer: ___
