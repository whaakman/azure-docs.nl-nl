---
title: Stroom Azure logboeken met diagnostische gegevens naar een event hub
description: Informatie over het streamen van Azure logboeken met diagnostische gegevens naar een event hub.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: johnkem
ms.component: ''
ms.openlocfilehash: c59b9982f5ba5a4fa52ab36df5ebb6995b2d45b0
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37085086"
---
# <a name="stream-azure-diagnostic-logs-to-an-event-hub"></a>Stroom Azure logboeken met diagnostische gegevens naar een event hub
**[Azure diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md)**  kan worden gestreamd in bijna realtime voor elke toepassing met behulp van de ingebouwde optie voor 'Exporteren naar Event Hubs' in de Portal of doordat de Event Hub autorisatie regel-ID in een diagnostische instelling via de Azure PowerShell-Cmdlets of Azure CLI 2.0.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Wat u kunt doen met Logboeken met diagnostische en Event Hubs
Hier volgen slechts enkele manieren kunt u de mogelijkheid streaming voor logboeken met diagnostische gegevens:

* **Logboeken met 3e systemen van derden logboekregistratie en telemetrie stream** – al uw diagnostische logboeken naar een enkele event hub voor pipe-logboekgegevens naar een derde partij SIEM of log analytics-hulpprogramma kan worden gestreamd.
* **Servicestatus weergeven door het streamen van gegevens naar PowerBI 'hot pad'** – met behulp van Event Hubs, Stream Analytics en Power BI, kunt u eenvoudig uw diagnostics-gegevens in transformeren naar near-realtime-inzichten aan uw Azure-services. [Deze documentatie-artikel biedt een goed overzicht van het instellen van Event Hubs, verwerken van gegevens met Stream Analytics en het gebruik van Power BI als uitvoer](../stream-analytics/stream-analytics-power-bi-dashboard.md). Hier volgen enkele tips voor het ophalen van instellen met Logboeken met diagnostische gegevens:

  * Een event hub voor een categorie van diagnostische logboeken wordt automatisch gemaakt wanneer u de optie te in de portal selecteren of via PowerShell, inschakelen zodat u de event hub selecteren in de naamruimte met de naam die begint wilt met **insights -**.
  * De volgende SQL-code is een Stream Analytics voorbeeldquery waarmee u kunt alle logboekgegevens in naar een tabel met Power BI parseren:

    ```sql
    SELECT
    records.ArrayValue.[Properties you want to track]
    INTO
    [OutputSourceName – the PowerBI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

* **Maken van een aangepaste Telemetrie en logboekregistratie platform** : als u al hebt een op maat gemaakte telemetrie platform of de zijn alleen nadenkt over het bouwen van een uiterst schaalbare voor publiceren / abonneren aard van Event Hubs kunt u logboeken met diagnostische gegevens flexibel opnemen. [Zie de handleiding voor het gebruik van Event Hubs in een wereldwijde schaal telemetrie platform hier Dan Rosanova van](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Streaming van logboeken met diagnostische gegevens inschakelen

U kunt inschakelen, streamen van diagnostische logboeken programmatisch via de portal of met behulp van de [Monitor REST-API's van Azure](https://docs.microsoft.com/en-us/rest/api/monitor/diagnosticsettings). In beide gevallen moet u een diagnostische instelling maken in die u opgeeft een Event Hubs-naamruimte en de logboek-categorieën en metrische gegevens die u verzenden wilt aan de naamruimte. Een event hub wordt gemaakt in de naamruimte voor elke categorie logboek is ingeschakeld. Een diagnose **logboek categorie** is een type-log dat een resource kan verzamelen.

> [!WARNING]
> Inschakelen en streaming van diagnostische logboeken van rekenresources (bijvoorbeeld virtuele machines of Service Fabric) [vereist een andere set stappen](../event-hubs/event-hubs-streaming-azure-diags-data.md).

De naamruimte van Event Hubs heeft geen zich in hetzelfde abonnement als de bron logboeken verzenden zolang de gebruiker die de instelling configureert juiste RBAC toegang tot beide abonnementen heeft.

> [!NOTE]
> Het verzenden van multidimensionale metrische gegevens via diagnostische instellingen wordt momenteel niet ondersteund. Metrische gegevens met dimensies worden geëxporteerd als platte eendimensionale metrische gegevens, als totaal van alle dimensiewaarden.
>
> *Een voorbeeld*: de metriek 'Binnenkomende berichten' voor een Event Hub kan worden verkend en uitgezet op wachtrijniveau. Maar wanneer de metriek wordt geëxporteerd via diagnostische instellingen, geeft de metriek alle binnenkomende berichten in alle wachtrijen in de Event Hub aan.
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>Diagnostische logboeken met behulp van de portal

1. In de portal, gaat u naar Azure Monitor en klikt u op **diagnostische instellingen**

    ![Sectie van de Monitor Azure bewaking](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-blade.png)

2. Optioneel de lijst filteren op resourcegroep of brontype en klik vervolgens op de bron die u wilt geen diagnostische instellen.

3. Als er geen instellingen bestaan op de bron voor hebt u geselecteerd, u wordt gevraagd om een instelling te maken. Klik op "Diagnostische gegevens inschakelen."

   ![Diagnostische instelling - geen bestaande instellingen toevoegen](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-none.png)

   Als er bestaande instellingen op de bron, ziet u een lijst met instellingen die al zijn geconfigureerd op deze resource. Klik op 'Diagnostische instelling toevoegen'.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-multiple.png)

3. Geef een naam van de instelling en schakel het selectievakje voor **Stream naar een event hub**, selecteer vervolgens een Event Hubs-naamruimte.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-configure.png)

   De naamruimte die is geselecteerd, zijn waar de event hub is gemaakt (als dit de eerste keer streaming diagnoselogboeken is) of gestreamd naar (als er nog resources die zijn die categorie logboek naar deze naamruimte streaming), en het beleid bepaalt de machtigingen die de Streaming-mechanisme heeft. Vandaag de dag vereist streaming naar een event hub machtigingen beheren, verzenden en luisteren. U kunt maken of wijzigen van Event Hubs naamruimte gedeeld toegangsbeleid in de portal onder het tabblad configureren voor de naamruimte. Voor het bijwerken van een van deze diagnostische instellingen, moet de client de machtiging ListKey hebben op de autorisatieregel Event Hubs. U kunt eventueel ook de naam van een event hub opgeven. Als u de naam van een event hub opgeeft, worden de logboeken worden gerouteerd naar die event hub, in plaats van naar een nieuwe event hub per categorie logboek.

4. Klik op **Opslaan**.

De nieuwe instelling wordt weergegeven in de lijst met instellingen voor deze bron na enkele ogenblikken en logboeken met diagnostische gegevens worden gestreamd naar die event hub zodra er nieuwe gebeurtenisgegevens wordt gegenereerd.

### <a name="via-powershell-cmdlets"></a>Via PowerShell-Cmdlets

Om in te schakelen streaming via de [Azure PowerShell-Cmdlets](insights-powershell-samples.md), kunt u de `Set-AzureRmDiagnosticSetting` cmdlet met de volgende parameters:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -EventHubAuthorizationRuleId [your Event Hub namespace auth rule ID] -Enabled $true
```

De Event Hub autorisatie regel-ID is een tekenreeks met deze indeling: `{Event Hub namespace resource ID}/authorizationrules/{key name}`, bijvoorbeeld `/subscriptions/{subscription ID}/resourceGroups/{resource group}/providers/Microsoft.EventHub/namespaces/{Event Hub namespace}/authorizationrules/RootManageSharedAccessKey`. U selecteren de naam van een specifieke event hub met PowerShell op dit moment niet.

### <a name="via-azure-cli-20"></a>Via Azure CLI 2.0

Om in te schakelen streaming via de [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/monitor?view=azure-cli-latest), kunt u de [az monitor diagnose-instellingen maken](https://docs.microsoft.com/en-us/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az-monitor-diagnostic-settings-create) opdracht.

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --event-hub <event hub name> \
    --event-hub-rule <event hub rule ID> \
    --resource <target resource object ID> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

U kunt extra categorieën toevoegen aan de diagnostische logboeken door woordenlijsten toe te voegen aan de JSON-matrix doorgegeven als de `--logs` parameter.

De `--event-hub-rule` argument gebruikt dezelfde notatie als de Event Hub autorisatie regel-ID, zoals wordt beschreven voor de PowerShell-Cmdlet.

## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Hoe ik de logboekgegevens van Event Hubs verbruiken?

Hier volgt een voorbeeld uitvoergegevens van Event Hubs:

```json
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| Elementnaam | Beschrijving |
| --- | --- |
| records |Een matrix met alle gebeurtenissen in deze nettolading. |
| tijd |Tijd waarop de gebeurtenis heeft plaatsgevonden. |
| category |De categorie van het logboek voor deze gebeurtenis. |
| resourceId |Bron-ID van de resource die deze gebeurtenis wordt gegenereerd. |
| operationName |Naam van de bewerking. |
| niveau |Optioneel. Geeft het niveau van de event log. |
| properties |Eigenschappen van de gebeurtenis. |

U kunt een lijst weergeven met alle resourceproviders die ondersteuning bieden voor streaming naar Event Hubs [hier](monitoring-overview-of-diagnostic-logs.md).

## <a name="stream-data-from-compute-resources"></a>Stroomgegevens van rekenresources

U kunt ook diagnostische logboeken van rekenresources met behulp van de agent voor Windows Azure Diagnostics streamen. [Raadpleeg dit artikel](../event-hubs/event-hubs-streaming-azure-diags-data.md) voor hoe die u instelt.

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Azure diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md)
* [Aan de slag met Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
