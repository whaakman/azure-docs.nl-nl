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
|routePrefix|api|Het voorvoegsel van de route die van toepassing op alle routes. Gebruik een lege tekenreeks te verwijderen van het standaardvoorvoegsel. |
|maxOutstandingRequests|-1|Het maximale aantal openstaande aanvragen die zijn ondergebracht op een bepaald moment. Deze limiet is inclusief aanvragen die in de wachtrij staan, maar niet wordt uitgevoerd, evenals in voortgang uitvoeringen zijn begonnen. Alle inkomende aanvragen via deze limiet worden met een 'Bezet' fout 429 geweigerd. Die kunt aanroepers te gebruiken strategieën voor opnieuw proberen op basis van tijd en ook helpt u bij het bepalen van de maximale aanvraag latenties. Hiermee bepaalt u alleen queuing die binnen het pad van script host worden uitgevoerd plaatsvindt. Andere wachtrijen, zoals de aanvraagwachtrij voor ASP.NET is nog steeds van kracht en wordt niet beïnvloed door deze instelling. De standaardwaarde is niet-gebonden.|
|maxConcurrentRequests|-1|Het maximum aantal http-functies die parallel worden uitgevoerd. Hiermee kunt u besturingselement gelijktijdigheid van taken, die kan helpen met het gebruik van resources te beheren. Bijvoorbeeld, kan er een http-functie die gebruikmaakt van een groot aantal systeembronnen (geheugen/cpu/sockets), zodat het ervoor zorgt problemen dat wanneer gelijktijdigheid te hoog is. Of mogelijk hebt u een functie waarmee uitgaande aanvragen naar een externe service en deze aanroepen moeten worden snelheidsbeperking van toepassing. In dergelijke gevallen kan toepassen van een vertraging hier helpen. De standaardwaarde is niet-gebonden.|
|dynamicThrottlesEnabled|false|Wanneer dit is ingeschakeld, worden deze instelling wordt de pijplijn voor aanvraagverwerking periodiek controleren de systeemprestaties items zoals threads/verbindingen/processen/cpu/geheugen/enzovoort en als een van deze items zijn over een ingebouwde hoge drempelwaarde (80%), vraagt geweigerd met een 'Bezet' fout 429 totdat de teller (s) terug naar de normale niveaus terechtkomen.|
