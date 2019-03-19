---
title: Bewaken van data factory's met behulp van Azure Monitor | Microsoft Docs
description: Informatie over het gebruik van Azure Monitor voor het bewaken van Data Factory-pijplijnen door het inschakelen van diagnostische logboeken met gegevens uit Azure Data Factory.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: shlo
ms.openlocfilehash: e96e462709ab0c715c831bd10c628869d5c617fe
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58013310"
---
# <a name="alert-and-monitor-data-factories-using-azure-monitor"></a>Waarschuwen en bewaken van data factory's met behulp van Azure Monitor
Cloud-Apps zijn complexe met veel bewegende onderdelen bevatten. Monitoring biedt gegevens om ervoor te zorgen dat uw toepassing actief en wordt uitgevoerd in een foutloze toestand bevindt. Ook kunt u potentiële problemen voorkomen of oplossen van het verleden zijn. Bovendien kunt u bewakingsgegevens diep om inzicht te krijgen over uw toepassing. Deze kennis kan u helpen te verbeteren van de prestaties van de toepassing of onderhoud, of Automatiseer acties die anders handmatig worden opgelost moeten zouden.

Azure Monitor biedt op basisniveau infrastructuur metrische gegevens en logboeken voor de meeste services in Microsoft Azure. Zie voor meer informatie, [bewakingsoverzicht](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor). Diagnostische logboeken in Azure zijn Logboeken door een resource met uitgebreide, regelmatig gegevens over de werking van die resource. Data Factory voert diagnostische logboeken in Azure Monitor.

## <a name="persist-data-factory-data"></a>Data Factory gegevens behouden
Data Factory worden alleen gegevens voor pijplijnuitvoering voor 45 dagen opgeslagen. Als u wilt om vast te leggen van de pijplijn gegevens langer dan 45 dagen worden uitgevoerd, met behulp van Azure Monitor, u kunt niet alleen routeren logboeken met diagnostische gegevens voor analyse, u kunt ze ook behouden in een storage-account, zodat u beschikt over factory informatie voor de duur van uw keuze.

## <a name="diagnostic-logs"></a>Diagnostische logboeken

* Opslaan naar een **Opslagaccount** voor controle of handmatige controle. U kunt de bewaartijd (in dagen) met behulp van de diagnostische instellingen opgeven.
* Stream ze **Event Hubs** voor opname van een service van derden of aangepaste analyseoplossing zoals Power BI.
* Analyseren met **Log Analytics**

U kunt een storage-account of event hub-naamruimte die zich niet in hetzelfde abonnement als de resource die is dat Logboeken verzendt. De gebruiker die de instelling configureert, moet de juiste rollen gebaseerde toegang (RBAC) toegang tot beide abonnementen hebben.

## <a name="set-up-diagnostic-logs"></a>Diagnostische logboeken instellen

### <a name="diagnostic-settings"></a>Diagnostische instellingen
Diagnostische logboeken voor niet-compute-resources zijn geconfigureerd met behulp van diagnostische instellingen. Diagnostische instellingen voor een resource-besturingselement:

* Waarnaar logboeken met diagnostische gegevens worden verzonden (Storage-Account, Event Hubs of Logboeken van Azure Monitor).
* Welke logboekcategorieën worden verzonden.
* Hoe lang elke categorie logboekbestanden worden bewaard in een storage-account.
* Een bewaarperiode van nul dagen betekent dat Logboeken altijd worden bewaard. De waarde kan anders een willekeurig aantal dagen tussen 1 en 2147483647 zijn.
* Als Logboeken opslaan in een storage-account is uitgeschakeld (bijvoorbeeld alleen Event Hubs of Azure Monitor logboeken opties zijn geselecteerd), bewaarbeleid worden ingesteld, maar hebben het bewaarbeleid geen effect.
* Bewaarbeleid zijn toegepast per dag, dus aan het einde van een dag (UTC), logboeken van de dag dat nu is buiten de bewaarperiode van beleid worden verwijderd. Bijvoorbeeld, als u een beleid voor het bewaren van één dag had, worden aan het begin van de dag vandaag nog de logboeken van de dag voor gisteren vernietigd.

### <a name="enable-diagnostic-logs-via-rest-apis"></a>Logboeken met diagnostische gegevens via REST API's inschakelen

Maken of bijwerken van een diagnostische instelling in Azure Monitor REST API

**Aanvraag**
```
PUT
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

**Headers**
* Vervang `{api-version}` door `2016-09-01`.
* Vervang `{resource-id}` met de resource-ID van de resource waarvoor u wilt bewerken diagnostische instellingen. Voor meer informatie [resourcegroepen voor het beheren van uw Azure-resources met behulp van](../azure-resource-manager/manage-resource-groups-portal.md).
* Stel de `Content-Type` koptekst `application/json`.
* De autorisatie-header ingesteld op een JSON webtoken die u uit Azure Active Directory verkrijgt. Zie voor meer informatie, [verifiëren van aanvragen](../active-directory/develop/authentication-scenarios.md).

**Hoofdtekst**
```json
{
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<LogAnalyticsName>",
        "metrics": [
        ],
        "logs": [
                {
                    "category": "PipelineRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "TriggerRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "ActivityRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                }
            ]
    },
    "location": ""
}
```

| Eigenschap | Type | Description |
| --- | --- | --- |
| storageAccountId |String | De resource-ID van het opslagaccount waarnaar u wilt verzenden van diagnostische logboeken |
| serviceBusRuleId |String | De service bus regel-ID van de service bus-naamruimte waarin u hebben van Event Hubs gemaakt wilt voor het streamen van diagnostische logboeken. De regel-ID van de indeling is: "{service bus-resource-ID} /authorizationrules/ {naam} '.|
| workspaceId | Complex Type | Matrix van metrische tijd rijstkorrels en hun beleid voor het bewaren. Deze eigenschap is momenteel leeg. |
|metrics| Parameterwaarden van de pijplijn uitvoeren om te worden doorgegeven aan de pijplijn aangeroepen| Namen van parameters toe te wijzen aan de waarden van het argument een JSON-object |
| logboeken| Complex Type| De naam van een categorie diagnostische logboeken voor een resourcetype. Als u de lijst met categorieën van diagnostische logboeken voor een resource, moet u eerst een GET-bewerking voor diagnostische instellingen uitvoeren. |
| category| String| Matrix van logboekcategorieën en hun bewaarbeleid |
| timeGrain | String | De granulatie van metrische gegevens die zijn vastgelegd in ISO 8601-notatie voor de duur. Moet PT1M (één minuut)|
| ingeschakeld| Booleaans | Hiermee geeft u op of de verzameling van deze categorie metrische gegevens of logboekbestanden is ingeschakeld voor deze resource|
| retentionPolicy| Complex Type| Beschrijft het bewaarbeleid voor categorie metrische gegevens of logboek. Voor de optie voor het opslagaccount alleen wordt gebruikt.|
| dagen| Int| Het aantal dagen te bewaren van de metrische gegevens en Logboeken. Een waarde van 0 worden de logboeken voor onbepaalde tijd bewaard. Voor de optie voor het opslagaccount alleen wordt gebruikt. |

**Antwoord**

200 OK


```json
{
    "id": "/subscriptions/<subID>/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.OperationalInsights/workspaces/<LogAnalyticsName>",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}
```

Informatie over het instellen van diagnostische gegevens ophalen in Azure Monitor REST API

**Aanvraag**
```
GET
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

**Headers**
* Vervang `{api-version}` door `2016-09-01`.
* Vervang `{resource-id}` met de resource-ID van de resource waarvoor u wilt bewerken diagnostische instellingen. Voor meer informatie met behulp van resourcegroepen voor het beheren van uw Azure-resources.
* Stel de `Content-Type` koptekst `application/json`.
* De autorisatie-header ingesteld op een JSON Web Token die u uit Azure Active Directory verkrijgt. Zie Authenticating aanvragen voor meer informatie.

**Antwoord**

200 OK

```json
{
    "id": "/subscriptions/<subID>/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/shloprivate/providers/Microsoft.Storage/storageAccounts/azmonlogs",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/shloprivate/providers/Microsoft.EventHub/namespaces/shloeventhub/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/ADF/providers/Microsoft.OperationalInsights/workspaces/mihaipie",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}
```
[Hier meer informatie](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)

## <a name="schema-of-logs--events"></a>Schema van Logboeken en gebeurtenissen

### <a name="activity-run-logs-attributes"></a>Activiteit uitvoeren registreert kenmerken

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "activityRunId":"",
   "pipelineRunId":"",
   "resourceId":"",
   "category":"ActivityRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "activityName":"",
   "start":"",
   "end":"",
   "properties":
       {
          "Input": "{
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "BlobSink"
              }
           }",
          "Output": "{"dataRead":121,"dataWritten":121,"copyDuration":5,
               "throughput":0.0236328132,"errors":[]}",
          "Error": "{
              "errorCode": "null",
              "message": "null",
              "failureType": "null",
              "target": "CopyBlobtoBlob"
        }
    }
}
```

| Eigenschap | Type | Description | Voorbeeld |
| --- | --- | --- | --- |
| Niveau |String | Niveau van de diagnostische logboeken. Niveau 4 is altijd het geval is bij Logboeken voor de uitvoering van activiteiten. | `4`  |
| correlationId |String | Unieke ID voor het bijhouden van een bepaalde aanvraag end-to-end | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| time | String | Tijd van de gebeurtenis in timespan UTC-notatie `YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|activityRunId| String| ID van de activiteit die wordt uitgevoerd | `3a171e1f-b36e-4b80-8a54-5625394f4354` |
|pipelineRunId| String| ID van de pijplijnuitvoering | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|resourceId| String | Gekoppelde resource-ID voor de data factory-resource | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| String | De categorie van diagnostische logboeken. Deze eigenschap instellen op "Uitvoering van activiteiten" | `ActivityRuns` |
|niveau| String | Niveau van de diagnostische logboeken. Deze eigenschap instellen op 'Ter informatie' | `Informational` |
|operationName| String |De naam van de activiteit met status. Als de status de heartbeat start is, is het `MyActivity -`. Als de status van de end-heartbeat is, is het `MyActivity - Succeeded` met de definitieve status | `MyActivity - Succeeded` |
|pipelineName| String | Naam van de pijplijn | `MyPipeline` |
|activityName| String | Naam van de activiteit | `MyActivity` |
|start| String | Begin van de activiteit in UTC-notatie timespan uitvoeren | `2017-06-26T20:55:29.5007959Z`|
|einde| String | Einde van de activiteit in UTC-notatie timespan uitvoeren. Als de activiteit is niet beëindigd, maar (diagnostische logboeken voor het starten van een activiteit), een standaardwaarde van `1601-01-01T00:00:00Z` is ingesteld.  | `2017-06-26T20:55:29.5007959Z` |

### <a name="pipeline-run-logs-attributes"></a>Uitvoering van de pijplijn registreert kenmerken

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "runId":"",
   "resourceId":"",
   "category":"PipelineRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "start":"",
   "end":"",
   "status":"",
   "properties":
    {
      "Parameters": {
        "<parameter1Name>": "<parameter1Value>"
      },
      "SystemParameters": {
        "ExecutionStart": "",
        "TriggerId": "",
        "SubscriptionId": ""
      }
    }
}
```

| Eigenschap | Type | Description | Voorbeeld |
| --- | --- | --- | --- |
| Niveau |String | Niveau van de diagnostische logboeken. Niveau 4 is het geval is bij Logboeken voor de uitvoering van activiteiten. | `4`  |
| correlationId |String | Unieke ID voor het bijhouden van een bepaalde aanvraag end-to-end | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| time | String | Tijd van de gebeurtenis in timespan UTC-notatie `YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|runId| String| ID van de pijplijnuitvoering | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|resourceId| String | Gekoppelde resource-ID voor de data factory-resource | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| String | De categorie van diagnostische logboeken. Deze eigenschap instellen op "PipelineRuns" | `PipelineRuns` |
|niveau| String | Niveau van de diagnostische logboeken. Deze eigenschap instellen op 'Ter informatie' | `Informational` |
|operationName| String |Naam van de pijplijn met de status. "Pipeline - geslaagd" met de definitieve status als de pijplijn-run is voltooid| `MyPipeline - Succeeded` |
|pipelineName| String | Naam van de pijplijn | `MyPipeline` |
|start| String | Begin van de activiteit in UTC-notatie timespan uitvoeren | `2017-06-26T20:55:29.5007959Z`|
|einde| String | Einde van de activiteit in UTC-notatie timespan wordt uitgevoerd. Als de activiteit is niet beëindigd, maar (diagnostische logboeken voor het starten van een activiteit), een standaardwaarde van `1601-01-01T00:00:00Z` is ingesteld.  | `2017-06-26T20:55:29.5007959Z` |
|status| String | De definitieve status van de pijplijnuitvoering (geslaagd of mislukt) | `Succeeded`|

### <a name="trigger-run-logs-attributes"></a>Triggeruitvoering registreert kenmerken

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "triggerId":"",
   "resourceId":"",
   "category":"TriggerRuns",
   "level":"Informational",
   "operationName":"",
   "triggerName":"",
   "triggerType":"",
   "triggerEvent":"",
   "start":"",
   "status":"",
   "properties":
   {
      "Parameters": {
        "TriggerTime": "",
       "ScheduleTime": ""
      },
      "SystemParameters": {}
    }
}

```

| Eigenschap | Type | Description | Voorbeeld |
| --- | --- | --- | --- |
| Niveau |String | Niveau van de diagnostische logboeken. Ingesteld op niveau 4 voor logboeken voor de uitvoering van activiteiten. | `4`  |
| correlationId |String | Unieke ID voor het bijhouden van een bepaalde aanvraag end-to-end | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| time | String | Tijd van de gebeurtenis in timespan UTC-notatie `YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|triggerId| String| ID van de trigger uitvoeren | `08587023010602533858661257311` |
|resourceId| String | Gekoppelde resource-ID voor de data factory-resource | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| String | De categorie van diagnostische logboeken. Deze eigenschap instellen op "PipelineRuns" | `PipelineRuns` |
|niveau| String | Niveau van de diagnostische logboeken. Deze eigenschap instellen op 'Ter informatie' | `Informational` |
|operationName| String |Naam van de trigger met de definitieve status of deze is geactiveerd. 'MyTrigger - geslaagd' als de heartbeat geslaagd is| `MyTrigger - Succeeded` |
|triggerName| String | Naam van de trigger | `MyTrigger` |
|triggerType| String | Type van de trigger (handmatige trigger of Schematrigger) | `ScheduleTrigger` |
|triggerEvent| String | Gebeurtenis van de trigger | `ScheduleTime - 2017-07-06T01:50:25Z` |
|start| String | Begin van de trigger worden gestart in timespan UTC-notatie | `2017-06-26T20:55:29.5007959Z`|
|status| String | De definitieve status van of trigger is geactiveerd (geslaagd of mislukt) | `Succeeded`|

## <a name="metrics"></a>Metrische gegevens

Azure Monitor kunt u telemetrie voor inzicht in de prestaties en status van uw werklasten op Azure gebruiken. De belangrijkste type Azure telemetrische gegevens is de metrische gegevens die (ook wel prestatiemeteritems) gegenereerd door de meeste Azure-resources. Azure Monitor biedt verschillende manieren om te configureren en gebruiken van deze metrische gegevens voor controle en probleemoplossing.

ADFV2 verzendt de volgende metrische gegevens

| **Gegevens**           | **De naam van de metrische gegevens weergeven**         | **Eenheid** | **Type samenvoeging geselecteerd** | **Beschrijving**                                       |
|----------------------|---------------------------------|----------|----------------------|-------------------------------------------------------|
| PipelineSucceededRun | Pijplijn-runs metrische gegevens is voltooid | Count    | Totaal                | Totaal aantal pijplijnen werkstroomuitvoeringen binnen een minuut |
| PipelineFailedRuns   | Pijplijn-runs metrische gegevens is mislukt    | Count    | Totaal                | Totale pijplijnen wordt uitgevoerd binnen een minuut    |
| ActivitySucceededRuns | Metrische gegevens de uitvoeringen van activiteiten is voltooid | Count    | Totaal                | Totaal aantal uitvoeringen van activiteit is voltooid binnen een minuut  |
| ActivityFailedRuns   | Kan geen metrische gegevens van activiteiten wordt uitgevoerd    | Count    | Totaal                | Totaal aantal uitvoeringen van activiteit is mislukt binnen een minuut     |
| TriggerSucceededRuns | Trigger wordt uitgevoerd metrische gegevens is voltooid  | Count    | Totaal                | Totaal aantal trigger werkstroomuitvoeringen binnen een minuut   |
| TriggerFailedRuns    | Trigger wordt uitgevoerd metrische gegevens is mislukt     | Count    | Totaal                | Totaal aantal trigger wordt uitgevoerd binnen een minuut      |

Voor toegang tot de metrische gegevens, volg de instructies in het artikel: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics

## <a name="monitor-data-factory-metrics-with-azure-monitor"></a>Monitor voor Gegevensfactory-metrieken met Azure Monitor

U kunt Azure Data Factory-integratie met Azure Monitor gebruiken het routeren van gegevens naar Azure Monitor. Deze integratie is handig in de volgende scenario's:

1.  Wilt u het schrijven van complexe query's op een uitgebreide set metrische gegevens die door Data Factory naar Azure Monitor is gepubliceerd. U kunt ook aangepaste waarschuwingen maken op deze query's via Azure Monitor.

2.  U wilt controleren in data factory's. U kunt gegevens uit meerdere data factory's doorsturen naar één Azure Monitor-werkruimte.

Bekijk de volgende video voor een inleiding van 7 minuten en demonstratie van deze functie:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Monitor-Data-Factory-pipelines-using-Operations-Management-Suite-OMS/player]

### <a name="configure-diagnostic-settings-and-workspace"></a>Diagnostische instellingen en werkruimte configureren

Diagnostische instellingen inschakelen voor uw data factory.

1.  Selecteer **Azure Monitor** -> **diagnostische instellingen** -> Selecteer de data factory -> Schakel diagnostische gegevens.

    ![monitor-oms-image1.png](media/data-factory-monitor-oms/monitor-oms-image1.png)

2.  Diagnostische instellingen zoals de configuratie van de werkruimte kan leveren.

    ![monitor-oms-image2.png](media/data-factory-monitor-oms/monitor-oms-image2.png)

### <a name="install-azure-data-factory-analytics-from-azure-marketplace"></a>Azure Data Factory Analytics installeren via Azure Marketplace

![monitor-oms-image3.png](media/data-factory-monitor-oms/monitor-oms-image3.png)

![monitor-oms-image4.png](media/data-factory-monitor-oms/monitor-oms-image4.png)

Klik op **maken** en selecteer de werkruimte en de werkruimte instellingen.

![monitor-oms-image5.png](media/data-factory-monitor-oms/monitor-oms-image5.png)

### <a name="monitor-data-factory-metrics"></a>Monitor voor Gegevensfactory-metrieken

Installeren van **Azure Data Factory Analytics** maakt u een standaardset van weergaven waarmee de volgende metrische gegevens:

- Pijplijnuitvoeringen ADF wordt-1) door Data Factory

- Uitvoeringen van activiteit ADF uitvoeringen-2) door Data Factory

- Triggeruitvoeringen ADF uitvoeringen-3) door Data Factory

- ADF-fouten-1) Top 10 pijplijn fouten door Data Factory

- ADF-fouten-2) Top 10 uitvoeringen van activiteit door Data Factory

- ADF-fouten-3) Top 10 Trigger fouten door Data Factory

- Uitvoeringen van activiteit ADF statistieken-1) per Type

- Triggeruitvoeringen ADF statistieken-2) per Type

- ADF-statistieken-3) Pijplijnuitvoeringen Max duur

![monitor-oms-image6.png](media/data-factory-monitor-oms/monitor-oms-image6.png)

![monitor-oms-image7.png](media/data-factory-monitor-oms/monitor-oms-image7.png)

U kunt de bovenstaande metrische gegevens visualiseren, kijken naar de query's achter deze metrische gegevens, de query's bewerken, maken van waarschuwingen, enzovoort.

![monitor-oms-image8.png](media/data-factory-monitor-oms/monitor-oms-image8.png)

## <a name="alerts"></a>Waarschuwingen

Meld u aan bij de Azure-portal en klikt u op **Monitor -&gt; waarschuwingen** om waarschuwingen te maken.

![Waarschuwingen in het menu van de portal](media/monitor-using-azure-monitor/alerts_image3.png)

### <a name="create-alerts"></a>Waarschuwingen maken

1.  Klik op **+ nieuwe waarschuwingsregel** om een nieuwe waarschuwing te maken.

    ![Nieuwe waarschuwingsregel](media/monitor-using-azure-monitor/alerts_image4.png)

2.  Definieer de **waarschuwen voorwaarde**.

    > [!NOTE]
    > Zorg ervoor dat u selecteert **alle** in de **filteren op resourcetype**.

    ![Ontvang een waarschuwing voorwaarde, scherm 1 van 3](media/monitor-using-azure-monitor/alerts_image5.png)

    ![Ontvang een waarschuwing voorwaarde, scherm 2 van 3](media/monitor-using-azure-monitor/alerts_image6.png)

    ![Ontvang een waarschuwing voorwaarde, 3 van 3 scherm](media/monitor-using-azure-monitor/alerts_image7.png)

3.  Definieer de **Waarschuwingsdetails**.

    ![Meldingsdetails](media/monitor-using-azure-monitor/alerts_image8.png)

4.  Definieer de **actiegroep**.

    ![Actiegroep, scherm 1 van 4](media/monitor-using-azure-monitor/alerts_image9.png)

    ![Actiegroep, scherm 2 van 4](media/monitor-using-azure-monitor/alerts_image10.png)

    ![Actiegroep, scherm 3 van 4](media/monitor-using-azure-monitor/alerts_image11.png)

    ![Actiegroep, scherm 4 van 4](media/monitor-using-azure-monitor/alerts_image12.png)

## <a name="next-steps"></a>Volgende stappen

Zie [bewaken en beheren van pijplijnen programmatisch](monitor-programmatically.md) artikel voor meer informatie over het controleren en beheren van pijplijnen met code.
