
# LB2 - Beschreibung und Recherche der Software Sysmon (15%)

## 1. Lernziele

- Ich kann die Applikation Sysmon in eigenen Worten beschreiben.
- Ich kann Sysmon automatisiert installieren lassen.
- Ich kann Sysmon manuell installieren.
- Ich kann Sysmon nach Vorgaben konfigurieren.

---

## 2. Beschreibung von Sysmon

### Was ist Sysmon und wofür wird es verwendet?

Sysmon (System Monitor) ist ein Teil der Sysinternals-Tools von Microsoft, das detaillierte Ereignisse im Zusammenhang mit Prozessen, Netzwerkverbindungen und Änderungen an der Dateisystemstruktur überwacht. Es wird hauptsächlich für die Sicherheitsüberwachung und das Troubleshooting verwendet.

- **Anwendungen:**
  - Überwachung verdächtiger Aktivitäten auf Systemen.
  - Unterstützung bei der Forensik durch detaillierte Log-Ereignisse.
  - Erkennung von Angriffsmustern durch Analyse der Windows-Ereignisprotokolle.

- **Aktuellste Version:**
  Die aktuellste Version von Sysmon ist [Sysmon 15.0](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon) (Stand November 2024).

- **Beispiele für Use-Cases:**
  - Nachverfolgung von Malware-Aktivitäten durch Prozessüberwachung.
  - Überwachung von Änderungen an Registrierungsschlüsseln.
  - Logging von Netzwerkverbindungen für forensische Untersuchungen.

---

## 3. Installation von Sysmon

### Manuelle Installation von Sysmon

1. **Download:**
   - Die Sysmon-Executable kann direkt von der [Microsoft Sysinternals-Seite](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon) heruntergeladen werden.

2. **Installation:**
   - Öffne die Eingabeaufforderung mit Administratorrechten.
   - Führe den folgenden Befehl aus, um Sysmon mit einer Konfigurationsdatei zu installieren:
     ```
     sysmon.exe -accepteula -i <Pfad zur Config-Datei>
     ```
   - Beispiel:
     ```
     sysmon.exe -accepteula -i sysmonconfig.xml
     ```

3. **Prüfung der Installation:**
   - Nach der Installation können die Ereignisse über die Windows-Ereignisanzeige unter **Applications and Services Logs > Microsoft > Windows > Sysmon** überprüft werden.

---

### Automatisierte Installation von Sysmon in unserer Umgebung

In unserer Umgebung wurde Sysmon mithilfe des Scripts `scripts/install-sysinternals.ps1` installiert. Dieses Skript übernimmt folgende Aufgaben:

- Herunterladen der Sysmon-Executable.
- Installation von Sysmon mit einer Standardkonfigurationsdatei.
- Einrichtung eines geplanten Tasks für die regelmäßige Aktualisierung.

Ein Beispiel für die PowerShell-Befehle aus dem Skript:
```powershell
Invoke-WebRequest -Uri "https://download.sysinternals.com/files/Sysmon.zip" -OutFile "C:\Tools\Sysmon.zip"
Expand-Archive -Path "C:\Tools\Sysmon.zip" -DestinationPath "C:\Tools\Sysmon"
Start-Process -FilePath "C:\Tools\Sysmon\Sysmon.exe" -ArgumentList "-accepteula -i C:\Tools\SysmonConfig.xml"
```

---

## 4. Konfiguration von Sysmon

### Config-File in unserer Umgebung

Das Config-File für Sysmon in unserer Umgebung basiert auf Best-Practices und wurde angepasst, um gängige Sicherheitsanforderungen zu erfüllen.

- **Inhalt der Datei:**
  - Regeln für die Überwachung spezifischer Prozesse.
  - Definition von Ausnahmen für bekannte sichere Anwendungen.
  - Logging von Netzwerkverbindungen und Dateiänderungen.

Beispielauszug:
```xml
<Sysmon schemaversion="4.22">
  <EventFiltering>
    <ProcessCreate onmatch="include">
      <Image condition="contains">powershell.exe</Image>
    </ProcessCreate>
    <NetworkConnect onmatch="include" />
    <FileCreateTime onmatch="exclude">
      <Image condition="contains">C:\Windows\</Image>
    </FileCreateTime>
  </EventFiltering>
</Sysmon>
```

### Struktur des Config-Files

1. **Root-Element:** `<Sysmon>` definiert die Schema-Version.
2. **EventFiltering:** Enthält Regeln, um spezifische Ereignisse einzuschließen oder auszuschließen.
3. **Regeln:**
   - `<ProcessCreate>`: Überwachung von neu erstellten Prozessen.
   - `<NetworkConnect>`: Logging von Netzwerkverbindungen.
   - `<FileCreateTime>`: Erfassung von Änderungen an Datei-Zeitstempeln.

---

## 5. Fazit

Die Nutzung von Sysmon in unserer Umgebung ermöglicht eine tiefgreifende Überwachung und Erkennung von Sicherheitsvorfällen. Durch die automatisierte Installation und die Verwendung einer standardisierten Konfiguration wird sichergestellt, dass alle relevanten Ereignisse protokolliert werden. Das Config-File bietet Flexibilität, um Sysmon an unsere spezifischen Bedürfnisse anzupassen.
