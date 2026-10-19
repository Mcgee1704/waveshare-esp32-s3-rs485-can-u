# Hardware und Bauteile

## Modell eindeutig benennen

Die Herstellerfamilie heißt ESP32-S3-RS485-CAN. Die Variante **ESP32-S3-RS485-CAN-U** hat SKU **34781** und verwendet ab Werk die externe Antenne mit SMA-Anschluss. Die Standardausführung ohne U hat SKU **32154** und eine interne Antenne. Das U ist kein anderer ESP32-Prozessortyp. [Waveshare-Varianten](https://docs.waveshare.net/ESP32-S3-RS485-CAN/)

Die beiden Varianten werden im gemeinsamen Wiki und Ressourcenbereich beschrieben. Der veröffentlichte Schaltplan trägt den Familiennamen ohne U. Er ist daher eine Referenz für die Schaltung, aber kein Fotobeleg für die konkrete Revision oder Antennenbestückung eines einzelnen U-Geräts.

## Rechen- und Speicherarchitektur

| Element | Einordnung |
| --- | --- |
| ESP32-S3R8, U2 | SoC im QFN-Gehäuse; zwei Xtensa-LX7-Kerne mit bis zu 240 MHz |
| R8-Speicher | 8 MB Octal-SPI-PSRAM im Chip-Package, kein zusätzlicher 8-MB-Flash |
| W25Q128JVSI, U6 | Separater SPI-NOR-Flash im Schaltplan; 128 Mbit entsprechen 16 MiB |
| 40-MHz-Quarz, Y1 | Haupttakt des ESP32 |
| 32,768-kHz-Quarz, Y2 | Zeitbasis der separaten RTC |

Die Speicherbestückung in dieser Tabelle folgt aus dem Schaltplan und dem R8-Datenblatt. Herstellerbeschreibungen nennen den S3R8 als Standardbestückung mit möglichen kundenspezifischen Varianten. [Schaltplan](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Schematic.pdf), [Espressif-Datenblatt, Typentabelle](https://documentation.espressif.com/esp32-s3_datasheet_en.pdf)

Am eigenen Exemplar meldete esptool 5.3.0 am 19.10.2026 einen ESP32-S3 (QFN56), Revision v0.2, 8 MB integriertes PSRAM laut Chip-Package-Erkennung, einen 40-MHz-Quarz und 16 MB Flash. Die Flash-JEDEC-Kennung lautet Hersteller `0x20`, Gerät `0x4018`; Flashtyp und Spannung laut eFuse sind Quad mit vier Datenleitungen und 3,3 V. Die konkrete Flash-Bauteilbezeichnung wurde damit nicht bestimmt. Der Schaltplaneintrag W25Q128JVSI darf daher nicht als abgelesene Bestückung dieses Exemplars übernommen werden. Es erfolgte kein PSRAM-Funktionstest. [Individuelle Auslesung und Grenzen](05-own-board.md).

Das Board ist im Schaltplan mit einem ESP32-S3R8 und separatem Flash aufgebaut. Die gelegentliche Bezeichnung als ESP32-S3-WROOM-Modul in Drittunterlagen ist keine belastbare Beschreibung dieser gezeigten Platine.

## Funktionsbauteile im veröffentlichten Schaltplan

| Kennzeichen | Bauteil | Aufgabe |
| --- | --- | --- |
| U1 | MP1605GTF-Z | Lokaler 5-V-zu-3,3-V-Abwärtsregler für die Controllerseite |
| U2 | ESP32-S3R8 | Mikrocontroller und WLAN/BLE |
| U3 | B0505LS-1W (K-CUT) | Isolierter DC/DC-Wandler für die Busseite |
| U4 | ME6217C33M5G | 3,3-V-LDO auf der isolierten Seite |
| ohne U-Nummer im Schaltplansymbol | XL1509-ADJE1 | Abwärtsregler des Versorgungsklemmenpfads |
| U6 | W25Q128JVSI | Externer Flash |
| U7 | SP3485EN | RS485-Transceiver |
| U8 | TJA1051T/3/1J | CAN-Transceiver |
| U9 | π163E31 | Digitaler Isolator zwischen Controller- und Busseite |
| U10 | PCF85063AT | Echtzeituhr und Kalender |

Diese Tabelle beschreibt die lesbaren Schaltplansymbole, keine aus Fotos erstellte vollständige Fertigungsstückliste. Ein aus dem Bauteilnamen erkennbarer Leistungswert ist keine zugesagte Ausgangsleistung des Gesamtboards. [Schaltplan](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Schematic.pdf)

## Funk und Antenne

WLAN arbeitet auf 2,4 GHz nach 802.11 b/g/n. Bluetooth ist Bluetooth LE 5; daraus folgt keine Unterstützung von Bluetooth Classic/SPP. Die U-Ausführung führt die externe Antenne am Gehäuse über SMA heraus. Im Familien-Schaltplan ist der interne Antennenanschluss J1 als U.FL/IPEX angegeben. Der Weg enthält die Antennenwahl bei R12 sowie das Anpassnetzwerk um R13/C19/C20. Externer SMA-Anschluss und interner IPEX-Stecker sind unterschiedliche Anschlüsse desselben Antennenwegs. [Hersteller](https://docs.waveshare.net/ESP32-S3-RS485-CAN/), [Schaltplan](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Schematic.pdf)

Die Auswahl wird hardwareseitig durch die Bestückung getroffen, nicht durch eine übliche Arduino-Softwareoption. Die FAQ zur Lötbrücke der Standardvariante darf nicht unverändert als Umbauanweisung für ein U-Exemplar verwendet werden. [Antennen-FAQ](https://docs.waveshare.net/ESP32-S3-RS485-CAN/FAQ/)

## RTC

PCF85063A stellt Kalender, Uhrzeit, Alarm und Timer bereit. Der Chip verwendet I2C; im Herstellerdemo ist die 7-Bit-Adresse **0x51** definiert. Das Adressbyte mit Lese-/Schreibbit ist davon zu unterscheiden. Interner RTC-Bus und externer J4-Anschluss sind nicht dasselbe: J4 führt GPIO1/2 heraus, während die RTC GPIO38/39 verwendet. [Herstellerdemo](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Demo.zip), [NXP RTC-Datenblatt](https://www.nxp.com/docs/en/data-sheet/PCF85063A.pdf)

J7 ist der Batterieanschluss im Schaltplan. Der Hersteller nennt eine wiederaufladbare RTC-Batterie. Eine konkrete Batteriechemie, Anschlussstromfreigabe oder passende Primärzelle wird daraus hier nicht abgeleitet. Eine RTC-Batterie versorgt nicht automatisch WLAN oder den gesamten Controller. [Herstellerressourcen](https://docs.waveshare.net/ESP32-S3-RS485-CAN/), [Schaltplan](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Schematic.pdf)

## Anzeigen und Gehäuse

- Rote PWR-Anzeige für vorhandene Versorgung.
- RS485-Anzeige: grün für Senden, blau für Empfangen.
- CAN-Anzeige für Busaktivität.
- BOOT und RESET als lokale Taster.
- Hutschienengehäuse mit Herstellermaßen 91,6 × 23,3 × 58,7 mm.

Die Kommunikationsanzeigen hängen an der Schaltung. Der Demoheader-Eintrag `GPIO_PIN_CH1=47` belegt weder ein frei programmierbares Statuslicht noch ein eingebautes Relais. [Herstellerdaten](https://www.waveshare.com/ESP32-S3-RS485-CAN.htm), [Schaltplan und Demo](https://docs.waveshare.net/ESP32-S3-RS485-CAN/Resources-And-Documents/)

## Technische Grenzen

| Eigenschaft | Aussage |
| --- | --- |
| CAN | Ein nativer TWAI-Kanal; klassisches CAN, kein CAN FD |
| RS485 | Ein Halbduplex-Bus mit zu konfigurierender Richtung |
| Netzwerk | WLAN/BLE; kein Ethernet-/PoE-Baustein im veröffentlichten Schaltplan |
| SD und Relais | Kein SD-Sockel oder Leistungsrelais durch den Schaltplan nachgewiesen |
| Ausgangsleistung | Keine extern nutzbaren 2 A allein aus dem Reglerdatenblatt ableiten |
| Temperatur/Isolation | Keine Gesamtboard-Zulassung oder Isolationsklasse aus Einzelchipwerten ableiten |
| Automobilbetrieb | 7–36 V Nennversorgung ist kein Nachweis einer Fahrzeugfreigabe |

[TWAI-Hardwaregrenze](https://docs.espressif.com/projects/esp-idf/en/v5.5.2/esp32s3/api-reference/peripherals/twai.html). Weitere Einzelheiten: [Stromversorgung und Isolation](03-power-and-isolation.md).
