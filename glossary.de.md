# Reactive Manifesto Glossary

* [Asynchron](#Asynchronous)
* [Ausfall (im Unterschied zu Fehler)](#Failure)
* [Benutzer](#User)
* [Delegieren](#Delegation)
* [Elastizität (im Unterschied zu Skalierbarkeit)](#Elasticity)
* [Gegendruck](#Back-Pressure)
* [Isolation (und Eindämmung)](#Isolation)
* [Komponente](#Component)
* [Nachrichtenorientiert (im Unterschied zu Ereignisorientiert)](#Message-Driven)
* [Nicht-Blockierend](#Non-Blocking)
* [Ortsunabhängigkeit](#Location-Transparency)
* [Protokoll](#Protocol)
* [Replizieren](#Replication)
* [Ressource](#Resource)
* [Skalierbarkeit](#Scalability)
* [Stapelverarbeitung](#Batching)
* [System](#System)

## <a name="Asynchronous"></a>Asynchron
*Englisch: [asynchronous](/glossary#Asynchronous)*

Die wörtliche Bedeutung ist _„nicht gleichzeitig“_. Im Kontext dieses Manifests ist gemeint, dass die Verarbeitung einer Anfrage an das System zu einem späteren Zeitpunkt geschieht, den der Anfragende nicht direkt beobachten kann. Das Gegenteil wäre synchrone Ausführung, wobei der Anfragende stets über den Fortschritt informiert ist; hiermit geht einher, dass der Verarbeitungsprozess den Fortschritt des Anfragenden hemmt, weil typischerweise dessen Ressourcen für die Verarbeitung herangezogen werden.

## <a name="Failure"></a>Ausfall (im Unterschied zu Fehler)
*Englisch: [failure (in contrast to error)](/glossary#Failure)*

Ein Ausfall ist ein unerwartetes Ereignis, welches das System seiner normalen Funktion beraubt. Ein ausgefallenes System kann im Allgemeinen nicht mehr auf Anfragen antworten, was neben den momentan ausstehenden auch alle zukünftigen betreffen kann. Ein Fehler hingegen ist eine vom Programmierer vorgesehene Bedingung, die eine negative Antwort des Systems hervorruft, also je nur eine einzelne Anfrage betrifft. Ein ausgefallenes [System](#System) bedarf eines externen Eingriffs, um wieder zu nominellem Verhalten zurückzukehren (etwa durch Neustart einer seiner [Komponenten](#Component)).

Beispiele für Ausfälle sind defekte Hardware, Programmabbruch aufgrund von Überlastung, korrupter Datenbestand. Beispiele für Fehler sind inkorrekte Eingaben, fehlerhafte Authentifizierung oder fehlende Autorisation.

## <a name="User"></a>Benutzer
*Englisch: [user](/glossary#User)*

Wir verwenden diesen Begriff universell für alles, was Anfragen an ein System stellt, sei es ein Mensch oder eine Maschine.

## <a name="Delegation"></a>Delegieren
*Englisch: [delegation](/glossary#Delegation)*

Eine Aufgabe [asynchron](#Asynchronous) an eine andere [Komponente](#Component) zu delegieren bedeutet, dass die Verarbeitung im Kontext dieser Komponente geschieht. Dies kann bedeuten, dass die Ausführung in einem anderen Fehlerbehandlungskontext geschieht, auf einer anderen CPU, in einem anderen Prozess oder auf einem anderen Netzwerkknoten. Hiermit wird bezweckt, dass für die Aufgabe andere oder zusätzliche Ressourcen zur Verfügung stehen. Außerdem obliegt die Verantwortung für die Erledigung der Aufgabe nun der Komponente, an die sie delegiert wurde. Dies ist insbesondere dann wichtig, wenn die delegierende Komponente aufgrund eines Ausfalls nicht mehr selbst zu ihrer Erledigung in der Lage ist; in diesem Sinne kann Delegieren auch implizit geschehen, indem Fortschritt und Zustand einer Komponente von außen überwacht werden.

## <a name="Elasticity"></a>Elastizität (im Unterschied zu Skalierbarkeit)
*Englisch: [elasticity (in contrast to scalability)](/glossary#Elasticity)*

Elastizität bedeutet, dass der Durchsatz eines Systems automatisch erhöht oder vermindert werden kann, um sich an sich verändernde Lastbedingungen anzupassen. Voraussetzung hierfür ist die [Skalierbarkeit](#Scalability) des Systems, damit der Durchsatz von vermehrten [Ressourcen](#Resource) profitieren kann. Elastizität erweitert also den Begriff der Skalierbarkeit um automatisches Ressourcenmanagement.

## <a name="Back-Pressure"></a>Gegendruck
*Englisch: [back-pressure](/glossary#Back-Pressure)*

Wenn eine [Komponente](#Component) überlastet ist, muss das [System](#System) im Ganzen darauf angemessen reagieren. Ohne eine geplante Strategie für diesen Fall werden die Nachrichtenpuffer volllaufen und so entweder ausfallen oder Nachrichten verwerfen. Es gilt, die Anforderung zu erfüllen, dass alle Komponenten stets antwortbereit bleiben müssen. Dies kann nur erreicht werden, wenn eine überlastete Komponente diesen Zustand erkennen und melden kann, entweder an ihre übergeordnete Komponente, um weitere Ressourcen bereitgestellt zu bekommen, oder an ihre Benutzer. Letzteres bezeichnet man als Gegendruck, weil dem hereinkommenden Fluss von Anfragen ein Hindernis in den Weg gestellt wird, welchen diesen verlangsamt. Die genaue Ausprägung kann variieren zwischen expliziter Flusskontrolle (indem der aufrufenden Komponente temporär die Erlaubnis entzogen wird Anfragen zu senden) und kontrolliertem Überlastverhalten mit verminderter Servicequalität (bis hin zu negativen Antworten). Auf diese Weise kann sich der Gegendruck im äußersten Fall bis zum Endbenutzer fortpflanzen, was dieser dann als explizite Systemantwort erhält. Die Alternative wäre, das System insgesamt ausfallen zu lassen, so dass gar keine Antwort möglich wäre.

Zusätzlich zur Erhaltung der Antwortbereitschaft bietet Gegendruck die Möglichkeit, übergeordnete Komponenten das System von innen beobachten zu lassen und auf das Auftreten von Gegendruck [elastisch](#Elasticity) zu antworten.

## <a name="Isolation"></a>Isolation (und Eindämmung)
*Englisch: [isolation and encapsulation](/glossary#Isolation)*

Isolation bedeutet Entkopplung in Zeit und Raum. Entkopplung entlang der Zeitachse heisst, dass Sender und Empfänger einer Nachricht in verschiedenen Lebenszyklen sein können, sie müssen nicht zur selben Zeit zur Kommunikation bereit sein. Dies wird erreicht durch das einfügen von [asynchronen](#Asynchronous) Grenzen zwischen [Komponenten](#Component), über die die [Nachrichten](#Message-Driven) verschickt werden. Um Komponenten räumlich zu entkoppeln, müssen sie die Freiheit besitzen, außerhalb desselben Prozesses oder Speicheradressraums ausgeführt zu werden, sie müssen [ortsunabhängig](#Location-Transparency) sein. Der Ausführungsort kann sich hierbei während der Laufzeit des Systems last- oder benutzungsabhängig verändern.

Vollständige Isolation geht über die objektorientierte Kapselung gängiger Programmiersprachen hinaus, indem Komponenten Kompartmentalisierung und komplette Trennung der folgenden Aspekte gewährleisten:

* Zustand und Verhalten stehen nicht dem direkten Zugriff durch andere Komponenten offen. Dieser als „share nothing“ bezeichnete Ansatz minimiert die Koordinationskosten in Systemen mit vielen oder verteilten Prozessorkernen, siehe auch das [Universal Scalability Law](http://www.perfdynamics.com/Manifesto/USLscalability.html).
* [Ausfälle](#Failure) können so eingedämmt und an der Ausbreitung auf andere Komponenten gehindert werden.

Da die Implementierung von Isolation zur Laufzeit die Verwendung von wohldefinierten [Nachrichtenprotokollen](#Protocol) zwischen den Komponenten erfordert, führt ihre Umsetzung auch in der Programmarchitektur und Umsetzung zu einer Entkopplung: die Komponenten sind weniger starr miteinander verbunden und das gesamte [System](#System) wird einfacher zu warten, zu validieren und verifizieren und zu erweitern.

## <a name="Component"></a>Komponente
*Englisch: [component](/glossary#Component)*

Reaktive Systeme basieren auf einer modularen Softwarearchitektur, einer Idee, die nicht neu ist (siehe z.B. [Parnas (1972)](https://www.cs.umd.edu/class/spring2003/cmsc838p/Design/criteria.pdf). Wir verwenden den Begriff der „Komponente“ wegen seiner Nähe zum Englischen Wort „compartment“ (Abteil), welches [Isolation](#Isolation) und Selbständigkeit betont. Die Entkopplung von Komponenten zur Laufzeit wird aufgrund der [Nachrichtenorientierung](#Message-Driven) in der Struktur des Quellcodes der Softwaremodule reflektiert: jede Komponente wird durch ein eigenes Modul realisiert, welches wiederkehrende Funktionalität durch gemeinsam genutzte Programmbibliotheken abdeckt. Die Grenzen einer Komponente fallen oft zusammen mit denen eines [„bounded context“](http://martinfowler.com/bliki/BoundedContext.html), ein Begriff aus dem Domain-Driven Design, welcher einen Teilgeschäftsbereich beschreibt, innerhalb dessen Prozesse, Akteure und Daten vermittels einer gemeinsamen Sprachregelung bezeichnet werden. Dies bedeutet, dass ein reaktiver Systementwurf sich an den realen Geschäftsprozessen orientiert und somit organisch an diese anpasst. Zwischen verschiedenen Komponenten sorgen [Protokolle](#Protocol) für die notwendige Übersetzung und ermöglichen so die Kommunikation.

## <a name="Message-Driven"></a>Nachrichtenorientiert (im Unterschied zu Ereignisorientiert)
*Englisch: [message driven (in contrast to event driven)](/glossary#Message-Driven)*

Eine Nachricht ist ein Datensatz, der an eine Zieladresse versandt wird. Ein Ereignis hingegen ist ein Signal, welches eine Komponente bei Erreichen eines bestimmten Zustands ungerichtet emittiert. In einem nachrichtenorientierten System warten adressierbare [Komponenten](#Component) auf das Eintreffen von Nachrichten und reagieren auf diese; wenn keine Nachrichten vorliegen bleiben sie inaktiv. In einem ereignisorientierten System werden Prozeduren bei den Ereignisquellen registriert, so dass diese ausgeführt werden, wenn das fragliche Ereignis eintritt. Der Unterschied zwischen beiden liegt also darin, dass im ersten Fall die Nachrichtenempfänger adressierbar sind, während im zweiten Fall der Fokus auf adressierbaren Ereignisquellen liegt. Es ist möglich, Ereignisse als Nachrichten zu versenden.

Widerstandsfähigkeit ist in ereignisorientierten Systemen schwieriger umzusetzen, weil die kurzlebige Zuordnung von Prozeduren zu Ereignissen eine nachhaltige Fehlerbehandlung erschwert: die Wiederherstellung des nominellen Zustandes erfordert eine stabil adressierbare und somit steuerbare Komponente, nicht die einmalige Übersetzung und Übermittlung einer negativen Antwort.

## <a name="Non-Blocking"></a>Nicht-Blockierend
*Englisch: [non-blocking](/glossary#Non-Blocking)*

In der nebenläufigen Programmierung wird ein Algorithmus als „nicht-blockierend“ bezeichnet, wenn selbst beim konkurrierenden Zugriff auf eine gemeinsam genutzte Ressource kein Thread auf unbegrenzte Zeit an der Ausführung gehindert ist. In der Praxis bedeutet dies, dass eine Programmierschnittstelle Methoden anbietet, die bei derzeitiger Nichtverfügbarkeit der Ressource unmittelbar mit einem entsprechenden Rückgabewert zurückkehren anstatt den Aufrufer zu blockieren, bis die Ressource wieder verfügbar ist. Damit erlaubt eine nicht-blockierende Schnittstelle dem Aufrufer, während der Wartezeit andere Aktivitäten auszuführen. Der Rückgabewert kann hierbei die Möglichkeit bieten, eine Benachrichtigung zu registrieren, die die Verfügbarkeit der Ressource anzeigt.

## <a name="Location-Transparency"></a>Ortsunabhängigkeit
*Englisch: [location transparency](/glossary#Location-Transparency)*

[Elastische](#Elasticity) Systeme müssen anpassungsfähig sein und ständig auf Veränderungen in ihrem Umfeld reagieren können, sie müssen mit Leichtigkeit und Effizienz ihre Ressourcen vergrößern und auch wieder verringern können. Eine Schlüsselerfahrung in diesem Sinne ist, dass verteiltes Rechnen kein Sonderfall mehr ist: ob mehrere Prozessorkerne über schnelle serielle Verbindungen innerhalb einer Maschine miteinander kommunizieren oder ob die Kommunikation über ein Netzwerkkabel erfolgt, macht in vielerlei Hinsicht keinen qualitativen Unterschied. Wenn wir uns diese Erkenntnis zu eigen machen, verschwimmt die Grenze zwischen vertikaler und horizontaler Skalierung von Anwendungen.

Wenn alle [Komponenten](#Component) unserer System nachrichtengetrieben sind und überall in einem Cluster ausgeführt werden können, ist die lokale Kommunikation im selben Adressraum nur noch eine Optimierungsmöglichkeit, wir müssen die Details der Verteilung auf verschiedene Rechenknoten nicht mehr statisch in der Planungsphase des Systems vornehmen. Vielmehr kann das System nun dynamisch und entsprechend seiner Auslastung zur Laufzeit verteilt und optimiert werden.

Diese räumliche Entkopplung (siehe auch [Isolation](#Isolation)) wird möglich durch asynchrone Nachrichtenübermittlung und das Loslösen der Referenzen von ihren zugrundeliegenden Serviceinstanzen, was wir Ortsunabhängigkeit nennen. Dies darf nicht verwechselt werden mit dem Begriff des transparenten verteilten Rechnen („transparent remoting“), es ist vielmehr das genaue Gegenteil desselben: wir respektieren die Effekte und Einschränkungen, die jegliche Netzwerkkommunikation beinhaltet — partielle Ausfälle, Netzwerkinseln, Nachrichtenverlust, Asynchronizität — und modellieren sie explizit anstatt das Modell des lokalen Methodenaufrufes auf das Netzwerk zu übertragen. In diesem Sinne stimmen wir überein mit [„A Note On Distributed Computing“](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.41.7628) von Waldo et al.

## <a name="Protocol"></a>Protokoll
*Englisch: [protocol](/glossary#Protocol)*

Ein Protokoll definiert die Behandlung von und die Art und Weise in der Nachrichten zwischen [Komponenten](#Component) übermittelt werden. Die Formulierung erfolgt als Relation zwischen den Kommunikationspartnern, dem angehäuften Protokollzustand sowie den zulässigen Nachrichtentypen. Somit bestimmt das Protokoll, welche Nachrichten von wem zu einem gewissen Zeitpunkt gesendet werden dürfen und mit wessen Empfang folglich gerechnet werden muss. Man unterteilt Protokolle grob nach allgemeinen Nachrichtenmustern, die häufigsten sind Frage–Antwort (auch wiederholt, wie in HTTP), publish–subscribe („veröffentlichen und abonnieren“) und gerichteter Datenstrom.

Im Vergleich zur Definition von lokalen Programmierschnittstellen ist ein Protokoll allgemeiner, es kann mehrere Akteure beinhalten und einen gemeinsam fortschreitenden Kommunikationszustand abbilden, während eine Schnittstelle nur einzelne Interaktionen zwischen Aufrufer und Aufgerufenem betrachtet.

Eine wichtige Eigenschaft von Protokollen ist, dass sie die zulässigen Nachrichten regeln, nicht aber wie diese übertragen oder kodiert werden, dies sind üblicherweise Details der tieferliegenden Protokollschichten.

## <a name="Replication"></a>Replizieren
*Englisch: [replication](/glossary#Replication)*

Die simultane Ausführung einer [Komponente](#Component) an mehreren Orten wird als Replizieren bezeichnet. Dies kann die Verwendung von mehreren Threads und damit Prozessorkernen bedeuten, schließt aber auch die Verteilung auf mehrere Netzwerkknoten oder Rechenzentren ein. Eine Komponente wird repliziert zum Zwecke der Skalierung, indem die eingehenden Anfragen auf mehrere Recheneinheiten verteilt werden, oder zur Widerstandsfähigkeit, wo zur Ausfallsicherheit dieselben Anfragen mehrfach ausgeführt werden. Diese Aspekte können auch gemischt werden, zum Beispiel indem sichergestellt wird, dass alle Anfragen einen Benutzer betreffend auf mindestens zwei verschiedenen Knoten bearbeitet werden, während die Gesamtknotenzahl mit der Systemauslastung variiert, siehe auch [Elastizität](#Elasticity).

## <a name="Resource"></a>Ressource
*Englisch: [resource](/glossary#Resource)*

Alles, wovon eine [Komponente](#Component) zu ihrer Ausführung abhängt, ist eine Ressource die bereitgestellt und verwaltet werden muss. Dies beinhaltet neben Prozessorkernen und Hauptspeicher auch Festplattenspeicher, Netzwerkbandbreite, Hauptspeicherbandbreite, Prozessorcaches, Kommunikationsverbindungen zwischen Prozessorsockeln, Zeitgeber und zeitgesteuerte Programmausführung, Eingabe- und Ausgabegeräte, Datenbanken, Netzwerkdateisysteme und so weiter. All diese Ressourcen müssen bei der Planung der [Elastizität](#Elasticity) einer Komponente berücksichtigt werden.

## <a name="Scalability"></a>Skalierbarkeit
*Englisch: [scalability](/glossary#Scalability)*

Die Fähigkeit eines [Systems](#System), von vermehrter Ressourcenzuweisung zu profitieren, wird charakterisiert durch die Verhältnisse von Durchsatz und Ressourcen in verschiedenen Konfigurationen. Für ein perfekt skalierbares System sind beide Verhältnisse proportional zueinander: doppelte Ressourcenzuweisung führt zu doppeltem Durchsatz. Dieses Ideal ist in der Praxis nicht zu erreichen, limitierende Faktoren sind globale Engstellen und die Notwendigkeit zur Synchronisation und Koordination zwecks Erreichen eines konsistenten Systemverhaltens. Für weitergehende Informationen siehe [Amdahl’s Law and Gunther’s Universal Scalability Model](http://blogs.msdn.com/b/ddperf/archive/2009/04/29/parallel-scalability-isn-t-child-s-play-part-2-amdahl-s-law-vs-gunther-s-law.aspx).

## <a name="Batching"></a>Stapelverarbeitung
*Englisch: [batching](/glossary#Batching)*

Die derzeitigen Prozessorarchitekturen sind optimiert auf die wiederholte Ausführung einer gegebenen Aufgabe: hochperformante Zwischenspeicher erhöhen die Rechenkapazität bei gleichbleibender Taktfrequenz. Dies bedeutet, dass es von Nachteil ist, einem Prozessorkern rasch wechselnde Aufgaben zuzuweisen, vielmehr sollte die Programmierung so strukturiert sein, dass gleichförmige Aufgaben aneinandergereiht werden, um von den Optimierungen der Prozessoren zu profitieren. Dies kann geschehen, indem eine gleichbleibende Rechenvorschrift nacheinander auf eine Reihe von verschiedenen Eingangsparametern angewandt wird oder indem für die Verarbeitung eines Datenstroms verschiedenen Prozessschritten der Reihe nach eigene Prozessorkerne zur Ausführung zugewiesen werden.

Dies lässt sich auf die Verwendung von externen Ressourcen übertragen, die Synchronisation und Koordination benötigen. Der Durchsatz eines Festspeichermediums kann signifikant verbessert werden, wenn Anfragen statt von mehreren konkurrierenden Threads nur von einem dedizierten Thread gestellt werden. Einen solchen gemeinsamen Zugriffspunkt zur Stapelverarbeitung zu nutzen erlaubt außerdem, die Anfragen so zu sortieren, dass die Zugriffsmuster mit dem internen Speicheraufbau harmonieren (wenn z.B. linear fortschreitende Zugriffe schneller ausgeführt werden als zufällig verteilte).

Stapelverarbeitung bietet darüber hinaus noch die Möglichkeit, mehrere nacheinander eintreffende Anfragen miteinander zu kombinieren, wenn sie dieselben Daten betreffen, um so die wiederholten Fixkosten einzusparen, die für jeden Zugriff anfallen.

## <a name="System"></a>System
*Englisch: [system](/glossary#System)*

Der Zweck eines Systems ist, seinen [Benutzern](#User) Dienste anzubieten. Je nach Dienst können Systeme klein oder groß sein, aus wenigen oder einer Vielzahl von [Komponenten](#Component) bestehen. Alle Komponenten eines Systems arbeiten bei der Bereitstellung der Dienste zusammen, sie stehen hierbei üblicherweise in Client–Server-Verhältnissen (zum Beispiel zwischen Front-End und Back-End). Ein System definiert ein Modell für die Widerstandsfähigkeit seiner Komponenten, [Ausfälle](#Failure) werden innerhalb des Systems erkannt und durch [Delegieren](#Delegation) behoben. Es ist zweckmäßig, Komponenten innerhalb eines Systems als Subsysteme zu gruppieren wenn sie in ihrer gemeinsamen Funktion, ihren [Ressourcen](#Resource) und ihrem Ausfallmodell vom Rest des Systems [isoliert](#Isolation) sind.

