# Medical Device Threat Model - Template

> A lightweight structure based on the four threat-modeling questions used in
> the MITRE/MDIC Playbook for Threat Modeling Medical Devices:
> https://www.mitre.org/sites/default/files/2021-11/Playbook-for-Threat-Modeling-Medical-Devices.pdf
>
> Center everything on patient-safety impact.

---

## 1. What are we working on?

**System description**
> Intended use, clinical context, and the users (clinician, patient, service tech).

**System / data-flow diagram**
> Device, hubs/programmers, network, interface engine, EHR/PACS, cloud, mobile app.
> Mark trust boundaries. (See docs/01-fundamentals.md for the ecosystem picture.)

**Assets**

| Asset | Type | Why it matters (safety / data / function) |
|-------|------|-------------------------------------------|
| Therapy delivery (dose/shock/vent) | Function | Direct patient harm if altered |
| Essential performance | Function | Harm if degraded or stopped |
| Patient data (ePHI) | Data | Privacy / regulatory |
| Firmware / update path | Function | Fleet-wide compromise if abused |
| Credentials / keys | Secret | Enables other attacks |

**Interfaces / entry points**
> List every one: USB, UART/JTAG/SWD, Ethernet, Wi-Fi, BLE, cellular, proprietary RF,
> removable media, DICOM/HL7/FHIR, web/admin, cloud API, mobile.

## 2. What can go wrong?

> Walk each interface and asset. STRIDE is a useful prompt (Spoofing, Tampering,
> Repudiation, Information disclosure, Denial of service, Elevation of privilege),
> but the organizing question is always patient impact.

| ID | Threat | Interface / asset | STRIDE | Patient-safety impact | Initial likelihood |
|----|--------|-------------------|--------|-----------------------|--------------------|
| T1 | Unsigned firmware accepted | Update path | Tampering | Attacker-controlled behaviour | |
| T2 | Replayable RF command | Proprietary RF | Spoofing | Unauthorized therapy change | |
| T3 | HL7 message injection | Clinical protocol | Tampering | Wrong data to clinician | |

## 3. What are we going to do about it?

| Threat ID | Existing control | Gap | Proposed mitigation | Residual risk |
|-----------|------------------|-----|---------------------|---------------|
| T1 | | | | |

> Tie mitigations back to requirements/standards where possible
> (see docs/02-regulations-and-standards.md).

## 4. Did we do a good job?

- [ ] Every interface and asset considered
- [ ] Each threat has a patient-safety impact statement
- [ ] Mitigations mapped to controls / requirements
- [ ] Residual risk reviewed and accepted by the right owner
- [ ] Threat model fed the test plan (see pentest-report-template.md)
- [ ] Model will be revisited when the design or threat landscape changes
