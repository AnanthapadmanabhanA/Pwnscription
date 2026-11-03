# 05 · Tooling

A practical toolbox, grouped by the surface it serves ([04 · Attack Surface](04-attack-surface.md)). Each entry says **what the tool is for** so you can choose, not collect. Nearly all are free/open-source; hardware is called out.

> Tools are neutral. Everything here has legitimate engineering/defensive uses. Use them only under authorization, on lab units, per [03 · Methodology](03-testing-methodology.md).

---

## General offensive/recon
- **Kali Linux / Parrot** - ready-made distros with most of the below. <https://www.kali.org/>
- **Nmap** - service/OS enumeration. <https://nmap.org/>
- **Wireshark** - packet analysis; has DICOM, HL7, and many protocol dissectors. <https://www.wireshark.org/>
- **Burp Suite** / **OWASP ZAP** - web/API interception & testing. <https://portswigger.net/burp> · <https://www.zaproxy.org/>
- **Metasploit Framework** - exploitation framework (for known issues, in the lab). <https://github.com/rapid7/metasploit-framework>
- **Shodan / Censys** - find internet-exposed devices/protocols (recon on *your own* assets; never attack what you find). <https://www.shodan.io/>

## Clinical protocols

### DICOM
- **DCMTK** - the reference DICOM toolkit (store/query/retrieve, parse). <https://dicom.offis.de/dcmtk/>
- **pynetdicom** + **pydicom** - scriptable DICOM networking/parsing in Python. <https://github.com/pydicom/pynetdicom>
- **Orthanc** - lightweight open-source DICOM server; perfect lab target/receiver. <https://www.orthanc-server.com/>
- **Evil-DICOM** - library for manipulating DICOM objects (useful for malformed-object/parser testing). <https://github.com/rexcardan/Evil-DICOM>

### HL7 v2 / FHIR / LIS
- **Mirth Connect / NextGen Connect** - interface engine to send, receive, transform, and replay HL7/FHIR. <https://github.com/nextgenhealthcare/connect>
- **HAPI** (Java) / **python-hl7** - build and parse HL7 v2 messages. <https://hapifhir.github.io/hapi-hl7v2/> · <https://python-hl7.readthedocs.io/>
- **HAPI FHIR** - FHIR server/client for standing up and testing FHIR. <https://hapifhir.io/>
- **MedAudit** - purpose-built helper for HL7 scanning/traffic analysis of medical infra. <https://github.com/anirudhduggal/medaudit>

## Firmware analysis
- **binwalk** - carve and identify firmware images. <https://github.com/ReFirmLabs/binwalk>
- **unblob** - robust extraction of many container/filesystem formats. <https://github.com/onekey-sec/unblob>
- **EMBA** - automated firmware security analyzer (great first-pass triage). <https://github.com/e-m-b-a/emba>
- **FACT (Firmware Analysis and Comparison Tool)** - full firmware analysis platform. <https://github.com/fkie-cad/FACT_core>
- **Ghidra** - the open-source reverse-engineering suite for the binaries that matter. <https://github.com/NationalSecurityAgency/ghidra>
- **radare2 / Cutter** - alternative RE tooling. <https://github.com/radareorg/radare2>
- **firmwalker** - grep firmware filesystems for secrets/interesting files. <https://github.com/craigz28/firmwalker>
- **OWASP FSTM** - the *methodology* to drive all of the above. <https://github.com/scriptingxss/owasp-fstm>

## Hardware / embedded
- **Logic analyzer** - Saleae or cheap FX2-based clones (use with **sigrok/PulseView**). <https://sigrok.org/>
- **Bus Pirate / FT2232H breakouts** - talk to UART/SPI/I²C. 
- **JTAGulator** - find JTAG/UART pinouts on unknown boards. <http://www.grandideastudio.com/jtagulator/>
- **OpenOCD** - JTAG/SWD debugging and flash read/write. <https://openocd.org/>
- **flashrom** - read/write SPI flash chips (recover firmware off-chip). <https://www.flashrom.org/>
- **ChipWhisperer** - side-channel and fault-injection/glitching research (sacrificial units only). <https://github.com/newaetech/chipwhisperer>
- **Flipper Zero** - handy multi-tool for sub-GHz/NFC/IR/GPIO poking during recon.

## Wireless & RF
### Bluetooth / BLE
- **Nordic nRF52840 + Sniffle** - excellent open BLE sniffer. <https://github.com/nccgroup/Sniffle>
- **nRF Connect** (mobile/desktop) - explore GATT, read/write characteristics. <https://www.nordicsemi.com/Products/Development-tools/nRF-Connect-for-mobile>
- **GATTacker / BtleJuice** - BLE MITM research frameworks.
- **Wireshark + nRF sniffer plugin** - decode captured BLE.

### SDR / sub-GHz / proprietary RF
- **GNU Radio** - the signal-processing workbench for custom protocols. <https://www.gnuradio.org/>
- **Universal Radio Hacker (URH)** - capture, analyze, and (lawfully, contained) replay wireless protocols. <https://github.com/jopohl/urh>
- **SDR hardware** - RTL-SDR (receive, cheap starter), **HackRF One**, **bladeRF**, **LimeSDR** (transmit-capable; shielded/lawful use only). <https://greatscottgadgets.com/hackrf/>

> ⚠️ Transmitting on medical/clinical bands outside a shielded enclosure can interfere with real devices and is often illegal. Receive-first; transmit only contained and lawful.

## Mobile
- **OWASP MASTG / MASVS** - the mobile testing methodology & standard. <https://mas.owasp.org/>
- **MobSF** - automated static/dynamic analysis of Android/iOS apps. <https://github.com/MobSF/Mobile-Security-Framework-MobSF>
- **Frida / Objection** - runtime instrumentation (hook crypto, bypass pinning in your lab). <https://frida.re/>
- **apktool / jadx** - decompile and inspect Android apps. <https://github.com/skylot/jadx>

## Cloud & API
- **Burp / ZAP** (above) for API testing.
- **OWASP API Security project** - the API Top 10 to test against. <https://owasp.org/www-project-api-security/>
- **Postman / mitmproxy** - craft/inspect API calls and device↔cloud traffic. <https://mitmproxy.org/>

## Scoring, SBOM & triage
- **MITRE Medical-Device CVSS Rubric (+ calculators)** - score with patient-safety impact. <https://www.mitre.org/md-cvss-rubric>
- **FIRST CVSS calculator** - base scoring. <https://www.first.org/cvss/calculator/>
- **Syft / Trivy / Grype** - generate SBOMs and match components to known CVEs. <https://github.com/anchore/syft> · <https://github.com/aquasecurity/trivy>
- **OSV / NVD** - vulnerability databases to cross-reference SBOM components. <https://osv.dev/> · <https://nvd.nist.gov/>
- **dependency-track** - monitor SBOM components over time. <https://dependencytrack.org/>

## Reference tool collections
- **MITRE CVSS Rubric Tools (GitHub)** - official calculators for the medical rubric. (Linked from <https://www.mitre.org/md-cvss-rubric>.)
- **awesome-medsec** - community link list of medical-security tools/talks/papers. <https://github.com/zseha/awesome-medsec>

---

### Minimum starter kit (if you're buying one of everything)

| Need | Starter pick |
|---|---|
| Logic/signal | Saleae-class analyzer + PulseView |
| UART/SPI/I²C | FT2232H breakout + flashrom |
| JTAG/SWD | Cheap SWD probe + OpenOCD |
| BLE | nRF52840 dongle + Sniffle |
| SDR | RTL-SDR to learn; HackRF to transmit (contained) |
| Firmware | binwalk + unblob + EMBA + Ghidra |
| Protocols | Orthanc + Mirth Connect + Wireshark |
| Mobile/API | MobSF + Burp/ZAP + Frida |

Next: **[06 · Labs, CTFs & Practice →](06-labs-ctfs-and-practice.md)**
