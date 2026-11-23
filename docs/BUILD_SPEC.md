# PulseAI build specification

Version 0.1, 22 November 2026. Source of truth for behaviour: `app/index.html` (prototype). Launch market: United States. Items in square brackets are placeholders listed in `PLACEHOLDERS.md`.

## 1. Goal

One product where a person can (a) keep health readings and records, (b) get plain-language explanations and a journey map, and (c) meet a verified clinician licensed in their state.

## 1a. Operating model (decided)

PulseAI is a technology platform. It does not employ clinicians or deliver care. Practitioners are independent: they hold their own licence, insurance and records, set their own fees and make every clinical decision. Consequences for the build:
- Each practitioner signs a platform agreement and a HIPAA business associate agreement before going live.
- Practitioners set their visit fee in their profile. PulseAI charges them a platform fee of [AMOUNT]; a percentage of visit fees is not assumed.
- Visit summaries and prescriptions belong to the practitioner's record; PulseAI stores them on the practitioner's behalf.
- Product wording never says "our doctors" and every profile states that the practitioner is independent.
- Staff can verify, suspend and remove practitioners, but cannot edit clinical content.

## 2. User roles

| Role | Can do |
| --- | --- |
| Patient | Create account, add readings and records, run symptom guidance, find and request practitioners, attend visits, receive prescriptions, control sharing, export and delete data |
| Practitioner | Apply, be verified, set availability, accept or decline requests, run visits, write visit summaries, prescribe within scope |
| Verifier (staff) | Review practitioner applications, check identity, state licence and national provider record, approve, suspend |
| Clinical lead (staff) | Approve symptom rules, explanation content and thresholds; review flagged visits |
| Support and admin (staff) | Manage accounts, refunds, audit logs. No access to clinical content unless granted for a case |

The prototype implements Patient and Practitioner. Staff roles need a separate admin console.

## 3. Screens

### Patient
| Screen | Purpose | Key elements | Prototype status |
| --- | --- | --- | --- |
| Welcome | Start | First name, sample data, professional path | Works. Real version needs sign up, sign in, consent |
| Home | Hub | Four actions, active visit, latest reading with explanation | Works |
| Symptom guidance | Decide how soon to seek care | Symptom, duration, warning signs, special situations, result | Works with demonstration rules |
| Find care | Directory | State selector, type filter, free now filter, cards | Works with six sample profiles |
| Practitioner profile | Trust and booking | Verification items, skills, languages, licensed states | Works |
| Request visit | Booking | Visit type, payment method, share readings switch, state check | Works; no payment |
| Visit room | Care | Waiting, accept, chat, end visit | Simulated; no video or audio |
| Prescription | After visit | Medicine, instructions, pharmacy code, visit summary | Placeholder content |
| My numbers | Understand | Type chips, latest value, tag, trend chart, explanation, add form, history | Works for three reading types |
| Journey | Orientation | Timeline of events, highlighted next step | Works |
| Me | Control | Name, who can see readings, choices, copy data, delete data, switch view, open questions | Works |

### Practitioner
| Screen | Purpose | Prototype status |
| --- | --- | --- |
| Application | Role, state, national provider identifier, state licence number | Works; nothing is sent |
| In review | Status | Simulated approval |
| Dashboard | Free now switch, requests with accept and decline, counts | Works |
| Visit room, notes, prescribing | Deliver care | Not built |
| Earnings and payouts | Money | Placeholder |

### Website
Hero, need tiles, phone mockups, wellness, how it works, Understand demo, global and languages, clinicians, security, investors view, Start flow.

## 4. Data model

Prototype stores one JSON object in browser storage. Production needs a relational database with the tables below.

| Entity | Fields |
| --- | --- |
| User | id, role, first name, email or phone, preferred language, current state, created at |
| Reading | id, user id, type (bp, pulse, temp), value a, value b (bp only), date, source (manual, device, upload) |
| JourneyEvent | id, user id, kind (start, symptoms, reading, visit, rx), title, date |
| SymptomCheck | id, user id, symptom, duration, warning signs[], special situations[], result level, rules version, date |
| Practitioner | id, user id, role, visit fee (set by practitioner), platform agreement accepted at, licensed states[], national provider identifier, licence numbers, languages, skills, verification status (review, verified, suspended), free now flag, weekly hours |
| VerificationCheck | id, practitioner id, type (identity, licence, provider record, certificates), result, checked by, date, next check date |
| Visit | id, patient id, practitioner id, patient state at time of visit, mode (video, audio, chat), status (requested, accepted, declined, done, cancelled), payment method, fee, share readings flag, started at, ended at |
| Message | id, visit id, sender id, text, sent at |
| VisitSummary | id, visit id, text, written by |
| Prescription | id, visit id, medicine, instructions, pharmacy code, status |
| AccessGrant | id, patient id, practitioner id, scope, visit id, expires at, revoked at |
| Consent | id, user id, type (terms, telehealth, privacy notice, research), version, accepted at |
| AuditLog | id, actor id, action, record, time |

## 5. Rules

### 5.1 Licence and location
- A visit can be requested only if the practitioner is licensed in the state the patient selects as their current location. The prototype enforces this.
- Production must confirm location at each visit and store it on the Visit.

### 5.2 Symptom guidance (demonstration; clinical lead must replace)
1. Any warning sign selected (chest pain or pressure, trouble breathing, heavy bleeding, fainting or confusion, one-sided weakness or trouble speaking, sudden severe pain) gives "Get emergency help now" with 911 guidance and no booking button.
2. Otherwise, any special situation (pregnant, baby under 3 months, long-term condition, weak immune system), or duration over a week, or fever for 3 days or more, gives "See a clinician today".
3. Otherwise, duration of 3 to 7 days gives "Book a visit in the next few days".
4. Otherwise "Self care for now".

### 5.3 Reading explanations for adults (demonstration; clinical lead must approve)
| Type | Condition | Tag |
| --- | --- | --- |
| Blood pressure | 180 or more, or 120 or more | Very high |
| | 140 or more, or 90 or more | High |
| | 120 or more, or 80 or more | A little above ideal |
| | under 90, or under 60 | On the low side |
| | otherwise | Healthy range |
| Resting pulse | over 100 | Faster than usual |
| | under 60 | Slower than usual |
| | otherwise | Usual range |
| Temperature (°F) | 103.1 or more | High fever |
| | 100.4 or more | Fever |
| | under 95 | Very low |
| | over 99.5 | Slightly raised |
| | otherwise | Usual range |

Each explanation shows three parts: what the number is, what yours means, what to do next.

Input limits: blood pressure first number 50 to 260, second 30 to 160 and lower than the first; pulse 25 to 230; temperature 86 to 111 °F.

### 5.4 Sharing
- Readings are shared with a practitioner only when the patient switches sharing on for a visit.
- Access ends when the visit ends, or sooner if the patient removes it.

### 5.5 Practitioner verification
- Application requires role, state, a 10-digit national provider identifier and a state licence number.
- Status moves review, verified, suspended. Only verified practitioners can switch to free now.
- Licence re-check interval: [PERIOD].

### 5.6 Journey map
Events are added when a symptom check finishes, a reading is saved, a visit is requested, cancelled or completed, and a prescription is issued. The next step is: the open visit; else a follow-up check if a visit was completed; else talk to a clinician if readings exist; else add a first reading.

## 6. Services to choose (all placeholders)

| Need | Requirement |
| --- | --- |
| Hosting and database | Signs a HIPAA business associate agreement; encryption at rest and in transit; audit logging |
| Sign in | Email or phone with multi-factor option |
| Video and audio | Signs a business associate agreement; falls back from video to audio to chat |
| Payments | Card, HSA or FSA cards, sponsor pay link; practitioner payouts |
| E-prescribing | Certified e-prescribing network; controlled medicines out of scope unless decided otherwise |
| Identity and licence checks | Identity document check, state licence lookup, national provider record lookup |
| AI explanations | Model under a business associate agreement; answers limited to approved content; clinician review path |
| Text messages | Reminders that hide health details by default |

## 7. Non-functional requirements
- Accessibility: WCAG 2.2 AA, large touch targets, readable at 200 percent zoom.
- Privacy by default: no sharing unless chosen; export and delete available to the user.
- Works on weak connections: local cache of records, queued uploads.
- Languages: English and Spanish first [CONFIRM]; right-to-left support kept.
- Every medical rule is versioned and signed off by the clinical lead.

## 8. Suggested phases
1. Accounts, readings, explanations, journey map (no clinical care).
2. Practitioner applications, verification console, directory.
3. Visits by video, audio and chat, with payments.
4. Prescriptions and pharmacies.
5. AI question answering, wearables, lab uploads, more languages.

## 9. Not in the prototype
Accounts, server storage, staff console, real video, payments, e-prescribing, uploads of files, wearables, AI answers, notifications, app translations.
