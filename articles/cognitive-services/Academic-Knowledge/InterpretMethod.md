---
title: Methode - Academic Knowledge API interpreteert
titlesuffix: Azure Cognitive Services
description: Gebruik de methode interpreteren om terug te keren opgemaakte interpretaties van querytekenreeksen op basis van gegevens van de Academic Graph en de academische grammatica in Microsoft Cognitive Services.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: b679f1da0ada3e61fca79cdb985a43dc445877ce
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57975488"
---
# <a name="interpret-method"></a>Methode interpreteren

De **interpreteren** REST-API gebruikt een eindgebruiker queryreeks (dat wil zeggen, een query die is ingevoerd door een gebruiker van uw toepassing) en retourneert interpretaties van de bedoeling van gebruikers op basis van de Academic Graph-gegevens en de academische grammatica geformatteerd.

Voor een interactieve ervaring, kunt u deze methode herhaaldelijk aanroept na elke teken ingevoerd door de gebruiker. In dat geval moet u instellen de **voltooid** parameter in op 1 om in te schakelen van automatisch aanvullen suggesties. Als uw toepassing niet automatisch aanvullen hoeft, moet u instellen de **voltooid** parameter op 0.

**REST-eindpunt:**

    https://westus.api.cognitive.microsoft.com/academic/v1.0/interpret?

## <a name="request-parameters"></a>Aanvraagparameters

Name     | Value | Vereist?  | Description
---------|---------|---------|---------
**query**    | Tekenreeks met tekst | Ja | De query is opgegeven door de gebruiker.  Als voltooid is ingesteld op 1, wordt de query worden geïnterpreteerd als een voorvoegsel voor genereren suggesties voor het automatisch aanvullen van query.        
**model**    | Tekenreeks met tekst | Nee  | De naam van het model dat u wilt zoeken.  Op dit moment wordt de standaardwaarde aan *nieuwste*.        
**Voltooien** | 0 of 1 | Nee<br>Standaard: 0  | 1 betekent dat er automatisch aanvullen suggesties worden gegenereerd op basis van de grammatica en graph-gegevens.         
**count**    | Aantal | Nee<br>Standaard: 10 | Maximum aantal interpretaties om terug te keren.         
**offset**   | Aantal | Nee<br>Standaard: 0  | De index van de eerste interpretatie om terug te keren. Bijvoorbeeld, *count = 2 & offset 0 =* retourneert interpretaties 0 en 1. *aantal = 2 & offset = 2* retourneert interpretaties 2 en 3.       
**Time-out**  | Aantal | Nee<br>Standaard: 1000 | Time-out in milliseconden. Alleen interpretaties gevonden voordat de time-out is verstreken, worden geretourneerd.

<br>
  
## <a name="response-json"></a>Antwoord (JSON)

Name     | Description
---------|---------
**query** |De *query* parameter van de aanvraag.
**interpretations** |Een matrix van 0 of meer verschillende methoden die overeenkomt met de invoer van de gebruiker op basis van de grammatica.
**interpretations[x].logprob**  |De relatieve natuurlijke logboek kans van de vertaling. Hogere waarden waarschijnlijk meer.
**interpretations[x].parse**  |Een XML-tekenreeks die laat zien hoe elk onderdeel van de query is geïnterpreteerd.
**interpretations[x].rules**  |Een matrix met 1 of meer regels, zoals gedefinieerd in de grammatica die tijdens de interpretatie zijn aangeroepen. Voor de Academic Knowledge-API, altijd worden er 1 regel.
**interpretaties [.name .rules [j] x]**  |naam van de regel.
**interpretations[x].rules[y].output**  |De uitvoer van de regel.
**interpretations[x].rules[y].output.type** |Het gegevenstype van de uitvoer van de regel.  Voor de Academic Knowledge-API, wordt dit altijd 'query' zijn.
**interpretations[x].rules[y].output.value**  |De uitvoer van de regel. Voor de Academic Knowledge-API is dit een querytekenreeks van de expressie die kan worden doorgegeven aan de methoden evalueren en calchistogram.
**afgebroken** | Waar, als de aanvraag is een time-out.

<br>

#### <a name="example"></a>Voorbeeld:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/interpret?query=papers by jaime&complete=1&count=2
 ```
<br>De reactie hieronder bevat de twee bovenste (vanwege de parameter *count = 2*) waarschijnlijk een perfecte ervaring bij die de gedeeltelijke gebruikersinvoer voltooid *documenten door jaime*: *documenten door jaime teevan*  en *documenten door jaime groen*.  De service gegenereerde query voltooiingen in plaats van overweegt alleen exacte overeenkomsten voor de auteur van de *jaime* omdat de aanvraag opgegeven *voltooid = 1*. Houd er rekening mee dat de canonieke waarde *"j" l groen* overeenkomende via het synoniem *Jan groen*, zoals aangegeven in het parseren.


```JSON
{
  "query": "papers by jaime",
  "interpretations": [
    {
      "logprob": -12.728,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#AA.AuN\">jaime teevan</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(AA.AuN=='jaime teevan')"
          }
        }
      ]
    },
    {
      "logprob": -12.774,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#AA.AuN\" canonical=\"j l green\">jaime green</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(AA.AuN=='j l green')"
          }
        }
      ]
    }
  ]
}
```  
<br>Gebruiken om op te halen entiteitsresultaten voor een interpretatie, *output.value* uit de **interpreteren** API, en door te geven die in de **evalueren** API via de *markering*  parameter. In dit voorbeeld is de query voor de eerste interpretatie: 
```
evaluate?expr=Composite(AA.AuN=='jaime teevan')
```
 
