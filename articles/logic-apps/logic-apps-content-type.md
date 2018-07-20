---
title: Inhoudstypen - Azure Logic Apps verwerken | Microsoft Docs
description: Meer informatie over Logic Apps-verwerkt type inhoud tijdens het ontwerpen en uitvoeringstijd
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: article
ms.date: 07/20/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 82eb9c895f016efe569651dc89885d2e4850fd59
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39159088"
---
# <a name="handle-content-types-in-azure-logic-apps"></a>Inhoudstypen in Azure Logic Apps verwerken

Verschillende typen inhoud kunnen door een logische app, bijvoorbeeld, JSON, XML, platte bestanden en binaire gegevens stromen. Terwijl de Logic Apps biedt ondersteuning voor alle typen inhoud, enkele systeemeigen ondersteuning bieden en vereisen casten of conversie in uw logische apps. Andere typen mogelijk casten of conversie indien nodig. Dit artikel wordt beschreven hoe logische Apps inhoudstypen verwerken en hoe u kunt correct cast-conversie uitvoeren of converteert deze typen wanneer dat nodig is.

Om te bepalen van de juiste methode voor de afhandeling van inhoudstypen, Logic Apps is afhankelijk van de `Content-Type` headerwaarde in HTTP-aanroepen, bijvoorbeeld:

* [Application/json](#application-json) (systeemeigen type)
* [text/plain](#text-plain) (systeemeigen type)
* [Application/xml en application/octet-stream](#application-xml-octet-stream)
* [Andere typen inhoud](#other-content-types)

<a name="application-json"></a>

## <a name="applicationjson"></a>application/json

Logic Apps worden opgeslagen en verwerkt elke aanvraag met de *application/json* inhoudstype als JavaScript Notation (JSON)-object. Standaard kunt u JSON-inhoud zonder een casten parseren. Voor het parseren van een aanvraag die een koptekst met het type ' application/json' inhoud heeft, kunt u een expressie. In dit voorbeeld retourneert de waarde `dog` uit de `animal-type` matrix zonder casten: 
 
`@body('myAction')['animal-type'][0]` 
  
  ```json
  {
    "client": {
       "name": "Fido",
       "animal-type": [ "dog", "cat", "rabbit", "snake" ]
    }
  }
  ```

Als u met JSON-gegevens die een koptekst niet opgeeft werkt, kunt u handmatig die gegevens naar JSON converteren met behulp van de [json() functie](../logic-apps/workflow-definition-language-functions-reference.md#json), bijvoorbeeld: 
  
`@json(triggerBody())['animal-type']`

### <a name="create-tokens-for-json-properties"></a>Maken van tokens voor JSON-eigenschappen

Logic Apps biedt de mogelijkheid om u voor het genereren van gebruiksvriendelijke tokens die staan voor de eigenschappen in JSON-inhoud zodat u kunt verwijzen naar en deze eigenschappen in uw logic app-werkstroom eenvoudiger te gebruiken.

* **Trigger voor de aanvraag**

  Wanneer u deze trigger in de Logic App Designer, kunt u opgeven dat een JSON-schema dat de nettolading die u verwacht beschrijft te ontvangen. 
  De ontwerpfunctie wordt geparseerd JSON-inhoud met behulp van dit schema en genereert gebruiksvriendelijke tokens die staan voor de eigenschappen in uw JSON-inhoud. 
  U kunt vervolgens gemakkelijk verwijzen naar en gebruiken van deze eigenschappen in de gehele werkstroom van uw logische app. 
  
  Als u een schema niet hebt, kunt u het schema genereren. 
  
  1. Selecteer in de aanvraagtrigger **voorbeeldnettolading gebruiken voor het genereren van schema**.  
  
  2. Onder **typt of plakt u een voorbeeld-JSON-nettolading**, Geef een voorbeeld van payload en kies vervolgens **gedaan**. Bijvoorbeeld: 

     ![Geef de voorbeeld-JSON-nettolading](./media/logic-apps-content-type/request-trigger.png)

     Het gegenereerde schema wordt nu weergegeven in de trigger.

     ![Geef de voorbeeld-JSON-nettolading](./media/logic-apps-content-type/generated-schema.png)

     Hier volgt de definitie van de onderliggende aanvraagtrigger in de weergave-editor:

     ```json
     "triggers": { 
        "manual": {
           "type": "Request",
           "kind": "Http",
           "inputs": { 
              "schema": {
                 "type": "object",
                 "properties": {
                    "client": {
                       "type": "object",
                       "properties": {
                          "animal-type": {
                             "type": "array",
                             "items": {
                                "type": "string"
                             },
                          },
                          "name": {
                             "type": "string"
                          }
                       }
                    }
                 }
              }
           }
        }
     }
     ```

  3. Zorg ervoor dat u in uw aanvraag een `Content-Type` header en stel de waarde van de koptekst op `application/json`.

* **De actie JSON parseren**

  Wanneer u deze actie in de Logic App Designer, kunt u JSON-uitvoer te parseren en gebruiksvriendelijke tokens die staan voor de eigenschappen in uw JSON-inhoud genereren. 
  U kunt vervolgens gemakkelijk verwijzen naar en gebruiken van deze eigenschappen in de gehele werkstroom van uw logische app. Net als bij de aanvraagtrigger, kunt u opgeven of genereren een JSON-schema dat beschrijft de JSON-inhoud die u wilt parseren. 
  Op die manier kunt u gemakkelijker gegevens uit Azure Service Bus en Azure Cosmos DB gebruiken.

  ![JSON parseren](./media/logic-apps-content-type/parse-json.png)

<a name="text-plain"></a>

## <a name="textplain"></a>text/plain

Als uw logische app ontvangt voor HTTP-berichten waarvoor de `Content-Type` header ingesteld op `text/plain`, uw logische app de berichten worden opgeslagen in onbewerkte vorm. Als u deze berichten in de volgende acties zonder casten opnemen, aanvragen gaat met de `Content-Type` header ingesteld op `text/plain`. 

Bijvoorbeeld, wanneer u met een plat bestand werkt, krijgt u mogelijk een HTTP-aanvraag met de `Content-Type` header ingesteld op `text/plain` type inhoud:

`Date,Name,Address`</br>
`Oct-1,Frank,123 Ave`

Als u vervolgens deze aanvraag op in een latere actie als wanneer u de instantie van een andere aanvraag, bijvoorbeeld verzendt `@body('flatfile')`, die tweede aanvraag heeft ook een `Content-Type` -header die ingesteld op `text/plain`. Als u met gegevens die als tekst zonder opmaak, maar een koptekst niet hebt gespecificeerd werkt, kunt u handmatig die gegevens naar tekst converteren met behulp van de [string() functie](../logic-apps/workflow-definition-language-functions-reference.md#string) zoals deze expressie: 

`@string(triggerBody())`

<a name="application-xml-octet-stream"></a>

## <a name="applicationxml-and-applicationoctet-stream"></a>Application/xml en application/octet-stream

Logic Apps bewaart altijd het `Content-Type` in een ontvangen HTTP-aanvraag of antwoord. Dus als uw logische app inhoud met ontvangt `Content-Type` ingesteld op `application/octet-stream`, en u dat de inhoud van een hoger opnieuw zonder het casten, de uitgaande aanvraag ook heeft `Content-Type` ingesteld op `application/octet-stream`. Op die manier kunnen kunt logische Apps garanderen dat gegevens niet verloren gaan ophalen tijdens het verplaatsen van via de werkstroom. Echter, de status van de actie, of de invoer en uitvoer, wordt opgeslagen in een JSON-object terwijl de status wordt verplaatst via de werkstroom. 

## <a name="converter-functions"></a>Conversieprogramma voor functies

Als u wilt behouden bepaalde gegevenstypen, Logic Apps inhoud converteert naar een binaire base64-gecodeerde tekenreeks met de juiste metagegevens die beide behoudt de `$content` nettolading en het `$content-type`, die automatisch worden geconverteerd. 

Deze lijst wordt beschreven hoe inhoud door Logic Apps worden geconverteerd wanneer u deze [functies](../logic-apps/workflow-definition-language-functions-reference.md):

* `json()`: Gegevens webcasts `application/json`
* `xml()`: Gegevens webcasts `application/xml`
* `binary()`: Gegevens webcasts `application/octet-stream`
* `string()`: Gegevens webcasts `text/plain`
* `base64()`: Inhoud converteert naar een Base 64-tekenreeks
* `base64toString()`: Een base64-gecodeerde tekenreeks die moet worden geconverteerd `text/plain`
* `base64toBinary()`: Een base64-gecodeerde tekenreeks die moet worden geconverteerd `application/octet-stream`
* `encodeDataUri()`: Een tekenreeks als een bytematrix van gegevens-URI die codeert
* `decodeDataUri()`: Decodeert een `dataUri` in een matrix van bytes

Bijvoorbeeld, als u een HTTP-aanvraag ontvangt waar `Content-Type` ingesteld op `application/xml`, zoals deze inhoud:

```html
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

U kunt deze inhoud casten met behulp van de `@xml(triggerBody())` expressie met de `xml()` en `triggerBody()` functies en gebruik vervolgens deze inhoud later opnieuw. Of u kunt de `@xpath(xml(triggerBody()), '/CustomerName')` expressie met de `xpath()` en `xml()` functies. 

## <a name="other-content-types"></a>Andere typen inhoud

Logische Apps werkt bij en biedt ondersteuning voor andere typen inhoud, maar mogelijk dat u handmatig de berichttekst door het decoderen van krijgen de `$content` variabele.

Stel bijvoorbeeld dat uw logische app wordt geactiveerd door een aanvraag met de `application/x-www-url-formencoded` type inhoud. Alle gegevens, behouden de `$content` variabele in de aanvraagtekst bevat een nettolading die gecodeerd als een met base64-tekenreeks:

`CustomerName=Frank&Address=123+Avenue`

Omdat de aanvraag niet als tekst zonder opmaak of JSON, is de aanvraag opgeslagen in de actie als volgt:

```json
"body": {
   "$content-type": "application/x-www-url-formencoded",
   "$content": "AAB1241BACDFA=="
}
```

Logic Apps biedt systeemeigen functies voor het verwerken van gegevens, bijvoorbeeld: 

* [triggerFormDataValue()](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataValue)
* [triggerFormDataMultiValues()](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataMultiValues)
* [formDataValue()](../logic-apps/workflow-definition-language-functions-reference.md#formDataValue) 
* [formDataMultiValues()](../logic-apps/workflow-definition-language-functions-reference.md#formDataMultiValues)

Of u kunt handmatig toegang tot de gegevens met behulp van een expressie, zoals in dit voorbeeld:

`@string(body('formdataAction'))` 

Als u de uitgaande aanvraag hebben dezelfde `application/x-www-url-formencoded` inhoudstype-header, u kunt toevoegen de aanvraag aan de hoofdtekst van de actie zonder eventuele casten met behulp van een expressie zoals `@body('formdataAction')`. Maar deze methode werkt alleen als de instantie is de enige parameter in de `body` invoer. Als u probeert te gebruiken de `@body('formdataAction')` expressie in een `application/json` aanvragen, treedt er een runtime-fout omdat de hoofdtekst van het gecodeerde wordt verzonden.
