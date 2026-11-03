# 01 · Fundamentals

Before you test a medical device, you need the mental model: what one *is*, how regulators bucket them by risk, what the surrounding ecosystem looks like, and the idea that makes this field different from the rest of security, **safety**.

---

## What is a "medical device"?

Broadly, a medical device is any instrument, machine, implant, in-vitro reagent, or **software** intended by its manufacturer for a medical purpose, diagnosis, prevention, monitoring, treatment, or alleviation of disease. The exact legal wording differs between the US (FD&C Act) and EU (MDR 2017/745), but the shape is the same.

Two consequences matter for testers:

- **Software can be the device.** *Software as a Medical Device* (SaMD) - e.g. an app that analyzes images and flags a tumor - is regulated as a device in its own right. A mobile app or a cloud service can be in scope.
- **Intended use defines it.** The same hardware can be a regulated device or not depending on what the manufacturer claims it does. "Intended use" drives the whole risk analysis.

### A rough taxonomy you'll meet in the field

| Category | Examples | Why testers care |
|---|---|---|
| **Implantable** | Pacemakers, ICDs, insulin pumps, neurostimulators | Highest stakes; often wireless telemetry; patched slowly or never |
| **Wearable / ambulatory** | Insulin pumps, CGMs, Holter monitors | BLE/proprietary RF; companion phone apps; cloud backends |
| **Bedside / point-of-care** | Patient monitors, infusion pumps, ventilators | On hospital networks; HL7/serial; long service life |
| **Imaging & diagnostics** | CT/MRI/ultrasound, PACS, DICOM workstations | DICOM everywhere; big Windows/Linux stacks; RIS/PACS integration |
| **In-vitro diagnostics (IVD)** | Analyzers, blood-gas, lab instruments | LIS integration; HL7/ASTM; sample-handling robotics |
| **Software (SaMD) & apps** | Triage apps, DiGA (Germany), AI/ML diagnostics | Web/mobile/cloud testing; model/data concerns |
| **Hospital infrastructure adjacent** | Nurse-call, RTLS, pharmacy robots, bed systems | Often overlooked; bridge IT ↔ clinical |

## Risk classification (why a pacemaker ≠ a tongue depressor)

Regulators classify devices by the harm they could cause. Higher class = more scrutiny = (usually) more security expectation.

- **US FDA:** Class I (low, e.g. bandages), Class II (moderate, e.g. infusion pumps - most devices), Class III (high, e.g. implantable pacemakers - require premarket approval).
- **EU MDR:** Class I, IIa, IIb, III, in ascending risk, with special rules for software.

As a tester, the class tells you how much rigor the manufacturer was *required* to apply, and often, inversely, where the surprises hide (a neglected Class II accessory can be the way into a Class III system).

### "Cyber device" (US)

US law (FD&C Act **section 524B**, added by the 2023 omnibus) created the notion of a **cyber device**, one that has software, can connect to the internet, and could contain a vulnerability. Cyber devices carry explicit obligations: a plan to monitor and patch, a secure development process, and a **software bill of materials (SBOM)**. See [02 · Regulations](02-regulations-and-standards.md).

## The ecosystem - a device is never alone

A single "device" is really a **system of systems**. Testing one means understanding what it talks to:

```
   [Implant / sensor]
        │  proprietary RF / BLE
        ▼
   [Bedside unit / programmer / home hub]
        │  Wi-Fi / Ethernet / cellular / serial
        ▼
   [Hospital network]────[HL7/FHIR interface engine]───[EHR / LIS / RIS]
        │                          │
        │ DICOM                    │
        ▼                          ▼
   [PACS / imaging]          [Manufacturer cloud]──[Mobile companion app]
                                   │
                                   ▼
                          [Update / telemetry servers]
```

Every arrow is attack surface. Every box has an owner (manufacturer, hospital/HDO, cloud provider, patient), which is why **shared responsibility** is a recurring theme in the regulations and why disclosure involves many parties.

Key actors you'll hear named:
- **MDM** - Medical Device Manufacturer.
- **HDO** - Healthcare Delivery Organization (hospital/clinic).
- **Regulators / coordinators** - FDA (US), notified bodies & competent authorities (EU), BfArM/BSI (Germany), and CERTs like **CISA** (which publishes **ICS Medical Advisories**).

## Safety vs. security - the core idea

This is the part worth getting right.

- **Safety risk management** (ISO 14971) asks: *how could this device harm a patient, and how do we reduce that to acceptable levels?* It long predates cybersecurity.
- **Security risk management** asks: *how could an adversary (or accident) compromise the device?* - and crucially, **a security problem becomes a safety problem the moment it can affect the device's safety, performance, or intended operation.**

So in medical devices, security is treated as *part of* safety. The standards make this explicit:
- **AAMI TIR57** introduced security risk management alongside ISO 14971.
- **ANSI/AAMI SW96:2023** is the security-risk-management standard for manufacturers, designed to run in parallel with ISO 14971. FDA recognizes it.
- **IEC 81001-5-1** defines the secure software lifecycle both FDA and EU treat as "state of the art."

Two terms from this world you'll use constantly:
- **Essential performance** - the performance necessary to achieve freedom from unacceptable risk (e.g. a ventilator *actually ventilating*). If your finding can degrade essential performance, it's serious regardless of CVSS.
- **Harm** - physical injury, damage to health, or damage to property/environment. In scoring, **patient harm is the top of the impact scale** (see the [medical CVSS rubric](08-reporting-and-disclosure.md)).

### What this means when you test

1. Frame every finding in terms of **potential impact on patient safety / essential performance**, not just CIA.
2. Expect **availability and integrity to often matter more than confidentiality**.
3. Respect that a device may run for **10–20 years**, unpatched, on an unsupported OS. "Just update it" is frequently not an option - hence the whole discipline of **legacy device** management (IMDRF N70, HSCC HIC-MaLTS; see [02](02-regulations-and-standards.md)).

## A minimal glossary

| Term | Meaning |
|---|---|
| **SaMD** | Software as a Medical Device — software that is itself the device |
| **SiMD** | Software *in* a Medical Device (embedded) |
| **SBOM** | Software Bill of Materials — inventory of software components/versions |
| **MDS2** | Manufacturer Disclosure Statement for Medical Device Security — standardized security-capability form (NEMA/MITA HN 1-2019) |
| **SPDF** | Secure Product Development Framework — FDA's term for a secure dev lifecycle |
| **TPLC** | Total Product Life Cycle — pre-market through retirement |
| **CVD** | Coordinated Vulnerability Disclosure |
| **Legacy device** | A device that can no longer be reasonably protected against current threats |
| **Essential performance** | Performance needed to keep risk acceptable |
| **PACS / RIS / LIS** | Imaging archive / radiology info system / lab info system |
| **DiGA** | *Digitale Gesundheitsanwendung* — a reimbursable health app in Germany, with its own security testing regime |

Next: **[02 · Regulations & Standards →](02-regulations-and-standards.md)**
