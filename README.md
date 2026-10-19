# Waveshare ESP32-S3-RS485-CAN-U

Technische Referenz zur Waveshare-Platine mit externer Antenne, isoliertem RS485 und CAN. Ziel ist eine nachvollziehbare Grundlage für weitere Projekte. Stand: 19.10.2026.

| Thema | Dokument |
| --- | --- |
| Modell, Speicher, Bauteile und Grenzen | [Hardware](docs/01-hardware.md) |
| Alle 20 Erweiterungspins, Anschlüsse und GPIO-Belegung | [Pinbelegung](docs/02-pinout.md) |
| Versorgung, Schutzbauteile, Isolation und Masseführung | [Elektrischer Aufbau](docs/03-power-and-isolation.md) |
| USB, Bootmodus, Arduino, ESP-IDF und ESPHome | [Programmierung](docs/04-firmware.md) |
| Tatsächlicher Erfassungsstand des eigenen Exemplars | [Eigenes Board](docs/05-own-board.md) |
| 14 Git-Repositories, Einsatzzweck und Lizenzhinweise | [Git-Projekte](docs/06-git-projects.md) |
| Quellen, Herstellerdownloads und Datenblätter | [Quellenregister](sources/README.md) |
| Wiederverwendbare strukturierte Angaben | [Boarddaten](data/board.json), [Pinliste als CSV](data/pinout.csv) |

## Kurzreferenz

- Variante **U**, SKU **34781**: externe Antenne mit SMA-Anschluss. Standardvariante ohne U: SKU 32154, interne Antenne.
- Laut veröffentlichtem Schaltplan ESP32-S3R8 mit 8 MB OPI-PSRAM und separatem W25Q128JV-Flash mit 16 MB.
- RS485: TX GPIO17, RX GPIO18, Richtungssteuerung GPIO21.
- CAN: TX GPIO15, RX GPIO16; ein klassischer CAN-Bus, kein CAN FD.
- RTC PCF85063A: SDA GPIO39, SCL GPIO38, Interrupt GPIO40, I2C-Adresse 0x51.
- Versorgung: USB-C 5 V oder DC-Klemme 7–36 V; Erweiterungsheader mit **2,0 mm** Raster.

Nachweise: [Waveshare-Modellübersicht](https://docs.waveshare.net/ESP32-S3-RS485-CAN/), [Schaltplan](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Schematic.pdf), [Herstellerdemo](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Demo.zip).

## Aussagekraft

Die Angaben wurden aus Herstellerunterlagen, dem Schaltplan, ausgewählten Demoquelldateien und den verlinkten Originalprojekten erarbeitet. Die Zuordnung zu einer konkreten Platinenrevision des eigenen Exemplars ist noch offen. Änderungen an Firmware oder Bestückung wurden nicht vorgenommen.

Am eigenen Board sind laut Besitzer keine weiteren Geräte angeschlossen. Bus-, Sensor- und Gegenstellentests gehören nicht zu dieser Erfassung und sind keine Voraussetzung für die Dokumentation.

Originaldownloads und lokale Untersuchungsunterlagen werden getrennt aufbewahrt. Dieses Repository enthält eigene technische Zusammenfassungen, Quellenverweise und Herkunftsdaten. Es ist keine offizielle Waveshare-Veröffentlichung.
