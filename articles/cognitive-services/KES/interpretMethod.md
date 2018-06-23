---
title: Methode in de API-Service exploratie kennis interpreteren | Microsoft Docs
description: Informatie over het gebruik van de methode Interpret in het Knowledge exploratie Service (KES) API in cognitieve Services.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: ef68d98dacf393abf8d030b9312217ea380947d2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344518"
---
# <a name="interpret-method"></a>Methode interpreteren
De *interpreteren* methode heeft een queryreeks van natuurlijke taal en retourneert opgemaakte interpretatie van het doel van gebruiker op basis van de index en grammatica van.  Voor een interactieve zoekervaring, kan deze methode worden aangeroepen omdat elk teken wordt ingevoerd door de gebruiker met de *voltooid* parameter die is ingesteld op 1 om in te schakelen suggesties voor automatisch aanvullen.

## <a name="request"></a>Aanvraag
`http://<host>/interpret?query=<query>[&<options>]`

Naam|Waarde| Beschrijving
----|----|----
query    | Tekenreeks | De query is ingevoerd door de gebruiker.  Als voltooid is ingesteld op 1, wordt query beschouwd als een voorvoegsel voor automatisch aanvullen van genereren Querysuggesties.        
voltooien | 0 (standaard) of 1 | 1 betekent dat suggesties voor automatisch aanvullen worden gegenereerd op basis van de grammatica- en -gegevens.         
count    | Nummer (standaard = 10) | Maximum aantal interpretaties om terug te keren.         
offset   | Nummer (standaard = 0) | De index van de eerste interpretatie om terug te keren.  Bijvoorbeeld: *count = 2 & offset = 0* interpretatie voor de 0 en 1 geretourneerd. *aantal = 2 & offset = 2* retourneert interpretatie voor de 2 en 3.       
timeout  | Nummer (standaard = 1000) | Time-out in milliseconden. Alleen interpretaties gevonden voordat de time-out is verstreken, worden geretourneerd.

Met behulp van de *aantal* en *offset* parameters, een groot aantal resultaten kan worden verkregen stapsgewijs voor meerdere aanvragen.

## <a name="response-json"></a>Antwoord (JSON)
JSONPath     | Beschrijving
---------|---------
$.query |*query* parameter van de aanvraag.
$.interpretations   |Matrix van 0 of meer manieren om te vergelijken van de invoer-query op de grammatica.
$.interpretations [\*] .logprob   |Relatieve logboek kans van de interpretatie (< = 0).  Hogere waarden zijn geneigd.
$.interpretations [\*] .parse |XML-tekenreeks die laat zien hoe u elk deel van de query is geïnterpreteerd.
$.interpretations [\*] .rules |Matrix van 1 of meer regels die zijn gedefinieerd in de grammatica aangeroepen tijdens de interpretatie van.
$.interpretations [\*] .rules [\*] .naam    |De naam van de regel.
$.interpretations [\*] .rules [\*] .output  |Semantische uitvoer van de regel.
$.interpretations [\*] .rules [\*]. output.type |Het gegevenstype van de semantische uitvoer.
$.interpretations [\*] .rules [\*]. output.value|De waarde van de semantische uitvoer.  
$.aborted | True als de aanvraag is een time-out.

### <a name="parse-xml"></a>XML parseren
Het parseren van XML annotates de (voltooid) query met informatie over hoe deze overeenkomt met tegen de regels in de grammatica en kenmerken in de index.  Hieronder volgt een voorbeeld van het domein academic publicaties:

```xml
<rule name="#GetPapers">
  papers by 
  <attr name="academic#Author.Name" canonical="heungyeung shum">harry shum</attr>
  <rule name="#GetPaperYear">
    written in
    <attr name="academic#Year">2000</attr>
  </rule>
</rule>
```

De `<rule>` element begrenst het bereik in de query die overeenkomt met de regel die is opgegeven door de `name` kenmerk.  Worden kan genest bij het parseren regel verwijzingen in de grammatica omvat.

De `<attr>` element begrenst het bereik in de query die overeenkomt met het indexkenmerk is opgegeven door de `name` kenmerk.  Bij de overeenkomen met een synoniem in de invoerquery omvat de `canonical` kenmerk bevat de canonieke waarde die overeenkomt met het synoniem vanaf de index.

## <a name="example"></a>Voorbeeld
In het voorbeeld academic publicaties retourneert de volgende aanvraag maximaal 2 automatisch aanvullen suggesties voor de query voorvoegsel 'papers door jaime':

`http://<host>/interpret?query=papers by jaime&complete=1&count=2`

Het antwoord bevat de twee bovenste ("count = 2 ') interpretatie voor de meest waarschijnlijke die de gedeeltelijke query 'papers door jaime' voltooien: 'papers door jaime teevan' en 'papers jaime groen'.  De service gegenereerd query voltooiingen in plaats van alleen overweegt exacte komt overeen met de auteur 'jaime' omdat de aanvraag opgegeven ' voltooid = 1". Houd er rekening mee dat de canonieke waarde "groen j l" overeenkomt via het synoniem 'Jan groen', zoals aangegeven in het parseren.


```json
{
  "query": "papers by jaime",
  "interpretations": [
    {
      "logprob": -5.615,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#Author.Name\">jaime teevan</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(Author.Name=='jaime teevan')"
          }
        }
      ]
    },
    {
      "logprob": -5.849,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#Author.Name\" canonical=\"j l green\">jaime green</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(Author.Name=='j l green')"
          }
        }
      ]
    }
  ]
}
```  

Als het type van semantische uitvoer 'vragen', zoals in dit voorbeeld wordt de overeenkomende objecten kunnen worden opgehaald door door te geven *output.value* naar de [ *evalueren* ](evaluateMethod.md) API via de *expr* parameter.

`http://<host>/evaluate?expr=Composite(AA.AuN=='jaime teevan')`
  
