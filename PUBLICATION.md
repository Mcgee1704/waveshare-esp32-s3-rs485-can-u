# Vorgesehener Veröffentlichungsumfang

Ziel: öffentliches GitHub-Repository `Mcgee1704/waveshare-esp32-s3-rs485-can-u`, Branch `main`.

Inhalt: README, sechs eigene technische Dokumente, strukturierte Board-/Pindaten, Git-Quellenmetadaten, Herkunftsdaten der Herstellerdownloads, dieses Umfangsdokument und Prüfsummen. Die Aktualisierung vom 19.10.2026 ergänzt bereinigte Ergebnisse der erfolgreichen Chip- und Flash-Abfrage des eigenen Exemplars.

Die öffentliche Fassung enthält keine Hersteller-ZIPs, übernommenen Schaltplanbilder, persönlichen Flashabbilder, Geräteseriennummern, MAC-Adressen, lokalen Anschlussnummern, Rechnerpfade, WLAN-Daten, Chat-/Sitzungsprotokolle oder Inventare anderer Hardware. Fremde Originale bleiben über die Quellenverweise auffindbar. Die noch offene Platinenrevision und genaue Flash-Bauteilbezeichnung sowie die Grenzen der PSRAM-Erkennung sind ausdrücklich dokumentiert.

## Vorbereitete Aktualisierung vom 19.10.2026

Geändert werden `README.md`, `docs/01-hardware.md`, `docs/05-own-board.md`, `data/board.json`, `sources/README.md`, `sources/checksums.sha256` und dieses Umfangsdokument. Hinzu kommen ausschließlich bereinigte Angaben zu USB-VID/PID, Chiptyp und Revision, gemeldeten Chipeigenschaften, Quarz, PSRAM-Package, Flash-JEDEC/Größe, Flashtyp und Spannung laut eFuse sowie Werkzeug, Methode und Nachweisgrenzen. Rohprotokolle unter `.local/` bleiben ausgeschlossen. Weitere Gerätezugriffe sind für diese Dokumentationsänderung nicht erforderlich.

Diese Aktualisierung ist lokal vorbereitet. Ihre Übertragung nach `main` benötigt eine neue, zielgenaue Freigabe für diesen Umfang; die frühere Freigabe der ursprünglichen Dokumentation deckt die hinzugekommenen Exemplarergebnisse nicht ab.

## Abschluss vor dem Upload

1. Dokumentverweise, Pinangaben und maschinenlesbare Dateien gegen die Quellen abgleichen.
2. Die tatsächlich ausgewählten Git-Dateien auf Umfang und persönliche Daten prüfen.
3. Genau diesen Stand lokal committen und Commit-SHA festhalten.
4. Separat vor der Übertragung die genaue Ziel- und Umfangswarnung gemäß der Benutzerregel ausgeben.
5. Erst nach der zielgenauen zweiten Bestätigung Repository anlegen und den freigegebenen Commit nach `main` übertragen.

Es werden keine Geräte- oder Gegenstellentests als Voraussetzung für die Veröffentlichung verlangt. Eine öffentliche Veröffentlichung wird erst nach der separaten Uploadfreigabe durchgeführt.
