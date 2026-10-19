# Erfassungsstand des eigenen Exemplars

Stand: 19.10.2026.

| Merkmal | Tatsächlich vorhandener Nachweis |
| --- | --- |
| Modell | Vom Besitzer als Waveshare ESP32-S3-RS485-CAN-U bezeichnet |
| Weitere angeschlossene Geräte | Laut Besitzer keine; Gegenstellentests sind nicht beauftragt |
| USB-Erkennung am Untersuchungsrechner | Erfolgreich als Espressif USB-Serial/JTAG erkannt; VID `0x303A`, PID `0x1001`; Windows-Gerätestatus OK |
| Chipmodell und Chiprevision | ESP32-S3 (QFN56), Siliziumrevision v0.2 |
| Kerne und gemeldeter CPU-Takt | esptool meldet „Dual Core + LP Core“; Architektur: zwei LX7-Kerne und ULP-Koprozessoren. 240 MHz als Chipeigenschaft, keine Messung des laufenden Firmwaretakts |
| Quarz | 40 MHz laut esptool |
| Funkmerkmale laut Chipkennung | Wi-Fi, Bluetooth 5 LE; kein Funkfunktionstest |
| Flash-JEDEC-Kennung | Hersteller `0x20`, Gerät `0x4018`; genaue Bauteilbezeichnung offen |
| Flashgröße | 16 MB laut Flash-Erkennung |
| Flashtyp und Spannung laut eFuse | Quad, vier Datenleitungen; 3,3 V |
| PSRAM laut Chip-Package-Erkennung | 8 MB integriert, `AP_3v3`; kein Speicherfunktionstest |
| Platinenrevision und Bauteilaufdrucke | Kein eigener Bild-/Sichtnachweis vorhanden |
| Antennenweg und Jumperzustände | U-Variante vom Besitzer genannt; tatsächliche Bestückung noch nicht gesichtet |
| Bestehende Firmware | Nicht ausgelesen oder verändert |

Die Auslesung war nach einem Wechsel des USB-Ports erfolgreich. Frühere fehlgeschlagene USB-Erkennungen sind damit als vorheriger Untersuchungsstand überholt. Die in den anderen Dokumenten beschriebenen Bauteile und Pins beziehen sich auf die veröffentlichte Herstellerreferenz, soweit kein eigener Nachweis ausdrücklich genannt wird.

## Methode der Auslesung

Quelle `LOCAL-READOUT-20261019`: Am 19.10.2026 wurden mit dem bereits vorhandenen esptool 5.3.0 die Befehle `chip-id` und `flash-id` ausgeführt. Beide endeten erfolgreich. Verwendete Verbindungsoptionen: `--chip esp32s3 --baud 115200 --before no-reset --after no-reset --no-stub --connect-attempts 1`; der lokale Anschluss wird in dieser öffentlichen Zusammenfassung weggelassen.

`chip-id` meldet beim ESP32-S3, dass keine separate Chip-ID vorhanden ist, und gibt stattdessen die MAC-Adresse aus. Diese eindeutige Kennung wird hier nicht übernommen. `flash-id` meldete vor der Identifikation das Aktivieren des Standard-SPI-Flashmodus; das ist eine Laufzeitkonfiguration für den Zugriff, kein Schreiben der Firmware. Es wurde kein RAM-Hilfslader hochgeladen. Das Werkzeug ließ das Board im Bootloader; ein regulärer Neustart war nicht Teil der Auslesung.

Kein Firmware-Schreibbefehl, kein Löschen, kein eFuse-Schreiben und kein Auslesen eines Firmwareabbilds wurden ausgeführt. Die lokal gesicherte Erfassung wurde für diese Zusammenfassung um eindeutige Gerätekennungen und Rechnerpfade bereinigt.

## Noch offene Nachweise

Zur Bestätigung der Platinenrevision und Bauteilbestückung sind zusätzlich eigene Ansichten der Platine bzw. lesbare Aufdrucke nötig. Eine ESP32-Chipkennung allein bestätigt die Waveshare-Variante nicht. Die Herstellerreferenz nennt W25Q128JVSI als Flash; die tatsächliche Auslesung wird mit Hersteller `0x20` und Gerät `0x4018` dokumentiert, ohne daraus eine ungesicherte Bauteilbezeichnung abzuleiten.

Die gemeldete PSRAM-Größe ist ein Nachweis aus der Chip-Package-Erkennung, kein Nachweis der Speicherfunktion. Firmwareversion, Antennenweg, Jumperzustände und Funktion von CAN, RS485 oder RTC wurden nicht bestimmt.

CAN-, RS485-, Sensor-, Last- und Gegenstellentests entfallen in diesem Auftrag. Auch neue Testfirmware, ein Löschvorgang, eFuse-Änderungen und ein Antennenumbau gehören nicht zur Bestandsaufnahme.

## Öffentlich und lokal

Die oben aufgeführten bereinigten Erkenntnisse dieses Exemplars und die entsprechenden Felder in [board.json](../data/board.json) wurden am 19.10.2026 mit dem Abschlusscommit [3b88771](https://github.com/Mcgee1704/waveshare-esp32-s3-rs485-can-u/commit/3b88771b7a1ab5279405e14b9c04b569ea1afa05) auf GitHub `main` veröffentlicht. Das Publikationsprotokoll steht in [PUBLICATION.md](../PUBLICATION.md).

MAC-Adressen, USB-Seriennummern, lokale Anschlussnummern, Rechnerpfade, WLAN-Zugänge, vollständige Flashabbilder und Inventare anderer Geräte sind vom Veröffentlichungsumfang ausgeschlossen. Das unbereinigte lokale Erfassungsprotokoll bleibt lokal. `psram_confirmed` in den strukturierten Daten bedeutet ausschließlich Bestätigung der Package-Angabe; `psram_evidence_status: package_identification` und `psram_functionally_tested: false` machen diese Grenze explizit.
