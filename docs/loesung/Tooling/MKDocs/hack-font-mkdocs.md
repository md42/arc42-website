---
description: "Das Einbetten von third-party Fonts in Material for MKDocs am Beispiel von Hack."
tags:
  - Fonts
  - CSS
  - MaterialForMKDocs
  - Der Software-Engineer
---

# Webfont Hack als monospaced Font in Material for MKDocs nutzen

1. Download der neuesten Version von Hack: [https://github.com/source-foundry/Hack/releases/latest](https://github.com/source-foundry/Hack/releases/latest)
1. Verschieben des Ordners `/fonts` in `/docs/stylesheets`
1. Verschieben von `hack.css` in `/docs/stylesheets`
1. Einbetten in den Code anhand der nachfolgenden Code-Snippets


```yaml title="mkdocs.yml"
# Sicherstellen, dass zusätzliche CSS eingebunden werden.
extra_css:
  - stylesheets/extra.css

```

```css title="/docs/stylesheets/extra.css"
/* Importieren des Stylesheets von Hack */
@import "hack.css";

:root {
    ..
    /* Zuweisung der Schriftfamilie */
    --md-code-font: "Hack";
    ..
}
```