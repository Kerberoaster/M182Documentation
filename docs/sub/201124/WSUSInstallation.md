# LB2 - Installation, Konfiguration und Testing von WSUS (20%)

## Inhaltsverzeichnis
1. [Lernziele](#lernziele)
2. [Installationsanleitung und Konfiguration](#installationsanleitung-und-konfiguration)
3. [Screenshots der Installation](#screenshots-der-installation)

---

## 1. Lernziele

- WSUS-Server auf einem Domain Controller einrichten und konfigurieren.
- Windows-Clients so konfigurieren, dass sie Updates vom WSUS-Server erhalten.
- Update-Funktionalität testen und sicherstellen.

---

## 2. Installationsanleitung und Konfiguration

### WSUS-Server auf Domain Controller einrichten

1. **WSUS-Rolle installieren:**
   - Server Manager öffnen und zu **Verwalten > Rollen und Features hinzufügen** navigieren.
   - Die Rolle **Windows Server Update Services** auswählen.
   - **Wählen Sie Speicherort:** Speicher für Updates auf eine zusätzliche virtuelle Festplatte (mindestens 30 GB) legen.
   - Installation starten und abschließen.

   _Platzhalter für Screenshot: Rollen und Features hinzufügen_

2. **WSUS nach der Installation konfigurieren:**
   - Server Manager öffnen und zu **Tools > Windows Server Update Services** navigieren.
   - Synchronisationseinstellungen festlegen:
     - **Produkte und Klassifikationen:** Windows 11 und Server 2019 auswählen.
     - **Sprache:** Deutsch und Englisch.
   - Synchronisation starten.

   _Platzhalter für Screenshot: Synchronisationseinstellungen_

3. **Gruppenrichtlinie (GPO) für Updates erstellen:**
   - Gruppenrichtlinienverwaltung öffnen und zu **Domain > Gruppenrichtlinienobjekte** navigieren.
   - Eine neue GPO erstellen und bearbeiten:
     - Zu **Computerkonfiguration > Administrative Vorlagen > Windows-Komponenten > Windows Update** navigieren.
     - Die WSUS-Serveradresse einstellen: `http://192.168.60.102`.
   - GPO mit der Organisationseinheit (OU) der Windows-Clients verknüpfen.

   _Platzhalter für Screenshot: GPO-Einstellungen_

---

### Windows-Client konfigurieren

1. **Übernahme der GPO prüfen:**
   - Eingabeaufforderung auf der Windows 11-VM öffnen und `gpresult /r` ausführen.
   - Überprüfen, ob die WSUS-GPO angewendet wurde.

   _Platzhalter für Screenshot: GPO-Anwendung_

2. **Update-Funktionalität testen:**
   - Windows-Update-Einstellungen öffnen und überprüfen, ob die Updates vom WSUS-Server (`http://192.168.60.102`) bezogen werden.
   - Updates suchen und installieren.

   _Platzhalter für Screenshot: Windows-Update-Einstellungen_

---

## 3. Screenshots der Installation

### WSUS-Server Installation
- **Screenshot 1:** Navigieren zu **Rollen und Features hinzufügen**.
- **Screenshot 2:** Auswahl der Rolle **Windows Server Update Services**.
- **Screenshot 3:** Auswahl des Speicherortes für Updates.

### WSUS-Konfiguration
- **Screenshot 4:** Synchronisationseinstellungen in der WSUS-Konsole.
- **Screenshot 5:** Synchronisation der Updates starten.

### GPO und Client-Testing
- **Screenshot 6:** WSUS-Serveradresse in der GPO konfigurieren.
- **Screenshot 7:** Ergebnis von `gpresult /r` auf der Windows-Client-VM.
- **Screenshot 8:** Updates erfolgreich vom WSUS-Server installiert.
- **Screenshot 9:** Mögkiche Reports von WSUS-Server.

---


