---
description: "Einzelne Commits können als Pull-Request veröffentlicht werden, indem sie per Cherry-Pick in einen neuen Branch aufgenommen, dieser in die eigene Fork gepusht und anschließend ein Pull-Request ins Upstream-Repository erstellt wird."
tags:
- CLI
- GIT
---

# Einzelne Commits als Pull-Request veröffentlichen

Manchmal ist es notwendig und / oder gewünscht, einzelne Commits als Pull-Request zur Verfügung zu stellen, beispielsweise
wenn gute Änderungen in einem Downstream-Repository ins Upstream-Repository fließen sollen.

Die Herangehensweise ist einfach:

1. Neuen Branch auf Basis des `main`-Branches des Upstream-Repository erstellen.
1. Die gewünschten Commits auf den neuen Branch cherry-picken.
1. Den neuen Branch _in die eigenen Fork_ pushen.
1. Einen Upstream-Pull-Request erstellen.

```bash
git fetch -all
git checkout -b awesome-hotfixes <fork/main>
git cherry-pick <hash>
git cherry-pick <hash>
git push -u origin awesome-hotfixes
```

Danach wie gehabt einen Pull-Request eröffnen.

**Wichtig**

Es macht keinen Sinn, einen Branch auf Basis von Main zu eröffnen, und dann die Commits, die auf Main vorhanden sind,
zu cherry-picken, da diese ja sowieso bereits vorhanden sind. Daher macht es Sinn, den PR nur auf Basis der entsprechenden 
Commits zu erstellen. 