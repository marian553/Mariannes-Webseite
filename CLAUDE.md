# CLAUDE.md

Kontext für Claude Code. Diese Datei wird beim Start automatisch gelesen.

---

## Was das hier ist

Landingpage für **Marianne Bauer**, Mentorin im Fichtelgebirge. Sie bietet eine
kostenlose „7-Tage-Entdeckungsreise" an — ein PDF-Journal, das Interessentinnen
gegen ihre E-Mail-Adresse bekommen. Die Seite hat genau ein Ziel: Eintragung ins
Formular.

Zielgruppe sind Frauen etwa ab 45, die im Alltag funktionieren, aber sich selbst
verloren haben. Der Ton ist warm, persönlich und ruhig — nie marktschreierisch,
keine künstliche Dringlichkeit, keine Rabatt-Rhetorik.

**Sprache: durchgehend Deutsch.** Texte, Kommentare im Code, Commit-Messages.

---

## Technischer Rahmen

Statische Website. **Kein Build-Schritt, kein Framework, kein npm.** Reines HTML,
CSS und etwas Vanilla-JavaScript. Das ist eine bewusste Entscheidung — die Seite
soll auch in drei Jahren noch ohne Toolchain wartbar sein.

```
index.html              Landingpage
danke.html              Dankeseite nach der Anmeldung (noindex)
impressum.html          Impressum nach § 5 DDG
datenschutz.html        Datenschutzerklärung inkl. KI-Abschnitte
favicon.svg
robots.txt · sitemap.xml
netlify.toml            Header, Caching, saubere URLs
images/                 alle Bilder
downloads/              das Freebie-PDF
```

CSS steht in jeder Datei inline im `<style>`-Block. Bitte **nicht** in externe
Dateien auslagern — bei dieser Seitengröße ist ein einziger Request schneller als
ein zusätzlicher Roundtrip. Die Design-Tokens stehen jeweils oben unter `:root`.

Hosting: **Netlify**, Deploy über GitHub-Push auf `main`.

---

## Design-System

Alle Farben als CSS-Variablen in `:root`. Bitte immer die Variablen verwenden, nie
Hex-Werte direkt in Regeln schreiben.

| Token | Wert | Verwendung |
|---|---|---|
| `--forest-dark` | `#1F3A18` | Überschriften, dunkle Panels |
| `--forest` | `#2C5924` | Links, Akzente |
| `--leaf` | `#4C8C3C` | Fokus-States, Hervorhebungen |
| `--leaf-light` | `#DCE9D4` | helle Flächen |
| `--gold` / `--gold-deep` | `#F2B705` / `#C98F02` | Buttons (Verlauf), Akzente |
| `--gold-soft` | `#FBEBB5` | Sektions-Hintergründe |
| `--cream` / `--cream-2` | `#F8F5EA` / `#F1ECDC` | Seitenhintergrund, Karten |
| `--ink` / `--ink-soft` | `#2A2A22` / `#5B5A4E` | Fließtext |

Schriften: **Fraunces** (Serif) für Überschriften, **Work Sans** für Fließtext.

Buttons sind ein Gold-Verlauf mit `border-radius: 999px`. Es gab vorher ein
grelles Neongrün — das ist bewusst entfernt worden, bitte nicht zurückholen.

Mobile Breakpoint: `max-width: 820px`.

---

## Regeln für Änderungen

**Bevor du Texte änderst:** Die Copy ist mit Marianne abgestimmt und mehrfach
überarbeitet worden. Formuliere nichts um, ohne dass es ausdrücklich gewünscht ist.

**Bevor du das Layout änderst:** Die Seite hat eine erzählerische Struktur
(Hero → Problem → Lösung → Nutzen → Über mich → Stimmen → FAQ → CTA). Diese
Reihenfolge ist Absicht.

**Nie ins HTML einbetten:** Bilder gehören als Dateien nach `/images`, nicht als
Base64 ins HTML. Die Seite war deswegen einmal 2,4 MB groß.

**Kein `localStorage`** ohne vorherige Einwilligung — siehe TDDDG.

**Barrierefreiheit:** Alt-Texte bleiben erhalten und beschreibend. Kontraste nach
WCAG AA. Interaktive Elemente per Tastatur bedienbar (die Flip-Cards reagieren
bereits auf Enter und Leertaste).

---

## Rechtlicher Rahmen — hier bitte besonders sorgfältig

Die Seite richtet sich an deutsche Nutzerinnen, es gelten DSGVO und TDDDG.

**Was in der Datenschutzerklärung steht, muss auch stimmen.** Wenn du einen Dienst
einbaust, trage ihn dort ein. Wenn ein Dienst nicht genutzt wird, lösche den
Abschnitt — eine Erklärung über nicht existierende Dienste ist nicht „sicherer",
sondern falsch.

**Das Impressum enthält bewusst keinen Hinweis auf die EU-Streitbeilegungsplattform.**
Die wurde zum 20.07.2025 abgeschaltet, der Hinweis ist seither zu löschen
(Verordnung (EU) 2024/3228). Bitte nicht aus alten Vorlagen wieder einfügen.

**Das Formular braucht zwingend:** Einwilligungs-Checkbox als Pflichtfeld mit Link
zur Datenschutzerklärung, Double-Opt-In, Abmeldemöglichkeit. Die Checkbox ist
vorhanden — bitte bei Umbauten nicht verlieren.

Die Rechtstexte sind Vorlagen und ersetzen keine Rechtsberatung. Bei Unsicherheit
lieber nachfragen als raten.

---

## Aktueller Stand des Formulars

Verdrahtet für **Netlify Forms**:

- `data-netlify="true"` — Netlify erkennt das Formular beim Deploy
- `netlify-honeypot="website"` — verstecktes Feld gegen Bots, per CSS ausgeblendet
- `action="/danke"` — Weiterleitung nach dem Absenden
- Checkbox `name="einwilligung"`, `required`

Das sammelt Einträge, **verschickt aber keine E-Mails und hat kein Double-Opt-In**.
Ein Newsletter-Dienst muss noch angebunden werden (siehe TODO.md Punkt 2).

`danke.html` wertet einen optionalen Query-Parameter `?name=` aus und begrüßt damit
persönlich. Der Wert wird vor der Ausgabe bereinigt (keine spitzen Klammern, max.
40 Zeichen) — diese Bereinigung bitte beibehalten.

---

## Offene Aufgaben

Stehen mit fertigen Prompts in **`TODO.md`**. Die Reihenfolge ist nach Dringlichkeit
sortiert, die ersten drei Punkte sind vor dem Livegang zwingend:

1. **Google Fonts lokal hosten** — werden aktuell von Googles Servern geladen, das
   ist ohne Einwilligung ein DSGVO-Verstoß und wird aktiv abgemahnt
2. **Newsletter-Dienst anbinden** — inklusive Double-Opt-In
3. **Platzhalter in den Rechtstexten füllen**, nicht genutzte Abschnitte löschen

---

## Arbeitsweise

Nach Änderungen an HTML oder CSS bitte kurz prüfen:

```bash
python3 -m http.server 8000     # lokal ansehen
```

- Sehen die Seiten auf 375px Breite noch gut aus?
- Funktionieren alle internen Links (Footer → Impressum → zurück)?
- Ist die Einwilligungs-Checkbox noch da und Pflichtfeld?

Commit-Messages auf Deutsch, knapp und im Imperativ:
`Schriften lokal einbinden statt von Google laden`
