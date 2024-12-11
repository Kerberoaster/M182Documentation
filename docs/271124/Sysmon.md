
# LB2 - Sysmon: Beschreibung, Installation, Konfiguration (15%)

## Inhaltsverzeichnis
1. [Lernziele](#lernziele)
2. [Recherche zu Sysmon](#recherche-zu-sysmon)
3. [Installation von Sysmon](#installation-von-sysmon)
4. [Konfiguration von Sysmon](#konfiguration-von-sysmon)
5. [Fazit](#fazit)

---

## 1. Lernziele

- **Beschreibung von Sysmon:** Verstehen, wofür Sysmon verwendet wird.
- **Manuelle Installation:** Sysmon auf Windows-Systemen installieren können.
- **Automatisierte Installation:** Installation und Einrichtung mittels Skripten durchführen.
- **Konfiguration:** Sysmon mithilfe eines Konfigurationsfiles anpassen.

---

## 2. Recherche zu Sysmon

### Was ist Sysmon?
Sysmon (System Monitor) ist ein Tool aus der Sysinternals-Suite von Microsoft, das detaillierte Ereignisse zu Prozessen, Netzwerkverbindungen und Änderungen an der Dateisystemstruktur aufzeichnet. Es wird häufig für Sicherheits- und Forensikzwecke eingesetzt.

### Wofür wird Sysmon verwendet?
- Überwachung verdächtiger Prozesse und Netzwerkaktivitäten.
- Sammlung detaillierter Systemereignisse für Forensik und Troubleshooting.
- Unterstützung bei der Bedrohungserkennung in Kombination mit SIEM-Systemen.

### Aktuellste Version von Sysmon
Die aktuellste Version, die auf dem System verwendet wird, ist **v15.15**.

### Beispielanwendungen/Use-Cases:
- **Forensische Analyse:** Erkennung bösartiger Aktivitäten, z. B. durch Prozessüberwachung.
- **Bedrohungserkennung:** Monitoring von anomalen Netzwerkaktivitäten.
- **Systemüberwachung:** Verfolgung von Dateiänderungen oder Registry-Zugriffen.

---

## 3. Installation von Sysmon

### Manuelle Installation
1. **Download von Sysmon:**
   - Lade die Sysmon-Binary (`Sysmon64.exe`) von der offiziellen Sysinternals-Website herunter: [live.sysinternals.com](https://live.sysinternals.com/).
2. **Installation starten:**
   - Führe in der Eingabeaufforderung oder PowerShell den Befehl aus:
     ```powershell
     .\Sysmon64.exe -accepteula -i C:\Path\To\ConfigFile.xml
     ```
     Der Parameter `-i` gibt das Konfigurationsfile an.
3. **Überprüfung:**
   - Stelle sicher, dass der Sysmon-Dienst läuft:
     ```powershell
     Get-Service -Name Sysmon64
     ```

### Automatisierte Installation
Die automatisierte Installation erfolgt mithilfe des folgenden PowerShell-Skripts (`install-sysinternals.ps1`):

```powershell
Write-Host "Installing SysInternals Tooling..."
$sysinternalsDir = "C:\Tools\Sysinternals"
$sysmonDir = "C:\ProgramData\Sysmon"

# Verzeichnis erstellen
If(!(test-path $sysinternalsDir)) {
  New-Item -ItemType Directory -Force -Path $sysinternalsDir
}

# Sysmon und andere Tools herunterladen
(New-Object System.Net.WebClient).DownloadFile('https://live.sysinternals.com/Sysmon64.exe', "$sysinternalsDir\Sysmon64.exe")

# Sysmon starten
Start-Process -FilePath "$sysinternalsDir\Sysmon64.exe" -ArgumentList "-accepteula -i $sysmonDir\sysmonConfig.xml"
```

---

## 4. Konfiguration von Sysmon

### Verwendetes Konfigurationsfile
Das Sysmon-Config-File, das auf dem System verwendet wird, befindet sich unter:  
`C:\ProgramData\Sysmon\sysmonConfig.xml`

### Struktur des Config-Files
Das Config-File nutzt Regeln zur Steuerung, welche Ereignisse erfasst werden. Es ist modular aufgebaut:
- **Hashing Algorithms:** SHA1, MD5, SHA256, IMPHASH.
- **Process Monitoring:** Überwachung bestimmter Prozesse wie `sc.exe`, `mshta.exe`.
- **Netzwerküberwachung:** Aufzeichnung von Verbindungen und DNS-Abfragen.
- **Erweiterte Filter:** Regeln für spezifische Bedrohungsszenarien (z. B. `ParentImage`, `CommandLine`).

### Beispielregel (aus der Config):
```xml
<RuleGroup name="ProcessCreate" groupRelation="or">
  <Rule name="Include All Executables">
    <Image condition="contains">sethc.exe</Image>
    <CommandLine condition="contains">-Embedding</CommandLine>
  </Rule>
</RuleGroup>
```

---

## 5. Fazit

Sysmon ist ein vielseitiges Werkzeug, das tiefgehende Einblicke in Systemaktivitäten ermöglicht. Die Kombination aus manueller und automatisierter Installation, gepaart mit einer gut durchdachten Konfiguration, erlaubt eine effiziente Überwachung und Forensik in komplexen Umgebungen.

---

### Platzhalter für Screenshots:
1. **Screenshot 1:** Download von Sysmon.
2. **Screenshot 2:** Manuelle Installation über PowerShell.
3. **Screenshot 3:** Automatisierte Installation via Script.
4. **Screenshot 4:** Config-File in der Verzeichnisstruktur.
5. **Screenshot 5:** Beispielregel im Config-File.
