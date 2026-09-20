# SEO-Audit — kinderhilfekapstadt.com

Datum: 2026-09-20 · Plattform: Squarespace · Sprache: Deutsch (`de-CH`) + Weglot-EN-Overlay · 27 Sitemap-URLs + Root
Geschäftstyp: Non-Profit / Hilfsorganisation (YMYL-nah: Spenden). Weder lokal noch E-Commerce.

← zurück zum [SEO-Plan](00_seo-plan.md)

## SEO Health Score: 54 / 100

| Kategorie | Gewicht | Score | Grundlage |
|---|---|---|---|
| Technisches SEO | 22 % | 72 | curl-Crawl aller 28 URLs |
| Content-Qualität / E-E-A-T | 23 % | 58 | Textextraktion von 17 Seiten |
| On-Page-SEO | 20 % | 42 | auf allen 28 URLs nachgeprüft |
| Schema | 10 % | 52 | JSON-LD auf 8 Seiten |
| Performance | 10 % | 45 | Lighthouse 13 Mobile-Lab, Einzellauf — siehe Vorbehalt |
| AI-Search-Readiness | 10 % | 47 | robots.txt, llms.txt, Entity-Checks |
| Bilder | 5 % | 50 | Schätzung, Teilbelege |

**Nicht abgedeckt:** Google Search Console / CrUX / GA4 (zum Audit-Zeitpunkt keine API-Credentials — inzwischen nachgeholt, siehe [phase-0_report-pagespeed-2026-09-20.md](phase-0_report-pagespeed-2026-09-20.md) und [phase-0_report-gsc-performance-2026-09-20.md](phase-0_report-gsc-performance-2026-09-20.md)), Backlinks (Script braucht `pip install -r ~/.claude/skills/seo/requirements.txt`), Drift (zum Audit-Zeitpunkt keine Baseline — inzwischen gesetzt). Die PageSpeed-Insights-API lieferte zweimal 429 → keine Felddaten. Die SERP-Analyse besteht nur aus zwei US-basierten Websuchen, nicht google.de/.ch.

## Was bereits gut ist

- Alle 28 URLs liefern 200; HTTPS + Apex→www in einem einzigen 301-Hop; HSTS; echte 404er.
- Der Content wird serverseitig gerendert (kein JS nötig, um den deutschen Inhalt zu lesen); eindeutige Titles; kein versehentliches noindex; Canonicals korrekt.
- Die robots.txt blockiert AI-Crawler **nicht**: Es gibt kein `Disallow: /`; GPTBot, ClaudeBot usw. teilen nur die üblichen partiellen Squarespace-Disallows mit `User-agent: *`. (Ein Teil-Check behauptete das Gegenteil; nachgeprüft und widerlegt.)
- Vertrauens-Grundlagen sind vorhanden: PBO 930078385 und NPO 322-576 auf /impressum, namentlich genannte Trustees mit Kurzbiografien, vollständige Bankdaten für DE/CH/ZA, Infos zur Spendenbescheinigung, PayPal/Twint, aktiver Blog (Beiträge bis August 2026).
- CLS ≈ 0, TTFB ~80–130 ms, Sitemap in der robots.txt referenziert, 193 Bildeinträge.

## Befunde

### Critical

1. **Meta-Descriptions leer auf 23 von 28 URLs** — `/`, `/spenden`, `/projekte`, alle 10 Projektseiten, `/ueberuns`, `/geschichte`, `/impressum`, `/blog`, 5 Blogbeiträge. Folge: og:description/twitter:description sind ebenfalls leer, Such-Snippets und Social-Vorschauen sind damit unkontrolliert.
2. **Kein seitenspezifisches H1 auf 15 Seiten** — `/spenden`, `/ueberuns`, `/geschichte`, `/impressum`, `/projekte` und alle 10 Projektseiten haben genau ein H1: den Site-Titel „The Kinder Hilfe Kapstadt Trust". Beispiel: `/projekte/sakhisizwe` hat kein H1 „Sakhisizwe". Blogbeiträge und Startseite machen es richtig.
3. **Keine Seite zur finanziellen Transparenz** (Jahresbericht / Mittelverwendung). Weder in der Navigation noch im Seitentext etwas gefunden. Für eine Website, die um Banküberweisungen bittet, ist das die grösste Vertrauenslücke.

### High

4. **Die englische Version ist für die Suche unsichtbar.** Weglot läuft als clientseitiges JS-Overlay (`language_from: de`, `en`, `auto_switch: true`). Englisch hat keine eigenen URLs (`/en/` → 404), kein hreflang; der Server liefert für jedes Accept-Language Deutsch aus. Englischsprachige Spender:innen/CSR-Prüfer:innen können nur die deutschen Seiten finden. Nebeneffekt: Englischsprachige Browser (und Test-Tools) sehen eine automatisch übersetzte Oberfläche.
5. **Startseite wiegt 12,8 MB, davon >10 MB Hintergrundvideo-Segmente** (Lighthouse). Lab-LCP 17,2 s, TBT 490 ms, Score 43. `/spenden` (3,3 MB, LCP-Element = Hero-Bild, Score 56) und `/projekte` (2,6 MB, Score 55) haben kein Video; ihr langsamer Lab-Paint kommt von ~1,5 s render-blockierendem CSS/JS, 346 KB reCAPTCHA auf jeder Seite und dem Fade-in der Überschriften im Template. *Vorbehalt: simuliertes Throttling, Einzellauf, während des Tests wurde ein AWS-WAF-Challenge-Script geladen — als „langsam" lesen, nicht als exakte Sekunden. Auf pagespeed.web.dev gegenprüfen.*
6. **Sehr schwere HTML-Dokumente**: `/projekte/funda-kunye` 1,7 MB, `/geschichte` 1,5 MB, `/projekte/kronendal-music-academy` 1,4 MB, `/ueberuns` 1,2 MB (normale Seiten: 120–350 KB). Vermutlich Inline-Daten von Galerien/Embeds.
7. **Kein Organization-/NGO-Schema.** Jede Seite trägt nur den identischen Squarespace-`WebSite`-Block (leere `description`, protokoll-relatives `image`). Kein `sameAs`, keine DonateAction, keine Breadcrumbs.
8. **Mobile: kein Spenden-CTA above the fold.** Desktop hat eine dauerhaft sichtbare „Spenden"-Pille im Header; Mobile zeigt nur das Hamburger-Menü (Screenshots [home_mobile.png](phase-1_screenshots/home_mobile.png), [spenden_mobile.png](phase-1_screenshots/spenden_mobile.png)). Auf `/spenden` beginnen die Bankdaten erst nach Hero + Intro.
9. **Drei Spenden-Rechtsträger, nirgends erklärt**: Deutschland → „Dietlich F. Liedelt Stiftung", Schweiz → „Ubungani Suisse", Südafrika → der Trust. Kein Satz erklärt, wie sie zusammenhängen. Wer zum ersten Mal spendet (und jedes AI-System), sieht drei Namen für eine Sache.
10. **Dünne Schlüsselseiten**: `/spenden` 280 Wörter, `/projekte` 225, Startseite 401, Projektseiten 318–473, ohne Wirkungszahlen pro Projekt.

### Medium

11. **Vier echte Blogbeiträge mit Platzhalter-Slugs**: „Frohe Ostern!", „Brand im Township", „Updates zu unseren Projekten", „Änderungen 2026" liegen unter `/blog/blog-post-title-two-…` usw.; ein Slug enthält echte Leerzeichen (`/blog/Blog Post Title One-3zaa9-zlxng-64p6e`), was in der Sitemap ungültig ist. **Umbenennen, nicht löschen** — der Inhalt ist echt. (Ein Teil-Check empfahl Löschen in der Annahme von Demo-Text; nachgeprüft und widerlegt.) → Anleitung: [phase-1_fix-blog-url-slugs-2026-09-20.md](phase-1_fix-blog-url-slugs-2026-09-20.md)
12. **Der Startseiten-Title ist nur der juristische Name** „The Kinder Hilfe Kapstadt Trust" — kein „Spenden", „Kinder", „Südafrika".
13. **Private Postfächer als offizielle Kontakte**: zwei gmail.com-Adressen auf /impressum, eine sunrise.ch auf der Startseite.
14. **Kein externer Entity-Fussabdruck verlinkt**: Es existiert eine LinkedIn-Unternehmensseite (`za.linkedin.com/company/the-kinderhilfe-kapstadt-trust`), aber die Website verlinkt auf kein einziges Social-Profil. Kein Wikidata-/Wikipedia-Eintrag gefunden. betterplace / Zewo / DZI / ZA-NPO-Register: nicht verifiziert.
15. **Konkurrenz in der Marken-SERP durch verwandte Domains**: Für „Kinderhilfe Kapstadt" erscheinen neben der Website `littlelambs-kapstadt.com` (Seitentitel „Kinder Hilfe Kapstadt", live, 200), `ubungani.org/project/kinderhilfe-kapstadt/` und `arkangels-educare.com`. Klären, wer littlelambs-kapstadt.com kontrolliert und ob deren Title die Marke weiterhin tragen soll.
16. **Defekte im automatisch erzeugten Schema** (von der Plattform ausgegeben): `@context` mit http, Article-`image` mit http, `author` als einfacher String. Nicht behebbar; geringe Auswirkung.
17. `/home` steht in der Sitemap, die Root-URL `/` nicht; das Canonical zeigt auf beiden auf die Root. In Squarespace nicht editierbar; praktisch geringes Risiko — nur nie auf `/home` verlinken (der Nav-Link „Kontakt" ist `/home#contact`).

### Low

18. „Click Here" steht 4× im HTML der ansonsten deutschen Startseite; ungenutzter `/cart`-Link im Header; Tippfehler „Standart Bank" auf `/spenden`; die dritte Trustee (Monique van Ginneken) wird auf /impressum genannt, aber nicht auf /ueberuns.
19. Kein WebP/AVIF im statischen HTML gesehen; bei einigen Bildern pro Seite fehlt der Alt-Text (Startseite 3/16, Spenden 3/8 laut einem Teil-Check; nicht unabhängig verifiziert).
20. Kein `/llms.txt` (404). Squarespace kann Root-Dateien nicht direkt hosten; nur über Datei-Upload + URL-Zuordnung als Redirect möglich. Marginaler Nutzen.
21. Security-Header (CSP, Referrer-Policy, doppeltes X-Frame-Options), Brotli, IndexNow: von der Plattform vorgegeben. Ignorieren.

### Manuell nachprüfen (automatische Erfassung nicht eindeutig)

- Die Ganzseiten-Screenshots zeigten leere Blöcke auf `/spenden` und einen leeren Footer — höchstwahrscheinlich, weil der Scroll-Fade-in des Templates im Headless-Capture nicht auslöst. Auf einem echten Smartphone prüfen.
- In keinem Capture erschien ein Cookie-Banner, obwohl reCAPTCHA, Weglot und PayPal laden. Consent-Handling prüfen (iubenda ist auf /impressum verlinkt).

## Synthese

**Wahrnehmen.** Die Website ist dank Plattform-Standard technisch gesund und hat echte Substanz (25 Jahre Arbeit, namentlich genannte Personen, Registrierungsnummern, echte Bankdaten). Was fehlt, sind fast ausschliesslich *nicht ausgefüllte Felder und nicht erklärte Strukturen*: leere Descriptions, Template-H1s, Standard-Slugs, kein Organisations-Markup, keine Erklärung der drei rechtlichen Spendenwege.

**Analysieren.** Die Befunde 1, 2, 7, 12 haben eine gemeinsame Ursache — die Squarespace-SEO-Felder wurden nach dem Relaunch im Juni 2026 nie ausgefüllt. Die Befunde 3, 9, 10, 13, 14 haben eine andere — die Website setzt voraus, dass Besucher:innen die Organisation bereits kennen und ihr vertrauen. Für eine kleine Hilfsorganisation sind die gewinnbaren Suchanfragen Marke, Projektnamen und Long-Tail („Baphumelele spenden", „Little Lambs Hout Bay") — also genau dort, wo H1/Title/Description auf Seitenebene am meisten zählen. Performance (5) ist eine einzige Entscheidung: das Startseiten-Video.

**Validieren.** Die Lab-Performance-Zahlen sind hier der schwächste Beleg; alles unter „Critical" wurde direkt an allen 28 URLs nachgeprüft. Ranking- oder Traffic-Daten lagen nicht vor, die Wirkung ist also aus Grundprinzipien hergeleitet, nicht gemessen. Akzeptieren, dass Security-Header, Sitemap-Interna und das Gewicht des Core-JS auf Squarespace nicht änderbar sind.

**Handeln.** Reihenfolge unten: zuerst Felder ausfüllen (Stunden, keine Abhängigkeiten), dann Vertrauens-Content, dann die strukturelle Weglot-Entscheidung.

## Aktionsplan

| # | Massnahme (wo in Squarespace) | Beruht auf | Hängt ab von | Gescheitert, wenn… | Beobachten |
|---|---|---|---|---|---|
| 1 | Meta-Descriptions für 23 Seiten schreiben (Seiten-Einstellungen → SEO). Beginnen mit `/`, `/spenden`, `/projekte`, 10 Projekten | 23/28 leer; behebt auch og:description | — | Google zeigt nach 4 Wochen für Marken-Suchanfragen weiterhin selbst gezogene Snippets | GSC → CTR bei Marken-Suchanfragen; `site:`-Snippets |
| 2 | Pro Seite ein echtes H1 setzen (ersten Textblock als Überschrift 1, z. B. „Baphumelele – Kinderheim in Khayelitsha"); Site-Titel herabstufen, falls das Template es zulässt | 15 Seiten teilen sich ein H1 | — | Seite rankt weiterhin nur für die Marke, nicht für den Projektnamen | GSC-Suchanfragen pro Projekt-URL |
| 3 | SEO-Titel der Startseite + Site-Beschreibung setzen (Marketing → SEO-Darstellung) | Title = nur juristischer Name | — | Google schreibt den Title in der SERP um | SERP-Title für „Kinderhilfe Kapstadt" |
| 4 | 4 Blog-Slugs umbenennen (Beitrags-Einstellungen → URL), URL-Zuordnungen alt→neu (301) anlegen | Platzhalter-Slugs, einer mit Leerzeichen | — | Alte URLs liefern 404 statt 301 | Sitemap zeigt saubere Slugs; GSC ohne neue 404er |
| 5 | NGO- + BreadcrumbList-JSON-LD einfügen (Einstellungen → Erweitert → Code-Injection; Breadcrumbs pro Seiten-Header) | Nur `WebSite`-Schema | 14 (sameAs) | Rich Results Test zeigt Fehler | Schema-Validator; GSC-Verbesserungen |
| 6 | Auf `/spenden` einen kurzen Block „So funktioniert deine Spende" ergänzen, der Trust ↔ Liedelt Stiftung ↔ Ubungani Suisse erklärt; Bankdaten direkt unter die Überschrift ziehen; „Standart" korrigieren | 3 unerklärte Rechtsträger; Daten below the fold | — | Spender:innen fragen weiterhin per Mail, welches Konto sie nehmen sollen | Spendenanfragen; Scrolltiefe |
| 7 | Seite „Transparenz / Jahresbericht" veröffentlichen: Einnahmen, Verteilung pro Projekt, Verwaltungsanteil, Trustees, Registrierungsnummern, PDF-Bericht | Nirgends Finanzinformationen | — | Seite existiert, enthält aber keine Zahlen | Anfragen von Stiftungen/CSR; Verweildauer |
| 8 | Hintergrundvideo der Startseite durch ein Standbild ersetzen (oder einen kurzen Clip ≤2 MB); Formular-/reCAPTCHA-Blöcke von Seiten ohne Formular entfernen | 10 MB Video auf einer 12,8-MB-Seite | — | PSI-Mobile-LCP auf `/` weiterhin >4 s | pagespeed.web.dev monatlich |
| 9 | „Spenden"-Button in den Mobile-Header aufnehmen (Header → Button → auf Mobile anzeigen) | Kein CTA above the fold auf Mobile | — | Button bei 390 px hinter dem Hamburger versteckt | Klicks auf `/spenden` von Mobile |
| 10 | Wirkungsfakten auf jeder Projektseite ergänzen (seit wann, wie viele Kinder, was Spenden finanzieren, ein datiertes Update) → 500+ Wörter | 318–473 Wörter, keine Zahlen | 2 | Nach 8 Wochen keine Long-Tail-Impressionen | GSC-Impressionen pro Projekt-URL |
| 11 | Englisch entscheiden: Weglot auf Subdirectory-/Subdomain-Integration umstellen (eigene URLs + hreflang) **oder** rein deutsches SEO akzeptieren und `auto_switch` abschalten | EN hat keine URLs | zuerst 1–3 (werden mitübersetzt) | `/en/` weiterhin 404 / kein hreflang im Quelltext | GSC → indexierte Seiten mit `/en/` |
| 12 | @kinderhilfekapstadt.com-Adressen für alle Trustees verwenden | gmail/sunrise auf rechtlichen Seiten | — | — | — |
| 13 | Die 4 schweren Seiten verschlanken: weniger Bilder pro Galerie-Block, native Bild-Blöcke | 1,2–1,7 MB HTML | — | HTML weiterhin >500 KB | Grössencheck per curl |
| 14 | LinkedIn im Footer verlinken; Wikidata-Eintrag anlegen; auf betterplace/relevanten Verzeichnissen listen; Title von littlelambs-kapstadt.com abstimmen | Kein Entity-Fussabdruck verlinkt | — | Marken-SERP nach 3 Monaten unverändert | Zusammensetzung von Seite 1 der Marken-SERP |
| 15 | „Click Here" durch deutsche Beschriftungen ersetzen; `/cart` aus dem Header entfernen; dritte Trustee auf /ueberuns ergänzen | Sprach-/UI-Hygiene | — | — | — |

Woche 1: 1–5, 9, 12, 15. Monat 1: 6–8, 10, 13. Quartal: 11, 14.

## Zum direkten Einfügen

### Titles und Meta-Descriptions (die Website duzt)

> Die vollständige, nach GSC-Daten priorisierte Liste für **alle** Seiten steht in [phase-0_report-pagespeed-2026-09-20.md](phase-0_report-pagespeed-2026-09-20.md) — jene Fassung ist neuer und hat Vorrang.

| Seite | SEO-Titel | Meta-Description |
|---|---|---|
| `/` | Kinderhilfe Kapstadt – Spenden für Kinder in Südafrika (54) | Seit über 25 Jahren unterstützt die Kinderhilfe Kapstadt zehn Bildungs- und Sozialprojekte in Townships rund um Kapstadt. Informiere dich und spende. (149) |
| `/spenden` | Spenden & Spendenbescheinigung – Kinderhilfe Kapstadt (53) | Spende aus Deutschland, der Schweiz oder Südafrika: Bankdaten, PayPal, Twint und Infos zur Spendenbescheinigung auf einen Blick. (128) |
| `/projekte` | Unsere Projekte in Kapstadt – Kinderhilfe Kapstadt (50) | Zehn Bildungs- und Sozialprojekte in Townships rund um Kapstadt: Kindergärten, Schulen, Musikakademie und Jugendförderung für rund 3'000 Kinder täglich. (152) |
| `/ueberuns` | Über uns – Kinderhilfe Kapstadt (31) | Wer hinter der Kinderhilfe Kapstadt steht: Gründerin Marlis Schaper, Projektleiterin Elke Zwicker und über 25 Jahre Engagement für Kinder in Südafrika. (151) |

Das Titelformat unter Marketing → SEO-Darstellung prüfen, damit der Site-Name nicht doppelt angehängt wird.

### NGO-Schema (Code-Injection → Header, site-weit)

Alle Werte stammen von /impressum und /geschichte. `sameAs`-Einträge nur für Profile ergänzen, die ihr selbst kontrolliert.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "NGO",
  "name": "The Kinder Hilfe Kapstadt Trust",
  "alternateName": ["Kinderhilfe Kapstadt", "Kinderhilfe-Kapstadt"],
  "url": "https://www.kinderhilfekapstadt.com",
  "logo": "https://images.squarespace-cdn.com/content/v1/69e7b73fa7a847762363eed5/f1269460-b875-48bf-9d0a-5092446f067f/KKT_LOGO_v7%404x.png",
  "description": "Der The Kinder Hilfe Kapstadt Trust begleitet und unterstützt zehn Bildungs- und Sozialprojekte für Kinder und Jugendliche in Townships rund um Kapstadt, Südafrika.",
  "foundingDate": "2020",
  "founder": { "@type": "Person", "name": "Marlis Schaper" },
  "areaServed": { "@type": "Place", "name": "Kapstadt, Western Cape, Südafrika" },
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "68 Forest Glade, 104 Tokai Road",
    "addressLocality": "Tokai, Kapstadt",
    "addressRegion": "Western Cape",
    "postalCode": "7945",
    "addressCountry": "ZA"
  },
  "email": "elke.zwicker@kinderhilfekapstadt.com",
  "telephone": "+27792683751",
  "identifier": [
    { "@type": "PropertyValue", "propertyID": "ZA PBO Number", "value": "930078385" },
    { "@type": "PropertyValue", "propertyID": "ZA NPO Number", "value": "322-576 NPO" }
  ],
  "sameAs": ["https://za.linkedin.com/company/the-kinderhilfe-kapstadt-trust"],
  "potentialAction": {
    "@type": "DonateAction",
    "name": "Jetzt spenden",
    "target": "https://www.kinderhilfekapstadt.com/spenden"
  }
}
</script>
```

### BreadcrumbList (Seiten-Einstellungen → Erweitert → Code-Injection im Seiten-Header, pro Projektseite)

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    { "@type": "ListItem", "position": 1, "name": "Startseite", "item": "https://www.kinderhilfekapstadt.com/" },
    { "@type": "ListItem", "position": 2, "name": "Projekte", "item": "https://www.kinderhilfekapstadt.com/projekte" },
    { "@type": "ListItem", "position": 3, "name": "Baphumelele", "item": "https://www.kinderhilfekapstadt.com/projekte/baphumelele" }
  ]
}
</script>
```

### llms.txt (optional, niedrige Priorität)

```
# The Kinder Hilfe Kapstadt Trust (Kinderhilfe Kapstadt)

> Gemeinnütziger Trust mit Sitz in Kapstadt, Südafrika. Begleitet und unterstützt
> zehn Bildungs- und Sozialprojekte in Townships rund um Kapstadt und erreicht
> täglich rund 3'000 Kinder und Jugendliche. Die Arbeit begann im Jahr 2000 mit
> dem Kindergarten Little Lambs (Imizamo Yethu, Hout Bay); der Trust selbst wurde
> 2020 von Marlis Schaper gegründet. Registriert als Public Benefit Organisation
> bei der South African Revenue Service (PBO 930078385, NPO 322-576 NPO).
>
> Non-profit trust based in Cape Town supporting ten education and social
> projects in townships around Cape Town, reaching about 3,000 children a day.

## Seiten / Pages
- [Über uns](https://www.kinderhilfekapstadt.com/ueberuns): Trustees und Team
- [Geschichte](https://www.kinderhilfekapstadt.com/geschichte): Entwicklung seit 2000
- [Projekte](https://www.kinderhilfekapstadt.com/projekte): Übersicht der zehn Projekte
- [Spenden](https://www.kinderhilfekapstadt.com/spenden): Spendenwege für Deutschland, Schweiz, Südafrika
- [Blog](https://www.kinderhilfekapstadt.com/blog): Aktuelle Berichte und Rundbriefe
- [Impressum](https://www.kinderhilfekapstadt.com/impressum): Rechtliche Angaben und Registrierungsnummern
```

## Belege

Aus dem temporären Session-Ordner in dieses Projekt gesichert:

- Screenshots (Desktop/Mobile, Startseite · Spenden · Projekte): [phase-1_screenshots/](phase-1_screenshots/)
- Lighthouse-Rohdaten: [Startseite](phase-1_daten-lighthouse-home-2026-09-20.json) · [Spenden](phase-1_daten-lighthouse-spenden-2026-09-20.json) · [Projekte](phase-1_daten-lighthouse-projekte-2026-09-20.json)
- Crawl-Ergebnisse aller 28 URLs: [phase-1_daten-crawl-2026-09-20.json](phase-1_daten-crawl-2026-09-20.json)

Quellen zur Marken-SERP: [LinkedIn](https://za.linkedin.com/company/the-kinderhilfe-kapstadt-trust) · [kapstadt.org](https://kapstadt.org/kapstadt/suedafrika/kinderhilfe/) · [ubungani.org](https://ubungani.org/project/kinderhilfe-kapstadt/) · [littlelambs-kapstadt.com](https://www.littlelambs-kapstadt.com/) · [arkangels-educare.com](https://www.arkangels-educare.com/) · [kids-kapstadt.de](https://kids-kapstadt.de/) · [herzenberuehren.de](https://www.herzenberuehren.de/cause/strassenkinder-projekt-in-kapstadt)
