# Device Assessment Checklist

> A working checklist to run during a medical-device assessment. Not every item
> applies to every device. Pair it with docs/03-testing-methodology.md and
> docs/04-attack-surface.md. Record evidence and a patient-safety impact note
> for anything you flag.

## Phase 0 - Authorization & safety

- [ ] Written authorization in hand (scope, targets, dates, emergency contact)
- [ ] All units are lab units, none patient-connected or in clinical use
- [ ] Actuating devices benched with simulated loads (test lung, saline, dummy load)
- [ ] Rules of engagement for destructive tests; spare units available
- [ ] Synthetic data only; PHI handling agreed
- [ ] Disclosure path agreed up front

## Phase 1 - Information gathering

- [ ] Collected: intended use, manuals, architecture, SBOM, MDS2, prior threat model/reports
- [ ] System-of-systems mapped (device → hub → network → EHR/PACS → cloud → app)
- [ ] All physical and logical interfaces enumerated
- [ ] Threat model built or reviewed (templates/threat-model-template.md)
- [ ] SBOM pre-screened against NVD / OSV

## Recon & network

- [ ] Services enumerated; OS/firmware versions and support status identified
- [ ] Default / weak / hardcoded credentials checked on every service
- [ ] Plaintext protocols (Telnet/FTP/HTTP) carrying control or sensitive data
- [ ] Device fails safe under network stress (flood, disconnect, malformed input)
- [ ] Remote-support / maintenance channels identified

## Clinical protocols

- [ ] DICOM: association auth, AE-title enforcement, parser robustness, DICOMweb authz
- [ ] HL7 v2: MLLP auth/encryption, message validation, injection into downstream
- [ ] FHIR: object-level authorization, OAuth/SMART scopes, injection, over-broad search

## Hardware

- [ ] ICs identified; debug/programming headers located (UART/JTAG/SWD)
- [ ] JTAG/SWD read-out protection state; serial console authentication
- [ ] External flash readable off-board (secrets/firmware recovery)
- [ ] Secure boot present and enforced; flash encryption
- [ ] Tamper detection meaningful and fails safe
- [ ] (Sacrificial units only) glitch/fault-injection resistance, if in scope

## Firmware

- [ ] Firmware obtained (update package / OTA / flash / debug)
- [ ] Extracted and unpacked (binwalk/unblob); filesystem and bootloader identified
- [ ] Hardcoded secrets / keys / certs / credentials
- [ ] Update verification: signed images, signatures actually validated, downgrade blocked
- [ ] Crypto review: algorithms, key management, cert validation
- [ ] Components cross-referenced vs SBOM and NVD/OSV
- [ ] Debug/dev leftovers (test accounts, verbose logging, debug services)
- [ ] Memory-safety review of network-facing parsers

## Wireless & RF

- [ ] BLE: pairing/bonding strength, command auth + replay protection, GATT permissions
- [ ] Wi-Fi/cellular: config strength, transport encryption, cert validation
- [ ] Proprietary RF/telemetry: auth, encryption, replay/spoofing (contained, lawful TX only)
- [ ] Connectivity loss does not cause unsafe behaviour

## Mobile & cloud/API

- [ ] App: hardcoded secrets, insecure storage, cert pinning, deep links/IPC (OWASP MASTG)
- [ ] API: object-level authz/IDOR, token handling, transport security (OWASP API Top 10)
- [ ] OTA/update endpoints: can they push to a fleet? authn/authz on them?
- [ ] Egress analysis: what does the device talk to by default? (remember Contec CMS8000)

## Web / management

- [ ] OWASP Top 10 against admin/service UIs (authn/authz, injection, XSS, CSRF, SSRF)
- [ ] Control functions (calibration, dosing limits, alarm thresholds) protected

## Wrap-up

- [ ] Findings reproduced with clean, PHI-free evidence
- [ ] Findings chained where relevant
- [ ] Clinical impact assessed (with a clinician/clinical engineer if possible)
- [ ] Scored with CVSS + MITRE medical rubric (vectors recorded)
- [ ] Report written (templates/pentest-report-template.md)
- [ ] Coordinated disclosure started if applicable (docs/08)
- [ ] Retest planned
