# TODO – Landingpage Marianne Bauer

Stand: September 2026. Abgearbeitet wird von oben nach unten, die ersten drei Punkte
sind **vor dem Livegang zwingend**.

---

## 1. ~~Google Fonts lokal hosten~~ — erledigt (liegen unter `/fonts`)

**Problem:** `index.html`, `danke.html`, `impressum.html` und `datenschutz.html` laden
Fraunces und Work Sans von `fonts.googleapis.com`. Dabei geht die IP-Adresse jedes
Besuchers an Google in die USA — ohne Einwilligung ein DSGVO-Verstoß (LG München,
Az. 3 O 17493/20), der aktiv abgemahnt wird.

**Prompt für Claude Code:**
> Lade die Schriften Fraunces (Weights 500, 600, 700, 900 + Italic 500, 600) und
> Work Sans (400, 500, 600, 700) als WOFF2 herunter, lege sie unter `/fonts` ab und
> ersetze in allen vier HTML-Dateien die Google-Fonts-`<link>`-Tags durch lokale
> `@font-face`-Regeln mit `font-display: swap`. Entferne die `preconnect`-Tags zu
> Google. Prüfe danach, dass die Seite optisch unverändert aussieht.

---

## 2. Formular an den Newsletter-Dienst anbinden

**Aktueller Stand:** Das Formular ist für **Netlify Forms** vorbereitet
(`data-netlify="true"`, Honeypot gegen Bots, Weiterleitung auf `/danke`).
Das sammelt Einträge zwar zuverlässig, verschickt aber **keine E-Mails** und hat
**kein Double-Opt-In** — beides ist für einen Newsletter Pflicht.

**Zu entscheiden:** Welcher Dienst? Empfehlung für den deutschen Markt: Brevo,
KlickTipp oder CleverReach (alle mit Servern in der EU und AV-Vertrag).

**Prompt für Claude Code (Beispiel Brevo):**
> Binde das Anmeldeformular in `index.html` an Brevo an. Nutze den Double-Opt-In-Flow,
> setze als Weiterleitungsziel nach erfolgreicher Anmeldung `/danke` und übergib den
> Vornamen als Query-Parameter `?name=`, den `danke.html` bereits auswertet. Entferne
> die Netlify-Forms-Attribute, falls sie nicht mehr gebraucht werden. Der Honeypot und
> die Einwilligungs-Checkbox müssen erhalten bleiben.

**Wichtig:** Der Anbieter muss anschließend in `datenschutz.html`, Abschnitt 6
namentlich eingetragen werden.

---

## 3. Platzhalter in den Rechtstexten füllen

In `impressum.html` und `datenschutz.html` stehen Platzhalter in eckigen Klammern.

**In `impressum.html`:** erledigt (USt-ID, Berufsbezeichnung und Streitschlichtung
entfernt, da für Marianne nicht zutreffend).

**In `datenschutz.html`:**
- [ ] Abschnitt 3: Hosting-Anbieter → **Netlify, Inc., 512 2nd Street, Suite 200,
      San Francisco, CA 94107, USA** (Drittlandtransfer, Abschnitt 16 deckt das ab)
- [ ] Abschnitt 4: Consent-Tool (nur falls Tracking eingesetzt wird)
- [ ] Abschnitt 6: Newsletter-Anbieter aus Punkt 2
- [ ] Abschnitt 7.2: nicht genutzte KI-Anbieter streichen
- [ ] Abschnitt 20: Stand-Datum eintragen
- [ ] **Ganze Abschnitte löschen**, wenn der Dienst nicht eingesetzt wird:
      8 (Chatbot), 11 (Terminbuchung), 12 (Zahlungsanbieter), 13 (Mitgliederbereich),
      14 (Affiliate)

> Eine Datenschutzerklärung darf nur beschreiben, was tatsächlich läuft. Abschnitte
> über nicht genutzte Dienste sind kein „sicherer Puffer", sondern schlicht falsch.

**Nicht vergessen:** Die Texte sind sorgfältig erstellte Vorlagen, ersetzen aber keine
Rechtsberatung. Vor dem Livegang anwaltlich prüfen lassen.

---

## 4. Domain eintragen

Die echte Domain ersetzt `DEINE-DOMAIN.de` in:
- [ ] `index.html` → `<link rel="canonical">` und alle `og:`-Tags
- [ ] `robots.txt` → Sitemap-Zeile
- [ ] `sitemap.xml` → alle drei `<loc>`-Einträge

---

## 5. Bilder weiter optimieren (optional, empfohlen)

Die Bilder liegen jetzt als Dateien unter `/images` statt als Base64 im HTML —
das HTML ist dadurch von 2,4 MB auf 45 KB geschrumpft. Weitere Verbesserung:

**Prompt für Claude Code:**
> Konvertiere alle JPGs in `/images` zusätzlich nach WebP und AVIF und binde sie über
> `<picture>`-Elemente mit Fallback ein. Erzeuge für die großen Bilder außerdem
> `srcset`-Varianten in 800px, 1200px und 1600px Breite. `images/cta-marianne-park.jpg`
> (371 KB) und `images/hero-hintergrund.webp` (209 KB) haben die größte Wirkung.

Das Hero-Hintergrundbild sollte zusätzlich per `<link rel="preload">` geladen werden,
da es für den Largest Contentful Paint verantwortlich ist.

---

## 6. Abschluss-Check

- [ ] Lighthouse laufen lassen (Ziel: Performance > 90, Accessibility > 95)
- [ ] Auf echtem Handy durchklicken — besonders Flip-Cards und das Formular
- [ ] Alle Links testen: Footer → Impressum → zurück, Download-Button auf `/danke`
- [ ] Testanmeldung durchführen: Kommt die Bestätigungsmail? Funktioniert der Link?
- [ ] Abmelde-Link in der E-Mail testen

---

## Was bereits erledigt ist

- Bilder und PDF aus dem HTML ausgelagert (2,4 MB → 45 KB HTML)
- Dankeseite als eigene `danke.html` statt JavaScript-Overlay
- Einwilligungs-Checkbox mit Link zur Datenschutzerklärung im Formular
- Honeypot-Feld gegen Spam-Bots
- Meta-Description, Open Graph, Twitter Card, Favicon, `theme-color`
- `robots.txt` und `sitemap.xml`
- `netlify.toml` mit Sicherheits-Headern, Caching und sauberen URLs
- `loading="lazy"` und feste Bildmaße gegen Layout-Sprünge
- Impressum nach aktueller Rechtslage: kein Verweis mehr auf die zum 20.07.2025
  abgeschaltete EU-Streitbeilegungsplattform
- Schriften Fraunces und Work Sans lokal unter `/fonts` statt von Google
