# Blog-URLs mit Vorlagen-Slugs bereinigen

Stand: 2026-09-20 · Site: https://www.kinderhilfekapstadt.com (Squarespace)

## Erkenntnis

Vier echte Blogbeiträge laufen unter Squarespace-Vorlagen-URLs (`blog-post-title-…`).
Ursache: Die Beiträge wurden aus den Demo-Posts der Vorlage erstellt bzw. dupliziert –
Titel und Inhalt wurden ersetzt, das **URL-Kürzel (Slug)** blieb unverändert.

**Die Beiträge sind echter Inhalt und dürfen nicht gelöscht werden.** Nur die URLs sind das Problem.

| Beitrag | Datum | Aktuelle URL | Neue URL |
|---|---|---|---|
| Frohe Ostern! | 03.04.2026 | `/blog/Blog Post Title One-3zaa9-zlxng-64p6e` | `/blog/frohe-ostern-2026` |
| Brand im Township | 27.03.2026 | `/blog/blog-post-title-two-t5my5-k4xmd-652g9` | `/blog/brand-im-township` |
| Updates zu unseren Projekten | 03.03.2026 | `/blog/blog-post-title-three-y3peb-4lwnz-f593j` | `/blog/updates-projekte-maerz-2026` |
| Änderungen 2026 | 03.02.2026 | `/blog/blog-post-title-four-lr658-tcthp-5ef7p` | `/blog/aenderungen-2026` |

### Warum beheben

- Die URL erscheint im Google-Suchergebnis und in jedem geteilten Link (Rundbrief, WhatsApp, Social) – `blog-post-title-two-t5my5` wirkt unfertig und sagt nichts über den Inhalt.
- „Title One" enthält **Leerzeichen und Großbuchstaben** (`Blog%20Post%20Title%20One…`) – solche Links brechen beim Kopieren/Teilen leicht.
- Sprechende Slugs sind ein (schwaches) Relevanzsignal und verbessern die Klickrate.
- **Günstiger Zeitpunkt:** Die vier URLs haben zusammen ca. 3 Impressionen in 28 Tagen (GSC, 23.08.–17.09.2026), 0 Klicks. Es gibt praktisch kein Ranking, das verloren gehen könnte. Zwei der URLs sind bereits indexiert (`…title-three…`, `…Title One…`) – deshalb sind 301-Weiterleitungen Pflicht.

### Wichtig zu wissen

Squarespace legt beim Ändern eines Blog-Slugs **keine automatische Weiterleitung** an. Ohne Schritt 3 liefern alle alten Links einen 404.

---

## Schritt-für-Schritt-Anleitung

Zeitaufwand: ca. 15–20 Minuten. Reihenfolge einhalten.

### Schritt 1 – Alte URLs sichern

Die Tabelle oben enthält die exakten alten URLs. Zur Sicherheit vorher prüfen, dass sie noch stimmen:
https://www.kinderhilfekapstadt.com/sitemap.xml im Browser öffnen und die vier `blog-post-title`-Einträge vergleichen.

### Schritt 2 – Slugs in Squarespace ändern

Für jeden der vier Beiträge:

1. Squarespace-Backend → **Website → Seiten → Blog**.
2. Beitrag anklicken → **„…" → Einstellungen** (bzw. Beitrag bearbeiten → Zahnrad).
3. Reiter **Optionen** → Feld **„Beitrags-URL" / „URL-Kürzel"**.
4. Alten Slug durch den neuen aus der Tabelle ersetzen (nur Kleinbuchstaben, Bindestriche, keine Umlaute, keine Leerzeichen).
5. **Speichern**.

Bei der Gelegenheit im selben Dialog prüfen: Reiter **SEO** → SEO-Titel und SEO-Beschreibung ausgefüllt? Falls dort noch Vorlagentext steht, ersetzen.

### Schritt 3 – 301-Weiterleitungen anlegen

1. **Einstellungen → Entwickler-Tools → URL-Zuordnungen** (engl. *Settings → Developer Tools → URL Mappings*).
2. Folgende Zeilen einfügen (eine pro Zeile, Format `alt -> neu 301`):

```
/blog/blog-post-title-two-t5my5-k4xmd-652g9 -> /blog/brand-im-township 301
/blog/blog-post-title-three-y3peb-4lwnz-f593j -> /blog/updates-projekte-maerz-2026 301
/blog/blog-post-title-four-lr658-tcthp-5ef7p -> /blog/aenderungen-2026 301
/blog/Blog%20Post%20Title%20One-3zaa9-zlxng-64p6e -> /blog/frohe-ostern-2026 301
/blog/Blog Post Title One-3zaa9-zlxng-64p6e -> /blog/frohe-ostern-2026 301
```

3. **Speichern**.

Hinweis zu „Title One": Es ist nicht sicher, ob Squarespace das Leerzeichen als `%20` oder als echtes Leerzeichen abgleicht – deshalb beide Varianten. Meldet Squarespace bei einer der beiden Zeilen einen Formatfehler, diese Zeile weglassen und mit Schritt 4 testen, ob die andere greift.

Weiterleitungen greifen in Squarespace nur, wenn unter der alten URL **keine Seite mehr existiert** – deshalb Schritt 2 vor Schritt 3.

### Schritt 4 – Weiterleitungen testen

Im Terminal:

```bash
for u in \
  "blog-post-title-two-t5my5-k4xmd-652g9" \
  "blog-post-title-three-y3peb-4lwnz-f593j" \
  "blog-post-title-four-lr658-tcthp-5ef7p" \
  "Blog%20Post%20Title%20One-3zaa9-zlxng-64p6e"; do
  curl -s -o /dev/null -w "%{http_code} -> %{redirect_url}\n" \
    "https://www.kinderhilfekapstadt.com/blog/$u"
done
```

**Erwartet:** viermal `301 -> https://www.kinderhilfekapstadt.com/blog/<neuer-slug>`.
**Fehlgeschlagen**, wenn `404` (Zuordnung greift nicht → Schreibweise in Schritt 3 prüfen) oder `200` (Slug wurde in Schritt 2 nicht gespeichert).

Zusätzlich die vier neuen URLs im Browser öffnen – jede muss den richtigen Beitrag zeigen.

### Schritt 5 – Interne Links prüfen

Squarespace aktualisiert die Blog-Übersicht (`/blog`) automatisch. Manuell gesetzte Links prüfen:

- Startseite, `/projekte/*`-Seiten und andere Blogbeiträge auf Textlinks zu den vier Beiträgen durchsehen und auf die neue URL umstellen.
- Die 301 fängt vergessene Links ab – direkte Links sind trotzdem sauberer.

### Schritt 6 – Sitemap kontrollieren

https://www.kinderhilfekapstadt.com/sitemap.xml neu laden (Squarespace generiert automatisch, ggf. einige Minuten bis Stunden Verzögerung).

**Erwartet:** die vier neuen Slugs stehen drin, kein `blog-post-title` mehr.

### Schritt 7 – Sitemap in der Google Search Console einreichen

Erst jetzt, damit nicht die alten URLs eingereicht werden:

1. https://search.google.com/search-console → Property `https://www.kinderhilfekapstadt.com/`.
2. Links **Sitemaps** → bei „Neue Sitemap hinzufügen" `sitemap.xml` eintragen → **Senden**.
3. Nach 1–2 Tagen: Status „Erfolgreich", 27 erkannte URLs.

### Schritt 8 – Neue URLs zur Indexierung anmelden (optional, beschleunigt)

In der Search Console oben in die **URL-Prüfung** jede der vier neuen URLs eingeben → **Indexierung beantragen**.

---

## Erfolgskontrolle

| Wann | Prüfung | Erfolg | Fehlschlag |
|---|---|---|---|
| Sofort | `curl`-Test aus Schritt 4 | 4× `301` auf neue URL | `404` oder `200` |
| 1–2 Tage | GSC → Sitemaps | „Erfolgreich", 27 URLs | Fehler / 0 URLs |
| 2–4 Wochen | GSC → Leistung → Seiten (oder `/seo google gsc`) | neue Slugs mit Impressionen, alte verschwunden | alte URLs weiter mit Impressionen, neue fehlen → URL-Prüfung der neuen URL ansehen |

## Für die Zukunft

Neue Blogbeiträge nicht durch Duplizieren eines alten Posts anlegen – oder nach dem Duplizieren **sofort** das URL-Kürzel unter Einstellungen → Optionen anpassen, bevor der Beitrag veröffentlicht wird.
