# Der einfachste Pfannkuchen der Welt

## Zutaten

Für vier Personen

|Zutat|Menge|
|:--|:--|
|Mehl|400g|
|Hafermilch[^1]|750ml|
|Eier|3 Mittelgroße bis Große|
|Butter|Für die Pfanne|
|Salz|Eine Prise|

## Zubereitung

```mermaid
sequenceDiagram
    autonumber
    
    Zutaten ->> Schüssel: Mehl
    activate Schüssel
    Zutaten ->> Schüssel: Hafermilch
    Zutaten ->> Schüssel: Eier
    Zutaten ->> Schüssel: Salz
    Schüssel ->> Kühlschrank: Teig verrühren bis er Blasen wirft
    deactivate Schüssel
    activate Kühlschrank
    Kühlschrank ->> Kühlschrank: 30 Minuten ruhen lassen
    Kühlschrank ->> Schüssel: Optional etwas Mineralwasser hinzugeben
    deactivate Kühlschrank
    activate Pfanne
    Pfanne ->> Pfanne: Auf Herd aufheizen bis Wärme ca 10cm über dem Boden fühlbar
    
    loop Für jeden Pfannkuchen wiederholen
        Zutaten ->> Pfanne: Butter hinzugeben und gleichmäßig verteilen
        Schüssel ->> Pfanne: Volle Schöpfkelle Teig in die Pfanne geben
        Pfanne ->> Pfanne: Teig gleichmäßig verteilen
        Pfanne ->> Pfanne: Pfannkuchen wenden sobald der Boden goldgelb und fest ist
        Pfanne ->> Teller: Pfannkuchen servieren
        deactivate Pfanne
        activate Teller
    end
    Teller ->> Bekochte: Essen
    deactivate Teller
```

[^1]: Oatly Barista