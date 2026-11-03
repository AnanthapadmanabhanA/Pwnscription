# 03 · Testing Methodology

How to run a medical-device security assessment that is **safe, repeatable, and useful**, from the first scoping call to the retest. This is the backbone of a professional engagement and the thing to standardize first if you're building a team.

> The method below is device-security-specific, but it maps closely to PTES, the OWASP testing guides, and OWASP FSTM for firmware. If you come from general pentesting, the *new* parts are **safety gating**, **the clinical/threat context**, and **coordinated disclosure**.

---

## Phase 0 - Authorization & safety gating (do not skip)

Nothing starts until these are true:

- [ ] **Written authorization** defining scope, in-scope/out-of-scope targets, test window, and a named emergency contact.
- [ ] **Lab units only.** Confirm no in-scope unit is, or could be, connected to a patient or in clinical use. If a device *can* actuate (pump, shock, dose, ventilate), confirm it is on a bench with no patient connection and, where possible, with actuation physically simulated (dummy loads, test lungs, saline, etc.).
- [ ] **Rules of engagement** for destructive tests (teardown, fault injection, fuzzing that may brick the unit) - who approves, and are spare units available.
- [ ] **Data handling** agreed: assume any real patient data (ePHI/PII) is off-limits; use synthetic data. If real data could appear, agree handling/destruction up front.
- [ ] **Disclosure path** agreed before you find anything (see [08](08-reporting-and-disclosure.md)).

> ⚠️ The golden rule: **a test that could plausibly affect a device's essential performance must never run on anything a patient relies on.** When unsure, isolate further.

## Phase 1 - Information gathering & threat modeling

Build the grey-box picture before touching hardware.

1. **Collect artifacts:** intended use, user/service manuals, architecture diagrams, **SBOM**, **MDS2**, prior threat model, prior assessment reports, network/IT requirements doc, OEM security docs. (See [02](02-regulations-and-standards.md) for what these are.)
2. **Map the system of systems.** Device ↔ hub/programmer ↔ network ↔ interface engine ↔ EHR/PACS ↔ cloud ↔ mobile app. Every link is a surface ([01 → ecosystem](01-fundamentals.md#the-ecosystem--a-device-is-never-alone)).
3. **Enumerate interfaces physically & logically:** USB, serial/UART, JTAG/SWD, Ethernet, Wi-Fi, Bluetooth/BLE, cellular, proprietary RF/NFC, removable media, optical/barcode, maintenance/service ports.
4. **Threat model.** Use the MITRE/MDIC playbook approach (answer: *what are we working on? what can go wrong? what are we doing about it? did we do a good job?*). Center **patient-safety impact**. Capture it in the [threat model template](templates/threat-model-template.md).
5. **Pre-screen the SBOM** against NVD/OSV for known-vulnerable components - findings before you've plugged anything in.

## Phase 2 - Premarket vs. postmarket framing

Decide which you're doing; it changes depth and goals.

- **Premarket / design-stage assessment** (for a manufacturer, before launch): goal is to find and fix before patients are exposed. You'll likely get source, design docs, and debug access. Deeper, more collaborative, maps to FDA/EU submission evidence.
- **Postmarket / deployed assessment** (for an HDO or on a shipping product): goal is real-world risk and compensating controls. More black/grey-box; legacy and "can't-patch" realities dominate (IMDRF N70, HIC-MaLTS).

## Phase 3 - Surface-by-surface testing

Work the attack surface systematically. Full detail per surface is in **[04 · Attack Surface](04-attack-surface.md)**; tools are in **[05 · Tooling](05-tooling.md)**. A typical order:

1. **Recon & network/services** - enumerate exposed services, default creds, outdated/EOL OS and components.
2. **Clinical protocols** - DICOM, HL7v2, FHIR, ASTM/LIS: auth, injection, spoofing, tampering.
3. **Hardware** - teardown, identify ICs, find debug ports (UART/JTAG/SWD), dump flash, assess tamper protection.
4. **Firmware** - extract, unpack, analyze: secrets, crypto, update verification, known-vuln components.
5. **Wireless / RF** - BLE, Wi-Fi, cellular, and proprietary telemetry: pairing, encryption, replay, spoofing.
6. **Mobile & cloud/API** - companion apps and backends: auth, authorization/IDOR, transport security, secrets.
7. **Web / management interfaces** - admin/service portals: standard web testing with a safety lens.

For each surface, record: interface, test performed, observed behavior, evidence, and, critically, **plausible patient-safety impact**.

## Phase 4 - Verification & impact analysis

- **Reproduce** each finding reliably; capture clean evidence (packet captures, screenshots, logic traces, before/after).
- **Chain** findings - a low-severity default credential + an unauthenticated command interface may together degrade essential performance.
- **Assess clinical impact** with a clinician or clinical engineer if you can. The question is always: *what could this do to a patient?*
- **Score** with the [MITRE medical-device CVSS rubric](08-reporting-and-disclosure.md#scoring-the-mitre-medical-device-cvss-rubric), which forces patient-safety and clinical-environment considerations CVSS alone misses.

## Phase 5 - Reporting

Write for three audiences in one document: executives (risk), engineers (fix), and regulators/quality (traceability). Use the [report template](templates/pentest-report-template.md). Each finding ties back to the threat model and, where relevant, to a regulatory/standard requirement it fails.

## Phase 6 - Coordinated disclosure (if applicable)

If this is a deployed/third-party product and the manufacturer isn't your client, you're now in **coordinated vulnerability disclosure**, often involving the MDM *and* a coordinator (CISA, a national CERT, or a regulator). See **[08 · Reporting & Disclosure](08-reporting-and-disclosure.md)**. Do **not** go public first.

## Phase 7 - Retest & closure

- Verify fixes actually fix (and don't introduce regressions or new safety risk).
- Confirm residual risk is documented and accepted by the right owner.
- Update the threat model and SBOM-cross-reference for next time.

---

## Building a repeatable lab setup

A good bench makes testing safe and fast. Minimum viable medical-device bench:

- **Isolated test network** (managed switch + VLANs; never the hospital/corporate LAN). A span/tap port for capture.
- **Hardware bench:** logic analyzer (Saleae-class), multimeter, USB-UART adapters, JTAG/SWD probe, soldering/rework, a bench PSU, and ESD-safe tools.
- **RF kit:** an SDR (RTL-SDR to start, HackRF/bladeRF/LimeSDR to transmit lawfully in a shielded setup), a BLE sniffer (nRF52840/Sniffle), and - where legal and contained - an RF-shielded enclosure for transmit testing.
- **Analysis workstation:** firmware tooling (binwalk/unblob/EMBA/FACT), Ghidra, Wireshark with DICOM/HL7 dissectors, a DICOM node (Orthanc) and an interface engine (Mirth Connect) to receive/replay.
- **Spare/sacrificial units** for destructive tests, plus actuation simulators (test lung, saline bags, dummy loads) so a device can "run" without a patient.

Details and tool links in **[05 · Tooling](05-tooling.md)** and **[06 · Labs](06-labs-ctfs-and-practice.md)**.

## Common mistakes to avoid

- **Bricking the only unit.** Always know if spares exist before destructive steps.
- **Testing the wrong thing.** The "device" is a system; the bug is often in the companion app or cloud API, not the box.
- **Legacy OS panic.** Finding Windows XP/CE embedded is normal. The finding isn't "it's old," it's "here's the specific reachable, exploitable path and its patient impact."
- **Confidentiality tunnel-vision.** Keep asking: can this affect *integrity* or *availability* of therapy? That's usually the bigger story.
- **Going public.** Never disclose before coordination completes.

Next: **[04 · Attack Surface →](04-attack-surface.md)**
