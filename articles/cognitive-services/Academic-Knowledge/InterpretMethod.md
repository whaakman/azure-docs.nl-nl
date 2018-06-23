---
title: Methode in de Academic Knowledge API interpreteren | Microsoft Docs
description: Gebruik de methode Interpret om terug te keren opgemaakte interpretatie van queryreeksen gebruiker op basis van Academic grafiekgegevens en de Academic grammatica in cognitieve Microsoft-Services.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: a46c792f14fabf6562666d1067ef880bd505741f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344430"
---
# <a name="interpret-method"></a>Methode interpreteren

De **interpreteren** REST-API neemt een eindgebruiker queryreeks (dat wil zeggen, een query die is ingevoerd door een gebruiker van uw toepassing) en retourneert opgemaakte interpretatie van het doel van gebruiker op basis van de Academic grafiekgegevens en Academic grammatica van.

Als u wilt een interactieve ervaring bieden, kunt u deze methode herhaaldelijk aanroepen na elke teken ingevoerd door de gebruiker. In dat geval moet u instellen de **voltooid** parameter in op 1 om suggesties voor automatisch aanvullen. Als uw toepassing niet automatisch aanvullen hoeft, moet u instellen de **voltooid** parameterwaarde 0.

**REST-eindpunt:**

    https://westus.api.cognitive.microsoft.com/academic/v1.0/interpret?

## <a name="request-parameters"></a>Aanvraagparameters

Naam     | Waarde | Vereist?  | Beschrijving
---------|---------|---------|---------
**Query**    | Tekenreeks | Ja | De query is ingevoerd door de gebruiker.  Als voltooid is ingesteld op 1, wordt query beschouwd als een voorvoegsel voor automatisch aanvullen van genereren Querysuggesties.        
**model**    | Tekenreeks | Nee  | Naam van het model dat u wilt zoeken.  Op dit moment wordt de standaardwaarde *nieuwste*.        
**voltooien** | 0 of 1 | Nee<br>standaardwaarde: 0  | 1 betekent dat suggesties voor automatisch aanvullen worden gegenereerd op basis van de grafiek en grammatica.         
**aantal**    | Aantal | Nee<br>Standaard: 10 | Maximum aantal interpretaties om terug te keren.         
**offset**   | Aantal | Nee<br>standaardwaarde: 0  | De index van de eerste interpretatie om terug te keren. Bijvoorbeeld: *count = 2 & offset = 0* interpretatie voor de 0 en 1 geretourneerd. *aantal = 2 & offset = 2* retourneert interpretatie voor de 2 en 3.       
**Time-out**  | Aantal | Nee<br>standaard 1000: | Time-out in milliseconden. Alleen interpretaties gevonden voordat de time-out is verstreken, worden geretourneerd.
<br>
  
## <a name="response-json"></a>Antwoord (JSON)
Naam     | Beschrijving
---------|---------
**Query** |De *query* parameter van de aanvraag.
**interpretaties** |Een matrix met 0 of meer verschillende manieren die overeenkomt met de gebruikersinvoer tegen de grammatica.
**[x] interpretaties .logprob**  |De relatieve natuurlijke logboek kans van de interpretatie. Hogere waarden zijn geneigd.
**[x] interpretaties .parse**  |Een XML-tekenreeks die laat zien hoe u elk deel van de query is geïnterpreteerd.
**[x] interpretaties .rules**  |Een matrix van 1 of meer regels die zijn gedefinieerd in de grammatica die is aangeroepen tijdens de interpretatie van. Voor de Academic Knowledge API, zal altijd er 1 regel.
**interpretaties [.naam .rules [-y] x]**  |De naam van de regel.
**interpretaties [.output .rules [-y] x]**  |De uitvoer van de regel.
**interpretaties [.output.type .rules [-y] x]** |Het gegevenstype van de uitvoer van de regel.  Voor de Academic Knowledge API, zal dit altijd 'query' zijn.
**interpretaties [.output.value .rules [-y] x]**  |De uitvoer van de regel. Voor de Academic Knowledge API is dit een queryreeks van de expressie die kan worden doorgegeven aan de methoden evalueren en calchistogram.
**afgebroken** | True als de aanvraag is een time-out.

<br>
#### <a name="example"></a>Voorbeeld:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/interpret?query=papers by jaime&complete=1&count=2
 ```
<br>Het onderstaande antwoord bevat de twee bovenste (vanwege de parameter *count = 2*) interpretatie voor de meest waarschijnlijke die de gedeeltelijke gebruikersinvoer voltooid *papers door jaime*: *papers door jaime teevan*  en *papers jaime groen*.  De service gegenereerd query voltooiingen in plaats van alleen overweegt exacte komt overeen met de auteur *jaime* omdat de aanvraag opgegeven *voltooid = 1*. Houd er rekening mee dat de canonieke waarde *j l groen* overeenkomende via het synoniem *Jan groen*, zoals aangegeven in het parseren.


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
<br>Gebruiken voor het ophalen van de resultaten van de entiteit voor een interpretatie *output.value* van de **interpreteren** API, en geef die naar de **evalueren** API via de *expr*  parameter. In dit voorbeeld is de query voor de eerste interpretatie: 
```
evaluate?expr=Composite(AA.AuN=='jaime teevan')
```
 