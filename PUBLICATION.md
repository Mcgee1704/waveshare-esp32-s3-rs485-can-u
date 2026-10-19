# Veröffentlichungsumfang und Historie

Ziel: öffentliches GitHub-Repository `Mcgee1704/waveshare-esp32-s3-rs485-can-u`, Branch `main`.

Inhalt: README, sechs eigene technische Dokumente, strukturierte Board-/Pindaten, Git-Quellenmetadaten, Herkunftsdaten der Herstellerdownloads, dieses Umfangsdokument und Prüfsummen.

Die öffentliche Fassung enthält keine Hersteller-ZIPs, übernommenen Schaltplanbilder, persönlichen Flashabbilder, Geräteseriennummern, MAC-Adressen, lokalen Anschlussnummern, Rechnerpfade, WLAN-Daten, Chat-/Sitzungsprotokolle oder Inventare anderer Hardware. Fremde Originale bleiben über die Quellenverweise auffindbar. Die noch offene Platinenrevision und genaue Flash-Bauteilbezeichnung sowie die Grenzen der PSRAM-Erkennung sind ausdrücklich dokumentiert.

## Bereits veröffentlichter Stand vom 19.10.2026

Die ursprüngliche Quellenreferenz wurde bis [910e174](https://github.com/Mcgee1704/waveshare-esp32-s3-rs485-can-u/commit/910e174cb7546fce56b2ec14538bfe6f34ab2b40) auf `main` veröffentlicht. Anschließend wurden die bereinigten Ergebnisse der erfolgreichen Chip- und Flash-Abfrage nach separater zielgenauer Freigabe in sieben Dateien ergänzt. Die Veröffentlichung erfolgte über den GitHub-Browserzugang in vier Commits und endete mit [3b88771](https://github.com/Mcgee1704/waveshare-esp32-s3-rs485-can-u/commit/3b88771b7a1ab5279405e14b9c04b569ea1afa05).

Der vollständige veröffentlichte Dateibaum stimmte mit dem freigegebenen lokalen Quellcommit `0bcb9dfbba30daae5d4b0b12bf85016e4727c4ec` überein. Veröffentlicht wurden USB-VID/PID, Chiptyp und Revision, gemeldete Chipeigenschaften, Quarz, PSRAM-Package, Flash-JEDEC/Größe und Flashtyp/-spannung laut eFuse sowie Methode und Nachweisgrenzen. Rohprotokolle wurden nicht übertragen.

## Umfang der Dokumentationskorrekturen nach der Prüfung

Die Überarbeitung betrifft `README.md`, `docs/01-hardware.md`, `docs/02-pinout.md`, `docs/03-power-and-isolation.md`, `docs/04-firmware.md`, `docs/05-own-board.md`, `data/board.json`, `data/pinout.csv`, `sources/README.md`, `sources/checksums.sha256` und dieses Umfangsdokument.

Sie korrigiert den Boot- und Veröffentlichungsstatus und ergänzt Busabschlüsse/Bias, RS485-Richtungslogik, GPIO-Charakteristik, die Einordnung von Versorgungskombinationen, den J7-Anschluss, den expliziten CSV-Umfang, ULP-/PSRAM-Nachweisbegriffe und ein Projektprofil für Folgeprojekte. Unbekannte Lastbudgets, Batteriechemie, Platinenrevision und physische Orientierung bleiben offen. Weitere Gerätezugriffe oder Gegenstellentests gehören nicht zu diesem Umfang. Der interne Prüfbericht und seine lokalen Nachweise unter `.local/` bleiben ausgeschlossen.

## Abschluss vor dem Upload

1. Dokumentverweise, Pinangaben und maschinenlesbare Dateien gegen die Quellen abgleichen.
2. Die tatsächlich ausgewählten Git-Dateien auf Umfang und persönliche Daten prüfen.
3. Genau diesen Stand lokal committen und Commit-SHA festhalten.
4. Separat vor der Übertragung die genaue Ziel- und Umfangswarnung gemäß der Benutzerregel ausgeben.
5. Erst nach der zielgenauen zweiten Bestätigung den freigegebenen Stand nach `main` übertragen.

Die vorherigen Uploadfreigaben gelten jeweils für ihren damaligen Umfang. Künftige Änderungen benötigen vor dem Upload eine neue, zielgenaue Freigabe. Dieser Ablauf gilt auch bei Verwendung von GitHub Desktop; dessen Einrichtung ist keine dauerhafte Freigabe aller zukünftigen Änderungen. Es werden keine Geräte- oder Gegenstellentests als Voraussetzung für eine Dokumentationsveröffentlichung verlangt.
