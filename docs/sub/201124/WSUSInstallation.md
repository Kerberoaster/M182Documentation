
# LB2 - Installation, Konfiguration und Testing von WSUS (20%)

## Inhaltsverzeichnis
1. [Lernziele](#lernziele)
2. [Installationsanleitung und Konfiguration](#installationsanleitung-und-konfiguration)
3. [PowerShell-Cmdlets für WSUS](#powershell-cmdlets-für-wsus)
4. [Screenshots der Installation](#screenshots-der-installation)

---

## 1. Lernziele

- WSUS-Server auf einem Domain Controller einrichten und konfigurieren.
- Windows-Clients so konfigurieren, dass sie Updates vom WSUS-Server erhalten.
- Update-Funktionalität testen und sicherstellen.

---

## 2. Installationsanleitung und Konfiguration

### **Wofür wird WSUS verwendet?**
Der WSUS-Dienst (Windows Server Update Services) ermöglicht es, Updates zentral zu verwalten und zu verteilen. Administratoren können kontrollieren, welche Updates genehmigt und an welche Geräte in der Organisation ausgeliefert werden. Dies verbessert die Sicherheit und reduziert den Bandbreitenverbrauch.

### **Alternativen zu WSUS**
- **Microsoft Endpoint Configuration Manager (MECM):** Bietet erweiterte Verwaltungsfunktionen für große Netzwerke.
- **Windows Update for Business:** Geeignet für Organisationen, die Cloud-basierte Update-Management-Tools bevorzugen.

---

### **WSUS-Server auf Domain Controller einrichten**

1. **WSUS-Rolle installieren:**
   - Server Manager öffnen und zu **Verwalten > Rollen und Features hinzufügen** navigieren.
   - Die Rolle **Windows Server Update Services** auswählen.
   - Bei der Speicherortauswahl eine **zusätzliche virtuelle Festplatte (mindestens 30 GB)** auswählen, um die Updates zu speichern.
   - Installation starten und abschließen.

2. **WSUS nach der Installation konfigurieren:**
   - Server Manager öffnen und zu **Tools > Windows Server Update Services** navigieren.
   - Synchronisationseinstellungen festlegen:
     - Produkte und Klassifikationen: Windows 11 und Server 2019.
     - Sprache: Deutsch und Englisch.
   - Synchronisation starten.

3. **Gruppenrichtlinie (GPO) für Updates erstellen:**
   - Gruppenrichtlinienverwaltung öffnen und zu **Domain > Gruppenrichtlinienobjekte** navigieren.
   - Neue GPO erstellen und bearbeiten:
     - Zu **Computerkonfiguration > Administrative Vorlagen > Windows-Komponenten > Windows Update** navigieren.
     - Die WSUS-Serveradresse eintragen: `http://192.168.60.102`.
   - GPO mit der Organisationseinheit (OU) der Windows-Clients verknüpfen.

---

### **Windows-Client konfigurieren**

1. **Übernahme der GPO prüfen:**
   - Eingabeaufforderung auf der Windows 11-VM öffnen und `gpresult /r` ausführen.
   - Sicherstellen, dass die WSUS-GPO angewendet wurde.

2. **Update-Funktionalität testen:**
   - Windows-Update-Einstellungen öffnen und prüfen, ob Updates vom WSUS-Server (`http://192.168.60.102`) bezogen werden.
   - Updates suchen und installieren.

---

## 3. PowerShell-Cmdlets für WSUS

### **Server-seitige Cmdlets**
1. **WSUS-Rolle installieren**
   ```powershell
   Install-WindowsFeature -Name UpdateServices, UpdateServices-WidDB, UpdateServices-RSAT -IncludeManagementTools
   ```

2. **WSUS-Synchronisation konfigurieren**
   ```powershell
   Set-WSUSServerSynchronization -SyncFromMicrosoftUpdate
   ```

3. **Updates im WSUS abrufen**
   ```powershell
   Get-WSUSUpdate -Classification All -Approval All
   ```

4. **Updates genehmigen**
   ```powershell
   Approve-WSUSUpdate -Update (Get-WSUSUpdate -Title "Update Title") -TargetGroupName "All Computers"
   ```

5. **WSUS-Bereinigung durchführen**
   ```powershell
   Invoke-WsusServerCleanup -CleanupObsoleteUpdates
   ```

6. **Computergruppen erstellen**
   ```powershell
   Add-WsusComputerTargetGroup -Name "TestGroup"
   ```

---

### **Client-seitige Cmdlets**
1. **WSUS-Server für den Client konfigurieren**
   ```powershell
   Set-ItemProperty -Path "HKLM:\Software\Policies\Microsoft\Windows\WindowsUpdate" -Name "WUServer" -Value "http://192.168.60.102"
   Set-ItemProperty -Path "HKLM:\Software\Policies\Microsoft\Windows\WindowsUpdate" -Name "WUStatusServer" -Value "http://192.168.60.102"
   ```

2. **Updates auf dem Client installieren**
   ```powershell
   Install-Module -Name PSWindowsUpdate
   Invoke-WUInstall -AcceptAll -Verbose
   ```

3. **Status der Updates prüfen**
   ```powershell
   Get-WindowsUpdateLog
   ```

---

## 4. Screenshots der Installation

### **WSUS-Server Installation**
- **Screenshot 1:** Navigieren zu **Rollen und Features hinzufügen**.
  *(Platzhalter für Screenshot der Auswahlseite einfügen)*
- **Screenshot 2:** Auswahl der Rolle **Windows Server Update Services**.
  *(Platzhalter für Screenshot der Rollenauswahl einfügen)*
- **Screenshot 3:** Auswahl des Speicherortes für Updates.
  *(Platzhalter für Screenshot der Speicherortauswahl einfügen)*

### **WSUS-Konfiguration**
- **Screenshot 4:** Synchronisationseinstellungen in der WSUS-Konsole.
  *(Platzhalter für Screenshot der WSUS-Einstellungen einfügen)*
- **Screenshot 5:** Synchronisation der Updates starten.
  *(Platzhalter für Screenshot der Synchronisation einfügen)*

### **GPO und Client-Testing**
- **Screenshot 6:** WSUS-Serveradresse in der GPO konfigurieren.
  *(Platzhalter für Screenshot der GPO-Einstellungen einfügen)*
- **Screenshot 7:** Ergebnis von `gpresult /r` auf der Windows-Client-VM.
  *(Platzhalter für Screenshot der GPO-Anwendung einfügen)*
- **Screenshot 8:** Updates erfolgreich vom WSUS-Server installiert.
  *(Platzhalter für Screenshot der Windows-Update-Seite einfügen)*

---

## Fazit

Die Dokumentation zeigt die erfolgreiche Einrichtung eines WSUS-Servers sowie die Konfiguration der Clients. Die PowerShell-Cmdlets ermöglichen eine flexible und automatisierte Verwaltung. Die Screenshots unterstützen die Nachvollziehbarkeit der einzelnen Schritte.
