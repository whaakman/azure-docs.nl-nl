---
title: Methode - Knowledge Exploration Service API interpreteren
titlesuffix: Azure Cognitive Services
description: Informatie over het gebruik van de methode interpreteren in de Knowledge Exploration Service KES () API.
services: cognitive-services
author: bojunehsu
manager: cgronlun
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 4f950734081be6cf76770b1c6cb2feca7efdae70
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55221400"
---
# <a name="interpret-method"></a>Methode interpreteren

De *interpreteren* methode neemt een querytekenreeks van natuurlijke taal en retourneert interpretaties van de bedoeling van gebruikers op basis van de grammatica en index gegevens opgemaakt.  Als u wilt een interactieve zoekopdracht-ervaring bieden, kan deze methode worden aangeroepen als elk teken wordt ingevoerd door de gebruiker met de *voltooid* parameter ingesteld op 1 om in te schakelen van automatisch aanvullen suggesties.

## <a name="request"></a>Aanvraag

`http://<host>/interpret?query=<query>[&<options>]`

Name|Value| Beschrijving
----|----|----
query    | Tekenreeks met tekst | De query is opgegeven door de gebruiker.  Als voltooid is ingesteld op 1, wordt de query worden geïnterpreteerd als een voorvoegsel voor genereren suggesties voor het automatisch aanvullen van query.        
Voltooien | 0 (standaardinstelling) of 1 | 1 betekent dat er automatisch aanvullen suggesties worden gegenereerd op basis van de gegevens en index grammatica.         
count    | Getal (standaard = 10) | Maximum aantal interpretaties om terug te keren.         
offset   | Getal (standaard = 0) | De index van de eerste interpretatie om terug te keren.  Bijvoorbeeld, *count = 2 & offset 0 =* retourneert interpretaties 0 en 1. *aantal = 2 & offset = 2* retourneert interpretaties 2 en 3.       
timeout  | Getal (standaard = 1000) | Time-out in milliseconden. Alleen interpretaties gevonden voordat de time-out is verstreken, worden geretourneerd.

Met behulp van de *aantal* en *offset* parameters, een groot aantal resultaten kan worden verkregen incrementeel ten opzichte van meerdere aanvragen.

## <a name="response-json"></a>Antwoord (JSON)

JSONPath     | Beschrijving
---------|---------
$.query |*query* parameter van de aanvraag.
$.interpretations   |Matrix van 0 of meer manieren zodat deze overeenkomen met de invoer voor de query op basis van de grammatica.
$.interpretations[\*].logprob   |Relatieve log kans van de interpretatie (< = 0).  Hogere waarden waarschijnlijk meer.
$.interpretations[\*].parse |XML-tekenreeks die laat zien hoe elk onderdeel van de query is geïnterpreteerd.
$.interpretations[\*].rules |Matrix van 1 of meer regels zoals gedefinieerd in de grammatica aangeroepen tijdens de interpretatie.
$.interpretations[\*].rules[\*].name    |naam van de regel.
$.interpretations[\*].rules[\*].output  |Semantische uitvoer van de regel.
$.interpretations[\*].rules[\*].output.type |Het gegevenstype van de semantische uitvoer.
$.interpretations[\*].rules[\*].output.value|De waarde van de semantische uitvoer.  
$.aborted | Waar, als de aanvraag is een time-out.

### <a name="parse-xml"></a>Parse XML

Het parseren van XML annotates de (voltooid) query met informatie over hoe deze overeenkomt met op basis van de regels in de grammatica en kenmerken in de index.  Hieronder volgt een voorbeeld van het domein academische publicaties:

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

De `<rule>` element begrenst het bereik in de query die overeenkomt met de regel die is opgegeven door de `name` kenmerk.  Het kan worden genest bij het parseren van verwijzingen van de regel in de grammatica omvat.

De `<attr>` element begrenst het bereik in de query die overeenkomt met de indexkenmerk dat is opgegeven door de `name` kenmerk.  Wanneer de overeenkomst een synoniem in de invoer voor de query moet, de `canonical` kenmerk bevat de canonieke waarde die overeenkomt met het synoniem uit de index.

## <a name="example"></a>Voorbeeld

In het voorbeeld academische publicaties retourneert de volgende aanvraag maximaal 2 automatisch aanvullen suggesties voor de query voorvoegsel 'documenten door jaime':

`http://<host>/interpret?query=papers by jaime&complete=1&count=2`

Het antwoord bevat de twee bovenste ("count = 2") waarschijnlijk een perfecte ervaring bij die de gedeeltelijke query "documenten door jaime" voltooien: "papers door jaime teevan" en "papers jaime groen".  De service gegenereerde query voltooiingen in plaats van overweegt alleen exacte overeenkomsten voor de auteur 'jaime' omdat de aanvraag opgegeven "voltooid = 1". Houd er rekening mee dat de canonieke waarde "groen" j "l" overeenkomende via het synoniem 'Jan groen', zoals aangegeven in het parseren.


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

Als het type van semantische uitvoer 'query', zoals in dit voorbeeld wordt de overeenkomende objecten kunnen worden opgehaald door door te geven *output.value* naar de [ *evalueren* ](evaluateMethod.md) API via de *expr* parameter.

`http://<host>/evaluate?expr=Composite(AA.AuN=='jaime teevan')`
  
