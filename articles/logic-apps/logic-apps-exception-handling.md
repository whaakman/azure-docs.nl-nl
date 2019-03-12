---
title: Fout- en afhandeling van uitzonderingen - Azure Logic Apps | Microsoft Docs
description: Meer informatie over patronen voor fout en de afhandeling van uitzonderingen in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: dereklee
ms.author: deli
manager: jeconnoc
ms.date: 01/31/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 3f812c1142b5cd40169f7340163295b0f7ea6a4d
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57779144"
---
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a>Fouten en uitzonderingen in Azure Logic Apps verwerken

De manier waarop de integratiearchitectuur van een op de juiste wijze verwerkt downtime of problemen veroorzaakt door afhankelijke systemen kan een uitdaging vormen. Voor hulp bij het maken van robuuste en flexibele integraties die zonder problemen verwerking van problemen en fouten, biedt Logic Apps een eersteklas ervaring voor het afhandelen van fouten en uitzonderingen. 

<a name="retry-policies"></a>

## <a name="retry-policies"></a>Beleid opnieuw proberen

Voor de meest eenvoudige uitzondering en de foutafhandeling, kunt u een *beleid voor opnieuw proberen* in een actie of trigger waar dit wordt ondersteund. Een beleid voor opnieuw proberen geeft aan of en hoe de actie of trigger probeert om opnieuw een aanvraag als de oorspronkelijke aanvraag een optreedt time-out of mislukt, dit is een aanvraag die in een 408, 429 en 5xx-antwoord resulteert. Als er geen andere beleid voor opnieuw proberen wordt gebruikt, wordt het standaardbeleid gebruikt. 

Hier volgen de beleidstypen opnieuw proberen: 

| Type | Description | 
|------|-------------| 
| **Standaard** | Dit beleid stuurt maximaal vier nieuwe pogingen op *exponentieel toenemende* intervallen, die door 7,5 seconden schalen, maar zijn beperkt tot tussen 5 en 45 seconden. | 
| **Exponentieel interval**  | Dit beleid wacht een random interval geselecteerd uit een exponentieel toenemende bereik voordat de volgende aanvraag wordt verzonden. | 
| **Vast interval**  | Dit beleid wordt het opgegeven interval gewacht voordat de volgende aanvraag wordt verzonden. | 
| **Geen**  | Geen dien de aanvraag opnieuw. | 
||| 

Zie voor meer informatie over de limieten van het beleid opnieuw proberen [Logic Apps-limieten en configuratie](../logic-apps/logic-apps-limits-and-config.md#request-limits). 

### <a name="change-retry-policy"></a>Beleid voor opnieuw proberen wijzigen

Selecteer een beleid voor verschillende nieuwe pogingen, de volgende stappen uit: 

1. Open uw logische app in Logic App Designer. 

2. Open de **instellingen** voor een actie of trigger.

3. Als de actie of trigger ondersteunt beleid voor opnieuw proberen, onder **beleid voor opnieuw proberen**, selecteert u het gewenste type. 

U kunt ook handmatig opgeven het beleid voor opnieuw proberen in de `inputs` sectie voor een actie of trigger die ondersteuning biedt voor beleid voor opnieuw proberen. Als u een beleid voor opnieuw proberen niet opgeeft, gebruikt de actie het standaardbeleid.

```json
"<action-name>": {
   "type": "<action-type>", 
   "inputs": {
      "<action-specific-inputs>",
      "retryPolicy": {
         "type": "<retry-policy-type>",
         "interval": "<retry-interval>",
         "count": <retry-attempts>,
         "minimumInterval": "<minimum-interval>",
         "maximumInterval": "<maximun-interval>"
      },
      "<other-action-specific-inputs>"
   },
   "runAfter": {}
}
```

*Vereist*

| Value | Type | Description |
|-------|------|-------------|
| <*retry-policy-type*> | String | Het beleidstype dat u wilt gebruiken: `default`, `none`, `fixed`, of `exponential` | 
| <*retry-interval*> | String | Het interval voor opnieuw proberen waarbij de waarde moet gebruiken [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Het minimale interval is standaard `PT5S` en de maximale interval `PT1D`. Wanneer u het beleid voor exponentieel interval gebruikt, kunt u verschillende minimale en maximale waarden opgeven. | 
| <*retry-attempts*> | Geheel getal | Het aantal nieuwe pogingen tussen 1 en 90 moet | 
||||

*Optioneel*

| Value | Type | Description |
|-------|------|-------------|
| <*minimum-interval*> | String | Voor het beleid voor exponentieel interval, de kleinste interval voor het willekeurig gekozen interval in [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) | 
| <*maximum-interval*> | String | Voor het beleid voor exponentieel interval, de grootste interval voor het willekeurig gekozen interval in [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) | 
|||| 

Hier vindt meer informatie over de verschillende beleidstypen.

<a name="default-retry"></a>

### <a name="default"></a>Standaard

Als u een beleid voor opnieuw proberen niet opgeeft, de actie maakt gebruik van het standaardbeleid, dat is eigenlijk een [exponentieel interval beleid](#exponential-interval) maximaal vier nieuwe pogingen exponentieel groeiende intervallen die worden geschaald door 7,5 seconden worden verzonden. Het interval wordt beperkt tot tussen 5 en 45 seconden. 

Hoewel niet expliciet is gedefinieerd in de actie of trigger, volgt de werking van het standaardbeleid in een voorbeeld van de HTTP-actie:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "http://myAPIendpoint/api/action",
      "retryPolicy" : {
         "type": "exponential",
         "interval": "PT7S",
         "count": 4,
         "minimumInterval": "PT5S",
         "maximumInterval": "PT1H"
      }
   },
   "runAfter": {}
}
```

### <a name="none"></a>Geen

Als u wilt opgeven dat mislukte aanvragen niet opnieuw de actie of trigger proberen, stelt u de <*beleidstype*> naar `none`.

### <a name="fixed-interval"></a>Vast interval

Als u wilt opgeven dat de actie of trigger het opgegeven interval wacht voordat de volgende aanvraag wordt verzonden, stelt de <*beleidstype*> naar `fixed`.

*Voorbeeld*

Dit beleid voor opnieuw proberen probeert op te halen van het laatste nieuws twee keer na de eerste mislukte aanvragen met een vertraging van 30 seconden tussen elke poging:

```json
"Get_latest_news": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "https://mynews.example.com/latest",
      "retryPolicy": {
         "type": "fixed",
         "interval": "PT30S",
         "count": 2
      }
   }
}
```

<a name="exponential-interval"></a>

### <a name="exponential-interval"></a>Exponentieel interval

Als u wilt opgeven dat de actie of trigger een random interval wacht voordat de volgende aanvraag wordt verzonden, stelt de <*beleidstype*> naar `exponential`. Het interval voor willekeurige is geselecteerd in een exponentieel toenemende bereik. U kunt eventueel ook de standaard minimale en maximale intervallen onderdrukken door uw eigen minimale en maximale intervallen op te geven.

**Willekeurige variabele bereiken**

Deze tabel ziet u hoe een uniform willekeurige-variabele in Logic Apps wordt gegenereerd in het opgegeven bereik voor elke nieuwe poging tot en met het aantal nieuwe pogingen:

| Aantal opnieuw proberen | Minimaal interval | Maximaal interval |
|--------------|------------------|------------------|
| 1 | max(0, <*minimum-interval*>) | min(interval, <*maximum-interval*>) |
| 2 | max(interval, <*minimum-interval*>) | min (2 * interval, <*maximaal interval*>) |
| 3 | max(2 * interval, <*minimum-interval*>) | min (4 * interval, <*maximaal interval*>) |
| 4 | max(4 * interval, <*minimum-interval*>) | min (8 * interval, <*maximaal interval*>) |
| .... | .... | .... | 
|||| 

## <a name="catch-and-handle-failures-with-the-runafter-property"></a>Variabel en afhandelen van fouten met de eigenschap RunAfter

Elke actie voor logische app worden de acties die moeten worden voltooid voordat deze actie wordt gestart, vergelijkbaar met hoe u de volgorde van stappen in uw werkstroom opgeven gedeclareerd. In het actiedefinitie van een, de **runAfter** eigenschap definieert deze volgorde en is een object dat wordt beschreven welke acties en de status van de actie uitvoeren van de actie.

Standaard alle acties die u in de Logic App Designer toevoegt zijn ingesteld om uit te voeren na de vorige stap, wanneer het resultaat van de vorige stap is **geslaagd**. U kunt echter de **runAfter** waarde zodat acties worden geactiveerd wanneer de vorige acties als gevolg **mislukt**, **overgeslagen**, of een combinatie van deze waarden. Bijvoorbeeld, een item toevoegen aan een specifieke Service Bus-onderwerp na een bepaalde **Insert_Row** actie mislukt, kunt u dit voorbeeld **runAfter** definitie:

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

De **runAfter** eigenschap is ingesteld om uit te voeren wanneer de **Insert_Row** actiestatus is **mislukt**. Voor het uitvoeren van de actie als de actiestatus van de is **geslaagd**, **mislukt**, of **overgeslagen**, gebruik de volgende syntaxis:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Acties die worden uitgevoerd en voltooid na een voorgaande actie is mislukt, zijn gemarkeerd als **geslaagd**. Dit gedrag betekent dat als u met succes catch alle fouten in een werkstroom, de uitvoering zelf is gemarkeerd als **geslaagd**.

<a name="scopes"></a>

## <a name="evaluate-actions-with-scopes-and-their-results"></a>Acties met bereiken en de bijbehorende resultaten evalueren

Vergelijkbaar met stappen die na de afzonderlijke acties met de **runAfter** eigenschap, kunt u groeperen acties binnen een [bereik](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md). U kunt bereiken wanneer u wilt logisch acties te groeperen, cumulatieve status van de scope beoordelen, en acties uitvoeren op basis van deze status. Nadat alle acties in een bereik uitgevoerd hebt, wordt het bereik zelf een eigen status. 

Om te controleren of de status van een scope, kunt u dezelfde criteria die u gebruikt om te controleren op de uitvoeringsstatus van een logische app, zoals **geslaagd**, **mislukt**, enzovoort. 

Standaard als van de scope acties mislukt, de status van de scope gemarkeerd **geslaagd**. Als de laatste actie in een bereik als resulteert **mislukt** of **Aborted**, is gemarkeerd als de status van de scope **mislukt**. 

Om af te vangen uitzonderingen in een **mislukt** bereik en uitvoeren acties die van deze fouten worden verwerkt, kunt u de **runAfter** eigenschap voor die **mislukt** bereik. Op die manier kunnen als *eventuele* acties binnen het bereik mislukken en u gebruikt de **runAfter** eigenschap voor deze scope, kunt u één actie om storingen te ondervangen.

Zie voor de limieten voor scopes [limieten en configuratie](../logic-apps/logic-apps-limits-and-config.md).

### <a name="get-context-and-results-for-failures"></a>Ophalen van context en de resultaten voor fouten

Hoewel het afvangen van fouten van een scope nuttig is, kunt u ook context, zodat u inzicht in de precies welke acties mislukt plus eventuele fouten of statuscodes die zijn geretourneerd. De `@result()` expressie biedt context over het resultaat van alle acties in een bereik.

De `@result()` expressie accepteert één parameter (naam van de scope) en retourneert een matrix met alle actie resultaten uit binnen dat bereik. Deze actie-objecten bevatten dezelfde kenmerken als de  **\@actions()** object, zoals van de actie begintijd, eindtijd, status, invoer, correlatie-id's en uitvoer. Voor het verzenden van context voor alle acties die niet binnen een bereik, u gemakkelijk kunt combineren een  **\@result()** werken met een **runAfter** eigenschap.

Voor het uitvoeren van een actie voor elke actie in een bereik dat is een **mislukt** resultaat, en om te filteren op de matrix van de resultaten naar de mislukte acties, kan u worden gekoppeld  **\@result()** met een **[ Matrix filteren](../connectors/connectors-native-query.md)** actie en een [ **voor elk** ](../logic-apps/logic-apps-control-flow-loops.md) lus. U kunt de gefilterde resultaat matrix en een actie uitvoeren voor elke fout met behulp van de **voor elk** lus. 

Hier volgt een voorbeeld, gevolgd door een gedetailleerde uitleg, waarmee een HTTP POST-aanvraag met de antwoordtekst voor alle acties die niet binnen het bereik 'My_Scope' worden verzonden:

```json
"Filter_array": {
   "type": "Query",
   "inputs": {
      "from": "@result('My_Scope')",
      "where": "@equals(item()['status'], 'Failed')"
   },
   "runAfter": {
      "My_Scope": [
         "Failed"
      ]
    }
},
"For_each": {
   "type": "foreach",
   "actions": {
      "Log_exception": {
         "type": "Http",
         "inputs": {
            "method": "POST",
            "body": "@item()['outputs']['body']",
            "headers": {
               "x-failed-action-name": "@item()['name']",
               "x-failed-tracking-id": "@item()['clientTrackingId']"
            },
            "uri": "http://requestb.in/"
         },
         "runAfter": {}
      }
   },
   "foreach": "@body('Filter_array')",
   "runAfter": {
      "Filter_array": [
         "Succeeded"
      ]
   }
}
```

Hier volgt een gedetailleerd overzicht waarin wordt beschreven wat er gebeurt in dit voorbeeld:

1. Het resultaat ophalen van alle acties in 'My_Scope', de **matrix filteren** actie maakt gebruik van deze filterexpressie: `@result('My_Scope')`

2. De voorwaarde voor **matrix filteren** is een `@result()` item met de status die gelijk is aan **mislukt**. Deze voorwaarde filtert de matrix waarvoor alle actie resultaten uit "My_Scope" omlaag naar een matrix met alleen de resultaten van de actie is mislukt.

3. Uitvoeren van een **voor elk** lus actie op de *gefilterde matrix* levert. Deze stap wordt een actie uitgevoerd voor elke mislukte actie resultaat dat eerder is gefilterd.

   Als één actie in het bereik is mislukt, de acties in de **voor elk** lus slechts eenmaal worden uitgevoerd. 
   Meerdere mislukte acties zorgen ervoor dat één actie per mislukt.

4. Verzenden van een HTTP POST op de **voor elk** item antwoordtekst, dit is de `@item()['outputs']['body']` expressie. 

   De `@result()` item vorm is hetzelfde als de `@actions()` vorm en kan op dezelfde manier worden geparseerd.

5. Twee aangepaste kopteksten met de actienaam van de mislukte bevatten (`@item()['name']`) en de tracerings-ID-client wordt uitgevoerd (`@item()['clientTrackingId']`).

Ter referentie, Hier volgt een voorbeeld van een enkel `@result()` artikel, waarin de **naam**, **hoofdtekst**, en **clientTrackingId** eigenschappen die worden geparseerd in de vorige voorbeeld. Buiten een **voor elk** actie `@result()` retourneert een matrix van deze objecten.

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

U kunt om uit te voeren verschillende patronen van afhandelingsservice voor uitzondering, de expressies die eerder in dit artikel wordt beschreven. U mogelijk kiezen voor het uitvoeren van een enkele uitzonderingsverwerking actie buiten het bereik dat de volledige gefilterde matrix van fouten accepteert en verwijdert de **voor elk** actie. U kunt ook andere nuttige eigenschappen van de  **\@result()** zoals eerder beschreven.

## <a name="azure-diagnostics-and-metrics"></a>Azure Diagnostics en metrische gegevens

De vorige patronen fantastische manier om het afhandelen van fouten en uitzonderingen in een uitvoering zijn, maar u kunt ook identificeren en reageren op fouten die onafhankelijk van de uitvoering zelf. 
[Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) biedt een eenvoudige manier voor het verzenden van alle werkstroomgebeurtenissen, met inbegrip van alle uitvoeren en de actie-statussen, een Azure Storage-account of een event hub gemaakt met Azure Event Hubs. 

Om te evalueren uitvoeren statussen, kunt u de logboeken en metrische gegevens controleren of ze in een controle hulpmiddel dat u wilt publiceren. Een mogelijke optie is om te streamen van alle gebeurtenissen tot en met Event Hubs in [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). In Stream Analytics kunt kunt u live-query's op basis van afwijkingen, gemiddelden, of mislukken van de diagnostische logboeken schrijven. Stream Analytics kunt u gegevens te verzenden naar andere gegevensbronnen, zoals wachtrijen, onderwerpen, SQL, Azure Cosmos DB of Power BI.

## <a name="next-steps"></a>Volgende stappen

* [Zie hoe een klant foutafhandeling met Azure Logic Apps bouwt](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Meer voorbeelden van Logic Apps en scenario's zoeken](../logic-apps/logic-apps-examples-and-scenarios.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
