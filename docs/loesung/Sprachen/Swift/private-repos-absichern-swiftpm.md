---
description: "Private Swift Packages in GitHub können sicher über SSH-Keys eingebunden werden, indem der Public Key im Repository als Deploy Key und der Private Key in den Pipeline Secrets des integrierenden Repositorys hinterlegt wird. Dieser Ansatz vermeidet Klartext-Anmeldedaten in der Package.swift und ermöglicht eine sichere Authentifizierung in SwiftPM sowie GitHub Actions."
tags:
- Der Software-Engineer
- Swift
- GitHub
---

# Private Repositories für Swift Packages über SSH Keys absichern

SwiftPM ist der aktuelle Standard um Abhängigkeiten in die eigene Software einzubinden. Manche Swift-Packages befinden sich innerhalb von
privaten Repositories auf GitHub. Dies ist beispielsweise oft der Fall, wenn man selbst oder das eigene Team ein Swift-Package entwickelt, und dieses
(noch) nicht öffentlich zur Verfügung stellen möchte.

Mir sind drei Authentifizierungsmöglichkeiten bekannt, die SwiftPM nutzen kann, um private Repositories anzubinden.

- Die Kombination aus **Nutzername und Passwort**.  
  Diese Authentifizierungsmöglichkeit ist angenehm, muss sich allerdings mit dem Problem auseinandersetzen, dass Nutzername und Passwort im Klartext in der `Package.swift` hinterlegt werden, 
  was ein Sicherheitsrisiko, insbesondere bei öffentlichen Repositories, bedeutet.
- Die Kombination aus **Nutzername und Token**.  
  Die zweite Möglichkeit bestände darin, ein GitHub Token mit `repo` Scope anzulegen, um zu verhindern, dass das Token zu etwas anderem als zur Authenfizierung für ein bestimmtes Repository genutzt werden kann.
  Allerdings bedeutet auch dieser Ansatz, dass Nutzername und Token im Klartext hinterlegt werden müssen, er ist also nur bedingt geeignet.
- Die Nutzung von **SSH Private / Public Keys**.  
  Die eleganteste Nutzung. Der Public Key stellt kein großes Risiko dar, und mit ein paar Konfigurationsdateien, kann SwiftPM mühelos mit SSH basierten GIT-Repositories arbeiten.

Wie genau sich ein Repository über diese SSH Keys schützen lässt, und wie man dieses in GitHub Actions integriert, möchte ich in folgendem Blogpost erklären.

SwiftPM soll das private Repository `flowinho/private-repo` über SSH ansteuern. Dazu ändern wir die URL, in `Package.swift`.

```swift
dependencies: [
    .package(url: "git@github.com:flowinho/private-repo.git",
    from: "1.0.0)
]
```

Als nächsten benötigen wir einen neuen SSH Key, den wir zur Authentifizierung nutzen können. Um eine problemlose Integration mit GitHub Actions zu ermöglichen,
erhält dessen PrivateKey kein Passwort. Wichtig: Zum Zeitpunkt der Verfassung dieses Artikels werden **nur RSA Schlüssel unterstützt**.

```bash
# -b 4096: Schlüssel mit 4096bit Länge
# -t rsa: Type RSA
# -f key: Schlüsseldatei namens "key"
# -N "": Ohne Name/Kommentar
ssh-keygen -b 4096 -t rsa -f key -N ""
```

Den öffentlich Teil des Schlüssels hinterlegen wir im **zu schützenden Repository** im Bereich Einstellungen > Sicherheit > Deploy Keys.
Den privaten Teil des Schlüssel hinterlegen wir in den Pipeline Secrets des **integrierenden Repository**, beispielsweise als SSH_PRIVATE_KEY.

Um die Authentifizierung des Runners gegenüber des privaten Repositories zu ermöglichen, müssen wir den privaten Schlüssel aus den Pipeline Secrets zur Verfügung stellen und im `ssh-agent` des Runners registrieren.
Diese Aufgabe lässt sich ganz ohne GitHub Marketplace App bewerkstelligen:

```yaml
{% raw %}
- name: Add private SSH Key to runner
run: |
    # SSH Verzeichnis anlegen
    mkdir -p /home/runner/.ssh

    # SSH Private Key aus den Pipeline Secrets in Datei ablegen.
    echo "${{ secrets.SSH_PRIVATE_KEY }}" > /home/runner/.ssh/gha

    # Zugriffsberechtigungen definieren um Zugriff anderer Prozesse auf dem Runner zu beschränken.
    chmod 600 /home/runner/.ssh/gha

    # Erzwungener Start des ssh-agents
    eval `ssh-agent -s`

    # Den privaten Schlüssel zum Schlüsselbund des Agents hinzufügen um es SPM zu ermöglichen,
    # sich mit diesem zu authentifizieren.
    ssh-add /home/runner/.ssh/gha

    # Die Konfigurationsdatei schreiben, um OpenSSH zu zeigen, wo sich der Schlüssel für den Host `github.com` befindet.
    echo -e "Host github.com\n    HostName github.com\n    User git\n    IdentityFile /home/runner/.ssh/gha" > /home/runner/.ssh/config

- name: build
  run: swift build -c release
{% endraw %}
```

Das wars.