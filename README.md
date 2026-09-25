# Landingpage Marianne Bauer

**Raus aus dem Kopf. Rein in deine Kraft.** — Landingpage für die kostenlose
7-Tage-Entdeckungsreise.

Statische Website ohne Build-Schritt: reines HTML, CSS und JavaScript.
Kein npm, kein Framework, kein Deployment-Prozess außer „Dateien hochladen".

---

## Struktur

```
.
├── index.html              Landingpage
├── danke.html              Dankeseite nach der Anmeldung (noindex)
├── impressum.html          Impressum nach § 5 DDG
├── datenschutz.html        Datenschutzerklärung inkl. KI-Abschnitte
├── favicon.svg
├── robots.txt
├── sitemap.xml
├── netlify.toml            Header, Caching, saubere URLs
├── images/                 alle Bilder
├── downloads/              das Freebie-PDF
└── TODO.md                 was vor dem Livegang noch fehlt
```

Das CSS steht in jeder Datei inline im `<style>`-Block. Bei einer Seite dieser Größe
ist das bewusst so: ein einziger Request, kein Render-Blocking, nichts zu verwalten.
Die Design-Tokens (Farben, Radien, Schatten) stehen jeweils ganz oben unter `:root`.

---

## Lokal ansehen

Die Dateien nutzen relative Pfade, ein Doppelklick auf `index.html` reicht.
Für ein realistischeres Bild — etwa um die Netlify-Redirects zu testen:

```bash
npx serve .
# oder
python3 -m http.server 8000
```

---

## Deployment auf Netlify

**Variante A — mit Git (empfohlen):**
Repository auf GitHub anlegen, in Netlify unter *Add new site → Import an existing
project* verbinden. Build command leer lassen, Publish directory auf `.` setzen.
Jeder Push auf `main` deployt automatisch.

**Variante B — Drag & Drop:**
Den gesamten Ordner auf https://app.netlify.com/drop ziehen.

Die `netlify.toml` wird automatisch erkannt. Sie setzt Sicherheits-Header, cacht
Bilder für ein Jahr und macht `/impressum`, `/datenschutz` und `/danke` ohne
`.html`-Endung erreichbar.

---

## Formular

Das Anmeldeformular in `index.html` ist aktuell für **Netlify Forms** verdrahtet:

- `data-netlify="true"` — Netlify erkennt das Formular beim Deploy automatisch
- `netlify-honeypot="website"` — verstecktes Feld gegen Spam-Bots
- `action="/danke"` — Weiterleitung nach dem Absenden
- Einwilligungs-Checkbox mit Pflichtfeld und Link zur Datenschutzerklärung

Einträge erscheinen im Netlify-Dashboard unter *Forms*. **Es werden keine E-Mails
verschickt und es gibt kein Double-Opt-In** — dafür muss ein Newsletter-Dienst
angebunden werden, siehe `TODO.md` Punkt 2.

`danke.html` wertet einen optionalen Query-Parameter `?name=` aus und begrüßt die
Person damit persönlich. Der Parameter wird vor der Ausgabe bereinigt.

---

## Vor dem Livegang

Siehe `TODO.md`. Die drei kritischen Punkte kurz zusammengefasst:

1. **Google Fonts lokal hosten** — aktuell werden sie von Googles Servern geladen,
   das ist ohne Einwilligung ein DSGVO-Verstoß und wird aktiv abgemahnt.
2. **Newsletter-Dienst anbinden** — inklusive Double-Opt-In.
3. **Platzhalter in den Rechtstexten füllen** und nicht genutzte Abschnitte löschen.

Die Rechtstexte sind sorgfältig erstellte Vorlagen, ersetzen aber keine
Rechtsberatung.
