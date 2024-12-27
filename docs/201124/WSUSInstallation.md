# Installation, Konfiguration und Testing von WSUS

## 1. Lernziele

- WSUS-Server auf einem Domain Controller einrichten und konfigurieren.
- Windows-Clients so konfigurieren, dass sie Updates vom WSUS-Server erhalten.
- Update-Funktionalität testen und sicherstellen.

---

### **Recherche, allgemein - WSUS**

**Wofür wird der WSUS-Dienst verwendet?**
Windows Server Update Services (WSUS) wird verwendet, um Updates zentral für Windows-Systeme in einer Organisation bereitzustellen. Administratoren können Updates genehmigen, ablehnen und die Installation überwachen, wodurch die Kontrolle über Update-Management-Prozesse erleichtert wird.

**Welche Alternativen gibt es für den WSUS-Dienst?**
1. **Microsoft Endpoint Configuration Manager (ehemals SCCM):** Erweiterte Verwaltungsmöglichkeiten für Updates und Anwendungen.
2. **Intune:** Cloud-basierte Verwaltungslösung für Updates und Geräte.
3. **Patch My PC:** Drittanbieter-Tool für die Verwaltung von Updates und Anwendungen.
4. **PDQ Deploy:** Tool für die Bereitstellung und Verwaltung von Software-Updates.

---

## **Installation Server - WSUS**

### Dokumentation der Installation

#### **WSUS-Rolle installieren:**
1. Server Manager öffnen und zu **Verwalten > Rollen und Features hinzufügen** navigieren.
2. Die Rolle **Windows Server Update Services** auswählen.
3. Speicherort für Updates angeben (zusätzliche virtuelle Festplatte mit mindestens 30 GB Speicher).
4. Installation starten und abschliessen.

#### **WSUS nach der Installation konfigurieren:**
1. WSUS starten und Synchronisationseinstellungen festlegen:
   - **Produkte und Klassifikationen:** Windows 11 auswählen.
   - **Sprache:** Deutsch und Englisch.
2. Synchronisation starten.

![WSUS Installation Screenshot](/../_Images/wsus1.png)
![WSUS Installation Screenshot](/../_Images/wsus2.png)
![WSUS Installation Screenshot](/../_Images/wsus3.png)
![WSUS Installation Screenshot](/../_Images/wsus4.png)
![WSUS Installation Screenshot](/../_Images/wsus5.png)
![WSUS Installation Screenshot](/../_Images/wsus6.png)

---

### **Dokumentation und Recherche der wichtigsten Optionen**

#### **Wichtige Optionen:**
1. **Speicherort für Updates:** Dedizierte Festplatte zur Trennung von Betriebssystemdaten und Update-Daten.
2. **Produkte und Klassifikationen:** Auswahl der benötigten Betriebssysteme und Software, um die Serverlast und den Speicherverbrauch zu minimieren.
3. **Synchronisationszeitplan:** Regelmässige Synchronisation zu Randzeiten, um die Netzwerkbelastung zu reduzieren.

**Begründung der Optionen:**
- Der Speicherort auf einer separaten Disk optimiert die Systemleistung.
- Die Auswahl von nur relevanten Updates reduziert Speicher- und Netzwerkbelastung.
- Ein geplanter Zeitplan verhindert Störungen im täglichen Betrieb.

---

## **Recherche, Powershell Server - WSUS**

**Welche Powershell-Cmdlets gibt es, um den WSUS-Dienst zu konfigurieren/steuern?**

1. `Get-WsusServer`: Verbindung zum WSUS-Server herstellen.
2. `Set-WsusServerSynchronization`: Synchronisationseinstellungen konfigurieren.
3. `Invoke-WsusServerCleanup`: Nicht mehr benötigte Updates entfernen.
4. `Get-WsusUpdate`: Liste der Updates anzeigen.
5. `Approve-WsusUpdate`: Updates zur Installation genehmigen.

---

## **Installation Client - WSUS**

### **Dokumentation der Konfiguration**

#### **WSUS-Client über Gruppenrichtlinien konfigurieren:**
1. Gruppenrichtlinienverwaltung öffnen und eine neue GPO erstellen.
2. Zu **Computerkonfiguration > Administrative Vorlagen > Windows-Komponenten > Windows Update** navigieren.
3. Die WSUS-Serveradresse eintragen:
   - **Intranet-Update-Service:** `http://192.168.60.102`
   - **Intranet-Statistikserver:** `http://192.168.60.102`
4. GPO mit der Organisationseinheit (OU) der Windows-Clients verknüpfen.

![WSUS Installation Screenshot](/../_Images/gpo1.png)
![WSUS Installation Screenshot](/../_Images/gpo2.png)
![WSUS Installation Screenshot](/../_Images/gp3.png)

### **Welche Powershell-Cmdlets gibt es, um den WSUS-Client zu konfigurieren/steuern?**
1. `Get-WindowsUpdateLog`: Zeigt Logs von Windows Update an.
2. `Get-WUInstall`: Installiert verfügbare Updates.
3. `Get-WUServiceManager`: Zeigt die verfügbaren Update-Dienste.
4. `Add-WUOfflineSync`: Konfiguriert einen Offline-Sync mit einem WSUS-Server.

---

## **Testing und Reporting - WSUS**

### **Testen der Update-Funktionalität:**
1. Überprüfen, ob die GPO auf den Windows-Clients angewendet wurde:
   - Mit `gpresult /h report.html` sicherstellen, dass die WSUS-GPO aktiv ist.
2. Windows Update auf dem Client öffnen und Updates suchen.
3. Sicherstellen, dass Updates vom WSUS-Server (`http://192.168.60.102`) bezogen werden.

#### **Testergebnisse:**
- Updates wurden erfolgreich erkannt und installiert.
- Über die Windows Update-Einstellungen war ersichtlich, dass die Updates vom WSUS-Server geladen wurden.

![Client-Update Screenshot](/../_Images/windows.png)

### **Welche Reports können aus dem WSUS über die GUI erstellt werden?**
1. **Update Status Summary:** Zeigt eine Übersicht über den Status aller Updates mit einer Seite pro Update.
2. **Update Detailed Status:** Detaillierte Informationen über den Status von Updates für alle Computer, dargestellt pro Update.
3. **Update Tabular Status:** Tabellarische Übersicht des Update-Status, geeignet für den Export in Tabellen.
4. **Update Tabular Status for Approved Updates:** Tabellarische Übersicht nur für genehmigte Updates.
5. **Computer Status Summary:** Übersicht des Update-Status pro Computer mit einer Seite pro Computer.
6. **Computer Detailed Status:** Detaillierte Informationen über den Update-Status pro Computer.
7. **Computer Tabular Status:** Tabellarische Übersicht des Computer-Status, geeignet für den Export.
8. **Computer Tabular Status for Approved Updates:** Tabellarische Übersicht des Computer-Status nur für genehmigte Updates.
9. **Synchronization Results:** Zeigt die Ergebnisse der letzten Synchronisation an.

![WSUS Reports Screenshot](/../_Images/reports.png)
