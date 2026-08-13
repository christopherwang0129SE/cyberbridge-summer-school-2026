# CyberBridge Summer School — Dag 1 Resumé

**Tirsdag d. 4. august 2026 | CIS18 Kontrollerne + Teknisk Operationel Sikkerhed**
**Underviser:** Peter Koch, Conscia

---

## 1. Hvorfor kløften eksisterer (IT vs. OT)

Kernen i hele dagen: IT-sikkerhed og OT-sikkerhed (industrielle kontrolsystemer) udviklede sig som separate discipliner, fordi:

- **CIA-triaden er vendt om**: IT prioriterer Confidentiality > Integrity > Availability. OT prioriterer **Availability > Integrity > Confidentiality** — en pumpestation kan ikke bare "tages offline for patching".
- **Forskellige levetider**: IT-udstyr skiftes hvert 3.-5. år; PLC'er og RTU'er kører 15-25 år, ofte på firmware, der aldrig blev designet til nutidens trusler.
- **Protokoldesign**: Modbus, DNP3, Profinet blev bygget til isolerede, tillidsfulde netværk — ofte helt uden indbygget autentificering.
- **Organisatoriske siloer**: IT-sikkerhed og drift/ingeniørteams har historisk forskellige rapporteringsveje, sprog og risikotolerance.
- **IT/OT-konvergens**: "Air gap"-antagelsen holder ikke længere — fjernovervågning og Industry 4.0 kobler i stigende grad de to netværk sammen, uden at sikkerhedsmodenheden har fulgt med.

## 2. Samme afkrydsning — vidt forskellig efficiens

En CIS-kontrol kan være "compliant" (afkrydset) i både IT og OT, men den reelle beskyttelse, den giver, kan variere enormt:

| Kontrol | IT-udførelse | OT-udførelse |
|---|---|---|
| Vulnerability Management (7) | Automatisk scanning, patch inden for dage | Kan sjældent patches live — kvartalsvis review + kompenserende kontroller |
| Access Control (6) | MFA, korte sessioner, central identitet | Legacy HMI/PLC understøtter ofte ikke MFA — fysisk adgangskontrol som erstatning |
| Network Monitoring (13) | Aktiv scanning, realtids-IDS/IPS | Skal være **passiv** — aktiv scanning kan crashe skrøbelige PLC'er |

**Pointen:** compliance ≠ reel sikkerhed. Afstanden mellem de to er markant større i OT end i IT.

## 3. De syv efficiens-dimensioner

(Kilde: Peter Kochs egen artikel "Compliant, men sikker?", Conscia)

1. **Funktionel dybde** — gør løsningen mere end minimumskravet?
2. **Dækning af udrulning** — dækker det alle relevante aktiver, eller kun en del?
3. **Konfigurationsdybde** — kører det på standardindstillinger, eller er det hærdet?
4. **Integration og korrelation** — deler kontrollen data med andre kontroller?
5. **Tuning og baselining** — er normal adfærd profileret, så afvigelser opdages?
6. **Responsautomatisering** — kan den handle selv, eller kun alarmere?
7. **Kontinuerlig forbedring** — bliver den revideret løbende, eller "sat og glemt"?

## 4. Sådan lukker du kløften (6 anbefalinger)

1. Etabler målbare efficiens-baselines pr. kontrol (brug MITRE ATT&CK til at prioritere ud fra reelle trusler)
2. Valider kontroller mod realistiske angrebsscenarier (purple team, BAS-værktøjer, pentest)
3. Audit konfigurationsdybde, ikke bare tilstedeværelse (brug CIS Benchmarks)
4. Vurder leverandører ud fra egen trusselsprofil, ikke deres marketingtal
5. Behandl integration som et kernekrav — isolerede kontroller er svagere
6. Genbesøg årligt — en kontrol, der stoppede 90% af angreb for 2 år siden, kan være nede på 40% i dag

## 5. Arbejdseksempel: Fra risiko til kontrol

**Scenarie:** Internet-eksponeret PLC på en pumpestation har en ukendt/upatchet sårbarhed.

1. **Risiko identificeret** → CIS Kontrol 7 (Vulnerability Management)
2. **Direkte patching ikke muligt** (kan ikke genstarte live PLC) → kompenserende kontrol: segmentering (12/13)
3. **Scoret på efficiens-dimensionerne**: Er segmenteringen fuldt dækkende? Stramt konfigureret? Er normal trafik baseline't, så en anomali reelt opdages?
4. **Valideres**: Test om PLC'en faktisk kan nås udefra i et kontrolleret forsøg

## 6. Tema A — Fundament: CIS Kontrol #1 (Asset Inventory)

**Aktivoverblik bærer alt** — næsten alle andre 17 kontroller afhænger af denne:
- **1.1** Før komplet, opdateret aktivliste (fysisk, virtuel, cloud, fjern) — opdateres min. hvert halve år
- **1.2** Proces for uautoriserede enheder (fjern/nægt/karantæne) — tjekkes ugentligt
- **1.3** Aktiv opdagelse (netværksscanning) — dagligt
- **1.4** DHCP-logging til opdatering af aktivliste — ugentligt
- **1.5** Passiv opdagelse (lytter uden at "banke på") — ugentligt

**OT-nuance:** Aktiv scanning (1.3) kan crashe skrøbelige PLC'er → passiv discovery (1.5) foretrækkes i OT.

## 7. Beskyt og hærd (Kontrol #3 + #4)

- **Kontrol 3 — Data Protection**: klassifikation, dataopgørelse, kryptering (i hvile/under transport), retention/disposal
- **Kontrol 4 — Secure Configuration**: fjern unødvendige services, standardiserede baselines (CIS Benchmarks), automatisk session-lås, aktiv firewall-styring

**OT-nuance:** Data Protection handler i OT mest om *integritet* (er sensordata manipuleret?), ikke fortrolighed. Hærdning bliver ofte netværksbaseret (segmentering) i stedet for konfiguration direkte på enheden.

## 8. Overvåg & forsvar (Kontrol #8 + #13)

- **Kontrol 8 — Audit Log Management**: central logindsamling, tidssynkronisering, tilstrækkelig opbevaringstid (90+ dage anbefalet)
- **Kontrol 13 — Network Monitoring and Defense**: trafikanalyse, IDS/IPS, DNS-filtrering, segmentering som forsvar

**OT-nuance:** Passiv overvågning (detektion/alarmering) foretrækkes frem for aktiv blokering, som kan forstyrre en realtidskritisk proces.

## 9. De 18 CIS-kontroller (fuld liste)

**Basic (1-6):** 1) Asset Inventory · 2) Software Inventory · 3) Data Protection · 4) Secure Configuration · 5) Account Management · 6) Access Control Management

**Foundational (7-16):** 7) Vulnerability Management · 8) Audit Log Management · 9) Email/Browser Protections · 10) Malware Defenses · 11) Data Recovery · 12) Network Infrastructure Management · 13) Network Monitoring and Defense · 14) Security Awareness Training · 15) Service Provider Management · 16) Application Software Security

**Organizational (17-18):** 17) Incident Response Management · 18) Penetration Testing

## 10. MITRE ATT&CK + Community Defense Model

- **ATT&CK-struktur**: Tactics (angriberens mål) → Techniques/sub-techniques (konkrete metoder). Matricer for Enterprise, Mobile og **ICS** (relevant for i morgen).
- **Angrebsfaser**: Reconnaissance → Initial Access → Execution → Persistence → Privilege Escalation → Defense Evasion → Credential Access → Discovery → Lateral Movement → Collection → C2 → Exfiltration → Impact
- **Community Defense Model (CDM)**: kortlægger CIS Safeguards direkte til ATT&CK-teknikker. IG1-safeguards alene forsvarer mod ~74% af alle teknikker; alle CIS Safeguards tilsammen dækker ~86%. CDM v3.0 ("Active Defense Lifecycle") er på vej i 2026.

**Workshop-øvelse:** Mappede et faktura-phishing → ransomware-scenarie til 4-6 ATT&CK-teknikker og tilhørende CIS-kontroller, markerede hvilke der lå i IG1, og identificerede en skjult afhængighed (fx: backup er ligegyldig, hvis samme kompromitterede admin-konto kan slette den).

## 11. Blok 4 — Rammer, der spiller sammen

- **CIS vs. NIST CSF**: NIST giver strukturen (Govern/Identify/Protect/Detect/Respond/Recover), CIS giver den konkrete "opskrift"
- **CIS vs. ISO 27001**: ISO 27001 er en certificerbar ISMS-standard (93 Annex A-kontroller i 4 temaer: Organisatoriske 37, Personale 8, Fysiske 14, Teknologiske 34). CIS dækker skønsmæssigt 60-70% af de tekniske Annex A-kontroller. ISO beskriver *hvad*, CIS beskriver *hvordan* — de konkurrerer ikke, de supplerer hinanden.
- **NIS2** (dansk/EU-kontekst, relevant for vandforsyning): stiller krav uden tekniske detaljer — CIS fungerer som det konkrete "hvordan"

**Skjulte afhængigheder — opsummeret:**
- Data Recovery (11) ← Access Control (6)
- MFA (6) ← Secure Configuration (4) (legacy-protokoller kan omgå MFA)
- Network Monitoring (13) ← Audit Log Management (8)
- Vulnerability Management (7) ← Asset Inventory (1)

## 12. Dagens kerneprincipper (opsamling)

1. **Aktivoverblik bærer alt** — Kontrol #1 er afhængighedsroden for næsten alle andre kontroller
2. **Logning er forudsætning for detektion** — Kontrol #13 er ubrugelig uden Kontrol #8 som fundament

**Den røde tråd gennem hele dagen:** rammeværker giver strukturen, men reel sikkerhed opstår i det, der binder kontrollerne sammen — ikke i den enkelte afkrydsning.

---

## Nøglekilder

- [Compliant, men sikker? — Peter Koch, Conscia](https://conscia.com/dk/blog/compliant-men-sikker/)
- [The 18 CIS Critical Security Controls](https://www.cisecurity.org/controls/cis-controls-list)
- [MITRE ATT&CK](https://attack.mitre.org/)
- [CIS Community Defense Model v3](https://www.cisecurity.org/insights/webinar/evolving-cyber-defenses-cdm-v3)
- [Sådan sikrer I jeres OT-miljø og overholder NIS2-direktivet](https://conscia.com/dk/blog/saadan-sikrer-i-jeres-ot-miljoe-og-overholder-nis2-direktivet/)
- [CIS Controls v8 Mapping to ISO/IEC 27001:2022](https://www.cisecurity.org/insights/white-papers/cis-controls-v8-mapping-to-iso-iec-27001-2022)
