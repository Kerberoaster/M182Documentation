# Dokumentation: Security Dashboards und Rules

## 1.3 Aufträge und Bewertungskriterien

### **Recherche - Security Dashboards/Rules (0-4P)**

#### Aufbau von Security Rules:
Security Rules in Elastic dienen dazu, verdächtige Aktivitäten in den Logs zu erkennen und automatisch darauf zu reagieren. Jede Regel ist wie folgt aufgebaut:

1. **Allgemeine Informationen:**
   - **Name der Regel**: Gibt den Zweck der Regel an.
   - **Beschreibung**: Beschreibt, was die Regel überwacht.
   - **Schweregrad (Severity)**: Zeigt die potenzielle Auswirkung eines Ereignisses an.
   - **Risikowert (Risk Score)**: Ein numerischer Wert, der die Wichtigkeit angibt.

2. **Definition:**
   - **Index Patterns:** Bestimmt, welche Logs die Regel analysiert.
   - **Custom Query:** Definiert die Abfragen, die ausgeführt werden, um verdächtige Aktivitäten zu identifizieren.
   - **Rule Type:** Gibt an, wie die Regel implementiert ist (z. B. Event Correlation).

3. **Zeitplan:**
   - **Runs every:** Gibt das Intervall an, in dem die Regel ausgeführt wird.
   - **Additional look-back time:** Legt fest, wie weit in die Vergangenheit die Logs überprüft werden.

#### Beispiel-Screenshot zum Aufbau:
![Aufbau Security Rule](upload:file-4AhZohHy9zCiVzx2yG1tyD)

---

### **Testing und Doku (0-8P)**

#### Beispiel-Regel: "Suspicious Cmd Execution via WMI"
Diese Regel identifiziert verdächtige Befehlsausführungen über die Windows Management Instrumentation (WMI) auf einem Remote-Host. Dies kann auf laterale Bewegungen eines Angreifers hinweisen.

- **Name der Regel:** Suspicious Cmd Execution via WMI
- **Severity:** Medium
- **Risk Score:** 47
- **Custom Query:**
  ```
  process where event.type in ("start", "process_started") and
  process.parent.name : "WmiPrvSE.exe" and process.name : "cmd.exe" and
  process.args : "\\127.0.0.1\\*" and process.args : ("2>&1", "1>")
  ```
- **Zeitplan:** Läuft alle 5 Minuten mit einem zusätzlichen Look-back von 4 Minuten.

#### Versuch zur Alert-Generierung:

Mit zwei verschiedenen Regeln habe ich versucht, Alerts zu generieren. Die Logs konnten erfolgreich gefunden werden, jedoch wurde kein Alert ausgelöst. Nach 50 Minuten des Testens habe ich den Versuch abgebrochen.

**Versuchte Alerts:**
1. Persistent Scripts in the Startup Directory
2. Suspicious Cmd Execution via WMI

**Beweis für den Versuch:**
- Screenshot der Logs:
  ![Logs WMI Rule](upload:file-4AhZohHy9zCiVzx2yG1tyD)
- Screenshot der Settings für die getesteten Regeln:
  ![Regel Einstellungen WMI](upload:file-4AhZohHy9zCiVzx2yG1tyD)

---

Die Dokumentation zeigt, dass die Regeln korrekt definiert wurden, jedoch der Alert nicht ausgelöst wurde, obwohl die zugrunde liegenden Logs vorhanden waren. Weitere Tests oder Anpassungen könnten erforderlich sein, um die Ursache zu identifizieren.
