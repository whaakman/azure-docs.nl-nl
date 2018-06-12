---
title: Fout- en uitzonderingsverwerking voor logische Apps in Azure | Microsoft Docs
description: Patronen voor fout- en afhandeling van uitzonderingen in Logic Apps.
services: logic-apps
documentationcenter: ''
author: dereklee
manager: jeconnoc
editor: ''
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: logic-apps
ms.date: 01/31/2018
ms.author: deli; LADocs
ms.openlocfilehash: ee2c4f1408dcb6527220cd3870ab00d83987f471
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300059"
---
# <a name="handle-errors-and-exceptions-in-logic-apps"></a>Voor het afhandelen van fouten en uitzonderingen in Logic Apps

Op de juiste wijze kunnen verwerking uitvaltijd of problemen van afhankelijke systemen een uitdaging voor elke integratiearchitectuur opleveren. Voor het maken van robuuste integraties gebruikt die bestand tegen problemen en fouten zijn biedt Logic Apps een uitstekende voor het afhandelen van fouten en uitzonderingen. 

## <a name="retry-policies"></a>Beleid voor opnieuw proberen

Voor de meest eenvoudige uitzondering en foutafhandeling, kunt u het beleid voor opnieuw proberen. Als een eerste aanvraag is een time-out of mislukt, die elk verzoek dat resulteert in een 429 of 5xx-antwoord, dit beleid wordt gedefinieerd of en hoe de actie nieuwe pogingen voor de aanvraag is. 

Er zijn vier typen beleid voor opnieuw proberen: standaard, geen vaste interval en exponentiële interval. Als de werkstroomdefinitie van de geen een beleid voor opnieuw proberen, wordt het standaardbeleid zoals gedefinieerd door de service gebruikt in plaats daarvan.

Open de Logic App-ontwerper voor uw logische app als u beleid voor opnieuw proberen, instelt, indien van toepassing, en Ga naar **instellingen** voor een specifieke actie in uw logische app. Of u kunt beleid voor opnieuw proberen in definiëren de **invoer** sectie voor een specifieke actie of trigger als herstelbare in de werkstroomdefinitie van de. Hier volgt de algemene syntaxis:

```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

Voor meer informatie over de syntaxis en de **invoer** sectie, raadpleegt u de [beleid voor opnieuw proberen sectie in werkstroomacties en Triggers][retryPolicyMSDN]. Zie voor meer informatie over de beperkingen van het beleid opnieuw [Logic Apps en configuratie](../logic-apps/logic-apps-limits-and-config.md). 

### <a name="default"></a>Standaard

Als u een beleid voor opnieuw proberen in niet definieert de **retryPolicy** sectie uw logische app gebruikmaakt van het standaardbeleid, dit is een [exponentiële interval beleid](#exponential-interval) die maximaal vier pogingen op exponentieel verzendt steeds groter wordende intervallen die worden geschaald door 7.5 seconden. Het interval is tussen 5 en 45 seconden beperkt. Dit beleid is gelijk aan het beleid in dit voorbeeld werkstroomdefinitie HTTP:

```json
"HTTP": {
    "type": "Http",
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "exponential",
            "count": 4,
            "interval": "PT7S",
            "minimumInterval": "PT5S",
            "maximumInterval": "PT1H"
        }
    },
    "runAfter": {}
}
```

### <a name="none"></a>Geen

Als u instelt **retryPolicy** naar **geen**, dit beleid geen nieuwe poging gedaan mislukte aanvragen.

| Elementnaam | Vereist | Type | Beschrijving | 
| ------------ | -------- | ---- | ----------- | 
| type | Ja | Reeks | **Geen** | 
||||| 

### <a name="fixed-interval"></a>Vast interval

Als u instelt **retryPolicy** naar **vaste**, dit beleid opnieuw probeert een mislukte aanvraag door te wachten op het opgegeven tijdsinterval voordat u de volgende aanvraag verzendt.

| Elementnaam | Vereist | Type | Beschrijving |
| ------------ | -------- | ---- | ----------- |
| type | Ja | Reeks | **Vaste** |
| aantal | Ja | Geheel getal | Het aantal pogingen moet tussen 1 en 90 | 
| interval | Ja | Reeks | Het interval in [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), die moet liggen tussen PT5S en PT1D | 
||||| 

<a name="exponential-interval"></a>

### <a name="exponential-interval"></a>Exponentiële interval

Als u instelt **retryPolicy** naar **exponentiële**, dit beleid opnieuw probeert een mislukte aanvraag na een willekeurig tijdsinterval van een exponentieel groeiende bereik. Het beleid ook wordt gegarandeerd dat elke nieuwe poging aan een random interval dat groter is dan verzenden **minimumInterval** en minder dan **maximumInterval**. Exponentiële beleid vereist **aantal** en **interval**, terwijl de waarden voor **minimumInterval** en **maximumInterval** zijn optioneel. Als u de standaardwaarden PT5S en PT1D respectievelijk overschrijven wilt, kunt u deze waarden toevoegen.

| Elementnaam | Vereist | Type | Beschrijving |
| ------------ | -------- | ---- | ----------- |
| type | Ja | Reeks | **exponential** |
| aantal | Ja | Geheel getal | Het aantal pogingen moet tussen 1 en 90  |
| interval | Ja | Reeks | Het interval in [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), die moet liggen tussen PT5S en PT1D. |
| minimumInterval | Nee | Reeks | De minimale interval in [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), die moet liggen tussen PT5S en **interval** |
| maximumInterval | Nee | Reeks | De minimale interval in [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), die moet liggen tussen **interval** en PT1D | 
||||| 

Deze tabel ziet u hoe een uniform willekeurige variabele in het opgegeven bereik wordt gegenereerd voor elke nieuwe poging tot en met **aantal**:

**Willekeurige variabele bereik**

| Probeer getal | Minimuminterval | Maximuminterval |
| ------------ | ---------------- | ---------------- |
| 1 | Max (0, **minimumInterval**) | Min(interval, **maximumInterval**) |
| 2 | Max (interval **minimumInterval**) | Min (2 * interval **maximumInterval**) |
| 3 | Maximum aantal (2 * interval **minimumInterval**) | Min (4 * interval **maximumInterval**) |
| 4 | Max (4 * interval **minimumInterval**) | Min (8 * interval **maximumInterval**) |
| .... | | | 
|||| 

## <a name="catch-and-handle-failures-with-the-runafter-property"></a>Catch- en fouten met de eigenschap RunAfter verwerken

Elke logische app actie declareert de acties die moeten worden voltooid voordat deze actie wordt gestart, vergelijkbaar met hoe u de volgorde van stappen in uw werkstroom opgeven. In de actiedefinitie van een de **runAfter** eigenschap definieert deze volgorde en is een object dat wordt beschreven welke acties en de actie status de actie uitvoeren.

Alle acties die u in de ontwerpfunctie voor Logic App toevoegt zijn standaard moet worden uitgevoerd na de vorige stap, wanneer het resultaat van de vorige stap is **geslaagd**. U kunt echter de **runAfter** zodat acties gestart als de vorige acties als leiden waarde **mislukt**, **overgeslagen**, of een combinatie van deze waarden. Als u bijvoorbeeld een item toevoegen aan een specifieke Service Bus-onderwerp na een specifieke **Insert_Row** actie mislukt, kunt u dit voorbeeld **runAfter** definitie:

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

De **runAfter** eigenschap is ingesteld op uitvoeren wanneer de **Insert_Row** actiestatus is **mislukt**. De actie wordt uitgevoerd als de status van de actie **geslaagd**, **mislukt**, of **overgeslagen**, gebruik de volgende syntaxis:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Acties die worden uitgevoerd en met succes voltooid na een voorgaande actie is mislukt, zijn gemarkeerd als **geslaagd**. Dit gedrag betekent dat als u met succes catch alle fouten in een werkstroom kan de sessie zelf is gemarkeerd als **geslaagd**.

<a name="scopes"></a>

## <a name="evaluate-actions-with-scopes-and-their-results"></a>Handelingen met scopes en de bijbehorende resultaten evalueren

Net als bij de stappen worden uitgevoerd, nadat de afzonderlijke acties met de **runAfter** eigenschap, kunt u groeperen acties binnen een [bereik](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md). U kunt bereiken gebruiken als u wilt logisch groeperen acties, cumulatieve status van het bereik bepalen, en acties uitvoeren op basis van die status. Nadat alle acties in een bereik uitgevoerd zijn, haalt de scope zelf een eigen status. 

Een scope om status te controleren, kunt u dezelfde criteria die u gebruikt een logische app uitvoeren om status te controleren, zoals **geslaagd**, **mislukt**, enzovoort. 

Standaard, wanneer de acties van de scope is gelukt, de status van de scope gemarkeerd als **geslaagd**. Als de laatste actie in een bereik als resulteert **mislukt** of **afgebroken**, de status van de scope is gemarkeerd als **mislukt**. 

Om af te vangen uitzonderingen in een **mislukt** bereik en uitvoeren van acties waarmee deze fouten worden verwerkt, kunt u de **runAfter** eigenschap voor die **mislukt** bereik. Op die manier als *eventuele* acties in de scope, mislukt dit en u gebruikt de **runAfter** eigenschap voor dat bereik, kunt u één actie voor het opsporen van fouten.

Zie voor de limieten voor scopes [limieten en config](../logic-apps/logic-apps-limits-and-config.md).

### <a name="get-context-and-results-for-failures"></a>Context en resultaten krijgen voor fouten

Hoewel het afvangen van fouten van een scope nuttig is, kunt u ook context om te begrijpen precies welke acties mislukt plus eventuele fouten of statuscodes die zijn geretourneerd. De  **@result()** Werkstroomfunctie biedt de context van het resultaat van alle acties in een bereik.

De  **@result()** functie accepteert van een enkele parameter (van de scope-naam) en retourneert een matrix met alle actie resultaten uit binnen dat bereik. Deze actie-objecten bevatten dezelfde kenmerken als de  **@actions()** object, zoals de actie begintijd, eindtijd, status, invoer, correlatie-id's en uitvoer. Als u wilt verzenden context voor acties die is mislukt binnen een bereik, u gemakkelijk kunt combineren een  **@result()** werken met een **runAfter** eigenschap.

Uitvoeren van een actie *voor elk* actie in een bereik dat is een **mislukt** resultaat, en om te filteren op de matrix van de resultaten naar de mislukte acties, kunt u koppel  **@result()** met een **[matrix van Filter](../connectors/connectors-native-query.md)** actie en een **[ForEach](../logic-apps/logic-apps-control-flow-loops.md)** lus. U kunt de gefilterde resultaten matrix en een actie uitvoeren voor elke fout met de **ForEach** lus. 

Hier volgt een voorbeeld, gevolgd door een gedetailleerde uitleg, die een HTTP POST-aanvraag met de hoofdtekst van de reactie van alle acties die niet binnen het bereik 'My_Scope' verzendt:

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

Hier volgt een gedetailleerd overzicht waarin wordt beschreven wat er gebeurt in dit voorbeeld:

1. Ophalen van het resultaat van alle acties in 'My_Scope' de **matrix van Filter** actie filters  **@result(My_Scope)**.

2. De voorwaarde voor **matrix van Filter** is  **@result()** item met de status gelijk zijn aan **mislukt**. Deze voorwaarde filtert de matrix van alle actie resultaten van 'My_Scope' naar een matrix met alleen de resultaten van de mislukte actie.

3. Voer een **voor elk** lus actie op de *gefilterde matrix* levert. Een actie in deze stap wordt uitgevoerd *voor elk* actie resultaat die eerder is gefilterd is mislukt.

   Als één actie in het bereik is mislukt, de acties in de **foreach** slechts eenmaal worden uitgevoerd. 
   Meerdere mislukte acties zorgen ervoor dat één actie per is mislukt.

4. Verzenden van een HTTP POST op de **foreach** item antwoordtekst die  **@item() ['uitvoer'] [hoofdtekst]**. De  **@result()** item vorm is hetzelfde als de  **@actions()** vorm en kan op dezelfde manier worden geparseerd.

5. Twee aangepaste headers met de naam van de mislukte actie opnemen  **@item() [name]** en de mislukte tracerings-ID-client wordt uitgevoerd  **@item() [clientTrackingId]**.

Ter referentie: Hier volgt een voorbeeld van een enkel  **@result()** artikel, waarin de **naam**, **hoofdtekst**, en **clientTrackingId** eigenschappen die in het vorige voorbeeld worden geparseerd. Buiten een **foreach** actie  **@result()** retourneert een matrix van deze objecten.

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

Als u verschillende uitzonderingsverwerking patronen, kunt u de expressies die eerder in dit artikel wordt beschreven. U mogelijk wilt uitvoeren van een enkele uitzonderingsverwerking actie buiten het bereik dat de volledige gefilterde matrix van fouten accepteert en verwijder de **foreach** in te grijpen. U kunt ook andere nuttige eigenschappen van de  **@result()** antwoord zoals eerder beschreven.

## <a name="azure-diagnostics-and-telemetry"></a>Azure diagnoses en telemetrie

De vorige patronen uitstekende manier om te verwerken fouten en uitzonderingen in een uitvoering zijn, maar u kunt ook zien en reageren op fouten, onafhankelijk van de sessie zelf. 
[Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) biedt een eenvoudige manier om alle werkstroomgebeurtenissen, met inbegrip van alle uitvoeren en de actie statussen, aan een Azure Storage-account of een event hub gemaakt met Azure Event Hubs verzenden. 

Om uitvoeren statussen, kunt u Logboeken en metrische gegevens controleren of ze in een controleprogramma dat u wilt publiceren. Een mogelijke mogelijkheid is om te streamen alle gebeurtenissen tot en met Event Hubs in [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). U kunt in de Stream Analytics, live query's op basis van eventuele afwijkingen, gemiddelden of fouten van de diagnostische logboeken schrijven. U kunt de Stream Analytics gebruiken om informatie te verzenden naar andere gegevensbronnen, zoals wachtrijen, onderwerpen, SQL, Azure Cosmos DB of Power BI.

## <a name="next-steps"></a>Volgende stappen

* [Zie hoe een klant builds foutafhandeling met Azure Logic Apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Meer voorbeelden van Logic Apps en scenario's zoeken](../logic-apps/logic-apps-examples-and-scenarios.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9