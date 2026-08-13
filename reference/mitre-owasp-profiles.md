# Organisationsprofiler: MITRE ATT&CK og OWASP

---

## MITRE (ATT&CK-frameworket)

**Hvad er MITRE?**
En amerikansk non-profit organisation, grundlagt i 1958 med rødder i MIT. MITRE driver flere federally funded research and development centers (FFRDC'er) for amerikanske myndigheder — inden for bl.a. cybersikkerhed, forsvar, sundhed og AI. "MITRE" er ikke et akronym; det er blot navnet, organisationen fik ved stiftelsen.

**Hvad er ATT&CK?**
Adversarial Tactics, Techniques, and Common Knowledge — en global, gratis tilgængelig videnbase over reelle, observerede angriberadfærd. Udviklet af MITRE i 2013 som et forskningsprojekt, frigivet offentligt i 2015.

**Struktur:**
- **Tactics** — angriberens mål i hver fase (fx Initial Access, Persistence)
- **Techniques/sub-techniques** — de konkrete metoder, der bruges til at nå målet
- Organiseret i matricer: **Enterprise** (Windows/Mac/Linux/cloud), **Mobile** (Android/iOS), og **ICS** (industrielle kontrolsystemer — relevant for OT-sporet)

**Hvorfor det er relevant:**
Standardsproget i branchen til at beskrive og kategorisere angriberadfærd — bruges af red teams, blue teams, og til at koble sikkerhedskontroller (som CIS) direkte til konkrete, dokumenterede trusler (jf. Community Defense Model).

---

## OWASP (Open Worldwide Application Security Project)

**Hvad er OWASP?**
En amerikansk non-profit fond (grundlagt 2001, indregistreret som 501(c)3-velgørenhed i 2004), dedikeret til at forbedre software- og applikationssikkerhed. Fungerer som et helt åbent, frivilligt-drevet community — alle kan bidrage, og alt indhold er gratis og offentligt tilgængeligt.

**Struktur:**
- Over 250 lokale chapters verden over
- Driver adskillige open source-projekter, værktøjer (fx OWASP ZAP til web-scanning), dokumentation og standarder
- Er en organisation/vidensbank — ikke selv et scanningsværktøj

**Mest kendte projekt: OWASP Top 10**
En liste over de mest kritiske sikkerhedsrisici i webapplikationer, baseret på bred konsensus blandt sikkerhedseksperter globalt. Første udgave udkom i 2003; opdateres typisk hvert 3.-4. år for at afspejle aktuelle trusler.

**Hvorfor det er relevant:**
Standardreferencen for websikkerhed — bruges af udviklere, sikkerhedsteams og compliance-folk til at prioritere, hvilke sårbarhedstyper der betyder mest. Mange SAST/DAST-værktøjer bruger OWASP Top 10 som deres kategoriseringsramme. Kommer op i kursusplanen tirsdag d. 11. august.

---

## Fælles pointe

Begge organisationer er non-profit, community-drevne og leverer *gratis, åbne* standarder — i modsætning til proprietære, kommercielle rammeværker. De udfylder forskellige niches: MITRE ATT&CK dækker angriberadfærd bredt på tværs af hele angrebskæden, mens OWASP er specifikt fokuseret på applikations-/websikkerhed. Begge er gode referencer at kunne nævne konkret i CV'et som en del af din tekniske ordforråd fra CyberBridge.
