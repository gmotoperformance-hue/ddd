# CLAUDE.md — GMP (gmotoperformance.de)

> **Hinweis zur Herkunft:** Diese Fassung ist aus `START-HIER.md` (Stand
> 9.10.2026) rekonstruiert. Die Original-`CLAUDE.md` des Projektordners
> `GMP_FINALE_v1.0` liegt hier nicht vor. Enthält sie mehr als das Folgende,
> **gilt sie** — dann diese Datei durch sie ersetzen. Hier steht nichts
> Erfundenes: jede Regel ist in `START-HIER.md` belegt.

## Grundsätzliches

- **Kein Build, kein npm, kein Framework.** Die Dateien, die im Repo liegen,
  sind die Dateien, die auf dem Server laufen. Keine Bundler, keine
  Transpiler, keine Paketverwaltung.
- **Deployment ist Handarbeit** über den Plesk-Dateimanager.
- **Zugangsdaten gehören nach `private/`**, außerhalb von `httpdocs`.
- **Technische Daten werden nicht erfunden.** Ölmengen, Ventilspiele und
  Drehmomente stammen aus den Modelldaten oder vom Betreiber — nie aus
  Schätzung, Analogie oder Erinnerung. Fehlt ein Wert, bleibt er offen.
- **Gebucht ≠ gezahlt.** Was in der App nicht gebucht ist, heißt „in der App
  ist nichts gebucht", nicht „es wurde nicht gezahlt". Diese Unterscheidung
  nie zusammenziehen.

## Ordnerstruktur

```
httpdocs/           der Bestand, wie er auf dem Server steht
tests/              15 Testreihen plus Sammelstarter (tests/alle.js)
pruefsummen.js      Abgleich nach jedem Upload
START-HIER.md       Stand, offene Punkte, Historie
_nicht-verwendet/   vier Hero-Bilder (7,4 MB), auf die nichts verweist
                    und die auch nie auf dem Server lagen
```

## Vor jeder Änderung

```bash
node tests/alle.js
```

Muss **„15 Reihen, 15 gruen"** melden. Erst dann anfangen. Meldet es das
nicht, ist das der erste Befund — nicht die eigentliche Aufgabe.

## Nach jeder Änderung

1. `node tests/alle.js` — wieder alles grün?
2. Bei Änderungen an `Werkstatt/app.js` oder `Werkstatt/styles.css`: die
   Cache-Marke in `httpdocs/Werkstatt/index.php` hochzählen. Sie steht an
   **zwei** Stellen. **Beide oder keine** — sonst lädt ein Teil der Besucher
   die alte Datei. (Stand 9.10.2026: `20261009-stempel-109`.)
3. Hochladen über den Plesk-Dateimanager.
4. **Abgleichen** — nicht optional:

```bash
node pruefsummen.js --browser
```

Der Befehl, den das ausgibt, läuft in der Konsole des geöffneten
Plesk-Dateimanagers. Er holt jede Datei zurück und vergleicht. **Nur was
gleich ist, ist angekommen.**

> **Plesk meldet Erfolg auch dann, wenn nichts ankam.** Am 9.10.2026 genau
> so passiert: Sitzung abgelaufen, Upload meldete „No upload response", auf
> dem Server stand weiter die alte Datei. Ein Upload ohne Abgleich ist kein
> Deployment.

## Die vier Dateien, bei denen der Server recht hat

Diese vier dürfen **nicht** aus dem Repo hochgeladen werden — die
Live-Fassungen können mehr:

| Datei | Server | hier |
|---|---|---|
| `Werkstatt/paypal.php` | 10.579 B | 9.423 B |
| `Werkstatt/bootstrap.php` | 5.028 B | 5.932 B |
| `Werkstatt/paypal-lib.php` | 2.748 B | 2.985 B |
| `bezahlen/start.php` | 5.286 B | 5.142 B |

`paypal.php` legt live die Positionsliste zum Zahllink ab. In
`bootstrap.php` steckt hier umgekehrt eine **ausdrücklich abgelehnte**
Sicherheitsänderung, die nie live gehen soll.

**Wer eine dieser vier ändern will, holt sie zuerst von Hand aus Plesk** und
arbeitet auf dieser Fassung weiter. Sonst gehen Funktionen verloren, die seit
Monaten laufen.

Zum Zurückholen: Der Plesk-Download liefert den Quelltext, aber der
Sicherheitsfilter des Browserwerkzeugs hält ihn zurück, weil er Zugangsdaten
darin vermutet. **Von Hand im Browser herunterladen geht** — diesen Weg
nehmen, statt es über das Werkzeug zu erzwingen.

## Arbeitsweise

- **Erst lesen, dann ändern.** Der Bestand in `httpdocs/` ist laufender
  Betrieb, kein Entwurf.
- **Nicht ausweiten.** Was gefragt war, wird geändert. Angrenzendes, das
  auffällt, wird genannt — nicht stillschweigend mitgeändert.
- **Kein Test wird weichgemacht.** Eine rote Reihe wird ursächlich behoben
  oder offen benannt, nie übersprungen oder angepasst, damit sie grün wird.
- **Festgeschriebene Rechnungen bleiben unberührt.** Korrekturen an der
  Rechenlogik gelten für Neues, nicht rückwirkend.
- **Vor „fertig" wird geprüft.** `node tests/alle.js` plus Abgleich, und
  berichtet wird, was wirklich herauskam — Fehlschläge eingeschlossen.
