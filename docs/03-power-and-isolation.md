# Stromversorgung, Isolation und Schutzschaltung

Die folgende Beschreibung leitet die Signal- und Versorgungspfade aus dem [veröffentlichten Schaltplan](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Schematic.pdf) ab. Bauteilbestückung und Leiterplattenrevision des eigenen Exemplars wurden noch nicht optisch bestätigt.

## Zwei Versorgungswege

```text
DC-Klemme J2, 7–36 V
  -> D4 / Eingangsschutz / XL1509 / D3 -> 5V
USB-C, 5 V
  -> USB-Versorgungspfad mit D1       -> 5V

5V -> MP1605 U1 -> 3V3 -> ESP32 / Flash / Controllerlogik
5V -> B0505 U3  -> Relay-5V, SGND
                     -> ME6217 U4 -> Relay-3V3, SGND
```

Das ist eine funktionale Darstellung, keine Ersatzzeichnung zum Verdrahten. USB-C ist kein 7–36-V-Eingang. Der zulässige Klemmenbereich des Produkts bleibt 7–36 V, auch wenn ein Reglerdatenblatt einen anderen Bereich nennt. Das Netz `Relay-5V` bezeichnet hier die isolierte Busversorgung; sein Name belegt kein vorhandenes Relais.

Der MP1605 ist im Schaltplan mit 3,314 V berechnet. Eine dort genannte 2-A-Reglergröße ist keine Zusage, dass am Erweiterungsstecker dauerhaft 2 A für zusätzliche Geräte verfügbar sind. DC/DC-Wandler, USB-Quelle, Eigenverbrauch und Wärmeabfuhr begrenzen das Gesamtgerät. [XL1509-Datenblatt](https://www.xlsemi.com/datasheet/XL1509-EN.pdf)

## Isolation genau einordnen

U9 überträgt CAN-TX/RX, RS485-TX/RX und das RS485-Enable zwischen den Potenzialseiten. U3 stellt die isolierte Versorgung bereit. Der Schaltplan trennt Controller-GND und busseitiges **SGND**.

**CAN und RS485 teilen sich im Schaltplan dieselbe isolierte Versorgungs- und SGND-Domäne.** Sie sind damit gemeinsam gegenüber der Controllerseite isoliert; eine zusätzliche gegenseitige galvanische Trennung der beiden Busse ist nicht dargestellt. P1, J4 und USB gehören zur Controllerseite.

R31 verbindet SGND und das als EARTH bezeichnete Schutznetz mit 0 Ohm. EARTH im Zeichnungsnetz ist weder automatisch der Schutzleiter einer Installation noch USB-GND. Eine externe Masseverbindung kann eine Isolation überbrücken. Eine Prüfspannung, dauerhafte Arbeitsspannung oder Schutzklasse des Gesamtprodukts wird aus dem vorhandenen Isolator allein nicht abgeleitet.

## CAN-Pfad

ESP32 GPIO15/16 -> digitaler Isolator -> TJA1051 U8 -> J5 CANH/CANL. R19 mit H1 ermöglicht den 120-Ohm-Abschluss. D8 ist als SM24CANB-02HTG zum Schutz der Busleitungen eingezeichnet. Aktivitätsanzeige und Versorgung liegen auf der Busseite.

Der TJA1051 ist ein CAN-Transceiver, kein zweiter CAN-Controller. Auch wenn eine Transceiverfamilie CAN-FD-taugliche physikalische Übertragung unterstützt, bleibt der interne ESP32-S3-Controller auf klassisches CAN beschränkt. [NXP TJA1051](https://www.nxp.com/docs/en/data-sheet/TJA1051.pdf), [Espressif TWAI](https://docs.espressif.com/projects/esp-idf/en/v5.5.2/esp32s3/api-reference/peripherals/twai.html)

## RS485-Pfad

ESP32 UART1 GPIO17/18 und Enable GPIO21 -> digitaler Isolator -> SP3485 U7 -> Schutzbeschaltung -> J6 A/B. H2 schaltet R25 mit 120 Ohm zu.

| Bauteilgruppe | Im Schaltplan beschriftet | Aufgabe im Pfad |
| --- | --- | --- |
| F1, F2 | BSMD1206-050 | Serienelemente in den beiden Busleitungen |
| D10 | SMF6.5CA | Transientenschutz zwischen den busseitigen Netzen |
| D11 | SM712 | Schutzbeschaltung zum EARTH-Netz |
| D12, D13 | SMF12CA | Weitere Transientenschutzpfade |
| R24, R27 | 4,7 kOhm | Bias-Verbindungen nach SGND bzw. Relay-3V3 |
| D9 | B32-150-LF | Zusätzliches Schutzelement im RS485-Abschnitt |

Die konkrete Polung der Bias-Netze ist der Zeichnung zu entnehmen. A/B-Bezeichnungen verschiedener Gegenstellen sind nicht immer konsistent. Die Schutzbauteile belegen ohne zugehörigen Prüfnachweis keine bestimmte Surge-/ESD-Festigkeit des gesamten Boards. [SP3485-Herstellerseite](https://www.maxlinear.com/product/interface/serial-transceivers/rs-485-rs-422/sp3485)

## USB und externe Signale

GPIO19/20 werden über R14/R15 mit jeweils 22 Ohm an den USB-C-Datenpfad geführt. D2 schützt den USB-Bereich; R3/R6 mit 5,1 kOhm gehören zu den CC-Leitungen. Der Schaltplan zeigt den direkten nativen USB-Pfad des ESP32-S3 und keinen zusätzlichen CH343-/CP210x-USB-UART-Chip.

Die Spannungen und Logikpegel von P1/J4 sind von den isolierten Schraubklemmen zu unterscheiden. Für die spätere Integration sind eine gewählte Versorgung und ihre Lastgrenzen festzulegen. Ein CAN- oder RS485-Gegenstellentest ist nicht Bestandteil dieser Dokumentation.
