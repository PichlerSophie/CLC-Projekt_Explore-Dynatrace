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

1. **Erstellen eines neuen Projekts in Google Cloud**
   - Projektname: `clc3-project-g-h-p`

2. **Hinzufügen alle Gruppenmitglieder zum Team**
   - Weisen Sie die Rolle `Administrator` für Kubernetes Engine Cluster zu.

3. **Aktivieren der GKE API**
   ```bash
   gcloud services enable container.googleapis.com --project=clc3-project-g-h-p

4. **Erstellen eines neuen Kubernetes Cluster**
   ```bash
   gcloud container clusters create-auto online-boutique \
       --project=clc3-project-g-h-p \
       --region=us-central1
   ```

5. **Bereitstellen der Demo-Anwendung**
   ```bash
   kubectl apply -f ./release/kubernetes-manifests.yaml
   ```
   - Warten, bis die Pods fertig sind. Dies kann ein paar Minuten dauern.
   ```bash
   kubectl get pods
   ```

6. **Projekt im Browser öffnen**
   - URL: [http://35.225.80.212](http://35.225.80.212)

7. **Zum CLuster verbinden**
   ```bash
   gcloud container clusters get-credentials online-boutique \
       --region us-central1 \
       --project clc3-project-g-h-p
   ```

---

## Dynatrace aufsetzen

### Schritte:

1. ***Starten einer Dynatrace Testversion**

2. **Helm installieren**
   - Download Helm and add it to your path variable.
   - [Helm Releases on GitHub](https://github.com/helm/helm/releases)

3. **Suche nach Kubernetes Monitoring**

4. **Folge dem Guide zum Aufsetzen des Monitorings**
   - Refer to the [Quickstart Guide](https://docs.dynatrace.com/docs/ingest-from/setup-on-k8s/quickstart).


---
## Microservice Architektur
![Systemarchitektur](https://github.com/PichlerSophie/CLC-Projekt_Explore-Dynatrace/blob/main/ArchitekturDiagramm.png)

## Dienste und Beschreibung

| Dienst                    | Sprache     | Beschreibung                                                                                     |
|---------------------------|-------------|-------------------------------------------------------------------------------------------------|
| **Frontend**              | Go          | Stellt einen HTTP-Server bereit, um die Website zu bedienen. Erfordert keine Anmeldung/Registrierung und generiert automatisch Sitzungs-IDs für alle Benutzer. |
| **Warenkorbservice**      | C#          | Speichert die Artikel im Warenkorb des Benutzers in Redis und ruft sie ab.                     |
| **Produktkatalogservice** | Go          | Bietet eine Liste von Produkten aus einer JSON-Datei sowie die Möglichkeit, Produkte zu durchsuchen und einzelne Produkte abzurufen. |
| **Währungsdienst**        | Node.js     | Konvertiert Geldbeträge in andere Währungen. Verwendet reale Werte, die von der Europäischen Zentralbank abgerufen werden. Dies ist der Dienst mit der höchsten Anfragerate. |
| **Zahlungsdienst**        | Node.js     | Belastet die angegebene Kreditkarteninformation (Mock) mit dem angegebenen Betrag und gibt eine Transaktions-ID zurück. |
| **Versanddienst**         | Go          | Gibt Versandkostenschätzungen basierend auf dem Warenkorb und liefert Artikel an die angegebene Adresse (Mock). |
| **E-Mail-Dienst**         | Python      | Sendet den Benutzern eine Bestellbestätigungs-E-Mail (Mock).                                   |
| **Checkoutservice**       | Go          | Ruft den Warenkorb des Benutzers ab, bereitet die Bestellung vor und koordiniert die Zahlung, den Versand und die E-Mail-Benachrichtigung. |
| **Empfehlungsdienst**     | Python      | Empfiehlt andere Produkte basierend auf dem Inhalt des Warenkorbs.                            |
| **Anzeigendienst**        | Java        | Stellt Textanzeigen basierend auf gegebenen Kontextwörtern bereit.                            |
| **Lastgenerator**         | Python/Locust | Sendet kontinuierlich Anfragen und simuliert realistische Benutzer-Einkaufsflüsse an das Frontend. |

---
## Dynatrace Features Step by Step
### Kubernetes Monitoring
### Log Monitoring
### Application Security
---

# Proposal:

[Written Proposal](proposal.md)
