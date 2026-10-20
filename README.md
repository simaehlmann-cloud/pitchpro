# PitchPro – Tore, Karten & Wechsel einfach protokollieren

Offline-App für Trainer:innen am Spielfeldrand. Aufgebaut wie **TacticBoard**: eine `index.html`,
Cordova-Builds über GitHub Actions für Google Play und den App Store, Deutsch/Englisch, dunkles Design.

## Funktionen

- **Mehrere Mannschaften** – jede als „Mein Team“ anlegen, oben auf der Startseite wechseln.
  Neues Spiel, Spielliste und Statistik beziehen sich auf die gewählte Mannschaft.
- **Teams & Kader** – Spieler einzeln, als eingefügte Liste (DFBnet, Excel, WhatsApp) oder per
  **„📥 Kader importieren“**: Kader-Datei (z. B. aus TacticBoard), CSV-Tabelle (Spalten wie Nr, Vorname,
  Nachname, Name, Position/TW werden erkannt; Trennzeichen `;` `,` oder Tab) oder Textliste.
- **Spieltag einfügen** – der geteilte Spieltag aus dem Kaderplaner (derselbe Text, den TacticBoard
  versteht) legt das Spiel direkt an: Team, Gegner, Heim/Auswärts, wer dabei ist, Torwart.
- **Spiel anlegen** – Format frei einstellbar (Vorlagen 5er bis 11er), Aufstellung per Tipp
  (Fehlt / Bank / Start), Torwart markieren.
- **Live** – Spieluhr (läuft über Zeitstempel, auch nach Neustart korrekt), Halbzeiten/Abschnitte,
  Nachspielzeit, Uhr-Korrektur. Spieler antippen → Tor, Elfmeter, Eigentor, Gelb, Gelb-Rot, Rot,
  Wechsel. Gegner: Tipp auf den Vereinsnamen genügt, mit Kader auch pro Spieler. Spieler nachtragen.
  Verlauf mit Löschen – Spielstand und Spielzeiten rechnen sich neu.
- **Schnellleiste** unter der Uhr: „⚽ Tor“ (Torschütze wählen oder später eintragen) und „↶ Rückgängig“.
- **Block-Wechsel** – mehrere Spieler auf einmal wechseln.
- **Feldansicht** – Spieler auf dem Platz als Aufstellung (Reihen aus den TacticBoard-Rollen, sonst nach Nummer).
- **Hell-Modus** für pralle Sonne (☀︎ im Live-Spiel oder unter ⚙︎).
- **Ereignisse bearbeiten** – Minute (direkt eintippen oder ±), Spieler, Elfmeter, Vorlage, Kartenart nachträglich ändern.
- **Spiel nachtragen** – vergangenes Spiel ohne Uhr: Datum, Aufstellung, dann Tore, Karten und Wechsel mit Minute.
  Abgeschlossene Spiele lassen sich im Bericht über „✎ Spiel bearbeiten“ ergänzen.
- **Vorlagen** (Option je Spiel) – nach jedem eigenen Tor kurze Abfrage, auch in der Statistik.
- **Notizen** pro Spiel mit Minuten-Stempel; im Bericht wählbar, ob sie mitgeteilt werden.
- **Verlängerung und Elfmeterschießen** bei Gleichstand nach Abpfiff (Bilanz zählt den Sieger i. E.).
- **Wechselerinnerung** – feste Intervalle *und* „Faire Spielzeit“ (Spielzeit je Spieler,
  konkrete Wechselvorschläge, Torwart ausgenommen). Fällige Wechsel: Hinweis, Vibration, Signalton.
  Optional **Wechselplan** vor dem Spiel (faire Rotation als Vorschlag, je Wechsel änderbar); die Erinnerung
  zeigt dann z. B. „Ben und Mia für Finn und Emma“ mit „Plan ausführen“ / „Überspringen“.
  Der Bildschirm bleibt an, solange die Uhr läuft (Plugin `cordova-plugin-insomnia`). Bei gesperrtem
  Display oder in einer anderen App kommt eine Benachrichtigung (`cordova-plugin-local-notification`;
  auf Android ohne Sonderrecht für exakte Wecker, daher im Energiesparmodus evtl. etwas verzögert).
- **Spielbericht** – Ergebnis, Halbzeitstand, Torfolge, Karten, Wechsel, Spielzeiten; als Text teilen
  oder **als Bild** (Ergebnis-Grafik 1080 px für WhatsApp & Co.).
- **Statistik** – über alle abgeschlossenen Spiele eines Teams: Bilanz (gesamt, Heim, Auswärts),
  pro Spieler Einsätze, Startelf, Minuten, Ø Minuten, Anteil an der Spielzeit, Tore (Elfmeter),
  Eigentore, Karten. Zeitraum Saison / Hinrunde / Rückrunde / Gesamt (Saison ab 1. Juli),
  sortierbar, als Text teilbar oder als CSV-Tabelle exportierbar. **Spielerprofil** (Spieler antippen: alle Spiele
  mit Minuten, Toren, Karten) und **Direktvergleich** (Bilanz gegen jeden Gegner, auch als Hinweis beim Anlegen).
- **Einführung** beim ersten Start (3 Schritte, mit Sprachwahl), erneut aufrufbar unter „Über PitchPro“.
- **Sprache** – Flaggen oben auf der Startseite (Deutsch/Englisch).
- **Über PitchPro** (ⓘ oben rechts) – Erklärung der App und Tipps.
- **Zurück-Taste (Android)** – schließt Fenster und geht zurück, beendet die App aber nie.
- **Sicherung** – alle Teams und Spiele als Datei exportieren/importieren (⚙︎). Ab 3 neuen Spielen und
  30 Tagen ohne Sicherung erinnert die Startseite daran.

## Eingebettet im U11-Kaderplaner

Liegt PitchPro im Unterordner `pitch/` des Kaderplaners und läuft dort in einem Rahmen (iframe),
tauschen beide Apps Nachrichten aus (`postMessage`, nur gleicher Ursprung). Eigenständig – Web,
Play Store, App Store – ist das ohne jede Wirkung.

| Nachricht | Richtung | Inhalt |
|---|---|---|
| `kp:start-match` | Kaderplaner → PitchPro | Spieltag-ID, Team, Gegner, Heim/Auswärts, Spieler auf dem Feld, Kader mit Kaderplaner-IDs |
| `kp:back` / `pp:back` | ↔ | Android-Zurück-Taste; Antwort `handled: true/false` |
| `kp:resync` | Kaderplaner → PitchPro | alle Ergebnisse erneut senden |
| `pp:ready` | PitchPro → Kaderplaner | Rahmen ist bereit |
| `pp:result` | PitchPro → Kaderplaner | Ergebnis und je Spieler (nur ID): Minuten, Startelf, Tore – auch nach späterem Bearbeiten |
| `pp:clock` | PitchPro → Kaderplaner | Uhr läuft → Bildschirm anlassen |
| `pp:reminder` | PitchPro → Kaderplaner | Zeitpunkt der nächsten Wechselerinnerung (oder `null`) für eine Benachrichtigung |
| `kp:caps` | Kaderplaner → PitchPro | `share: true`, wenn der Rahmen nativ teilen kann (Android-App) |
| `pp:share` | PitchPro → Kaderplaner | Spielbericht/Statistik als Text teilen |

Alle Nachrichten tragen `src` (`kaderplaner`/`pitchpro`) und `v: 1`. Das Ergebnis enthält keine
Namen, nur Kaderplaner-IDs. Ein zweites `kp:start-match` mit derselben Spieltag-ID öffnet das
vorhandene Spiel statt ein neues anzulegen.

## Zusammenspiel mit TacticBoard

| Richtung | Weg | Inhalt |
|---|---|---|
| TacticBoard → PitchPro | 👥 Kader → „📤 Kader exportieren“, in PitchPro *Teams & Kader → 📥 Kader importieren* | Namen, Rückennummern, Torwart (Rolle `tw`), „dabei“-Haken, Spielform |
| Kaderplaner → beide | Geteilten Spieltag einfügen | Team, Gegner, Heim/Auswärts, Kader, (TW) |
| PitchPro → TacticBoard | Team öffnen → „📤 Kader exportieren“, in TacticBoard „📥 Kader importieren“ | wie oben; Rollen (Abwehr/Mittelfeld/Sturm), Kapitän und Formation aus TacticBoard bleiben erhalten |

Beim Anpfiff setzt PitchPro die „dabei“-Haken auf die Spieler dieses Spiels – exportiert man danach,
stehen in TacticBoard genau diese Spieler zur Aufstellung bereit.

**Aufstellung vom Taktikbrett:** In TacticBoard (ab der Version mit „📲 Aufstellung an PitchPro“ im
Kader-Dialog) wird der Spieltag-Text mit `*Startelf (n)*` und `*Bank (n)*` geteilt. In PitchPro unter
„📋 Spieltag einfügen“ eingefügt, sind Start, Bank, Torwart und – aus dem Taktiknamen – der Gegner übernommen.

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
  TacticBoard nutzt dieselbe Fassung des Plugins.
- Weitere Plugins, nur in den Workflows eingebunden: `cordova-plugin-insomnia@4.3.0` (Bildschirm an),
  `cordova-plugin-local-notification@1.2.3` (Wechselerinnerung bei gesperrtem Display).

## Lokal ausprobieren

Einfach `index.html` über einen lokalen Webserver öffnen, z. B. `python3 -m http.server`
und dann `http://localhost:8000` im Handy-Browser oder in den Entwicklertools (Handy-Ansicht).
