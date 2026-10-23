# Changelog

Alle nennenswerten Änderungen an diesem Projekt werden hier dokumentiert.
Format nach [Keep a Changelog](https://keepachangelog.com/de/1.1.0/),
Versionierung nach [Semantic Versioning](https://semver.org/lang/de/).

## [Unveröffentlicht]

### Hinzugefügt
- `setState()` setzt einen der Betriebszustände `Ready` bis `Laden` und gibt jeden Zustandswechsel über UART aus.
- `setStatus()` setzt den Fehlerstatus `StateNormal`, `StateWarning`, `StateError` oder `CriticalError`.
- Ein Fehler wird frühestens 5 min nach seinem letzten Auftreten zur Warnung zurückgestuft, eine Warnung frühestens 30 s danach zu Normal; ein kritischer Fehler bleibt bestehen.
- Ein ungültiger Statuswert setzt einen kritischen Fehler und wird über UART gemeldet.
- Enum `states` mit zehn Betriebszuständen und vier Fehlerstatus.
- Union `Statemaschine`, die Zustand und Fehlerstatus in einem Byte zusammenfasst, und die globale Variable `Main_Statemaschine`, die im Zustand `Start` mit `StateNormal` beginnt.
- Konstanten `ERROR_RESET`, `WARNING_RESET` und `STANDBYTIME`.
- Versions-Defines `STATEMASCHINE_MAJOR`, `STATEMASCHINE_MINOR`, `STATEMASCHINE_PATCH` und `STATEMASCHINE_DEV` in `statemaschine.h`.
- Debug-Symbol `DEBUG_STATEMASCHINE`, das bei gesetztem `DEBUG` definiert wird.
- README mit Beschreibung, Konfigurationsparametern, Zuständen, Fehlerverhalten, API und Verwendung.
- Lizenz GPL-3.0.
