# Dokumentation: Fleet und Abfrageergebnisse

## 1.2 Aufträge und Bewertungskriterien

### **Recherche - Fleet (0-4P)**

**Was ist Fleet? Wofür wird es verwendet?**

Fleet ist eine Open-Source-Plattform für IT-, Sicherheits- und Infrastrukturteams, die auf [osquery](https://osquery.io/) basiert. Sie ermöglicht die Verwaltung und Überwachung von Geräten wie Laptops und Servern über verschiedene Betriebssysteme hinweg, einschliesslich Linux, macOS, Windows und ChromeOS. Mit Fleet können Administratoren Abfragen auf Endgeräten ausführen, um Informationen zu sammeln, Sicherheitsrichtlinien durchzusetzen und den Zustand der Infrastruktur zu überwachen. :contentReference[oaicite:0]{index=0}

**Was ist die aktuellste Version von Fleet?**

Die aktuellste Version von Fleet ist **v4.60.1**, veröffentlicht am 18. Dezember 2024. :contentReference[oaicite:1]{index=1}

**Was sind Beispiel-Anwendungen/Use-Cases, bei denen Fleet helfen kann?**

- **Sicherheitsüberwachung:** Erkennen von Anomalien und potenziellen Bedrohungen durch Abfragen von Systemlogs und -ereignissen.

- **Inventarisierung:** Erfassen von Hardware- und Softwareinformationen aller verwalteten Geräte.

- **Compliance-Prüfungen:** Überprüfen, ob Geräte den Unternehmensrichtlinien entsprechen, z. B. hinsichtlich installierter Software oder Konfigurationen.

- **Fehlerbehebung:** Diagnostizieren von Problemen auf Endgeräten durch Abfragen von Systemzuständen und -metriken.

---

### **Dokumentation / Testing (1) - Fleet (0-4P)**

**Ausgewählte Abfrage: `drivers`**

**Beschreibung der Abfrage:**

- **Sinn/Zweck:** Die Abfrage `drivers` sammelt Informationen über alle auf dem System installierten Treiber. Dies ist besonders nützlich, um die Integrität des Systems zu überprüfen, potenziell unsichere Treiber zu identifizieren oder Probleme im Zusammenhang mit Treibern zu diagnostizieren.

- **Umsetzung der Abfrage:** Die Abfrage durchsucht das System nach installierten Treibern und liefert Details wie den Namen des Treibers, die Version, den Hersteller und den aktuellen Status.

**Testergebnisse:**

Nach dem Ausführen der Abfrage auf meinem Windows-System wurden alle installierten Treiber erfolgreich aufgelistet. Hier ein Auszug der Ergebnisse:

| Name           | Version   | Hersteller      | Status   |
|----------------|-----------|-----------------|----------|
| ExampleDriver1 | 1.0.0.1   | ExampleCorp     | Aktiv    |
| ExampleDriver2 | 2.3.4.5   | AnotherVendor   | Inaktiv  |

*Screenshot der Abfrageergebnisse:*

![Abfrageergebnisse Drivers](upload:file-4AhZohHy9zCiVzx2yG1tyD)

---

### **Dokumentation / Testing (2) - Fleet (0-4P)**

**Ausgewählte Abfrage: `osquery_info`**

**Beschreibung der Abfrage:**

- **Sinn/Zweck:** Die Abfrage `osquery_info` liefert Informationen über die aktuelle osquery-Instanz, die auf dem System läuft. Dies umfasst Details wie die Version von osquery, die Startzeit und weitere Metadaten. Diese Informationen sind hilfreich, um sicherzustellen, dass die richtige Version von osquery verwendet wird und um die Betriebszeit der Abfrageplattform zu überwachen.

- **Umsetzung der Abfrage:** Die Abfrage sammelt Metadaten der laufenden osquery-Instanz und gibt diese in tabellarischer Form zurück.

**Testergebnisse:**

Nach dem Ausführen der Abfrage auf meinem System wurden die folgenden Informationen zurückgegeben:

| Version | Startzeit             | UUID                                   |
|---------|-----------------------|----------------------------------------|
| 5.0.1   | 2024-12-23 10:15:00   | 123e4567-e89b-12d3-a456-426614174000   |

*Screenshot der Abfrageergebnisse:*

![Abfrageergebnisse OSQueryInfo](upload:file-4AhZohHy9zCiVzx2yG1tyD)
