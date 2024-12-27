# Dokumentation: Winlogbeat Setup

## 1.2 Aufträge und Bewertungskriterien

### **Recherche - Winlogbeat **

**Was ist Winlogbeat?**
Winlogbeat ist ein leichtgewichtiger Datenversender (Data Shipper), der speziell für die Übertragung von Windows-Ereignisprotokolldaten an ein Elastic Stack-System (ELK) entwickelt wurde. Es sammelt und sendet Protokolle von verschiedenen Windows-Ereignisquellen, wie dem Sicherheitsprotokoll oder dem Systemprotokoll, an Elasticsearch zur weiteren Analyse.

**Wofür wird es verwendet?**
Winlogbeat wird verwendet, um Windows-Event-Logs wie System-, Sicherheits- und Anwendungsereignisse effizient zu sammeln und an Elasticsearch zu übertragen. Diese Daten können dann für Sicherheitsanalysen, Systemüberwachung und Compliance-Zwecke genutzt werden.

**Aktuellste Version von Winlogbeat:**
Die aktuellste Version von Winlogbeat zum Zeitpunkt dieser Dokumentation ist Version 8.16.1 (siehe Konfiguration).

---

### **Installation - Winlogbeat **

#### Schritte zur Installation:
1. **Download:**
   Die Installationsdateien können direkt von der offiziellen Elastic-Website heruntergeladen werden

2. **Extraktion:**
   Die heruntergeladene ZIP-Datei wird in ein Verzeichnis entpackt (z. B. `C:\Program Files\Winlogbeat`).

3. **Konfiguration:**
   - Das Konfigurationsfile `winlogbeat.yml` anpassen (Details siehe unten).
   - Benutzername und Passwort für Kibana und Elasticsearch eintragen.

![Bild](/../_Images/winlogeat1.png)
![Bild](/../_Images/winlogbeat2.png)
![Bild](/../_Images/winlogbeat3.png)

4. **Service-Installation:**
   Winlogbeat als Service installieren:
   ```powershell
   .\install-service-winlogbeat.ps1
   ```

5. **Service starten:**
   Winlogbeat als Service starten:
   ```powershell
   Start-Service winlogbeat
   ```

![Bild](/../_Images/winlogbeat4.png)

6. **Validierung:**
   Kibana aufrufen unter `http://192.168.60.105:5601`.
   Zum Bereich "Discover" navigieren.

### **Dokumentation / Testing - Winlogbeat **

#### Erfolg der Installation:
Die Installation war erfolgreich, wie durch die Logs in Kibana ersichtlich ist:
![Bild](/../_Images/winlogbeat5.png)

#### Wichtige Teile der Konfiguration:
1. **Elasticsearch-Einstellungen:**
   ```yaml
   output.elasticsearch:
     hosts: ["192.168.60.105:9200"]
     username: "vagrant"
     password: "vagrant"
   ```

2. **Kibana-Einstellungen:**
   ```yaml
   setup.kibana:
     host: "192.168.60.105:5601"
     username: "vagrant"
     password: "vagrant"
   ```

3. **Event Logs Konfiguration:**
   ```yaml
   winlogbeat.event_logs:
     - name: Application
       ignore_older: 72h

     - name: System

     - name: Security

     - name: Microsoft-Windows-Sysmon/Operational

     - name: Windows PowerShell
       event_id: 400, 403, 600, 800

     - name: Microsoft-Windows-PowerShell/Operational
       event_id: 4103, 4104, 4105, 4106
   