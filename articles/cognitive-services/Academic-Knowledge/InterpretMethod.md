---
title: Methode interpreteren-Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Gebruik de methode interpret om opgemaakte interpretaties van gebruikers query reeksen te retour neren op basis van academische grafiek gegevens en de academische grammatica in Microsoft Cognitive Services.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: d960aff109e0eca70cb87463770620093e563f63
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706676"
---
# <a name="interpret-method"></a>Methode interpreteren

De **interpret** rest API neemt een query reeks voor eind gebruikers (een query die is ingevoerd door een gebruiker van uw toepassing) en retourneert opgemaakte interpretaties van gebruikers intentie op basis van de academische grafiek gegevens en de academische grammatica.

Als u een interactieve ervaring wilt bieden, kunt u deze methode herhaaldelijk aanroepen na elk teken dat door de gebruiker is ingevoerd. In dat geval moet u de para meter **complete** instellen op 1 om suggesties voor automatisch aanvullen in te scha kelen. Als uw toepassing niet automatisch moet worden voltooid, stelt u de para meter **complete** in op 0.

**REST-eind punt:**

    https://westus.api.cognitive.microsoft.com/academic/v1.0/interpret?

## <a name="request-parameters"></a>Aanvraagparameters

Name     | Value | Vereist?  | Description
---------|---------|---------|---------
**query**    | Tekst teken reeks | Ja | De query is ingevoerd door de gebruiker.  Als volt ooien is ingesteld op 1, wordt de query geïnterpreteerd als een voor voegsel voor suggesties voor het automatisch volt ooien van query's.        
**model**    | Tekst teken reeks | Nee  | De naam van het model dat u wilt opvragen.  Op dit moment wordt de waarde standaard ingesteld op *meest recente*.        
**aangevuld** | 0 of 1 | Nee<br>standaard waarde: 0  | 1 betekent dat suggesties voor automatisch aanvullen worden gegenereerd op basis van de gegevens over de grammatica en de grafiek.         
**count**    | Number | Nee<br>standaard: 10 | Het maximum aantal interpretaties dat moet worden geretourneerd.         
**offset**   | Number | Nee<br>standaard waarde: 0  | Index van de eerste interpretatie die moet worden geretourneerd. *Aantal = 2 & offset = 0* retourneert interpretaties 0 en 1. *aantal = 2 & verschuiving = 2* retourneert interpretaties 2 en 3.       
**out**  | Number | Nee<br>standaard: 1000 | Time-out in milliseconden. Alleen interpretaties die zijn gevonden voordat de time-out is verstreken, worden geretourneerd.

<br>
  
## <a name="response-json"></a>Antwoord (JSON)

Name     | Description
---------|---------
**query** |De *query* parameter van de aanvraag.
**interpretaties** |Een matrix van 0 of meer verschillende manieren om de gebruikers invoer te vergelijken met de grammatica.
**interpretaties [x]. logprob**  |De relatieve natuurlijke logboek kans van de interpretatie. Grotere waarden zijn waarschijnlijker.
**interpretaties [x]. parseren**  |Een XML-teken reeks die laat zien hoe elk deel van de query is geïnterpreteerd.
**interpretaties [x]. regels**  |Een matrix van 1 of meer regels die zijn gedefinieerd in de grammatica die tijdens de interpretatie is aangeroepen. Voor de Academic Knowledge API is er altijd één regel.
**interpretaties [x]. rules [y]. name**  |De naam van de regel.
**interpretations[x].rules[y].output**  |Uitvoer van de regel.
**interpretaties [x]. rules [y]. output. type** |Het gegevens type van de uitvoer van de regel.  Voor de Academic Knowledge API is dit altijd ' query '.
**interpretaties [x]. rules [y]. output. Value**  |De uitvoer van de regel. Voor de Academic Knowledge API is dit een query-expressie teken reeks die kan worden door gegeven aan de methoden evaluate en calchistogram.
**afgebroken** | Waar als er een time-out is opgetreden voor de aanvraag.

<br>

#### <a name="example"></a>Voorbeeld:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/interpret?query=papers by jaime&complete=1&count=2
 ```
<br>De onderstaande reactie bevat de bovenste twee (vanwege het aantal para meters *= 2*) van de meest waarschijnlijke interpretaties die het gedeeltelijke invoer papier van de gebruiker *door Jaime*: *documenten door Jaime teevan* en *documenten door Jaime groen*te volt ooien.  De door de service gegenereerde query is voltooid in plaats van alleen exacte overeenkomsten voor de auteur *Jaime* omdat de aanvraag is opgegeven op *voltooid = 1*. Houd er rekening mee dat de canonieke waarde *j l groen* overeenkomt met het synoniem *Janny*, zoals aangegeven in de parser.


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
<br>Als u de resultaten van de entiteit voor een interpretatie wilt ophalen, gebruikt u *output. Value* van de API interpreteren en geeft u deze door aan de **Evaluate** -API via de para meter *expr* . In dit voor beeld is de query voor de eerste interpretatie: 
```
evaluate?expr=Composite(AA.AuN=='jaime teevan')
```
 
