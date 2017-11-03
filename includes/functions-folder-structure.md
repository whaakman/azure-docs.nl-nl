
De code voor alle functies in een bepaalde functie-app woont in een hoofdmap met een configuratiebestand van de host en een of meer submappen, elk waarvan de code voor een afzonderlijke functie, zoals in het volgende voorbeeld bevat:

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

De *host.json* bestand bevat een runtime-specifieke configuratie en bevindt zich in de hoofdmap van de functie-app. Zie voor informatie over de instellingen die beschikbaar zijn, [host.json verwijzing](../articles/azure-functions/functions-host-json.md).

Elke functie heeft een map met een of meer codebestanden, de configuratie van de function.json en andere afhankelijkheden.

