# Week 7 — DEP-2026 Final Review

**Dates:** Aug 15 – Aug 21, 2026
**Target hours:** ~2h
**Focus:** DEP-2026 retrieval and gap-fill. No exam this week.

> SUP-2026 should already be booked or passed by this point (targeted after Week 4). DEP-2026's exam is Week 9 — this week is purely final review and retrieval, no new content, no exam day.

---

## Saturday Aug 15 — DEP-2026 Cold Recall

**Hours:** 1.5h | Retrieval only, no reading

Answer from memory first, then verify against Weeks 5–6 notes.

**ABM + ADE:**
1. ABM roles: list all 6 roles and one key permission each.
2. ADE zero-touch flow: describe it in 5 steps from factory-new device to enrolled desktop.
3. What MDM enrollment profile setting makes a Mac supervised? What does supervision unlock?
4. Can a device enrolled via ADE be removed from MDM by the user? What happens if the device is wiped?
5. How does Configurator add devices to ABM? What workflow does it follow?

**Enrollment Types:**
1. User Enrollment: what identifier does the MDM server see instead of serial number? Why?
2. Device Enrollment: can the user remove the MDM profile? What type of devices is it designed for?
3. ADE enrollment: what two things make it different from Device Enrollment?

**Configuration Profiles:**
1. `PayloadRemovalDisallowed = true` on an unsupervised device: what happens when the user tries to remove the profile?
2. Two MDM profiles push the same payload type (e.g., passcode). Which setting wins?
3. Name 4 supervised-only payload types or restrictions.
4. What file format is a configuration profile? What is the root structure?

**DDM:**
1. What are the 4 declaration types in DDM?
2. What is the status channel and which direction does it communicate?
3. Minimum OS requirements for DDM?
4. Can DDM and legacy MDM commands coexist on the same device?

**VPP:**
1. User-based vs device-based licensing: which requires a Managed Apple ID on device?
2. VPP workflow: describe 4 steps from purchasing in ABM to app appearing on device.
3. A license is revoked from a user. What happens to the app on their device?
4. What is a custom app (B2B) and how is it distributed via ABM?

### Post-session
- Anki: review all "Again" + "Hard" cards from Apple Weeks 5–6 (and SUP-2026 decks if any are still shaky)
- Flag any cold-recall failures — revisit on Mon
- If you have real observations from the 2nd MacBook Pro's live ADE enrollment, use them to sanity-check your answers above instead of relying purely on notes

---

## Sunday Aug 16 — REST

No study. Full rest day, no exceptions.

---

## Monday Aug 17 — DEP-2026 Top Exam Traps

**Hours:** 0.5h | Light touch only

- **`PayloadRemovalDisallowed`:** only enforced on supervised devices — on unsupervised, user CAN remove the profile despite the flag
- **User Enrollment:** no serial number in MDM, no full wipe possible, no device-based CA compliance — designed for BYOD with privacy separation
- **Two profiles, same payload type:** most restrictive setting wins — NOT an error state (unlike Intune, which shows Error for conflicting profiles)
- **DDM minimum OS:** macOS 13 / iOS 16 — devices running older OS fall back to legacy MDM silently
- **Device-based VPP:** no Apple ID required on device — correct answer for any shared or kiosk scenario
- **ADE + supervised:** supervision persists through OS wipe — a wiped ADE device re-enrolls automatically when connected to internet
- **Configurator add to ABM:** uses "Add to Organization's Apple Business Manager" workflow inside Configurator 2 — requires Configurator to be authorized in ABM first
- **ABM Content Manager role:** can buy and assign apps/books in ABM but cannot manage devices or enroll MDM servers

### DEP-2026 Anki: final pass
Review all "Again" cards from Weeks 5–7. Nothing else this week.

---

## Rest of the week

No new content. If you feel ready, this is a good window to book the DEP-2026 exam for Week 9. Otherwise, use any remaining short sessions this week for weak-card Anki review only — see `week-09.md` for exam-day logistics and the exam day itself.
