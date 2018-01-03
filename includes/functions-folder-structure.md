
De code voor alle functies in een specifieke functie-app bevindt zich in een hoofdmap met een configuratiebestand van de host en een of meer submappen. Elke submap bevat de code voor een afzonderlijke functie, zoals in het volgende voorbeeld:

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

Het bestand host.json bevat enkele runtime-specifieke configuraties en bevindt zich in de hoofdmap van de functie-app. Zie voor informatie over de instellingen die beschikbaar zijn, de [host.json verwijzing](../articles/azure-functions/functions-host-json.md).

Elke functie heeft een map met een of meer codebestanden, de configuratie van de function.json en andere afhankelijkheden.

