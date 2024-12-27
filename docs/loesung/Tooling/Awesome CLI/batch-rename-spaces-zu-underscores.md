---
description: "
Mit einem einfachen Bash-Skript können Leerzeichen in Dateinamen durch Unterstriche ersetzt werden, um Probleme bei der Stapelverarbeitung zu vermeiden."
tags:
- CLI
---

# Batch: Leerzeichen zu Underscores konvertieren

Für die Stapelverarbeitung von Dateien, zB. durch pigz oder gpg ist es nicht hilfreich, wenn Dateinamen Leerzeichen beinhalten,
da Leerzeichen die Pfadangaben stören können. Manchmal ist es notwendig, alle Dateien und / oder Verzeichnisse in einem Verzeichnis automatisiert so umzubenennen, dass die Leerzeichen zu _ werden.

Dies lässt sich recht einfach mit `bash` bewerkstelligen.

```bash
for file in *
    do mv -- "$file" "${file// /_}"
done
```