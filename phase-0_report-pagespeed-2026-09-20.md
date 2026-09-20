# Google PageSpeed Report — kinderhilfekapstadt.com

**URL:** https://www.kinderhilfekapstadt.com/
**Datum:** 2026-09-20 (Lighthouse-Lab-Läufe 08:21–08:22 UTC, Mobile 2× gemessen)
**Credential-Tier:** 1 (API-Key + Service Account)
**Plattform:** Squarespace (+ Weglot, MailerLite, reCAPTCHA Enterprise)
**Rohdaten:** [phase-0_daten-pagespeed-2026-09-20.json](phase-0_daten-pagespeed-2026-09-20.json)

> **Datenlage:** Es gibt **keine CrUX-Felddaten** (weder URL- noch Origin-Level) — die Seite hat zu wenig Chrome-Traffic. Alle Werte unten sind **Lab-Daten** (simuliertes Moto G Power, Slow-4G-Throttling). Echte Nutzer auf WLAN/LTE erleben deutlich bessere Zeiten; Google hat mangels Felddaten aktuell **kein CWV-Ranking-Signal** für diese Seite. INP ist im Lab nicht messbar (TBT dient als Proxy).

## Lighthouse-Scores

| Kategorie | Mobile (Lauf 1 / Lauf 2) | Desktop |
|---|---|---|
| Performance | 🔴 55 / 47 | 🟠 59 |
| Accessibility | 🟠 88 | 🟠 88 |
| Best Practices | 🟢 96 | 🟢 96 |
| SEO | 🟢 92 | 🟢 92 |

## Lab-Metriken

| Metrik | Mobile Lauf 1 | Mobile Lauf 2 | Desktop | Schwelle „Good" |
|---|---|---|---|---|
| FCP | 🔴 13,4 s | 🔴 12,3 s | 🟢 0,9 s | ≤ 1,8 s |
| LCP | 🔴 18,9 s | 🔴 18,4 s | 🟠 2,5 s | ≤ 2,5 s |
| TBT (INP-Proxy) | 🟢 40 ms | 🟠 360 ms | 🔴 540 ms | ≤ 200 ms |
| CLS | 🟢 0 | 🟢 0 | 🟢 0,002 | ≤ 0,1 |
| Speed Index | 🔴 13,4 s | 🔴 12,3 s | 🟠 2,0 s | ≤ 3,4 s |
| TTI | 🔴 19,1 s | 🔴 18,9 s | 🟠 4,3 s | ≤ 3,8 s |
| TTFB (Server) | 🟢 64 ms | — | 🟢 | ≤ 800 ms |

Der Mobile-Befund ist reproduzierbar (zwei Läufe, ±1 s). Server ist schnell (64 ms) — das Problem liegt vollständig im **Frontend: render-blockierende Ressourcen + 3,1 MB Payload**.

## Ursachen (Mobile)

### 1. Render-blockierende Requests — geschätzt −2,9 s FCP
| Ressource | Größe | Blockiert |
|---|---|---|
| Squarespace `site.css` | 110 KB (80 % ungenutzt) | 5,1–5,6 s |
| **`cdn.weglot.com/weglot.min.js`** | 73 KB | **3,8–3,9 s** |
| Squarespace `static.css` | 40 KB (93 % ungenutzt) | 2,7 s |
| Form-Component-CSS, user-account-CSS | je ~2 KB | je 0,75 s |

### 2. Ungenutztes JavaScript — 982 KB, geschätzt −7,3 s
| Script | Größe | Ungenutzt |
|---|---|---|
| **reCAPTCHA Enterprise `recaptcha__en.js`** — wird **3× geladen** | 3 × 345 KB ≈ 1 MB | ~54 % |
| Squarespace `common.js` | 253 KB | 140 KB |
| Squarespace Template-Chunk `3155.js` | 164 KB | 131 KB |
| Squarespace `common-vendors.js` | 231 KB | 97 KB |

Third-Party-Anteil: Squarespace 1,5 MB · Google (reCAPTCHA) 755 KB · Weglot 110 KB · sqspcdn 100 KB.

### 3. Bilder — ~110–150 KB
- **Logo `KKT_LOGO_v7@4x.png` wird mit `?format=1500w` geladen (78 KB, 96 % verschwendet)** — dargestellt wird es nur in Header-Größe.
- Hero-Bild `…little-lambs-kindergarten-lernen.jpg` 750w: ~35 KB Einsparung möglich (Kompression/Format).
- Einige `<img>` ohne `width`/`height` (CLS aktuell trotzdem 0).

### 4. Sonstiges
- Legacy-JS/Polyfills: 119 KB (Squarespace-Core, nicht beeinflussbar)
- Kurze Cache-Lifetimes: Weglot (30 min), MailerLite (5 Tage) — Third-Party, nicht beeinflussbar
- Konsole: `requestStorageAccess: Permission denied` aus reCAPTCHA-iframe (harmlos)

## SEO-Audit (92/100)
- ❌ **Meta-Description fehlt auf der Startseite** — einziger fehlgeschlagener SEO-Check
- ✅ crawlbar, Title, HTTP 200, robots.txt, canonical, hreflang, Alt-Texte, Linktexte

## Accessibility (88/100)
- Links ohne erkennbaren Namen (`link-name`) — vermutlich Icon-/Bild-Links (Social, Logo)
- Farbkontrast unzureichend
- Überschriften-Reihenfolge springt (z. B. H1 → H3)
- ARIA: `aria-required-children`, `presentation-role-conflict` (Squarespace-Template-Markup, kaum beeinflussbar)

## Maßnahmenplan

Auf Squarespace sind Core-CSS/JS nicht editierbar — der Hebel liegt bei **Third-Party-Scripts und Inhalten**.

| # | Prio | Maßnahme | Grundlage | Erfolgskontrolle (falsifizierbar) |
|---|---|---|---|---|
| 1 | **High** | **Meta-Description** für die Startseite setzen (Seiten-Einstellungen → SEO), 140–155 Zeichen, DE | Einziger roter SEO-Check; beeinflusst CTR direkt | PSI-SEO-Score → 100; GSC-CTR der Startseite nach 4 Wochen vergleichen |
| 2 | **High** | **reCAPTCHA von der Startseite entfernen/verzögern**: Formular-/Newsletter-Block (Squarespace-Form + MailerLite-Popup) von der Startseite auf eine Unterseite/Kontaktseite verlagern oder im Footer durch einfachen Link ersetzen | ~1 MB JS (3× geladen) nur für ein Formular; größter einzelner Payload-Posten | `recaptcha__en.js` taucht im PSI-Netzwerk-Log nicht mehr auf; Payload < 2,2 MB |
| 3 | **High** | **Weglot nicht render-blockierend laden**: Snippet mit `async`/`defer` einbinden bzw. Weglot-Doku für Squarespace prüfen. Alternative: Weglot-Subdomain/-Subdirectory-Integration (serverseitig, besser für hreflang/SEO als JS-Übersetzung) | 3,8 s Blockierzeit im Mobile-Lab | Weglot verschwindet aus „Render-blocking requests"; Mobile-FCP sinkt um > 2 s. Risiko: kurzes Aufblitzen der Originalsprache — nach Umstellung prüfen |
| 4 | Medium | **Logo neu hochladen**: als SVG oder PNG mit max. ~2× Darstellungsbreite (z. B. 400–500 px) | 78 KB für ein Header-Logo, 96 % verschwendet | Logo-Request < 10 KB |
| 5 | Medium | Hero-Bild vor Upload auf ~1500 px Breite, JPEG-Qualität ~75 komprimieren (Squarespace liefert WebP selbst aus) | LCP-Element auf Mobile | LCP-Ressource < 50 KB bei 750w |
| 6 | Medium | Überschriften-Hierarchie korrigieren (H1 → H2 → H3 ohne Sprünge), Icon-Links mit Beschriftung/`aria-label` versehen, Kontrast der betroffenen Textfarben anheben | A11y 88; Heading-Struktur hilft auch SEO/AI-Extraktion | A11y-Score ≥ 95 |
| 7 | Low | MailerLite-Popup-Script nur laden, wenn Popup wirklich genutzt wird | Zusätzliches Third-Party-JS | Request entfällt |

**Abhängigkeiten:** #2 und #3 sind unabhängig und bringen zusammen den Großteil. #4/#5 lohnen erst danach messbar. Nicht beeinflussbar: Squarespace-Core-CSS/JS (~1,5 MB), Legacy-Polyfills, ARIA-Template-Markup — ein Mobile-Score > ~70 ist auf Squarespace 7.x realistisch die Obergrenze.

**Leitindikator ohne erneuten Audit:** PSI-Mobile-Performance-Score und „Total size" nach jeder Änderung; sobald genügend Traffic vorhanden ist, erscheint in GSC → „Core Web Vitals" ein Bericht mit Felddaten.

## Einordnung
- Desktop ist ok-ish (LCP 2,5 s an der Grenze, TBT 540 ms durch JS-Ausführung).
- Mobile-Lab-Werte (18 s LCP) sind durch Slow-4G-Simulation × render-blockierende Kette × 3 MB Payload extrem; reale Spender:innen in DE auf LTE/WLAN dürften eher 3–5 s erleben — trotzdem zu langsam für eine Spendenseite, bei der mobile Absprünge direkt Spenden kosten.
- Ohne CrUX-Daten kein direkter CWV-Ranking-Nachteil, aber auch kein Bonus.

---

## Meta-Descriptions zum Eintragen in Squarespace (Maßnahme #1)

**Wo:** Seiten → Zahnrad neben der jeweiligen Seite → **SEO** → „SEO-Beschreibung" (füllt auch `og:description` für WhatsApp/Facebook-Vorschau). SEO-Titel im selben Dialog.
**Hinweise:** „du"-Ansprache und Schweizer Schreibweise (ss statt ß) wie auf der Website. Alle Fakten stammen von der jeweiligen Seite. Unter Marketing → SEO-Darstellung prüfen, dass der Site-Name nicht doppelt an den Titel angehängt wird.

### Priorität nach echten Suchdaten (GSC, letzte 90 Tage)

Descriptions sind kein Ranking-Faktor; sie wirken nur auf die Klickrate dort, wo eine Seite bereits Impressionen hat — und auf die Link-Vorschau beim Teilen.

| Prio | Seite | Impressionen | Klicks | Ø Position | Urteil |
|---|---|---|---|---|---|
| 1 | `/` | 222 | 34 | 11,3 | **Pflicht** |
| 1 | `/ueberuns` | 171 | 1 | 12,6 | **Pflicht** — viele Impressionen, fast keine Klicks |
| 1 | `/spenden` | 26 | 0 | 20,5 | **Pflicht** — wichtigste Seite, wird per WhatsApp/Mail geteilt |
| 2 | `/geschichte`, `/projekte` | 36 / 31 | 0 | 8,0 / 14,5 | sinnvoll |
| 3 | 10 Projektseiten | je 1–11 | 0 | — | optional — in der Suche kaum messbar, nützt nur der Teilen-Vorschau |
| 3 | `/blog` | 4 | 0 | — | optional |
| – | `/impressum` | 40 | 1 | 8,6 | keine Description; dass es vor `/spenden` rankt, zeigt schwache Titel/H1 der Hauptseiten |

Gesamt ~580 Impressionen in 90 Tagen: Das Hauptproblem ist **Sichtbarkeit**, nicht Snippet-Text. Mehr Hebel haben SEO-Titel + eigene H1 pro Seite (echte Ranking-Signale), Organization-Schema und externe Erwähnungen — siehe [phase-1_audit-gesamt-2026-09-20.md](phase-1_audit-gesamt-2026-09-20.md).

### Hauptseiten

- [ ] **`/` (Startseite)**
  - SEO-Titel: `Kinderhilfe Kapstadt – Spenden für Kinder in Südafrika` (54)
  - Description (152):
    ```
    Seit über 25 Jahren fördert die Kinderhilfe Kapstadt zehn Bildungs- und Sozialprojekte für Kinder in Townships rund um Kapstadt. Hilf mit deiner Spende.
    ```

- [ ] **`/spenden`**
  - SEO-Titel: `Spenden & Spendenbescheinigung – Kinderhilfe Kapstadt` (53)
  - Description (153):
    ```
    Spende für Kinder in Kapstadt – aus Deutschland, der Schweiz oder Südafrika. Bankdaten, PayPal, Twint und Infos zur Spendenbescheinigung auf einen Blick.
    ```
  - Nebenbei: Tippfehler „Standart Bank" → „Standard Bank" korrigieren.

- [ ] **`/projekte`**
  - SEO-Titel: `Unsere Projekte in Kapstadt – Kinderhilfe Kapstadt` (50)
  - Description (150):
    ```
    Zehn Projekte in Townships rund um Kapstadt: Kindergärten, Waisenhaus, Musikschule und Nachmittagsbetreuung. Lerne die Partner der Kinderhilfe kennen.
    ```

### Projektseiten

- [ ] **`/projekte/little-lambs`** (154)
  ```
  Little Lambs ist ein Kindergarten im Township Imizamo Yethu, Hout Bay: Seit 1999 erhalten hier rund 300 Kinder von 1 bis 6 Jahren Betreuung und Förderung.
  ```
- [ ] **`/projekte/imizamo-yethu-learning-hub`** (153)
  ```
  Der Imizamo Yethu Learning Hub in Hout Bay begleitet 125 Schulkinder nach dem Unterricht: Hausaufgabenhilfe, Mathe-Förderung, Kunst, Sport und Mentoring.
  ```
- [ ] **`/projekte/heuwel-speelskool`** (155)
  ```
  Die Heuwel Speelskool in Calitzdorp ist ein christlicher Kindergarten für Kinder von 2 bis 6 Jahren – mit festem Tagesablauf, Mahlzeiten und Frühförderung.
  ```
- [ ] **`/projekte/true-north`** (149)
  ```
  True North bildet seit 2007 Erzieherinnen in den Townships Capricorn, Vrygrond und Overcome Heights aus – für gute frühkindliche Bildung in Kapstadt.
  ```
- [ ] **`/projekte/funda-kunye`** (152)
  ```
  Funda Kunye heisst „gemeinsam lernen“: Das Projekt schult und begleitet Erzieherinnen in 22 kleinen Kindergärten in Imizamo Yethu und Hangberg, Hout Bay.
  ```
- [ ] **`/projekte/baphumelele`** (148)
  ```
  Baphumelele in Khayelitsha, Kapstadts grösstem Township: Kinderheim, Gemeinschaftsküche und Jugendprogramm – 1989 von „Mama Rosie“ Mashale gegründet.
  ```
- [ ] **`/projekte/sakhisizwe`** (153)
  ```
  Sakhisizwe ist ein Afterschool-Programm für Jugendliche von 13 bis 20 Jahren in Hout Bay: Lernhilfe, Mentoring, Sport und ein sicherer Ort am Nachmittag.
  ```
- [ ] **`/projekte/kronendal-music-academy`** (154)
  ```
  Die Kronendal Music Academy in Hout Bay eröffnet Kindern aus Townships seit 2007 Musik: Unterricht in über 26 Instrumenten, Chöre, Ensembles und Konzerte.
  ```
- [ ] **`/projekte/ikhaya-le-themba`** (155)
  ```
  iKhaya le Themba, das „Haus der Hoffnung“ in Imizamo Yethu: Täglich erhalten rund 160 Kinder von 6 bis 12 Jahren Hausaufgabenhilfe und eine warme Mahlzeit.
  ```
- [ ] **`/projekte/ark-angels`** (150)
  ```
  Ark Angels Educare ist ein Kindergarten in Overcome Heights / Vrygrond, Kapstadt: frühkindliche Förderung seit 2015 – heute in einem eigenen Gebäude.
  ```

### Weitere Seiten

Einordnung: Meta-Descriptions sind **kein Ranking-Faktor** — sie steuern nur den Snippet-Text (Klickrate) und die Link-Vorschau beim Teilen. Deshalb nur dort, wo es etwas bringt:

- [ ] **`/ueberuns`** — sinnvoll: Vertrauensseite, erscheint bei Marken-/Personensuchen („Marlis Schaper", „Elke Zwicker") und als Sitelink (156)
  - SEO-Titel: `Über uns – Kinderhilfe Kapstadt` (31)
  ```
  Wer hinter der Kinderhilfe Kapstadt steht: Gründerin Marlis Schaper, Projektleiterin Elke Zwicker und ein Netzwerk, das täglich rund 3'000 Kinder begleitet.
  ```
- [ ] **`/geschichte`** — sinnvoll: Sitelink-Kandidat, wird gern geteilt (144)
  - SEO-Titel: `Geschichte – Kinderhilfe Kapstadt seit 1999` (43)
  ```
  Vom Container-Kindergarten Little Lambs 1999 im Township Imizamo Yethu zum Netzwerk aus zehn Projekten: die Geschichte der Kinderhilfe Kapstadt.
  ```
- [ ] **`/blog`** — geringer Nutzen, aber 1 Minute Aufwand; vor allem für die Link-Vorschau (145)
  ```
  Neuigkeiten aus den Projekten der Kinderhilfe Kapstadt: Berichte aus den Townships, Bauprojekte, Rundbriefe und was deine Spende vor Ort bewirkt.
  ```
- **`/impressum` — bewusst keine Description.** Niemand sucht danach, die Seite konkurriert um keinen Klick, und Google zeigt bei Bedarf ohnehin den passenden Textausschnitt (Adresse, PBO-/NPO-Nummer). Zeit besser in die Blogposts stecken.
- **Blogposts:** Squarespace nutzt den **Auszug (Excerpt)** des Beitrags als Description — dort je 1–2 Sätze eintragen. Wichtiger ist zuerst das Umbenennen der Platzhalter-Slugs (siehe [phase-1_fix-blog-url-slugs-2026-09-20.md](phase-1_fix-blog-url-slugs-2026-09-20.md)).

### Offene Punkte beim Eintragen
- `/blog`: Titel „Vorschulen von True **Noth**" → „True North". Der Oster-Beitrag spricht von „acht Projekten", die Website sonst von zehn (alter Beitrag — so lassen oder Hinweis ergänzen).
- `/geschichte` nennt im ersten Absatz 2000 als Beginn, weiter unten 1999 als Gründung von Little Lambs — im Einleitungssatz klarstellen (1999 Gründung Little Lambs, 2000 Einstieg Marlis Schaper).
- Die Zahl „rund 3'000 Kinder täglich" steht auf `/ueberuns` — sie gehört auch auf die Startseite und `/projekte` (stärkstes Wirkungsargument), dann kann sie dort auch in die Description.
- `/projekte/sakhisizwe` und `/projekte/ikhaya-le-themba`: Seitentext sagt „unterstützt **wurde**" (Vergangenheit) — falls die Unterstützung noch läuft, im Text korrigieren.
- True North: Startseite nennt „rund 30 Kindergärten", Projektseite keine Zahl — vereinheitlichen, dann ggf. in die Description aufnehmen.
- Auf allen 10 Projektseiten fehlt ein eigenes H1 (siehe [phase-1_audit-gesamt-2026-09-20.md](phase-1_audit-gesamt-2026-09-20.md), Punkt 2) — beim Öffnen der Seite gleich die erste Überschrift als H1 formatieren, z. B. „Baphumelele – Kinderheim in Khayelitsha".

**Kontrolle:** Nach dem Speichern PSI erneut laufen lassen → SEO-Score der Startseite sollte 100 sein; in GSC nach ~4 Wochen CTR der Marken-Suchanfragen vergleichen.
