# SEO-Plan — kinderhilfekapstadt.com

Website: https://www.kinderhilfekapstadt.com/ (Squarespace) · Start: 2026-09-20
Vorlage: Traffic-Playbook aus `Claude-SEO.md` (Obsidian-Vault, `04_Ressourcen/AI/Plugins & Skills/`)

## ⚠️ Nicht vergessen

- [ ] **Neue Drift-Baseline nach den Phase-1-Fixes setzen** — die Baseline vom 2026-09-20 hat die leere Meta-Description als Soll-Zustand gespeichert.
- [ ] **Sitemap in der GSC einreichen** — bewusst noch offen, weil erst nach dem Slug-Fix sinnvoll. Falls schon eingereicht: einfach abhaken.

---

**Prinzip:** erst Lecks stopfen, dann Content skalieren, dann monitoren. Nur CRITICAL/HIGH umsetzen — MEDIUM ist Ablenkung.

## Konventionen

- **Dateinamen:** `phase-<n>_<typ>-<thema>-<datum>.<ext>` — Typen: `report`, `audit`, `fix`, `daten`, `brief`, `screenshots`
- **Sprache:** alle Reports/Audits auf Deutsch
- **Python:** immer `~/.claude/skills/seo/.venv/bin/python` (System-Python hat kein `requests`)
- Neue Dateien nach dem Erstellen hier unter der jeweiligen Phase verlinken
- **👥 = Aufgabe für Mitarbeitende** (Inhalte in Squarespace). Jede 👥-Aufgabe verlinkt direkt auf ihre Schritt-für-Schritt-Anleitung. Alles ohne 👥 erledigt Haui. Einstieg für Mitarbeitende: [README.md](README.md)

## Übersicht

- [x] Phase 0 – Datenzugang einrichten
- [ ] Phase 1 – Bestandsaufnahme *(Analyse erledigt, Fixes offen)*
- [ ] Phase 2 – Technische Lecks schliessen
- [ ] Phase 3 – Bestehende Seiten hochziehen
- [ ] Phase 4 – Content-Lücken skalieren
- [ ] Phase 5 – Autorität & Wettbewerb
- [ ] Phase 6 – Monitoring (monatlich)

---

## Phase 0 – Datenzugang einrichten ✅

Einmalig. Ergebnis: Credential-Tier 1 (API-Key + Service Account mit GSC-Zugriff).

- [x] `/seo google setup` — Cloud-Projekt, APIs, API-Key, Service Account, GSC-Freigabe
- [x] Funktionstest `/seo google gsc` und `/seo google pagespeed`
- [ ] Sitemap in der GSC einreichen — **erst nach** dem Slug-Fix aus Phase 1, damit nicht die alten URLs eingereicht werden
- [ ] Optional: GA4 anbinden (Tier 2) — nur falls auf der Website GA4 läuft

**Dateien**
- [phase-0_report-pagespeed-2026-09-20.md](phase-0_report-pagespeed-2026-09-20.md) — PageSpeed-Report (Lab-Daten Mobile/Desktop), Massnahmenplan, **fertige Meta-Descriptions für alle Seiten**, priorisiert nach GSC-Daten
- [phase-0_report-gsc-performance-2026-09-20.md](phase-0_report-gsc-performance-2026-09-20.md) — Search-Console-Performance 28 Tage
- [phase-0_report-gsc-performance-de-2026-09-20.pdf](phase-0_report-gsc-performance-de-2026-09-20.pdf) — Search-Console-Performance 28 Tage als PDF (deutsch, inkl. Diagramm)
- [phase-0_daten-pagespeed-2026-09-20.json](phase-0_daten-pagespeed-2026-09-20.json) — PSI-/CrUX-Rohdaten

---

## Phase 1 – Bestandsaufnahme (Woche 1)

**Analyse**
- [x] `/seo audit https://www.kinderhilfekapstadt.com/` — Health Score **54/100**
- [x] `/seo drift baseline https://www.kinderhilfekapstadt.com/` — Baseline #1, 2026-09-20 09:07 UTC

**Fixes — Critical**
- [ ] 👥 Meta-Descriptions + SEO-Titel eintragen (Audit #1, #3); Pflicht: `/`, `/ueberuns`, `/spenden` → **[Anleitung](phase-1_fix-team-aufgaben-2026-09-20.md#aufgabe-1-meta-descriptions-und-seo-titel)**
- [ ] 👥 Eigenes H1 auf 15 Seiten setzen (Audit #2) → **[Anleitung](phase-1_fix-team-aufgaben-2026-09-20.md#aufgabe-2-hauptüberschrift-h1-auf-15-seiten)**
- [ ] 👥 Seite „Transparenz / Jahresbericht" veröffentlichen (Audit #7) → **[Anleitung](phase-1_fix-team-aufgaben-2026-09-20.md#aufgabe-3-seite-transparenz-und-jahresbericht)**

**Fixes — High**
- [ ] NGO- + Breadcrumb-Schema per Code-Injection einfügen (Audit #5) → Code im Audit unter „Zum direkten Einfügen"
- [ ] 👥 „Spenden"-Button im Mobile-Header (Audit #9) → **[Anleitung](phase-1_fix-team-aufgaben-2026-09-20.md#aufgabe-4-spenden-button-auf-dem-handy)**
- [ ] 👥 `/spenden`: drei Rechtsträger erklären, Bankdaten nach oben, „Standart" → „Standard" (Audit #6) → **[Anleitung](phase-1_fix-team-aufgaben-2026-09-20.md#aufgabe-5-spenden-seite-verständlicher-machen)**
- [ ] 👥 Wirkungsfakten auf den Projektseiten, 500+ Wörter (Audit #10) → **[Anleitung](phase-1_fix-team-aufgaben-2026-09-20.md#aufgabe-6-wirkungsfakten-auf-den-projektseiten)**
- [ ] Englisch-Entscheidung Weglot: Subdirectory mit hreflang **oder** `auto_switch` aus (Audit #11)
- [ ] Startseiten-Video ersetzen, reCAPTCHA von der Startseite entfernen → gehört inhaltlich zu Phase 2

**Fixes — bereits ausgearbeitet (Medium, aber 15 Minuten)**
- [ ] 👥 Kleine Textkorrekturen: „Click Here", Tippfehler, dritte Trustee, widersprüchliche Zahlen (Audit #15, #18) → **[Anleitung](phase-1_fix-team-aufgaben-2026-09-20.md#aufgabe-7-kleine-textkorrekturen)**
- [ ] 4 Blog-Slugs umbenennen + 301 (Audit #4) → [phase-1_fix-blog-url-slugs-2026-09-20.md](phase-1_fix-blog-url-slugs-2026-09-20.md)

**Abschluss**
- [ ] Nach den Fixes neue Drift-Baseline setzen — die aktuelle Baseline enthält die leere Meta-Description als „Soll-Zustand"

**Dateien**
- [phase-1_audit-gesamt-2026-09-20.md](phase-1_audit-gesamt-2026-09-20.md) — Vollständiger Audit: Befunde, Synthese, Aktionsplan (15 Punkte), Schema-Code zum Einfügen
- [phase-1_fix-team-aufgaben-2026-09-20.md](phase-1_fix-team-aufgaben-2026-09-20.md) — 👥 **Schritt-für-Schritt-Anleitungen für alle Team-Aufgaben**, inkl. fertiger Texte zum Kopieren
- [phase-1_fix-blog-url-slugs-2026-09-20.md](phase-1_fix-blog-url-slugs-2026-09-20.md) — Schritt-für-Schritt-Anleitung Blog-URLs
- [phase-1_screenshots/](phase-1_screenshots/) — Desktop/Mobile-Screenshots von Startseite, Spenden, Projekte
- [phase-1_daten-crawl-2026-09-20.json](phase-1_daten-crawl-2026-09-20.json) — Crawl-Ergebnisse aller 28 URLs
- Lighthouse-Rohdaten: [Startseite](phase-1_daten-lighthouse-home-2026-09-20.json) · [Spenden](phase-1_daten-lighthouse-spenden-2026-09-20.json) · [Projekte](phase-1_daten-lighthouse-projekte-2026-09-20.json)
- Drift-Baseline: keine Datei — liegt in `~/.cache/claude-seo/drift/baselines.db` (Title „The Kinder Hilfe Kapstadt Trust", Description leer, 1 H1, 11 H2, 3 H3, 1 Schema-Block, 7 OG-Tags, Status 200)

---

## Phase 2 – Technische Lecks schliessen (Woche 1–2)

- [ ] `/seo technical https://www.kinderhilfekapstadt.com/`
- [ ] `/seo images https://www.kinderhilfekapstadt.com/`
- [ ] Fixes umsetzen (nur Critical/High)

Bereits bekannt aus Phase 0/1: Startseiten-Video >10 MB, reCAPTCHA 3× geladen (~1 MB), Weglot render-blockierend (3,8 s), Logo 78 KB bei `1500w`, vier HTML-Dokumente mit 1,2–1,7 MB.

**Dateien** — *noch keine* (erwartet: `phase-2_audit-technik-<datum>.md`, `phase-2_audit-bilder-<datum>.md`)

---

## Phase 3 – Bestehende Seiten hochziehen (Woche 2–3, bester ROI)

- [ ] `/seo google gsc` — Keywords/Seiten auf Position 5–20 mit Impressionen finden
- [ ] Pro schwacher Seite: `/seo page <url>` · `/seo content <url>` · `/seo sxo <url>`
- [ ] Fixes umsetzen

Kandidaten laut GSC (90 Tage): `/` Pos. 11,3 (222 Impr.) · `/ueberuns` Pos. 12,6 (171 Impr., 1 Klick) · `/projekte` Pos. 14,5 · `/geschichte` Pos. 8,0

**Dateien** — *noch keine* (erwartet: `phase-3_report-gsc-<datum>.md`, `phase-3_audit-seite-<slug>-<datum>.md`)

---

## Phase 4 – Content-Lücken skalieren (Woche 3–6)

- [ ] `/seo cluster <haupt-keyword>` — Hub-and-Spoke-Architektur + `cluster-map.html`
- [ ] 5–10 lohnendste Spokes auswählen
- [ ] Pro Spoke: `/seo content-brief <thema>`
- [ ] Nach dem Schreiben pro neuer Seite: `/seo schema <url>` · `/seo geo <url>`

**Dateien** — *noch keine* (erwartet: `phase-4_report-cluster-<datum>.md`, `phase-4_brief-<thema>.md`)

---

## Phase 5 – Autorität & Wettbewerb (ab Woche 6, laufend)

- [ ] `/seo backlinks https://www.kinderhilfekapstadt.com/` — Competitor-Gap = Outreach-Liste
- [ ] `/seo competitor-pages generate` — für eine Hilfsorganisation vermutlich nicht passend; vor dem Ausführen prüfen
- [ ] Entity-Fussabdruck: LinkedIn verlinken, Wikidata-Eintrag, betterplace/Verzeichnisse (Audit #14)

**Dateien** — *noch keine* (erwartet: `phase-5_report-backlinks-<datum>.md`)

---

## Phase 6 – Monitoring (monatlich, 10 Min)

Schleife: Drift prüfen → GSC ansehen → zurück zu Phase 3 mit den neuen 5–20er-Keywords.

- [ ] 2026-10 — `/seo drift compare` + `/seo google gsc`
- [ ] 2026-11 — `/seo drift compare` + `/seo google gsc`
- [ ] 2026-12 — `/seo drift compare` + `/seo google gsc`

**Dateien** — *noch keine* (erwartet: `phase-6_report-drift-<jjjj-mm>.md`, `phase-6_report-gsc-<jjjj-mm>.md`)

---

## Zusätzlich je nach Bedarf

- [ ] `/seo hreflang` — nur falls die Weglot-Entscheidung (Phase 1) auf eigene EN-URLs fällt
- `/seo local`, `/seo ecommerce`, `/seo programmatic`: für diese Website nicht relevant
