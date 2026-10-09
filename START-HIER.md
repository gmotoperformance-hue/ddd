# GMP_FINALE_v1.0 — Stand 9. Oktober 2026

Dieser Ordner ist der abgeschlossene Stand. Was hier liegt, läuft so auf
gmotoperformance.de — am 9.10.2026 Datei für Datei gegen den Server geprüft.

## Das Wichtigste in drei Sätzen

Die Werkstatt-App trägt die Cache-Marke **`20261009-stempel-109`**. Alle 15
Testreihen sind grün. Vier Dateien dürfen **nicht** aus diesem Ordner
hochgeladen werden — dazu unten mehr, das ist die einzige echte Stolperstelle.

## Was wo liegt

```
httpdocs/           der Bestand, wie er auf dem Server steht
tests/              15 Testreihen plus Sammelstarter
pruefsummen.js      Abgleich nach jedem Upload
CLAUDE.md           die Regeln dieses Projekts — wird automatisch gelesen
_nicht-verwendet/   vier Hero-Bilder (7,4 MB), auf die nichts verweist
                    und die auch nie auf dem Server lagen
```

## Vor jeder Änderung

```bash
node tests/alle.js
```

Muss „15 Reihen, 15 gruen" melden. Erst dann anfangen.

## Nach jeder Änderung

1. `node tests/alle.js` — wieder alles grün?
2. Bei Änderungen an `Werkstatt/app.js` oder `Werkstatt/styles.css`: die
   Cache-Marke in `httpdocs/Werkstatt/index.php` hochzählen. Sie steht an
   **zwei** Stellen. Beide oder keine — sonst lädt ein Teil der Besucher die
   alte Datei.
3. Hochladen über den Plesk-Dateimanager.
4. **Abgleichen.** Plesk meldet Erfolg auch dann, wenn nichts ankam. Am
   9.10.2026 ist genau das passiert: Die Sitzung war abgelaufen, der Upload
   meldete „No upload response", auf dem Server stand weiter die alte Datei.

```bash
node pruefsummen.js --browser
```

Das gibt einen Befehl aus, den man in der Konsole des geöffneten
Plesk-Dateimanagers ausführt. Er holt jede Datei zurück und vergleicht. Nur
was gleich ist, ist angekommen.

## Die vier Dateien, bei denen der Server recht hat

| Datei | Server | hier |
|---|---|---|
| `Werkstatt/paypal.php` | 10.579 B | 9.423 B |
| `Werkstatt/bootstrap.php` | 5.028 B | 5.932 B |
| `Werkstatt/paypal-lib.php` | 2.748 B | 2.985 B |
| `bezahlen/start.php` | 5.286 B | 5.142 B |

Die Live-Fassungen können mehr als die hier. `paypal.php` legt die
Positionsliste zum Zahllink ab — in `bootstrap.php` steckt umgekehrt eine
ausdrücklich **abgelehnte** Sicherheitsänderung, die nie live gehen soll.

**Wer eine dieser vier ändern will, holt sie zuerst von Hand aus Plesk** und
arbeitet auf dieser Fassung weiter. Sonst gehen Funktionen verloren, die seit
Monaten laufen.

Zurückholen ließen sie sich bisher nicht: Der Plesk-Download liefert den
Quelltext, aber der Sicherheitsfilter des Browserwerkzeugs hält ihn zurück,
weil er Zugangsdaten darin vermutet. Von Hand im Browser herunterladen geht.

## Was offen ist

1. **Rechnung `20261008_0011`, 502,94 €.** Noch nicht gebucht — das Geld war
   am 9.10. noch nicht da. Beim Buchen zieht die App die Anzahlung von
   180,02 € automatisch ab.
2. **Vespa LX50 ohne Kennzeichen und FIN.** Steht auf dem Bericht als „—".
   Entscheidung des Betreibers: kann so bleiben.

## Was am 9.10.2026 behoben wurde

**Die Bezahlseite war für jeden gültigen Zahllink kaputt** — sie rief `h()`
auf, ohne die Funktion zu haben, und brach mitten im Aufbau mit HTTP 500 ab.
Der Kunde sah einen leeren Kasten. Mit einem ungültigen Link fiel das nie auf.

**Die Positionsliste** wurde seit Monaten gespeichert und nie angezeigt. Steht
jetzt unter „Dafür" auf der Bezahlseite.

**Halbe Cents:** `totals()` summierte ungerundet, im Schnappschuss stand
682,955 bei gedruckten 682,96. Jede Positionszeile wird jetzt auf den Cent
gebracht. Festgeschriebene Rechnungen bleiben unberührt.

**Wettlauf beim Speichern:** Die Versionsprüfung lag vor dem Schreiben statt
darin — zwei Geräte konnten sich gegenseitig überschreiben. Die Bedingung
steht jetzt in der WHERE-Klausel.

**Vespa LX50:** Antriebsart „Variomatik" gesetzt. Die Inspektion zeigt statt
elf Antriebspunkten die sechs richtigen; Kettendurchhang und Kettenverschleiß
sind an einem Roller verschwunden.

**Wappen als Stempel** unter den Unterschriften. Auf der Website bleibt der
Bär — das Wappen gehört auf Belege und Stempel, nicht auf die Startseite.

## Grundsätzliches

Steht in `CLAUDE.md` und gilt unverändert: kein Build, kein npm, kein
Framework. Deployment ist Handarbeit über Plesk. Zugangsdaten gehören nach
`private/` außerhalb von `httpdocs`. Ölmengen, Ventilspiele und Drehmomente
werden nicht erfunden — sie stammen aus den Modelldaten oder vom Betreiber.
Und: Was in der App nicht gebucht ist, heißt „in der App ist nichts gebucht",
nicht „es wurde nicht gezahlt".
