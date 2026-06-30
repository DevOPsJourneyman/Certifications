# Week 2 — MD-102 Domain 2: Identity & Compliance

**Dates:** Jul 11 – Jul 17, 2026
**Target hours:** ~10.5h (70% of weekly target)
**Focus:** Entra join types, Conditional Access, compliance policies, WHfB, MDE, SSPR
**Exam weight:** ~25–30% of MD-102 (same weight as D1 — most-tested domain)

---

## Daily Session Template

1. **Review** (10 min) — Anki cards from previous session
2. **Read** (varies) — topic notes + MS Learn module
3. **Lab** (varies) — hands-on in dev tenant
4. **Self-check** (15 min) — 4–5 exam-style questions; mark failures
5. **Anki update** (10 min) — add cards for today's exam gotchas

---

## Saturday Jul 11 — Entra Join Types + Conditional Access Foundations

**Hours:** 3.5h | Big block — theory + lab

### Topics
- Entra ID device join types:
  - **Azure AD Registered** — personal/BYOD; MAM or user-initiated MDM; no org credentials at OS level
  - **Azure AD Joined** — cloud-only org device; no on-prem domain; MDM enrollment automatic
  - **Hybrid Azure AD Joined** — on-prem AD joined + Entra registered; requires Entra Connect sync
- Device identity in CA: "Require compliant device" vs "Require Hybrid Azure AD joined" as grant controls
- Conditional Access fundamentals:
  - Assignments: users/groups, cloud apps, conditions (sign-in risk, device platform, location, client app)
  - Grant controls: block, require MFA, require compliant device, require HAADJ, require approved app
  - Session controls: sign-in frequency, persistent browser session, app enforced restrictions
- Named locations: IP ranges or countries — used to exclude trusted networks from MFA
- Report-only mode: evaluate policy impact before enforcing

### MS Learn Modules
- [Manage device identities in Azure AD](https://learn.microsoft.com/en-us/training/modules/manage-device-identities-in-azure-ad/)
- [Implement Conditional Access](https://learn.microsoft.com/en-us/training/modules/implement-conditional-access/)

### Lab Tasks
1. Entra admin center → Devices → Overview — identify join type of a device in your tenant
2. Create Named Location: your home IP range (or office subnet), mark as trusted
3. Create CA policy #1 — Require MFA for all users, all cloud apps, exclude named location — set Report-only first
4. Create CA policy #2 — Require compliant device for O365 apps, exclude break-glass account
5. Sign-ins → filter by "Conditional Access" = Success/Failure — examine applied policies

### Self-Check
1. A personal Windows laptop is Azure AD **Registered**. A CA policy requires "compliant device." Will the user pass or be blocked?
2. What is the difference between "Require compliant device" and "Require Hybrid Azure AD joined" in a CA grant?
3. A CA policy has no conditions set (all users, all apps, no location filter). What is the scope of enforcement?
4. Report-only mode in CA — what does it do to existing user sign-ins?
5. A CA policy blocks access if sign-in risk is High. The user's Entra ID P2 license is expired. Will the risk-based condition still evaluate?

### Anki Cards to Build
- Join type comparison: Registered (personal) / Joined (cloud-only corp) / Hybrid (on-prem + Entra)
- CA grant controls: block, MFA, compliant device, HAADJ, approved app, app protection policy
- Named location: IP-based vs country-based
- Report-only: evaluates and logs — does NOT enforce
- Entra ID P2 required for: risk-based CA, PIM, Identity Protection

---

## Sunday Jul 12 — REST

No study. Full rest day.

---

## Monday Jul 13 — Compliance Policies + WHfB + Identity Protection

**Hours:** 3.5h | Big block — theory + lab

### Topics
- **Intune compliance policies:**
  - Settings evaluated: OS version min/max, password, encryption, Defender state, jailbreak
  - Compliance states: Compliant, Not compliant, Not evaluated, In grace period, In error
  - Grace period: device marked non-compliant but CA still grants access during period
  - Actions for noncompliance: send email, remotely lock, retire, push notification
  - Default compliance: device with **no compliance policy assigned** = **compliant** (configurable)
- **Windows Hello for Business (WHfB):**
  - Trust models: Cloud Trust (preferred), Key Trust (requires KB5014754 on DCs), Certificate Trust (PKI required)
  - Cloud Trust: easiest — no PKI, no DC patches, uses Kerberos Cloud Trust extension
  - PIN: not a password — backed by TPM, doesn't leave device
  - WHfB vs FIDO2: both phishing-resistant; WHfB tied to device, FIDO2 tied to hardware key
- **Microsoft Entra Identity Protection:**
  - Sign-in risk: real-time (anonymous IP, atypical travel, malicious IP, unfamiliar sign-in)
  - User risk: accumulated (leaked credentials, high sign-in risk pattern)
  - Policies: sign-in risk policy → require MFA; user risk policy → require password change
  - Risk remediation: admin dismiss, user self-remediate via SSPR, mark as safe
- **SSPR:**
  - Methods: authenticator app, phone, email, security questions (weakest)
  - Min methods required: configurable (1 minimum, 2 recommended)

### MS Learn Modules
- [Configure device compliance policies](https://learn.microsoft.com/en-us/training/modules/configure-device-compliance/)
- [Implement Windows Hello for Business](https://learn.microsoft.com/en-us/training/modules/implement-windows-hello-for-business/)
- [Implement Azure AD Identity Protection](https://learn.microsoft.com/en-us/training/modules/implement-azure-ad-identity-protection/)

### Lab Tasks
1. Compliance policy → Windows 10 and later: min OS 10.0.19041, password required (8 chars, complex), require BitLocker, require Defender real-time — assign to All Devices
2. Set noncompliance actions: Day 0 = mark non-compliant; Day 3 = email user; Day 7 = remotely lock
3. Entra → Security → Identity Protection → Sign-in risk policy: Medium and above → require MFA → Report-only
4. SSPR: Entra → Password reset → enable for pilot group, require 2 methods
5. WHfB: Intune → Devices → Enrollment → Windows Hello for Business → Enable, require TPM, 6-digit PIN min

### Self-Check
1. A device has no compliance policy assigned. A CA policy requires "compliant device." Can the user access resources?
2. Compliance grace period = 3 days. A device becomes non-compliant on Day 1. When does CA start blocking?
3. WHfB Cloud Trust vs Key Trust: which requires **no additional domain controller patches**?
4. Identity Protection: user has High user risk, user risk policy set to require password change. User resets via SSPR. What happens to their risk level?
5. SSPR is enabled for Group A. A user not in Group A forgets their password. Can they use SSPR?

### Anki Cards to Build
- Compliance default: no policy = compliant (NOT non-compliant)
- Grace period: CA still grants access during grace period
- WHfB Cloud Trust prerequisites: Kerberos Cloud Trust extension only (no PKI, no DC patch)
- Identity Protection requires Entra ID P2
- User risk remediation: successful SSPR password change clears High user risk

---

## Tuesday Jul 14 — MDE + Intune Security Baselines

**Hours:** 2.5h | WFH — focused topic

### Topics
- **Microsoft Defender for Endpoint (MDE):**
  - Onboarding via Intune: configuration profile (preferred), onboarding package, security baseline
  - MDE Plan 1 vs Plan 2: Plan 1 = AV + ASR + device control; Plan 2 = EDR + threat analytics + automated investigation
  - EDR in block mode: blocks threats even when non-Microsoft AV is primary AV
  - Attack Surface Reduction (ASR) rules: block Office macros from child processes, block executable content from email
  - Threat and Vulnerability Management (TVM): device exposure score, software inventory
- **Intune security baselines:**
  - Pre-built policy templates from Microsoft security benchmark
  - Available for: MDM Security Baseline, Defender for Endpoint baseline, Edge baseline
  - Baseline vs custom policy: baseline = opinionated defaults; custom = granular control
  - Conflicts: baseline + config profile set same setting differently → **conflict** state — neither applies

### MS Learn Modules
- [Protect devices with Microsoft Defender for Endpoint](https://learn.microsoft.com/en-us/training/modules/protect-devices-with-microsoft-defender-for-endpoint/)
- [Use security baselines in Intune](https://learn.microsoft.com/en-us/training/modules/use-security-baselines-intune/)

### Lab Tasks
1. Intune → Endpoint security → Security baselines → MDM Security Baseline → create profile, assign to pilot group
2. Endpoint security → Antivirus → create Microsoft Defender Antivirus policy: real-time protection = enabled, cloud-delivered protection = enabled
3. Endpoint security → Attack surface reduction → create ASR rules policy: set key rules to Audit mode first
4. Review: Endpoint security → Microsoft Defender for Endpoint → MDE connector status
5. Reports → Endpoint security → Antivirus → check for non-compliant devices

### Self-Check
1. MDE is licensed separately from Intune. Which Microsoft 365 SKU bundles MDE Plan 2?
2. EDR in block mode blocks threats on a device running a third-party AV as the primary. Does MDE need to be the primary AV for EDR in block mode to work?
3. A security baseline setting and a device configuration profile set the same setting to different values. What is the resulting compliance state?
4. ASR rule "Block Office applications from creating executable content" — what attack pattern does this address?
5. TVM exposure score: what does a high score indicate about a device?

### Anki Cards to Build
- MDE P1: AV + ASR + device control (no EDR)
- MDE P2: adds EDR, threat analytics, automated investigation
- EDR in block mode: works as secondary AV — blocks without being primary AV
- Security baseline conflict: neither setting applies when baseline + config profile conflict
- ASR rules: Audit → Warn → Block mode (stage rollout this way)

---

## Wednesday Jul 15 — SSPR + MFA Deep-Dive

**Hours:** 2h | Office — focused review

### Topics
- MFA methods: authenticator app notification, TOTP code, phone call, SMS, FIDO2 key, Windows Hello, certificate
- Passwordless: authenticator app (phone sign-in), FIDO2 security key, WHfB, Temporary Access Pass (TAP)
- SSPR + MFA registration: combined registration portal (`/register`) — one registration for both
- Authentication strengths: built-in strengths (MFA, passwordless MFA, phishing-resistant MFA)
- Conditional Access authentication strength: enforce specific MFA method for sensitive apps
- Entra ID per-user MFA vs CA-enforced MFA: per-user MFA is legacy — CA is the correct approach
- TAP (Temporary Access Pass): time-limited passcode for onboarding or lost authenticator recovery

### MS Learn Modules
- [Plan and implement multifactor authentication](https://learn.microsoft.com/en-us/training/modules/plan-implement-administer-conditional-access/)

### Self-Check
1. A user loses their phone (authenticator app). An admin wants to give them time-limited access to re-register MFA. What feature enables this?
2. What is the difference between phishing-resistant MFA and standard MFA in terms of authentication methods allowed?
3. Per-user MFA is enabled for a user. A CA policy also enforces MFA for all apps. Which takes precedence, or do both apply?
4. Combined registration: a user registers for SSPR. Do they also register for MFA in the same session?
5. Authentication strength: an admin creates a CA policy requiring "Phishing-resistant MFA" for Azure portal access. Which methods satisfy this requirement?

### Anki Cards to Build
- Passwordless methods: authenticator phone sign-in, FIDO2 key, WHfB, TAP
- TAP: one-time or limited-use passcode for MFA recovery / new device onboarding
- Phishing-resistant MFA: WHfB + FIDO2 only (no SMS, no TOTP, no push)
- Combined registration: single portal for SSPR + MFA — not separate registrations
- Per-user MFA: legacy — migrate to CA-enforced MFA

---

## Thursday Jul 16 — D2 Gap Fill + Practice Assessment Prep

**Hours:** 2h | Office — review + practice

### Topics
- Revisit any self-check failures from Sat–Wed
- Conditional Access edge cases: what happens when no CA policies match? (access granted by default)
- Compliance policy platform support: Windows, macOS, iOS, Android, Linux
- Identity Protection + CA integration: how risk policies connect to CA sign-in risk conditions

### Self-Check (mixed D2)
1. No CA policies exist in a tenant. A user signs in to O365 from an unmanaged device. What happens?
2. A compliance policy assigns a grace period of 5 days. On Day 6, CA checks device compliance. Result?
3. WHfB PIN is 6 digits. The user changes it from device settings. Where is the new PIN stored?
4. An admin sets SSPR to require 2 authentication methods. The user has only registered 1. Can they reset?
5. MDE TVM shows a device has a critical vulnerability. No automated remediation configured. Who is responsible for remediation?

---

## Friday Jul 17 — Anki Review + D1+D2 Practice Assessment

**Hours:** 1.5h | Office

### Tasks
1. Review all D1 + D2 Anki cards — both decks
2. Take a 20-question mixed D1+D2 practice assessment (MeasureUp, MS Learn practice, or self-quiz)
3. Score ≥ 75%? → Week 3 is optional catchup — proceed to Week 4 content
4. Score < 75%? → Week 3 is mandatory — note specific failing topics now
5. Log result in progress.json or study notes
