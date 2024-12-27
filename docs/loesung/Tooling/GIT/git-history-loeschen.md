---
description: "Die gesamte Git-Historie eines Repositories kann durch das Erstellen eines neuen, leeren Branches und einen forcierten Push gelöscht und auf einen einzigen konsolidierten Commit reduziert werden."
tags:
- GIT
- CLI
---

# GIT History eines Repository löschen

Manchmal bedarf es Aufräumarbeiten innerhalb eines GIT-Repos, beispielsweise wenn ein altes Projekt migriert werden soll, ohne tausende von Commits
zu übertragen. Zu diesem Zweck kann es sinnvoll sein, alle bisherigen Commits zu entfernen und alles konsolidiert zu pushen.

```shell
git checkout --orphan cleanup_branch
git add .
git commit -am "Initial commit"
git branch -D main
git branch -m main
git push -f origin main
```