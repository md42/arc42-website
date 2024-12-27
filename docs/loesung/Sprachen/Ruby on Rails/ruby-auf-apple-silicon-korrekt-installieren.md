---
description: "Die mit macOS gelieferte Ruby-Version ist veraltet und sollte nicht mit Homebrew aktualisiert werden. Stattdessen empfiehlt es sich, Ruby mit ruby-install zu installieren und die Version anschließend mit chruby zu verwalten und zu konfigurieren."
tags:
- Der Software-Engineer
- macOS
- Ruby on Rails
---

# macOS Sonona mit Apple Silicon: Ruby korrekt installieren

Die Version von ruby, die Apple mit macOS verteilt ist extrem veraltet. Sollte eine neuere Version benötigt werden,
ist es leider nicht trivial auf eine neuere Version von ruby zu aktualisieren.

!!! warning "Wichtig"

    `ruby` sollte _nicht mit homebrew_ installieren werden. Du wurdest hiermit ausdrücklich gewarnt.  
    `ruby` sollte _niemals_ über mehrere Ruby-Manager betrieben werden.

Stattdessen ist es sinnvoll ruby über `ruby-install` herunterzuladen, zu kompillieren und zu installieren und anschließend mit `chruby` die korrekte ruby-Version auszuwählen.

```bash
# Paketquellen aktualisieren und chruby und ruby-install installieren.
brew update && brew install chruby ruby-install

# Die neueste Version von Ruby installieren
ruby-install ruby
```

Die Installation ist noch nicht abgeschlossen. `chruby` muss anschließen noch konfiguriert werden.

```bash
# Sourcing von chruby in zsh registrieren
echo "source $(brew --prefix)/opt/chruby/share/chruby/chruby.sh" >> ~/.zshrc
echo "source $(brew --prefix)/opt/chruby/share/chruby/auto.sh" >> ~/.zshrc

# Installierte Ruby Version über chruby konfigurieren.
# Syntax zum Beispiel: chruby ruby-3.3.4 
echo "chruby ruby-<version>" >> ~/.zshrc
```

Um die Installation abzuschließen, sollte der Terminal-Emulator neugestartet werden.