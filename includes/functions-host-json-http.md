```json
{
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    }
}
```

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------| 
|routePrefix|api|Het routeprefix die voor alle routes geldt. Gebruik een lege tekenreeks om te verwijderen van het standaardvoorvoegsel. |
|maxOutstandingRequests|-1|Het maximale aantal openstaande aanvragen die op elk moment (-1 betekent unbounded) wordt gehouden. De limiet omvatten aanvragen die in de wachtrij staan, maar nog niet begonnen wordt uitgevoerd, evenals alle uitvoeringen wordt uitgevoerd. Alle binnenkomende aanvragen gedurende deze limiet worden met een 429 'Bezet' antwoord geweigerd. Aanroepfuncties kunnen reactie op basis van tijd nieuwe pogingen strategieën alvast gebruiken. Deze instelling bepaalt u alleen queuing die deze gebeurtenis treedt op binnen het pad taak host worden uitgevoerd. Er zijn andere wachtrijen, zoals de aanvraagwachtrij ASP.NET niet is beïnvloed door deze instelling. |
|maxConcurrentRequests|-1|Het maximum aantal HTTP-functies die parallel (-1 betekent unbounded) wordt uitgevoerd. U kunt bijvoorbeeld een limiet instellen als uw HTTP-functies te veel systeembronnen gebruikt als gelijktijdigheid hoog is. Of als uw functies uitgaande aanvragen in een service van derden aanbrengen, deze aanroepen moeten mogelijk worden snelheid beperkt.|
|dynamicThrottlesEnabled|onwaar|Zorgt ervoor dat de verwerking van aanvragen pijplijn systeemprestatiemeteritems periodiek controleren. Prestatiemeteritems zijn verbindingen, threads, processen, geheugen en cpu. Als een van de items via een ingebouwde drempelwaarde (80%), worden aanvragen met een 429 'Bezet' antwoord geweigerd totdat de counter(s) terug naar de normale niveaus terechtkomen.|
