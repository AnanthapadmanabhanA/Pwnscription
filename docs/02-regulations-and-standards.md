# 02 · Regulations & Standards

You don't have to be a regulatory specialist to pentest medical devices, but you do have to understand the rules, because they define what the manufacturer was *supposed* to do, hand you your best test artifacts (threat models, SBOMs, MDS2 forms), and dictate how findings get disclosed.

This page is a map, not legal advice. Dates and revisions change, always check the primary source.

> **How to read this as a tester:** every "shall / should" in these documents is a testable claim. "The device shall authenticate firmware updates" is a test case. Mine them.

---

## United States - FDA

The FDA regulates device cybersecurity mainly through **premarket submission expectations** and **postmarket obligations**.

- **Current premarket guidance:** *Cybersecurity in Medical Devices: Quality Management System Considerations and Content of Premarket Submissions* - finalized **February 2026**, superseding the June 2025 edition. The 2026 update mainly re-aligns the document with the new **Quality Management System Regulation (QMSR)** (21 CFR Part 820, which now incorporates **ISO 13485:2016** by reference); the substantive security expectations - Secure Product Development Framework (SPDF), threat modeling, security architecture, security testing, and an SBOM for "cyber devices" - carry over.
  - Landing page: <https://www.fda.gov/regulatory-information/search-fda-guidance-documents/cybersecurity-medical-devices-quality-management-system-considerations-and-content-premarket>
  - PDF: <https://www.fda.gov/media/119933/download>
- **Statutory basis - FD&C Act § 524B ("cyber devices"):** manufacturers must submit a plan to monitor/identify/address postmarket vulnerabilities, design for updates/patches, and provide an **SBOM**. This is law, not guidance.
- **Postmarket:** FDA's postmarket cybersecurity guidance + the shift to embedding cybersecurity in the quality system (QMSR). Expect vulnerability handling to be audited as part of CAPA and postmarket surveillance.

**What FDA expects you to see in a submission (and thus can test against):** security risk management file, threat model, SBOM + third-party/supply-chain analysis, security architecture views, security testing evidence (including **vulnerability and penetration testing**), unresolved-anomaly assessment, and a postmarket management plan.

## European Union - MDR / IVDR + MDCG 2019-16

- **MDR 2017/745** and **IVDR 2017/746** set out, in Annex I, *General Safety and Performance Requirements (GSPRs)* that include IT security and "state of the art."
- **MDCG 2019-16 Rev.1 - *Guidance on Cybersecurity for medical devices*** is the key interpretive document: it maps the MDR/IVDR cybersecurity requirements across the device lifecycle (secure design, risk management, verification/validation, post-market).
  - Official PDF: <https://health.ec.europa.eu/document/download/b23b362f-8a56-434c-922a-5b3ca4d0a7a1_en>
  - Docsroom entry: <https://ec.europa.eu/docsroom/documents/41863>
  - It's **non-binding guidance**, but in practice it's the accepted framework for MDR/IVDR cybersecurity conformity.
- **Overlapping EU law** you'll hear about: **GDPR** (health data), the **Cyber Resilience Act (CRA)** and **NIS2** (broader product/operator security). MDR acts as *lex specialis* for devices, but CRA/NIS2 context increasingly matters.
- **Post-market:** **MDCG 2025-10** (Dec 2025) broadened Commission guidance on post-market surveillance for devices and IVDs.

> EU ≈ US on the fundamentals (secure design, risk management, SBOM-style transparency, lifecycle). A good study is the **gap analysis** of MDCG 2019-16 vs FDA guidance: <https://pmc.ncbi.nlm.nih.gov/articles/PMC12301760/>

## International harmonization - IMDRF

The **International Medical Device Regulators Forum** publishes the documents most other regulators build on. Three to know:

- **N60 - Principles and Practices for Medical Device Cybersecurity** (2020): the foundational TPLC framework; introduces legacy-device and SBOM concepts.
- **N70 - Principles and Practices for the Cybersecurity of Legacy Medical Devices** (2023): how to handle devices that can't be fully protected/patched. PDF: <https://www.imdrf.org/sites/default/files/2023-04/IMDRF%20Principles%20and%20Practices%20of%20Cybersecurity%20for%20%20Legacy%20Medical%20Devices%20Final%20%28N70%29.pdf>
- **N73 - Principles and Practices for SBOM for Medical Device Cybersecurity** (2023): what a good medical SBOM looks like.
- IMDRF documents index: <https://www.imdrf.org/documents>

## Germany 🇩🇪 (focus region)

If you're testing for the German/EU market, two BSI threads matter beyond MDR:

- **BSI TR-03161 - security requirements for digital health applications.** Since **1 Jan 2025**, a **TR-03161 certificate is mandatory for DiGA** (reimbursable health apps) under §139e SGB V; it requires source-code analysis and **penetration testing**, alongside an ISO 27001 / IT-Grundschutz ISMS and (for backends) a **BSI C5** report. BSI: <https://www.bsi.bund.de/> (search "TR-03161").
- **BSI ManiMed project** - BSI commissioned real security assessments of networked medical devices (infusion pumps, patient monitors, pacemaker ecosystems, insulin pumps, ventilators) and published the findings. Excellent methodology reading.
  - BSI projects page: <https://www.bsi.bund.de/EN/Themen/Unternehmen-und-Organisationen/Standards-und-Zertifizierung/E-Health/Medizintechnik/Projekte/projekte_node.html>
  - ERNW's detailed public write-ups (HL7v2 injection in patient monitors, insulin-pump telemetry, etc.): <https://insinuator.net/2021/01/manimed-part-1/>

## Standards you'll test against

These are the technical standards manufacturers build to, i.e. your checklist of "did they actually do it."

| Standard | What it is | Why it matters to you |
|---|---|---|
| **ISO 14971:2019** | Safety risk management for devices | The spine; security risk plugs into it |
| **AAMI TIR57** | Principles for medical device security — risk management | First bridge from 14971 to security |
| **AAMI TIR97** | Security risk management for *postmarket*/fielded devices | Legacy & fielded-device handling |
| **ANSI/AAMI SW96:2023** | Security risk management *for manufacturers* | FDA-recognized; runs with ISO 14971; covers threat modeling, security testing, CVD, patching, retirement |
| **IEC 81001-5-1:2021** | Secure software lifecycle for health software | Cited as "state of the art" by both FDA and EU |
| **IEC 60601-4-5** | Security capabilities for medical electrical equipment | Technical security capability levels |
| **UL 2900-2-1** | Software cybersecurity for network-connectable healthcare devices | Testable security requirements |
| **NIST SP 800-53 / CSF / 800-30** | General security controls, framework, risk | Controls vocabulary & risk method |

## Transparency artifacts you'll be handed (and should demand)

These are genuinely useful to a tester: they turn a black box into a grey box.

- **SBOM (Software Bill of Materials).** Inventory of components + versions. Cross-reference against known-vulnerable versions (NVD/OSV) before you even touch the device. Formats: SPDX, CycloneDX. (IMDRF N73; US § 524B.)
- **MDS2 - Manufacturer Disclosure Statement for Medical Device Security** (NEMA/MITA **HN 1-2019**). A standardized ~200-question form describing a device's security capabilities (auth, encryption, patching, ePHI handling, SBOM availability, CVD). Read it as a claims list to verify. Standard: ANSI/NEMA HN 1-2019.
- **Threat model.** Required/expected by FDA. If you get theirs, test its assumptions; if not, build one - see the [threat model template](templates/threat-model-template.md) and the MITRE playbook below.

## Playbooks & cross-industry references (free, high-value)

- **MITRE/MDIC - Playbook for Threat Modeling Medical Devices** (FDA-funded). The best free introduction to device threat modeling. PDF: <https://www.mitre.org/sites/default/files/2021-11/Playbook-for-Threat-Modeling-Medical-Devices.pdf> · MDIC page: <https://mdic.org/resource/playbook-for-threat-modeling-medical-devices/>
- **MITRE - Rubric for Applying CVSS to Medical Devices** (FDA-qualified MDDT). How to score device vulns with patient-safety impact in mind. <https://www.mitre.org/md-cvss-rubric> - used in [08 · Reporting](08-reporting-and-disclosure.md).
- **HSCC (Health Sector Coordinating Council):**
  - **Medical Device and Health IT Joint Security Plan v2 (JSP2)**, 2024 - consensus product-security practices. <https://healthsectorcouncil.org/jsp2/>
  - **HIC-MaLTS - Managing Legacy Technology Security**, 2023. <https://healthsectorcouncil.org/legacy-tech-security/>
  - All HSCC publications: <https://healthsectorcouncil.org/hscc-publications>
- **CISA ICS Medical Advisories (ICSMA).** The running feed of disclosed medical-device vulns - read these weekly. <https://www.cisa.gov/news-events/cybersecurity-advisories> (filter to "ICS Medical Advisory").
- **OWASP Secure Medical Device Deployment Standard** - deployment-side hardening for HDOs.

## The ethical backbone

- **I Am The Cavalry - Hippocratic Oath for Connected Medical Devices (HOCMD):** five principles (cyber-safety by design, third-party collaboration/CVD, evidence capture, resilience & containment, cyber-safety updates). Short, worth reading in full. <https://iamthecavalry.org/oath>

---

### Tester's cheat-sheet: map a claim to a test

| They claim (in MDS2/threat model/SBOM) | You test |
|---|---|
| "Firmware updates are signed" | Try to flash modified/unsigned firmware; check signature validation ([04 → Firmware](04-attack-surface.md#firmware)) |
| "Communications are encrypted" | Capture traffic; check TLS config, cert validation, downgrade ([04 → Network/Wireless](04-attack-surface.md)) |
| "No hardcoded credentials" | Extract firmware; grep for secrets; check default accounts ([04 → Firmware](04-attack-surface.md#firmware)) |
| "Component X v1.2.3" (SBOM) | Look up X v1.2.3 in NVD/OSV for known CVEs |
| "Supports CVD" | Verify a real reporting channel exists and works ([08](08-reporting-and-disclosure.md)) |

Next: **[03 · Testing Methodology →](03-testing-methodology.md)**
