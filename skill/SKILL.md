---
name: "lernpfad-sitzungen"
description: Erstellt zu einem Faktendokument einen geprüften Lernpfad und führt kurze Lernsitzungen (5 bis 10 Minuten) im Bildschirm- oder Sprachmodus, mit wörtlich vorgetragenem Ausschnitt, drei tolerant bewerteten Quizfragen, fortgeschriebener Lernprotokolldatei und Wiederholungssitzungen zu offenen Fragen, wörtlich, erklärend oder beides. Verwenden bei Lernpfad erstellen, Lernpfad, Lernsitzung, Sitzung n, weiter lernen, Quiz zum Faktendokument, Wiederholungssitzung.
metadata:
  version: '2.6.6'
---

# Lernpfad-Sitzungen

Einziger Fachinhalt ist das Faktendokument. Dieser Skill enthält nur Ablaufregeln und ein Prüfskript. Er arbeitet mit jedem Faktendokument, erstellt dazu auf Wunsch den Lernpfad (Fahrplan) und führt die Sitzungen.

## Welche Dokumente

1. Faktendokument, Lernpfad und Lernprotokoll in dieser Reihenfolge suchen: zuerst im Chat beigefügte Dateien, dann Dateien des Claude-Projekts, zuletzt das mitgelieferte Paar in `references/`.
2. Das mitgelieferte Paar:
   - `references/Faktendokument_Geologie_Suednorwegen.md`
   - `references/Lernpfad_Geologie_Suednorwegen.md` (30 Sitzungen)
3. Passen mehrere Dateien oder ist unklar, welches Dokument gemeint ist, einmal fragen.
4. Ein Lernpfad gehört genau zu der Fassung des Faktendokuments, aus der er erstellt wurde. Ist Code ausführbar, ein beigefügtes oder im Projekt liegendes Paar vor der ersten Sitzung mit `pruefen` (siehe unten) prüfen; besteht es nicht, das melden und nicht lernen.

## Unverrückbare Regeln

- Der Ausschnitt einer Sitzung wird wörtlich und vollständig vorgetragen, Satz für Satz, in der Reihenfolge des Dokuments. Nichts kürzen, nichts zusammenfassen, nichts umformulieren, nichts ergänzen.
- Bildblöcke gehören nicht zum Vortrag und nicht zum Lernstoff (siehe „Bildblöcke“).
- Keine Tatsache außerhalb des Faktendokuments. Kein Wissen von außen, keine Websuche.
- Antworten auf Quizfragen und Nachfragen sind wörtliche Stellen aus dem Faktendokument. Beantwortet das Dokument eine Nachfrage nicht, das offen sagen und die Frage ins Protokoll nehmen.
- Eine Zusammenfassung nur auf ausdrücklichen Wunsch, dann als solche benannt.
- Nur lesen, was die Sitzung braucht: den Ausschnitt und für Nachfragen die passende Stelle; in einer Wiederholung die Kernstellen und verwandten Stellen ihrer Themen.
- Deutsch, keine Gedankenstriche.
- Einzige Ausnahme sind die erklärenden Teile der Wiederholungssitzung, also der Modus „erklären“ und Einleitung und Abschluss im Modus „beides“; in Lernsitzungen und im Modus „wörtlich“ gelten diese Regeln ohne Ausnahme.

## Lernpfad erstellen

Auf Wunsch („Lernpfad erstellen“, „baue einen Lernpfad“) aus einem beigefügten oder im Claude-Projekt liegenden Faktendokument. Den Inhalt des Faktendokuments nie ändern. Voraussetzung ist Codeausführung; fehlt sie, das sagen und nicht von Hand bauen.

1. Format klären: Markdown, Kapitel als `## `, Abschnitte als `### `, Tabellen als Markdown-Tabellen. Liegt PDF oder Word vor, in Markdown umwandeln, den Wortlaut nicht ändern und die Markdown-Datei als Faktendokument mitliefern, denn der Lernpfad gilt nur für sie. Abbildungen stehen als Bildblöcke (siehe „Bildblöcke“). Kästen dürfen als Zitatblock stehen, ihr Titel fett; Zitatzeichen und Fettung sind Form und zählen nicht als Wörter.
2. Zuschnitt (Skript im Ordner dieses Skills): `python3 scripts/lernpfad.py zuschnitt FAKTEN.md Lernpfad_<Name>.md --titel "<Titel>"`. Das Skript bildet die Sitzungen (ganze Absätze, bis etwa 550 Wörter, keine Kapitelgrenze, Anker eindeutig), schreibt das Gerüst mit Platzhaltern OFFEN und je Sitzung den wörtlichen Ausschnitt nach `Lernpfad_<Name>_ausschnitte/`. Quellenverzeichnisse und Literaturlisten sind kein Lernstoff; weitere Kapitel mit `--ohne "<Kapitelüberschrift>"` ausnehmen, wenn der Nutzer es sagt.
3. Fragen: je Sitzung die Ausschnittdatei lesen und drei Fragen bilden. Jede Frage fragt nach einem Sachverhalt, nicht nach einer Quelle; die drei Fragen verteilen sich über den Ausschnitt. Die Fragen zielen auf Zusammenhänge und Ereignisse, also Ursachen, Abläufe, Folgen und Deutungen (warum, wodurch, was folgte daraus, wie wird es gedeutet); reine Zahlen-, Datums- oder Namensabfragen nur, wo der Ausschnitt nichts anderes hergibt. Enthält ein Kapitel einen Kasten mit Lernzielen, etwa „Learning Objectives“, leiten dessen Punkte die Fragen der Sitzungen dieses Kapitels an; die Antworten stammen weiterhin wörtlich aus dem jeweiligen Ausschnitt. Aus Bildblöcken wird keine Frage und keine Antwort gebildet. Eine Quelle darf in der Frage stehen, wenn sie nur abgrenzt, welche von mehreren Positionen gemeint ist. Die Antwort ist ein wörtlicher Auszug von 5 bis 30 Wörtern, aus der Ausschnittdatei kopiert, nie aus dem Gedächtnis. Keine Gedankenstriche in den Fragen. Die Platzhalter im Gerüst ersetzen und unter den Titel die Zeile „Version vN, Datum. Autor: …“ setzen, sonst am Gerüst nichts ändern.
4. Prüfen: `python3 scripts/lernpfad.py pruefen FAKTEN.md Lernpfad_<Name>.md`. Geprüft werden fortlaufende Nummern, eindeutige Anker, Umfang, lückenlose Abdeckung ohne Überschneidung und wörtliche Antworten sowie, dass jeder Bildblock in genau einer Sitzung liegt. Befunde beheben und erneut prüfen, bis BESTANDEN.
5. Übergeben: Faktendokument und Lernpfad als Paar, dazu die Messwerte der Prüfung. Einen Lernpfad, der nicht bestanden hat, nie übergeben. Die Ausschnittdateien sind Arbeitsdateien und werden nicht übergeben.

## Lernpfad (Fahrplan), wenn vorhanden

Der Lernpfad enthält keinen Fachtext. Je Sitzung stehen dort:

- `### Sitzung n: <Kapitelüberschrift>`
- `- Beginn:` ein Anker, entweder `Absatz, der beginnt mit „…“` oder `Tabellenzeile „…“` (erste Zelle der Zeile)
- `- Ende:` ein Anker, entweder `Absatz, der endet mit „…“` oder `Tabellenzeile „…“`
- `Quiz n:` eine Tabelle mit drei Fragen und wörtlichen Antworten

Den Ausschnitt so bestimmen: im genannten Kapitel des Faktendokuments vom Beginn-Anker bis einschließlich zum Ende-Anker, mit allen Absätzen, Zwischenüberschriften und Tabellenzeilen dazwischen. Anker sind innerhalb ihres Kapitels eindeutig. Findet sich ein Anker nicht, abbrechen und das melden; nicht raten.

## Bildblöcke

Ein Bildblock ist ein Absatz des Faktendokuments, dessen erste Zeile mit `![` beginnt oder `<!-- Bild -->` lautet. Im ersten Fall folgen dem Bild Legende und Bildnachweis, im zweiten steht ein Hinweis auf ein Bild, das nicht eingebunden ist. Bildverweise `![…](…)` in Tabellenzellen werden wie Bildblöcke behandelt.

- Bildblöcke sind kein Lernstoff: Sie zählen nicht zum Umfang, sind nie Anker und nie Quelle einer Antwort, auch nicht bei Nachfragen.
- Ein Bildblock gehört zum Ausschnitt der Sitzung, in deren Bereich er steht. Steht er unmittelbar nach dem Lernstoff einer Sitzung, gehört er noch zu dieser Sitzung. Steht zwischen ihm und dem vorangehenden Lernstoff eine Überschrift, oder steht er vor dem ersten Lernstoff des Kapitels, gehört er zur Sitzung, die mit dem nächsten Lernstoff beginnt.
- Bildpfade gelten relativ zum Ordner des Faktendokuments.

## Ohne Lernpfad

Ohne Lernpfad keine Sitzung. Liegt zum Faktendokument kein Lernpfad vor, keine Sitzung beginnen und sagen, dass zuerst mit „Lernpfad erstellen“ ein Lernpfad zu bauen ist, in einem Chat mit Codeausführung. Ist Code ausführbar, das gleich anbieten. Sitzungen nie im laufenden Chat selbst zuschneiden.

## Start

1. Dokument klären (siehe oben).
2. Sitzung klären: Nummer, oder „weiter“, dann die Sitzung nach der letzten erledigten Sitzung der Fortschrittstabelle; Wiederholungen zählen dabei nicht. Sonst einmal fragen.
3. Modus klären: „Bildschirm“ oder „Sprache“; sonst einmal fragen. Sagt der Nutzer, er fahre oder sei unterwegs, gilt „Sprache“. Umschalten ist jederzeit möglich; die Sitzung läuft an derselben Stelle weiter.

## Ablauf

1. Sitzung und Kapitel kurz ansagen.
2. Den Ausschnitt vortragen.
3. Drei Fragen nacheinander stellen und jeweils auf die Antwort warten.
4. Bewerten: richtig, teilweise richtig oder nicht richtig, mit Toleranz nach „Bewertung“. Danach die wörtliche Antwort aus dem Dokument nennen, als Ergänzung, nicht als Korrektur.
5. Nachfragen aus dem Faktendokument beantworten, mit wörtlicher Stelle.
6. Protokollzeile ausgeben und das Lernprotokoll fortschreiben (siehe unten).

## Bewertung

Maßgeblich ist, ob der Kern der Antwort stimmt, nicht ob sie vollständig oder wörtlich ist.

- Richtig: Der Kern stimmt. Ein Wert, der im genannten Zeitraum liegt oder ihn gut trifft, ist richtig, etwa „ungefähr 1,8 Milliarden Jahre“ für „zwischen 1,92 und 1,77 Ga“. Nennt das Dokument zu einer Größe zwei gleichwertige Angaben aus verschiedenen Quellen, genügt eine. Bei Aufzählungen genügt der tragende Teil. Andere Wörter für dieselbe Sache sind richtig.
- Teilweise richtig: Vom Kern fehlt ein wesentlicher Teil, oder nur ein Randdetail ist getroffen, oder ein ähnlicher Begriff für eine andere Sache steht an der Stelle, etwa „Rinne“ statt „Becken“. Ein passender Wert mit vertauschter Größenordnung, etwa Ma statt Ga, ist teilweise richtig; dann auf die Einheit hinweisen.
- Nicht richtig: Der Kern ist nicht getroffen, oder ein anderer Sachverhalt ist gemeint.
- Die Bewertung wird ohne Vorhaltung mitgeteilt: erst was stimmt, dann was fehlt.

## Bildschirmmodus

- Ausschnitt als Text genau wie im Dokument, mit Quellenangaben, Tabellen als Tabellen.
- Fragen einzeln; ein interaktives Quizelement darf verwendet werden, mit der wörtlichen Antwort als Erklärung.
- Bildblöcke an ihrer Stelle zeigen: das Bild, darunter Legende und Bildnachweis wörtlich. Liegt die Bilddatei nicht vor, Legende und Nachweis mit dem Satz „Bild nicht verfügbar“ zeigen. Ein Hinweis ohne Bild wird als Text gezeigt.
- Kästen als Kasten zeigen, mit ihrem Titel.

## Sprachmodus

Für das Hören, auch freihändig beim Autofahren. Im Sprachanruf lassen sich Dateien oft weder lesen noch schreiben. Deshalb vor dem Wechsel in den Anruf im Textchat den Ausschnitt der Sitzung, bei einer Wiederholung die Stellen ihrer Themen, und die Fragen aus dem Lernpfad ausgeben, die Fragen ohne die Antworten, damit sie im Anruf im Wortlaut vorliegen; das Lernprotokoll wird nach dem Anruf im Textchat fortgeschrieben. Liegt ein Ausschnitt im Anruf nicht vor, ihn nicht aus dem Gedächtnis vortragen, sondern darum bitten, kurz in den Textchat zu wechseln. Im Anruf wird der Ausschnitt immer vollständig vorgetragen, auch wenn er zuvor im Textchat stand.

Erlaubt ist nur die Umformung der Form, nie des Inhalts:

- Quellenangaben werden nicht gesprochen: Klammern mit Autor und Jahr weglassen. Ist eine Quelle Satzsubjekt („Bingen et al. (2021) gliedern …“), „die Autoren“ oder „eine Quelle“ sagen. Stehen zwei Quellen mit abweichenden Werten nebeneinander, „nach einer Quelle … nach einer anderen …“ sagen.
- Einheiten, Zeichen und Abkürzungen immer aussprechen, so wie das Faktendokument sie selbst erklärt; nicht erklärte Abkürzungen buchstabieren. Die folgenden Regeln sind Beispiele aus der Geologie und gelten, wo solche Angaben vorkommen.
- Altersangaben als Wörter: „1,92 Ga“ als „1,92 Milliarden Jahre“, „425 Ma“ als „425 Millionen Jahre“, „Mio. Jahre“ als „Millionen Jahre“, „ka“ als „tausend Jahre“, „cal yr BP“ als „Jahre vor 1950“, „cal ka BP“ als „tausend Jahre vor 1950“, „14C-Jahre BP“ als „Radiokarbonjahre vor 1950“.
- Nachkommastellen Ziffer für Ziffer sprechen, in der Reihenfolge ihres Auftretens: „2,43“ als „zwei Komma vier drei“, nicht „zwei Komma dreiundvierzig“.
- Zeichen und Einheiten aussprechen: „±“ plus minus, „km“ Kilometer, „km²“ Quadratkilometer, „km³“ Kubikkilometer, „°C“ Grad Celsius, „%“ Prozent, „Gew.-%“ Gewichtsprozent, „ca.“ etwa, „z. B.“ zum Beispiel.
- Tabellen Zeile für Zeile: je Zeile die Zellen mit ihrem Spaltennamen, soweit nötig.
- Zwischenüberschriften als kurze Ansage.
- Den Titel eines Kastens als kurze Ansage sprechen, dann seinen Text.
- Bildblöcke überspringen, ohne sie anzukündigen; Legenden, Bildnachweise und Hinweise nicht vorlesen.
- Das Ende des Ausschnitts ansagen („Ende des Ausschnitts“), bevor die erste Frage kommt.
- Keine Listen, Tabellen, Karten oder Formatierung anzeigen; nie zum Hinsehen auffordern.
- Fragen im Wortlaut des Lernpfads stellen; nur Quellennamen werden wie im Vortrag ersetzt („eine Quelle“). Bittet der Nutzer darum, eine Frage anders zu formulieren, ist das erlaubt; die neue Fassung ändert nur die Form und enthält keinen inhaltlichen Hinweis.
- Keine inhaltlichen Hinweise geben, auch nicht, um zu sagen, welche Quelle gemeint ist, und auch nicht, wenn der Nutzer um einen Tipp bittet; fehlt etwas, höchstens neutral nachfragen („noch etwas?“).
- Nach jeder Frage schweigen und warten; Rückmeldungen kurz.
- Zwischenstände im Anruf, etwa „zwei von drei“, sind vorläufig; verbindlich ist das Lernprotokoll.
- Nie in eine Antwort hineinreden: Pausen von ein, zwei Sekunden und Zögerlaute aushalten; endet ein Satz unvollständig, weiter zuhören, statt zu antworten oder „ich warte“ zu sagen. Ein Angebot wie „Magst du raten, oder soll ich auflösen?“ nur nach „weiß ich nicht“ oder nach längerer Stille.
- Befehle sofort befolgen: „Pause“ (warten bis „weiter“), „wiederholen“ (letzten Absatz oder letzte Frage), „langsamer“, „überspringen“ (nächste Frage), „Stopp“ (beenden und Protokollzeile ausgeben).
- Nicht drängen; der Straßenverkehr geht vor.

## Protokollzeile

Am Ende jeder Sitzung, in beiden Modi als Text im Chat:

| Sitzung | Datum | Ausschnitt | Offene Fragen | Zu wiederholen |
|---|---|---|---|---|

Eine Zeile ausfüllen: Nummer, Datum, Kapitel mit Beginn- und Ende-Anker, offene Fragen wörtlich, Nummern der nicht oder teilweise richtig beantworteten Fragen. Nach einer Wiederholung steht in der Spalte Sitzung „W n“, in der Spalte Ausschnitt die Themen, und in der letzten Spalte die Nummern, die offen bleiben. Im Sprachmodus nur sagen: „Die Protokollzeile steht im Chat.“

## Wiederholungssitzung

Eine Wiederholung behandelt noch einmal den Stoff der Fragen, die in der Liste „Zu wiederholen“ des Lernprotokolls offen sind. Sie ist keine Sitzung des Lernpfads: Sie verschiebt weder die Fortschrittstabelle noch „weiter“.

1. Auslöser: „Wiederholungssitzung“ oder „Wiederholung starten“. Nicht „wiederholen“ allein, das ist im Sprachmodus der Befehl für den letzten Absatz. Ohne Lernprotokoll mit offenen Einträgen keine Wiederholung.
2. Modus: „beides“ ist der Standard; „wörtlich“ oder „erklären“ gelten, wenn der Nutzer sie nennt. Bildschirm und Sprache gelten wie in der Sitzung.
3. Auswahl: die offenen Einträge nach Thema bündeln, also Fragen, deren Antworten an derselben Stelle stehen, zusammenfassen. Höchstens 4 Themen je Wiederholung, die ältesten zuerst; der Rest folgt in der nächsten.
4. Stellen: je Thema die Kernstelle, aus der die wörtliche Antwort stammt, und verwandte Stellen im ganzen Faktendokument suchen, auch im Glossar.
5. Ton: Stoff, keine Fehlerliste. Die Wiederholung wird als Rückblick auf die Themen angekündigt, nicht als Korrektur; frühere Antworten des Nutzers werden nicht zitiert.
6. Vortrag im Modus „wörtlich“: je Thema die Kernstelle mit dem Satz davor und danach, dann die verwandten Stellen, alles wörtlich nach den Regeln der Sitzung.
7. Vortrag im Modus „erklären“: nach „Erklärmodus“.
8. Vortrag im Modus „beides“: je Thema zuerst eine kurze einleitende Erklärung nach den Regeln des Erklärmodus, dann die Kernstelle wörtlich nach den Regeln der Sitzung, dann ein kurzer Abschluss mit Zusammenfassung, der aus dem Thema zurückführt.
9. Abfrage: die ursprünglichen Fragen der offenen Einträge aus dem Lernpfad, eine nach der anderen, bewertet nach „Bewertung“, danach die wörtliche Antwort.
10. Abschluss: Protokollzeile im Chat und Fortschreibung des Lernprotokolls nach „Lernprotokoll“.

Probeläufe zum Testen, auch mit angenommenen Antworten, werden nicht ins Lernprotokoll geschrieben.

Kein Ergebnis ohne Beleg: Bricht eine Sitzung, eine Wiederholung oder ein Anruf ab, bevor Antworten vorliegen, bleibt das Lernprotokoll unverändert; Antworten oder Bewertungen werden nie ergänzt oder angenommen. Nach „Stopp“ oder einem Abbruch mitten in der Abfrage werden nur die bis dahin gegebenen Antworten eingetragen, und die Sitzung bleibt offen.

### Erklärmodus

Nur in der Wiederholungssitzung: im Modus „erklären“ auf Wunsch („erklären“, „erkläre mir den Inhalt nochmal“) und in Einleitung und Abschluss des Modus „beides“. Ziel ist, dass der Stoff verstanden wird; dafür darf umschrieben werden.

- Die Erklärung wird als solche angekündigt, damit klar ist, dass jetzt nicht wörtlich vorgetragen wird.
- Der Inhalt des Faktendokuments wird in eigenen Worten beschrieben und umschrieben, mit Quellenangabe, wo eine Tatsache aus dem Dokument stammt; im Bildschirmmodus sind Tabellen oder kurze Gegenüberstellungen erlaubt, wenn sie ordnen.
- Allgemeines Wissen, das nicht im Faktendokument steht, ist erlaubt, wenn es dem Verständnis dient. Es wird im Bildschirmmodus mit „Hintergrund, nicht aus dem Faktendokument“ gekennzeichnet und im Sprachmodus mit „Zum Hintergrund, nicht aus dem Dokument:“ eingeleitet. Es widerspricht dem Faktendokument nie; bei Zweifel gilt das Dokument.
- Am Ende jedes Themas steht die Kernstelle wörtlich.

## Lernprotokoll

Zu jedem Faktendokument wird genau eine Protokolldatei geführt: `Lernprotokoll_<Name>.md`. Sie ist das Gedächtnis der Lernreise und wird am Ende jeder Sitzung und jeder Wiederholung fortgeschrieben, in beiden Modi, und dem Nutzer als Datei übergeben. Sie ist ein Arbeitsdokument, Zustände und Datum gehören ausdrücklich hinein.

Liegt eine Protokolldatei vor (beigefügt oder im Claude-Projekt, siehe „Welche Dokumente“), wird diese fortgeschrieben, nie eine zweite angelegt. Fehlt sie, vor der Neuanlage einmal fragen, ob es schon eine gibt, etwa aus einem anderen Chat; nur wenn nicht, wird sie beim ersten Sitzungsende neu angelegt.

Aufbau der Datei, in dieser Reihenfolge:

1. Kopf: Titel, Faktendokument mit Dateiname und Version, Lernpfad mit Dateiname und Version, Zahl der Sitzungen, Datum der letzten Fortschreibung. Ein Protokoll gehört genau zu diesem Paar; weicht die Version ab, das melden und nicht stillschweigend weiterschreiben.
2. Fortschritt: eine Tabelle mit allen Sitzungen des Lernpfads, je Zeile Sitzung, Kapitel, Zustand (erledigt oder offen), Datum und Treffer (etwa 2 von 3). Offene Sitzungen stehen von Anfang an mit Zustand offen darin, damit der Überblick vollständig ist.
3. Sitzungsblöcke: je erledigter Sitzung ein Abschnitt `### Sitzung n: <Kapitel>` mit Datum, Modus, Beginn- und Ende-Anker sowie einer Tabelle der drei Fragen mit Spalten Nr., Frage, Antwort des Nutzers (sinngemäß, kurz), Bewertung und wörtliche Antwort aus dem Faktendokument. Unter der Tabelle wird vermerkt, was vom Regelablauf abwich: eine auf Bitte umformulierte Frage, eine anders als im Lernpfad gestellte Frage, ein erbetener oder ein gegebener Hinweis. Fiel ein Hinweis, wird die Antwort trotzdem nach „Bewertung“ bewertet und der Hinweis neben der Bewertung genannt.
4. Wiederholungen: je Wiederholung ein Abschnitt `### Wiederholung n` mit Datum, Modus („beides“, „wörtlich“ oder „erklären“), Themen und einer Tabelle der gestellten Fragen mit Spalten Nr., Thema, Antwort des Nutzers (sinngemäß, kurz), Bewertung und Folge (erledigt oder bleibt offen). Fehlt der Abschnitt „Wiederholungen“, wird er bei der ersten Wiederholung vor „Offene Fragen“ angelegt.
5. Offene Fragen: eine Sammelliste aller Nachfragen, die das Faktendokument nicht beantwortet, je Zeile Sitzung, Frage wörtlich und Datum.
6. Zu wiederholen: eine Sammelliste der Stellen aus nicht oder teilweise richtig beantworteten Fragen, je Zeile Sitzung, Fragennummer, Stichwort und Zustand. Zustand ist „offen“, nach Wiederholungen „offen, n Wiederholungen“, oder „erledigt in Wiederholung n, Datum“, sobald die ursprüngliche Frage in einer Wiederholung richtig beantwortet ist. Ein Eintrag bleibt, bis er erledigt ist; er scheidet nicht nach einer Zahl von Wiederholungen aus. Fehlt einer älteren Datei die Spalte Zustand, wird sie ergänzt und mit „offen“ gefüllt.

Regeln der Fortschreibung: nur anfügen und den Zustand der betroffenen Sitzung oder des betroffenen Eintrags ändern, frühere Blöcke nie überschreiben. Wörtliche Antworten stammen aus dem Ausschnitt, nie aus dem Gedächtnis. Nach dem Schreiben prüfen und Messwerte nennen: Zahl der erledigten Sitzungen in der Fortschrittstabelle gleich Zahl der Sitzungsblöcke, Nummern lückenlos, keine doppelte Sitzung oder Wiederholung, Treffer je Sitzung gleich den Bewertungen ihres Blocks, Zustände in „Zu wiederholen“ im Einklang mit den Wiederholungsblöcken. Erfolg erst danach melden.

Neubewertung: Ändert der Nutzer die Bewertungsregel und wünscht, dass frühere Antworten danach neu bewertet werden, wird in der Bewertungszelle die neue Bewertung mit Datum und der vorherigen Bewertung vermerkt, im Kopf die Regel mit dem Datum der Neubewertung, und Treffer sowie „Zu wiederholen“ werden nachgezogen; ein Eintrag, der dadurch richtig wird, erhält den Zustand „erledigt durch Neubewertung, Datum“. Sonst bleiben frühere Blöcke unverändert.

Bei „weiter“ gibt die letzte erledigte Sitzung der Fortschrittstabelle die nächste vor; Wiederholungen zählen dabei nicht.
