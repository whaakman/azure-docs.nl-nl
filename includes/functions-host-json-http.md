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
|maxOutstandingRequests|-1|Het maximale aantal openstaande aanvragen die op elk moment worden bewaard. Deze limiet omvatten aanvragen die in de wachtrij staan, maar wordt uitgevoerd, evenals een voortgang uitvoeringen nog niet begonnen. Alle binnenkomende aanvragen gedurende deze limiet worden met een 429 'Bezet' antwoord geweigerd. Dat kunt aanroepfuncties alvast op basis van tijd nieuwe pogingen strategieën en ook helpt u bij het bepalen van de maximale aanvraag latenties. Hiermee bepaalt u alleen queuing die wordt uitgevoerd binnen het pad script host worden uitgevoerd. Andere wachtrijen zoals de aanvraagwachtrij ASP.NET nog steeds van kracht en niet is beïnvloed door deze instelling. De standaardwaarde is unbounded.|
|maxConcurrentRequests|-1|Het maximum aantal http-functies die parallel worden uitgevoerd. Hiermee kunt u besturingselement gelijktijdigheid van taken, die kan helpen bij bronnen beter worden benut. Bijvoorbeeld wellicht een http-functie die gebruikmaakt van een groot aantal systeembronnen (cpu-geheugen/sockets) zodanig dat deze problemen veroorzaakt wanneer gelijktijdigheid van taken te hoog is. Of moet u wellicht een functie die uitgaande aanvragen aan een derde partij service maakt en deze aanroepen moeten worden beperkt snelheid. In dergelijke gevallen kunt toepassen van een vertraging hier u. De standaardwaarde is unbounded.|
|dynamicThrottlesEnabled|onwaar|Wanneer dit is ingeschakeld, worden deze instelling past de pipeline die aanvragen verwerkt voor het periodiek controleren op de systeemprestaties prestatiemeteritems gebruikt, zoals threads/verbindingen/processen/cpu/geheugen/enzovoort, en als een van deze items via een ingebouwde hoge drempelwaarde (80%), aanvragen geweigerd met een 429 'Bezet' antwoord totdat de counter(s) terug naar de normale niveaus terechtkomen.|
