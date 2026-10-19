# USB, Bootmodus und Softwaregrundlagen

Dieses Dokument beschreibt die Programmierwege. Es ist kein Auftrag zum Flashen oder zum Ändern des eigenen Boards.

## USB und Wiedererkennung

Die USB-C-Datenleitungen führen direkt zum ESP32-S3. Eine sichtbare Espressif-USB-Kennung kann den Chip erkennen helfen, belegt aber weder Waveshare als Platinenhersteller noch die U-Variante oder Platinenrevision. Ein reines Ladekabel kann das Board versorgen, ohne eine Datenverbindung herzustellen. Die laufende Firmware kann das USB-Verhalten verändern. [Schaltplan](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Schematic.pdf), [Espressif-Datenblatt](https://documentation.espressif.com/esp32-s3_datasheet_en.pdf)

Zum dokumentierten manuellen Downloadmodus: BOOT halten, RESET drücken und loslassen, danach BOOT loslassen. Das startet den Chip neu und ist deshalb von einer passiven Betriebssysteminventur zu unterscheiden. Der Vorgang wurde hier nicht durchgeführt. [Waveshare-FAQ](https://docs.waveshare.net/ESP32-S3-RS485-CAN/FAQ/)

## Herstellerdemo

Im [ZIP](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Demo.zip) sind zwei Arduino-Beispiele enthalten:

| Beispielpfad | Zweck |
| --- | --- |
| `Arduino/examples/MAIN_WIFI_AP` | Lokaler WLAN-Zugangspunkt und Bedienoberfläche für die Schnittstellen |
| `Arduino/examples/MAIN_WIFI_STA` | Verbindung mit einem vorhandenen WLAN; Zugangsdaten projektspezifisch setzen |
| `Arduino/libraries` | Beigepackte Bibliotheken |
| `Firmware/ESP32-S3-RS485-CAN.bin` | Zusammengefasste Hersteller-Testfirmware |

Der Hersteller dokumentiert Arduino-ESP32 3.0.x, ArduinoJson 6.21.4, PubSubClient 2.8.0 und NTPClient 3.2.1. Diese Stände gehören zum Demo und sind keine Auswahl aktueller Versionen für jedes Folgeprojekt. [Herstelleranleitung](https://docs.waveshare.net/ESP32-S3-RS485-CAN/Arduino/)

### Belegte Konfiguration

| Parameter | Herstellerreferenz |
| --- | --- |
| Arduino-Board | ESP32S3 Dev Module |
| CPU | 240 MHz |
| Flash | 16 MB, QIO 80 MHz |
| PSRAM | OPI PSRAM |
| Demo-Partitionierung | 16M Flash mit 3-MB-App/9,9-MB-FATFS laut verlinktem IDE-Bild |
| USB-Modus im Bild | Hardware CDC and JTAG |
| USB CDC On Boot im Bild | Disabled; daher keine automatische USB-Konsole allein aus diesem Bild ableiten |
| CAN-Startwert im Demo | 250 kbit/s, TWAI_MODE_NORMAL |
| RS485-Startwert im Demo | 9600 Baud, 8 Datenbits, keine Parität, 1 Stoppbit |

Der [IDE-Screenshot](https://www.waveshare.com/w/upload/e/e9/ESP32-S3-16MB-OPI-Parameters.png) stammt aus einer allgemeinen Herstellerkonfiguration und enthält weitere IDE-/Projektangaben. Der dortige COM-Port gilt nicht für andere Rechner. Eine aktive CDC-Konsole, OTA und die passende Partitionstabelle werden im Folgeprojekt bewusst konfiguriert.

### Entscheidende Quelldateien

| Datei in beiden Beispielverzeichnissen | Relevanter Inhalt |
| --- | --- |
| `WS_GPIO.h` | TX17/RX18/EN21 für RS485, TX15/RX16 für CAN |
| `WS_RS485.cpp` | UART1; GPIO21 als RTS, `UART_MODE_RS485_HALF_DUPLEX` |
| `WS_CAN.cpp` | TWAI-Initialisierung und Demo-Bitrate; normaler, nicht passiver CAN-Modus |
| `I2C_Driver.h` | RTC-SCL38 und SDA39 |
| `WS_PCF85063.h` | RTC-Adresse 0x51 und Registerkonstanten |
| `WS_Information.h` | Demooptionen und WLAN-Vorgaben |

„Automatische Richtungsumschaltung“ heißt beim Herstellerdemo: Der konfigurierte UART-Treiber steuert RTS. GPIO21 darf nicht einfach weggelassen werden. Ein fremdes Projekt mit automatischer RS485-Hardware ist nicht allein durch Tauschen von TX/RX-Pins passend.

## CAN und RS485 als Softwarebausteine

Der S3 besitzt einen TWAI-Controller mit klassischen CAN-Frames: 11-/29-Bit-Identifier, maximal acht Nutzdatenbytes. Er unterstützt keine CAN-FD-Frames. Der Transceiver auf der Platine benötigt keine MCP2515-SPI-Bibliothek. Ein Projekt für den MCP2515 muss auf den tatsächlichen Controller angepasst werden. [Espressif TWAI](https://docs.espressif.com/projects/esp-idf/en/v5.5.2/esp32s3/api-reference/peripherals/twai.html)

Die ältere Demo-API `driver/twai.h` und neuere IDF-Node-APIs sind versioniert zu behandeln. Listen-only ist ein eigener Controllerzustand; ein vorhandener Empfangscode im Normalmodus macht ein Programm nicht zum rein passiven Zuhörer.

RS485 ist die elektrische Schnittstelle, Modbus RTU ein mögliches Protokoll darüber. Registerpläne, Adressen, CRC, Geräteprofile und Abfragetiming kommen aus der jeweiligen Anwendung. Die Hersteller-FAQ nennt bis 1 Mbit/s für CAN und empfiehlt für RS485 zunächst höchstens 115200 Baud. Die Chipdatenraten allein sind keine Zusage für die komplette isolierte Platine. [Waveshare-FAQ](https://docs.waveshare.net/ESP32-S3-RS485-CAN/FAQ/)

## Firmwareformate unterscheiden

Der Hersteller nennt **0x0** für seine zusammengefasste Test-BIN. Dieser Offset gilt nicht generell für eine einzelne `firmware.bin`, einen Bootloader oder ein OTA-Image. Die jeweilige Partitionstabelle und Herkunft des Images bestimmen das Format. [Hersteller-Flashingbeschreibung](https://docs.waveshare.net/ESP32-S3-RS485-CAN/Firmware-Flashing/)

Die Hardware-Flashgröße 16 MB bedeutet weder eine 16-MB-App noch automatisch zwei OTA-Slots. Die Demoaufteilung enthält Dateisystemplatz; eine spätere OTA-Firmware benötigt eine passende eigene Aufteilung. Der Firmwareinhalt eines persönlichen Geräts kann WLAN-Zugänge und andere Betriebsdaten enthalten und gehört nicht in ein öffentliches Boardrepository.

## Geeignete Entwicklungsumgebungen

| Weg | Verwendung |
| --- | --- |
| Arduino | Direkter Einstieg über das Herstellerdemo und dessen Kernversion |
| ESP-IDF | Direkter Zugriff auf UART, TWAI, Netzwerk, Speicher und Aufgabenverwaltung |
| ESPHome | Bestehende Home-Assistant-/Geräteintegration mit UART/Modbus oder `esp32_can` |

Für ESPHome wird GPIO21 als Flow-Control passend zur konkreten Komponente konfiguriert. Pinpolarität und Zuständigkeit nicht durch doppelte Steuerung in UART und Modbus widersprüchlich setzen. Die Dokumentation der verwendeten Version ist maßgeblich. [ESPHome UART](https://esphome.io/components/uart/), [ESPHome CAN](https://esphome.io/components/canbus/)

Die Framework-Commits im Quellenregister sind Recherche-Snapshots der damaligen Standardbranches, keine getesteten Release-Pins für ein Bauprojekt.
