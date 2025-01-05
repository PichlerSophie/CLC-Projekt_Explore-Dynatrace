# Kubernetes Monitoring und Deployment Guide
Wir haben nach längerem Überlegen und Probieren die Dynatrace Features für den Standard Kubernetes Cluster getestet und für den Autopilot Kubernetes Cluster. Darum wird bei der Erklärung zum Aufsetzen des Kubernetes Clusters auf beide Clusterarten eingegangen.
## Research Summary

### Kubernetes Monitoring
- [Dynatrace Kubernetes Monitoring Overview](https://www.dynatrace.com/technologies/kubernetes-monitoring/)
- [Dynatrace Kubernetes Setup Documentation](https://docs.dynatrace.com/docs/ingest-from/setup-on-k8s)


### Log Monitoring
- [Dynatrace Log Monitoring Overview](https://www.dynatrace.com/de/platform/log-monitoring/)

### Application Security

- [Dynatrace Application Security Overview](https://www.dynatrace.com/platform/application-security/)
- [Dynatrace Application Security Documentation](https://docs.dynatrace.com/docs/secure/application-security)

---

## Einrichten von Kubernetes-Cluster und Deployment - Überarbeiten!!

### Schritte:
Voraussetzung: Google CLoud SDK muss installiert sein

1. Clone Git Repository in gewünschten Ordner
   ```bash
   git clone https://github.com/GoogleCloudPlatform/microservices-demo.git
   cd microservices-demo/
   ```
2. **Erstellen eines neuen Projekts in der Google Cloud**
   - Projektname: `clc3-project-g-h-p`
  
3. GCloud Login
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
   **Autopilot Cluster**  
   - Gehe zu: https://console.cloud.google.com/kubernetes
   - Cluster erstellen klicken
   - Name veregebn und "Erstellen" klicken

7. **Zum Cluster verbinden**
   ```bash
   gcloud container clusters get-credentials online-boutique \
       --region us-central1 \
       --project clc3-project-g-h-p
   ```

8. **Bereitstellen der Demo-Anwendung**
   ```bash
   kubectl apply -f ./release/kubernetes-manifests.yaml
   ```
   - Warten, bis die Pods fertig sind. Dies kann ein paar Minuten dauern.
   ```bash
   kubectl get pods
   ```

9. **Projekt im Browser öffnen**
   - URL: [http://35.225.80.212](http://34.57.8.76)

10. **Hinzufügen aller Gruppenmitglieder zum Team**
   - Bei Projekteinstellungen "Diesem Projekt Personen hinzufügen" klicken & gewünschte Personen hinzufügen
   - Wechsel zu "IAM und Verwaltung"
   - Konto auswählen und "Weitere Rolle hinzufügen" klicken
   - Zuweisung der Rolle `Administrator für Kubernetes Engine Cluster`
   - Wiederholung der letzten beiden Schritte für alle Konten der Teammitglieder

---

## Dynatrace aufsetzen

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

---
## Dynatrace Features Step by Step Tutorial
### Kubernetes Monitoring
### Log Monitoring

Mithilfe von Dynatrace können die Logs des Kubernetes-Clusters effizient analysiert und durchsucht werden. So funktioniert das Logging:

1. **Dynatrace OneAgent installieren:**
   Zuerst muss man den Dynatrace OneAgent auf dem Cluster installieren. Das geht ganz einfach, wenn man den Schritten aus der [Dynatrace-Dokumentation für Log Monitoring](https://www.dynatrace.com/de/platform/log-monitoring/) folgt. Auf der Dynatrace Webanwendung wird aber auch dem User automatisch die Aufsetzung von einer Log-Ingestion-Anwendung vorgeschlagen, sobald man im Suchfeld nach "Logs" sucht, und auf die Ansicht der Logs-App wechselt.

   INSERT FOTO VON SETUP LOG INGESTION

3. **Logs konfigurieren:**
   In Dynatrace geht man in die Einstellungen und wählt "Set up log ingest". Hier kann man die Quellen der Logs festlegen, wie z. B. Container-Logs, und die Log-Ingestion aktivieren.
   
![logIngestionSetup](/img/logIngestionSetup.png)

5. **Logs durchsuchen und analysieren:**
   In der Logs-App kann man die gesammelten Logs durchsuchen und analysieren. Zum Beispiel kann man nach Status wie INFO, WARN oder ERROR filtern oder nach bestimmten Mustern oder Zeitfenstern suchen. Wenn man mehr Kontrolle braucht, kann man auch die Dynatrace Query Language (DQL) verwenden.

   Ein Beispiel für eine DQL-Query:
   ```sql
   fetch logs
   | filter dt.entity.kubernetes_service == "KUBERNETES_SERVICE-F9B26EEE73A1B38D"
   | sort timestamp desc
   ```

6. **Visualisierung:**
   Es gibt auch die Möglichkeit, Dashboards zu erstellen, die die Log-Daten zusammen mit anderen Observability-Daten wie Metriken und Traces anzeigen. So hat man alles im Blick.

INSERT FOTO VON LOG-QUERY ANSICHT
![Dynatrace Logs App Screenshot](https://github.com/user-attachments/assets/0191b85d-92be-453e-9a32-6f407f1332ab)

Mit diesen Schritten kann man die Logs des Kubernetes-Clusters effizient analysieren und schnell Probleme identifizieren.


### Application Security
#### Einstellungen
Um die Application Security zu verwenden, müssen zuvor einige Einstellungen in Dynatrace getätigt werden. 
Dafür navigiert man zu den *Settings Classic*. Dort wählt man den Unterpunkt für Application Security aus. 
Hier muss man dann bei den *Vulnerability Analytics: General settings* noch die Punkte aktivieren, die man überwachen möchte. 
Einmal für die Third-party Vulnerability Analytics:

![applicationsecurity](https://github.com/user-attachments/assets/0191b85d-92be-453e-9a32-6f407f1332ab)

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

---
## Lessons Learned

