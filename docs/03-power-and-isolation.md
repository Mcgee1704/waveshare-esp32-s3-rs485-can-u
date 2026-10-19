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

## Versorgungskombinationen

| Betriebsart | Beleg und Einordnung für die Planung |
| --- | --- |
| Nur USB-C mit 5 V | Vom Hersteller als Versorgung vorgesehen; nutzbare Leistung hängt von USB-Quelle, Kabel und Boardverbrauch ab. |
| Nur J2 mit 7–36 V DC | Vom Hersteller als Versorgung vorgesehen; Polarität laut J2-Schaltplannummerierung beachten. |
| USB-C und J2 gleichzeitig | Der Schaltplan führt beide Quellen über D1 bzw. D3 auf die nominelle 5V-Schiene. Daraus allein folgt keine geprüfte Freigabe für jeden Parallelbetrieb; eine ausdrückliche Produktfreigabe liegt in den ausgewerteten Unterlagen nicht vor. Für ein neues Projekt zunächst eine einzelne Quelle vorsehen. |
| Einspeisen über P1 5V oder 3V3 bzw. J4 3V3 | Das sind angeschlossene Versorgungsschienen; ein als Produkteingang freigegebener Einspeisebetrieb ist nicht belegt. J4 3V3 wird vom Hersteller als Ausgang beschrieben. |
| Zusätzliche Last an P1/J4 | Kein belastbares externes Dauerstrombudget erfasst. Vor Festlegung einer Last Eigenverbrauch, Quelle und thermische Grenzen berücksichtigen; Bauteil-Nennströme ersetzen kein Boardbudget. |
| Fremd versorgte Peripherie an P1/J4 | Logikpegel, Einschaltreihenfolge und gemeinsame Controller-Masse planen. Aktive Signale bei ausgeschaltetem ESP32 nicht ungeprüft auf dessen GPIOs legen; Rückspeisung ist durch geeignete Entkopplung oder abgestimmte Versorgung zu vermeiden. |

Die Netzbezeichnung `5V` ist eine nominelle Schaltplanbezeichnung, keine Messung der Spannung hinter den Dioden. P1/J4-GND bleibt Controller-GND und ist kein herausgeführter isolierter Bus-Masseanschluss. [Waveshare-Versorgungs- und Anschlussbeschreibung](https://docs.waveshare.net/ESP32-S3-RS485-CAN/), [Familienschaltplan](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Schematic.pdf).

## GPIO-Pegel und Erweiterungslasten

Die folgenden Werte beziehen sich auf die **ESP32-S3-Chipcharakteristik**, nicht auf ein vermessenes Lastbudget des Boards. P1 und J4 sind 3,3-V-Logikanschlüsse. Die 5V-Schiene an P1 macht keinen GPIO 5-V-tolerant.

| Größe | Datenblattwert und Bedeutung |
| --- | --- |
| Versorgung der 3,3-V-Chipdomänen | Empfohlener Bereich 3,0–3,6 V; der Boardregler legt die Schiene fest. Das ist kein zulässiger Spannungsbereich für USB oder J2. |
| Eingangs-Low, VIL | Bis 0,25 × VDD; bei nominell 3,3 V höchstens 0,825 V für einen sicher erkannten Low-Pegel. |
| Eingangs-High, VIH | Ab 0,75 × VDD; bei nominell 3,3 V mindestens 2,475 V für einen sicher erkannten High-Pegel. |
| Oberer Eingangswert in der DC-Tabelle | VDD + 0,3 V, bei nominell 3,3 V also 3,6 V. Das ist keine Freigabe für 5-V-Signale oder Spannungsspitzen; ein Pegelwandler ist bei inkompatiblen Logikpegeln erforderlich. |
| Typische Ausgangstreiberwerte | 40 mA Quelle bei VOH ≥ 2,64 V bzw. 28 mA Senke bei VOL = 0,495 V, VDD = 3,3 V und PAD_DRIVER = 3. Das sind charakterisierte typische Werte, keine garantierte Dauerlastfreigabe je Headerpin. |
| Externes 3V3-/5V-Strombudget | Nicht bestimmt. Motoren, Relaiswicklungen und andere Leistungslasten benötigen passende Treiber und eine ausgelegte Versorgung. |

Die Pegel- und Treiberwerte stehen in Tabelle 5-4 bei 3,3 V und 25 °C; die Versorgungsempfehlungen in Tabelle 5-2. Absolute Grenzwerte sind von empfohlenen Betriebsbedingungen zu unterscheiden. Eingangs-Maximalwert und Schaltschwellen beziehen sich auf die jeweilige tatsächliche Versorgung, nicht pauschal auf 3,3 V. [Espressif-Datenblatt v2.2, Abschnitte 5.1, 5.2 und 5.4](https://documentation.espressif.com/esp32-s3_datasheet_en.pdf).

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
| R24 | 4,7 kOhm | RS485-B-Leitung auf der Transceiverseite nach SGND |
| R27 | 4,7 kOhm | RS485-A-Leitung auf der Transceiverseite nach Relay-3V3 |
| D9 | B32-150-LF | Zusätzliches Schutzelement im RS485-Abschnitt |

A/B-Bezeichnungen verschiedener Gegenstellen sind nicht immer konsistent. Die genannten Pull-up-/Pull-down-Widerstände sind fest gezeichnete Bias-Netze; H2 schaltet den Abschluss, nicht das Bias. Ein definierter Ruhezustand des gesamten RS485-Netzes folgt daraus nicht automatisch: vorhandene Abschlüsse, weitere Bias-Netze, Versorgung und Empfängerschwellen müssen zusammenpassen. Die Schutzbauteile belegen ohne zugehörigen Prüfnachweis keine bestimmte Surge-/ESD-Festigkeit des gesamten Boards. [SP3485-Datenblatt](https://www.maxlinear.com/ds/sp3485.pdf), [TI RS-485 Design Guide, Abschnitte 6 und 7](https://www.ti.com/lit/an/slla272d/slla272d.pdf).

## Busstruktur und Abschlusswiderstände

CAN und RS485 bilden zwei getrennte Signalbusse. Für jeden Bus wird die Topologie gesondert geplant; sie teilen sich lediglich die im Schaltplan gezeigte isolierte Versorgung und SGND-Domäne.

| Anschlussposition | CAN: H1 / R19, 120 Ohm | RS485: H2 / R25, 120 Ohm |
| --- | --- | --- |
| Physisches Leitungsende | Bei einem üblichen linearen High-Speed-CAN-Bus ist an jedem der beiden Enden ein 120-Ohm-Abschluss vorgesehen. Den Boardabschluss nur nutzen, wenn dieses Ende noch keinen passenden Abschluss besitzt. | Bei einer üblichen 120-Ohm-Leitung den Abschluss passend zur Topologie am Leitungsende vorsehen; vorhandene Abschlüsse anderer Geräte berücksichtigen. |
| Zwischenknoten / kurzer Abzweig | Keinen zusätzlichen Endabschluss allein deshalb einschalten, weil das Board ein weiterer Teilnehmer ist. | Ebenfalls nicht automatisch einen weiteren 120-Ohm-Abschluss hinzufügen; die geplante Leitungsanpassung ist maßgeblich. |

Die Hauptleitung als Linie mit kurzen Stichleitungen planen; eine beliebige Sternverdrahtung wird durch die Jumper nicht angepasst. Die Herstellerbeschreibung nennt die Abschlüsse standardmäßig NC und per Jumper aktivierbar. Die tatsächlich gesteckten Jumper des eigenen Exemplars wurden nicht gesichtet. Für das Projekt sind ihre erforderlichen Zustände zu dokumentieren; die Tabelle meldet keinen am Board eingestellten Zustand. Es wird hier weder ein Jumper verändert noch eine Widerstandsmessung verlangt. [Waveshare-Modellbeschreibung](https://docs.waveshare.net/ESP32-S3-RS485-CAN/), [Waveshare-FAQ zu CAN-Abschlüssen](https://docs.waveshare.net/ESP32-S3-RS485-CAN/FAQ/), [TI RS-485 Design Guide, Abschnitt 6](https://www.ti.com/lit/an/slla272d/slla272d.pdf).

Die zweipoligen Busklemmen J5 und J6 führen keinen separaten SGND-Kontakt heraus. Masse-/Schirmkonzept und zulässige Potenzialdifferenzen müssen zur konkreten Installation passen; USB-/P1-GND ist kein Ersatz für SGND. Eine undifferenzierte zusätzliche Massebrücke kann die dargestellte Isolation aufheben. [Schaltplan](https://files.waveshare.com/wiki/ESP32-S3-RS485-CAN/ESP32-S3-RS485-CAN-Schematic.pdf), [TI RS-485 Design Guide, Abschnitt 11](https://www.ti.com/lit/an/slla272d/slla272d.pdf).

## USB und externe Signale

GPIO19/20 werden über R14/R15 mit jeweils 22 Ohm an den USB-C-Datenpfad geführt. D2 schützt den USB-Bereich; R3/R6 mit 5,1 kOhm gehören zu den CC-Leitungen. Der Schaltplan zeigt den direkten nativen USB-Pfad des ESP32-S3 und keinen zusätzlichen CH343-/CP210x-USB-UART-Chip.

Die Spannungen und Logikpegel von P1/J4 sind von den isolierten Schraubklemmen zu unterscheiden. Für die spätere Integration sind eine gewählte Versorgung und ihre Lastgrenzen festzulegen. Ein CAN- oder RS485-Gegenstellentest ist nicht Bestandteil dieser Dokumentation.
