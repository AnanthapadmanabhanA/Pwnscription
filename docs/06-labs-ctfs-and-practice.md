# 06 · Labs, CTFs & Practice

You learn this field by doing, but "doing" on real medical devices is legally and ethically fraught. This page is about practicing **legally and safely**: CTFs, simulators, open-source stand-ins, and building a home lab.

> ⚠️ Never practice on a device that is or could be in clinical use, and never on hardware you don't own or aren't authorized to test. Buy your own, use simulators, or play in sanctioned events.

---

## Sanctioned events & CTFs

### DEF CON Biohacking Village - Device Lab & "Hospital Under Siege" CTF
The single best place to get hands-on legally. Manufacturers bring **real devices** into a **Device Lab** and invite researchers to test and disclose on-site; the **Hospital Under Siege** CTF runs real and simulated devices and clinical protocols (DICOM, HL7, FHIR) in a mock hospital ("St. Elvis").
- Biohacking Village: <https://www.villageb.io/>
- It runs in collaboration with **I Am The Cavalry**; the Device Lab is explicitly a high-trust, coordinated-disclosure environment - exactly the etiquette you want to build.
- Even if you can't attend, read past CTF write-ups and the talks (DEF CON media server hosts them).

### Training courses with hands-on labs
- **Hardwear.io - "Medical Device Hacking and Defense"** - teardown, depotting, debug ports, BLE/Wi-Fi/cellular/custom RF, mobile/cloud, kiosk escapes, PoV/PoC development. A good syllabus to learn *from* even if you don't take it. <https://hardwear.io/>
- **DEF CON Biohacking Village "Medical Device Hacking" workshops** (101/201) - periodically offered.

## Open-source stand-ins (build the ecosystem, not the patient)

You can replicate most of the *software/protocol* surface with free tools, no patient, no risk:

| Replicate | With | Practice |
|---|---|---|
| **DICOM** node / PACS | **Orthanc** (<https://www.orthanc-server.com/>) + **DCMTK**/pynetdicom | Query/retrieve auth, malformed-object parsing, DICOMweb authz |
| **HL7 v2 / FHIR** flow | **Mirth Connect** + **HAPI** / python-hl7 / HAPI FHIR | Message injection/spoofing, field injection, MLLP security |
| **Firmware analysis** | any cheap **non-medical IoT device** + binwalk/unblob/EMBA/Ghidra | Extraction, secrets, update verification — transferable skills |
| **BLE device** | **nRF52840** dev kit running a sample peripheral | Pairing, GATT permissions, replay |
| **Web/API** | the device's companion app + its backend (if you own the device) | MASTG / API Top 10 |

> The protocols (DICOM/HL7/FHIR) and firmware/RF *techniques* are the transferable core. Master them on safe targets and you're 80% of the way to any real device.

## General security practice grounds (build the base skills)

Medical-device testing is applied embedded/web/network/RF/mobile security. If your base skills are thin, grind them here first:

- **Firmware/IoT/embedded:** OWASP **FSTM** labs, IoT CTFs, the **IoTGoat** deliberately-vulnerable firmware. 
- **Binary/pwn:** pwn.college, ROP Emporium, picoCTF, pwnable.* for exploitation fundamentals.
- **Web/API:** PortSwigger Web Security Academy, OWASP Juice Shop, crAPI (for API/IDOR).
- **Mobile:** OWASP MASTG crackmes, injured-android-style targets.
- **RF/SDR:** work through GNU Radio tutorials and capture benign signals before anything clinical.

## Buying practice hardware (responsibly)

Second-hand medical hardware appears on auction sites. If you go this route:
- **Only ever test units you own, on an isolated bench, never patient-connected.** Decommissioned ≠ sanitized - wipe any residual PHI you encounter and don't exfiltrate it.
- Favor **non-implantable, non-actuating** gear to start (patient monitors, older PACS workstations, lab instruments) over anything that doses/shocks/ventilates.
- Keep **spare/sacrificial units** for destructive work.
- Research the device's status first; some are export- or sale-controlled.

## Build a home lab bench

The minimum viable medical-device bench (from [03 · Methodology](03-testing-methodology.md), with tool links in [05](05-tooling.md)):

```
┌─ Isolated test network (managed switch + VLANs, SPAN port) ── never the real LAN
│
├─ Hardware bench: logic analyzer · USB-UART · JTAG/SWD probe · flashrom rig ·
│                  soldering/rework · bench PSU · ESD-safe tools
│
├─ RF kit: RTL-SDR (learn) · HackRF/bladeRF (transmit, shielded) · nRF52840 (BLE) ·
│          RF-shielded enclosure for any transmit testing
│
├─ Analysis workstation: binwalk/unblob/EMBA/FACT · Ghidra · Wireshark(+DICOM/HL7) ·
│                         MobSF/Frida · Burp/ZAP
│
├─ Protocol stand-ins: Orthanc (DICOM) · Mirth Connect (HL7/FHIR)
│
└─ Safety sim: test lung · saline bags · dummy loads  ── so devices "run" with no patient
```

**Network hygiene:** the test VLAN must not route to any clinical or corporate network. Assume devices will do noisy/unsafe things; contain them.

## Keeping current (a weekly practice)

Treat these like a newsfeed, reading real advisories *is* practice:
- **CISA ICS Medical Advisories (ICSMA)** - new disclosed vulns, with technical detail and affected versions. <https://www.cisa.gov/news-events/cybersecurity-advisories>
- **FDA safety communications** for cybersecurity. <https://www.fda.gov/medical-devices/safety-communications>
- **CVE/NVD/OSV** searches for your target vendors/components.
- Conference talk archives (DEF CON Biohacking Village, Hardwear.io, Black Hat, Troopers) - see [09](09-learning-and-community.md).

Next: **[07 · Case Studies & CVEs →](07-case-studies-and-cves.md)**
