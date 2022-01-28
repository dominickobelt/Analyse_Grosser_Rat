# So hat der Grosse Rat Aargau gearbeitet

## Eine Analyse verschiedener Quellen, die die Arbeit des Grossen Rats dokumentieren
In meiner Arbeit möchte ich die Grossrätinnen und Grossräte genauer unter die Lupe nehmen. Das Resultat sollen verschiedene Statistiken sein, anhand derer die Leserinnen und Leser einen Überblick erhalten sollen, welche Parlamentarierinnen und Parlamentarier besonders fleissig, redselig, oder eher ruhig und zurückhaltend waren. Dazu gibt es zwei Quellen, die mit zwei Python-Scripts analysiert werden sollen, die Ergebnisse werden zu einer umfassenden Analyse verbunden.

Diese Arbeit ist im Zuge meiner Ausbildung CAS Datenjournalismus am Medienausbildungszentrum (MAZ) Luzern entstanden.


### Ausgangsthese
Ich gehe davon aus, dass sich die Zeit, die die Grossrätinnen und Grossräte am Rednerpult verbringen, stark unterscheiden. Einerseits hat das ganz offensichtliche Gründe: Der Grossratspräsident, der die Sitzungen leitet, oder auch Vertreterinnen und Vertreter von Kommissionen, werden mehr Redezeit beanspruchen. Trotzdem gibt es sicherlich solche Politikerinnen und Politiker, die ihre Meinung zu sehr vielen Geschäften kundtun möchten, und andere, die sich eher im Hintergrund halten. Diese möchte ich identifizieren, und in einem Nachzug auch zu den Gründen befragen.

### Quellen
Die Analyse besteht aus zwei Teilen:

#### 1. Audioanalyse
Der Grosser Rat Aargau verfügt über ein <a href="https://ag.recapp.ch" target="_blank">Audioarchiv</a>, das bis zum August 2019 zurückreicht. Darin sind alle Wortmeldungen inklusive Zeitangabe erfasst. Die Seite verfügt über eine API, die aber nicht dokumentiert ist. Das von mir erarbeitete Python-Script soll anhand der Datenbank ausrechenen, wie oft eine Person gesprochen hat, wie lange sie insgesamt gesprochen hat und wie lange eine durschnittliche Wortmeldung war. Ich werde für diese Analyse als Zeitraum das letzte Jahr verwenden, dieser soll aber für künftige Projekte angepasst werden können.

In einem zweiten Schritt möchte ich die Redezeit aus den Fraktionen zusammenrechnen und so herausfinden, welche Partei am meisten und welche am wenigsten Redezeit für sich in Anspruch genommen hat.

#### 2. Analyse der Vorstösse
Auf der Webpage des Grossen Rats gibt es auch ein <a href="https://www.ag.ch/grossrat/grweb/de/196/Gesch%C3%A4fte?ResetBreadCrumbs=T&ResetFilter=T" target="_blank">Verzeichnis aller Vorstösse</a>, dieses umfasst Motionen, Postulate, Interpellationen und parlamentarische Initiativen. Für alle Parlamentarierinnen und Parlamentarier gibt es auch eine persönliche Seite, auf der auch aufgeführt ist, an welchen Vorstössen er oder sie beteiligt war. 

Ich möchte für alle Grossrätinnen und Grossräte herausfinden, an wie vielen Vorstössen sie oder er beteiligt war. Damit die Analyse später wiederholt werden kann, muss der Zeitraum der Untersuchung wählbar sein. 

In einem weitern Schritt möchte ich herausfinden, welche Parteien besonders oft zusammenarbeiten. Also beispielsweise: Wie viele Vorstösse haben Politikerinnen und Politiker von der SVP als auch von der SP unterschrieben?

### Einschätzung von Aufwand/Ertrag vor Beginn des Projektes
Ich schätze den Aufwand des Projekts als relativ hoch ein, ich rechne damit, dass die 5 Tage, die als Richtwert angegeben sind, nicht reichen. Andererseits möchte ich die Arbeit auch so gestalten, dass sie einen längerfristigen Nutzen hat. So soll beispielsweise zur Hälft oder zum Ende einer Legislatur mit wenig Aufwand die Analyse wiederholt werden können, so dass eine regelmässige Berichterstattung möglich ist. Zudem möchte ich in Zukunft die Analyse noch erweitern, beispielsweise einfliessen lassen, welche Politikerinnen und Politiker wie oft gefehlt haben, oder wer zu einem bestimmten Thema besonders viel geredet hat.


### Knackpunkte des Projektes
Beim Erarbeiten des Projekts bin ich auf verschiedene Porbleme gestossen. Hier ein kurzer Überblick über die Knackpunkte, und wie ich sie gelöst habe:

#### Audioanalyse
- Da eine Dokumentation der API fehlt, musste ich zuerst herausfinden, wie die Variablen benannt sind, die ich benötige. In zwei Punkten musste ich externe Hilfe beiziehen:
    - Auf der Startseite sind alle Redner in einem Dorpdown-Menü. Allerdings konnte ich im Quelltext die entsprechenden ID's nicht finden, die die Seite dann übermittelt, um die Resultate zu einer Person aufzuführen. Unser Dozent Simon Schmid (Republik) konnte dieses Problem lösen und hat die Adresse im Reiter "Sources" entdeckt. Die Liste der Redner ist zu finden unter https://ag.recapp.ch/viewer/api/speaker. Dort wird für würd für jede Person die ID angegeben. 
    - Eine weiter Schwirigkeit war, zu den jeweiligen Personen das ganze Ergebnis abzurufen. Ich hatte schnell herausgefunden, dass ich mit "Limit=" steuern kann, wie viele Ergebnisse die Seite liefert. Allerdings funktionieren Zahlen über 50 nicht. Mir war es auch unmöglich herauszufinden, wie ich "weiterblättern", also auf die nächste Seite der Resultate gelangen kann. Also habe ich auf <a href="https://stackoverflow.com/questions/69826013/cant-get-full-result-of-a-html-query" target="_blank">Stackoverflow</a> um Hilfe gebeten, und diese nach kurzer Zeit erhalten. Die Seite arbeitet mit "offset", damit kann man angeben, dass die Datenabfrage zum Beispiel ab Datensatz 50 starten soll. So ist mit einer Schleife die Abfrage aller Resultate möglich. Dann trat noch das Problem auf, dass das Scirpt stoppte, wenn ein Redner z.B. genau 100 Datensätze hatte, weil das Script immer nach 50 Datensätzen versucht hat, weitere Ergebnisse abzurufen. Das war aber relativ einfach zu beheben mit einer try/except-Anweisung.
- Da in der Analyse der Vorstösse nur Politikerinnen und Politiker mit einbezogen werden konnten, die momentan im Grossen Rat aktiv sind (wer zurück tritt wird aus dem Datensatz gelöscht), wollte ich bei der Audioanalyse ebenfalls nur diese Personen betrachten. Allerdings handelt es sich um zwei unterschiedliche Datensätze, in denen die Personen nicht immer gleich heissen (Mittelnamen). Hier musste ich "von Hand" einige Namen korrigieren, damit die Datensätze miteinander verbunden werden konnten. Somit kann auch künftig das Programm nicht voll automatisiert laufen, es braucht eine Betrachtung und allenfalls ein Eingreifen durch den Journalisten. Dies ist aber ein vertretbarer Aufwand.

#### Vorstösse
- Der grösste Knackpunkt war hier der Datensatz der Grossrätinnen und Grossräte. Wer im laufenden Jahr zurücktritt, der wird aus dem Datensatz, der auf der <a href="https://www.ag.ch/grossrat/grweb/de/164/Ratsmitglieder?ResetBreadCrumbs=T" target="_blank">Website des Kantons</a> abrufbar ist, herausgelöscht. Eine Anfrage an den Kanton hat ergeben, dass dieser nicht den kompletten Datensatz herausrücken möchte. Mit einer Liste derjenigen Politikerinnen und Politiker, von denen wir die Daten beziehen möchten, wäre das allenfalls möglich. Aus Überlegungen zur Automatisierung habe ich darauf verzichtet und die Auswertung nur für die aktiven Grossrätinnen und Grossräte gemacht. 
- Weil ich den Leserinnen und Lesern in einer seperaten Liste die Politikerinnen und Politiker aus ihrem Bezirk präsentieren wollte, musste ich die Orte im Datensatz den jeweiligen Bezirken zuordnen. Ich habe dazu auf der jeweiligen Wikipedia-Seite zu jedem Bezirk die Tabelle mit den Ortsnahmen in ein Dataframe gespeichert. Einige Grossrätinnen und Grossräte waren aber noch Gemeinden zugeordnet, die in der Zwischenzeit fusioniert haben und die deshalb in den Tabellen nicht aufgetaucht sind. Diese habe ich dann einzeln angepasst. Zudem wurde bei einem Datensatz der Ort "Aarau" nicht dem Bezirk Aarau zugeornet, warum hat sich mir nicht erschlossen. Diesen Datensatz musste ich ebenfalls separat bearbeiten.

## Besprechung mit dem Team
Wir haben die Arbeit im Team besprochen. Die Idee, die Redezeit nicht nur für einzelne Politikerinnen und Politiker, sondern jeweils auch für die ganze Fraktion auszurechenen, stammt von unserem Politikchef Fabian Hägler. Chefredaktor Rolf Cavalli hat Inputs zum Titel geliefert und er hat mich darauf aufmerksam gemacht, dass sich die Zwischentitel und die Titel der Grafiken zu oft wiederholen, weshalb ich dann auf Titel für die Grafiken verzichtet habe. Mit Storytelling-Expertin Alexandra Stark habe ich die Geschichte ebenfalls besprochen, von ihr stammt die Idee, die Politikerinnen und Poltiker pro Bezirk zu präsentieren, so dass die Leserinnen und Leser sehen können, wer in ihrem Bezirk wie aktiv war. Zudem haben wir Lead und Einstig besprochen, und dass wir einen Nachzug generieren sollten. Dies haben wir umgesetzt, indem wir <a href="https://www.aargauerzeitung.ch/aargau/kanton-aargau/datenanalyse-jetzt-reden-die-schweiger-aus-dem-grossen-rat-und-die-redseligen-erklaeren-sich-auch-ld.2237317" target="_blank">die "schweigsamen" und die "redseligen" Grossrätinnen und Grossräte am nächsten Tag zum Ergebnis befragt haben.</a>

## Datensatz
Meinen Datensatz habe ich mir aus verschiedenen Quellen geholt:
- Die Liste aller Grossräte von der [Website des Kantons Aargau](https://www.ag.ch/grossrat/grweb/de/164/Ratsmitglieder?ResetBreadCrumbs=T)
- Die Liste aller Motionen, Postulate, Interpellationen und parlamentarische Initiativen, jeweils einzeln als csv  [von der Website des Kantons](https://www.ag.ch/grossrat/grweb/de/196/Gesch%C3%A4fte?ResetBreadCrumbs=T&ResetFilter=T)
- Sämtliche Angaben zu den Redezeiten aus dem [Audioarchiv](https://ag.recapp.ch/viewer/)
- Die Liste aller Gemeinden des jeweiligen Bezirks, z.B. Baden, von [Wikipedia](https://de.wikipedia.org/wiki/Bezirk_Baden_(Aargau))

## Programmcode
Der Code des Scripts ist auf dieser Seite abgelegt, er umfasst die beiden Files <b>Vorstösse</b> und <b>Audiofiles analysieren</b>.

## Arbeitsprotokoll 

### 19. Oktober
- Erste Abfrage des Audioarchivs, Ergebnis speichern. Klappt, wenn ich die ID in die URL schreibe, die ich mitgebe. Problem: Wie komme ich an alle ID's, die zur Verfüng stehen? Alle durchzuprobieren scheint nicht praktikabel, zu grosse Range.
- An den folgenden Tagen mehrmals an dem ID-Problem gearbeitet, ohne Ergebnis.

### 29. Oktober
- Habe mich mit dem Problem an Simon Schmid gewandt, er hat mir gezeigt, wie man die Seite untersucht und unter Sources folgende Adresse findet: https://ag.recapp.ch/viewer/api/speaker

### 3. November
- Liste aller ID's abrufen.
- Anhand der ID alle Wortmeldungen eines Politikers abrufen.
- die ersten 50 klappen, kann aber den Rest nicht abrufen (kann nicht auf die nächste Seite springen).
- Frage auf [Stackoverflow](https://stackoverflow.com/questions/69826013/cant-get-full-result-of-a-html-query) gestellt und umgehend eine Antwort erhalten, mit offset ist die Abfrage möglich.
- Das Zusammenrechnen der Zeiten/Anzahl Voten in eine Funktion gepackt.
- Script so umgeschrieben, dass ein Zeitraum angegeben werden kann.

### 23. November
- Schleife mit Hilfe von offset gebaut.
- Problem: Das Programm stoppt, wenn ein Datensatz genau durch 50 teilbar ist, weil das Script eine weitere Seite abrufen möcht, diese aber nicht vorhanden ist. Nach einigem Probieren Problem gelöst mit try/except.
- Zeiten und Anzahl Wortmeldungen zusammenzählen.
- Alle Grossrätinnen und Grossräte speichern, indem die [Webseiten mit dem Grossratsverzeichnis](https://www.ag.ch/grossrat/grweb/de/164/Ratsmitglieder?) gespeichert werden. Es wäre auch möglich, die Liste als csv herunterzuladen, aber ich brauche die ID's, resp. die Links auf die einzelnen Seiten der Politiker, um zu sehen, wie viele Vorstösse sie gemacht haben.
- Versucht, beide Datensets miteinander zu verbinden, stimmt nicht zu 100% überein (müssten 140 Grossrätinnen und Grossräte sein).

### 24. November
- Für die Analyse der Anzahl Vorstösse die Seiten aller Poltiker aufrufen, und die Seiten mit all Ihren Vorstössen abspeichern. Liste erstellt mit allen Politiker als Minidict (Name,Partei, Ort, Link).
- Wieviele Vorstösse muss ich maximal erfassen? Script ist ausgelegt bis 150, ansonsten erscheint eine Meldung.


### 2. Dezember
- Bezirk soll dem Datenset mit den Politikerinnen und Politiker hinzugefügt werden, anhand des Orts.
- Liste der Gemeinden des Bezirks Bremgarten aus Wikipedia geladen und in ein Datenset gespeichert.
- Adresse aller Bezirksseiten auf Wikipedia sind gleich aufgebaut (ausser Baden), deshalb ein Funktion programmiert, die ein Dict mit allen Bezirken und darin die Listen der Gemeinden erhält. Wenn man der Funktion einen Gemeindenamen übergibt, liefert sie den Bezirk zurück.


### 4. Dezember
- Herausgefunden, wie ich die Datensets herausfiltere, denen noch kein Bezirk zugeordnet wurde.
- Grösstenteils liegt es daran, dass Gemeinden eingetragen sind, die es aufgrund von Fusionen gar nicht mehr gibt. Script ändert jetzt die Namen dieser Gemeinden in den Namen der fusionierten Gemeinde.
- Nicht herausgefunden, warum bei Leila Hunziker, Aarau, (id 63) der Bezirk nicht zugeordnet wird. Script ordnet diesem Datensatz den Bezirk jetzt separat zu (etwas unbefriedigende Lösung, hätte lieber den Grund für den Fehler gewusst, habe aber nach einiger Zeit die Recherche abgebrochen, da meine Lösung ja funktioniert).

### 10. Dezember
- Grossrätinnen und Grossräte, die in den beiden Datensets nicht gleich beschriftet sind, herausgefiltert und Namen in einem Datenset angepsst.
- Vier Personen sind im Datensatz mit den Redezeiten gar nicht vorhanden. Diese haben noch nie etwas gesagt. Deshalb setze ich die Anazhl Statements und Redezeit auf 0.

### 21. Dezember
- Problem: Auch Fraktionsvorstösse sollen berücksichtigt werden, diese erscheinen nicht auf der Seite der Politiker. Lade zuerst das CSV mit den Postulaten herunter und speichere es als Datenset.
- Bereinigung des Datensetzs (komische Sonderzeichen herausgefiltert).
- Funktion geschrieben, die anhand der Vorstossnummer (z.B. 21.110) das Jahr herausgibt (2021).
- Wenn dem Script eine Zeitspanne mitgegeben werden soll, muss ich das Jahr von 21 auf 2021 umrechnen, sonst ist z.B. das Jahr 98 höher als 21. Entsprechende Funktion geschrieben.
- Funktion geschrieben, die nach Persönlichen- und Fraktionsvorstössen aufteilt.

### 24. Dezember
- Auch dem Script zur Analyse von Vorstössen kann jetzt eine Zeitspanne mitgegeben werden.
- Nebst Postulaten auch Motionen, Interpellationen und parlamentarische Initiativen mit einbezogen.

### 28. Dezember
- Funktion geschrieben, um herauszufinden, ob zwei Parteien an einem Vorstoss beteiligt waren. Man übergibt der Funktion beispielsweise FDP und SP und den Titel eines Vorstosses, und sie gibt wahr oder falsch zurück.
- Alle Parteien in eine Liste geschrieben und für alle Kombinationen ausgerechnet, wie viele gemeinsame Vorstösse dass es gibt.


### 4. Januar
- Separates CSV für jeden Bezirk abspeichern.
- Entdeckt, dass Dominik Gersch im Datensatz mit den Rednern 2 Mal vorhanden ist. Im Audioarchiv überprüft, scheint ein Fehler im Archiv selber zu sein. Redezeiten zusammengezählt.
- Alle Berechnungen nochmals durchlaufen lassen, damit das Ergebnis für 2021 aktuell ist.
- Fehler entdeckt. Die Redezeit von der Sitzung vom 5. Januar wird nicht erfasst. Offenbar ist das Datum im Datensatz der 1. Januar und nicht der 5. Januar. Weil in meiner Berechnung < und nicht <= steht, wurde die Sitzung nicht berücksichtigt, ist jetzt angepasst. 
- Onlineartikel angelegt und mit dem Schreiben begonnen. Besprechung der Resultate mit Fabian Hägler.

### 5. Januar
- Rückmeldung von Ressortleiter Fabian Hägler: Herausfinden, wie viel Redezeit die Fraktionen insgesamt beansprucht haben. 
- Die beiden Politker der EDU sind in der Fraktion der SVP. Musste noch eine Spalte Fraktion hinzufügen, damit man zwischen Partei und Fraktion unterscheiden kann (in allen anderen Fällen ist da im Grossen Rat Aargau momentan dasselbe, aber künftig könnte sich das auch ändern).
- Funktionen und Resultate nochmals eingehend überprüft, Script an manchen Orten noch etwas ausführlicher kommentiert und ausgebessert.


### 6. Januar
- Storytelling mit Alexandra Stark besprochen.

### 8. Januar
- Redezeit pro Wortmeldung ausgerechnet und der Tabelle hinzugefügt.
- Verknüpfen beider Scripts, Auswertungen und Resultate verfollständigt, am Text geschrieben.

### 9. Januar
- Grafiken erstellen, probiere verschiedene Darstellungen aus. Input von Chefredaktor Rolf Cavalli: Die schweigenden Grossräte möchte man auch sehen, deshalb hier eine zusätzliche Grafik erstellt. 
- Redezeit ist in Sekunden angegeben, mit Formel in Google Sheets umgerechnet.

### 10. Januar
- Letzte Arbeiten zur Finalisierung, Anpassung einiger Grafiken, Artikel im Print angepasst.

### 11. Januar
- Artikel ist erschienen. Wir planen einen Nachzug, wir wollen mit den schweigenden Grossräten sprechen. Von den "redseligen" melden sich zwei von sich aus und erklären sich. Dies lassen wir ebenfalls in den Nachzug einfliessen. Von den schweigenden Grossräten erreiche ich zwei, einer schweigt weiter.

### 12. Januar
- Nachzug ist erschienen.

# Ergebnis

## Hauptartikel
[Grossräte unter der Lupe: Die Redseligsten, die Eifrigsten, die Schweigsamsten – und so fleissig waren 2021 die Gewählten aus Ihrem Bezirk
](https://www.aargauerzeitung.ch/aargau/kanton-aargau/analyse-die-stillen-die-aktiven-die-brueckenbauer-so-arbeiteten-die-aargauer-grossraete-aus-ihrem-bezirk-ld.2234588)

## Nachzug
[Jetzt reden die «Schweiger» aus dem Grossen Rat – und die «Redseligen» erklären sich auch](https://www.aargauerzeitung.ch/aargau/kanton-aargau/datenanalyse-jetzt-reden-die-schweiger-aus-dem-grossen-rat-und-die-redseligen-erklaeren-sich-auch-ld.2237317)

<i>Herzlichen Dank an die Dozenten Barnaby Skinner, Thomas Ebermann, Simon Schmid, Alexandra Stark, Lea Senn und Mark Walther, und an das Team der Aargauer Zeitung, das mich bei der Umsetzung unterstützt hat.</i>