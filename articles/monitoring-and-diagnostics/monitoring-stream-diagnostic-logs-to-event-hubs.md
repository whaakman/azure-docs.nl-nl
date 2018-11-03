---
title: Stream Azure diagnostische logboeken naar een event hub
description: Meer informatie over het streamen van diagnostische logboeken naar een event hub in Azure.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: johnkem
ms.component: ''
ms.openlocfilehash: d178041a420c49480c6043869f139eb3a09c91fd
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50959270"
---
# <a name="stream-azure-diagnostic-logs-to-an-event-hub"></a>Stream Azure diagnostische logboeken naar een event hub
**[Diagnostische logboeken in Azure](monitoring-overview-of-diagnostic-logs.md)**  kan worden gestreamd in bijna realtime voor elke toepassing met behulp van de ingebouwde 'Exporteren naar Event Hubs'-optie in de Portal of door in te schakelen van de Event Hub autorisatie regel-ID in een diagnostische instelling via de Azure PowerShell-Cmdlets of Azure CLI.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>U kunt doen met diagnostische logboeken en Event Hubs
Hier volgen een paar manieren waarop u de streaming-mogelijkheden voor diagnostische logboeken kunt gebruiken:

* **Stream logboeken naar 3e systemen van derden logboekregistratie en telemetrie** – u kunt al uw diagnostische logboeken naar een enkele event hub om pipe logboekgegevens naar een derde partij SIEM of log analytics-hulpprogramma te streamen.
* **Servicestatus weergeven door het streamen van gegevens naar Power BI 'hot path'** – met behulp van Event Hubs, Stream Analytics en Power BI, kunt u eenvoudig uw diagnostische gegevens in transformeren bijna realtime inzichten in uw Azure-services. [Deze documentatieartikel biedt een goed overzicht van het instellen van Event Hubs, gegevens te verwerken met Stream Analytics en Power BI gebruiken als uitvoer](../stream-analytics/stream-analytics-power-bi-dashboard.md). Hier volgen enkele tips voor het ophalen van instellen met Logboeken met diagnostische gegevens:

  * Een event hub voor een categorie van logboeken met diagnostische gegevens wordt automatisch gemaakt wanneer u schakelt u de optie in de portal of via PowerShell, inschakelen, zodat u de event hub selecteren in de naamruimte met de naam die begint wilt met **insights -**.
  * De volgende SQL-code is een voorbeeld van Stream Analytics-query die u gebruiken kunt voor het parseren van alle logboekgegevens in een Power BI-tabel:

    ```sql
    SELECT
    records.ArrayValue.[Properties you want to track]
    INTO
    [OutputSourceName – the Power BI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

* **Een aangepaste Telemetrie-en logboekregistratie platform** : als u al een op maat gemaakte telemetrie platform of net nadenken over het bouwen van een zeer schaalbare voor publiceren / abonneren aard van Event Hubs kunt u logboeken met diagnostische gegevens flexibel opnemen. [Zie de handleiding voor het gebruik van Event Hubs in een wereldwijde schaal telemetrie platform hier Dan Rosanova van](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Streaming van logboeken met diagnostische gegevens inschakelen

U kunt inschakelen via een programma, via de portal van diagnostische logboeken streamen of met behulp van de [Azure Monitor REST API's](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings). In beide gevallen maakt u een diagnostische instelling in die u geeft een Event Hubs-naamruimte en de logboekcategorieën en metrische gegevens die u verzenden wilt in de naamruimte. Een event hub wordt gemaakt in de naamruimte voor elke logboekcategorie die u inschakelt. Een diagnose **logboekcategorie** is een type logboek dat een resource kan verzamelen.

> [!WARNING]
> Inschakelen en logboeken met diagnostische gegevens van de Compute-resources (bijvoorbeeld virtuele machines of Service Fabric) streaming [vereist een andere reeks stappen](azure-diagnostics-streaming-event-hubs.md).

De Event Hubs naamruimte zich in hetzelfde abonnement als de resource dat Logboeken verzendt, zolang de gebruiker die de instelling configureert juiste RBAC toegang tot zowel abonnementen als beide abonnementen heeft heeft geen uitmaken deel van dezelfde AAD-tenant.

> [!NOTE]
> Het verzenden van multidimensionale metrische gegevens via diagnostische instellingen wordt momenteel niet ondersteund. Metrische gegevens met dimensies worden geëxporteerd als platte eendimensionale metrische gegevens, als totaal van alle dimensiewaarden.
>
> *Een voorbeeld*: de meetwaarde 'Binnenkomende berichten' voor een Event Hub kan worden verkend en uitgezet op wachtrijniveau. Wanneer de waarde wordt geëxporteerd via diagnostische instellingen, wordt deze echter voorgesteld als alle binnenkomende berichten voor alle wachtrijen in de Event Hub.
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>Stream diagnostische logboeken met behulp van de portal

1. In de portal, gaat u naar Azure Monitor en klikt u op **diagnostische instellingen**

    ![Sectie van Azure Monitor bewaking](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-blade.png)

2. (Optioneel) de lijst met door de resourcegroep of resourcetype filteren en klik vervolgens op de resource waarvoor u wilt een diagnostische instelling instellen.

3. Als er geen instellingen zijn op de resource hebt u geselecteerd, wordt u gevraagd om een instelling te maken. Klik op "Diagnostische gegevens inschakelen."

   ![Diagnostische instelling - er zijn geen bestaande instellingen toevoegen](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-none.png)

   Als er bestaande instellingen op de resource, ziet u een lijst met instellingen die al zijn geconfigureerd voor deze resource. Klik op 'Diagnostische instelling toevoegen'.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-multiple.png)

3. Geef een naam van uw instelling en schakel het selectievakje voor **Stream naar een event hub**, selecteert u een Event Hubs-naamruimte.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-configure.png)

   De geselecteerde naamruimte is waar de event hub is gemaakt (als dit uw streaming diagnostische logboeken voor eerste keer is) of gestreamd naar (als er al resources zijn die zijn die logboekcategorie naar deze naamruimte streaming), en het beleid bepaalt de machtigingen die de Streaming-mechanisme heeft. Vandaag de dag vereist streamen naar een event hub machtigingen beheren, verzenden en luisteren. U kunt maken of wijzigen van de Event Hubs-naamruimte gedeeld toegangsbeleid in de portal onder het tabblad configureren voor uw naamruimte. Als u wilt een van deze diagnostische instellingen bijwerken, moet de client de ListKey-machtiging hebben voor de Event Hubs-autorisatieregel. U kunt eventueel ook een event hub-naam opgeven. Als u een event hub-naam opgeeft, worden de logboeken worden gerouteerd naar die event hub, in plaats van naar een nieuwe event hub per logboekcategorie.

4. Klik op **Opslaan**.

Na enkele ogenblikken wordt de nieuwe instelling wordt weergegeven in de lijst met instellingen voor deze resource en logboeken met diagnostische gegevens worden gestreamd naar die event hub als de gegevens van een nieuwe gebeurtenis wordt gegenereerd.

### <a name="via-powershell-cmdlets"></a>Via PowerShell-Cmdlets

Om in te schakelen streaming via de [Azure PowerShell-Cmdlets](insights-powershell-samples.md), kunt u de `Set-AzureRmDiagnosticSetting` cmdlet met de volgende parameters:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -EventHubAuthorizationRuleId [your Event Hub namespace auth rule ID] -Enabled $true
```

Regel-ID voor de Event Hub-autorisatie is een tekenreeks zijn met deze indeling: `{Event Hub namespace resource ID}/authorizationrules/{key name}`, bijvoorbeeld `/subscriptions/{subscription ID}/resourceGroups/{resource group}/providers/Microsoft.EventHub/namespaces/{Event Hub namespace}/authorizationrules/RootManageSharedAccessKey`. U selecteren de naam van een specifieke event hub met PowerShell op dit moment niet.

### <a name="via-azure-cli"></a>Via Azure CLI

Om in te schakelen streaming via de [Azure CLI](https://docs.microsoft.com/cli/azure/monitor?view=azure-cli-latest), kunt u de [az monitor diagnostic-settings maken](https://docs.microsoft.com/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az-monitor-diagnostic-settings-create) opdracht.

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

U kunt extra categorieën toevoegen aan de diagnostische logboeken van woordenlijsten toe te voegen aan de JSON-matrix die is doorgegeven als de `--logs` parameter.

De `--event-hub-rule` argument gebruikt dezelfde indeling als de Event Hub autorisatie regel-ID, zoals wordt beschreven voor de PowerShell-Cmdlet.

## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Hoe ik de logboekgegevens van Event Hubs gebruiken?

Hier volgt een voorbeeld van uitvoergegevens uit Event Hubs:

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

| De naam van element | Beschrijving |
| --- | --- |
| records |Een matrix van alle gebeurtenissen in deze nettolading. |
| tijd |Tijd waarop de gebeurtenis heeft plaatsgevonden. |
| category |Logboekcategorie voor deze gebeurtenis. |
| resourceId |Resource-ID van de resource die deze gebeurtenis is gegenereerd. |
| operationName |Naam van de bewerking. |
| niveau |Optioneel. Geeft het niveau van de event log. |
| properties |De eigenschappen van de gebeurtenis. |

U kunt een lijst weergeven met alle resourceproviders die ondersteuning bieden voor streamen naar Event Hubs [hier](monitoring-overview-of-diagnostic-logs.md).

## <a name="stream-data-from-compute-resources"></a>Stream-gegevens van de Compute-resources

U kunt ook diagnostische logboeken van de Compute-resources met behulp van de Windows Azure Diagnostics-agent streamen. [Raadpleeg dit artikel](azure-diagnostics-streaming-event-hubs.md) voor informatie over het instellen daarvan.

## <a name="next-steps"></a>Volgende stappen

* [Logboeken van de Stream Azure Active Directory met Azure Monitor](../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md)
* [Meer informatie over Azure-logboeken met diagnostische gegevens](monitoring-overview-of-diagnostic-logs.md)
* [Aan de slag met Event Hubs](../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)
