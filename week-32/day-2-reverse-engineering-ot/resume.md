# CyberBridge Summer School — Dag 2 Resumé

**Onsdag d. 5. august 2026 | Reverse Engineering & Binary Exploitation in OT**
**Undervisere:** Alexander Koefod og Sofia Rita Tocco, ICSRange

---

## 1. IT vs. OT — den tekniske dybde

Genopfriskning fra dag 1 (CIA-triaden vendt om), udvidet med hvorfor standard IT-RE-værktøjer ikke bare virker på OT:

- **Proprietære compilere/formater**: Siemens (STEP 7), Allen-Bradley (Studio 5000), CODESYS — standard ELF/PE-værktøjer (IDA, Ghidra) forstår dem ikke uden tilpasning
- **PLC-programmeringssprog**: IEC 61131-3 (ladder logic, structured text, function block diagrams) — kompileres til embedded maskinkode (ofte ARM/MIPS)
- **Manglende moderne mitigations**: ingen ASLR/stack canaries/DEP i embedded firmware pga. realtids-determinisme og ressourcebegrænsninger
- **Konsekvens af exploit**: fysisk, ikke bare digital (ventil, motor, sikkerhedskritisk proces)
- **ICSRange's platform**: en dedikeret, simuleret OT security range — samme princip som en sandbox

## 2. System under Consideration (SuC) — IEC 62443-3-2

Det første skridt i enhver risikovurdering: definér præcist, hvad "systemet" er (hardware, software, netværk, mennesker), før risiko kan analyseres.

- **Zones & Conduits**: opdel SuC i zoner (aktiver med samme sikkerhedskrav) og conduits (kontrollerede kommunikationsveje mellem zoner)
- Pointe: det handler ikke kun om at *se* systemet, men at *forstå* det

## 3. Sikkerhedstest-omfang: hvad skal dækkes

Firmware · Webapplikation · Desktop-applikation (engineering workstation-software) · Netværksudstyr · Dev-miljøer · **CI/CD**

**CI/CD-uddybning**: supply chain-risiko (angreb på selve build-pipelinen, ikke koden), secrets-eksponering, adgangskontrol til builds, dependency-risiko (SCA), artifact-integritet (signeret firmware). Kobling til dag 1: firmwaresikkerhed *afhænger af* CI/CD-pipelinesikkerhed.

## 4. Organisationsprofiler: MITRE og OWASP

- **MITRE**: non-profit (1958), driver ATT&CK-frameworket (2013/2015). Tactics → Techniques/sub-techniques. Matricer: Enterprise, Mobile, **ICS**.
- **OWASP**: non-profit fond (2001/2004), community-drevet. Kendt for OWASP Top 10 (kritiske webapp-sårbarheder, opdateres hvert 3.-4. år).

## 5. Netværksovervågning: SPAN vs. TAP

- **SPAN-port** (port mirroring): kopierer trafik til en overvågningsport — billig, indbygget, men risiko for droppede pakker ved høj belastning
- **Network TAP**: dedikeret hardware, fuldstændig, uændret trafikkopi — dyrere, men mere pålidelig
- OT foretrækker passiv overvågning generelt (ikke-forstyrrende)

## 6. Responsible Disclosure

Tre kerneprincipper:
1. **Keep a clear audit trail** — dokumentér alt (juridisk beskyttelse + procesydelse, typisk 90-dages norm)
2. **Avoid confrontation** — samarbejdsorienteret tone, selv ved leverandør-modvilje
3. **Provide Proof of Concept** — nok bevis til verifikation, ikke en fuldt våbengjort exploit

OT-kanal: CISA's Coordinated Vulnerability Disclosure-program, ICS-CERT-rådgivninger.

## 7. SCADA-arkitektur (genopfriskning)

Feltenheder → PLC/RTU (lokal kontrol) → kommunikationsnetværk (Modbus, DNP3 m.fl.) → SCADA-master/HMI → historian.

## 8. Modbus TCP vs. RTU

- **RTU**: seriel (RS-232/485), CRC-fejlkontrol, maks. 247 enheder, kabelbegrænset
- **TCP**: samme funktionskoder/registre, wrappet i TCP/IP med MBAP-header, port 502, ingen distancebegrænsning
- **Sikkerhed**: ingen indbygget autentificering i nogen af varianterne — TCP fjerner kravet om fysisk adgang til kablet (jf. Lviv-fjernvarme-angrebet). Modbus/TCP Security (2018, port 802) findes, men er langt fra universelt udrullet.

## 9. OSINT/overvågnings-værktøjskæden

1. **Shodan** — søgemaskine for internetforbundne enheder/SCADA (port 502, 102, 47808, 44818 osv.)
2. **GreyNoise** — filtrerer internettets baggrundsstøj fra reel målrettet aktivitet, reducerer alarm-træthed op til 25%
3. **AbuseIPDB** — community-baseret IP-omdømme, abuse confidence score (0-100%)
4. **Honeypots (Conpot)** — ICS-lokkesystem, bygget af en norsk/dansk Honeynet Project-duo; balance mellem realisme og opdagelsesrisiko

## 10. Firmware-arkitektur

**Boot-stadier**: BIOS (minimal hardware-init) → Bootloader (U-Boot; secure boot/signaturverifikation lever her)

**Arkitekturvalg**:
- **Bare-metal**: ingen OS, direkte hardwarekontrol, mindst fodaftryk
- **RTOS**: deterministisk timing, garanterede svartider — hvor de fleste PLC'er lever (FreeRTOS, VxWorks, QNX, Zephyr)
- **Kernel + RootFS**: fuld embedded Linux, MMU-krævende, mest kompleks

**SquashFS**: komprimeret (ikke krypteret!), read-only rodfilsystem — standard i embedded Linux. Genkendes af Binwalk via magic bytes `68 73 71 73`.

## 11. Unpacking-værktøjer

- **Binwalk**: signatur- og entropi-baseret identifikation/udpakning
- **fdisk**: partitionstabel-detektion (`fdisk -lu`) — hurtigere alternativ/supplement til Binwalk
- **dd**: manuel, byte-præcis udtrækning (`dd if=... bs=1 skip=OFFSET of=...`)

**Manuel offset-finding — nøglepunkter:**
- Kend magic bytes udenad (ELF: `7f 45 4c 46`, SquashFS: `68 73 71 73`, gzip: `1f 8b`)
- Skeln fil-offset fra load-adresse
- Brug interne længdefelter, når de findes
- Sektioner er alignet til flash-blokstørrelser (4K/64K/128K)
- Iterativ validering (udtræk → `file`/udpak → justér), ikke gætteri i blinde
- `strings -tx` for gratis kontekst

## 12. Bytecode (.pyc, .luac)

Mellemliggende repræsentation eksekveret af en VM, ikke direkte af CPU'en. Moderne embedded systemer bruger i stigende grad Python/Lua-scripting oven på bare-metal/RTOS-laget.

- **.pyc**: Python-bytecode, versionsspecifikt format, dekompileres med `uncompyle6`
- **.luac**: Lua-bytecode, udbredt i netværksudstyr, registerbaseret (bevarer mere struktur end stack-baseret bytecode)
- **Sikkerhedspointe**: bytecode er markant lettere at reverse-engineere end kompileret maskinkode — bevarer eksekverbar logik intakt, selvom variabelnavne/kommentarer fjernes

## 13. Bare-metal firmware RE i praksis

- **Interrupt Vector Table (IVT)**: for ARM Cortex-M — adresse 0x0 (stack pointer) og 0x4 (reset-vektor/entry point) er altid til stede, uden dokumentation
- **MMIO (Memory-Mapped I/O)**: perifere registre på faste adresser — SVD-filer + Ghidra-plugin (SVD-Loader) automatiserer annotering
- Praktisk workflow: identificér arkitektur → load rå binær → læs IVT → byg hukommelseskort → krydstjek datablad

## 14. GCC vs. M/o/Vfuscator

En reel C-compiler, der oversætter til **udelukkende mov-instruktioner** — bevis for at mov alene er Turing-komplet. Betingelser/løkker emuleres via hukommelsesadresser og opslagstabeller i stedet for jumps/sammenligninger. Ødelægger standard disassembleres kontrolflow-analyse fuldstændigt — et ekstremt, men reelt eksempel på bevidst anti-RE-obfuskering.

---

## Gemte ressourcer fra dagen

- [CVE.org](https://www.cve.org/) — sårbarhedsdatabase
- [GreyNoise](https://www.greynoise.io/) — internettrafik-klassifikation
- [IEC 62443-serien](https://www.isa.org/standards-and-publications/isa-standards/isa-iec-62443-series-of-standards) — OT-sikkerhedsstandard
- [CyLab Security Academy (PicoGym)](https://picoctf.org/) — gratis øvelsesplatform
- [eJPT-certificering](https://ine.com/security/certifications/ejpt-certification) — entry-level pentest-cert
- [SANS EMEA](https://www.sans.org/emea) — kurser/GIAC-certificeringer
- [BSides](https://bsides.org/) / [BSides København](https://bsideskbh.dk/) — lokale sikkerhedskonferencer
