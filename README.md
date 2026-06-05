# STM32 Zustandsmaschine
 
Eine modulare, in C implementierte Zustandsmaschine für STM32-Mikrocontroller,
ausgelegt für den Einsatz in eingebetteten Systemen – insbesondere im Bereich
Batteriemanagement (BMS) für Fahrzeuganwendungen.
 
## Beschreibung
 
Dieses Projekt stellt eine vollständige Zustandsmaschine zur Steuerung des
Programmablaufs auf einem STM32-Mikrocontroller bereit. Die Bibliothek verwaltet
alle relevanten Betriebszustände eines Fahrzeugs – vom Startvorgang über den
Fahrbetrieb bis zum kontrollierten Abschalten – und integriert dabei ein robustes
Fehler- und Warnungsmanagement.
 
Jeder Zustand wird zusammen mit dem aktuellen Fehlerstatus in einem einzigen
`uint8_t`-Register kodiert. Dadurch lassen sich Zustand und Diagnose effizient
und ressourcenschonend gleichzeitig abfragen.
 
## Zustände
 
Die Zustandsmaschine durchläuft folgende Zustände:
 
| Zustand        | Wert | Beschreibung                                      |
|----------------|------|---------------------------------------------------|
| `Start`        | 0    | Systemstart, Initialisierung                      |
| `Ready`        | 1    | System gestartet und betriebsbereit               |
| `KL15`         | 2    | Zündung aktiv (KL15)                              |
| `Anlassen`     | 3    | Anlasser betätigt                                 |
| `Precharge`    | 4    | Vorladung des Fahrzeugsystems                     |
| `ReadyToDrive` | 5    | System bereit für den Fahrbetrieb                 |
| `Drive`        | 6    | Fahrzeug im Fahrmodus                             |
| `Standby`      | 7    | Abschaltvorgang eingeleitet (Zeitverzögerung)     |
| `Ausschalten`  | 8    | System wird vollständig abgeschaltet              |
| `Laden`        | 9    | Lademodus aktiv                                   |
 
## Fehlerstatus
 
Zusätzlich zum Betriebszustand wird ein Fehlerstatus verwaltet:
 
| Status          | Wert | Beschreibung                   |
|-----------------|------|--------------------------------|
| `StateNormal`   | 0x10 | Normalbetrieb, kein Fehler     |
| `StateWarning`  | 0x20 | Warnung aktiv                  |
| `StateError`    | 0x40 | Fehler aktiv                   |
| `CriticalError` | 0x80 | Kritischer Fehler              |
 
### Fehlerverhalten
 
- **Warnungen** werden für mindestens **30 Sekunden** gespeichert, auch wenn die
Ursache zwischenzeitlich nicht mehr vorliegt.
- **Fehler** werden für mindestens **5 Minuten** gespeichert, bevor sie zurückgesetzt
werden können.
- **Kritische Fehler** können nicht automatisch zurückgesetzt werden.
Dieses Verhalten verhindert, dass kurzzeitig auftretende Probleme unbemerkt bleiben.
 
## API
 
```c
void setState(uint8_t state);        // Betriebszustand setzen
void setStatus(uint8_t status);      // Fehlerstatus setzen
uint32_t getLongError(void);         // Langzeit-Fehler abfragen
uint32_t getLongWarning(void);       // Langzeit-Warnung abfragen
```
 
## Verwendung
 
1. `statemaschine.h` und `statemaschine.c` in das STM32-Projekt einbinden.
2. Die globale Variable `Main_Statemaschine` steht nach dem Start im Zustand `Start`
mit dem Status `StateNormal` bereit.
3. Zustand und Fehlerstatus können jederzeit über die bereitgestellten Funktionen
gesetzt und abgefragt werden.
4. Jeder Zustandswechsel wird automatisch über UART ausgegeben (erfordert `basicuart.h`).

## Abhängigkeiten
 
- `main.h` – STM32 HAL
- [`basicuart.h`](https://github.com/Diveturtle93/STM32_Basicuart) – UART-Sende- und Empfangsfunktionen
- `millis.h` – Zeitbasis für Fehler-Timeouts
## Lizenz
 
Dieses Projekt steht unter der [GPL-3.0 Lizenz](LICENSE).
