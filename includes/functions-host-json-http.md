---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 35b087d13099b975a1c9c6d2dbd449935f5f0d1d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66248857"
---
```json
{
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 200,
        "maxConcurrentRequests": 100,
        "dynamicThrottlesEnabled": true
    }
}
```

|Eigenschap  |Standaard | Description |
|---------|---------|---------| 
|routePrefix|api|Het voorvoegsel van de route die van toepassing op alle routes. Gebruik een lege tekenreeks te verwijderen van het standaardvoorvoegsel. |
|maxOutstandingRequests|200<sup>*</sup>|Het maximale aantal openstaande aanvragen die zijn ondergebracht op een bepaald moment. Deze limiet is inclusief aanvragen die in de wachtrij staan, maar niet wordt uitgevoerd, evenals in voortgang uitvoeringen zijn begonnen. Alle inkomende aanvragen via deze limiet worden met een 'Bezet' fout 429 geweigerd. Die kunt aanroepers te gebruiken strategieën voor opnieuw proberen op basis van tijd en ook helpt u bij het bepalen van de maximale aanvraag latenties. Hiermee bepaalt u alleen queuing die binnen het pad van script host worden uitgevoerd plaatsvindt. Andere wachtrijen, zoals de aanvraagwachtrij voor ASP.NET is nog steeds van kracht en wordt niet beïnvloed door deze instelling. <sup>*</sup>De standaardwaarde voor versie 1.x is niet-gebonden (`-1`). De standaardwaarde voor versie 2.x in een verbruiksabonnement is 200. De standaardwaarde voor versie 2.x in een toegewezen planning is niet-gebonden (`-1`).|
|maxConcurrentRequests|100<sup>*</sup>|Het maximum aantal http-functies die parallel worden uitgevoerd. Hiermee kunt u besturingselement gelijktijdigheid van taken, die kan helpen met het gebruik van resources te beheren. Bijvoorbeeld, kan er een http-functie die gebruikmaakt van een groot aantal systeembronnen (geheugen/cpu/sockets), zodat het ervoor zorgt problemen dat wanneer gelijktijdigheid te hoog is. Of mogelijk hebt u een functie waarmee uitgaande aanvragen naar een externe service en deze aanroepen moeten worden snelheidsbeperking van toepassing. In dergelijke gevallen kan toepassen van een vertraging hier helpen. <sup>*</sup>De standaardwaarde voor versie 1.x is niet-gebonden (`-1`). De standaardwaarde voor versie 2.x in een verbruiksabonnement is 100. De standaardwaarde voor versie 2.x in een toegewezen planning is niet-gebonden (`-1`).|
|dynamicThrottlesEnabled|true<sup>*</sup>|Wanneer dit is ingeschakeld, worden deze instelling wordt de pijplijn voor aanvraagverwerking periodiek controleren de systeemprestaties items zoals threads/verbindingen/processen/cpu/geheugen/enzovoort en als een van deze items zijn over een ingebouwde hoge drempelwaarde (80%), vraagt geweigerd met een 'Bezet' fout 429 totdat de teller (s) terug naar de normale niveaus terechtkomen. <sup>*</sup>De standaardwaarde voor versie 1.x is ingesteld op false. De standaardwaarde voor versie 2.x in een verbruiksabonnement is ingesteld op true. De standaardwaarde voor versie 2.x in een toegewezen planning is ingesteld op false.|
