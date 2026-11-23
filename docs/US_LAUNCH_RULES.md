# United States launch rules for PulseAI

Prepared 22 November 2026 as a starting point for a US health lawyer. This is research, not legal advice. Items marked [VERIFY] come from general knowledge and were not confirmed against a source in this pass.

## Summary

The US rules are well documented, but they are not simple: most of them are set state by state. Seven areas decide what PulseAI can launch.

| Area | What it means for PulseAI | Confidence |
| --- | --- | --- |
| Clinician licensure | A clinician must be authorised in the state where the patient is | Sourced |
| Company structure | Decided: technology platform, clinicians independent. Rules against companies employing doctors still shape the wording and fees | Sourced, needs counsel |
| Prescribing | Ordinary medicines are possible; controlled medicines are uncertain after 31 December 2026 | Sourced |
| HIPAA | Applies once PulseAI delivers care; vendors need signed agreements | Sourced |
| FTC rule | Applies to the records and insights part if HIPAA does not | Sourced |
| FDA | The symptom check and explanations may count as device software | Sourced, needs counsel |
| State privacy laws, consent, insurance | Vary by state | [VERIFY] |

## 1. Licensure across state lines

- The federal telehealth site says the ability to deliver care across state lines varies with each state's regulations. It lists the routes: a full licence in the patient's state, temporary practice laws, licensure compacts, and telehealth registration. Source: [Licensing across state lines, telehealth.hhs.gov](https://telehealth.hhs.gov/licensure/licensing-across-state-lines).
- Compacts are a faster route to interstate practice and exist for several professions. Source: [Licensure compacts, telehealth.hhs.gov](https://www.telehealth.hhs.gov/licensure/licensure-compacts).
- Most states take part in the Interstate Medical Licensure Compact, the Nurse Licensure Compact, or both. Source: [AAFP, Legal requirements for telehealth](https://www.aafp.org/practice-operations/telehealth-and-telemedicine/legal-requirements-for-telehealth).

**Design consequence:** the app asks which state the patient is in and only shows practitioners licensed there. This is already in the prototype. Nutritionists, therapists and midwives have their own state rules [VERIFY for each profession].

## 2. Company structure: corporate practice of medicine

- Many states bar a general business corporation from practising medicine or employing physicians; the usual answer is a physician-owned professional corporation supported by a management company (the "MSO and PC" model). Sources: [Wilson Sonsini](https://www.wsgr.com/en/insights/the-corporate-practice-of-medicine-essential-guidance-for-digital-health-companies-and-investors.html), [Epstein Becker Green](https://www.ebglaw.com/health-law-advisor/corporate-practice-of-medicine-the-unseen-hurdle-in-telehealth).
- One law-firm summary counts at least 34 states with some form of the prohibition. Source: [K&L Gates](https://files.klgates.com/files/228827_part_one_d2c_telehealth.pdf).
- States are tightening these rules. Oregon's SB 951, signed 9 June 2025, limits management company influence over professional entities. Source: [Foley & Lardner](https://www.foley.com/p/102kfsh/oregons-new-corporate-practice-of-medicine-restrictions-five-takeaways-for-digi/).

**Decision (Charles, 22 November 2026):** PulseAI will be a technology platform only. It does not employ clinicians or deliver care; independent practitioners use it to reach and treat their own patients.

What the platform model implies, for the lawyer to confirm [VERIFY]:
- Each practitioner holds their own licence, malpractice cover and patient records, and makes every clinical decision.
- PulseAI must not direct care, set clinical protocols or present clinicians as "our doctors". The symptom guidance and explanations must stay informational.
- Fees: practitioners set their own visit fees. Some states restrict sharing a percentage of clinical fees with a non-clinical company, so a flat or subscription platform fee is the safer starting assumption.
- HIPAA: PulseAI is likely a business associate of each practitioner and needs an agreement with each one.
- Verification and removal: PulseAI can check licences and remove practitioners without controlling how they practise.

## 3. Prescribing

- Controlled medicines: DEA and HHS extended the pandemic telemedicine flexibilities through 31 December 2026, so a DEA-registered practitioner can prescribe Schedule II to V medicines without a prior in-person visit until then. Source: [Holland & Knight](https://www.hklaw.com/en/insights/publications/2026/01/dea-and-hhs-extend-telemedicine-prescribing-flexibilities-through-2026).
- The permanent "special registration" rule is still proposed, not final, and a permanent rule was reported under White House review in October 2026. Sources: [Health Law Alliance](https://www.healthlawalliance.com/blog/dea-telemedicine-special-registration-where-the-rule-stands), [WCHSB](https://insights.wchsb.com/2026/10/08/under-90-days-telehealth-prescribing-rules-are-about-to-change-but-the-final-rule-is-still-under-review/).
- **This deadline is about five weeks away.** Check the DEA's own announcements before any decision.
- Non-controlled medicines follow state rules on establishing a patient relationship by telehealth [VERIFY per state]. Electronic prescribing rules and certified e-prescribing vendors [VERIFY].

**Recommendation to consider:** launch with no controlled medicines. It removes the largest regulatory uncertainty.

## 4. HIPAA

- Covered providers must use technology vendors that comply with HIPAA and will sign business associate agreements for video and other remote communication tools. Source: [HIPAA rules for telehealth technology, telehealth.hhs.gov](https://telehealth.hhs.gov/providers/policy-changes-during-the-covid-19-public-health-emergency/hipaa-flexibility-for-telehealth-technology).
- HHS has separate guidance for audio-only telehealth. Source: [HHS Office for Civil Rights](https://www.hhs.gov/hipaa/for-professionals/privacy/guidance/hipaa-audio-telehealth/index.html).

**Design consequence:** every vendor that touches patient information (hosting, video, messaging, AI model, analytics) needs a signed agreement. The prototype must not hold real patient data.

## 5. FTC Health Breach Notification Rule

- Health apps that are not covered by HIPAA must notify consumers, the FTC and sometimes the media after a breach of identifiable health information. The 2024 update made clear that health apps are covered. Sources: [FTC business guidance](https://www.ftc.gov/business-guidance/resources/health-breach-notification-rule-basics-business), [Federal Register, 30 May 2024](https://www.federalregister.gov/documents/2024/05/30/2024-10855/health-breach-notification-rule).

**Design consequence:** the records, readings and insights features fall under either HIPAA or this rule. A lawyer should decide which applies to each feature.

## 6. FDA and health software

- FDA issued a final Clinical Decision Support Software guidance in January 2026. Source: [FDA](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/clinical-decision-support-software).
- The statutory exemption is for software aimed at health care professionals that meets four criteria. FDA noted that its other digital health guidances continue to apply to consumer or patient-facing decision support. FDA also revised its General Wellness guidance. Source: [DLA Piper summary](https://www.dlapiper.com/en/insights/publications/2026/01/fda-updates-its-clinical-decision-support-and-general-wellness-guidances-key-points).

**Open question:** PulseAI's symptom guidance and its explanations of a person's own results are patient-facing. Counsel should decide whether they are general wellness, enforcement discretion, or a regulated device, and how wording changes the answer.

## 7. Still to research [VERIFY]

- State consumer health data laws (for example Washington's My Health My Data Act).
- Telehealth informed consent wording required by each state.
- Insurance: Medicare and Medicaid telehealth coverage, and whether PulseAI is cash-pay only at launch.
- Malpractice cover for each profession and platform liability cover.
- Rules for minors, and for patients travelling outside their home state.
- Advertising rules for health claims.

## 8. Questions for the lawyer

1. Which three to five states should PulseAI launch in, given licensure and company structure rules?
2. Does the platform-only model hold in Florida as designed, and what must the practitioner agreement and fee model say?
3. Is PulseAI a HIPAA covered entity, a business associate, or outside HIPAA for each feature?
4. Does the symptom guidance need FDA review as designed?
5. What can clinicians prescribe on day one?
