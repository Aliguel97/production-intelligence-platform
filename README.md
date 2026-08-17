# production-intelligence-platform
Entwicklung einer datengetriebenen Business-Intelligence-Plattform für ein Produktionsunternehmen
# Production Analytics – End-to-End Data Analyst Project

## Projektübersicht

Dieses Projekt ist ein umfassendes End-to-End-Data-Analytics-Projekt für ein fiktives Produktionsunternehmen.

Ziel war es, eine realitätsnahe Analytics-Lösung zu entwickeln, die operative Unternehmensdaten aus verschiedenen Bereichen automatisiert verarbeitet, zentral speichert und anschließend für Business-Analysen und interaktive Power-BI-Dashboards zur Verfügung stellt.

Das Projekt umfasst insgesamt:

| Kennzahl             |           Umfang |
| -------------------- | ---------------: |
| Datensätze           | **ca. 4,5 Mio.** |
| Tabellen             |           **26** |
| Unternehmensbereiche |            **7** |
| Datenplattform       |   **PostgreSQL** |
| Cloud Storage        |       **AWS S3** |
| Automatisierung      |          **n8n** |
| Visualisierung       |     **Power BI** |

Der Fokus liegt dabei nicht nur auf der Visualisierung von Daten, sondern auf der Entwicklung einer vollständigen **Data Pipeline vom Rohdatensatz bis zum fertigen Business Dashboard**.

---

# Projektziel

Das Ziel des Projekts war es, eine zentrale Datenbasis für ein Produktionsunternehmen aufzubauen und daraus relevante Informationen für Management und operative Fachbereiche bereitzustellen.

Dabei sollten insbesondere folgende Fragen beantwortet werden:

* Wie effizient arbeitet die Produktion?
* Welche Maschinen verursachen die meisten Stillstandszeiten?
* Welche Schichten erzielen die beste Produktionsleistung?
* Wo entstehen Qualitätsprobleme und Ausschuss?
* Welche Produkte sind besonders profitabel?
* Welche Lieferanten weisen eine schlechte Performance auf?
* Wie effizient wird das Lager verwaltet?
* Wie hoch ist der Energieverbrauch pro produziertem Stück?
* Welche Faktoren beeinflussen die Produktionskosten?
* Wo bestehen konkrete Optimierungspotenziale?

---

# Datenumfang

Das Projekt basiert auf einem umfangreichen simulierten Unternehmensdatensatz mit rund **4,5 Millionen Datensätzen**, verteilt auf **26 Tabellen**.

Die Daten bilden verschiedene Prozesse eines produzierenden Unternehmens ab und ermöglichen dadurch eine bereichsübergreifende Analyse.

## 7 Unternehmensbereiche

### 1. Produktion

* Produktionsaufträge
* Produktionsmengen
* Produktionszeiten
* Produktionsleistung
* Schichten

### 2. Maschinen & Wartung

* Maschinen
* Maschinenstatus
* Stillstände
* Störungen
* Wartungsaktivitäten

### 3. Qualität

* Qualitätskontrollen
* Fehler
* Ausschuss
* Fehlerarten
* Qualitätskennzahlen

### 4. Mitarbeiter

* Mitarbeiter
* Abteilungen
* Schichten
* Fehlzeiten
* Krankheitsdaten
* Fluktuation

### 5. Supply Chain & Lager

* Lieferanten
* Bestellungen
* Rohstoffe
* Lagerbestände
* Wareneingänge
* Logistik

### 6. Finanzen & Wirtschaftlichkeit

* Umsätze
* Produktionskosten
* Wartungskosten
* Einkaufskosten
* Gewinne

### 7. Energie & Nachhaltigkeit

* Energieverbrauch
* Energiequellen
* Energieverbrauch je Maschine
* Energieverbrauch je Produktionseinheit

---

# Systemarchitektur

Die technische Architektur wurde als End-to-End-Datenpipeline aufgebaut.

```text
                    Datenquellen
                         │
                         ▼
                ┌─────────────────┐
                │     AWS S3      │
                │   Raw Storage   │
                └────────┬────────┘
                         │
                         ▼
                ┌─────────────────┐
                │      n8n        │
                │  Automation /   │
                │  Data Pipeline  │
                └────────┬────────┘
                         │
                         ▼
                ┌─────────────────┐
                │   PostgreSQL    │
                │  Data Platform  │
                └────────┬────────┘
                         │
                         ▼
                ┌─────────────────┐
                │    Data Model   │
                │   / KPI Layer   │
                └────────┬────────┘
                         │
                         ▼
                ┌─────────────────┐
                │    Power BI     │
                │    Dashboards   │
                └─────────────────┘
```

## AWS S3

**AWS S3** dient als zentrale Ablage für die Rohdaten.

Die Daten werden dort zunächst in ihrer ursprünglichen Form gespeichert, bevor sie für die weitere Verarbeitung bereitgestellt werden.

Dies ermöglicht eine klare Trennung zwischen:

* Rohdaten
* verarbeiteten Daten
* analytischen Daten

---

## n8n

**n8n** übernimmt die Automatisierung der Datenpipeline.

Die Pipeline kann beispielsweise:

1. neue Dateien erkennen
2. Daten aus dem Storage abrufen
3. Daten prüfen
4. Daten transformieren
5. Daten an PostgreSQL übergeben
6. Prozesse protokollieren
7. Fehler erkennen und entsprechend reagieren

Damit wird aus einer manuellen Datenverarbeitung ein automatisierter Workflow.

---

## PostgreSQL

**PostgreSQL** dient als zentrale relationale Datenbank.

Die 26 Tabellen werden dort strukturiert gespeichert und miteinander verknüpft.

SQL wird unter anderem für:

* Datenabfragen
* Datenvalidierung
* Transformationen
* Aggregationen
* KPI-Berechnungen
* Business Questions

eingesetzt.

---

## Power BI

**Power BI** bildet die Präsentations- und Analyseebene.

Die Daten aus PostgreSQL werden modelliert und anschließend in interaktiven Dashboards visualisiert.

Dabei wird zwischen Management-KPIs und detaillierten operativen Analysen unterschieden.

---

# Datenmodell

Die verschiedenen Unternehmensbereiche werden über ein relationales Datenmodell miteinander verbunden.

Vereinfacht:

```text
                         Mitarbeiter
                              │
                              ▼
                           Schichten
                              │
                              ▼
Produkte ───────────────► Produktion ◄────────────── Maschinen
                              │                         │
                              │                         ▼
                              │                     Wartung
                              │
                              ▼
                           Qualität


Lieferanten ─────► Einkauf ─────► Rohstoffe ─────► Lager
                                                      │
                                                      ▼
                                                   Logistik


Produktion ─────► Energie
Produktion ─────► Finanzen
```

Dadurch können Zusammenhänge zwischen verschiedenen Unternehmensbereichen analysiert werden.

---

# Wichtige Kennzahlen

Ein zentraler Bestandteil des Projekts ist die Entwicklung relevanter Business-KPIs.

## OEE – Overall Equipment Effectiveness

Die **OEE (Overall Equipment Effectiveness)** bzw. Gesamtanlageneffektivität misst die tatsächliche Effektivität einer Produktionsanlage.

Sie setzt sich aus drei Faktoren zusammen:

```text
OEE = Verfügbarkeit × Leistung × Qualität
```

### Verfügbarkeit

Wie viel der geplanten Produktionszeit steht die Maschine tatsächlich zur Verfügung?

### Leistung

Wie schnell produziert die Maschine im Verhältnis zu ihrer theoretisch möglichen Produktionsgeschwindigkeit?

### Qualität

Wie hoch ist der Anteil fehlerfreier produzierter Einheiten?

Die OEE ermöglicht dadurch eine ganzheitliche Bewertung der Maschinenperformance.

---

## Produktions-KPIs

* Produktionsmenge
* Produktionsleistung
* OEE
* Maschinenverfügbarkeit
* Stillstandszeit
* Produktionsleistung je Schicht
* Produktionsleistung je Maschine

## Qualitäts-KPIs

* Ausschussquote
* Fehlerquote
* Anzahl Qualitätsfehler
* Fehler je Produkt
* Fehler je Maschine
* Qualitätsentwicklung über die Zeit

## Mitarbeiter-KPIs

* Mitarbeiteranzahl
* Krankheitsquote
* Fehlzeiten
* Fluktuationsquote
* Produktionsleistung je Schicht

## Supply-Chain-KPIs

* Lieferzeit
* Lieferantentermintreue
* Lagerbestand
* Lagerumschlag
* Beschaffungsvolumen

## Finanz-KPIs

* Umsatz
* Gewinn
* Produktionskosten
* Wartungskosten
* Einkaufskosten
* Profitabilität je Produkt

## Energie-KPIs

* Energieverbrauch
* Energieverbrauch je Maschine
* Energieverbrauch pro produziertem Stück
* Energiekosten

---

# Power-BI-Dashboard

Die Analyseergebnisse werden in mehreren interaktiven Power-BI-Seiten dargestellt.

## Management Overview

Die Managementübersicht zeigt die wichtigsten Unternehmenskennzahlen auf einen Blick:

* OEE
* Produktionsmenge
* Ausschussquote
* Maschinenverfügbarkeit
* Umsatz
* Gewinn
* Energieverbrauch

### Dashboard Preview

![Power BI Management Dashboard](images/dashboard_overview.png)

---

## Produktions- und Maschinenanalyse

Dieser Bereich untersucht die operative Produktionsperformance.

Analysiert werden unter anderem:

* Maschinenperformance
* Stillstandszeiten
* Produktionsleistung
* Schichtperformance
* OEE
* Störungsursachen

![Power BI Production Dashboard](images/dashboard_production.png)

---

## Qualitäts- und Business-Analyse

Hier werden Qualitäts-, Kosten- und Wirtschaftlichkeitskennzahlen miteinander verbunden.

Beispielsweise können folgende Zusammenhänge untersucht werden:

```text
Ausschuss
   ↓
Produkt
   ↓
Maschine
   ↓
Schicht
   ↓
Mögliche Ursache
```

![Power BI Quality Dashboard](images/dashboard_quality.png)

---

# Analyseprozess

Der gesamte Analyseprozess wurde in mehrere Schritte unterteilt.

## 1. Data Ingestion

Die Rohdaten werden in AWS S3 bereitgestellt und anschließend über automatisierte Workflows verarbeitet.

## 2. Data Processing

Die Daten werden geprüft, bereinigt und für die Speicherung vorbereitet.

Dazu gehören unter anderem:

* Prüfung fehlender Werte
* Erkennung von Duplikaten
* Datentypprüfung
* Validierung von Beziehungen
* Standardisierung von Kategorien
* Datenbereinigung

## 3. Data Storage

Die aufbereiteten Daten werden in PostgreSQL gespeichert.

## 4. Data Modeling

Die Tabellen werden zu einem analytischen Datenmodell verbunden.

## 5. KPI Development

Auf Basis der Geschäftsanforderungen werden relevante Kennzahlen mit SQL und DAX berechnet.

## 6. Exploratory Data Analysis

Die Daten werden untersucht, um:

* Trends
* Ausreißer
* Zusammenhänge
* Performanceunterschiede
* mögliche Ursachen

zu identifizieren.

## 7. Visualization

Die Ergebnisse werden anschließend in Power BI visualisiert.

## 8. Business Analysis

Im letzten Schritt werden die Ergebnisse interpretiert und in konkrete Business Insights überführt.

---

# Beispiel einer Root-Cause-Analyse

Ein wichtiger Bestandteil des Projekts ist die Untersuchung nicht nur des Problems, sondern seiner möglichen Ursache.

Beispiel:

```text
Niedrige OEE
      │
      ▼
Geringe Verfügbarkeit
      │
      ▼
Hohe Stillstandszeit
      │
      ▼
Maschine 07
      │
      ▼
Häufige technische Störungen
      │
      ▼
Erhöhter Wartungsbedarf
```

Dadurch wird aus einer reinen KPI-Auswertung eine **datenbasierte Ursachenanalyse**.

---

# Verwendete Technologien

| Technologie | Verwendung                                   |
| ----------- | -------------------------------------------- |
| Python      | Datenaufbereitung und Analyse                |
| Pandas      | Datenverarbeitung                            |
| SQL         | Datenabfragen, Transformationen und Analysen |
| PostgreSQL  | Zentrale relationale Datenbank               |
| AWS S3      | Speicherung der Rohdaten                     |
| n8n         | Automatisierung der Datenpipeline            |
| Power BI    | Visualisierung und Dashboarding              |
| DAX         | KPI- und Measure-Berechnungen                |
| Docker      | Containerisierung                            |
| GitHub      | Versionskontrolle und Dokumentation          |

---

# Repository-Struktur

```text
production-analytics/
│
├── data/
│   ├── raw/
│   └── processed/
│
├── python/
│   ├── data_cleaning.py
│   ├── exploratory_analysis.py
│   └── analysis.py
│
├── sql/
│   ├── data_cleaning.sql
│   ├── kpi_analysis.sql
│   └── business_questions.sql
│
├── n8n/
│   └── workflows/
│
├── powerbi/
│   └── production_analytics.pbix
│
├── documentation/
│   ├── data_dictionary.md
│   └── architecture.md
│
├── images/
│   ├── dashboard_overview.png
│   ├── dashboard_production.png
│   └── dashboard_quality.png
│
└── README.md
```

---

# Business Value

Das Projekt zeigt, wie aus großen Mengen heterogener Unternehmensdaten ein zentraler Analytics-Prozess aufgebaut werden kann.

Die entwickelte Lösung ermöglicht unter anderem:

* Produktionsengpässe zu erkennen
* Maschinenstillstände zu analysieren
* Qualitätsprobleme zu identifizieren
* Schichtperformance zu vergleichen
* Lieferanten zu bewerten
* Lagerbestände zu überwachen
* Energieeffizienz zu analysieren
* Kostenfaktoren zu identifizieren
* Profitabilität zu vergleichen
* datenbasierte Entscheidungen zu unterstützen

Der Fokus liegt dabei auf der Verbindung von **technischer Datenanalyse und konkreten Business-Fragestellungen**.

---

# Im Projekt demonstrierte Kompetenzen

### Data Analytics

* Data Cleaning
* Explorative Datenanalyse
* SQL
* Python
* Pandas
* KPI-Entwicklung
* Root-Cause-Analyse

### Data Engineering

* Datenpipelines
* AWS S3
* PostgreSQL
* n8n
* ETL-Prozesse
* Datenmodellierung
* Automatisierung

### Business Intelligence

* Power BI
* DAX
* Dashboard Design
* KPI Reporting
* Data Storytelling
* Business Analysis

---

# Projektstatus

**Status:** Abgeschlossen

**Projektart:** Data Analyst Abschlussprojekt / Portfolio-Projekt

**Branche:** Produktion / Manufacturing

**Datenumfang:** ca. 4,5 Mio. Datensätze

**Datenmodell:** 26 Tabellen

**Unternehmensbereiche:** 7

**Technischer Schwerpunkt:** End-to-End Data Analytics Pipeline

---

# Fazit

Dieses Projekt zeigt einen vollständigen Data-Analytics-Workflow – von der Datenspeicherung und automatisierten Verarbeitung über die Datenmodellierung und KPI-Berechnung bis hin zur interaktiven Visualisierung in Power BI.

Dabei wurde besonderer Wert darauf gelegt, nicht nur Daten darzustellen, sondern aus den Daten **geschäftlich relevante Erkenntnisse und mögliche Handlungsempfehlungen** abzuleiten.

Der zentrale Ansatz des Projekts lautet:

> **Rohdaten in belastbare Erkenntnisse und konkrete Handlungsmöglichkeiten für das Unternehmen übersetzen.**
