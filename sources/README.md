# Quellenregister

Stand: 19.10.2026. Die Beschreibungen sind eigene technische Zusammenfassungen. Herstellerzeichnungen, Datenblatt-PDFs, Demo-ZIPs und fremde README-Dateien werden verlinkt; sie werden nicht pauschal unter einer eigenen Lizenz weiterveröffentlicht.

## Boardhersteller

| ID | Quelle | Verwendung |
| --- | --- | --- |
| WS-MODEL | [Modellfamilie und U-Variante](https://docs.waveshare.net/ESP32-S3-RS485-CAN/) | SKU, Anschlüsse, mechanische Angaben |
| WS-PRODUCT | [Internationale Produktseite](https://www.waveshare.com/ESP32-S3-RS485-CAN.htm) | Variantenvergleich und SMA-Antenne; Webabruf kann durch 403 eingeschränkt sein |
| WS-SCHEMATIC | [Herstellerschaltplan als PDF](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Schematic.pdf) | Pins, Bauteile, Versorgungs- und Isolationspfade; lokal visuell gelesen |
| WS-DEMO | [Demo-ZIP](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Demo.zip) | Pins, UART-Modus, CAN-Defaults, RTC und Dateistruktur |
| WS-ARDUINO | [Arduino-Anleitung](https://docs.waveshare.net/ESP32-S3-RS485-CAN/Arduino/) | Dokumentierte Kern-/Bibliotheksstände |
| WS-SETTINGS | [Verlinkte Arduino-Konfiguration](https://www.waveshare.com/w/upload/e/e9/ESP32-S3-16MB-OPI-Parameters.png) | Flash, PSRAM, Partitionierung und USB-Einstellungen |
| WS-FAQ | [FAQ](https://docs.waveshare.net/ESP32-S3-RS485-CAN/FAQ/) | 2-mm-Raster, Bitraten und manueller Downloadmodus |
| WS-FLASH | [Hersteller-Flashingbeschreibung](https://docs.waveshare.net/ESP32-S3-RS485-CAN/Firmware-Flashing/) | Offset der vollständigen Hersteller-BIN |

Dateigrößen und SHA256-Werte der drei lokal gesicherten Originaldownloads stehen in [manufacturer-downloads.json](manufacturer-downloads.json). Eine Prüfsumme fixiert die bezogene Datei; sie ist weder Herstellerunterschrift noch Kompatibilitätsnachweis.

## Bauteile und Frameworks

| Hersteller | Primärquelle | Bedeutung |
| --- | --- | --- |
| Espressif | [ESP32-S3 Series Datasheet](https://documentation.espressif.com/esp32-s3_datasheet_en.pdf) | SoC, R8-PSRAM, Pinmux und Strapping |
| Espressif | [TWAI für ESP32-S3, IDF 5.5.2](https://docs.espressif.com/projects/esp-idf/en/v5.5.2/esp32s3/api-reference/peripherals/twai.html) | Ein CAN-Controller, klassisches CAN und Betriebsarten |
| NXP | [TJA1051-Datenblatt](https://www.nxp.com/docs/en/data-sheet/TJA1051.pdf) | CAN-Transceiver; Controllergrenze bleibt separat |
| NXP | [PCF85063A-Datenblatt](https://www.nxp.com/docs/en/data-sheet/PCF85063A.pdf) | RTC und I2C |
| MaxLinear | [SP3485-Produkt und Datenblattzugang](https://www.maxlinear.com/product/interface/serial-transceivers/rs-485-rs-422/sp3485) | RS485-Transceiver |
| Winbond | [W25Q128JV-Dokumentationssuche](https://www.winbond.com/hq/support/documentation/?__locale=en&pno=W25Q128JV) | Weiterführender Herstellerzugang zum Flash; Modell im Schaltplan gelesen |
| XLSEMI | [XL1509-Datenblatt](https://www.xlsemi.com/datasheet/XL1509-EN.pdf) | Versorgungsklemmen-Regler; nicht mit Gesamtboardfreigabe verwechseln |
| ESPHome | [UART](https://esphome.io/components/uart/), [CAN](https://esphome.io/components/canbus/) | Softwarekonfiguration im jeweiligen Versionsstand |

## Git-Repositories

[repositories.json](repositories.json) hält für 14 öffentliche Repositories vollständige Commit-SHAs, unveränderliche README-Links und API-Metadaten fest. Die bewertete Übersicht steht in [Git-Projekte](../docs/06-git-projects.md). Einzelne Lizenzdateien wurden zusätzlich lokal nachgesehen; die API-Kennung NOASSERTION wird nicht mit fehlendem Lizenztext gleichgesetzt.

## Offene Nachweise

Es liegt keine vollständige Fertigungs-BOM des U-Exemplars, keine bestätigte eigene PCB-Revision, kein Messbericht und kein Gesamtboard-Isolationsprüfbericht vor. Solche Angaben werden nicht durch Einzelchipdaten oder Aussagen anderer Hobbyprojekte ersetzt.
