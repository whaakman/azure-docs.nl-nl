---
title: Fout & uitzonderingsverwerking - Azure Logic Apps | Microsoft Docs
description: Patronen voor fout- en afhandeling van uitzonderingen in Azure Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 9af2f71b3d288cc6f4e271d0915545d43a1249bc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a>Voor het afhandelen van fouten en uitzonderingen in Azure Logic Apps

Logische Apps van Azure biedt uitgebreide hulpprogramma's en om te controleren of uw integraties zijn robuust en robuuste tegen fouten. Integratiearchitectuur vormt het lastiger om ervoor te zorgen voor het afhandelen van op de juiste wijze uitvaltijd of problemen van afhankelijke systemen. Logic Apps kunt u afhandelen van fouten een uitstekende ervaring, zodat u de hulpmiddelen die u nodig hebt om actie te ondernemen uitzonderingen en fouten in uw werkstromen.

## <a name="retry-policies"></a>Beleid voor opnieuw proberen

Een beleid voor opnieuw proberen is het meest eenvoudige type uitzondering en de foutafhandeling. Als een eerste aanvraag is een time-out of mislukt (elke aanvraag die in een 429 resulteert of 5xx-antwoord), dit beleid bepaalt of de actie moet opnieuw proberen. Standaard hebben alle handelingen. opnieuw proberen 4 extra keer via intervallen 20 seconden. Als de eerste aanvraag ontvangt een `500 Internal Server Error` antwoord wordt de werkstroom-engine voor 20 seconden wordt onderbroken en probeert u de aanvraag opnieuw. Als na alle pogingen het antwoord nog steeds een uitzondering of mislukt is, de werkstroom blijft en markeert de status van de actie als `Failed`.

U kunt beleid voor opnieuw proberen in de **invoer** voor een bepaalde actie. U kunt bijvoorbeeld een beleid voor opnieuw proberen om te proberen wel 4 keer via 1 uur. Zie voor volledige informatie over eigenschappen voor de invoer [werkstroomacties en Triggers][retryPolicyMSDN].

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

Als u uw HTTP-actie 4 keer opnieuw proberen en wacht tien minuten tussen elke poging wilt, gebruikt u de volgende definitie:

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

Zie voor meer informatie over ondersteunde syntaxis de [beleid voor opnieuw proberen sectie in werkstroomacties en Triggers][retryPolicyMSDN].

## <a name="catch-failures-with-the-runafter-property"></a>Catch-fouten met de eigenschap RunAfter

Elke logische app actie wordt gedeclareerd welke acties moeten worden voltooid voordat de actie wordt gestart, zoals het bestellen van de stappen in uw werkstroom. In de definitie van de actie, deze volgorde staat bekend als de `runAfter` eigenschap. Deze eigenschap is een object dat wordt beschreven welke acties en de actie status de actie uitvoeren. Standaard worden alle acties die zijn toegevoegd via de Logic App-ontwerper ingesteld op `runAfter` de vorige stap als de vorige stap `Succeeded`. U kunt deze waarde acties gestart wanneer de vorige acties hebben echter aanpassen `Failed`, `Skipped`, of een mogelijke set van deze waarden. Als u wilt een item toevoegen aan een aangewezen Service Bus-onderwerp na een specifieke actie `Insert_Row` mislukt, kunt u de volgende `runAfter` configuratie:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

U ziet de `runAfter` eigenschap is ingesteld moet worden gestart als de `Insert_Row` in te grijpen `Failed`. De actie wordt uitgevoerd als de status van de actie `Succeeded`, `Failed`, of `Skipped`, gebruik de volgende syntaxis:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Acties die worden uitgevoerd en voltooid na een voorgaande actie is mislukt, zijn gemarkeerd als `Succeeded`. Dit gedrag betekent dat als u met succes catch alle fouten in een werkstroom kan de sessie zelf is gemarkeerd als `Succeeded`.

## <a name="scopes-and-results-to-evaluate-actions"></a>Scopes en resultaten acties evalueren

Vergelijkbaar met hoe u kunt uitvoeren nadat de afzonderlijke acties, kunt u ook groeperen acties binnen een [bereik](../logic-apps/logic-apps-loops-and-scopes.md), die fungeren als een logische groepering van acties. Scopes zijn nuttig voor het ordenen van uw logische app acties, zowel voor het uitvoeren van statistische evaluaties van de status van een scope. Het bereik zelf krijgt de status nadat alle acties in een bereik hebt. De status van het bereik wordt bepaald met dezelfde criteria als een uitvoering. Als de laatste actie in een vertakking uitvoering `Failed` of `Aborted`, de status is `Failed`.

Bepaalde acties voor fouten die hebben plaatsgevonden binnen het bereik wordt gestart, kunt u `runAfter` met een bereik dat is gemarkeerd als `Failed`. Als *eventuele* acties in het bereik mislukken, uitgevoerd nadat een scope kunt mislukt u één actie voor het opsporen van fouten.

### <a name="getting-the-context-of-failures-with-results"></a>De context van fouten met resultaten ophalen

Hoewel het afvangen van fouten van een scope nuttig is, kunt u ook context om te begrijpen precies welke bewerkingen is mislukt, en eventuele fouten of statuscodes die zijn geretourneerd. De `@result()` Werkstroomfunctie biedt de context van het resultaat van alle acties in een bereik.

`@result()`neemt een enkele parameter, de naam van het bereik en retourneert een matrix met alle actie resultaten uit binnen dat bereik. Deze actie-objecten bevatten dezelfde kenmerken als de `@actions()` object, met inbegrip van actie-begintijd, eindtijd actie Actiestatus, actie invoer, actie correlatie-id's en actie levert. Als u wilt verzenden context van de acties die is mislukt binnen een bereik, u gemakkelijk kunt combineren een `@result()` werken met een `runAfter`.

Uitvoeren van een actie *voor elk* actie in een bereik dat `Failed`, filter van de matrix van de resultaten naar acties die is mislukt, kan worden gekoppeld `@result()` met een  **[matrix van Filter](../connectors/connectors-native-query.md)**  actie en een  **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)**  lus. U kunt de gefilterde resultaten matrix en een actie uitvoeren voor elke fout met de **ForEach** lus. Hier volgt een voorbeeld, gevolgd door een gedetailleerde beschrijving die stuurt een HTTP POST-aanvraag met de hoofdtekst van de reactie van alle acties die niet binnen het bereik `My_Scope`.

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Hier volgt een gedetailleerd overzicht naar beschreven wat er gebeurt:

1. Ophalen van het resultaat van alle acties in `My_Scope`, wordt de **matrix van Filter** actie filters `@result('My_Scope')`.

2. De voorwaarde voor **matrix van Filter** is `@result()` item met de status is gelijk aan `Failed`. Deze voorwaarde filtert de matrix met alle actie resultaten van `My_Scope` naar een matrix met resultaten van de actie alleen mislukt.

3. Voer een **voor elk** actie op de **gefilterd matrix** levert. Een actie in deze stap wordt uitgevoerd *voor elk* actie resultaat die eerder is gefilterd is mislukt.

    Als één actie in het bereik is mislukt, de acties in de `foreach` slechts eenmaal worden uitgevoerd. 
    Veel mislukte acties zorgen ervoor dat één actie per is mislukt.

4. Verzenden van een HTTP POST op de `foreach` antwoordtekst, item of `@item()['outputs']['body']`. De `@result()` item vorm is hetzelfde als de `@actions()` vorm en kan op dezelfde manier worden geparseerd.

5. Twee aangepaste headers met de naam van de mislukte actie opnemen `@item()['name']` en de mislukte tracerings-ID-client wordt uitgevoerd `@item()['clientTrackingId']`.

Ter referentie: Hier volgt een voorbeeld van een enkel `@result()` artikel, waarin de `name`, `body`, en `clientTrackingId` eigenschappen die in het vorige voorbeeld worden geparseerd. Buiten een `foreach`, `@result()` retourneert een matrix van deze objecten.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/folder-name/resource-name does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

Als u verschillende uitzonderingsverwerking patronen, kunt u de expressies die eerder is weergegeven. U mogelijk wilt uitvoeren van een enkele uitzonderingsverwerking actie buiten het bereik dat de volledige gefilterde matrix van fouten accepteert en verwijder de `foreach`. U kunt ook andere nuttige eigenschappen van de `@result()` antwoord eerder weergegeven.

## <a name="azure-diagnostics-and-telemetry"></a>Azure diagnoses en telemetrie

De vorige patronen uitstekende manier om te verwerken fouten en uitzonderingen in een uitvoering zijn, maar u kunt ook zien en reageren op fouten, onafhankelijk van de sessie zelf. 
[Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) biedt een eenvoudige manier om alle werkstroomgebeurtenissen (met inbegrip van alle uitvoeren en de actie status) verzenden naar een Azure Storage-account of een Azure Event Hub. Om uitvoeren statussen, kunt u de logboeken en metrische gegevens controleren of ze in elke gewenste controleprogramma publiceren. Een mogelijke mogelijkheid is om te streamen alle gebeurtenissen tot en met Azure Event Hub in [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). In de Stream Analytics, kunt u live query's uit alle afwijkingen, gemiddelden of mislukte schrijven van de diagnostische logboeken. Stream Analytics kunt eenvoudig uitvoeren met andere gegevensbronnen zoals wachtrijen, onderwerpen, SQL, Azure Cosmos DB en Power BI.

## <a name="next-steps"></a>Volgende stappen

* [Zie hoe een klant builds foutafhandeling met Azure Logic Apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Meer voorbeelden van Logic Apps en scenario's zoeken](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Informatie over het maken van geautomatiseerde implementaties voor logic apps](../logic-apps/logic-apps-create-deploy-template.md)
* [Logische apps maken en implementeren met Visual Studio](logic-apps-deploy-from-vs.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
