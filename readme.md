# Kubernetes Monitoring und Deployment Guide
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

## Einrichten von Kubernetes-Cluster und Deployment

### Schritte:

1. **Erstellen eines neuen Projekts in der Google Cloud**
   - Projektname: `clc3-project-g-h-p`

2. Clone Git Repository in gewünschten Ordner
   ```bash
   git clone https://github.com/GoogleCloudPlatform/microservices-demo.git
   cd microservices-demo/
   ```

4. **Hinzufügen aller Gruppenmitglieder zum Team**
   - Bei Projekteinstellungen "Diesem Projekt Personen hinzufügen" klicken & gewünschte Personen hinzufügen
   - Wechsel zu "IAM und Verwaltung"
   - Konto auswählen und "Weitere Rolle hinzufügen" klicken
   - Zuweisung der Rolle `Administrator für Kubernetes Engine Cluster`
   - Wiederholung der letzten beiden Schritte für alle Konten der Teammitglieder

5. **Aktivieren der GKE API**
   ```bash
   gcloud services enable container.googleapis.com --project=clc3-project-g-h-p
   ```

6. **Erstellen eines neuen Kubernetes Cluster**
   ```bash
   gcloud container clusters create-auto online-boutique \
       --project=clc3-project-g-h-p \
       --region=us-central1
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
   - URL: [http://35.225.80.212](http://35.225.80.212)

9. **Zum CLuster verbinden**
   ```bash
   gcloud container clusters get-credentials online-boutique \
       --region us-central1 \
       --project clc3-project-g-h-p
   ```

---

## Dynatrace aufsetzen

### Schritte:

1. **Starten einer Dynatrace Testversion**

2. **Helm installieren**
   - Download Helm und füge es zu den Pfadvariablen hinzu
   - [Helm Releases on GitHub](https://github.com/helm/helm/releases)

3. **Suche nach Kubernetes Monitoring**

4. **Folge dem Guide zum Aufsetzen des Monitorings**
   - Siehe dazu [Quickstart Guide](https://docs.dynatrace.com/docs/ingest-from/setup-on-k8s/quickstart).


---
## Microservice Architektur
![Systemarchitektur](https://github.com/PichlerSophie/CLC-Projekt_Explore-Dynatrace/blob/main/architecture-diagram.png)


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
### Application Security
---
## Lessons Learned

