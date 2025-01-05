## Analyse ausgewählter Dynatrace Funktionalitäten

### Kubernetes Monitoring
[Kubernetes Monitoring](https://www.dynatrace.com/technologies/kubernetes-monitoring/)

[Setup Doku](https://docs.dynatrace.com/docs/ingest-from/setup-on-k8s)


### Log Monitoring
[Log Monitoring](https://www.dynatrace.com/de/platform/log-monitoring/)

Dynatrace bietet umfangreiche Möglichkeiten, Logs aus Kubernetes-Clustern zu sammeln, anzuzeigen und zu analysieren. Hier sind die wichtigsten Features und Schritte zur Nutzung:

#### Automatische Erkennung und Sammlung von Logs
Dynatrace erkennt automatisch Logs aus verschiedenen Quellen, darunter:
- WindowsEvent-Logs
- Container-Logs
- Anwendungs-Logs

Die Logs können ohne zusätzliche Konfiguration zentral gesammelt werden. Das macht es einfach, mit dem Logging zu starten.

#### Logs-App für einfache Analyse
Mit der Logs-App in Dynatrace hat man eine benutzerfreundliche Oberfläche, um Logs anzuzeigen und zu durchsuchen. Dabei sieht man:
- Eine Übersicht in einer Zeitleiste, die zeigt, wie viele Logs generiert wurden und welcher Status (INFO, WARN, ERROR) sie haben.
- Eine Filterfunktion, mit der man spezifische Ereignisse oder Zeitfenster finden kann.

#### Erweiterte Analyse mit Dynatrace Query Language (DQL)
Wenn man tiefere Einblicke benötigt, kann man die Dynatrace Query Language (DQL) verwenden. Damit lassen sich Logs flexibel filtern, sortieren und aggregieren. 

Ein einfaches Beispiel:
```sql
fetch logs
| filter dt.entity.kubernetes_service == "KUBERNETES_SERVICE-F9B26EEE73A1B38D"
| sort timestamp desc
```
DQL ermöglicht es, komplexe Abfragen zu erstellen, die genau auf spezifische Anwendungsfälle zugeschnitten sind.

#### Zentralisierte Ansicht und Integration
Alle Logs werden in einer zentralen Ansicht kombiniert und können mit anderen Observability-Daten wie Metriken und Traces analysiert werden. So bekommt man ein umfassendes Bild der System-Performance und kann Probleme effizient identifizieren.

#### Visualisierung und Dashboards
Dynatrace erlaubt es, Dashboards zu erstellen, die Log-Daten visualisieren. Diese können individuell angepasst oder mit vorhandenen Vorlagen erstellt werden. 

### Application Security
[Application Security](https://www.dynatrace.com/de/platform/application-security/)
