---
title: Data Factory met Azure Prestatiemeter bewaken | Microsoft Docs
description: Informatie over het gebruik van de Monitor voor Azure Data Factory-pijplijnen bewaken door het inschakelen van diagnostische logboeken met gegevens van Azure Data Factory.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: shlo
ms.openlocfilehash: cae3c797171c3904f100ae3cdec47a31b06d3b31
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="monitor-data-factories-using-azure-monitor"></a>Monitor voor data Factory met Azure-Monitor  
Cloud-toepassingen zijn complexe met veel bewegende onderdelen. Monitoring biedt gegevens om ervoor te zorgen dat uw toepassing up blijft en wordt uitgevoerd in een foutloze toestand bevindt. Ook kunt u potentiële problemen voorkomen of oplossen uit het verleden zijn. Bovendien kunt u bewakingsgegevens grondige om inzicht te krijgen over uw toepassing. Deze kennis kan u helpen bij het verbeteren van de prestaties van toepassingen of onderhoud of acties die anders worden handmatige interventie moeten automatiseren.

Azure biedt een basisniveau infrastructuur metrische gegevens en logboeken voor de meeste services in Microsoft Azure. Zie voor meer informatie [bewakingsoverzicht](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor). Azure diagnostische logboeken zijn verzonden door een resource van logboeken die uitgebreide, regelmatig gegevens over de werking van die bron leveren. Data Factory levert diagnostische logboeken in de Azure-Monitor. 

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [bewaken en beheren van pijplijnen in Data Factory version1 gedefinieerd](v1/data-factory-monitor-manage-pipelines.md).

## <a name="diagnostic-logs"></a>Diagnostische logboeken

* Bewaar ze op een **Opslagaccount** voor inspectie controle of handmatig. U kunt de retentietijd (in dagen) met behulp van de diagnostische instellingen opgeven.
* Ze streamt **Event Hubs** voor opname door een service van derden of aangepaste analytics-oplossing zoals Power BI.
* Analyseer ze met **Operations Management Suite (OMS) Log Analytics**

U kunt een opslag-account of gebeurtenis hub naamruimte die zich niet in hetzelfde abonnement als de resource die is tekensetcodering Logboeken kunt gebruiken. De gebruiker die u configureert u de instelling moet de juiste rollen gebaseerde toegang toegangsbeheer (RBAC) toegang hebben tot beide abonnementen.

## <a name="set-up-diagnostic-logs"></a>Diagnostische logboeken instellen

### <a name="diagnostic-settings"></a>Diagnostische instellingen
Diagnostische logboeken voor niet-rekenresources zijn geconfigureerd met behulp van diagnostische instellingen. Diagnostische instellingen voor een besturingselement resource:

* Diagnostische logboeken waarnaar worden verzonden (Storage-Account, Event Hubs en/of logboekanalyse OMS).
* Welke categorieën logboek worden verzonden.
* Hoe lang elke categorie van het logboek moet worden bewaard in een opslagaccount
* Een bewaartermijn van nul dagen betekent logboeken permanent worden bewaard. Anders wordt mag de waarde een onbeperkt aantal dagen tussen 1 en 2147483647.
* Als het bewaarbeleid worden ingesteld, maar Logboeken opslaan in een opslagaccount is uitgeschakeld (bijvoorbeeld alleen Event Hubs of OMS-opties zijn geselecteerd), het bewaarbeleid hebben geen effect.
* Bewaarbeleid zijn toegepaste per dag, dus aan het einde van een dag (UTC), logboeken van de dag dat nu is buiten de bewaarperiode beleid worden verwijderd. Bijvoorbeeld, als u had een bewaarbeleid van één dag, zou aan het begin van vandaag de dag de logboeken van de dag voordat gisteren worden verwijderd.

### <a name="enable-diagnostic-logs-via-rest-apis"></a>Schakel diagnostische logboeken via REST-API 's

Maken of bijwerken van een instelling diagnostische gegevens in Azure Monitor REST-API

**Aanvraag**
```
PUT
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

**Headers**
* Vervang `{api-version}` met `2016-09-01`.
* Vervang `{resource-id}` aan de bron-ID van de resource die u wilt bewerken van diagnostische instellingen. Voor meer informatie [resourcegroepen gebruiken voor het beheren van uw Azure-resources](../azure-resource-manager/resource-group-portal.md).
* Stel de `Content-Type` koptekst tot `application/json`.
* Stel de autorisatie-header op een JSON web token die u via Azure Active Directory aanschaft. Zie voor meer informatie [aanvragen verifiëren](../active-directory/develop/active-directory-authentication-scenarios.md).

**Hoofdtekst**
```json
{
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<OMSName>",
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

| Eigenschap | Type | Beschrijving |
| --- | --- | --- |
| storageAccountId |Tekenreeks | De resource-ID van het opslagaccount waarnaar u wilt verzenden van diagnostische logboeken |
| serviceBusRuleId |Tekenreeks | De service bus regel-ID van de service bus-naamruimte waarin u hebben van Event Hubs gemaakt wilt voor het streamen van diagnostische logboeken. De regel-ID van de indeling is: "{service bus resource ID} /authorizationrules/ {naam} '.|
| WorkspaceId | Complex Type | Matrix van metrisch tijdsinterval rijstkorrels en hun bewaarbeleid. Deze eigenschap is momenteel leeg. |
|metrics| Parameterwaarden van de pijplijn uitvoeren om te worden doorgegeven aan de pijplijn aangeroepen| Een JSON-object parameternamen toewijzen aan argumentwaarden | 
| logboeken| Complex Type| Naam van een categorie diagnostische logboeken voor een resourcetype. Als u de lijst met categorieën van diagnostische logboeken voor een resource, moet u eerst een GET-bewerking diagnostische instellingen uitvoeren. |
| category| Tekenreeks| Matrix van logboek-categorieën en hun bewaarbeleid |
| TimeGrain | Tekenreeks | De granulatie van de metrische gegevens die zijn vastgelegd in de ISO 8601-notatie voor de duur. Moet PT1M (één minuut)|
| ingeschakeld| Boole-waarde | Geeft aan of de verzameling van deze categorie metriek of logboekbestanden is ingeschakeld voor deze bron|
| retentionPolicy| Complex Type| Beschrijft het bewaarbeleid voor een categorie metriek of logboekbestand. Gebruikt voor het account opslagoptie alleen.|
| dagen| Int| Aantal dagen wilt bewaren van de logboeken of metrische gegevens. De waarde 0 wordt de logboeken voor onbepaalde tijd bewaard. Gebruikt voor het account opslagoptie alleen. |

**Antwoord**

200 OK


```json
{
    "id": "/subscriptions/1e42591f-1f0c-4c5a-b7f2-a268f6105ec5/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.OperationalInsights/workspaces/<OMSName>",
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

Ophalen van informatie over het instellen van diagnostische gegevens in Azure Monitor REST-API

**Aanvraag**
```
GET
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

**Headers**
* Vervang `{api-version}` met `2016-09-01`.
* Vervang `{resource-id}` aan de bron-ID van de resource die u wilt bewerken van diagnostische instellingen. Voor meer informatie resourcegroepen gebruiken voor het beheren van uw Azure-resources.
* Stel de `Content-Type` koptekst tot `application/json`.
* Stel de autorisatie-header op een JSON Web Token die u via Azure Active Directory aanschaft. Zie voor meer informatie Authenticating aanvragen.

**Antwoord**

200 OK

```json
{
    "id": "/subscriptions/1e42591f-1f0c-4c5a-b7f2-a268f6105ec5/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/1e42591f-1f0c-4c5a-b7f2-a268f6105ec5/resourceGroups/shloprivate/providers/Microsoft.Storage/storageAccounts/azmonlogs",
        "serviceBusRuleId": "/subscriptions/1e42591f-1f0c-4c5a-b7f2-a268f6105ec5/resourceGroups/shloprivate/providers/Microsoft.EventHub/namespaces/shloeventhub/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/0ee78edb-a0ad-456c-a0a2-901bf542c102/resourceGroups/ADF/providers/Microsoft.OperationalInsights/workspaces/mihaipie",
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
Hier voor meer informatie] (https://msdn.microsoft.com/en-us/library/azure/dn931932.aspx)

## <a name="schema-of-logs--events"></a>Schema van Logboeken en gebeurtenissen

### <a name="activity-run-logs-attributes"></a>Uitvoeren van de activiteit logboeken kenmerken

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
   "properties:" 
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

| Eigenschap | Type | Beschrijving | Voorbeeld |
| --- | --- | --- | --- |
| Niveau |Tekenreeks | Niveau van de diagnostische logboeken. Niveau 4 is altijd het geval voor activiteit uitgevoerd aanmeldt. | `4`  |
| correlationId |Tekenreeks | Unieke ID voor het bijhouden van een bepaalde aanvraag end-to-end | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| tijd | Tekenreeks | Tijd van de gebeurtenis in timespan, UTC-notatie | `YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|activityRunId| Tekenreeks| ID van de activiteit die wordt uitgevoerd | `3a171e1f-b36e-4b80-8a54-5625394f4354` |
|pipelineRunId| Tekenreeks| ID van de pijplijn uitvoeren | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|resourceId| Tekenreeks | Gekoppelde resource-ID voor de data factory-bron | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| Tekenreeks | De categorie van diagnostische logboeken. Deze eigenschap instellen op "Activiteiten" | `ActivityRuns` |
|niveau| Tekenreeks | Niveau van de diagnostische logboeken. Deze eigenschap instellen op 'Ter informatie' | `Informational` |
|operationName| Tekenreeks |Naam van de activiteit met status. Als de status de heartbeat gestart is, is het `MyActivity -`. Als de status de heartbeat end is, is het `MyActivity - Succeeded` met de laatste status | `MyActivity - Succeeded` |
|pipelineName| Tekenreeks | Naam van de pijplijn | `MyPipeline` |
|activityName| Tekenreeks | Naam van de activiteit | `MyActivity` |
|start| Tekenreeks | Begin van de activiteit die wordt uitgevoerd in timespan, UTC-notatie | `2017-06-26T20:55:29.5007959Z`|
|einde| Tekenreeks | Einde van de activiteit wordt uitgevoerd in timespan, UTC-notatie. Als de activiteit is niet beëindigd nog (Diagnostische logboekregistratie voor het starten van een activiteit), een standaardwaarde van `1601-01-01T00:00:00Z` is ingesteld.  | `2017-06-26T20:55:29.5007959Z` |


### <a name="pipeline-run-logs-attributes"></a>Uitvoeren van de pijplijn logboeken kenmerken

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

| Eigenschap | Type | Beschrijving | Voorbeeld |
| --- | --- | --- | --- |
| Niveau |Tekenreeks | Niveau van de diagnostische logboeken. Niveau 4 geldt voor de activiteit uitgevoerd aanmeldt. | `4`  |
| correlationId |Tekenreeks | Unieke ID voor het bijhouden van een bepaalde aanvraag end-to-end | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| tijd | Tekenreeks | Tijd van de gebeurtenis in timespan, UTC-notatie | `YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|runId| Tekenreeks| ID van de pijplijn uitvoeren | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|resourceId| Tekenreeks | Gekoppelde resource-ID voor de data factory-bron | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| Tekenreeks | De categorie van diagnostische logboeken. Deze eigenschap instellen op 'PipelineRuns' | `PipelineRuns` |
|niveau| Tekenreeks | Niveau van de diagnostische logboeken. Deze eigenschap instellen op 'Ter informatie' | `Informational` |
|operationName| Tekenreeks |Naam van de pijplijn met de status. 'Pipeline - is voltooid' met de laatste status als de pijplijn uitgevoerd is voltooid| `MyPipeline - Succeeded` |
|pipelineName| Tekenreeks | Naam van de pijplijn | `MyPipeline` |
|start| Tekenreeks | Begin van de activiteit die wordt uitgevoerd in timespan, UTC-notatie | `2017-06-26T20:55:29.5007959Z`|
|einde| Tekenreeks | Einde van de activiteit wordt uitgevoerd in timespan, UTC-notatie. Als de activiteit is niet beëindigd nog (Diagnostische logboekregistratie voor het starten van een activiteit), een standaardwaarde van `1601-01-01T00:00:00Z` is ingesteld.  | `2017-06-26T20:55:29.5007959Z` |
|status| Tekenreeks | Definitieve status van de pijplijn uitvoeren (geslaagd of mislukt) | `Succeeded`|


### <a name="trigger-run-logs-attributes"></a>Voer Logboeken kenmerken activeren

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

| Eigenschap | Type | Beschrijving | Voorbeeld |
| --- | --- | --- | --- |
| Niveau |Tekenreeks | Niveau van de diagnostische logboeken. Ingesteld op niveau 4 voor activiteit uitgevoerd aanmeldt. | `4`  |
| correlationId |Tekenreeks | Unieke ID voor het bijhouden van een bepaalde aanvraag end-to-end | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| tijd | Tekenreeks | Tijd van de gebeurtenis in timespan, UTC-notatie | `YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|triggerId| Tekenreeks| ID van de trigger uitvoeren | `08587023010602533858661257311` |
|resourceId| Tekenreeks | Gekoppelde resource-ID voor de data factory-bron | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| Tekenreeks | De categorie van diagnostische logboeken. Deze eigenschap instellen op 'PipelineRuns' | `PipelineRuns` |
|niveau| Tekenreeks | Niveau van de diagnostische logboeken. Deze eigenschap instellen op 'Ter informatie' | `Informational` |
|operationName| Tekenreeks |Naam van de trigger met de laatste status of deze is gestart. 'MyTrigger - is voltooid' als de heartbeat geslaagd is| `MyTrigger - Succeeded` |
|triggerName| Tekenreeks | Naam van de trigger | `MyTrigger` |
|triggerType| Tekenreeks | Type van de trigger (handmatige trigger of schema Trigger) | `ScheduleTrigger` |
|triggerEvent| Tekenreeks | Gebeurtenis van de trigger | `ScheduleTime - 2017-07-06T01:50:25Z` |
|start| Tekenreeks | Begin van de trigger brand in timespan, UTC-notatie | `2017-06-26T20:55:29.5007959Z`|
|status| Tekenreeks | Eindstatus of trigger met succes gestart (geslaagd of mislukt) | `Succeeded`|

### <a name="metrics"></a>Metrische gegevens

Monitor voor Azure kunt u telemetrie om meer inzicht verkrijgen in de prestaties en de status van uw workloads in Azure gebruiken. De belangrijkste type Azure telemetriegegevens is de metrische gegevens die (ook wel prestatiemeteritems) die door de meeste Azure-resources. Monitor voor Azure biedt verschillende manieren configureren en gebruiken van deze metrische gegevens voor bewaking en probleemoplossing.

ADFV2 verzendt de volgende metrische gegevens

| **Gegevens**           | **Metrische weergavenaam**         | **Unit** | **Samenvoegingstype** | **Beschrijving**                                       |
|----------------------|---------------------------------|----------|----------------------|-------------------------------------------------------|
| PipelineSucceededRun | Pijplijn wordt uitgevoerd metrische gegevens is voltooid | Count    | Totaal                | Totaal aantal pijplijnen geslaagd uitgevoerd binnen een minuut venster |
| PipelineFailedRuns   | Pijplijn wordt uitgevoerd metrische gegevens is mislukt    | Count    | Totaal                | Totaal aantal pijplijnen mislukte uitgevoerd binnen een minuut    |
| ActiviySucceededRuns | Metrische gegevens van activiteiten wordt uitgevoerd is voltooid | Count    | Totaal                | Totale activiteit geslaagd uitgevoerd binnen een minuut venster  |
| ActivityFailedRuns   | Metrische gegevens van activiteiten wordt uitgevoerd is mislukt    | Count    | Totaal                | Totale activiteit actief is mislukt binnen een minuut     |
| TriggerSucceededRuns | Trigger wordt uitgevoerd metrische gegevens is voltooid  | Count    | Totaal                | Totaal aantal trigger voert geslaagd binnen een minuut   |
| TriggerFailedRuns    | Trigger wordt uitgevoerd metrische gegevens is mislukt     | Count    | Totaal                | Totaal aantal trigger voert is mislukt binnen een minuut      |

Toegang tot de metrische gegevens, volg de instructies in het artikel - https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics 

## <a name="next-steps"></a>Volgende stappen
Zie [bewaken en beheren van pijplijnen programmatisch](monitor-programmatically.md) artikel voor meer informatie over het controleren en beheren van pijplijnen door te voeren. 