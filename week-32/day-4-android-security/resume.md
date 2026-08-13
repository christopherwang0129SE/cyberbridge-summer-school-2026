# CyberBridge Summer School — Dag 4 Resumé

**Fredag d. 7. august 2026 | Android sikkerhed og Attack Surface**
**Underviser:** William Ben Embarek, Mobile Security Engineer, Aikido

*Bemærk: rekonstrueret fra samtalen — ikke fra et gemt slide-resumé.*

---

## Sandboxing: UID + filsystem

- **Hver app får sit eget UID** — Linux-baseret proces-isolation, appen kan ikke tilgå andre apps' data som standard
- **Filsystem-lag:** `/system` (OS, read-only) · `/vendor` (hardware-drivere/HAL) · `/data` (skrivbar bruger-/app-data) · `/data/data/<pkg>` (appens egen private sandkasse)

## APK-struktur

- En APK er en **ZIP-fil**: `classes.dex`, `resources.arsc`, `res/`, evt. `lib/`, `AndroidManifest.xml`, `META-INF/`
- **APK Signing Block** — v2/v3-signeringsskema, hasher/signerer hele APK'en som én blob (lukker Janus-angrebs-hullet fra den gamle v1/JAR-metode)
- **classes.dex er ikke native kode** — Dalvik/ART-bytecode, eksekveret af Android Runtime, ikke direkte af CPU'en (samme princip som .pyc/.luac fra RE-dagen)

## Code signing

- **Integritet** — beviser APK'en ikke er ændret
- **Update trust** — kun opdateringer signeret med samme nøgle accepteres
- **Developer identity** — selv-signeret nøgle, beviser "samme udvikler", ikke en verificeret identitet

## Permissions

**Tildeling:** Normal ved installation, "dangerous" kræver runtime-brugergodkendelse (siden Android 6.0)

**De fire beskyttelsesniveauer:**
1. **Normal** — automatisk, ingen prompt
2. **Dangerous** — kræver eksplicit brugergodkendelse ved runtime
3. **Signature** — automatisk kun hvis samme certifikat som den definerende app
4. **Signature\|Privileged** — som Signature, men også automatisk til apps i `/priv-app`-partitionen

## SELinux

Obligatorisk adgangskontrol (MAC) oven på UID-sandkassen — begrænser processer via sikkerhedskontekst, uafhængigt af UID. Fuldt håndhævet siden Android 5.0.

## jadx

Den mest brugte decompiler til Android — konverterer DEX-bytecode tilbage til læsbar Java.

## Angrebsflade: exported components

- **Services & exported** — komponenter markeret `exported="true"` kan kaldes af andre apps direkte
- **Broadcast receivers & exported** — samme princip; en eksporteret receiver kan udløses af enhver app med craftet data
- **Intent extras** — nøgle-værdi-data i en Intent; ubetroet input her er en klassisk sårbarhedsklasse, hvis ikke valideret
- **Deep links** — URL-skemaer der åbner specifikke app-skærme direkte; angrebsflade når eksterne parametre ikke valideres

## WebView: JavaScript Bridge

`addJavascriptInterface()` eksponerer native app-metoder til JavaScript i en WebView. Historisk alvorligt (før Android 4.2: alle offentlige metoder tilgængelige refleksivt); siden kræves `@JavascriptInterface`-annotering pr. metode. Alvorligt RCE-scenarie, hvis WebView'en indlæser ubetroet webindhold.
