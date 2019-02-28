---
title: Inschakelen en logboeken met diagnostische gegevens weergeven voor Azure Data Lake Analytics
description: Meer informatie over het instellen en diagnostische logboeken openen voor Azure Data Lake Analytics
services: data-lake-analytics
ms.service: data-lake-analytics
author: jasonwhowell
ms.author: jasonh
ms.assetid: cf5633d4-bc43-444e-90fc-f90fbd0b7935
ms.topic: conceptual
ms.date: 02/12/2018
ms.openlocfilehash: 7fd88383e909ebd6be64c22721b813946e37179e
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56959122"
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Diagnostische logboeken openen voor Azure Data Lake Analytics

Diagnostische logboekregistratie kunt u voor het verzamelen van audittrails voor gegevenstoegang. Deze logboeken vindt u informatie, zoals:

* Een lijst met gebruikers die toegang krijgen de gegevens tot.
* Hoe vaak de gegevens worden gebruikt.
* Hoeveel gegevens worden opgeslagen in het account.

## <a name="enable-logging"></a>Logboekregistratie inschakelen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Open uw Data Lake Analytics-account en selecteer **diagnostische logboeken** uit de __Monitor__ sectie. Selecteer vervolgens __diagnostische gegevens inschakelen__.

    ![Diagnostische gegevens voor het verzamelen van controle en logboeken aanvragen inschakelen](./media/data-lake-analytics-diagnostic-logs/turn-on-logging.png)

3. Van __diagnostische instellingen__, voer een __naam__ voor deze configuratie voor logboekregistratie en opties voor logboekregistratie en selecteer vervolgens.

    ![Diagnostische gegevens voor het verzamelen van controle en aanvragen van logboek inschakelen](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "logboeken met diagnostische gegevens inschakelen")

   * U kunt opslaan en verwerken de gegevens op drie verschillende manieren.

     * Selecteer __archiveren naar een opslagaccount__ voor het opslaan van Logboeken in Azure storage-account. Gebruik deze optie als u wilt archiveren van de gegevens. Als u deze optie selecteert, moet u een Azure storage-account om op te slaan van de logboeken opgeven.

     * Selecteer **Stream naar een Event Hub** logboek om gegevens te streamen naar een Azure Event Hub. Gebruik deze optie als u een downstream-verwerkingen pijplijn hebt die wordt binnenkomende Logboeken in realtime analyseren. Als u deze optie selecteert, moet u de details opgeven voor de Azure Event Hub die u wilt gebruiken.

     * Selecteer __verzenden naar Log Analytics__ de gegevens te verzenden naar de Azure Monitor-service. Gebruik deze optie als u met behulp van Azure Monitor-logboeken wilt te verzamelen en analyseren van Logboeken.
   * Geef op of u wilt ophalen van de logboeken voor controle of aanvraag zich aanmeldt of beide.  Een aanvraag logboek vastgelegd elke API-aanvraag. Een controlelogboek registreert alle bewerkingen die worden geactiveerd door deze API-aanvraag.

   * Voor __archiveren naar een opslagaccount__, geef het aantal dagen om de gegevens te behouden.

   * Klik op __Opslaan__.

        > [!NOTE]
        > U moet een selecteren __archiveren naar een opslagaccount__, __Stream naar een Event Hub__ of __verzenden naar Log Analytics__ voordat u op de __opslaan__ de knop.

### <a name="use-the-azure-storage-account-that-contains-log-data"></a>Gebruik de Azure Storage-account met logboekgegevens

1. Als u wilt weergeven van de blobcontainers die opslaan van gegevens voor logboekregistratie, opent u het Azure Storage-account voor logboekregistratie gebruikt voor Data Lake Analytics en klik vervolgens op __Blobs__.

   * De container **insights-logs-audit** bevat de auditlogboeken.
   * De container **insights-logs-aanvragen** bevat de logboeken van de aanvraag.

2. De logboeken worden opgeslagen in de containers, onder de structuur van het volgende:

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json

   > [!NOTE]
   > De `##` vermeldingen in het pad bevatten het jaar, maand, dag en uur waarin het logboek is gemaakt. Data Lake Analytics maakt u één bestand per uur, dus `m=` bevat altijd een waarde van `00`.

    Het volledige pad naar een auditlogboek kan bijvoorbeeld worden:

        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Het volledige pad naar een logboek van de aanvraag kan op dezelfde manier worden:

        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Logboek-structuur

Logboeken van de audit en -aanvraag zijn in een gestructureerde JSON-indeling.

### <a name="request-logs"></a>Logboeken van de aanvraag

Hier volgt een voorbeeldvermelding voor het in het logboek van de aanvraag voor JSON-indeling. Elke blob heeft een basis-object met de naam **records** die een matrix met objecten logboek bevat.

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Schema voor het logboek van aanvraag

| Name | Type | Description |
| --- | --- | --- |
| time |String |De tijdstempel (in UTC) van het logboek |
| resourceId |String |De id van de resource die bewerking duurde plaats op |
| category |String |De logboekcategorie. Bijvoorbeeld, **aanvragen**. |
| operationName |String |Naam van de bewerking die wordt vastgelegd. Bijvoorbeeld: GetAggregatedJobHistory. |
| resultType |String |De status van de bewerking, bijvoorbeeld 200. |
| callerIpAddress |String |Het IP-adres van de client die de aanvraag |
| correlationId |String |De id van het logboek. Deze waarde kan worden gebruikt voor het groeperen van een set van gerelateerde vermeldingen. |
| identity |Object |De identiteit die het logboek is gegenereerd |
| properties |JSON |Zie de volgende sectie (aanvraag log eigenschappen schema) voor meer informatie |

#### <a name="request-log-properties-schema"></a>Schema voor eigenschappen van aanvraag-logboek

| Name | Type | Description |
| --- | --- | --- |
| HttpMethod |String |De HTTP-methode gebruikt voor de bewerking. Bijvoorbeeld, krijgen. |
| Pad |String |Het pad naar de bewerking is uitgevoerd op |
| RequestContentLength |int |De lengte van de inhoud van de HTTP-aanvraag |
| ClientRequestId |String |De id die is uniek voor deze aanvraag |
| StartTime |String |Het tijdstip waarop de server de aanvraag ontvangen |
| EndTime |String |Het tijdstip waarop de server een antwoord verzonden |

### <a name="audit-logs"></a>Controlelogboeken

Hier volgt een voorbeeldvermelding voor het in het auditlogboek voor JSON-indeling. Elke blob heeft een basis-object met de naam **records** die een matrix met objecten logboek bevat.

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job",
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Schema van auditlogboek

| Name | Type | Description |
| --- | --- | --- |
| time |String |De tijdstempel (in UTC) van het logboek |
| resourceId |String |De id van de resource die bewerking duurde plaats op |
| category |String |De logboekcategorie. Bijvoorbeeld, **Audit**. |
| operationName |String |Naam van de bewerking die wordt vastgelegd. Bijvoorbeeld: JobSubmitted. |
| resultType |String |Een substatus voor de status van de taak (operationName). |
| resultSignature |String |Meer informatie over de status van de taak (operationName). |
| identity |String |De gebruiker die de bewerking aangevraagd. Bijvoorbeeld susan@contoso.com. |
| properties |JSON |Zie de volgende sectie (Audit log eigenschappen schema) voor meer informatie |

> [!NOTE]
> **resultType** en **resultSignature** bevatten informatie over het resultaat van een bewerking, en alleen een waarde bevatten als een bewerking is voltooid. Bijvoorbeeld, ze alleen bevatten een waarde wanneer **operationName** bevat een waarde van **JobStarted** of **JobEnded**.
>
>

#### <a name="audit-log-properties-schema"></a>Audit log eigenschappen schema

| Name | Type | Description |
| --- | --- | --- |
| JobId |String |De ID die is toegewezen aan de job |
| JobName |String |De naam die is opgegeven voor de taak |
| JobRunTime |String |De runtime die wordt gebruikt voor het verwerken van de taak |
| SubmitTime |String |De tijd (in UTC) dat de taak is verzonden |
| StartTime |String |De tijd die de taak is gestart na het opsturen hiervan (in UTC) |
| EndTime |String |De tijd die de taak is beëindigd |
| Parallelle uitvoering |String |Het aantal aangevraagd voor deze taak tijdens het indienen van Data Lake Analytics-eenheden |

> [!NOTE]
> **SubmitTime**, **StartTime**, **EndTime**, en **parallelle uitvoering** bevatten informatie over een bewerking. Deze vermeldingen alleen een waarde bevatten als die bewerking is gestart of voltooid. Bijvoorbeeld, **SubmitTime** bevat alleen een waarde na **operationName** heeft de waarde **JobSubmitted**.

## <a name="process-the-log-data"></a>De logboekgegevens worden verwerkt

Azure Data Lake Analytics bevat een voorbeeld over het verwerken en analyseren van de logboekgegevens. U vindt het voorbeeld in [ https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample ](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md)
