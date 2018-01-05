---
title: Fout- en uitzonderingsverwerking voor logische Apps in Azure | Microsoft Docs
description: Patronen voor fout- en afhandeling van uitzonderingen in Logic Apps.
services: logic-apps
documentationcenter: .net,nodejs,java
author: derek1ee
manager: anneta
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; deli
ms.openlocfilehash: a74c7d18306359c9152f139299de1208b5932fe5
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="handle-errors-and-exceptions-in-logic-apps"></a>Voor het afhandelen van fouten en uitzonderingen in Logic Apps

Logic Apps in Azure biedt uitgebreide hulpprogramma's en patronen om te zorgen dat uw integraties robuust en robuuste tegen fouten zijn. Integratiearchitectuur vormt de uitdaging op de juiste wijze afhandelen uitvaltijd omdat of problemen van afhankelijke systemen. Logic Apps kunt u een uitstekende ervaring afhandeling van fouten. Dit biedt u de hulpmiddelen die u nodig hebt om actie te ondernemen uitzonderingen en fouten in uw werkstromen.

## <a name="retry-policies"></a>Beleid voor opnieuw proberen

Een beleid voor opnieuw proberen is het meest eenvoudige type uitzondering en de foutafhandeling. Als een eerste aanvraag een optreedt time-out of deze mislukt (elke aanvraag die in een 429 resulteert of 5xx-antwoord), een beleid voor opnieuw proberen wordt aangegeven of en hoe de actie moet opnieuw worden geprobeerd. 

Er zijn vier typen beleid voor opnieuw proberen: standaard, geen vaste interval en exponentiële interval. Als een beleid voor opnieuw proberen niet in de werkstroomdefinitie van de opgegeven is, wordt het standaardbeleid zoals gedefinieerd door de service gebruikt. 

U kunt beleid voor opnieuw proberen in de *invoer* voor een bepaalde actie of trigger als het herstelbare. Op deze manier kunt u beleid voor opnieuw proberen (indien van toepassing) in Logic App-ontwerper. Instellen van een beleid voor opnieuw proberen, in Logic App-ontwerper, gaat u naar **instellingen** voor een specifieke actie.

Zie voor meer informatie over de beperkingen van beleid voor opnieuw proberen [Logic Apps en configuratie](../logic-apps/logic-apps-limits-and-config.md). Zie voor meer informatie over ondersteunde syntaxis de [probeer beleidssectie in de werkstroomacties en Triggers][retryPolicyMSDN].

### <a name="default"></a>Standaard

Als u een beleid voor opnieuw proberen niet definieert (**retryPolicy** is niet gedefinieerd), het standaardbeleid wordt gebruikt. Het standaardbeleid is een beleid voor exponentiële interval dat maximaal vier nieuwe pogingen, op exponentieel steeds groter wordende intervallen geschaald met 7.5 seconden verzendt. Het interval is beperkt tot tussen 5 en 45 seconden. Dit standaardbeleid is gelijk aan het beleid in dit voorbeeld werkstroomdefinitie HTTP:

```json
"HTTP":
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "exponential",
            "count": 4,
            "interval": "PT7.5S",
            "minimumInterval": "PT5S",
            "maximumInterval": "PT45S"
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

### <a name="none"></a>None

Als **retryPolicy** is ingesteld op **geen**, een mislukte aanvraag is niet opnieuw geprobeerd.

| Elementnaam | Vereist | Type | Beschrijving |
| ------------ | -------- | ---- | ----------- |
| type | Ja | Tekenreeks | **geen** |

### <a name="fixed-interval"></a>Vast interval

Als **retryPolicy** is ingesteld op **vaste**, het beleid opnieuw probeert een mislukte aanvraag door te wachten op het opgegeven tijdsinterval voordat u de volgende aanvraag verzendt.

| Elementnaam | Vereist | Type | Beschrijving |
| ------------ | -------- | ---- | ----------- |
| type | Ja | Tekenreeks | **vaste** |
| aantal | Ja | Geheel getal | Het aantal nieuwe pogingen. Moet tussen 1 en 90 liggen. |
| interval | Ja | Tekenreeks | Interval in poging [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Moet liggen tussen PT5S en PT1D. |

### <a name="exponential-interval"></a>Exponentiële interval

Als **retryPolicy** is ingesteld op **exponentiële**, het beleid opnieuw probeert een mislukte aanvraag na een willekeurig tijdsinterval van een exponentieel groeiende bereik. Elke nieuwe poging kan worden gegarandeerd worden verzonden aan een random interval dat groter is dan **minimumInterval** en minder dan **maximumInterval**. Een uniform willekeurige variabele in het bereik aangegeven in de volgende tabel wordt gegenereerd voor elke nieuwe poging tot en met **aantal**:

**Willekeurige variabele bereik**

| Probeer getal | Minimuminterval | Maximuminterval |
| ------------ |  ------------ |  ------------ |
| 1 | Max (0, **minimumInterval**) | Min (interval **maximumInterval**) |
| 2 | Max (interval **minimumInterval**) | Min (2 * interval **maximumInterval**) |
| 3 | Maximum aantal (2 * interval **minimumInterval**) | Min (4 * interval **maximumInterval**) |
| 4 | Max (4 * interval **minimumInterval**) | Min (8 * interval **maximumInterval**) |
| ... |

Voor beleid voor exponentiële type **aantal** en **interval** zijn vereist. Waarden voor **minimumInterval** en **maximumInterval** zijn optioneel. U kunt toevoegen om de standaardwaarden van PT5S en PT1D, respectievelijk overschrijven.

| Elementnaam | Vereist | Type | Beschrijving |
| ------------ | -------- | ---- | ----------- |
| type | Ja | Tekenreeks | **exponentiële** |
| aantal | Ja | Geheel getal | Het aantal nieuwe pogingen. Moet tussen 1 en 90 liggen.  |
| interval | Ja | Tekenreeks | Interval in poging [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Moet liggen tussen PT5S en PT1D. |
| minimumInterval | Nee | Tekenreeks | Minimale herhalingsinterval in [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Moet liggen tussen PT5S en **interval**. |
| maximumInterval | Nee | Tekenreeks | Minimale herhalingsinterval in [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Moet liggen tussen **interval** en PT1D. |

## <a name="catch-failures-with-the-runafter-property"></a>Catch-fouten met de eigenschap runAfter

Elke logische app actie wordt gedeclareerd welke acties moeten worden voltooid voordat de actie wordt gestart. Dit is vergelijkbaar met het bestellen van de stappen in uw werkstroom. In de definitie van de actie, deze volgorde staat bekend als de **runAfter** eigenschap. 

De **runAfter** eigenschap is een object dat wordt beschreven welke acties en de actie status de actie uitvoeren. Standaard alle acties die u hebt toegevoegd via Logic App-ontwerper moet worden uitgevoerd na de vorige stap zijn ingesteld als de voorgaande stapresultaat **geslaagd**. 

U kunt echter de **runAfter** waarde acties activeert wanneer de voorgaande acties hebben een resultaat van **mislukt**, **overgeslagen**, of een mogelijke set van deze waarden. Als u wilt een item toevoegen aan een aangewezen Azure Service Bus-onderwerp na een specifieke actie **Insert_Row** mislukt, kunt u de volgende **runAfter** configuratie:

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

Houd er rekening mee dat **runAfter** is ingesteld moet worden gestart als de **Insert_Row** actie resultaat is **mislukt**. De actie wordt uitgevoerd als de status van de actie **geslaagd**, **mislukt**, of **overgeslagen**, gebruik de volgende syntaxis:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Acties die worden uitgevoerd en met succes voltooid na een voorgaande actie is mislukt, zijn gemarkeerd als **geslaagd**. Dit betekent dat als u met succes catch alle fouten in een werkstroom kan de sessie zelf is gemarkeerd als **geslaagd**.

## <a name="scopes-and-results-to-evaluate-actions"></a>Scopes en resultaten acties evalueren

U kunt groeperen acties binnen een [bereik](../logic-apps/logic-apps-loops-and-scopes.md), vergelijkbaar met de manier waarop u na afzonderlijke acties uitvoeren. Een scope fungeert een logische groepering van acties. 

Scopes zijn nuttig voor het ordenen van uw logische App-acties, zowel voor het uitvoeren van statistische evaluaties van de status van een scope. Het bereik zelf krijgt de status nadat alle acties in een bereik hebt. De status van het bereik wordt bepaald met dezelfde criteria als een uitvoering. Als de laatste actie in een vertakking uitvoering **mislukt** of **afgebroken**, de status is **mislukt**.

Bepaalde acties voor fouten die zijn opgetreden binnen het bereik wordt gestart, kunt u **runAfter** met een bereik dat is gemarkeerd als **mislukt**. Als *eventuele* acties in het bereik mislukken, als u **runAfter** voor een scope, kunt u één actie voor het opsporen van fouten.

### <a name="get-the-context-of-failures-with-results"></a>De context van fouten met resultaten ophalen

Hoewel het afvangen van fouten van een scope nuttig is, kunt u ook context om te begrijpen precies welke acties mislukt en om te begrijpen eventuele fouten of statuscodes die zijn geretourneerd. De  **@result()** Werkstroomfunctie biedt de context van het resultaat van alle acties in een bereik.

De  **@result()** functie heeft een enkele parameter (scopenaam) en retourneert een matrix met alle actie resultaten uit binnen dat bereik. Deze actie-objecten bevatten dezelfde kenmerken als de  **@actions()** object, met inbegrip van actie-begintijd, eindtijd actie Actiestatus, actie invoer, actie correlatie-id's en actie levert. 

Als u wilt verzenden context van de acties die is mislukt binnen een bereik, u gemakkelijk kunt combineren een  **@result()** werken met een **runAfter** eigenschap.

Uitvoeren van een actie *voor elk* actie in een bereik dat is een **mislukt** resultaat, en om te filteren op de matrix van de resultaten naar acties die is mislukt, kunt u koppel  **@result()** met een [Filter_array](../connectors/connectors-native-query.md) actie en een [foreach](../logic-apps/logic-apps-loops-and-scopes.md) lus. Met de gefilterde resultaten matrix, kunt u een actie uitvoeren voor elke mislukte met behulp van de **foreach** lus. 

Hier volgt een voorbeeld van een HTTP POST verzendt met de hoofdtekst van de reactie van alle acties die niet in het bereik My_Scope aanvragen:

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

Hier volgt een gedetailleerd overzicht te beschrijven wat gebeurt er in het voorgaande voorbeeld:

1. Ophalen van het resultaat van alle acties in My_Scope, de **Filter_array** actie filters  **@result(My_Scope)**.

2. De voorwaarde voor **Filter_array** is  **@result()** item met de status gelijk zijn aan **mislukt**. Deze voorwaarde filtert de matrix van alle actie resultaten van My_Scope, naar een matrix met resultaten van de enige actie die is mislukt.

3. Voer een **foreach** actie op de *gefilterde matrix* levert. Een actie in deze stap wordt uitgevoerd *voor elk* actie resultaat die eerder is gefilterd is mislukt.

    Als één actie in het bereik is mislukt, de acties in de **foreach** slechts eenmaal worden uitgevoerd. Meerdere mislukte acties zorgen ervoor dat één actie per is mislukt.

4. Verzenden van een HTTP POST op de **foreach** antwoordtekst, item of  **@item() ['uitvoer'] [hoofdtekst]**. De  **@result()** item vorm is hetzelfde als de  **@actions()** vorm. Dit kan op dezelfde manier worden geparseerd.

5. Twee aangepaste headers met de naam van de mislukte actie opnemen  **@item() [name]** en de mislukte tracerings-ID-client wordt uitgevoerd  **@item() [clientTrackingId]**.

Ter referentie: Hier volgt een voorbeeld van een enkel  **@result()** item. Het bevat de **naam**, **hoofdtekst**, en **clientTrackingId** eigenschappen die in het voorgaande voorbeeld worden geparseerd. Buiten een **foreach** actie  **@result()** retourneert een matrix van deze objecten.

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

Voor verschillende patronen verwerking van uitzonderingen, kunt u de expressies die eerder in het artikel is beschreven. U mogelijk wilt uitvoeren van een enkele uitzonderingsverwerking actie buiten het bereik dat de volledige gefilterde matrix van fouten accepteert en verwijder de **foreach**. U kunt ook andere nuttige eigenschappen van de  **@result()** antwoord, zoals eerder beschreven.

## <a name="azure-diagnostics-and-telemetry"></a>Azure diagnoses en telemetrie

De patronen die zijn beschreven in dit artikel bieden geweldige manieren voor het afhandelen van fouten en uitzonderingen in een run, maar u kunt ook zien en reageren op fouten, onafhankelijk van de sessie zelf. [Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) biedt een eenvoudige manier om alle werkstroomgebeurtenissen (met inbegrip van alle uitvoeren en de actie status) verzenden naar Azure storage-account of een event hub in Azure Event Hubs. 

Als u wilt evalueren uitvoeren statussen, kunt u Logboeken en metrische gegevens controleren of op elk controleprogramma dat u wilt publiceren. Een mogelijke optie worden alle gebeurtenissen tot en met Event Hubs voor gestreamd is [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). U kunt in de Stream Analytics, live query's op basis van eventuele afwijkingen, gemiddelden of fouten van de diagnostische logboeken schrijven. U kunt de Stream Analytics gebruiken om informatie te verzenden naar andere gegevensbronnen, zoals wachtrijen, onderwerpen, SQL, Azure Cosmos DB of Power BI.

## <a name="next-steps"></a>Volgende stappen

* Zie hoe een klant [foutafhandeling met logische Apps in Azure maakt](../logic-apps/logic-apps-scenario-error-and-exception-handling.md).
* Meer informatie vinden [Logic Apps-voorbeelden en scenario's](../logic-apps/logic-apps-examples-and-scenarios.md).
* Meer informatie over het maken van [geautomatiseerde implementaties voor logic apps](../logic-apps/logic-apps-create-deploy-template.md).
* Meer informatie over hoe [bouwen en logic apps implementeren met Visual Studio](logic-apps-deploy-from-vs.md).

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
