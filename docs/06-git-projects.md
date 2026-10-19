# Git Quellen für Waveshare ESP32 S3 RS485 CAN U

Stand: 19.10.2026. Es wurden **14 öffentliche Repositories** mit festgehaltenem Commit und unveränderlichem README-Link erfasst. Die Tabelle bewertet dokumentierten Einsatzzweck und ausgewählte Konfigurationsdateien. Kein Repository wurde gebaut, ausgeführt oder auf das eigene Board übertragen. Die ausdrücklichen Boardangaben betreffen überwiegend ESP32-S3-RS485-CAN ohne separat getestete U-Variante.

## Hersteller und Frameworks

Eine öffentliche Herstellerquelle für das Demo ist das [Waveshare-ZIP](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Demo.zip). Der konkret geprüfte GitHub-Endpunkt `waveshareteam/ESP32-S3-RS485-CAN` lieferte 404. Daraus folgt nicht, dass es nirgendwo ein weiteres Herstellerrepository gibt.

| Repository | Nutzen | Snapshot | Lizenzstand |
| --- | --- | --- | --- |
| [espressif/esp-idf](https://github.com/espressif/esp-idf) | Offizielles Framework; UART, TWAI, Netzwerk und OTA | `383dda6c182e` | Apache-2.0 laut API; LICENSE gesichert |
| [espressif/arduino-esp32](https://github.com/espressif/arduino-esp32) | Offizieller Arduino-Core; Grundlage des Herstellerdemos | `bb00b32cb1ce` | LGPL-2.1 laut API; maßgebliche Dateilizenzen vor Übernahme prüfen |
| [esphome/esphome](https://github.com/esphome/esphome) | Konfiguration für Home Assistant und bestehende Geräteprotokolle | `85dd0f71b379` | Gemischt: C/C++-Laufzeit GPLv3, Python und übrige Teile MIT laut LICENSE |

Die Snapshots von `master` bzw. `dev` dienen der Quellenfixierung. Für einen Bau ist eine zum Projekt passende Releaseversion zu wählen.

## Konkrete Boardprojekte

| Repository | Aufgabe und Nutzen für Folgeprojekte | Snapshot | Lizenz und offene Punkte |
| --- | --- | --- | --- |
| [trailcurrentoss/TrailCurrentPicket](https://github.com/trailcurrentoss/TrailCurrentPicket) | ESP-IDF; Reedkontakte an Türen/Schränken, Zustände über CAN. Thematisch besonders passend zum Van. | `02b80307ad44` | MIT. Eigene CAN-Nachrichten und OTA-/Discovery-Umgebung. README nennt 2,54-mm-Header, Hersteller 2,0 mm: Mechanik vor Übernahme abgleichen. |
| [miq28/OpenELM32](https://github.com/miq28/OpenELM32) | ELM327-kompatibler CAN-Adapter über BLE, WLAN/TCP oder USB; auch GVRET/SavvyCAN beschrieben. | `cbffdc92884e` | MIT. Explizite Waveshare-Buildumgebung. `platformio.ini` verwendet `file://../pio/55.03.38-1`: ohne diese lokale Toolchain nicht unmittelbar reproduzierbar. Entwicklungsfirmware, kann CAN senden. |
| [Sleeper85/esphome-yambms](https://github.com/Sleeper85/esphome-yambms) | ESPHome; BMS-Daten und Kommunikation mit Wechselrichtern. Eigene Waveshare-Boarddokumentation samt GPIO21-Hinweis. | `e183d206bc3b` | GPL-3.0 laut API. Unterstützte Batterieprotokolle einzeln prüfen; nicht jedes BMS ist kompatibel. |
| [gongloo/TigoTell](https://github.com/gongloo/TigoTell) | RS485-Mithören zwischen Tigo CCA und TAP; Telemetrie, Weboberfläche, InfluxDB-Ausgabe. | `16a703ce8f20` | MIT. Für Tigo-Verkehr, kein allgemeiner Modbus-Logger. Ziel der Datenübertragung bei Wiederverwendung prüfen. |
| [DavidDeeds/Aurora-ESP32-Dashboard](https://github.com/DavidDeeds/Aurora-ESP32-Dashboard) | ESP-IDF; lokales Dashboard für Aurora/Power-One-Wechselrichter über RS485. RTC und Speicherung als Referenz. | `849d1c429a7f` | MIT. Autor nennt Boardtests; hier nicht nachgeprüft. Optionale PVOutput-Übertragung bei lokalem Einsatz bewusst deaktivieren. |
| [vincentmakes/ComfoSense-Touch](https://github.com/vincentmakes/ComfoSense-Touch) | Zehnder-ComfoAir/CAN und MQTT-Bridge; Hersteller verlinkt das Projekt. | `0b27bac20e5c` | LICENSE enthält MIT-Text und einen auffälligen JSON-Anhang; API meldet NOASSERTION. Vor Codeübernahme klären. README verlangt für die Bridge Pinanpassungen. |
| [vincentmakes/canbus_hoval](https://github.com/vincentmakes/canbus_hoval) | Hoval TopTronic E über CAN an Home Assistant; ebenfalls vom Hersteller verlinkt. | `69a9e6696397` | `licence.md`: CC BY-NC-SA 4.0. Wegen NonCommercial keine allgemeine Grundlage für kommerzielle Übernahme. |
| [zampix1/midea-mitsui-pdc-esphome](https://github.com/zampix1/midea-mitsui-pdc-esphome) | ESPHome/Modbus RTU für Midea-/Mitsui-Wärmepumpen. Autor beschreibt Erprobung auf einem passenden Gerät. | `bf02a961a241` | MIT. Registerplan und Gerätevariante vor aktiven Schreibfunktionen prüfen. |
| [espforge/esphome-waterfurnace](https://github.com/espforge/esphome-waterfurnace) | ESPHome für WaterFurnace-Aurora-Wärmepumpen; dokumentiert TX17/RX18/Flow21. | `8f1ec620730c` | API liefert keine Lizenz. Als Referenz erfasst; Freigabe zur Codeübernahme nicht nachgewiesen. |

## Weitere Treffer mit engem Zielgebiet

| Repository | Einordnung | Snapshot | Lizenzstand |
| --- | --- | --- | --- |
| [JordanzhaoD/waveshare-single-can-firmware](https://github.com/JordanzhaoD/waveshare-single-can-firmware) | Vom Herstellerprofil aus auffindbarer Atlas-Single-CAN-Treffer. Tesla/FSD-bezogene CAN- und WebUI-Firmware mit aktiven Funktionen; kein neutraler CAN-Logger. | `75ac7e72c5fd` | GPL-3.0 laut API; LICENSE gesichert |
| [JordanzhaoD/ESP-IDF_waveshare_ESP32_S3_RS485_CAN](https://github.com/JordanzhaoD/ESP-IDF_waveshare_ESP32_S3_RS485_CAN) | Waveshare-Anpassung eines Fahrzeug-CAN-Dashboards einschließlich OTA und Gatewayfunktionen. Für allgemeine Projektübernahme zusätzliche, sachfremde Funktionen. | `aa9e2e64f6ff` | GPL-3.0 laut API; LICENSE gesichert |

Für einen eigenen allgemeinen CAN-Knoten ist eine kleine ESP-IDF-/Arduino-Grundlage überschaubarer als die Übernahme dieser spezialisierten Fahrzeugfunktionen. Das ist eine Empfehlung aus dem Funktionsumfang, kein Ergebnis eines Quellcodeaudits.

## Auswahl nach nächstem Projekt

- **Eigene Van-Sensoren:** Herstellerpins und ESP-IDF als Basis; Picket für Datenmodell und Sensoranbindung ansehen.
- **Home-Assistant-Sensorik:** ESPHome und das passende Geräteprotokoll; YamBMS nur bei tatsächlich passendem BMS.
- **CAN-Diagnose:** OpenELM32 näher prüfen, zunächst mit Simulator oder Prüfbus; lokale Toolchain-Abhängigkeit auflösen.
- **Spezielle Energie-/Heizungsgeräte:** Nur den Treffer zur konkreten Gerätefamilie vertiefen.

## Herkunft und Nachprüfbarkeit

[repositories.json](../sources/repositories.json) enthält vollständige Commit-SHAs, Branches, Commitdatum, API-Lizenzkennung, README-Pfade und unveränderliche GitHub-Links. Die hier präzisierten Lizenztexte wurden zusätzlich lokal eingesehen; NOASSERTION bedeutet lediglich, dass die API keine eindeutige SPDX-Zuordnung geliefert hat. Das ersetzt keine Prüfung der tatsächlich übernommenen Dateien und Abhängigkeiten.

Die verlinkten README-Dateien sind fremde Quellen. Darin beschriebene Upload-, Installations-, Flash- und Releasebefehle sind nicht Teil dieser Erfassung. Bei eigener Implementierung müssen Onlinefunktionen und Standardzugänge gesondert geprüft werden.
