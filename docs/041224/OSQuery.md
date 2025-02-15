
# OSQuery Dokumentation

## 1. Lernziele
- Die Applikation OSQuery in eigenen Worten beschreiben.
- OSQuery manuell und automatisiert konfigurieren können.
- Die Konfiguration von OSQuery testen und dokumentieren.

---

## 2. Recherche - OSQuery

### Was ist OSQuery? Wofür wird es verwendet?
OSQuery ist ein Open-Source-Framework, das SQL-Abfragen zur Überwachung und Verwaltung von Systemen ermöglicht. Es erlaubt Benutzern, systeminterne Informationen (z. B. installierte Programme, laufende Prozesse, Netzwerkinformationen) in Form von SQL-Datenbanken abzufragen. OSQuery wird häufig für Sicherheitsüberwachung, Incident Response und IT-Compliance eingesetzt.

### Was ist die aktuellste Version von OSQuery?
Die aktuellste Version von OSQuery zum Zeitpunkt dieser Dokumentation ist 5.12.0.

### Was sind Beispiel-Anwendungen/Use-Cases bei welchen OSQuery helfen kann?
1. **Sicherheitsüberwachung**: Überwachung von Änderungen an Systemdateien und laufenden Prozessen.
2. **IT-Compliance**: Prüfung auf Einhaltung von Sicherheitsrichtlinien (z. B. installierte Softwareversionen).
3. **Incident Response**: Untersuchung von Sicherheitsvorfällen durch detaillierte Abfragen von Systemereignissen.

---

## 3. Konfiguration und Testing

### Dokumentation des Config-Files
Das auf unserem System installierte Config-File hat den folgenden Inhalt:

```json
{
  "options": {
    //"logger_path": "/var/log/osquery",
    //"disable_logging": "false",
    //"schedule_splay_percent": "10"
  },
  "schedule": {
    "system_info": {
      "query": "SELECT hostname, cpu_brand, physical_memory FROM system_info;",
      "interval": 3600
    }
  },
  "decorators": {
    "load": [
      "SELECT uuid AS host_uuid FROM system_info;",
      "SELECT user AS username FROM logged_in_users ORDER BY time DESC LIMIT 1;"
    ]
  },
  "packs": {
    // Add additional packs if required.
  },
  "feature_vectors": {
    "character_frequencies": [ /* Abgekürzt */ ]
  }
}
```

### Struktur des Config-Files
1. **Options**: Konfiguriert die grundlegenden Optionen wie Logging und Intervalle.
2. **Schedule**: Enthält Abfragen, die in definierten Intervallen ausgeführt werden.
3. **Decorators**: Führt Abfragen aus, die zusätzliche Metadaten zu allen anderen Abfragen hinzufügen.
4. **Packs**: Ermöglicht die Integration von vordefinierten Abfragelisten.
5. **Feature Vectors**: Unterstützt einfache statistische Analysen (derzeit nur auf Windows).

### Installation Testing von OSQuery

#### Installation:

![Bild](/../_Images/osquery1.png)
![Bild](/../_Images/osquery2.png)

Für den ersten Test wurde folgende Abfrage verwendet:
```sql
SELECT * FROM programs;
```
**Resultat:**
- Hostname: `dc.windomain.local`
- Es wurden 13 installierte Programme erkannt.

![Bild](/../_Images/osquery3.png)