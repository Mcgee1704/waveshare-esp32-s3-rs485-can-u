# USB, Bootmodus und Softwaregrundlagen

Dieses Dokument beschreibt die Programmierwege. Es ist kein Auftrag zum Flashen oder zum Ändern des eigenen Boards.

## USB und Wiedererkennung

Die USB-C-Datenleitungen führen direkt zum ESP32-S3. Eine sichtbare Espressif-USB-Kennung kann den Chip erkennen helfen, belegt aber weder Waveshare als Platinenhersteller noch die U-Variante oder Platinenrevision. Ein reines Ladekabel kann das Board versorgen, ohne eine Datenverbindung herzustellen. Die laufende Firmware kann das USB-Verhalten verändern. [Schaltplan](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Schematic.pdf), [Espressif-Datenblatt](https://documentation.espressif.com/esp32-s3_datasheet_en.pdf)

Zum dokumentierten manuellen Downloadmodus: BOOT halten, RESET drücken und loslassen, danach BOOT loslassen. Das startet den Chip neu und ist deshalb von einer passiven Betriebssysteminventur zu unterscheiden. Am eigenen Exemplar wurde dieser Modus vor der lesenden Chip- und Flash-Abfrage am 19.10.2026 durch den Besitzer hergestellt. Die anschließende Auslesung erfolgte ohne Firmware-Schreibbefehl, RAM-Hilfslader oder angeforderten automatischen Reset. [Auslesung des eigenen Boards](05-own-board.md), [Waveshare-FAQ](https://docs.waveshare.net/ESP32-S3-RS485-CAN/FAQ/).

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

### RS485-Richtung und Treiberzuständigkeit

Im Familienschaltplan führt GPIO21 (`RS485_EN`) über den Isolatorkanal U9 VIC/VOC zum gemeinsamen Signal an DE und aktivem Low-RE des SP3485. Die Tabelle ist eine Ableitung aus Schaltung und Bauteilfunktion, kein Gegenstellentest des Exemplars.

| GPIO21 / gemeinsames Enable im dargestellten Signalpfad | SP3485 DE | SP3485 /RE | Betriebszustand |
| --- | --- | --- | --- |
| Low | Low | Low | Sender hochohmig, Empfänger aktiv |
| High | High | High | Sender aktiv, Empfänger deaktiviert |

Die Tabelle setzt gültige, aktiv angesteuerte Logikpegel voraus. Sie legt keinen garantierten GPIO21-Zustand während Reset, Boot oder vor der Treiberinitialisierung fest. Das Folgeprojekt muss die Initialisierung der Richtung berücksichtigen; ein solcher Startzustand wurde hier nicht am Board gemessen.

Beim ESP-IDF-UART-Treiber im Modus `UART_MODE_RS485_HALF_DUPLEX` wird RTS beim Senden auf 1 gesetzt und nach dem letzten tatsächlich ausgesendeten Bit auf 0 zurückgenommen. Die normale RTS/CTS-Hardware-Flusssteuerung ist dafür zu deaktivieren; RTS ist hier Richtungssteuerung. Das Arduino-Herstellerdemo ordnet GPIO21 dem RTS-Pin von UART1 zu und wählt diesen Halbduplexmodus. [ESP-IDF 5.5.2 UART, RS485-Modus und Circuit B](https://docs.espressif.com/projects/esp-idf/en/v5.5.2/esp32s3/api-reference/peripherals/uart.html), [SP3485, Pin Functions](https://www.maxlinear.com/ds/sp3485.pdf), [Schaltplan](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Schematic.pdf).

Für Folgeprojekte gilt: **Eine Komponente besitzt GPIO21.** Übernimmt der UART-Treiber die Richtung, darf eine Modbus-Komponente den Pin nicht zusätzlich unabhängig schalten. Bei manueller Steuerung vor dem Senden auf High schalten, das Ende der tatsächlichen UART-Übertragung einschließlich letztem Stoppbit abwarten und anschließend auf Low zurückschalten. Ein leerer Anwendungspuffer allein belegt noch keinen leeren UART-Sender. Protokollpausen und Antwortzeiten richten sich zusätzlich nach dem verwendeten Protokoll. Frameworkversion, Richtungspolarität und zuständige Komponente gehören ins Projektprofil.

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

## Projektprofil für ein Folgeprojekt

Vor der Übernahme diese Felder im jeweiligen Projekt ausfüllen. Die Herstellerwerte dienen als Referenz; es werden hier keine neuen Firmwarevorgaben auf das Board übertragen.

| Entscheidung | Einzutragender Projektwert |
| --- | --- |
| Aufgabe und Gegenstellen | Gerätefamilie, Datenrichtung, Protokoll und Adressierung; kein universelles Geräteprofil voraussetzen |
| Hardwarebasis | ESP32-S3-RS485-CAN-U laut Besitzer; PCB-Revision, Steckerlage und unbekannte Bauteile als offen übernehmen |
| Framework | Arduino, ESP-IDF oder ESPHome mit exakter Releaseversion; Quellcommit und Abhängigkeiten festhalten |
| Speicher | 16 MB Flash; 8 MB PSRAM laut Package-Erkennung, OPI-Referenz; passende Flash-/PSRAM-Einstellungen bewusst wählen |
| Partitionen und Updateweg | Eigene Partitionstabelle und gegebenenfalls OTA-Slots; Demoaufteilung nicht ungeprüft übernehmen |
| USB und Protokollierung | Gewünschter USB-Modus, CDC-Verhalten und Logausgabe; UART0 43/44 und USB 19/20 nicht versehentlich anderweitig belegen |
| CAN | TX15/RX16; Bitrate, Identifier, Normal- oder Listen-only-Modus und erforderlicher H1-Abschluss |
| RS485 | TX17/RX18/EN21; Baudrate, Datenformat, Richtungspolarität, einzelne zuständige Treiberkomponente und H2-Abschluss |
| Erweiterungen und Versorgung | Verwendete P1-/J4-Pins, Strapping-Konflikte, Pegel, Quelle, Peripherielast und dokumentierte offene Strombudgets |
| Netzwerkfunktionen | Lokale Dienste und gegebenenfalls ausdrücklich gewünschte externe Ziele; persönliche Zugangsdaten getrennt verwalten |

Das Profil dokumentiert Planungsentscheidungen. Es ist weder eine gebaute Firmwarevorlage noch ein Nachweis der angeschlossenen Gerätefunktion. Anschlussregeln und offene elektrische Grenzen stehen im [elektrischen Aufbau](03-power-and-isolation.md).
