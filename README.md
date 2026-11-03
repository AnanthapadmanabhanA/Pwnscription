<div align="center">

# 💊 Pwnscription

### *The cure-all for medical device pentesting.*

A single, curated home for everything you need to **learn, practice, and perform security testing of medical devices**, from your first teardown to a mature program. Built for beginners finding their feet and for experienced researchers who want one reference instead of forty browser tabs.

[![Maintained](https://img.shields.io/badge/maintained-yes-2ea44f)](https://github.com/AnanthapadmanabhanA/Pwnscription/graphs/commit-activity)
[![Contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen)](CONTRIBUTING.md)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey)](LICENSE)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-blue)](CONTRIBUTING.md)

</div>

---

> [!WARNING]
> **Patients first. Always.** Medical devices keep people alive. Test only devices you own or are **explicitly authorized in writing** to assess, never on or near a device connected to a patient, and always within a lab you control. Everything here is for **lawful, authorized, defensive security work and education**. See [Legal & Ethics](#-legal--ethics) before you touch anything.

---

## Why this exists

Medical-device security knowledge is scattered across regulator PDFs, conference talks, half-dead wikis, and tribal knowledge locked in a few consultancies. A newcomer doesn't know where to start; an experienced pentester coming from web/network/IoT doesn't know what's *different* about health tech (hint: the threat model ends in **patient harm**, not data loss).

**Pwnscription puts it under one roof:** the regulations that shape the work, a repeatable testing methodology, the full attack surface broken down surface by surface, the tools, the places to practice legally, the landmark research to learn from, and the community to plug into.

## Who it's for

| You are… | Start here |
|---|---|
| **New to the field** (student, web/network tester, biomed engineer) | [Start Here → Beginner path](docs/00-start-here.md) |
| **An experienced pentester** new to *medical* | [Start Here → Crossover path](docs/00-start-here.md#crossover-path--experienced-pentester-new-to-medical) |
| **Building or running a test program** | [Methodology](docs/03-testing-methodology.md) + [Reporting & Disclosure](docs/08-reporting-and-disclosure.md) |
| **A manufacturer / blue team** | [Regulations & Standards](docs/02-regulations-and-standards.md) + [Templates](docs/templates/) |
| **A researcher chasing a bug** | [Attack Surface](docs/04-attack-surface.md) + [Case Studies & CVEs](docs/07-case-studies-and-cves.md) |

## What's inside

| # | Guide | What it covers |
|---|---|---|
| 00 | [**Start Here**](docs/00-start-here.md) | Learning paths by background; how to use this repo; a 30/60/90-day plan |
| 01 | [**Fundamentals**](docs/01-fundamentals.md) | What counts as a medical device, risk classes, the clinical ecosystem, and why *safety ≠ security but drives it* |
| 02 | [**Regulations & Standards**](docs/02-regulations-and-standards.md) | FDA, EU MDR / MDCG 2019-16, IMDRF, Germany (BSI), AAMI/IEC/UL/NIST, SBOM, MDS2 — what each one asks of you |
| 03 | [**Testing Methodology**](docs/03-testing-methodology.md) | How to scope, plan, and run a medical-device assessment safely — pre-engagement to retest |
| 04 | [**Attack Surface**](docs/04-attack-surface.md) | Hardware/PCB, firmware, wireless (BLE/Wi-Fi/cellular/proprietary RF), DICOM/HL7/FHIR, mobile, cloud/API, web, network |
| 05 | [**Tooling**](docs/05-tooling.md) | The practical toolbox, grouped by surface, with what each tool is for |
| 06 | [**Labs, CTFs & Practice**](docs/06-labs-ctfs-and-practice.md) | Where to practice *legally* — CTFs, simulators, and how to build a home lab |
| 07 | [**Case Studies & CVEs**](docs/07-case-studies-and-cves.md) | The landmark research every med-device tester should know, and what it teaches |
| 08 | [**Reporting & Disclosure**](docs/08-reporting-and-disclosure.md) | Scoring with the medical CVSS rubric, writing the report, coordinated disclosure |
| 09 | [**Learning & Community**](docs/09-learning-and-community.md) | Books, courses, conferences, villages, blogs, people, and orgs to follow |

**Templates** you can copy into an engagement: [pentest report](docs/templates/pentest-report-template.md) · [threat model](docs/templates/threat-model-template.md) · [device assessment checklist](docs/templates/device-assessment-checklist.md)

## Quick start

```text
1. Read docs/00-start-here.md and pick the path that matches your background.
2. Skim docs/01-fundamentals.md so the vocabulary and risk classes make sense.
3. Keep docs/04-attack-surface.md and docs/05-tooling.md open as working references.
4. Practice only in docs/06-labs-ctfs-and-practice.md environments.
5. When you find something real, follow docs/08-reporting-and-disclosure.md.
```

## Legal & Ethics

Security testing of medical devices sits on top of real patients and a dense body of law. Before any hands-on work:

- **Authorization in writing.** Scope, targets, dates, and a named point of contact. No written authorization → no testing.
- **Never test on a device in clinical use** or anything that is, or could be, connected to a patient. Use lab units.
- **Coordinated disclosure, always.** Manufacturers, and often a regulator or CERT, are part of the loop. See [Reporting & Disclosure](docs/08-reporting-and-disclosure.md).
- **Know your jurisdiction.** Computer-misuse, medical-device, and data-protection laws vary by country and can carry serious penalties.
- **Spirit of the work:** the security research community's [Hippocratic Oath for Connected Medical Devices](https://iamthecavalry.org/oath) says it best - *first, do no harm*, then find the bugs.

This repository contains **no exploit payloads or weaponization steps**, it teaches methodology, points to primary sources, and links tools that have their own legitimate uses.

## Contributing

This is meant to be a living, community-maintained resource. Found a dead link, a missing tool, a new regulation, or a great talk? **PRs and issues are very welcome**, see [CONTRIBUTING.md](CONTRIBUTING.md) and our [Code of Conduct](CODE_OF_CONDUCT.md).

If this repo saves you time, **give it a ⭐**, it helps other people in the field find it.

## License

Documentation is released under [**CC BY 4.0**](LICENSE), use it, adapt it, share it, just give credit. Linked third-party tools and documents remain under their own licenses.

## Maintainer & credits

Curated and maintained by [**@AnanthapadmanabhanA**](https://github.com/AnanthapadmanabhanA). Built on the shoulders of the researchers, regulators, and communities cited throughout, including the folks behind the DEF CON Biohacking Village, I Am The Cavalry, MITRE/MDIC, OWASP, and the many researchers in [Case Studies & CVEs](docs/07-case-studies-and-cves.md).

Related community list worth knowing: [`zseha/awesome-medsec`](https://github.com/zseha/awesome-medsec). Pwnscription aims to be the *opinionated, structured, learn-and-do* companion to link lists like it.

---

<div align="center">
<sub>Pwnscription is an independent educational project. It is not affiliated with, endorsed by, or sponsored by any manufacturer, regulator, or the author's employer.</sub>
</div>
