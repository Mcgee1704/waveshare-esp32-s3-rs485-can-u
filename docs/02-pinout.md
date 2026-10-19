# Anschlüsse und vollständige Pinbelegung

Quelle ist der [Herstellerschaltplan](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Schematic.pdf), ergänzt durch die Header `WS_GPIO.h` und `I2C_Driver.h` im [Demo](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Demo.zip). Die Tabellen verwenden die **Schaltplannummerierung**, keine ungeprüfte Draufsicht auf das Gehäuse. Physische Orientierung und Pin-1-Markierung sind an der Platine abzugleichen.

## P1 mit 20 Pins

Das Raster ist laut [Hersteller-FAQ](https://docs.waveshare.net/ESP32-S3-RS485-CAN/FAQ/) **2,0 mm**, nicht 2,54 mm.

| Ungerader Pin | Signal | Gerader Pin | Signal |
| --- | --- | --- | --- |
| 1 | 3V3 | 2 | 5V |
| 3 | GND | 4 | GND |
| 5 | TXD = UART0 TX, GPIO43 | 6 | D_P = USB D+, GPIO20 |
| 7 | RXD = UART0 RX, GPIO44 | 8 | D_N = USB D-, GPIO19 |
| 9 | GPIO3 | 10 | GPIO14 |
| 11 | GPIO4 | 12 | GPIO13 |
| 13 | GPIO5 | 14 | GPIO12 |
| 15 | GPIO6 | 16 | GPIO11 |
| 17 | GPIO7 | 18 | GPIO10 |
| 19 | GPIO8 | 20 | GPIO9 |

GPIO43/44 sind die ESP32-S3-Zuordnung der Signale U0TXD/U0RXD. TXD liegt im Schaltplan über R10 mit 499 Ohm am UART0-Ausgang. Die Headerpins sind Controllerlogik und teilen GND mit USB; sie sind nicht galvanisch isolierte RS485- oder CAN-Leitungen. Die 5-V-Versorgung an P1 bedeutet nicht, dass die GPIOs 5-V-tolerant sind. [Espressif-Pinfunktionen](https://documentation.espressif.com/esp32-s3_datasheet_en.pdf)

## Weitere Steckverbinder

| Bezeichnung | Pins laut Schaltplan | Bedeutung |
| --- | --- | --- |
| J2 | 1 = GND, 2 = positive Versorgung | Versorgungsklemme 7–36 V; räumliche Lage aus Nummern nicht erraten |
| J4 | 1 = GPIO1, 2 = GPIO2, 3 = 3V3, 4 = GND | Externer SH1.0-Anschluss |
| J5 | 1 = CANH, 2 = CANL | Isolierter CAN-Bus |
| J6 | 1 = RS485A, 2 = RS485B | Isolierter RS485-Bus; Gehäusekennzeichnung A+/B- |
| J7 | 1 = RTC-VDD, 2 = GND | Zweipoliger RTC-Batterieanschluss; SH1.0 laut Hersteller; physische Pinorientierung und Batterieart am Original abgleichen |
| J1 | U.FL/IPEX, HF-Signal und Schirm | Interner Anschluss im Antennenpfad |
| Type_C1 | USB-C | USB-Daten und 5-V-Versorgung |

J4 und J7 haben im Schaltplan zusätzlich jeweils den mechanischen Pin 0 auf GND. Er ist kein zusätzlicher belegbarer Kabelkontakt. Die Farbfolge eines beliebigen SH1.0-Kabels ist kein Beleg für seine Pinreihenfolge. J7 Pin 1 ist der RTC-Versorgungsknoten hinter D7, kein allgemeiner Versorgungseingang für den ESP32; eine Akku- oder Ladefreigabe folgt aus der Pinbelegung nicht.

## Intern belegte GPIOs

| GPIO | Funktion | Verfügbarkeit |
| --- | --- | --- |
| 0 | BOOT/Strapping | Nicht als frei beschaltbaren Eingang beim Einschalten behandeln |
| 1, 2 | J4 | Für projektspezifische digitale Funktionen oder geeignete Peripherie konfigurierbar |
| 3 | P1 und Strapping | Externe Beschaltung beeinflusst mögliche JTAG-Auswahl beim Reset |
| 4–14 | P1 | Allgemeine Projekterweiterung innerhalb der elektrischen GPIO-Grenzen |
| 15 | CAN TX | Durch Onboard-CAN belegt |
| 16 | CAN RX | Durch Onboard-CAN belegt |
| 17 | RS485 TX | Durch Onboard-RS485 belegt |
| 18 | RS485 RX | Durch Onboard-RS485 belegt |
| 19 | USB D- | Mit USB-C und P1 verbunden |
| 20 | USB D+ | Mit USB-C und P1 verbunden |
| 21 | RS485 EN/RTS | Für Richtungsschaltung des Bus-Transceivers erforderlich |
| 26–32 | Speicherbus | Nicht als Erweiterungspins verwenden |
| 33–37 | Interne Speicherbelegung bei dieser Bestückung | Im Schaltplan als intern belegt markiert |
| 38 | RTC SCL | Onboard-I2C |
| 39 | RTC SDA | Onboard-I2C; auch eine alternative JTAG-Funktion des Chips |
| 40 | RTC INT | Onboard-RTC; auch alternative JTAG-Funktion |
| 41, 42 | Im allgemeinen GPIO-Raster aufgeführt | Kein frei zugänglicher Anschluss durch das Raster allein nachgewiesen |
| 43, 44 | UART0 über P1 | Serielle Logiksignale, keine RS485-Pegel |
| 45, 46 | Strapping | Nicht auf P1 herausgeführt |
| 47, 48 | Im Schaltplan als IO-Signale benannt | Kein P1-Anschluss; GPIO47 im Demo ist kein Relaisbeleg |

GPIO-Nummern sind nicht die Gehäusepinnummern des ESP32. Auf P1 vorhandene Pins, physisch zugängliche Testpunkte und nur intern benannte Netze sind unterschiedliche Dinge. [Espressif-Datenblatt](https://documentation.espressif.com/esp32-s3_datasheet_en.pdf)

## Jumper und Widerstandsoptionen

| Kennzeichnung | Funktion laut Schaltplan |
| --- | --- |
| H1 / R19 | CAN-Abschluss mit 120 Ohm |
| H2 / R25 | RS485-Abschluss mit 120 Ohm |
| H3 / H4 | Als NC-Optionen im USB-Bereich dargestellt; nicht als BOOT-/RESET-Taster umdeuten |
| R12 / R13 / C19 / C20 | Antennenwahl bzw. HF-Anpasspfad; tatsächliche U-Bestückung beachten |
| R20 / R30 und R40 / R41 | Bestückungsoptionen an der isolierten Versorgung: gezeichnete Null-Ohm-Verbindung jeweils zu 3,3 V, alternative 5-V-Verbindung NC |

NC bedeutet hier unbestückt bzw. nicht verbunden. Die Bestückungsoptionen sind keine Softwareeinstellungen und keine Aufforderung zum Umlöten. Eine einmal angenommene Jumperlage wird nicht als beobachteter Zustand des eigenen Boards ausgegeben.

Für den Busanschluss gelten die [Regeln für End- und Zwischenknoten](03-power-and-isolation.md#busstruktur-und-abschlusswiderstände). GPIO21 wird nach der [Sende-/Empfangstabelle](04-firmware.md#rs485-richtung-und-treiberzuständigkeit) gesteuert. Elektrische Pegel und Versorgungskombinationen stehen im [elektrischen Aufbau](03-power-and-isolation.md#gpio-pegel-und-erweiterungslasten).

Die [pinout.csv](../data/pinout.csv) enthält **32 elektrische Kabel-/Headerkontakte**: P1 (20), J2 (2), J4 (4), J5 (2), J6 (2), J7 (2). J1/HF, die einzelnen USB-C-Kontakte sowie mechanische Massekontakte sind nicht Teil dieser CSV. Tabellen und CSV verwenden weiterhin die Schaltplannummerierung; eine Gehäusedraufsicht des eigenen Exemplars ist damit nicht nachgewiesen.
