# 00 · Start Here

Welcome. This page gets you oriented and pointed at the right path for your background. Read it once, pick a lane, and use the rest of the repo as a working reference.

> New to the whole field? Read [01 · Fundamentals](01-fundamentals.md) right after this, the vocabulary there makes every other page easier.

## How this repo is organized

The nine numbered guides in [`docs/`](.) go roughly in the order you'd actually work:

```
00 Start Here            ← you are here
01 Fundamentals          what a device is, risk classes, the ecosystem, safety vs security
02 Regulations & Std.    the rules that shape the work (FDA, EU, IMDRF, BSI, AAMI/IEC…)
03 Methodology           how to run an assessment, safely and repeatably
04 Attack Surface        what to test, surface by surface
05 Tooling               what to test it with
06 Labs, CTFs & Practice where to practice legally
07 Case Studies & CVEs   what the landmark research teaches
08 Reporting & Disclosure how to write it up and disclose it
09 Learning & Community   where to keep learning
```

[Templates](templates/) are copy-paste starting points for real engagements.

## The one rule that changes everything

In IT security the worst case is usually **data loss or downtime**. In medical-device security the worst case is **a patient is harmed or killed**. That single difference reshapes the whole discipline:

- *Availability and integrity often outrank confidentiality.* A ventilator that stops is worse than one that leaks a log.
- *Safety and security risk management are joined at the hip* (see [02](02-regulations-and-standards.md)).
- *You cannot "just try it" on production.* Test rigs and lab units only.
- *Disclosure is a careful, coordinated, multi-party process* (see [08](08-reporting-and-disclosure.md)).

Keep that framing in your head the whole way through.

---

## Beginner path - new to the field

You might be a student, a biomedical/clinical engineer, or a web/network tester who's never touched health tech. Work through this in order:

1. **[01 · Fundamentals](01-fundamentals.md)** - learn the vocabulary: device classes, SaMD, the hospital ecosystem, "essential performance," safety vs. security.
2. **[02 · Regulations & Standards](02-regulations-and-standards.md)** - you don't need to memorize these, but knowing *why* manufacturers do what they do makes you a far better tester. Skim FDA, MDCG 2019-16, and IMDRF N60.
3. **Build fundamentals in general offensive security if you don't have them yet.** Medical-device testing is applied IoT/embedded/web/network/RF testing. Get comfortable with Linux, networking, and at least one of: web app testing, embedded/firmware, or RF. [09 · Learning & Community](09-learning-and-community.md) lists on-ramps.
4. **[06 · Labs, CTFs & Practice](06-labs-ctfs-and-practice.md)** - do the Biohacking Village CTF material and stand up a small home lab. Hands-on beats reading.
5. **[04 · Attack Surface](04-attack-surface.md)** + **[05 · Tooling](05-tooling.md)** - now the "how" will click because you have context.
6. **[07 · Case Studies & CVEs](07-case-studies-and-cves.md)** - read these like a pilot reads crash reports. Each one is a lesson.

**First 90 days, suggested:** see the [30/60/90-day plan](#a-30609-day-starter-plan) below.

## Crossover path - experienced pentester, new to medical

You already know how to break things. What you need is the *delta*: what's different, what's higher-stakes, and what's regulated.

1. **[01 · Fundamentals](01-fundamentals.md) → "Safety vs security" and "The ecosystem."** This is the mindset shift. Read it even if you skim everything else.
2. **[02 · Regulations & Standards](02-regulations-and-standards.md).** You'll be handed SBOMs, MDS2 forms, and threat models and asked to test against them. Know what they are.
3. **[03 · Methodology](03-testing-methodology.md).** The safety gating, the "do no harm" test setup, and premarket-vs-postmarket framing are the parts that differ most from a standard pentest.
4. **[04 · Attack Surface](04-attack-surface.md).** Jump to the surfaces you *don't* already own - most crossovers need the **clinical protocols** section (DICOM, HL7v2, FHIR) and the **proprietary RF / telemetry** section.
5. **[08 · Reporting & Disclosure](08-reporting-and-disclosure.md).** Scoring with the **MITRE medical-device CVSS rubric** and running **coordinated disclosure** with a manufacturer + regulator is different from a normal client report.

## By role - quick jump

- **Building a team / program:** [03 · Methodology](03-testing-methodology.md) → [Templates](templates/) → [02 · Regulations](02-regulations-and-standards.md). Standardize your methodology and report format first; they're force multipliers.
- **Manufacturer / product security:** [02 · Regulations](02-regulations-and-standards.md) → [threat model template](templates/threat-model-template.md) → [04 · Attack Surface](04-attack-surface.md) as a design-review checklist.
- **Hospital / HDO blue team:** [02 · Regulations](02-regulations-and-standards.md) (MDS2, HIC-MaLTS) → [07 · Case Studies](07-case-studies-and-cves.md) → [09 · Community](09-learning-and-community.md) (H-ISAC, CISA ICSMA).

---

## A 30/60/90-day starter plan

A concrete ramp if you're starting from a general-security background. Adjust to your pace.

### Days 1–30 - Context & vocabulary
- Read [01 Fundamentals](01-fundamentals.md) and [02 Regulations](02-regulations-and-standards.md).
- Read IMDRF N60 and skim the current FDA premarket cybersecurity guidance (both linked in [02](02-regulations-and-standards.md)).
- Set up a home lab bench: a Linux box, a logic analyzer, a cheap SDR, a BLE adapter. See [06](06-labs-ctfs-and-practice.md).
- Read 5 case studies in [07](07-case-studies-and-cves.md) and write one paragraph on what each teaches.

### Days 31–60 - Hands-on on *safe* targets
- Work the Biohacking Village CTF / "Hospital Under Siege" material ([06](06-labs-ctfs-and-practice.md)).
- Stand up open-source DICOM (Orthanc) and an HL7/FHIR interface engine (Mirth Connect) and *talk to them* - these protocols are everywhere in health tech ([04 → Clinical protocols](04-attack-surface.md#clinical-protocols-dicom-hl7v2-fhir)).
- Practice firmware extraction/analysis on a non-medical IoT device with binwalk/unblob/EMBA ([05](05-tooling.md)).

### Days 61–90 - Do a full mock assessment
- Pick a lab device you own (or a simulator). Run the full [methodology](03-testing-methodology.md) end to end: scope → threat model → surface-by-surface testing → findings.
- Score findings with the [medical CVSS rubric](08-reporting-and-disclosure.md#scoring-the-mitre-medical-device-cvss-rubric).
- Write it up with the [report template](templates/pentest-report-template.md). This artifact is your portfolio.

---

## Conventions used in this repo

- 🔗 Links go to **primary sources** wherever possible (regulators, standards bodies, original research).
- ⚠️ Safety/legal callouts look like blockquotes with a warning.
- Tool mentions describe *what the tool is for*, not step-by-step offensive recipes - this repo teaches methodology, not weaponization.

Next: **[01 · Fundamentals →](01-fundamentals.md)**
