---
description: "Mit dem Tool heif-convert können HEIC-Bilder effizient in JPEGs konvertiert werden, wobei der Befehl find alle passenden Dateien in Verzeichnissen und Unterverzeichnissen automatisiert verarbeitet."
tags:
- CLI
- Ubuntu
---

# HEIC in JPEG konvertieren (Batch)

Fotos die mit iPhones aufgenommen werden, werden standardmäßig in Apples **H**igh **E**fficiency **I**mage **F**ormat aufgenommen.
Leider ist dieses Format nur bedingt zur Archivierung von Fotos geeignet, und auch nicht zum weitersenden an Nicht-Apple-Nutzer:innen.

Gibt es große Bestände an Fotos, die konvertiert werden sollen, hilft das exzellente Tool `heif-convert`. Es lässt sich unter Ubuntu mit folgendem Befehl installieren:

```bash
sudo apt install libheif-examples
```

Nun muss nur noch über alle Verzeichnisse und Unterverzeichnisse gelaufen werden, un die konvertierten Dateien
werden direkt neben ihren Originalen abgelegt. `-q 100` definiert die bestmögliche Bildqualität.

```bash
find . -iname "*.heic" -exec heif-convert -q 100 {} {}.jpg \;
```