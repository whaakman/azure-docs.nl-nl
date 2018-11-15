---
title: Fouten opsporen in UDF's in Azure, digitale dubbels | Microsoft Docs
description: Richtlijn over fouten opsporen in UDF's in Azure, digitale dubbels
author: stefanmsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: stefanmsft
ms.openlocfilehash: ac7664e94c6e02ab90dbb1b32a54c8234614afe2
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636268"
---
# <a name="how-to-debug-issues-with-user-defined-functions-in-azure-digital-twins"></a>Fouten opsporen in problemen met de gebruiker gedefinieerde functies in Azure, digitale dubbels

In dit artikel bevat een overzicht van hoe u de gebruiker gedefinieerde functies vaststelt. Vervolgens bevat deze enkele van de meest voorkomende scenario's is opgetreden bij het werken met hen.

## <a name="debug-issues"></a>Spoor fouten op

Weten hoe u eventuele problemen die in uw exemplaar van Azure digitale dubbels optreden vaststelt, helpt u het probleem, de oorzaak van het probleem en een oplossing effectief te identificeren.

### <a name="enable-log-analytics-for-your-instance"></a>Log analytics voor uw exemplaar inschakelen

Logboeken en metrische gegevens voor uw exemplaar van Azure digitale dubbels worden weergegeven via Azure Monitor. De volgende documentatie wordt ervan uitgegaan dat u hebt gemaakt een [Azure Log Analytics](../log-analytics/log-analytics-queries.md) werkruimte via de [Azure Portal](../log-analytics/log-analytics-quick-create-workspace.md), tot en met [Azure CLI](../log-analytics/log-analytics-quick-create-workspace-cli.md), of via [ PowerShell](../log-analytics/log-analytics-quick-create-workspace-posh.md).

> [!NOTE]
> Een vertraging van 5 minuten kunnen optreden bij het verzenden van gebeurtenissen naar **Log Analytics** voor de eerste keer.

Lees het artikel ['Verzamelen en gebruiken van logboekgegevens van uw Azure-resources'](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) om in te schakelen van diagnostische instellingen voor uw exemplaar van Azure digitale dubbels via de Portal, Azure CLI of PowerShell. Zorg ervoor dat alle logboekcategorieën, metrische gegevens en uw Azure Log Analytics-werkruimte te selecteren.

### <a name="trace-sensor-telemetry"></a>Sensor-tracetelemetrie

Zorg ervoor dat de diagnostische instellingen zijn ingeschakeld op uw exemplaar van Azure digitale Twins, alle logboekcategorieën zijn geselecteerd en dat de logboeken worden verzonden naar Azure Log Analytics.

Om een bericht van de sensor telemetrie naar de respectieve Logboeken, kunt u een correlatie-ID opgeven op de gegevens van de gebeurtenis wordt verzonden. Om dit te doen, stel de `x-ms-client-request-id` eigenschap naar een GUID.

Na het verzenden van telemetrie, opent u de Azure Log Analytics om op te vragen met behulp van de set logboeken correlatie-ID:

```Kusto
AzureDiagnostics
| where CorrelationId = 'YOUR_CORRELATION_IDENTIFIER'
```

| Waarde opvragen | Vervangen door |
| --- | --- |
| YOUR_CORRELATION_IDENTIFIER | De correlatie-ID die is opgegeven in de gebeurtenisgegevens |

Als u zich aanmeldt voor de gebruiker gedefinieerde functie, deze logboeken wordt weergegeven in uw Azure-logboekanalyse-exemplaar met de categorie `UserDefinedFunction`. Om op te halen ze, voer de volgende queryvoorwaarde in Azure Log Analytics:

```Kusto
AzureDiagnostics
| where Category = 'UserDefinedFunction'
```

Zie voor meer informatie over krachtige querybewerkingen [aan de slag met query's](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries).

## <a name="identify-common-issues"></a>Veelvoorkomende problemen identificeren

Zowel opsporen en identificeren van algemene problemen zijn belangrijk bij het oplossen van uw oplossing. Enkele veelvoorkomende problemen opgetreden bij het ontwikkelen, door de gebruiker gedefinieerde functies worden samengevat hieronder.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

### <a name="ensure-a-role-assignment-was-created"></a>Zorg ervoor dat een roltoewijzing is gemaakt

De gebruiker gedefinieerde functie heeft toegang tot het uitvoeren van acties zoals het verzenden van meldingen, bij het ophalen van metagegevens, niet zonder een roltoewijzing gemaakt binnen de beheer-API, en instelling berekende waarden in de topologie.

Controleer of een roltoewijzing voor de gebruiker gedefinieerde functie door uw API Management:

```plaintext
GET YOUR_MANAGEMENT_API_URL/roleassignments?path=/&traverse=Down&objectId=YOUR_USER_DEFINED_FUNCTION_ID
```

| Parameter | Vervangen door |
| --- | --- |
| *YOUR_USER_DEFINED_FUNCTION_ID* | De ID van de gebruiker gedefinieerde functie om op te halen van roltoewijzingen voor|

Als er geen roltoewijzing wordt opgehaald, volgt u in dit artikel op [over het maken van een roltoewijzing voor de gebruiker gedefinieerde functie](./how-to-user-defined-functions.md).

### <a name="check-if-the-matcher-will-work-for-a-sensors-telemetry"></a>Of als de matcher voor de telemetrie van een sensor werkt

Met de volgende oproep verzenden op basis van uw digitale dubbels Azure-instanties Management-API, kunt u zich om te bepalen of een bepaalde matcher voor de opgegeven sensor geldt.

```plaintext
GET YOUR_MANAGEMENT_API_URL/matchers/YOUR_MATCHER_IDENTIFIER/evaluate/YOUR_SENSOR_IDENTIFIER?enableLogging=true
```

| Parameter | Vervangen door |
| --- | --- |
| *YOUR_MATCHER_IDENTIFIER* | De ID van de matcher die u wilt evalueren |
| *YOUR_SENSOR_IDENTIFIER* | De ID van de sensor die u wilt evalueren |

Reactie:

```JavaScript
{
    "success": true,
    "logs": [
        "$.dataType: \"Motion\" Equals \"Motion\" => True"
    ]
}
```

### <a name="check-what-a-sensor-will-trigger"></a>Controleer wat een sensor wordt geactiveerd

Met de volgende oproep verzenden op basis van uw digitale dubbels Azure-instanties Management-API, kunt u zich aan de id's van de gebruiker gedefinieerde functies die worden geactiveerd door de opgegeven sensor binnenkomende telemetriegegevens te bepalen:

```plaintext
GET YOUR_MANAGEMENT_API_URL/sensors/YOUR_SENSOR_IDENTIFIER/matchers?includes=UserDefinedFunctions
```

| Parameter | Vervangen door |
| --- | --- |
| *YOUR_SENSOR_IDENTIFIER* | De ID van de sensor die van telemetrie verzenden |

Reactie:

```JavaScript
[
    {
        "id": "48a64768-797e-4832-86dd-de625f5f3fd9",
        "name": "MotionMatcher",
        "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
        "conditions": [
            {
                "id": "024a067a-414f-415b-8424-7df61392541e",
                "target": "Sensor",
                "path": "$.dataType",
                "value": "\"Motion\"",
                "comparison": "Equals"
            }
        ],
        "userDefinedFunctions": [
            {
                "id": "373d03c5-d567-4e24-a7dc-f993460120fc",
                "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
                "name": "Motion User-Defined Function",
                "disabled": false
            }
        ]
    }
]
```

### <a name="issue-with-receiving-notifications"></a>Probleem met het ontvangen van meldingen

Als niet ontvangen meldingen van binnen de gebruiker gedefinieerde geactiveerde functie, zorg ervoor dat is uw topologie object typeparameter komt overeen met het type van de id die wordt gebruikt.

**Onjuiste** voorbeeld:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will not work'
};

sendNotification(telemetry.SensorId, "Space", JSON.stringify(customNotification));
```

In dit scenario optreedt, omdat de gebruikte id naar een sensor verwijst terwijl het opgegeven type topologie-object is 'Ruimte'.

**Juiste** voorbeeld:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will work'
};

sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(customNotification));
```

De eenvoudigste manier om niet ondervindt dit probleem is met de `Notify` methode voor het metagegevensobject.

Voorbeeld:

```JavaScript
function process(telemetry, executionContext) {
    var sensorMetadata = getSensorMetadata(telemetry.SensorId);

    var customNotification = {
        Message: 'Custom notification'
    };

    // Short-hand for above methods where object type is known from metadata.
    sensorMetadata.Notify(JSON.stringify(customNotification));
}
```

## <a name="common-diagnostic-exceptions"></a>Algemene diagnostische uitzonderingen

Als u diagnostische instellingen inschakelen, kunt u deze algemene uitzonderingen optreden:

1. **Beperking**: als de gebruiker gedefinieerde functie groter is dan de uitvoeringssnelheid van limieten die worden beschreven in de [Servicelimieten](./concepts-service-limits.md) artikel, worden beperkt. Beperking houdt in dat er geen verdere bewerkingen worden uitgevoerd met succes totdat de limieten zijn verlopen.

1. **Gegevens niet gevonden**: als de gebruiker gedefinieerde functie probeert te krijgen tot metagegevens die niet bestaat, zal de bewerking mislukken.

1. **Niet gemachtigd**: als de gebruiker gedefinieerde functie beschikt niet over een roltoewijzing instellen of beschikt niet over voldoende machtigingen voor toegang tot bepaalde metagegevens van de topologie, zal de bewerking mislukken.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het inschakelen [bewaking en logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) in digitale dubbels van Azure.
