# 10 · Advanced Resources & Deep Dives

For experienced testers and teams going past the core guides. These are heavier frameworks, device-specific reference architectures, dynamic-analysis tooling, and the research corpus you grow into. Everything here assumes you already have the fundamentals in [00](00-start-here.md)–[09](09-learning-and-community.md).

Jump to: [Threat-modeling frameworks](#advanced-threat-modeling-frameworks) · [Reference architectures](#device-specific-reference-architectures-nist-nccoe) · [Firmware emulation](#firmware-emulation--rehosting) · [Fuzzing](#fuzzing-clinical-protocols--parsers) · [Advanced RF](#advanced-wireless--implant-rf) · [AI/ML devices](#aiml-enabled-device-security) · [SBOM/VEX at scale](#sbom-at-scale-vex--machine-readable-advisories) · [Incident response](#incident-response-hdo-side) · [FHIR conformance](#fhir--api-conformance-testing) · [Deeper standards](#deeper-standards-register) · [Research reading](#academic--seminal-research)

---

## Advanced threat-modeling frameworks

Beyond the MITRE/MDIC playbook in [02](02-regulations-and-standards.md):

- **MITRE EMB3D** - a threat-model knowledge base built specifically for *embedded devices*. It maps threats to concrete device properties and suggests technical mitigations for each, so you can enumerate a device's exposure from its features. Healthcare was one of the piloting sectors. <https://emb3d.mitre.org>
- **MITRE ATT&CK for ICS** - adversary tactics and techniques for OT/industrial environments, which map well onto hospital clinical networks and the systems devices live on. <https://attack.mitre.org/matrices/ics/>
- **MITRE D3FEND** - a knowledge graph of defensive countermeasures; useful when you need to recommend specific mitigations, not just name the gap. <https://d3fend.mitre.org/>

## Device-specific reference architectures (NIST NCCoE)

The NCCoE Healthcare "practice guides" are free, detailed, build-it-yourself reference architectures for securing whole classes of device in a real HDO. Invaluable for deployed/postmarket work and for understanding the compensating controls a hospital can actually deploy.

- **NIST SP 1800-8** - Securing Wireless Infusion Pumps. <https://www.nccoe.nist.gov/publication/1800-8/>
- **NIST SP 1800-24** - Securing Picture Archiving and Communication Systems (PACS). <https://www.nccoe.nist.gov/publication/1800-24-ipg/>
- **NIST SP 1800-30** - Securing Telehealth Remote Patient Monitoring. 
- **NIST SP 1800-1** - Securing Electronic Health Records on Mobile Devices.
- NCCoE Healthcare portfolio (all of them): <https://www.nccoe.nist.gov/healthcare>

## Firmware emulation & rehosting

When you can't (or shouldn't) run tests on the physical unit, emulate it. Rehosting lets you fuzz network services, debug freely, and scale analysis.

- **FirmAE** - automated full-system emulation of Linux-based firmware, with fixes to get stubborn images booting. <https://github.com/pr0v3rbs/FirmAE>
- **Firmadyne** - the original full-system emulation framework for Linux firmware. <https://github.com/firmadyne/firmadyne>
- **Qiling** - a cross-platform binary emulation framework (built on Unicorn) for running and instrumenting firmware binaries. <https://github.com/qilingframework/qiling>
- **Unicorn Engine** - lightweight CPU emulator, the building block under a lot of rehosting work. <https://github.com/unicorn-engine/unicorn>
- **EMBA** also has an emulation mode (see [05](05-tooling.md)).

## Fuzzing clinical protocols & parsers

The network-facing parsers (DICOM, HL7, proprietary protocol stacks) are where memory-safety bugs become remotely reachable. Fuzz them.

- **boofuzz** - network-protocol fuzzer; good fit for building DICOM/HL7/MLLP harnesses. <https://github.com/jtpereyda/boofuzz>
- **AFL++** - coverage-guided fuzzing for libraries and file/format parsers (DICOM object parsing, firmware components). <https://github.com/AFLplusplus/AFLplusplus>
- Approach: pick the highest-privilege network-facing parser, build a harness (native or in emulation), seed with valid clinical messages/objects, and watch for crashes that map to an essential-performance impact.

## Advanced wireless & implant RF

Past the BLE basics in [04](04-attack-surface.md):

- **Mirage** - a modular framework for wireless security analysis (BLE, Zigbee, and more) with UNIX-pipe-style module chaining; notable for the InjectaBLE attack (injecting into an established BLE connection). <https://github.com/RCayre/mirage>
- **BTLEJack** - BLE sniffing, jamming, and connection hijacking. <https://github.com/virtualabs/btlejack>
- **Sniffle** - high-quality open BLE sniffer (also in [05](05-tooling.md)), worth mastering beyond basic capture. <https://github.com/nccgroup/Sniffle>
- Foundational implant-RF research: **Halperin et al., "Pacemakers and Implantable Cardiac Defibrillators: Software Radio Attacks and Zero-Power Defenses"** (IEEE S&P 2008) - the paper that opened the field, using a software radio to analyze and attack an ICD's telemetry, and proposing zero-power defenses. <https://www.secure-medicine.org/>

> The RF-safety rule from [04](04-attack-surface.md) still applies, doubly, to implant work: receive first, transmit only in a shielded, lawful, contained setup.

## AI/ML-enabled device security

A fast-growing surface as ML-enabled devices proliferate. Two things to understand:

- **FDA Predetermined Change Control Plan (PCCP)** - final guidance (Dec 2024) for AI-enabled device software functions. It lets manufacturers pre-authorize a bounded set of model changes without a new submission. For a tester, the PCCP defines what the device is *allowed* to change post-clearance, so it frames your retest scope. FDA guidance: search "Predetermined Change Control Plan" at <https://www.fda.gov/regulatory-information/search-fda-guidance-documents>.
- **MITRE ATLAS** - the adversarial-ML counterpart to ATT&CK: data poisoning, evasion, model extraction, etc. Apply it to the model/data pipeline of an ML-enabled device. <https://atlas.mitre.org/>
- **Good Machine Learning Practice (GMLP)** - joint FDA / Health Canada / MHRA guiding principles for ML device development; context for what "good" looks like.

## SBOM at scale, VEX & machine-readable advisories

SBOMs ([02](02-regulations-and-standards.md)) are only useful if you can process them at fleet scale and cut the false positives.

- **CSAF (Common Security Advisory Framework)** - OASIS standard for machine-readable security advisories, pushed by CISA. <https://www.csaf.io/>
- **VEX (Vulnerability Exploitability eXchange)** - a profile within CSAF that lets a vendor state "component X is present but this product is *not affected*," with a justification. Pairs with the SBOM to kill noise. (CISA VEX resources, via <https://www.cisa.gov/>.)
- **MedCrypt Helm** - medical-specific SBOM + VEX/VDR management (commercial). <https://www.medcrypt.com/>
- **MedSBOM** - open-source FDA / IEC 62304-oriented SBOM checks. <https://github.com/karthikraja14/MedSBOM>
- Pair with **Syft / Grype / Dependency-Track** from [05](05-tooling.md).

## Incident response (HDO side)

- **MITRE / FDA - Medical Device Cybersecurity Regional Incident Preparedness and Response Playbook** (2022 revision) plus its Quick Start Companion Guide. The reference for how hospitals prepare for and respond to device-related incidents (post-WannaCry). <https://www.mitre.org/securemed>
- Combine with **HSCC JSP2** and **HIC-MaLTS** from [02](02-regulations-and-standards.md).

## FHIR / API conformance testing

- **Inferno** (ONC) - the FHIR conformance test framework, including the SMART App Launch Test Kit. Use it two ways: as a lab target to learn FHIR/SMART-on-FHIR, and as an oracle to probe auth and scope behavior on a device's FHIR interface. <https://inferno.healthit.gov/> · framework: <https://inferno-framework.github.io/>

## Deeper standards register

Beyond the core set in [02](02-regulations-and-standards.md):

| Standard | Scope | Why it matters |
|---|---|---|
| **IEC 62443** | Industrial automation & control systems security | Applies to hospital OT infrastructure and device manufacturing environments |
| **IEC 80001-1** | Risk management for IT-networks incorporating medical devices | The HDO-side counterpart: securing the network the device lives on |
| **ISO/IEC 27799** | Health informatics - information security management | Health-sector profile of ISO 27001 |
| **ISO/IEC 27001 / 27002** | ISMS and controls | Baseline for manufacturers and HDOs alike (and required alongside BSI TR-03161 for DiGA) |

## Academic & seminal research

Read the source papers, not just the summaries. Start with:

- **Halperin et al. (2008)** - ICD software-radio attacks (above). The origin point.
- **Ostermann et al. (2025)** - gap analysis of MDCG 2019-16 vs FDA premarket cybersecurity guidance, *Computational and Structural Biotechnology Journal*. <https://pmc.ncbi.nlm.nih.gov/articles/PMC12301760/>
- **Archimedes / Secure Medicine** (Kevin Fu et al.) - the academic home of much of this field. <https://www.secure-medicine.org/>
- Track the top venues for new device/embedded/health security work: **USENIX Security**, **IEEE S&P (Oakland)**, **ACM CCS**, **NDSS** - plus the case studies in [07](07-case-studies-and-cves.md).

---

### Folding this into practice

- Use **EMB3D + ATT&CK for ICS** to enrich the threat model ([templates/threat-model-template.md](templates/threat-model-template.md)).
- Use **NCCoE guides** to frame realistic compensating controls in deployed/postmarket reports.
- Use **emulation + fuzzing** when hardware access is limited or destructive testing is off-limits.
- Use **CSAF/VEX + SBOM tooling** to make vulnerability findings actionable at fleet scale.
- Use **ATLAS + PCCP** when the device has an ML component.

← Back to **[README](../README.md)** · Start at **[00 · Start Here](00-start-here.md)**
