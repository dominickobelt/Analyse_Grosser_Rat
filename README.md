# So hat der Grosse Rat Aargau gearbeitet

## Eine Analyse verschiedener Quellen, die die Arbeit des Grossen Rats dokumentieren
In meiner Arbeit möchte ich die Grossrätinnen und Grossräte genauer unter die Lupe nehmen. Das Resultat sollen verschiedene Statistiken sein, anhand derer die Leserinnen und Leser einen Überblick erhalten sollen, welche Parlamentarierinnen und Parlamentarier besonders fleissig, redselig, oder eher ruhig und zurückhaltend waren. Dazu gibt es zwei Quellen, die mit zwei Python-Scripts analysiert werden sollen, die Ergebnisse werden zu einer umfassenden Analyse verbunden.

Diese Arbeit ist im Zuge meiner Ausbildung CAS Datenjournalismus am Medienausbildungszentrum (MAZ) Luzern entstanden.

### Ausgangsthese
Ich gehe davon aus, dass sich die Zeit, die die Grossrätinnen und Grossräte am Rednerpult verbringen, stark unterscheiden. Einerseits hat das ganz offensichtliche Gründe: Der Grossratspräsident, der die Sitzungen leitet, oder auch vertreter von Kommissionen, werden mehr Redezeit beanspruchen. Trotzdem gibt es sicherlich solche Politikerinnen und Politiker, die ihre Meinung zu sehr vielen Geschäften kundtun möchten, und andere, die sich eher im Hintergrund halten. Diese möchte ich identifizieren, und in einem Nachzug auch zu den Gründen befragen.

### Quellen
Die Analyse besteht aus zwei Teilen:

#### 1. Audioanalyse
Der Grosser Rat Aargau verfügt über ein <a href="https://ag.recapp.ch/" target="_new">Audioarchiv</a>, das bis zum August 2019 zurückreicht. Darin sind alle Wortmeldungen inklusive Zeitangabe erfasst. Die Seite verfügt über eine API, die aber nicht dokumentiert ist. Das von mir erarbeitete Python-Script soll anhand der Datenbank ausrechenen, wie oft eine Person gesprochen hat, wie lange sie insgesamt gesprochen hat und wie lange eine durschnittliche Wortmeldung war.

In einem zweiten Schritt möchte ich die Redezeit aus den Fraktionen zusammenrechnen und so herausfinden, welche Partei am meisten und welche am wenigsten Redezeit für sich in Anspruch genommen hat.

#### 2. Analyse der Vorstösse
Auf der Webpage des Grossen Rats gibt es auch ein <a href="https://www.ag.ch/grossrat/grweb/de/196/Gesch%C3%A4fte?ResetBreadCrumbs=T&ResetFilter=T" target="_new">Verzeichnis aller Vorstösse</a>, dieses umfasst Motionen, Postulate, Interpellationen und parlamentarische Initiativen. Für alle Parlamentarierinnen und Parlamentarier gibt es auch eine persönliche Seite, auf der auch aufgeführt ist, an welchen Vorstössen er oder sie beteiligt war. 

Ich möchte für alle Grossrätinnen und Grossräte herausfinden, an wie vielen Vorstössen sie oder er beteiligt war. Damit die Analyse später wiederholt werden kann, muss der Zeitraum der Untersuchung wählbar sein. 

In einem weitern Schritt möchte ich herausfinden, welche Parteien besonders oft zusammenarbeiten. Also beispielsweise: Wie viele Vorstösse haben Politikerinnen und Politiker von der SVP als auch von der SP unterschrieben?

### Einschätzung von Aufwand/Ertrag vor Beginn des Projektes
Ich schätze den Aufwand des Projekts als relativ hoch ein, ich rechne damit, dass die 5 Tage, die als Richtwert angegeben sind, nicht reichen. Andererseits möchte ich die Arbeit auch so gestalten, dass sie einen längerfristigen Nutzen hat. So soll beispielsweise zur Hälft oder zum Ende einer Legislatur mit wenig Aufwand die Analyse wiederholt werden können, so dass eine regelmässige Berichterstattung möglich ist. Zudem möchte ich in Zukunft die Analyse noch erweitern, beispielsweise einfliessen lassen, welche Politikerinnen und Politiker wie oft gefehlt haben, oder wer zu einem bestimmten Thema besonders viel geredet hat.


### Knackpunkte des Projektes
Beim Erarbeiten des Projekts bin ich auf verschiedene Porbleme gestossen. Hier ein kurzer Überblick über die Knackpunkte, und wie ich sie gelöst habe:

#### Audioanalyse
- Da eine Dokumentation der API fehlt, musste ich zuerst herausfinden, wie die Variablen benannt sind, die ich benötige. In zwei Punkten musste ich externe Hilfe beiziehen:
    - Auf der Startseite sind alle Redner in einem Dorpdown-Menü. Allerdings konnte ich im Quelltext die entsprechenden ID's nicht finden, die die Seite dann übermittelt, um die Resultate zu einer Person aufzuführen. Unser Dozent Simon Schmid (Republik) konnte dieses Problem lösen und hat die Adresse im Reiter "Sources" entdeckt. Die Liste der Redner ist zu finden unter https://ag.recapp.ch/viewer/api/speaker. Dort wird für würd für jede Person die ID angegeben. 
    - Eine weiter Schwirigkeit war, zu den jeweiligen Personen das ganze Ergebnis abzurufen. Ich hatte schnell herausgefunden, dass ich mit "Limit=" steuern kann, wie viele Ergebnisse die Seite liefert. Allerdings funktionieren Zahlen über 50 nicht. Mir war es auch unmöglich herauszufinden, wie ich "weiterblättern", also auf die nächste Seite der Resultate gelangen kann. Also habe ich auf <a href="https://stackoverflow.com/questions/69826013/cant-get-full-result-of-a-html-query" target="_new">Stackoverflow</a> um Hilfe gebeten, und diese nach kurzer Zeit erhalten. Die Seite arbeitet mit "offset", damit kann man angeben, dass die Datenabfrage zum Beispiel ab Datensatz 50 starten soll. So ist mit einer Schleife die Abfrage aller Resultate möglich. Dann trat noch das Problem auf, dass das Scirpt stoppte, wenn ein Redner z.B. genau 100 Datensätze hatte, weil das Script immer nach 50 Datensätzen versucht hat, weitere Ergebnisse abzurufen. Das war aber relativ einfach zu beheben mit einer try/except-Anweisung.
- Da in der Analyse der Vorstösse nur Politikerinnen und Politiker mit einbezogen werden konnten, die momentan im Grossen Rat aktiv sind (wer zurück tritt wird aus dem Datensatz gelöscht), wollte ich bei der Audioanalyse ebenfalls nur diese Personen betrachten. Allerdings handelt es sich um zwei unterschiedliche Datensätze, in denen die Personen nicht immer gleich heissen (Mittelnamen). Hier musste ich "von Hand" einige Namen korrigieren, damit die Datensätze miteinander verbunden werden können. Somit kann auch künftig das Programm nicht voll automatisiert laufen, es braucht eine Betrachtung und allenfalls ein Eingreifen durch den Journalisten. Dies ist aber ein vertretbarer Aufwand.

#### Vorstösse
- Der grösste Knackpunkt war hier der Datensatz der Grossrätinnen und Grossräte. Wer im laufenden Jahr zurücktritt, der wird aus dem Datensatz, der auf der <a href="https://www.ag.ch/grossrat/grweb/de/164/Ratsmitglieder?ResetBreadCrumbs=T" target="_new">Website des Kantons</a> abrufbar ist, herausgelöscht. Eine Anfrage an den Kanton hat ergeben, dass dieser nicht den kompletten Datensatz herausrücken möchte. Mit einer Liste derjenigen Politikerinnen und Politiker, von denen wir die Daten beziehen möchten, wäre das allenfalls möglich. Aus Überlegungen zur Automatisierung habe ich darauf verzichtet und die Auswertung nur für die aktiven Grossrätinnen und Grossräte gemacht. 
- Weil ich den Leserinnen und Lesern in einer seperaten Liste die Politikerinnen und Politiker aus ihrem Bezirk präsentieren wollte, musste ich die Orte im Datensatz den jeweiligen Bezirken zuordnen. Ich habe dazu auf der jeweiligen Wikipedia-Seite zu jedem Bezirk die Tabelle mit den Ortsnahmen in ein Dataframe gespeichert. Einige Grossrätinnen und Grossräte waren aber noch Gemeinden zugeordnet, die in der Zwischenzeit fusioniert haben und die deshalb in den Tabellen nicht aufgetaucht sind. Diese habe ich dann einzeln angepasst. Zudem wurde bei einem Datensatz der Ort "Aarau" nicht dem Bezirk Aarau zugeornet, warum hat sich mir nicht erschlossen. Diesen Datensatz musste ich ebenfalls separat bearbeiten.

## Besprechung mit dem Team
Wir haben die Arbeit im Team besprochen. Die Idee, die Redezeit nicht nur für einzelne Politikerinnen und Politiker, sondern jeweils auch für die ganze Fraktion auszurechenen, stammt von unserem Potlikchef Fabian Hägler. Chefredaktor Rolf Cavalli hat Inputs zum Titel geliefert und er hat mich darauf aufmerksam gemacht, dass sich die Zwischentitel und die Titel der Grafiken zu oft wiederholen, weshalb ich dann auf Titel für die Grafiken verzichtet habe. Mit Storytelling-Expertin Alexandra Stark habe ich die Geschichte ebenfalls besprochen, von ihr stammt die Idee, die Politikerinnen und Poltiker pro Bezirk zu präsentieren, so dass die Leserinnen und Leser sehen können, wer in ihrem Bezirk wie aktiv war. Zudem haben wir Lead und Einstig besprochen, und dass wir einen Nachzug generieren sollten. Dies haben wir umgesetzt, indem wir <a href="https://www.aargauerzeitung.ch/aargau/kanton-aargau/datenanalyse-jetzt-reden-die-schweiger-aus-dem-grossen-rat-und-die-redseligen-erklaeren-sich-auch-ld.2237317" target="_new">die "schweigsamen" und die "redseligen" Grossrätinnen und Grossräte am nächsten Tag zum Ergebnis befragt haben.



_Datensatz (auch bereits strukturierte Daten können verwendet werden)
_Programmiercode
_Arbeitsprotokoll (Was hast du wann weshalb gemacht?)
_je nach gewählter Variante 1,2 oder 3: Endprodukt, Skizze des weiteren
Vorgehens oder Protokoll des Scheiterns
