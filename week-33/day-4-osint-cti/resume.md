# CyberBridge Summer School — Open Source Threat Intelligence in Practice

**Torsdag d. 13. august 2026 | Open Source Threat Intelligence in Practice**
**Underviser:** David Clayton, The Tech Collective (CTI delivery — client reporting, training, tooling)

---

## Om underviseren

11 års erfaring i cybersikkerhed — bygget trusselsefterretnings-funktioner og undervist andre i det. Baseret i Danmark, driver CTI-levering for en portefølje af danske/nordiske kunder. Rejsen: JN Data (Cyber Defence Centre) → Combitech (rådgivning) → Elastic (Solution Architect) → NFCERT (Product Lead, trusted sharing) → The Tech Collective (nu). Pointe fra hans "How I got here"-slide: *hver eneste rolle forbrugte intelligens, andre havde produceret — det meste var ikke brugbart.* Det er selve motivationen for dagens indhold.

---

## 1. Hvad er en trussel?

En trussel kræver **tre ting samtidigt**:

| Element | Betyder | Uden det har du... |
|---|---|---|
| **Intent** | Nogen ønsker at skade dig (motiv: penge, spionage, forstyrrelse, ideologi) | **Ingen intent** → en hazard (fx en tordenstorm, der ødelægger jeres datacenter — den "ønsker" det ikke, håndteres med resiliens, ikke trusselsefterretning) |
| **Capability** | De er i stand til at gøre det (færdigheder, værktøjer, infrastruktur, folk) | **Ingen capability** → en vrede/et nag ("intent uden capability er et grudge") |
| **Opportunity** | Du har efterladt en vej ind (upatchet system, en person der kan narres) | **Ingen opportunity** → en kapabel, motiveret modstander uden adgang er "somebody else's problem" |

**Vigtig pointe fra David:** Fjern én, og du har ikke længere en trussel — men det fortæller dig faktisk noget nyttigt, nemlig **hvor jeres forsvar reelt virker**.

**Hvad I kan gøre ved hvert element:**
- **Intent** → vær ikke et værdifuldt mål, tiltræk ikke opmærksomhed
- **Capability** → detektion, forebyggelse, respons (EDR, AV, IR)
- **Opportunity** → patching, hærdning, awareness — **"the controllable one"**

**Nøglepointe:** Intelligence om capability og opportunity er der, hvor et sikkerhedsteam reelt bruger sin tid.

---

## 2. Hvad er CTI?

**Cyber Threat Intelligence** = at lære af angreb, der allerede er sket, så I kan træffe en bedre beslutning om det angreb, der endnu ikke er sket.

### Data er ikke intelligence

En liste med tusind ondsindede IP-adresser er **data**. Ingen kan handle på den. At filtrere listen gør den ikke til intelligence — det gør den bare til en kortere liste.

| Niveau | Eksempel |
|---|---|
| **Data** | Tusind IP-adresser, rå og usorteret |
| **Information** | Hvilke er aktive, og hvor de resolver til (*→ dette er bogstaveligt talt DNS-opslag, se afsnittet om DNS nedenfor*) |
| **Intelligence** | "Denne aktør målretter vores sektor, og sådan gør de det" |
| **Decision** | "Så vi ændrer noget mandag morgen" |

**Start med spørgsmålet, ikke dataen.** Gode spørgsmål ("intelligence requirements"):
- *"Hvilke afdelinger forårsagede flest hændelser sidste år ved at falde for phishing?"* → I kan finde svaret, og svaret afgør, hvor awareness-budgettet går hen
- *"Hvilke ransomware-grupper har angrebet danske energiselskaber det seneste år, og hvordan kom de ind?"* → I kan svare ud fra offentlig rapportering, og det fortæller jer, hvilke adgangsveje I skal tjekke hos jer selv

### Udfordringen: Volumen, relevans, aktualitet, tillid

Verden producerer mere trusselsinformation, end noget team kan læse (tusindvis af artikler dagligt). Det reelle spørgsmål er ikke "hvad sker der", men **"hvad betyder noget for os, og hvorfor."**

---

## 3. De tre typer CTI

Samme hændelse, skrevet tre måder, til tre forskellige målgrupper:

| Type | Spørgsmål | Indhold | Målgruppe | Tidshorisont |
|---|---|---|---|---|
| **Tactical** | Hvad blokerer jeg? | Hashes, domæner, CVE'er, filstier, detektionslogik | SOC-analytikere | Dage til uger |
| **Operational** | Hvem gør dette? | Aktøradfærd, kampagner, targeting, **dwell time** (se forklaring nedenfor) | Threat hunters | Uger til måneder |
| **Strategic** | Hvad betyder det for os? | Sektortrends, regulering, forretningsrisiko, budget | CISO/bestyrelse | Måneder til år |

**Central pointe:** I kan kopiere en taktisk indikator direkte ind i et værktøj. I kan **ikke** kopiere en strategisk vurdering ind i noget — den kræver dømmekraft.

**Strategiske spørgsmål (eksempler fra slidet):**
- **Sektor:** "Hvad er de vigtigste fremvoksende trusler mod vores branche det næste år?"
- **Supply chain** (meget vigtig — *se SolarWinds-eksemplet nedenfor*): "Hvad er vores eksponering, hvis en af vores leverandører kompromitteres?"
- **Sammenligning:** "Hvordan er vi målrettet sammenlignet med branchefæller?"

Svar på strategiske spørgsmål kommer som **vurderinger med tilknyttet konfidens**, ikke som fakta.

---

## 4. Din "second brain": Obsidian

En analytiker, der stoler på hukommelsen, mister evnen til at svare på spørgsmål om sidste måned. Et "second brain" er et eksternt system til at fange og organisere, hvad I ved, så jeres faktiske hjerne kan bruge tiden på at *tænke* i stedet for at *huske*.

**Hvorfor Obsidian specifikt (uddybet fra tidligere i dag):** Ren markdown, lokalt på jeres maskine — ingen konto, ingen cloud, ingen lock-in. En "vault" er bare en mappe. Forsvinder Obsidian i morgen, åbner jeres noter stadig i Notepad. **Det betyder noget for CTI, fordi meget af det, I skriver, ikke bør ligge på en andens server.**

---

## 5. Case study: Novo Nordisk-bruddet

*(Direkte svar på dit spørgsmål — jeg har verificeret detaljerne, da hændelsen er efter min træningsdato)*

**Hvad der faktisk skete:** Novo Nordisk offentliggjorde den 11. juni 2026 uautoriseret adgang til et begrænset antal interne IT-systemer. Gruppen **FulcrumSec** — en cyberafpresningsgruppe aktiv siden september 2025, specialiseret i højhastigheds-dataeksfiltrering fra cloud-miljøer (ingen ransomware/kryptering, ren "exfiltrate-and-extort") — stod frem og hævdede ansvaret.

**Tidslinje:**
- **Marts 2026:** FulcrumSec får adgang (ifølge egne oplysninger)
- **~2,5 måneder:** Gruppen forbliver inde i netværket og indsamler data løbende ("low-and-slow")
- **1. juni:** Gruppen kontakter Novo Nordisk-ledere
- **3. juni:** Novo Nordisk svarer, verificerer identitet via Proton Mail-korrespondance
- **11. juni:** Officiel offentliggørelse af hændelsen
- **Efterfølgende:** $25 millioner afpresningskrav **afvist** af Novo Nordisk → gruppen begynder at søge private købere til dataen

**Omfanget (ifølge FulcrumSec's egne påstande, delvist ubekræftet af Novo Nordisk):** ~700.000 filer, ca. 1,3 TB — inklusive kildekode (4.750 repositories), oplysninger om 41.000+ proprietære lægemiddelstoffer, 30 trænede AI-modeller, forskningsdata for kliniske forsøg (11.500 patienter, pseudonymiserede), samt data om 163.000 medarbejdere.

**Adgangsvejen (matcher jeres slide):** Legitimationsoplysninger efterladt i **offentligt JavaScript** på to glemte udviklings-subdomæner — *se JavaScript-forklaringen nedenfor for hvorfor det er en reel, tilbagevendende sårbarhedsklasse.* Nogle kilder peger specifikt på en eksponeret GitHub-adgangstoken fundet i marts 2026 som den konkrete udløser.

**En ekstra kompleksitet, værd at kende:** En **anden**, ubekræftet aktør ("TheUSERS007") hævdede separat at have kompromitteret Novo Nordisk mellem 5.-7. juni, med et andet værktøj ("venomware") og et $50 millioner-krav. Kun FulcrumSec-hændelsen er bekræftet af Novo Nordisk — dette er sandsynligvis grunden til, at slidet nævner "two groups claimed the breach, only one is confirmed" som den "hårdere" bonusøvelse.

**Hvorfor det er et perfekt CTI-undervisningscase:** Ingen malware, ingen kryptering — driften fortsatte upåvirket. Det er et rendyrket eksempel på, at **opportunity** (efterladte credentials) alene, kombineret med tilstrækkelig **capability** (metodisk, "low-and-slow" dataindsamling) og **intent** (finansiel afpresning), er nok til en af årets største brud — uden en eneste avanceret teknik.

---

## 6. Fire kilder til intelligence

| Kilde | Indhold | Karakteristik |
|---|---|---|
| **OSINT** | Nyheder, leverandørresearch, leak-sites, sociale medier | Gratis og righoldig — kvaliteten varierer enormt |
| **Technical** | Egen telemetri (logs, EDR, mailgateway, netflow) | Den eneste kilde, der fortæller jer noget om **jer selv** |
| **Commercial** | Betalte feeds (fx Recorded Future) | I køber valuta og dækning |
| **HUMINT** | Folk I stoler på (ISAC'er, sektorgrupper, CERT-kontakter) | Langsomst at opbygge, sværest at erstatte |

Alt, I læste i formiddagens øvelse, var OSINT — den billigste kilde, og den med bredest kvalitetsspredning.

---

## 7. Lab 2 (formiddag): Tag fem hændelser fra OSINT

**Jeres tasking:** I er lige startet i CTI-teamet hos et dansk energiselskab. 15 artikler, tre spørgsmål: Er nogen af disse hændelser forbundet? Hvad forbinder dem i så fald? Er der noget, vi bør tjekke på vores eget netværk?

**Tag-vokabularet:** `cti_type`, `victim_sector`, `initial_access`, `vulnerability`, `actor` — med **fast, lukket værdiliste**, ingen fritekst. **Den vigtigste regel:** *"Unknown is an answer."* Skriv "unknown" hvis artiklen ikke siger det — ikke jeres bedste gæt, og ikke noget I læste i en anden artikel. At registrere et hul ærligt er en professionel færdighed.

**Resultatet ("Find the overlap"):** Tre af de fem hændelser (en havn, en kommune, en fabrik — ingen fælles kunder, sektor eller leverandører) delte **samme sårbarhed, samme produkt**. Gruppen bag vælger ikke brancher — den vælger, hvem der ikke har patchet.

---

## 8. Eftermiddag: Fra rapportering til rå telemetri

Formiddagen brugte I andres skrevne rapporter (OSINT). Eftermiddagen brugte I **rå telemetri** fra fem malware-prøver, detoneret på en isoleret Windows 11-vært (SECDIS) med en Elastic-agent, der registrerede hver proces, filskrivning, registreringsdatabase-ændring og netværksforbindelse.

**Forskellen, der blev pointeret eksplicit:** Rapportering er "nogen andre valgte, hvad der betød noget, skrevet dage/uger senere." Telemetri er "alt, systemet så, tidsstemplet til millisekundet." Intet blev filtreret fra, fordi en journalist ikke fandt det interessant.

**De fire tags for hver sag (kun disse fire har svar for en lab-detonation):**
- `malware` — familienavnet, fra hash-opslag
- `attack_vector` — hvordan den kom på maskinen
- `ttp` — persistensmetoden, slået op på attack.mitre.org
- `infrastructure` — domæner/adresser fra artefaktlisten

**Værktøjer brugt til opslag:** VirusTotal (hash/domæne/IP på tværs af mange motorer) og MalwareBazaar (abuse.ch's åbne prøvearkiv) — **vigtig sikkerhedsnote fra David: download/åbn aldrig prøver uden for detonationslabbet.**

---

## 9. Closing: Automatiseringsplatformen

David har selv bygget en platform, der indtager offentlig rapportering og udtrækker objekter automatisk efter samme skema, I brugte hele dagen — 46.900 artikler ind, 39.423 objekter, 25.979 forbindelser ud, uden en eneste manuel tagging.

**Den vigtigste pointe i hele dagen, måske:** *"It does not replace the analyst. It replaces the data entry."* Alt frem til sidste linje i tabellen (læs artiklen, identificér malware/aktør, list infrastruktur, kortlæg TTP) er **ekstraktion** — det kan automatiseres. Det, der overlever automatisering, er **dømmekraft**: at afgøre, hvad et overlap reelt beviser. Det er præcis det, I øvede hele dagen.

**Interessant datapunkt fra platformen:** 10.946 "incidents" vs. kun 603 "techniques" udtrukket — rapportering siger langt oftere *at* noget skete, end den siger *hvordan*. Og kun 96 "indicators" ud af 46.900 artikler — handlingsbare artefakter er det sjældneste, OSINT indeholder.

---

## Dine indlejrede spørgsmål, besvaret

### Hvad er dwell time?
Den tid, en angriber forbliver **uopdaget** inde i et kompromitteret system — fra første adgang til opdagelse/udsmidning. Det er en central KPI i både incident response og CTI: jo længere dwell time, jo mere data kan indsamles, og jo dybere kan angriberen forankre sig. I Novo Nordisk-sagen var dwell time ca. **2,5 måneder** (marts til 11. juni) — nok tid til rekognoscering, privilegie-eskalering og systematisk dataindsamling, ifølge sikkerhedsanalyser af sagen.

### Hvad er DNS?
Domain Name System — internettets "telefonbog", der oversætter menneskelæsbare domænenavne (fx `eksempel.dk`) til IP-adresser, som computere reelt bruger til at kommunikere. Direkte relevant til dagens indhold: da Data→Information-slidet nævnte at finde ud af *"hvilke IP'er er aktive, og hvor de resolver til"* — det er bogstaveligt talt et DNS-opslag. Det er også kernen i `infrastructure`-tagget fra eftermiddagens øvelse: et domæne brugt af en trusselsaktør er værdiløst som indikator, før I ved, hvilken IP det resolver til lige nu.

### SolarWinds-eksemplet
Nævnt som det klassiske eksempel på en **supply chain**-hændelse (den strategiske spørgsmålstype fra CTI-typerne). Kort version: I 2020 kompromitterede en formodet russisk statsstøttet aktør SolarWinds' softwarebygningsproces og indsatte en bagdør (kaldet SUNBURST) direkte i legitime opdateringer af deres Orion-netværksovervågningssoftware. Opdateringen blev distribueret til ~18.000 kunder, inklusive amerikanske myndigheder og Fortune 500-virksomheder — uden at nogen af dem gjorde noget forkert selv. Det er selve definitionen på, hvorfor "hvad er vores eksponering, hvis en leverandør kompromitteres" er et strategisk spørgsmål: jeres egen sikkerhed er kun så stærk som den svageste leverandør i kæden.

### Hvad er JavaScript bundles?
Direkte relevant til selve Novo Nordisk-sagen! Når en webapplikation bygges (React, Vue osv.), samler et byggeværktøj (fx Webpack) al JavaScript-koden til én eller flere "bundles" — filer, der sendes til og køres i **brugerens browser**. Den kritiske sikkerhedspointe: alt i en JavaScript-bundle er **offentligt synligt og downloadbart af enhver**, der besøger siden — inklusive eventuelle hardkodede API-nøgler, adgangstokens eller credentials, en udvikler ved et uheld har efterladt i koden. Det er præcis, hvad der skete hos Novo Nordisk: legitimationsoplysninger efterladt i offentligt JavaScript på to glemte udviklings-subdomæner — en overraskende almindelig og fuldt undgåelig adgangsvej, der ikke krævede nogen avanceret teknik overhovedet.

---

## Ressourcer at bogmærke (verificeret)

- **[securitydistractions.com/students](https://securitydistractions.com/students/)** — Davids samlede ressourceside: login til Elastic-labbet, CTI Agent-platformen, VirusTotal, MalwareBazaar, og GitHub-linket til øvelsesvaulten
- **[CTI Agent](https://cti-agent.securitydistractions.com/)** — automatiseringsplatformen fra afslutningen (login: student)
- **Øvelsesvault:** `github.com/dclayton454/cyberbridge-summerschool2026` (åbnes i Obsidian)

---

## Tre ting at tage med (Davids egne, fra sidste slide)

1. **Start med spørgsmålet, ikke dataen**
2. **Sig, hvad I ikke ved** — "unknown" er et gyldigt, professionelt svar
3. **Offentlig rapportering fortæller jer, hvor I skal kigge — ikke hvad I vil finde**

---

## 11. Tillæg — Yderligere punkter fra dagen

### Hvad står OSINT for?
**Open Source Intelligence** — efterretning indhentet fra offentligt tilgængelige kilder (nyheder, sociale medier, leak-sites, myndighedsrapporter), i modsætning til klassificerede eller betalte/lukkede kilder. Den billigste og mest righoldige af de fire kilder fra tidligere (OSINT/Technical/Commercial/HUMINT), men også den med størst kvalitetsspredning.

### Lab session #2 — metodisk tilgang
Samme tre spørgsmål som formiddagens øvelse, nu anvendt på CTI Agent-platformen (i stedet for de 15 udleverede artikler):

1. **Er nogen af hændelserne forbundet?** — søg efter delte værdier på tværs af `infrastructure`, `malware`, eller `actor`-felter, præcis som "Find the overlap"-øvelsen
2. **Hvad forbinder dem, i så fald?** — samme sårbarhed/produkt? Samme aktør? Samme adgangsvej (`initial_access`)?
3. **Er der noget, vi bør tjekke på eget netværk?** — oversæt fundet til en konkret hunt-handling (som NN-02's syv nummererede hunt actions)

**Tag-vokabularet, du lister** (`cti_type`, `victim_sector`, `initial_access`, `vulnerability`, `unknown`) er identisk med formiddagens — samme disciplin gælder: **"unknown" er et gyldigt svar**, ikke et gæt.

### CTI Agent — konto og formål
`cti-agent.securitydistractions.com` kræver login (bruger: `student`, adgangskode udleveret i sessionen — ikke selvregistrering). Din note *"Not CTI, use the tool to create intelligence"* fanger pointen fra afslutningsslidet præcist: platformen erstatter ikke analytikeren, den erstatter **data entry** — I bruger den til at *skrive* færdig intelligence (rapporter), ikke kun læse den.

### Trusselsaktører nævnt: The Com & Lazarus Group
- **The Com** ("The Community") — et løst, decentraliseret, overvejende engelsktalende cyberkriminelt økosystem (FBI anslår ~1.000 personer), primært unge/teenagere. Ikke selv en angrebsgruppe, men **grobunden**, hvorfra navngivne grupper som Scattered Spider (også kendt som UNC3944, Octo Tempest) er opstået. Kendt for avanceret social engineering (SIM-swapping, phishing af IT-helpdesks) frem for teknisk malware-udvikling.
- **Lazarus Group** — nordkoreansk, statssponsoreret APT-gruppe, aktiv siden mindst 2009. Kendt for finansielt motiverede angreb (kryptovaluta-tyverier i milliardklassen) kombineret med spionage — en sjælden kombination af statslig og kriminel motivation i samme aktør.

### Kildemetodik (scraping, BleepingComputer, chatbot-skimmere)
Din note om, at platformen skraber nyhedsartikler — **BleepingComputer** er en anerkendt, ofte citeret kilde i branchen for hurtig, teknisk retvisende breach-dækning. "Chatbot til at bygge rapport-skimmere" beskriver præcis den type LLM-drevne udtræksautomatisering, vi så i eftermiddagens 46.900-artikler-statistik — en sprogmodel læser råtekst og udtrækker strukturerede felter (aktør, malware, IOC) automatisk.

### Hvad er HHS-adgang?
**HHS** = U.S. Department of Health and Human Services (det amerikanske sundhedsministerium). I en trusselsefterretningskontekst optræder "HHS access" typisk som en kategori-betegnelse for adgang til sundhedssektor-systemer, solgt eller omtalt af initial access brokers — relevant, hvis det dukkede op i en CTI Agent-sag om en sundheds-/medicinalvirksomhed. Sig til, hvis det stod i en specifik kontekst i værktøjet — så kan jeg being mere præcis.

### Hvad er DigitalOcean?
En cloud-hosting-udbyder (VPS'er, simple og billige at oprette). Legitimt meget brugt af udviklere — men af samme grund også populær blandt trusselsaktører til hurtigt at spinne C2-infrastruktur eller phishing-sider op. Relevant som en mulig værdi i `infrastructure`-tagget: et domæne, der resolver til en DigitalOcean-IP, er ikke i sig selv mistænkeligt, men værd at bemærke i konteksten.

### app.recordedfuture.com
Verdens mest kendte kommercielle CTI-platform — matcher direkte "Commercial"-kilden fra Davids fire-kilder-slide. Aggregerer og korrelerer enormt datavolumen (millioner af artikler, hundredvis af trusselsaktør-profiler) til risikoscorede, handlingsbare feeds, ofte leveret via STIX/TAXII til jeres SIEM/SOAR. I køber i bund og grund "valuta og dækning" — kuratering og bredde, I ikke selv kan bygge fra bunden.

### Hvad er Sentinel?
Sandsynligvis **Microsoft Sentinel** — Microsofts cloud-native SIEM/SOAR-platform, som vi faktisk stødte på i vulnerability management-dagen (Defender for IoT fodrer data ind i Sentinel for en samlet IT+OT-sikkerhedsvisning). Kan alternativt referere til **SentinelOne**, en stor EDR/XDR-leverandør — hvis konteksten var endpoint-beskyttelse frem for logstyring, er det den rigtige. Sig til, hvilken kontekst det stod i, hvis du vil have det bekræftet.

### Hvad er en MCP Server?
**Model Context Protocol** — en åben standard (introduceret af Anthropic i november 2024), der lader sprogmodeller (som mig) tilgå eksterne værktøjer og datakilder på en standardiseret måde. En MCP-server "udgiver" en liste af konkrete værktøjer (fx `search_database`, `send_email`, `fetch_webpage`), som modellen kan opdage og kalde — i stedet for at hver integration skal bygges fra bunden for hver enkelt model/platform. Det er faktisk den samme mekanisme, jeg selv bruger til at slå op i fx Google Drive eller Gmail, hvis du forbinder dem. I en CTI-kontekst betyder det: en chatbot kan forbindes til fx en MITRE ATT&CK-database eller et internt ticketsystem via en MCP-server, og derved selv hente og handle på data — grundlaget for de "chatbot report skimmers", du nævnte ovenfor.

### Feedly (for Threat Intelligence)
Et AI-drevet CTI-produkt (adskilt fra almindelige Feedly RSS-læseren) — indsamler kontinuerligt fra sikkerhedspublikationer, leverandørrådgivninger og dark web-fora, udtrækker automatisk IOC'er, TTP'er og CVE'er ind i en "Threat Graph", og lader analytikere generere skræddersyede rapporter/briefinger. Fungerer som en direkte konkurrent/parallel til Recorded Future — begge er eksempler på "Commercial"-kilden.

### Claude Cowork
Anthropics agentiske arbejdsområde til ikke-udviklere — kører i Claude Desktop-appen, læser/skriver lokale filer direkte, og kan udføre flertrins-opgaver selvstændigt (research, dokumenter, regneark) uden manuel prompt-for-prompt styring. Kører sessioner i skyen (isoleret miljø på Anthropics servere), så arbejdet fortsætter, selvom du lukker laptoppen — men kræver stadig, at Claude Desktop-appen er åben og din computer er tændt, når en opgave rører dine lokale filer eller browser.

**Om "hvor god er den":** det afhænger reelt af opgavetypen — den er stærk til at samle research, organisere filer og producere polerede dokumenter/regneark selvstændigt, men jeg vil undgå en ren markedsføringsdom her. Bedste kilde til at vurdere det selv: [claude.com/docs/cowork/overview](https://claude.com/docs/cowork/overview).

### The Tech Collective — kort baggrundstjek
David Claytons arbejdsgiver er en del af **Implement Consulting Group** (dansk ledelseskonsulentvirksomhed) — beskriver sig selv som "et kollektiv af teknologer, udviklere, testere og ingeniører", organiseret i specialiserede hubs. Baseret i København og Aarhus. Deres cybersikkerhedsudbud dækker netop det, David repræsenterer: trusselsefterretning (overflade-, deep- og dark web-scanning), 24/7 SOC-drevet detektion, pentest, og security awareness-træning — matcher fuldstændig hans egen "CTI delivery: client reporting, training, tooling"-titel fra introslidet.

### Hvad er en dropper? (relateret til IOC'er)
En **dropper** er en malware-type, hvis eneste formål er at **levere og installere** en anden, ofte mere skadelig payload (ransomware, spyware, en bagdør) på offerets system — selve dropperen udfører sjældent den skadelige handling selv. Den optræder som en **fil-baseret IOC**: en genkendelig hash eller et mistænkeligt script, der bruges til at identificere en tidlig fase af et angreb, før hovedpayloaden overhovedet er aktiveret. Relevant til NN-02-adviseringen: `rclone` (det legitime overførselsværktøj, FulcrumSec brugte) er ikke selv en dropper, men et eksempel på "living-off-the-land" — brug af legitime værktøjer til ondsindede formål, hvilket gør signaturbaseret detektion (som ville fange en klassisk dropper) utilstrækkelig.

### GIC-roller i cyberefterretning
Jeg kunne desværre **ikke bekræfte "GIC"** som en standardbetegnelse i cyber threat intelligence-litteraturen — hverken som forkortelse for en rolle, et team eller en fase i efterretningscyklussen (som normalt består af Planning, Collection, Processing, Analysis, Dissemination, Feedback). Det kan være en mishørt forkortelse, eller specifik til en bestemt organisation/ramme, jeg ikke har fundet. Hvis du kan stave det, som det stod, eller give lidt mere kontekst (fx var det på et slide om roller i et SOC?), finder jeg det gerne præcist i stedet for at gætte.

### Skrive intelligence-rapporterne
Noteret som en handling, ikke et spørgsmål — dette er sandsynligvis en reference til CTI Agent-øvelsens formål: bruge platformens udtrukne data som råmateriale, men selv skrive den færdige, kontekstualiserede rapport (tactical/operational/strategic, som formiddagens NN-02/03/05-øvelse). Sig til, hvis du vil have hjælp til at strukturere en konkret rapport ud fra det, du finder i værktøjet.



Din egen observation — *"CTI is an easy intro to cybersecurity"* — passer godt med, hvordan dagen var bygget op: ingen forudsætning om dyb teknisk baggrund for formiddagens OSINT-arbejde, og eftermiddagens telemetri-arbejde byggede naturligt oven på det samme analytiske skema. Kombineret med din civilingeniør-baggrund er "start med spørgsmålet, ikke dataen" i øvrigt et princip, der overføres direkte fra ingeniørarbejde — samme logik som at definere et System under Consideration, før man analyserer det (dag 2).
