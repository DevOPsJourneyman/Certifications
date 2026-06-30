# Week 3 — MD-102 Catchup: D1 + D2 Review Buffer

**Dates:** Jul 18 – Jul 24, 2026
**Target hours:** ~10.5h (if full catchup needed) / skip if ≥ 75% on Week 2 assessment
**Focus:** Targeted gap fill on D1 + D2 weak spots; Anki consolidation; re-test before D3

---

## Skip Rule

**If your Week 2 practice assessment score was ≥ 75%:** skip this week and go straight to `week-04.md`.

**If score was < 75%:** use this week. Start by listing every question you got wrong — that list drives everything below.

---

## How to Use This Week

1. Pull your wrong answers from the Week 2 assessment.
2. Map each wrong answer to a topic (use the section headers below).
3. Work only the sections that match your gaps — skip the rest.
4. Re-test at the end of each day using the self-check questions.
5. End of week: re-run the 20-question D1+D2 practice assessment. Target ≥ 80% before starting Week 4.

---

## D1 Weak Spot: Autopilot Modes + Pre-Provisioning

### Key Facts to Drill
- **User-driven:** user authenticates at OOBE, device joins AAD or Hybrid AAD — Intune MDM enrollment follows automatically
- **Self-deploying:** no user interaction, no user affinity — kiosk/shared device use case; requires TPM 2.0 with device attestation
- **Pre-provisioning (white glove):** technician phase first (device half-configured, rebooted, sealed), then user phase at delivery
- **Technician phase:** installs device-targeted apps + policies; user sees ready-to-use OOBE
- **ESP during white glove:** must complete before device is handed off; failure = device not ready

### Practice Questions
1. Autopilot self-deploying mode fails with "AADSTS50020." What does this error mean, and what is the likely cause?
2. During white glove pre-provisioning, the technician phase completes but the ESP shows app install failure. What happens to the device — is it handed to the user anyway?
3. You need to deploy 200 shared kiosk devices with no user sign-in. Which Autopilot mode and which Intune enrollment type is correct?
4. An Autopilot device joins AAD but does not enroll in Intune. What is the most common cause?
5. White glove pre-provisioning requires which Intune license minimum?

### Anki Cards to Review
- Autopilot mode comparison table: user-driven / self-deploying / pre-provisioning
- ESP: what it tracks, when it blocks, how to troubleshoot failures
- Self-deploying: requires TPM 2.0 + device attestation certificate

---

## D1 Weak Spot: WUfB Deferral Math

### Key Facts to Drill
- **Quality update deferral:** up to 30 days after Microsoft release date
- **Feature update deferral:** up to 365 days after Microsoft release date
- **Deadline:** days after deferral period ends — not after release date
- **Example:** quality update released Jan 1, deferral = 7d, deadline = 5d → device must install by Jan 13
- **Active hours:** Windows will not restart during active hours; grace period applies after deadline
- **Autopatch difference:** Microsoft manages ring progression; you define no deferral settings — just exclude rings or pause

### Practice Questions
1. A WUfB ring has quality deferral = 14 days and deadline = 3 days. A quality update releases on Jul 1. What is the latest install date?
2. A user reports their device restarted at 2am despite active hours set to 8am–10pm. The restart was to apply an update past its deadline. Is this expected behavior?
3. Windows Autopatch is enabled. An admin tries to create a WUfB update ring in Intune. What happens?
4. What is the maximum feature update deferral period?
5. A device is in an update ring with no deadline set. The deferral period expires. When does the device install the update?

### Anki Cards to Review
- WUfB deadline math: deferral starts at release date; deadline starts when deferral ends
- Post-deadline restart: active hours no longer respected — device restarts when idle
- Autopatch: takes over update ring management — custom rings still exist but Autopatch controls cadence

---

## D1 Weak Spot: Co-Management Workloads

### Key Facts to Drill
- 7 workloads: Compliance Policies, Conditional Access, Resource Access, Endpoint Protection, Device Configuration, Office Click-to-Run, Windows Update Policies
- Default: all workloads owned by ConfigMgr
- Slider per workload: Intune / Pilot Intune / ConfigMgr
- Pilot Intune: workload owned by Intune only for devices in a pilot collection
- "Conditional Access" workload: determines which tool *evaluates* CA — but Entra ID enforces CA regardless
- Enrollment path: ConfigMgr existing clients auto-enroll to Intune via cloud attach + co-management enablement

### Practice Questions
1. A co-managed device is in the pilot collection for the "Compliance Policies" workload (set to Pilot Intune). A device NOT in the pilot collection has no Intune compliance policy. What is its compliance state?
2. An admin sets the "Windows Update Policies" workload to Intune for all devices. ConfigMgr still has a software update policy. Which policy applies?
3. Co-management is enabled. A device in the "Endpoint Protection" pilot collection has both a ConfigMgr AV policy and an Intune AV policy targeting it. Which applies?
4. What is the minimum ConfigMgr version required for co-management?
5. A newly enrolled Autopilot device (cloud-only, no ConfigMgr) can participate in co-management: True or False?

---

## D2 Weak Spot: Conditional Access Logic

### Key Facts to Drill
- **No CA policy match → access granted** (CA is deny-by-exception, not allow-by-exception)
- **Report-only mode:** policy is evaluated and logged; no enforcement action taken
- **Grant controls (AND vs OR):** "Require all selected controls" = AND; "Require one of the selected controls" = OR
- **Break-glass account:** global admin account excluded from all CA policies; used for emergency access
- **Named location + trusted flag:** if sign-in from trusted IP, risk-based policies may be skipped
- **Device filter conditions:** can target specific devices by attribute (e.g. model, compliant=true)

### Practice Questions
1. A CA policy requires MFA AND compliant device. A user passes MFA but their device is non-compliant. Are they granted access?
2. A user signs in from an IP not covered by any CA named location. No CA policy matches their sign-in. Are they blocked?
3. CA policy is in report-only mode. A user signs in and the policy would have blocked them. What does the user experience?
4. A break-glass admin account has no MFA registered. A CA policy requiring MFA targets "All users." The break-glass account is not excluded. What happens when it signs in?
5. A CA policy targets "All cloud apps" and grants access only if sign-in risk = Low. The user has no Entra ID P2 license. Does the policy evaluate the risk condition?

---

## D2 Weak Spot: Compliance + WHfB + Identity Protection

### Key Facts to Drill
- **No compliance policy assigned = compliant** (default — can be changed to "Mark as not compliant" in tenant settings)
- **Grace period:** device is flagged non-compliant but CA still grants access; email sent based on noncompliance actions
- **WHfB Cloud Trust:** requires Kerberos Cloud Trust Samba extension — no PKI, no DC patches needed
- **WHfB Key Trust:** requires KB5014754 on DCs + hybrid Entra join — no PKI but DC-dependent
- **Identity Protection risk:** requires Entra ID P2; without P2, risk conditions in CA are ignored
- **User risk remediation:** user must self-remediate via SSPR (password change) — admin can also manually dismiss
- **MFA methods hierarchy:** hardware FIDO2 > WHfB > authenticator app > phone call > SMS > security questions

### Practice Questions
1. A device has a compliance policy assigned. The policy has a 7-day grace period. On Day 8, the device remains non-compliant. A CA policy requires "compliant device." Can the user access O365?
2. An admin configures a user risk policy at "High" risk → block access. A user has accumulated High risk from leaked credentials. They reset their password via SSPR. Can they now access resources without admin intervention?
3. WHfB is deployed with Key Trust. A domain controller is missing KB5014754. What symptom will users experience?
4. An Intune compliance policy is assigned to All Users. A device has no user signed in (shared device). What is its compliance state?
5. SSPR is configured with 2 required methods. A user has registered: authenticator app + phone. Their phone is lost and they try SSPR using only the authenticator app. Can they reset?

---

## Saturday Jul 19 (wait — Jul 18 is Sat? Let me check)

Week 3: Jul 18–24. Jul 18 = Saturday.

## Saturday Jul 18 — D1 Gap Fill

**Hours:** 3.5h | Big block

Work through the D1 weak spot sections above that match your wrong answers. Focus time:
- 90 min: re-read Newton topic file for relevant D1 section + MS Learn module
- 60 min: lab task (if applicable)
- 60 min: self-check from sections above

## Sunday Jul 19 — REST

No study. Full rest day.

## Monday Jul 20 — D2 Gap Fill

**Hours:** 3.5h | Big block

Work through the D2 weak spot sections above that match your wrong answers. Focus time:
- 90 min: re-read Newton topic file for relevant D2 section + MS Learn module
- 60 min: lab task (if applicable)
- 60 min: self-check from sections above

## Tuesday Jul 21 — Mixed Practice

**Hours:** 2.5h | WFH

1. 30-question mixed D1+D2 practice set (MeasureUp or self-quiz from both week files)
2. Score and log result
3. For every wrong answer: identify root cause (wrong fact vs misread question vs gap in lab experience)
4. Add any new Anki cards from wrong answers

## Wednesday Jul 22 — Anki Consolidation

**Hours:** 2h | Office

1. Full review of all D1 + D2 Anki cards — both decks
2. Mark hard/failed cards — those get re-reviewed Thursday
3. No new content — consolidation only

## Thursday Jul 23 — Hard Card Drill + Lab Loose Ends

**Hours:** 2h | Office

1. Re-drill hard Anki cards from Wednesday
2. Complete any outstanding dev tenant labs from Weeks 1 or 2
3. Review: Intune → Reports → Device compliance → check any flagged devices

## Friday Jul 24 — Final D1+D2 Assessment

**Hours:** 1.5h | Office

1. Take 20-question D1+D2 practice assessment
2. Target: ≥ 80% before starting Week 4
3. Score ≥ 80%? → Green light for Week 4 (`week-04.md`)
4. Score < 80%? → Flag specific weak topics; carry as "must revisit" during Week 4 review sessions
5. Log result in progress.json
