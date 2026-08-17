# Portfolio-Review – liamdahl.com

Stand: 07.08.2026 · Basis: Live-Site + Repo (`src/`), Astro 6 + Tailwind 4

---

## 0. Vorab: Scope-Konflikt

Deine Projektdefinition sagt: **nur** die Buchungssystem-Seite wird in diesem
Durchgang neu gebaut, Foundation zuerst, die anderen zwei Seiten folgen separat.

Dein Punkt 4 ("Projektseiten upgraden") betrifft aber alle drei. Und Punkte 1–3
(Ausbildung, Zertifikate, Projekt-Struktur) sind Startseiten-Arbeit, nicht
Projektseiten-Arbeit.

Das ist nicht falsch – aber es sind drei verschiedene Baustellen. Entscheide,
was in diesen Durchgang gehört, bevor irgendwas angefasst wird.

---

## 1. Blocker

Status: **erledigt ✅ = gefixt und gebaut · offen ⏳ = braucht Input von dir**

Entschiedener Scope für diesen Durchgang: **nur Startseite** (+ diese Blocker).

### 1.1 ⏳ Dein Foto ist eine tote Figma-URL
`src/pages/index.astro`

```js
const photoSrc = 'https://www.figma.com/api/mcp/asset/3cf6ae73-...';
```

Das ist eine temporäre Figma-MCP-Asset-URL. Sie liefert bereits jetzt leeren
Inhalt und läuft ohnehin ab. Auf deiner Über-mich-Sektion steht dann ein
kaputtes Bild.

**Braucht dich:** Original-Datei nach `src/assets/images/` legen, dann binde ich
sie über `astro:assets` ein (inkl. Responsive-Varianten und Lazy-Loading, was
die Figma-URL beides nicht kann).

### 1.2 ⏳ Widersprüchliche Stichprobengröße in WärmeCheck

Das ist nicht nur ein Zahlendreher – die Seite widerspricht sich sechsfach:

| Stelle | Aussage |
|---|---|
| Projektübersicht | "Usability-Test mit **4 Probanden**" |
| Abschnitt 04, H3 | "**4 Probanden**, ein Flow, klare Ergebnisse" |
| Abschnitt 04, Fließtext | "Vier Probanden vor Ort getestet. … **4 Personen** mittleren Alters … und **2 Jüngeren**" |
| Abschnitt 04, Stat-Karte | "**6** Probanden vor Ort" |
| Abschnitt 04, Finding | "Alle **vier** Probanden waren … verwirrt" |
| Abschnitt 05, Ergebnis | "Usability-Test mit **6 Probanden**" |

Der Fließtext rechnet selbst 4 + 2 = **6** vor, sagt im selben Satz aber "Vier
Probanden". Die Stat-Karten sagen 6, die Überschriften 4.

**Das kann ich nicht raten.** Meine Vermutung: es waren **6** (4 ältere +
2 jüngere), die Überschriften sind falsch, und "alle vier Probanden" beim
PLZ-Finding meint *vier von sechs* – was dann sogar korrekt wäre, nur
missverständlich formuliert.

In einer Case Study, deren Kernaussage "ich arbeite methodisch sauber" ist, ist
das der teuerste mögliche Fehler. Wer es bemerkt, glaubt dir den Rest auch nicht.

### 1.3 ✅ Tote Klick-Affordanz (2×, nicht 1×)
"Vorher / Nachher ansehen →" stand in den Top-4-Findings **und** bei den
Design-Entscheidungen. Sah aus wie die funktionierenden "Fehler anzeigen"-Buttons,
war aber ein `<div>` ohne Handler – ausgerechnet in dem Abschnitt, in dem du
fremde Systeme für irreführende Affordanzen kritisierst.

Beide entfernt. **Besser wäre:** echte Vorher/Nachher-Bilder ergänzen und die
Buttons zurückholen. Siehe Punkt 4.

### 1.4 ✅ Falsche Alt-Texte (2×)
`alt="WärmeCheck Buchungssystem Mockup"` stand auch auf **Persona Markus
Hoffmann** (waermecheck) und **Persona Marie Lauenberg** (greybox). Beide
korrigiert.

*(Korrektur zu meiner ersten Einschätzung: `ai-pdf-reader` war in Ordnung. Die
Datei heißt zwar `persona_Lea.png`, die Persona aber tatsächlich Johanna Wendt –
nur der Dateiname ist veraltet, nicht der Alt-Text.)*

### 1.5 Kleinkram
- ✅ **Typo** "Spielgelinstitut" → "Spiegel Institut – UI/UX Design"
- ✅ **Leerer Zertifikate-Tab** wird nicht mehr gerendert. Sobald du im Array
  `allTabs` Einträge einträgst, erscheint der Tab automatisch wieder.
- ✅ **Meta-Tags** in `Layout.astro`: description, canonical, Open Graph,
  Twitter Card. Alle drei Projektseiten haben eigene Descriptions.
- ✅ **Bio-Schriftgröße**: war auf Desktop 15px / Mobile 16px. Jetzt 17px auf
  Desktop, plus `max-w-[68ch]` – der Absatz lief vorher über die volle
  Spaltenbreite, was bei 12 Zeilen Fließtext die Zeilenführung kaputt macht.
- ⏳ **Instagram-Link** zeigt auf `https://instagram.com` – kein Profil.
  Dein Handle oder soll der Link raus?
- ⏳ **11 uncommittete Dateien** im Repo (`astro.config.mjs`, `global.css`,
  `package.json`, …). Vor dem Refactor committen oder verwerfen – sonst
  vermischen sich Altlasten mit den neuen Änderungen.

### 1.6 ⏳ OG-Image ist ein Platzhalter
Ich habe `public/og-image.png` (1200×630) erzeugt, damit der Meta-Tag nicht ins
Leere zeigt. Aber: die Sandbox hat weder Inter noch Manrope, das Bild ist in
DejaVu Sans gesetzt – **off-brand**. Du hast Figma; ein Export in deinen echten
Schriften ist in fünf Minuten gemacht und ersetzt die Datei 1:1.

---

## 2. Zu deinen vier Punkten

### Punkt 1 – Konstruktionsmechaniker hervorheben

**Ist-Zustand:** Die Ausbildung steht 2× als nackte Zeile in einem
Timeline-Tab ("2017–2021 Ausbildung Konstruktionsmechaniker" /
"Konstruktionsmechaniker Lutz Aufzüge"). Null Kontext, null Verbindung zum Rest.

**Problem:** Deine Positionierung ist "ich optimiere komplexe technische
Systeme". Die Ausbildung ist der einzige Beleg dafür, dass du diese Systeme
wirklich von innen kennst – und sie ist der Teil, den kein anderer
UX-Bewerber hat. Aktuell ist sie eine Fußnote.

**Optionen:**

| Option | Pro | Contra |
|---|---|---|
| **A – Bio umschreiben.** Ausbildung als erster Satz statt Ingenieurpsychologie. | Kein neues Layout, sofort wirksam | Bleibt Fließtext, wird überlesen |
| **B – Eigener Abschnitt "Warum technische Kontexte".** 3–4 Zeilen + 1 Bild (Werkstatt/Aufzug), zwischen Hero und About. | Macht das Alleinstellungsmerkmal sichtbar; passt zu "weniger Text, mehr Bild" | Neuer Abschnitt = mehr Foundation-Arbeit |
| **C – Timeline-Einträge um je einen Satz erweitern.** | Minimal, konsistent mit Bestand | Bleibt versteckt hinter einem Tab-Klick |

Meine Einschätzung: **B**. A und C behandeln dein stärkstes Argument weiter
wie eine Lebenslauf-Zeile. Aber es ist deine Entscheidung – B kostet echte
Layout-Arbeit.

**Egal welche Option:** Der Satz muss die Brücke schlagen, nicht nur den Beruf
nennen. Sinngemäß "Ich habe vier Jahre lang Anlagen gebaut, deren Bedienung
Menschen gefährden kann, wenn sie schlecht gestaltet ist" – nicht
"Ausbildung zum Konstruktionsmechaniker, 2017–2021".

---

### Punkt 2 – Zertifikate Anforderungsmanagement & Robotik

**Ist-Zustand:** Tab existiert, ist leer, sagt das dem Besucher auch.

**Zu klären, bevor irgendwas gebaut wird:**
1. Wie heißen die Zertifikate **exakt**? (IREB CPRE Foundation Level? Welche
   Robotik-Zertifizierung, welcher Anbieter?)
2. Ausstellendes Institut + Jahr?
3. Gibt es PDFs/Verifizierungs-Links zum Verlinken?

Ohne Aussteller und Jahr wirkt ein Zertifikat wie ein Wochenendkurs. Mit
Verifizierungslink wirkt es wie eine Qualifikation.

**Layout-Option, die ich empfehle:** Zertifikate nicht als dritten
Timeline-Tab, sondern als Karten-Reihe mit Aussteller-Logo + Jahr +
Verifizierungslink. Timeline-Optik sagt "Vergangenheit", Karten-Optik sagt
"Nachweis".

**Wichtiger Zusammenhang:** Das IREB-Zertifikat und deine
Anforderungsmanagement-Skill sind aktuell **nirgends** mit einem Projekt
verbunden. Ein Zertifikat ohne Projekt, das es anwendet, ist nur ein Logo.
Siehe Punkt 3.

---

### Punkt 3 – Projekte besser strukturieren / Wissen sichtbar machen

Das ist der Punkt mit dem größten Hebel – und der, den du am unschärfsten
formuliert hast.

**Ist-Zustand:** Drei Projektkarten, abwechselnd Bild links/rechts, jeweils nur
**Titel + Jahr**. Sonst nichts. Ein Besucher kann nicht erkennen, worum es geht
oder welche Methode dahintersteckt, ohne die Seite zu öffnen. Bei drei
Projekten heißt das: drei Klicks ins Ungewisse.

**Was deine Skills tatsächlich hergeben** (HMI, Requirements Engineering/IREB,
Usability-Evaluation, Accessibility) – und wie es aktuell auf den Karten
sichtbar ist:

| Projekt | Methodenkern | Auf der Karte sichtbar |
|---|---|---|
| WärmeCheck | Heuristische Evaluation (Nielsen), Usability-Test, Persona, Frontend | nichts |
| Greybox | HMI, physisches Produkt, Prototyp, Studie | nichts |
| Accessible PDF Reader | Accessibility/Screenreader, Architektur, Test | nichts |
| — | **Requirements Engineering / IREB** | **kein Projekt belegt es** |

**Zwei Entscheidungen, die du treffen musst:**

**Entscheidung A – Wie strukturieren?**

| Option | Pro | Contra |
|---|---|---|
| **A1 – Karten anreichern.** Pro Karte: 1 Satz Problemstellung + 2–3 Methoden-Tags. | Kleinster Eingriff, sofort verständlich, nutzt den `Badge`-Component, den du schon hast | Löst nicht das Problem "welche Kompetenz habe ich insgesamt" |
| **A2 – Methoden-Ebene über den Projekten.** Ein Abschnitt "Wie ich arbeite" mit 3–4 Methodenblöcken, jeder verlinkt auf die Projekte, die ihn belegen. | Macht Wissen zum Hauptargument statt Nebenprodukt; verbindet Zertifikate mit Projekten | Deutlich mehr Arbeit; Gefahr von Methoden-Bingo ohne Substanz |
| **A3 – Filterbar nach Methode.** | "Innovativ" | Bei 3 Projekten ist ein Filter Schnickschnack. Nicht empfohlen. |

Meine Einschätzung: **A1 jetzt, A2 wenn du eine vierte Case Study hast.** A2
mit drei Projekten wirkt aufgeblasen.

**Entscheidung B – Requirements Engineering: Lücke schließen oder weglassen?**
Du willst ein IREB-Zertifikat zeigen, hast aber kein Projekt, das RE anwendet.
Entweder du machst RE in einer bestehenden Case Study explizit sichtbar (die
WärmeCheck-Anforderungen existieren ja irgendwo), oder das Zertifikat steht
allein da. Zweiteres ist schwächer.

---

### Punkt 4 – Projektseiten: weniger Text, mehr Bild, sauberere Methodik

**Ist-Zustand, gemessen:**

| Seite | Zeilen Code | ca. Wörter | Bilder | Verhältnis |
|---|---|---|---|---|
| WärmeCheck | 587 | ~1.540 | 5 | ~310 Wörter/Bild |
| Greybox | 561 | ~1.565 | 5 | ~310 Wörter/Bild |
| AI PDF Reader | 492 | ~1.130 | 2 | ~565 Wörter/Bild |

Zum Vergleich: eine gut lesbare Case Study liegt bei 600–900 Wörtern und
8–15 Visuals. Du hast fast das Doppelte an Text bei einem Drittel der Bilder.
**Dein Instinkt ist richtig.**

**Aber – wo der Text wirklich zu viel ist, ist spezifischer als "überall":**

Die Struktur ist gut. Side-Nav mit 5 nummerierten Phasen, Meta-Karten
(Rolle/Zeitraum/Tools/Methoden), Bug-Karten mit Overlay – das ist
handwerklich solide und über alle drei Seiten konsistent. Das würde ich
**nicht** wegwerfen.

Zu viel Text ist konkret hier:
- **Projektübersicht-Karte** (waermecheck:94): 5 Sätze, die den Rest der Seite
  vorwegnehmen. Sollte 2 Sätze sein.
- **Kontext & Problemstellung** (111–112): zwei volle Absätze Firmenbeschreibung.
  Interessiert niemanden. 3 Zeilen reichen.
- **Findings-Karten**: jede hat 3–4 Zeilen Erklärung. Bei vier Karten
  nebeneinander liest das niemand. Titel + 1 Zeile + Bild.
- **Learnings**: zwei Absätze à 4 Sätze. Je 2 Sätze.

Was **fehlt**, und wichtiger ist als das Kürzen:
- **Kein Vorher/Nachher-Vergleich.** Du beschreibst 10 UX-Probleme in Text.
  Eine Vorher/Nachher-Gegenüberstellung würde mehr sagen als alle Absätze
  zusammen – und die Screenshots existieren teilweise schon.
- **Kein User Flow / keine Prozessdarstellung.** "Methodik sauber zeigen"
  heißt Diagramm, nicht Fließtext.
- **Keine Ergebnis-Zahlen prominent.** "100% Erfolgsrate, 1:46 Min
  Durchlaufzeit" steht als Nebensatz in Absatz 5 von Abschnitt 5. Das gehört
  in den Hero.

**Reihenfolge, die ich empfehle:** erst Visuals ergänzen, dann kürzen. Kürzen
ohne Bilder macht die Seite dünn statt dicht.

---

## 3. Technische Befunde (Foundation-relevant)

Diese fließen direkt in Phase 1 deines Refactors ein.

### 3.1 `--spacing: 1px` bricht das Tailwind-Spacing-System
`src/styles/global.css:8`

```css
--spacing: 1px;   /* p-32 = 32px, gap-24 = 24px */
```

Das ist der "Tailwind-Config nicht sauber"-Punkt aus deiner Projektdefinition.
Funktioniert, aber: es gibt damit **keine Skala mehr**. Jeder Wert ist
gleichwertig, `gap-16`, `gap-20`, `gap-24`, `gap-32`, `gap-40`, `gap-48`,
`gap-56`, `gap-64`, `gap-80`, `gap-100` sind alle im Code, ohne dass eine
Regel erkennbar wäre. Bei drei Projektseiten, die dieselbe Foundation nutzen
sollen, ist das genau die Stelle, die dich später einholt.

Das ist die erste Entscheidung deines Refactors: Skala definieren
(z.B. 4/8/12/16/24/32/48/64/96) oder bewusst bei 1px bleiben und die Disziplin
über Components erzwingen.

### 3.2 Typografie ist komplett arbiträr
Kein einziger Text nutzt Tailwind-Textklassen. Überall
`text-[16px] leading-26 md:text-[15px] md:leading-20 md:tracking-[0.5px]`.
Auch hier: keine Skala, jede Größe einzeln entschieden. Gehört in die Tokens.

Nebenbei: die Bio wird auf Desktop **kleiner** (15px) als auf Mobile (16px).
Vermutlich unbeabsichtigt.

### 3.3 Accessibility – konkrete Befunde
Relevant, weil du ein Accessibility-Projekt zeigst und danach beurteilt wirst.

- **Mobile-Nav (`Layout.astro`):** `role="dialog" aria-modal="true"`, aber der
  Fokus wird beim Öffnen **nicht** in den Dialog bewegt und beim Schließen
  nicht zurückgegeben. Es gibt keine Fokusfalle – ein Screenreader-Nutzer
  tabbt durch die Seite dahinter. Das ist der klassische Dialog-Fehler.
- **Tabs (`index.astro`):** kein `aria-controls`, kein `id` auf den Buttons,
  keine Pfeiltasten-Navigation, kein `tabindex="-1"` auf inaktiven Tabs.
  Als `role="tablist"` deklariert, verhält sich aber nicht so.
- **Bug-Overlay (`waermecheck.astro`):** gleiches Fokus-Problem wie die
  Mobile-Nav.
- **Kontaktformular:** Placeholder statt `<label>`. Sobald getippt wird, ist
  die Feldbezeichnung weg – für alle Nutzer, nicht nur Screenreader.
- **Gradient-Überschrift im Hero** (`bg-clip-text text-transparent`):
  Kontrast am linken Ende (#9d9d9d auf #f7f3ee) liegt bei ca. 2.4:1.
  WCAG AA für große Schrift verlangt 3:1. Fällt durch.

### 3.4 Header frisst Viewport
`py-40 md:py-40` + sticky ⇒ ca. 121px permanent belegt (siehe
`scroll-padding-top: 121px`). Auf einem 13"-Laptop ist das viel. Auf Mobile
72px – vertretbar.

---

## 4. Was gut ist (nicht anfassen)

- Positionierung ist scharf und ungewöhnlich. "Ich optimiere komplexe Systeme
  für Menschen" + Ingenieurpsychologie ist eine echte Nische, kein
  Generalisten-Claim.
- Case-Study-Struktur (5 nummerierte Phasen + Side-Nav) ist über alle drei
  Seiten konsistent durchgezogen. Das ist mehr Disziplin als die meisten
  Junior-Portfolios zeigen.
- Bug-Overlay-Interaktion ist genau die richtige Art von "innovativ": löst ein
  echtes Problem (Screenshots zeigen ohne die Seite zuzumüllen), kein Selbstzweck.
- Live-Demo-Verlinkung bei WärmeCheck. Ein klickbares Ergebnis schlägt jedes
  Mockup.
- Farbpalette und Ruhe im Layout. Da ist kein Schnickschnack drin – gut so.

---

## 5. Zum Thema "innovativer, aber kein Schnickschnack"

Dein Portfolio ist aktuell nicht langweilig, weil es zu wenig Effekte hat. Es
ist zurückhaltend, weil die **Inhalte** nicht sichtbar sind: Karten ohne
Beschreibung, Ergebnisse ohne Zahlen, Methoden ohne Belege, ein leerer Tab.

Der wirksamste "Innovations"-Hebel ist deshalb nicht Animation, sondern:
Ergebnisse in Zahlen, Methoden als Tags, Vorher/Nachher als Bild.

Falls du danach noch Lust auf einen visuellen Kniff hast, wäre der einzige,
der zu deiner Positionierung passt, ein interaktiver Vorher/Nachher-Vergleich
(Slider oder Toggle). Der zeigt Kompetenz statt nur Aufmerksamkeit. Alles
andere – Scroll-Animationen, Custom Cursor, Parallax – ist genau der
Schnickschnack, den du nicht willst.

---

## 6. Reihenfolge

**Entschieden:** Scope = nur Startseite. Projektseiten komplett vertagt.

| # | Schritt | Status |
|---|---|---|
| 1 | Blocker (Abschnitt 1) | ✅ bis auf Foto, Probanden, Instagram, OG-Image |
| 2 | Repo-Entscheidung: `portfolio-v2` oder Branch? | ⏳ deine Entscheidung |
| 3 | Uncommittete Änderungen aufräumen | ⏳ |
| 4 | Zertifikat-Daten sammeln (Aussteller, Jahr, Links) | ⏳ nur du |
| 5 | Foundation: Spacing- und Typo-Skala (3.1, 3.2) | offen |
| 6 | Startseite Punkt 1 – Konstruktionsmechaniker | offen |
| 7 | Startseite Punkt 2 – Zertifikate | blockiert durch 4 |
| 8 | Startseite Punkt 3 – Projektkarten anreichern | offen |
| 9 | A11y-Durchgang Startseite (3.3) | offen |

**Wichtig zu Schritt 5:** Die Foundation wird in diesem Durchgang nur an der
Startseite erprobt. Das ist ein bewusster Trade-off deiner Scope-Entscheidung –
ob Spacing- und Typo-Skala für Case-Study-Layouts taugen, zeigt sich erst, wenn
die WärmeCheck-Seite drankommt. Rechne damit, dass die Tokens dann nochmal
angefasst werden. Das ist kein Fehler, nur eine Erwartung, die du haben solltest.

**Alle drei Projektseiten (WärmeCheck, Greybox, AI PDF Reader) bleiben in diesem
Durchgang unverändert** – auch der Probanden-Widerspruch, sobald du mir sagst,
welche Zahl stimmt, ist ein Ein-Zeilen-Fix und keine Seitenüberarbeitung.

---

## Offene Fragen an dich

**Blockierend:**

1. **Probanden:** Waren es 4 oder 6? (Der Fließtext rechnet 4 + 2 = 6 vor.)
2. **Foto:** Original-Datei vorhanden? Bitte nach `src/assets/images/` legen.
3. **Zertifikate:** Exakte Bezeichnungen, Aussteller, Jahr, Verifizierungslinks?
4. **Instagram:** Handle – oder Link entfernen?

**Für die inhaltliche Richtung:**

5. **Requirements Engineering:** Gibt es RE-Artefakte aus WärmeCheck
   (Anforderungsliste, Use Cases, Akzeptanzkriterien), die man sichtbar machen
   kann? Sonst steht das IREB-Zertifikat ohne Beleg da.
6. **Konstruktionsmechaniker:** Welche Option aus Punkt 1 – und hast du Fotos
   aus der Zeit (Werkstatt, Anlagen, Aufzugstechnik)? Ohne Bild wird auch
   Option B nur ein weiterer Textblock.
7. **Praktikum Spiegel Institut:** Läuft bis August 2026, also jetzt. Kommt da
   ein viertes Projekt raus? Das würde die Struktur-Entscheidung bei Punkt 3
   verändern (bei 4 Projekten wird Option A2 plausibel).
