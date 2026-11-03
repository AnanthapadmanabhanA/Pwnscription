# 08 · Reporting & Disclosure

Finding the bug is half the job. Communicating it clearly, scoring it with patient safety in mind, and disclosing it responsibly is the other half, and in medical devices it's the half that protects patients and keeps you on the right side of the law.

---

## Writing the report

Your report serves three readers at once. Structure it so each finds what they need:

1. **Executives / risk owners** → an executive summary with overall risk and the handful of things that matter.
2. **Engineers** → precise, reproducible findings with evidence and concrete remediation.
3. **Quality / regulatory** → traceability: which requirement, standard, or threat-model entry each finding maps to.

Use the **[pentest report template](templates/pentest-report-template.md)**. Per finding, include:

- **Title & ID**
- **Affected component / interface / firmware version**
- **Description** - what the flaw is
- **Reproduction** - enough to verify, with evidence (captures, traces, screenshots)
- **Impact - framed as patient-safety / essential-performance**, not just CIA
- **Severity** - CVSS base + the **medical rubric** result (below)
- **Likelihood / exploitability** - access required, preconditions, skill
- **Remediation** - specific and prioritized
- **References** - the threat-model entry, the standard/requirement it fails, related CVEs

> The one thing that makes a medical-device report different from a normal pentest report: **every finding's impact section answers "what could this do to a patient?"** If a finding has no safety/essential-performance angle, say so explicitly, that's useful information too.

## Scoring: the MITRE medical-device CVSS rubric

Plain **CVSS** was built for enterprise IT; it under-weights the clinical environment and patient-safety impact. MITRE (for FDA) published a **Rubric for Applying CVSS to Medical Devices**, an FDA-**qualified Medical Device Development Tool (MDDT)**, that structures how to pick CVSS metric values for a device, considering patient safety, the clinical/network environment, and multiple stakeholder views.

- Rubric + calculators: <https://www.mitre.org/md-cvss-rubric>
- **How to use it:** work the rubric's decision questions to choose each CVSS metric consistently, then **always publish the full CVSS vector string alongside the score** so others can see your reasoning. Pair the number with a plain-language patient-impact statement - the score supports the story, it doesn't replace it.

> Don't let a modest CVSS number bury a real safety issue. If a "medium" can stop a ventilator's essential performance, the narrative must say so loudly. Score *and* explain.

## Coordinated Vulnerability Disclosure (CVD)

If you found the issue in a **deployed or third-party product** (i.e. the manufacturer isn't the client who hired you), you are now in coordinated disclosure. The norms here are stricter than consumer software because patients are downstream.

### The principles
- **Contact the manufacturer first**, through their security/CVD channel (many publish a `security.txt`, a PSIRT address, or a policy - an expectation reinforced by FDA § 524B and AAMI SW96).
- **Loop in a coordinator** when needed: **CISA** (which issues ICS Medical Advisories), a national **CERT**, or the relevant **regulator** (FDA; in the EU, the manufacturer/competent authority; in Germany, BSI/BfArM). Coordinators help when the vendor is unresponsive or many parties are affected.
- **Give reasonable time to remediate** before any public disclosure - remember devices patch slowly and some can't patch at all (legacy).
- **Never disclose publicly first.** The St. Jude/MedSec–Muddy Waters episode ([07](07-case-studies-and-cves.md)) is the field's cautionary tale.
- **Minimize harm throughout** - no testing on patient-connected devices, no release of working exploits against fielded medical devices, careful handling of any PHI encountered.

### A workable CVD flow
```
Discover ─▶ Document & score ─▶ Find the vendor's CVD channel
   │                                   │  (no channel? → engage CISA/CERT to coordinate)
   │                                   ▼
   └────────────────────────▶ Report privately (encrypted) ─▶ Acknowledge & triage
                                                                   │
                        Agree timeline & coordination ◀────────────┘
                                   │
          Vendor remediates ─▶ Advisory published (vendor + CISA ICSMA/CVE) ─▶ Coordinated public disclosure
```

### Useful references
- **ISO/IEC 29147** (vulnerability disclosure) & **30111** (vulnerability handling) - the general CVD standards vendors align to.
- **CISA Coordinated Vulnerability Disclosure** process - how a US coordinator runs it. <https://www.cisa.gov/coordinated-vulnerability-disclosure-process>
- **FDA** treats CVD as an expected postmarket capability; **AAMI SW96** and **IMDRF N60** build it in.
- **MDIC/MITRE - "Coordinated Vulnerability Disclosure for Medical Device Cybersecurity"** reporting (via MDIC resources).
- **I Am The Cavalry HOCMD** - the ethical framing: collaborate, enable third-party research, capture evidence, support updates. <https://iamthecavalry.org/oath>

## Getting a CVE

Vulnerabilities in medical devices are routinely assigned **CVE IDs**, frequently through **CISA/ICS-CERT** as the CNA for OT/medical. Work with the coordinator/vendor to request and publish a CVE as part of the advisory, it's how the finding becomes trackable against SBOMs everywhere.

## If you're on the receiving end (manufacturer/HDO)

Turn the report into action:
- **Reproduce, confirm scope** (which versions/models/fleet), and assess **safety impact** via your ISO 14971 / SW96 process.
- Feed it into **CAPA / postmarket surveillance** (now explicitly expected under the QMSR-aligned FDA guidance).
- Decide remediation vs. compensating controls (for legacy/can't-patch, see **IMDRF N70** and **HSCC HIC-MaLTS** in [02](02-regulations-and-standards.md)).
- Communicate to customers (advisory, updated MDS2/SBOM) and coordinate public disclosure.

---

### Quick checklist before you send anything

- [ ] Findings framed in **patient-safety** terms
- [ ] Scored with **CVSS + MITRE medical rubric**, vector strings included
- [ ] Evidence is clean, reproducible, and **contains no real PHI**
- [ ] Remediation is specific and prioritized
- [ ] Disclosure path agreed; **vendor-first**, coordinator looped if needed
- [ ] Nothing published publicly before coordination completes

Next: **[09 · Learning & Community →](09-learning-and-community.md)**
