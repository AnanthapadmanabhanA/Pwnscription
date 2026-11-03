# 04 · Attack Surface

A surface-by-surface reference for *what to look at* on a medical device and its ecosystem, *what tends to go wrong*, and *what to check*. This is a methodology and checklist resource, it points at techniques and tools, not ready-made exploits. Tools for each surface are in **[05 · Tooling](05-tooling.md)**.

For every surface, keep asking the medical question: **could this affect patient safety, therapy integrity, or essential performance?**

Jump to: [Recon & network](#recon--network-services) · [Clinical protocols](#clinical-protocols-dicom-hl7v2-fhir) · [Hardware](#hardware) · [Firmware](#firmware) · [Wireless & RF](#wireless--rf) · [Mobile](#mobile-companion-apps) · [Cloud & API](#cloud--api) · [Web & management](#web--management-interfaces) · [Supply chain](#supply-chain--third-party-components)

---

## Recon & network services

Most deployed devices sit on a network and expose more than their brochure admits.

**Look at:** exposed TCP/UDP services, management/service ports, remote-support channels, the embedded OS and its age/patch level, default and hardcoded accounts, unnecessary services, and whether the device trusts the network implicitly.

**Common weaknesses:** end-of-life OS (embedded XP/CE/old Linux), default or shared credentials, plaintext management protocols (Telnet/FTP/HTTP), services bound to all interfaces, no network segmentation assumptions baked in, "it's safe because it's on the hospital VLAN" thinking.

**Check:**
- [ ] Full service enumeration; identify OS/firmware versions and their support status.
- [ ] Default/weak/hardcoded credentials on every service.
- [ ] Plaintext protocols carrying sensitive or control data.
- [ ] Whether the device fails safe when the network misbehaves (flood, disconnect, malformed packets) - a monitor or pump must not stop its essential performance.
- [ ] Remote-support/maintenance tunnels and who can reach them.

## Clinical protocols (DICOM, HL7v2, FHIR)

The protocols that make health tech *health* tech. If you come from general pentesting, **this is the surface you most need to learn**, it's usually under-tested and high-impact.

### DICOM (imaging)
Imaging devices, PACS, and workstations speak **DICOM** for images and the network layer (DIMSE / DICOMweb).
- **Watch for:** DICOM services (e.g. the classic port 104/11112) with weak or no association-level authentication, acceptance of any Application Entity Title, directory traversal/parsing bugs in DICOM file handling, and the well-known quirk where **malware can be embedded in the preamble of a DICOM file** while it still opens as a valid image.
- **Check:** can an unauthorized node store/query/retrieve studies? Are AE titles / calling hosts enforced? Does the parser survive malformed objects (fuzzing)? Is DICOMweb (QIDO/WADO/STOW) authenticated and authorized?
- **Practice safely:** stand up **Orthanc** (open-source DICOM server) and talk to it with DCMTK / pynetdicom.

### HL7 v2 (clinical messaging)
The backbone of hospital data exchange (orders, results, ADT). Pipe-delimited, old, and frequently **unauthenticated over MLLP/TCP**.
- **Watch for:** message injection/spoofing (forge an order or a result), tampering in transit (no integrity), and **injection through unvalidated HL7 fields** into downstream systems (the ManiMed/ERNW work documented HL7v2 injection reaching patient monitors).
- **Check:** is MLLP authenticated/encrypted? Are inbound messages validated? Can a crafted message alter what a clinician sees or a device does?
- **Practice safely:** **Mirth Connect / NextGen Connect** as an interface engine to send/receive/replay.

### FHIR (modern APIs)
The REST/JSON successor for health data; increasingly the app/cloud interface.
- **Watch for:** classic API problems with clinical stakes - broken object-level authorization (patient A reads patient B), weak OAuth2/SMART-on-FHIR scopes, mass assignment, injection, and over-broad search.
- **Check:** authorization on every resource and operation; scope enforcement; rate limits; and whether writes can affect care decisions.

## Hardware

Physical access turns a black box grey. This is where firmware extraction and debug access begin.

**Look at:** the PCB and its ICs (MCU/SoC, flash, EEPROM, radios, secure elements), debug/programming headers (**UART, JTAG, SWD**), test points, external storage (SPI/I²C/eMMC/SD), and tamper evidence/response.

**Common weaknesses:** open or recoverable debug ports (unlocked JTAG/SWD), readable external flash (dump SPI flash directly), serial consoles that drop to a root shell or bootloader, no secure boot, no flash encryption, and tamper protection that's cosmetic.

**Check:**
- [ ] Identify ICs (markings → datasheets) and locate debug interfaces.
- [ ] Is JTAG/SWD locked (read-out protection enabled)? Is the UART console authenticated?
- [ ] Can external flash be read off-board (chip-off / in-circuit) to recover firmware/secrets?
- [ ] Is secure boot present and actually enforced? Flash encrypted?
- [ ] Does tamper detection do anything meaningful (and fail safe for the patient)?
- [ ] Fault-injection/glitching resistance *if in scope and on sacrificial units only*.

> Teardown, depotting, and PCB reverse-engineering are standard in this field (see the Hardwear.io "Medical Device Hacking" syllabus in [09](09-learning-and-community.md)). Do destructive work only on spare units with written approval.

## Firmware

Often the richest source of findings: secrets, crypto, update logic, and a map of the whole device.

**Get it by:** vendor update packages, OTA capture, reading external flash (hardware), or a debug interface. Then unpack and analyze.

**Look for:**
- [ ] **Hardcoded secrets** - credentials, keys, certs, API tokens, Wi-Fi PSKs.
- [ ] **Update verification** - are images **signed** and signatures **actually validated**? Can you flash modified/downgraded/unsigned firmware? (A top-tier finding.)
- [ ] **Crypto** - weak/obsolete algorithms, hardcoded/ predictable keys, home-grown crypto, disabled cert validation.
- [ ] **Known-vulnerable components** - cross-reference extracted versions and the SBOM against NVD/OSV.
- [ ] **Debug/dev leftovers** - test accounts, verbose logging, enabled debug services.
- [ ] **Memory-safety bugs** in network-facing parsers (DICOM/HL7/protocol stacks) - the stuff that becomes RCE.

**Workflow:** `binwalk`/`unblob` to carve → identify filesystem/bootloader → `EMBA` or `FACT` for automated triage → `Ghidra` for the interesting binaries. See [05 · Tooling](05-tooling.md) and OWASP **FSTM** (Firmware Security Testing Methodology).

## Wireless & RF

Medical devices talk over a *lot* of radios, including proprietary ones. This surface reaches implants and wearables directly.

### Bluetooth / BLE
Ubiquitous in wearables, pumps, CGMs, and companion-app links.
- **Watch for:** "Just Works" pairing, no app-layer encryption on top of weak link-layer security, static keys, replayable commands, and known BLE-stack bugs (e.g. the **SweynTooth** family affected medical devices).
- **Check:** pairing/bonding strength, whether commands are authenticated and replay-protected, and GATT attribute permissions.

### Wi-Fi / cellular
- **Watch for:** weak/!enterprise Wi-Fi config, cleartext over Wi-Fi, cert-less TLS, exposed cellular management, and roaming/connectivity loss causing unsafe behavior.

### Proprietary RF / telemetry (incl. implants)
Pacemakers, ICDs, and programmers historically used proprietary telemetry bands (e.g. MICS/MedRadio ~400 MHz, and older inductive links).
- **Watch for:** unauthenticated or replayable telemetry commands, weak/no encryption, and programmer↔implant trust that assumes physical proximity = authorization. The Medtronic **Conexus** telemetry findings are the canonical study ([07](07-case-studies-and-cves.md)).
- **Approach:** SDR capture → identify modulation/protocol → analyze for auth/replay/spoofing. **Transmit only in a shielded, contained, lawful setup**; RF emission is heavily regulated and can interfere with real devices.

> ⚠️ RF testing can leak outside your bench and affect real medical devices nearby. Contain transmissions (shielded enclosure), know your local spectrum law, and never transmit on clinical bands in an uncontrolled environment.

## Mobile companion apps

The phone app is often one of the easier things to test, and often one of the weakest.

**Look for:** hardcoded secrets/keys, insecure local storage of ePHI/credentials, weak/absent certificate pinning, broken auth to the cloud, insecure deep links/IPC, and overly trusted BLE command paths.

**Check against OWASP MASVS/MASTG** (the mobile security standard & testing guide). Decompile, inspect storage, intercept traffic, test the API behind it (next section).

## Cloud & API

Modern devices lean on cloud backends for data, config, OTA, and telemetry, a breach here can hit *every* device at once.

**Look for:** broken object-level authorization (patient/device IDOR), weak OAuth/JWT handling, secrets in clients, over-broad device provisioning, tenant isolation failures, and OTA/update endpoints that could push firmware to fleets.

**Check against OWASP API Security Top 10.** The multiplier here is scale and the **OTA path**, can the cloud be used to push a malicious or bad update to many devices? (See the Medfusion/various infusion-pump cloud research.)

## Web & management interfaces

Service portals, admin UIs, embedded web servers.

**Look for:** the usual OWASP Top 10 (authn/authz, injection, XSS, CSRF, SSRF, deserialization) **plus** command interfaces that can change device behavior (calibration, dosing limits, alarm thresholds). A config-change bug here can be a safety bug.

**Check against OWASP WSTG**, with a safety lens on any control the UI exposes.

## Supply chain & third-party components

A single vulnerable library can ripple across vendors and models, which is exactly why SBOMs exist.

**Know the landmark component-level vuln sets** that hit many medical devices at once:
- **URGENT/11** (VxWorks / IPnet TCP-IP stack)
- **Ripple20** (Treck TCP/IP stack)
- **SweynTooth** (BLE SoC stacks)
- **Access:7** (PTC Axeda remote management agent)

**Check:** build/maintain an SBOM; map components to NVD/OSV; track whether the vendor monitors and patches third-party components (an FDA § 524B obligation); and watch CISA ICSMA for component-driven advisories.

---

### Surface → impact quick map

| Surface | Typical worst-case patient impact |
|---|---|
| Firmware update bypass | Attacker-controlled device behavior at fleet scale |
| Proprietary RF / telemetry | Unauthorized therapy change to an implant/pump |
| HL7/DICOM integrity | Wrong data in front of a clinician → wrong decision |
| Network/availability | Monitor/pump stops essential performance |
| Cloud/OTA | Mass compromise across a device fleet |
| Mobile → BLE command path | Remote manipulation of a wearable's therapy |

Next: **[05 · Tooling →](05-tooling.md)**
