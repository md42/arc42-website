---
description: "Unter macOS Sonoma sollte Python3 mit Homebrew installiert und für Paketinstallationen ein virtuelles Environment (venv) genutzt werden, da systemweite Installationen durch die neue Verwaltung blockiert werden. Durch das Erstellen und Aktivieren eines venv wird die isolierte Installation von Python-Paketen ermöglicht, ohne das System zu beeinträchtigen."
tags:
- Der Software-Engineer
- Python
- macOS
---

# Python3 unter macOS Sonoma mittels venv nutzen

Apple hat mit der Einführung von macOS Sonoma den Unterbau der Python-Runtimes und den Python-Environments geändert. Ich gehe davon aus, dass die jeweils aktuellste Version von Python über Homebrew installiert wurde, da macOS nicht mit der aktuellen Version ausgeliefert wird.

```bash
brew install python3
```

Wichtig ist es hierzu sich durchzulesen, [welche Änderungen in der Handhabung von Python seitens Homebrew](https://docs.brew.sh/Homebrew-and-Python#python-3) vorgenommen wurde.

Will man nun eine Python-Applikation installieren wird man mit einem, vor macOS Sonoma nicht vorhandenen, Fehler konfrontiert:

```bash
pip3 install mkdocs
error: externally-managed-environment

× This environment is externally managed
╰─> To install Python packages system-wide, try brew install
    xyz, where xyz is the package you are trying to
    install.
    
    If you wish to install a Python library that isn't in Homebrew,
    use a virtual environment:
    
    python3 -m venv path/to/venv
    source path/to/venv/bin/activate
    python3 -m pip install xyz
    
    If you wish to install a Python application that isn't in Homebrew,
    it may be easiest to use 'pipx install xyz', which will manage a
    virtual environment for you. You can install pipx with
    
    brew install pipx
    
    You may restore the old behavior of pip by passing
    the '--break-system-packages' flag to pip, or by adding
    'break-system-packages = true' to your pip.conf file. The latter
    will permanently disable this error.
    
    If you disable this error, we STRONGLY recommend that you additionally
    pass the '--user' flag to pip, or set 'user = true' in your pip.conf
    file. Failure to do this can result in a broken Homebrew installation.
    
    Read more about this behavior here: <https://peps.python.org/pep-0668/>

note: If you believe this is a mistake, please contact your Python installation or OS distribution provider. You can override this, at the risk of breaking your Python installation or OS, by passing --break-system-packages.
hint: See PEP 668 for the detailed specification.
```

Alle Instruktionen, um den Fehler zu beheben, werden bereits mit der Fehlermeldung ausgegeben. Dennoch halte ich es für sinnvoll eine konkrete und funktionierende Lösung für diesen Fehler hier abzubilden, die die vorgeschlagene Lösung mittels venv[^1] nutzt.

```bash
mkdir ~/.venv   # Das virtuelle Environment benötigt ein Verzeichnis um seine Packages abzulegen.

python3 -m venv ~/.venv # Konfiguriert das Python-Modul venv mit dem Verzeichnis ~/.venv

# Auszug aus 'man python3'
# -m module-name
# Searches sys.path for the named module and runs the corresponding .py file as a script. This terminates the option list (following options are passed as arguments to the module).

source ~/.venv/bin/activate # Aktiviert das virtuelle Environment und wechselt die Eingabeaufforderung dort hin.

# Nun ist die "alte" Funktionsweise wieder hergestellt.
```

[^1]: venv — Creation of virtual environments, Python 3 Documentation, [https://docs.python.org/3/library/venv.html](https://docs.python.org/3/library/venv.html)