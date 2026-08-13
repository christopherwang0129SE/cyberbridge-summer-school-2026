# CyberBridge Summer School 2026 — Notes

Personal notes from CyberBridge Summer School (Aalborg Universitet i København, uge 32–33, august 2026). Focus: bridging a civil engineering background (water/wastewater, MIKE+, AutoCAD Civil 3D) into OT/ICS cybersecurity.

## Status

| Day | Topic | Instructor | Status |
|---|---|---|---|
| Pre-course | Program relevance analysis | — | ✅ |
| W32 Day 1 | CIS18 Controls + Teknisk Operationel Sikkerhed | Peter Koch, Conscia | ✅ |
| W32 Day 2 | Reverse Engineering & Binary Exploitation in OT | Alexander Koefod & Sofia Rita Tocco, ICSRange | ✅ |
| W32 Day 3 | Vulnerability Management | Ronni Hessellund, The Tech Collective | ✅ |
| W32 Day 4 | Android sikkerhed og Attack Surface | William Ben Embarek, Aikido | ✅ |
| W33 Day 1 | Forensics | Olivia Wenya Chen, Campfire Security | ⬜ not covered yet |
| W33 Day 2 | OWASP Top 10 | Olivia Wenya Chen, Campfire Security | ⬜ not covered yet |
| W33 Day 3 | CV og bliv klar til dit første job | Andrada Son, SectorCert | ✅ |
| W33 Day 4 | Open Source Intelligence / CTI | David Clayton, The Tech Collective | ✅ |
| W33 Day 5 | Bug Bounty | Emil Hørning, Defend Denmark | ⬜ tomorrow |

## Structure

```
00-pre-course/            Why the program is relevant to a civil eng. → OT security pivot
week-32/
  day-1-cis18-controls/       CIS18 framework, IT vs OT, efficiency dimensions, MITRE ATT&CK/CDM
  day-2-reverse-engineering-ot/   SCADA, firmware architecture, binwalk/dd, bare-metal RE, anti-RE
  day-3-vulnerability-management/ CVE/CVSS/EPSS/KEV/NVD/CWE, Log4Shell, Heartbleed, patch process
  day-4-android-security/         APK structure, permissions, SELinux, exported components, WebView
week-33/
  day-1-forensics/            (placeholder)
  day-2-owasp-top-10/         (placeholder)
  day-3-cv-and-career/        CV structure/tone, skills section, certification roadmap, homelab
  day-4-osint-cti/            Threat model, CTI types, Obsidian, Novo Nordisk/FulcrumSec case study
  day-5-bug-bounty/           (placeholder — last day, not yet run)
reference/
  mitre-owasp-profiles.md     Org profiles: MITRE ATT&CK and OWASP
```

## Recurring threads

- **The civil engineer → OT security angle** is the throughline — SCADA/Purdue model, IEC 62443, water/wastewater infrastructure as critical infrastructure (SektorCERT context from the CV day).
- **"Same checkbox, different efficiency"** (day 1) resurfaces constantly: CVSS-only prioritization, compliance vs. real security, patch-alone-isn't-enough (PuTTY/Heartbleed).
- **Bookmarked tools/resources** are scattered across the day files rather than centralized yet — worth consolidating into a single `resources.md` at some point.

## Certifications/training bookmarked (not yet started)

eJPT (INE Security) · CyLab Security Academy / PicoGym · SANS EMEA · OffSec (OSCP/OSED/OSCE³) · itucation.dk (Security+/CEH) · Cybrary · Antisyphon Training

## Community

BSides København · OWASP København (both co-led by Andrada Son, SectorCert)

---

*Repo scaffolded with Claude, filled in day-by-day during the course. Bug Bounty (day 5) and the two missing days should be added once covered.*
