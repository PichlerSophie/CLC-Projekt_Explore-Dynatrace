# Kubernetes Monitoring und Deployment Guide
Wir haben nach längerem Überlegen und Probieren die Dynatrace Features für den Standard Kubernetes Cluster getestet und für den Autopilot Kubernetes Cluster. Darum wird bei der Erklärung zum Aufsetzen des Kubernetes Clusters auf beide Clusterarten eingegangen.
## Research Summary

Dynatrace bietet eine Plattform zur Überwachung und Optimierung von Anwendungen, Microservices und Container-Orchestrierungsplattformen wie Kubernetes. Die Plattform bietet dazu verschiedene Lösungen:
- Infrastructure Observability
- Application Observabilty
- Digital Experience
- Log Analytics
- Application Security
- Threat Observability
- Software Delivery
- Business Analytics

### Kubernetes Monitoring
- [Dynatrace Kubernetes Monitoring Overview](https://www.dynatrace.com/technologies/kubernetes-monitoring/)
- [Dynatrace Kubernetes Setup Documentation](https://docs.dynatrace.com/docs/ingest-from/setup-on-k8s)

Dynatrace bietet verschiedene Funktionen um einen Kubernetes Cluster zu monitoren. 
Es werden automatisch Kubernetes-Nodes und -Pods erkannt und kontinuierlich deren Status sowie Ressourcenverbrauch überwacht. Dadurch können Ausfälle und Ressourcenengpässe erkannt werden. Dadurch kann die Ressourcenauslastung besser verstanden und darauf reagiert werden. 

Durch das Erstellen von einem Dashboard können weitere Metriken, Ereignisse und Logs zu den Kubernetes-Pods und -Nodes zusammengefasst werden. Diese sind dann übersichtlich in einer einzigen Ansicht angeordnet und können für weitere Analysen genutzt werden. 

Die Logs können auch noch über eine Log-Analyse genauer untersucht werden. Dabei können Einblicke in die Container-Workloads erstellt werden, was auch dabei helfen kann Probleme im System zu identifzieren. 


### Log Monitoring
- [Dynatrace Log Monitoring Overview](https://www.dynatrace.com/de/platform/log-monitoring/)

Dynatrace bietet umfangreiche Möglichkeiten Logs aus Kubernetes-Clustern zu sammeln, anzuzueigen und zu analysieren. 
Das Log Monitoring bietet hierbei eine automatische Erkennung der eingehenden Logs. Es werden dabei dabei alle Logs aus verschiedenen Quellen, darunter auch WindowsEvent-Logs, Container-Logs und Anwedungslogs zentral gesammelt werden,  ohne zusätzliche Konfigurationen zu benötigen. 

Um nun diese Logs einzusehen, können diese über eine zentralisierte Übersicht aufgerufen werden. Darin werden zusätzlich noch andere Observability-Daten wie Traces und Metriken kombiniert und angezeigt. So bekommt man ein umfassendes Bild der System-Performance und kann Probleme effizient identifizieren.

Eine weitere Übersicht bietet eine Zeitleiste, die zeigt, wie viele Logs generiert wurden und welchen Status (INFO, WARN, ERROR) sie haben. Über eine Filterfunktion kann man dann nach spezifischen Ereignissen suchen oder ein gewisses Zeitfenster finden. 

Durch dieses Monitoring erhält man Einblicke und kann speziell auf Fehlersuche gehen. Mithilfe einer integrierten Analyse ist es leichter spezifische Fehler oder ungewähnliche Verhaltensweise in Logs aufzuspüren. Dafür kann man zusätzlich die Dynatrace Query Language (DQL) verwenden und Logs dabei flexibel filtern, sortieren und aggregieren. 

### Application Security

- [Dynatrace Application Security Overview](https://www.dynatrace.com/platform/application-security/)
- [Dynatrace Application Security Documentation](https://docs.dynatrace.com/docs/secure/application-security)

Mithilfe von Dynatrace können auch Schwachstellen, die sofortiger Aufmerksamkeit bedürfen erkannt werden und eine automatische Risiko- und Auswirkungsbewertung bereitgestellt werden. Dies passiert in Echtzeit und kann über die Vulnerabilities analysiert werden. 

Dynatrace bietet außerdem einen Laufzeit-Anwendungsschutz. Dieser kann in Echtzeit Angriffe auf die Anwendung automatisch erkennen und blockieren, ohne kritische Prozesse zu beeinträchtigen. 

Eine automatische Überprüfung neuer kritscher Schwachstellen hilft dabei die Applikation geschützt vor Angriffen zu halten. Es wird sofort darauf hingewiesen, wenn eine neue Schwachstelle implementiert wurde. Diese Ergebnisse und das Monitoring können ebenfalls in einem zusammengefassten Dashboard analysiert werden. 

---

## Einrichten von Kubernetes-Cluster und Deployment

### Schritte:
Voraussetzung: Google CLoud SDK muss installiert sein

1. **Clone Git Repository in gewünschten Ordner**
   ```bash
   git clone https://github.com/GoogleCloudPlatform/microservices-demo.git
   cd microservices-demo/
   ```
2. **Erstellen eines neuen Projekts in der Google Cloud**
   - Projektname: `clc3-project-g-h-p`
  
3. **GCloud Login**
   - Im Terminal muss folgendes Command ausgeführt werden und alle Schritte befolgt werden
    ```bash
    gcloud auth login
    ```

4. **Aktivieren der GKE API**
   ```bash
   gcloud services enable container.googleapis.com --project=clc3-project-g-h-p
   ```

5. **Erstellen eines neuen Kubernetes Cluster**  
   **Standard Cluster**  
   - Gehe zu: https://console.cloud.google.com/kubernetes
   - Erstellen klicken und "Zum Standardcluster wechseln" auswählen
   - Nodes Anzahl anpassen, Name vergeben, Region auswählen
   - "Erstellen" klicken
   - Da der Standard-Cluster nicht gleichzeitig die microservices-demo-Anwendung und Dynatrace stabil betreiben konnte, wurden die Ressourcen folgendermaßen erhöht:
     1. Speicher: Persistent Disk SSD-Quota von 200GB auf insgesamt 1,3TB erhöht
     2. CPU & Limits: CPU-Limit von 25 auf 50 erhöht
     
   **Autopilot Cluster**  
   - Gehe zu: https://console.cloud.google.com/kubernetes
   - Cluster erstellen klicken
   - Name veregebn und "Erstellen" klicken

6. **Zum Cluster verbinden**
   ```bash
   gcloud container clusters get-credentials online-boutique \
       --region us-central1 \
       --project clc3-project-g-h-p
   ```

7. **Bereitstellen der Demo-Anwendung**
   ```bash
   kubectl apply -f ./release/kubernetes-manifests.yaml
   ```
   - Warten, bis die Pods fertig sind. Dies kann ein paar Minuten dauern.
   ```bash
   kubectl get pods
   ```

8. **Projekt im Browser öffnen**
   - Mithilfe des nachfolgendem Befehls kann die URL ausgegeben werden, worüber das Frontend des Microservices erreicht werden kann.
   ```bash
   kubectl get service frontend-external
   ```
   - Alternativ dazu kann die URL auch von Dynatrace aus, aufgerufen werden.

9. **Hinzufügen aller Gruppenmitglieder zum Team**
   - Bei den Projekteinstellungen in der GKE "Diesem Projekt Personen hinzufügen" klicken & gewünschte Personen hinzufügen
   - Wechsel zu "IAM und Verwaltung"
   - Konto auswählen und "Weitere Rolle hinzufügen" klicken
   - Zuweisung der Rolle `Administrator für Kubernetes Engine Cluster`
   - Wiederholung der letzten beiden Schritte für alle Konten der Teammitglieder

---

## Dynatrace/Kubernetes Monitoring einrichten

### Schritte:

1. **Starten einer Dynatrace Testversion**

2. **Helm installieren**
   - Download Helm und füge es zu den Pfadvariablen hinzu
   - [Helm Releases on GitHub](https://github.com/helm/helm/releases)

3. **Suche nach Kubernetes Monitoring**
   - In der Dynatrace Übersicht muss nach Kubernetes Monitoring gesucht werden

4. **Folge dem Guide zum Aufsetzen des Monitorings**
   - Siehe dazu [Quickstart Guide](https://docs.dynatrace.com/docs/ingest-from/setup-on-k8s/quickstart).


---
## Microservice Architektur
![Systemarchitektur](https://github.com/PichlerSophie/CLC-Projekt_Explore-Dynatrace/blob/main/img/architecture-diagram.png)


| Dienst                    | Sprache     | Beschreibung                                                                                     |
|---------------------------|-------------|-------------------------------------------------------------------------------------------------|
| **Frontend**              | Go          | Stellt einen HTTP-Server bereit, um die Website zu bedienen. Erfordert keine Anmeldung/Registrierung und generiert automatisch Sitzungs-IDs für alle Benutzer. |
| **cartservice**      | C#          | Speichert die Artikel im Warenkorb des Benutzers in Redis und ruft sie ab.                     |
| **productcatalogservice** | Go          | Bietet eine Liste von Produkten aus einer JSON-Datei sowie die Möglichkeit, Produkte zu durchsuchen und einzelne Produkte abzurufen. |
| **currencyservice**        | Node.js     | Konvertiert Geldbeträge in andere Währungen. Verwendet reale Werte, die von der Europäischen Zentralbank abgerufen werden. Dies ist der Dienst mit der höchsten Anfragerate. |
| **paymentservice**        | Node.js     | Belastet die angegebene Kreditkarteninformation (Mock) mit dem angegebenen Betrag und gibt eine Transaktions-ID zurück. |
| **shippinhservice**         | Go          | Gibt Versandkostenschätzungen basierend auf dem Warenkorb und liefert Artikel an die angegebene Adresse (Mock). |
| **emailservice**         | Python      | Sendet den Benutzern eine Bestellbestätigungs-E-Mail (Mock).                                   |
| **checkoutservice**       | Go          | Ruft den Warenkorb des Benutzers ab, bereitet die Bestellung vor und koordiniert die Zahlung, den Versand und die E-Mail-Benachrichtigung. |
| **recommendationservice**     | Python      | Empfiehlt andere Produkte basierend auf dem Inhalt des Warenkorbs.                            |
| **adservice**        | Java        | Stellt Textanzeigen basierend auf gegebenen Kontextwörtern bereit.                            |
| **loadgenerator**         | Python/Locust | Sendet kontinuierlich Anfragen und simuliert realistische Benutzer-Einkaufsflüsse an das Frontend. |

##### Quelle: https://github.com/GoogleCloudPlatform/microservices-demo
---
## Dynatrace Features Step by Step Tutorial
### Kubernetes Monitoring
Wenn man auf der Dynatrace Startseite auf *Apps* klickt kann man alle möglichen Apps auswählen unter anderem auch *Kubernetes*, um den Cluster zu überwachen. Zuerst muss das Kubernetes Monitoring aber wie oben beschrieben aufgesetzt werden. Man gelangt nach dem Klick auf *Kubernetes* zu einer Übersicht, in der man alle möglichen Ressourcen genauer anschauen kann darunter fallen der Cluster, Nodes, Namespaces, Pods, Services und Container.  

![HowToKubernetes](https://github.com/PichlerSophie/CLC-Projekt_Explore-Dynatrace/blob/main/img/HowToKubernetes.png)

#### Cluster Analyse
Wenn man nun auf Cluster klickt, kann man den gewünschten Cluster auswählen. Dort wird dann eine Reihe an Information angezeigt, um die Auslastung des Cluster näher betrachten zu können. Es wird die aktuelle CPU Auslastung angezeigt, aber auch wie viel CPU Requests im gesamten Cluster angefordert werden und welches CPU Limit im gesamten Cluster definiert ist.  Dasselbe sieht man auch für die Arbeitsspeicherauslastung. Außerdem kann man sehen wie viele Pods aktuell tatsächlich laufen und wie viele verfügbar wären. Außerdem hat man eine schnelle Übersicht ob alle Nodes bereit sind, ob es Nodes mit Warnungen gibt und ob es Nodes mit Problemen gibt. Auch über die Workloads hat man auf einem Blick eine schnelle Übersicht der wichtigsten Informationen. Man sieht ob es Workloads gibt mit Container die gerade neu starten, ob es Workloads gibt mit fehlgeschlagenen Pods etc.
![ClusterÜbersicht](https://github.com/PichlerSophie/CLC-Projekt_Explore-Dynatrace/blob/main/img/ClusterUebersicht.png)

Um das Ganze näher betrachten zu können kann man sich die einzelnen Auslastungen auch für Nodes, Namespaces, Workloads, Services, Pods und Container ansehen. Links im Seitenmenü kann man die einzelnen Ressourcen auswählen. Das Prinzip ist dasselbe wie bei dem Cluster, nur dass die Auslastung genau für die ausgewählte Ressource angezeigt wird. 

#### Nodes Analyse
So kann man sich wenn man auf einen Node klickt ebenfalls die CPU und Arbeistsspeicherauslastung anzeigen lassen. Es ist genau aufgelistet wie viele Pods sich in dem Node befinden und wie viele Container darin laufen. Auch Probleme und Vulnerabilities würden angezeigt werden wenn es welche gibt. Die laufenden Pods in dem ausgewählten Nodes können über eine Liste eingesehen werden. Die Übersicht über CPU AUslastung etc.s sieht ähnlcih aus wie auf der Clusterübersicht. In dem Standard Cluster Monitoring kann man auch noch Diagramme im Reiter Logs und Events sehen (siehe nachfolgender Screenshot)
![LogsNodes](https://github.com/PichlerSophie/CLC-Projekt_Explore-Dynatrace/blob/main/img/LogsEventsStandard.png)

#### Pods Analyse
Auch wenn man Pods aus der Liste auswählt bekommt man ähnliche Auswertungen wie bei Nodes. Man bekommt wieder eine Liste der Pods und sieht dort auch gleich wie viele Container darin jeweils laufen und den Status des Pods. Die Übersicht über CPU Auslastung und Speicher Auslastung ist hier dieselbe wie bei den bereits vorgestellten. Zusätzlich gibt es eine Pods Analyse darin sieht man den Status und wie viele Neustarts die Container hatten. 
![PodAnalyse](https://github.com/PichlerSophie/CLC-Projekt_Explore-Dynatrace/blob/main/img/PodAnalysis.png)
![PodsAnalyse2](https://github.com/PichlerSophie/CLC-Projekt_Explore-Dynatrace/blob/main/img/Pods.png)

#### Container Analyse
Auch die jeweiligen Container kann man genauer unter die Lupe nehmen hier kann man aber lediglich die CPU Auslastung und die Speicherauslastung sehen. Natürlich allgemeine Infos wie zugehöriger Namespace, Pod, Node und Workload ist ebenfalls ersichtlich.

#### Namespaces Analyse
Wenn man Namespaces aus der Liste auswählt bekommt man eine kompakte Übersichtsliste aller laufenden Namespaces, dort kann man auch sehen wie lange diese schon aktiv sind und wie viele Workloads, Pods und Services sich in dem Namespace befinden. Wenn man auch hier wieder einen auswählt bekommt man eine ähnliche Übersicht mit CPU Auslastung und Speicher Auslastung, laufenden Pods und Workloads wie im Beispielbild vom Cluster allgemein. Die Workloads eines spezifischen  Namespaces können wieder über eine Liste angezeigt werden.   
![Namespaces](https://github.com/PichlerSophie/CLC-Projekt_Explore-Dynatrace/blob/main/img/Namespaces.png)


#### Workloads Analyse
Wenn man Workloads aus der Liste auswählt bekommt man alle Workloads in einer Übersicht angezeigt. Auf dieser ersten Übersicht kann man die Podsanzahl und das Alter ablesen. Wenn man eine Workload genauer betrachten möchte muss man auch hier lediglich auf die gewünschte Workload klicken und man bekommt wieder einen Auswertung der physischen Ressourcen (CPU, Memory). Auch den dazu gehörigen Pod kann man durch einfaches klicken wieder ansehen.  
![Workloads](https://github.com/PichlerSophie/CLC-Projekt_Explore-Dynatrace/blob/main/img/Workloads.png)

Wenn Workloads fehlerhaft sind können auch Probleme angezeigt werden, indem man auf Problems klickt.
![Problem](https://github.com/PichlerSophie/CLC-Projekt_Explore-Dynatrace/blob/main/img/ProblemWorkload.png)

#### Services Analyse
Wenn man nun Services aus der Liste auswählt, bekommt man ebenfalls wieder eine Liste mit allen laufenden Services. Hier kann man den zugehörigen Namespace ablesen und die Anzahl der Pods. Auch hier kann man wieder jeden einzelnen Service genauer unter die Lupe nehmen. Dort kann man sich dann den zugehörigen Pod ausgeben lassen. Außerdem wird der zugehörige Port aufgelistet.

#### Dashboards
Zu einer schnelleren Übersicht können auch Dashboards angesehen werden. Dafür muss nur in der linken Menüleiste auf Dashboards geklickt werden. 
![HowToDashboard](https://github.com/PichlerSophie/CLC-Projekt_Explore-Dynatrace/blob/main/img/DashboardHowTo.png)
Das erste Dashboard ist das Kubernetes Cluster Dashboard. Darin kann man alle Infos schnell auf einen Blick erkennen. Auch hier CPU Auslastung etc. Man kann das Ganze in einem Zeitraum einschränken.  

![Dashboard](https://github.com/PichlerSophie/CLC-Projekt_Explore-Dynatrace/blob/main/img/Dashboard.png)
Hier können dann weitere Dashboards ausgewählt werden wie Namespaces – Pods, oder Nodes -Pods. Dadurch bekommt man eine schnelle Übersicht über die wichtigsten Informationen von Nodes/Pods und Namespaces. Es gibt auch die Möglichkeit eigene Dashboards zu entwerfen.

Man sieht also die Bedienung ist sehr intuitiv und man kann sich schnell einen Überblick über die Kubernetes Ressourcen beschaffen. Außerdem kann man einfach die Auslastung ansehen und auch detaillierte Infos abfragen. Dynatrace erkennt also automatisch die Kubernetes Ressourcen wie Pods und Nodes und überwacht kontinuierlich deren Status. Somit können Ausfälle und mögliche Ressourcenengpässe schneller erkannt werden. 


### Log Monitoring

Mithilfe von Dynatrace können die Logs des Kubernetes-Clusters effizient analysiert und durchsucht werden. So funktioniert das Logging:

1. **Dynatrace OneAgent installieren:**
   Zuerst muss man den Dynatrace OneAgent auf dem Cluster installieren. Das geht ganz einfach, wenn man den Schritten aus der [Dynatrace-Dokumentation für Log Monitoring](https://www.dynatrace.com/de/platform/log-monitoring/) folgt. Auf der Dynatrace Webanwendung wird aber auch dem User automatisch die Aufsetzung von einer Log-Ingestion-Anwendung vorgeschlagen, sobald man im Suchfeld nach "Logs" sucht, und auf die Ansicht der Logs-App wechselt.

![OneagentSetup](/img/oneagent.png)

2. **Logs konfigurieren:**
   In Dynatrace geht man in die Einstellungen und wählt "Set up log ingest". Hier kann man die Quellen der Logs festlegen, wie z. B. Container-Logs, und die Log-Ingestion aktivieren.
   
![LogIngestionSetup](/img/setuplogingest.png)

3. **Logs durchsuchen und analysieren:**
   In der Logs-App kann man die gesammelten Logs durchsuchen und analysieren. Zum Beispiel kann man nach Status wie INFO, WARN oder ERROR filtern oder nach bestimmten Mustern oder Zeitfenstern suchen. Wenn man mehr Kontrolle braucht, kann man auch die Dynatrace Query Language (DQL) verwenden.

![LogsAppOverview](/img/logsappOverview.png)

   Ein Beispiel für eine DQL-Query:
   ```sql
   fetch logs
   | filter dt.entity.kubernetes_service == "KUBERNETES_SERVICE-F9B26EEE73A1B38D"
   | sort timestamp desc
   ```
In diesem Beispiel werden die Logs des Kubernetes-Services mit der ID F9B26EEE73A1B38D abgefragt. Dabei handelt es sich um den cartservice unseres Microservice, in welchem Informationen über den Einkaufswagen gesammelt werden (zum Beispiel welches Produkt zu welcher Zeit im Einkaufswagen gelandet ist).

So sieht das Ergebnis dieser Query aus:

![QueryResult](/img/queryResult.png)

4. **Visualisierung:**
   Es gibt auch die Möglichkeit, Dashboards zu erstellen, die die Log-Daten zusammen mit anderen Observability-Daten wie Metriken und Traces anzeigen. So hat man alles im Blick.

Mit diesen Schritten kann man die Logs des Kubernetes-Clusters effizient analysieren und schnell Probleme identifizieren.


### Application Security
#### Einstellungen
Um die Application Security zu verwenden, müssen zuvor einige Einstellungen in Dynatrace getätigt werden. 
Dafür navigiert man zu den *Settings Classic*. Dort wählt man den Unterpunkt für Application Security aus. 
Hier muss man dann bei den *Vulnerability Analytics: General settings* noch die Punkte aktivieren, die man überwachen möchte. 
Einmal für die Third-party Vulnerability Analytics:

![applicationsecurity](/img/applicationsecurityenable.png)

Man kann auch noch alle nötigen Technologien auswählen, die hierbei überwacht werden sollen. 
Im zweiten Tab *Code-Level Vulnerability Analytics* kann man die Analyse für die Überwachung auf Codeebene einschalten. 

![grafik](https://github.com/user-attachments/assets/7bec5389-4517-4660-b666-a0ac4b9ad41e)

Hier können Java und .Net überwacht werden. 

Um das Monitoring zur Laufzeit zu aktivieren, muss unter *Application Protection* zu den General Settings navigiert werden. Hier aktiviert man die Runtime Application Protection. Dynatrace kontrolliert hierbei hereinkommende Angriffe auf die Applikation und kann diese Monitoren oder blockieren. Es besteht auch die Möglichkeit Angriffe zu ignorieren. 

![grafik](https://github.com/user-attachments/assets/154f7cdb-772e-453b-a761-17393d39b434)

Möchte man die nachfolgenden Punkte überwachen, müssen die oben erwähnten 3 Einstellungen zuvor aktiviert werden. 

#### Dashboards

Es können Dashboards erstellt werden, um die Application Security zu überwachen. Dazu kann man für die App Third-party vulnerabilities *Create report* drücken, um für diese mit einem Template ein Dashboard erstellen zu können.
![grafik](https://github.com/user-attachments/assets/27d63460-baaa-4518-becd-4b21a634f023)
![grafik](https://github.com/user-attachments/assets/8282feac-f687-4ff1-af37-ebe5af97df12)

Diese kann man dann über die Dashboards aufrufen und analysieren. 

#### Security Overview, Third-party vulnerabilities und Code-level vulnerabilities
Über die Apps kann man unter der Sektion Application Security die verschiedenen Apps auswählen, die für dieses Überwachen zuständig sind. 

Hier kann man die Security Overview, Third-Party Vulnerabilities, Code-Level Vulnerabilities und Attacks App aufrufen, um genauere Informationen zu bekommen und genauer zu analysieren, welche Risiken auftreten. 

#### Eine detaillierte Analyse der drei ausgewählten Features befindet sich im Dokument !Verlinkung!
---
## Lessons Learned
### Kubernetes Autopilot Einschränkungen
- Dynatrace kann Logging in Kubernetes Autopilot nicht vollständig überwachen.
- Für vollständige Observability ist der Standard Kubernetes Cluster besser geeignet.

### Ressourcenmanagement in Kubernetes
- Die begrenzte Ressourcenzuweisung der kostenlosen Google Cloud Variante stellte eine große Herausforderung dar. Dynatrace benötigt für die Überwachung und das Sammeln von Metriken ebenfalls signifikante Ressourcen, was zusammen mit dem Microservice zu einer unzureichenden Ressourcenkapazität führte. Dies machte es erforderlich, sowohl die CPU- als auch die Speicherzuweisungen abzustimmen, um den Standard-Cluster stabil  betreiben zu können.

### Dynatrace Setup & Nutzung
- Die Einrichtung von Dynatrace erforderte eine gewisse Einarbeitungszeit, nach der Einarbeitungszeit war die Bedienung jedoch sehr intuitiv.
- Sehr gute und umfangreiche Dokumentation.
- Sobald Dynatrace eingerichtet war, lieferte es wertvolle Einblicke in Performance, Logs und Sicherheit.

### Dynatrace bietet starke Monitoring- und Security-Features
- Durch die detaillierte Überwachung des Kubernetes Clusters konnten wir wichtige Erkenntnisse über die Auslastung und das Verhalten gewinnen. Die präzise Erfassung und Analyse der Logs ermöglichte es uns, Fehlerquellen schnell zu identifizieren und auch interessante Einblicke zu gewinnen. Engpässe und Optimierungsmöglichkeiten konnten dadurch identifiziert werden.
- Die Application Security Features waren sehr spannend, da man dadurch schnell einen Einblick über potenzielle Risiken bekommt.

### Weitere Dynatrace Features
- Dynatrace bietet deutlich mehr spannende Funktionen, die jedoch aufgrund von Zeit- und Ressourcenbegrenzungen nicht alle getestet werden konnten.


