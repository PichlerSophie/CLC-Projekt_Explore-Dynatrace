# Proposal - Explore Dynatrace to observe your application 

## Ziel: 
In diesem Projekt soll ein Microservice mithilfe von Dynatrace überwacht werden. Die Anwendung soll in einem Kubernetes Cluster aufgesetzt sein. Unser Hauptziel ist es, mithilfe spezifischer Dynatrace-Funktionen wie Kubernetes Monitoring, Distributed Tracing und Service Monitoring die Infrastruktur und das Verhalten des Microservices zu analysieren. Damit möchten wir Ressourcenauslastung, Leistungsengpässe und Fehlerquellen in der Anwendung identifizieren.

## Was existiert bereits? 
- Dynatrace Testversion wird genutzt, um die Überwachung durchzuführen 
- Der Microservice (https://github.com/GoogleCloudPlatform/microservices-demo)  *Da wir keine eigene Microservice-Implementierung zur Verfügung haben, haben wir uns für die Verwendung einer Demo-Lösung aus dem Internet entschieden. Die Demo Lösung ist auf GitHub frei verfügbar.

## Was wird neu konfiguriert bzw. erstellt? 
- Ein neuer Kubernetes Cluster wird erstellt, darin wird der zu überwachende Microservice aufgesetzt. 
- Das Produkt Dynatrace wird eingerichtet, um unsere Anwendung überwachen zu können.

## Dynatrace Features im Detail
### Kubernetes Monitoring
- **Kontinuierliche Erkennung und Überwachung:** Dynatrace erkennt automatisch Kubernetes-Nodes und -Pods und überwacht kontinuierlich deren Status sowie Ressourcenverbrauch, dadurch können wir Ausfälle und Ressourcenengpässe überwachen. Außerdem können wir dadurch die Ressourcenauslastung besser verstehen. 
- **Metriken, Ereignisse und Logs in einer Übersicht:** Alle relevanten Daten zu den Kubernetes-Pods und -Nodes werden in einem Dashboard zusammengefasst. 
- **Log-Analyse für Workload-Einblicke:** Durch die Analyse gewinnen wir Einblicke in die Container-Workloads, was uns helfen kann Probleme zu identifizieren.

### Log Monitoring
- **Automatische Erkennung von eingehenden Logs**: Hier sammelt Dynatrace Logs aus verschiedenen Quellen, darunter WindowsEvent-Logs, Container-Logs und Anwendungslogs, ohne zusätliche Konfigurationen zu benötigen.
- **Zentralisierte Übersicht aller Log-Daten**: Alle Logs werden mit anderen Observability-Daten wie Traces und Metriken in einer zentralen Ansicht kombiniert.
- **Einblicke und Fehlersuche**: Die integrierte Analyse macht es leicht, spezifische Fehler oder ungewöhnliche Verhaltensweisen in Logs aufzuspüren. 

## Architektur Diagramm 
![Architektur Diagramm](https://github.com/PichlerSophie/CLC-Projekt_Explore-Dynatrace/blob/main/ArchitekturDiagramm.png)

## Welche Cloud Technologien werden verwendet? 
- Google Cloud 
- Kubernetes (GKE) 
- Dynatrace 

## Meilensteine 
![Meilensteine](https://github.com/PichlerSophie/CLC-Projekt_Explore-Dynatrace/blob/main/meilensteineCLC.png)
1) Kubernetes Cluster einrichten + Microservice darin deployen (26.12.2014) 
2) Dynatrace Testversion beantragen + einrichten (02.01.2025)
3) Integration von Dynatrace zum Monitoring (06.01.2025)
4) Testen des Arbeitsprozesses (06.01.2025)
5) Erkunden der Dynatrace Funktionalitäten (17.01.2025)
6) Dokumentation + Präsentation anfertigen (24.01.2025)
7) Präsentation + Live Demo (03.02.2025) 

## Arbeitsteilung & Verantwortlichkeiten 

### Gabath: 
- Meilenstein 4 

### Huber: 
- Meilenstein 1 

### Pichler: 
- Meilenstein 2 & 3 

### Alle gemeinsam 
- Meilenstein 5 
- Meilenstein 6 
- Meilenstein 7 
