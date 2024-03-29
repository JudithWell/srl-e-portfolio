# Digitaler Programmierkurs mit tech.io
Im Netz existieren bereits viele Webseiten mit interaktiven Programmierbeispielen - siehe z.B. [w3schools](https://www.w3schools.com/python/python_lists.asp) oder [inf-schule](https://www.inf-schule.de/imperative-programmierung/python/konzepte/funktion/beispiel_caesar).
Jedoch existieren wenig Möglichkeiten interaktive Programmierelemente in digitalem Unterrichtsmaterial unterzubringen. [tech.io](https://tech.io) bietet hier Abhilfe und erlaubt es:
- Online-Kurse auf ihrer Website zu erstellen.
- Unterstützt Text, Multiple-Choice-Fragen, Video-Einbettung o.ä. sowie ausführbare und editierbare Code-Blöcke in vielen Programmiersprachen.
- Hinterlegen von Testprogrammen, welche überprüfen, ob der eingegebene Code die Aufgabe tatsächlich erfüllt.
- Farbliche Rückmeldung ob Aufgaben korrekt oder inkorrekt ausgeführt wurden.

## tech.io-Kurs: Listen in Python
Für meinen Algorithmik-Unterricht in der 11. Jgst. habe ich einen Kurs zur Verwendung von Listen in Python erstellt. Der Kurs ist [hier](https://tech.io/playgrounds/135793/listen-in-python/mehrere-informationen-speichern) zu finden.
Der Kurs bietet einen Einstieg ins Thema. Die Motivation erfolgt über das führen einer Einkaufsliste in einem Pythonprogramm.
In den Lernabschnitten wechseln sich Verständnisfragen, Programmierbeispiele und -aufgaben sowie Kontrollfragen ab.
![[Pasted image 20240312122824.png]]
**Abbildung:** Erklärtexte und Auswahlfragen aus dem tech.io-Kurs.
![[Pasted image 20240312122950.png]]
**Abbildung:** Programmieraufgabe mit Feedback zur Lösung.
Zusätzliche Tipps und Hilfestellungen zu den Aufgaben können ausgeklappt werden.
Zur Differenzierung exisitiert am Ende eine Bonus-Aufgabe zur Implementierung eines Bibliotheksverwaltungssystems.
Um den Fortschritt der Lernen und ihre Selbsteinschätzung zu überprüfen, führte ich [Umfragen](https://survey.lamapoll.de/Inf11-Feedback-Listen) durch.
## Reflektion des tech.io-Kurses
Den Kurs hatte ich tatsächlich als Reaktion auf dieses Seminar erstellt, um eine freier Arbeit beim Programmierenlernen zu ermöglichen. Bei der Umsetzung sind mir die folgenden Probleme aufgefallen:
- Eine initiale Input-Phase hätte mir die Möglichkeit gegeben auf Fragen zum Grundverständnis direkt eingehen zu können, statt Motivation, Einstieg und Vermittlung neuer Konzepte komplett in den Kurs auszulagern
- Die Lernenden neigten dazu, die Erklärtexte nicht gründlich zu lesen, sondern immer schnell zum nächsten interaktiven Element zu springen. Dadurch entstanden viele Verständnisprobleme und Unklarheiten bzgl. der Aufgabenstellung. Hier wäre eine feingranularere Einteilung in mehr kleine Seiten wohl sinnvoller gewesen.
- Viele Lernende hatten tatsächlich Spaß an der Arbeit mit dem Kurs und die Stunde hatte einen entspannt produktiven Charakter. Einzelne schwächere Schüler:innen waren jedoch auch davon überfordert.

## Nachteile von tech.io
Leider ist tech.io nicht perfekt. Der Dienst befindet sich schon sehr lange in einem "beta"-Zustand und wird scheinbar nicht mehr weiterentwickelt. Die Kurse sind zwar open-source und können in einem github-Projekt abgelegt werden, jedoch ist die Darstellung als Website nicht quelloffen und von den Betreibern abhängig. Das Erstellen von automatischen Tests kann zeitaufwendig sein und läd sehr zum "verkünsteln" ein; auf der anderen Seite besteht die Gefahr, dass bei einem Code-Block ohne Testcode sofort "Success" angezeigt wird, auch ohne Änderungen durch die Lernenden.
Ein weiterer Nachteil ist, dass es keine Diagnosemöglichkeiten gibt, sprich der Fortschritt der Schüler:innen lässt sich nicht nachverfolgen. In der Praxis müsste hier dann wohl auf [Umfragen](https://survey.lamapoll.de/Inf11-Feedback-Listen) zurückgegriffen werden. 