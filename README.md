# PitchPro – Tore, Karten & Wechsel einfach protokollieren

Offline-App für Trainer:innen am Spielfeldrand. Aufgebaut wie **TacticBoard**: eine `index.html`,
Cordova-Builds über GitHub Actions für Google Play und den App Store, Deutsch/Englisch, dunkles Design.

## Funktionen

- **Teams & Kader** – Spieler einzeln, als eingefügte Liste (DFBnet, Excel, WhatsApp) oder als
  **Kader-Datei aus TacticBoard**.
- **Spieltag einfügen** – der geteilte Spieltag aus dem Kaderplaner (derselbe Text, den TacticBoard
  versteht) legt das Spiel direkt an: Team, Gegner, Heim/Auswärts, wer dabei ist, Torwart.
- **Spiel anlegen** – Format frei einstellbar (Vorlagen 5er bis 11er), Aufstellung per Tipp
  (Fehlt / Bank / Start), Torwart markieren.
- **Live** – Spieluhr (läuft über Zeitstempel, auch nach Neustart korrekt), Halbzeiten/Abschnitte,
  Nachspielzeit, Uhr-Korrektur. Spieler antippen → Tor, Elfmeter, Eigentor, Gelb, Gelb-Rot, Rot,
  Wechsel. Gegner: Tipp auf den Vereinsnamen genügt, mit Kader auch pro Spieler. Spieler nachtragen.
  Verlauf mit Löschen – Spielstand und Spielzeiten rechnen sich neu.
- **Wechselerinnerung** – feste Intervalle *und* „Faire Spielzeit“ (Spielzeit je Spieler,
  konkrete Wechselvorschläge, Torwart ausgenommen). Fällige Wechsel: Hinweis, Vibration, Signalton.
  Der Bildschirm bleibt an, solange die Uhr läuft (Wake Lock, wo das System es zulässt).
- **Spielbericht** – Ergebnis, Halbzeitstand, Torfolge, Karten, Wechsel, Spielzeiten; als Text teilen.
- **Sicherung** – alle Teams und Spiele als Datei exportieren/importieren (⚙︎).

## Zusammenspiel mit TacticBoard

| Richtung | Weg | Inhalt |
|---|---|---|
| TacticBoard → PitchPro | 👥 Kader → „📤 Kader exportieren“, in PitchPro *Teams & Kader → 📥 Kader aus TacticBoard* | Namen, Rückennummern, Torwart (Rolle `tw`), „dabei“-Haken, Spielform |
| Kaderplaner → beide | Geteilten Spieltag einfügen | Team, Gegner, Heim/Auswärts, Kader, (TW) |
| PitchPro → TacticBoard | Team öffnen → „📤 Kader für TacticBoard“, in TacticBoard „📥 Kader importieren“ | wie oben; Rollen (Abwehr/Mittelfeld/Sturm), Kapitän und Formation aus TacticBoard bleiben erhalten |

Beim Anpfiff setzt PitchPro die „dabei“-Haken auf die Spieler dieses Spiels – exportiert man danach,
stehen in TacticBoard genau diese Spieler zur Aufstellung bereit.

**Für später in TacticBoard (Vorschlag):** ein Knopf „Aufstellung teilen“, der den Spieltag-Text mit
den Abschnitten `*Startelf (n)*` und `*Bank (n)*` erzeugt. PitchPro versteht dieses Format bereits und
übernimmt dann Start und Bank direkt vom Taktikbrett.

## Builds

Wie bei TacticBoard – die Workflows sind übernommen und auf PitchPro angepasst
(App-ID `de.pitchpro.app`, keine Export-Bibliotheken nötig):

- `.github/workflows/android.yml` – jeder Push auf `main` baut; ohne Secrets eine Debug-APK,
  mit `KEYSTORE_BASE64`, `KEYSTORE_PASSWORD`, `KEY_ALIAS` (= `pitchpro`), `KEY_PASSWORD`
  ein signiertes AAB für die Play Console. **Eigenen Keystore für PitchPro anlegen.**
- `.github/workflows/ios.yml` – gleiche Secrets wie bei TacticBoard, aber ein **neues
  App-Store-Profil für `de.pitchpro.app`** (App-ID in Apple Developer anlegen). Upload nach
  App Store Connect nur per „Run workflow“.
- Version in `config.xml` vor jedem Store-Update erhöhen.
- Icons: `image.png`, `icon_fg.png`, `icon_bg.png`, `icon_store.png`, `icon_maskable.png`
  (gleicher Rasen wie TacticBoard); `tools/` erzeugt daraus alle Größen.
- Teilen-Plugin `plugins/cordova-plugin-tb-share` ist das TacticBoard-Plugin, ergänzt um
  `shareText` (Spielbericht direkt an WhatsApp & Co.) und Ordnernamen nach App-Name.
  Gleiche Änderung kann TacticBoard bei Gelegenheit übernehmen.

## Lokal ausprobieren

Einfach `index.html` über einen lokalen Webserver öffnen, z. B. `python3 -m http.server`
und dann `http://localhost:8000` im Handy-Browser oder in den Entwicklertools (Handy-Ansicht).
