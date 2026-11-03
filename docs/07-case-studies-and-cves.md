# 07 · Case Studies & CVEs

Read these like a pilot reads incident reports. Each landmark piece of research teaches something durable about *where medical devices break and why*. Dates/figures are from public reporting and advisories, follow the links for primary detail.

> These are summarized for learning, not as attack recipes. The lesson is in the **class of flaw** and the **disclosure story**, not in reproduction steps.

---

## Implantables & telemetry

### Medtronic "Conexus" telemetry (2019)
Researchers found the proprietary **Conexus** wireless telemetry used between certain Medtronic cardiac implants, home monitors, and programmers **lacked authentication and encryption**, allowing crafted/replayed radio traffic to read or alter device data/settings within RF range.
- Advisory: CISA **ICSMA-19-080-01** (CVE-2019-6538, CVE-2019-6540).
- **Lesson:** proprietary RF is not security. "Physical proximity = authorization" is a broken assumption for implants. Authenticate and encrypt the *link*, not just the app.

### St. Jude Medical / Abbott pacemakers & the MedSec–Muddy Waters episode (2016–2017)
Security firm **MedSec** disclosed vulnerabilities in St. Jude's Merlin@home ecosystem **via a short-seller (Muddy Waters)** rather than the vendor, a controversial break from coordinated disclosure. FDA and ICS-CERT later confirmed issues; Abbott issued firmware updates.
- **Lesson:** two lessons. (1) Home-monitoring hubs and their RF links are real implant attack surface. (2) *How* you disclose matters enormously - this case is the cautionary tale that shaped modern medical CVD norms.

### Insulin pumps - Radcliffe, Barnaby Jack, and J&J Animas OneTouch Ping
A line of research going back to **Jerome Radcliffe** and **Barnaby Jack** (early 2010s) showed wireless insulin pumps could be commanded to alter dosing. Later, **Rapid7** disclosed the **J&J Animas OneTouch Ping** used cleartext/weakly-authenticated RF (R7-2016-07), enabling spoofed remote dosing commands in range.
- **Lesson:** unauthenticated remote-control radio + a device that doses = direct patient-harm path. Protecting the availability and integrity of therapy is what matters most.

## Infusion pumps

### Hospira / ICU Medical (Billy Rios et al., 2015)
Research into Hospira LifeCare/Symbiq/Plum pumps found issues including the ability to alter drug-library limits and, in some models, reach the pump over the network, leading to an FDA safety communication recommending against continued use of Symbiq.
- **Lesson:** "drug library" limits are a safety control; if you can change them, you can enable an unsafe dose. Network-reachable pumps are high-impact.

### Medfusion 4000 (Scott Gayou, 2017)
Detailed public research + a MQX RTOS RCE on the **Smiths Medical Medfusion 4000** syringe pump, a great, readable end-to-end teardown-to-exploit methodology.
- Research repo: <https://github.com/sgayou/medfusion-4000-research>
- **Lesson:** a model for how to document a full device assessment; also shows RTOS/stack bugs as the path to control.

## Patient monitors & the supply chain

### Contec CMS8000 backdoor (2025)
CISA and FDA warned that **Contec CMS8000** patient monitors (also rebadged, e.g. Epsimed MN-120) contained firmware that **beacons to a hard-coded external IP and transmits plaintext patient data**, plus a function allowing remote file upload/overwrite, effectively a backdoor.
- Advisory: **ICSMA-25-030-01**; CVE-2025-0626, CVE-2025-0683, CVE-2024-12248. CISA fact sheet: <https://www.cisa.gov/resources-tools/resources/contec-cms8000-contains-backdoor>
- **Lesson:** supply-chain/provenance risk is real; always check *what a device talks to by default*. Egress analysis is a core test, not an afterthought.

### BSI ManiMed project (Germany, 2020)
BSI-commissioned assessments of ten networked devices across five categories. Public ERNW write-ups cover, e.g., **HL7v2 injection reaching Philips IntelliVue patient monitors**, **B. Braun infusion-pump (Space) issues**, and insulin-pump telemetry findings.
- Write-ups: <https://insinuator.net/2021/01/manimed-part-1/>
- **Lesson:** a thorough, methodical, coordinated, government-backed study - and proof that clinical protocols (HL7) are an injection surface, not just plumbing.

## Component-level vulns that hit *many* devices at once

These are why SBOMs exist: one library, many victims.

| Name | Component | Nature |
|---|---|---|
| **URGENT/11** (2019) | VxWorks / IPnet TCP-IP stack | Multiple RCE-class TCP/IP bugs; affected many embedded/medical devices |
| **Ripple20** (2020) | Treck TCP/IP stack | Widespread embedded TCP/IP flaws |
| **SweynTooth** (2020) | BLE SoC stacks | BLE crashes/deadlocks/bypass; flagged by FDA for affected medical devices |
| **Access:7** (2022) | PTC Axeda remote-management agent | Remote code execution / full access in widely-OEM'd agent |

- **Lesson:** test the *bill of materials*, not just the box. A device can be "securely built" and still inherit a fleet-wide vuln from a third-party stack. Cross-reference every SBOM component against NVD/OSV.

## Clinical-protocol & imaging lessons

### DICOM preamble malware (2019)
Researchers showed the **128-byte preamble** of the DICOM file format can carry executable content such that a file is simultaneously a valid medical image *and* a runnable executable, complicating AV and trust.
- **Lesson:** clinical file formats carry parsing and trust assumptions worth testing; "it's just an image" is wrong.

### HL7 v2's trust problem (ongoing; see ManiMed)
HL7 v2 over MLLP is frequently unauthenticated and unvalidated; crafted messages can spoof orders/results or inject into downstream systems and devices.
- **Lesson:** integrity of clinical messaging = integrity of clinical decisions. Test it.

---

## How to use these

For each case study, write yourself three lines:
1. **What class of flaw** was it (auth, RF, parsing, supply chain, protocol trust…)?
2. **What was the patient-safety path** (how does the bug reach harm)?
3. **How was it disclosed** - and what would *good* disclosure have looked like?

That exercise builds the instincts this field runs on. Keep a running log as you read new **CISA ICSMA** advisories ([06](06-labs-ctfs-and-practice.md)).

### Where to find more, continuously
- **CISA ICS Medical Advisories:** <https://www.cisa.gov/news-events/cybersecurity-advisories>
- **FDA cybersecurity safety communications:** <https://www.fda.gov/medical-devices/safety-communications>
- **NVD / OSV** keyword & component searches.
- Conference archives in [09 · Learning & Community](09-learning-and-community.md).

Next: **[08 · Reporting & Disclosure →](08-reporting-and-disclosure.md)**
