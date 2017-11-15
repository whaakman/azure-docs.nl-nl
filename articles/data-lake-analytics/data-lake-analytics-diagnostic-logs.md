---
title: Diagnostische logboeken bekijken voor Azure Data Lake Analytics | Microsoft Docs
description: 'Begrijpen hoe instellen en toegang tot diagnoselogboeken voor Azure Data Lake analytics '
services: data-lake-analytics
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf5633d4-bc43-444e-90fc-f90fbd0b7935
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/08/2017
ms.author: larryfr
ms.openlocfilehash: 5bab7a0646d34de3b6d71370a0fa4216845ee6a2
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Toegang tot diagnoselogboeken voor Azure Data Lake Analytics

Diagnostische logboekregistratie kunt u voor het verzamelen van gegevens audittrails voor bestandstoegang. Deze logboeken vindt u informatie, zoals:

* Een lijst met gebruikers die toegang de gegevens tot.
* Hoe vaak de gegevens worden gebruikt.
* Hoeveel gegevens worden opgeslagen in het account.

## <a name="enable-logging"></a>Logboekregistratie inschakelen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Uw Data Lake Analytics-account openen en selecteer **diagnostische logboeken** van de __Monitor__ sectie. Selecteer vervolgens __diagnostische gegevens inschakelen__.

    ![Diagnostische gegevens voor het verzamelen van de audit en logboeken aanvragen inschakelen](./media/data-lake-analytics-diagnostic-logs/turn-on-logging.png)

3. Van __diagnostische instellingen__, voer een __naam__ voor deze configuratie voor logboekregistratie en selecteer vervolgens registratieopties.

    ![Diagnostische gegevens voor het verzamelen van de audit en logboeken aanvragen inschakelen](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "logboeken met diagnostische gegevens inschakelen")

   * U kunt kiezen om te verwerken/store de gegevens op drie verschillende manieren.

     * Selecteer __archiveren naar een opslagaccount__ voor het opslaan van Logboeken in Azure storage-account. Gebruik deze optie als u wilt de gegevens te archiveren. Als u deze optie selecteert, moet u een Azure storage-account voor het opslaan van de logboeken naar opgeven.

     * Selecteer **Stream naar een Event Hub** stroom logboek gegevens naar een Azure Event Hub. Gebruik deze optie als u hebt een downstreamverwerking pijplijn die is binnenkomende Logboeken in realtime analyseren. Als u deze optie selecteert, moet u de details opgeven voor de Azure Event Hub die u wilt gebruiken.

     * Selecteer __verzenden met logboekanalyse__ de gegevens verzenden naar het Log Analytics-service. Gebruik deze optie als u wilt Log Analytics gebruiken om te verzamelen en analyseren van Logboeken.
   * Geef op of u wilt ophalen controlelogboeken of Logboeken aanvragen of beide.  Een aanvraag logboek worden vastgelegd elke API-aanvraag. Een controlelogboek registreert alle bewerkingen die worden geactiveerd door deze API-aanvraag.

   * Voor __archiveren naar een opslagaccount__, geef het aantal dagen om de gegevens te behouden.

   * Klik op __Opslaan__.

        > [!NOTE]
        > U moet een selecteren __archiveren naar een opslagaccount__, __Stream naar een Event Hub__ of __verzenden met logboekanalyse__ voordat u op de __opslaan__ knop.

### <a name="use-the-azure-storage-account-that-contains-log-data"></a>Gebruik de Azure Storage-account met logboekgegevens

1. Als u wilt weergeven in de blobcontainers die logboekgegevens houdt, open de Azure Storage-account voor Data Lake Analytics gebruikt voor aanmelding en klik vervolgens op __Blobs__.

   * De container **insights-logboeken-controlegebeurtenissen** bevat de controlelogboeken.
   * De container **insights-logboeken-aanvragen** bevat de logboeken van de aanvraag.

2. De logboeken worden opgeslagen in de containers onder de structuur van het volgende:

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
   > De `##` vermeldingen in het pad bevatten het jaar, maand, dag en uur waarin het logboek is gemaakt. Data Lake Analytics maakt een bestand om het uur, dus `m=` bevat altijd een waarde van `00`.

    Het volledige pad naar een controlelogboek kan bijvoorbeeld worden:

        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Daarnaast kan het volledige pad naar een logboek met aanvragen worden:

        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Logboek-structuur

Logboeken van de audit en aanvraag zijn in een gestructureerde JSON-indeling.

### <a name="request-logs"></a>Logboeken aanvragen

Hier volgt een voorbeeldvermelding voor het in het logboek voor aanvraag JSON-indeling. Elke blob heeft een basis-object aangeroepen **records** die een matrix van logboek-objecten bevat.

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

#### <a name="request-log-schema"></a>Schema voor aanvraag-logboek

| Naam | Type | Beschrijving |
| --- | --- | --- |
| tijd |Tekenreeks |De tijdstempel (in UTC) van het logboek |
| resourceId |Tekenreeks |De id van de resource die bewerking vond plaats op |
| category |Tekenreeks |De logboek-categorie. Bijvoorbeeld: **aanvragen**. |
| operationName |Tekenreeks |De naam van de bewerking die wordt vastgelegd. Bijvoorbeeld: GetAggregatedJobHistory. |
| resultType |Tekenreeks |De status van de bewerking, bijvoorbeeld 200. |
| callerIpAddress |Tekenreeks |Het IP-adres van de client die de aanvraag |
| correlationId |Tekenreeks |De id van het logboek. Deze waarde kan worden gebruikt voor het groeperen van een set van gerelateerde logboekvermeldingen. |
| identity |Object |De identiteit die door het logboek is gegenereerd |
| properties |JSON |Zie de volgende sectie (aanvraag logboek eigenschappen schema) voor meer informatie |

#### <a name="request-log-properties-schema"></a>Aanvraag logboek eigenschappen schema

| Naam | Type | Beschrijving |
| --- | --- | --- |
| HttpMethod |Tekenreeks |De HTTP-methode gebruikt voor het opnieuw. Bijvoorbeeld, ophalen. |
| Pad |Tekenreeks |Het pad van de bewerking is uitgevoerd op |
| RequestContentLength |int |De lengte van de inhoud van de HTTP-aanvraag |
| clientRequestId |Tekenreeks |De id die is uniek voor deze aanvraag |
| StartTime |Tekenreeks |Het tijdstip waarop de server de aanvraag ontvangen |
| Eindtijd |Tekenreeks |Het tijdstip waarop de server een antwoord verzonden |

### <a name="audit-logs"></a>Controlelogboeken

Hier wordt een voorbeeldvermelding voor het controlelogboek met JSON-indeling. Elke blob heeft een basis-object aangeroepen **records** die een matrix van logboek-objecten bevat.

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

#### <a name="audit-log-schema"></a>Audit log schema

| Naam | Type | Beschrijving |
| --- | --- | --- |
| tijd |Tekenreeks |De tijdstempel (in UTC) van het logboek |
| resourceId |Tekenreeks |De id van de resource die bewerking vond plaats op |
| category |Tekenreeks |De logboek-categorie. Bijvoorbeeld: **Audit**. |
| operationName |Tekenreeks |De naam van de bewerking die wordt vastgelegd. Bijvoorbeeld: JobSubmitted. |
| resultType |Tekenreeks |Een substatus voor de status van de taak (operationName). |
| resultSignature |Tekenreeks |Meer informatie over de status van de taak (operationName). |
| identity |Tekenreeks |De gebruiker die de bewerking wordt aangevraagd. Bijvoorbeeld susan@contoso.com. |
| properties |JSON |Zie de volgende sectie (schema Audit log-eigenschappen) voor meer informatie |

> [!NOTE]
> **resultType** en **resultSignature** bieden informatie over het resultaat van een bewerking, en alleen een waarde bevatten als een bewerking is voltooid. Bijvoorbeeld, ze alleen bevatten een waarde wanneer **operationName** bevat een waarde van **JobStarted** of **JobEnded**.
>
>

#### <a name="audit-log-properties-schema"></a>Audit log eigenschappen schema

| Naam | Type | Beschrijving |
| --- | --- | --- |
| JobId |Tekenreeks |De ID die is toegewezen aan het project |
| Taaknaam |Tekenreeks |De naam die is opgegeven voor de taak |
| JobRunTime |Tekenreeks |De runtime gebruikt voor het verwerken van de taak |
| SubmitTime |Tekenreeks |De tijd (in UTC) dat de taak is verzonden |
| StartTime |Tekenreeks |De tijd dat de taak is gestart na het indienen (in UTC) |
| Eindtijd |Tekenreeks |De tijd dat de taak is beëindigd |
| Parallelle uitvoering |Tekenreeks |Het aantal aangevraagde voor deze taak tijdens het indienen van Data Lake Analytics-eenheden |

> [!NOTE]
> **SubmitTime**, **StartTime**, **EndTime**, en **parallelle uitvoering** bieden informatie over een bewerking. Deze vermeldingen alleen een waarde bevatten als die opnieuw is gestart of voltooid. Bijvoorbeeld: **SubmitTime** bevat alleen een waarde na **operationName** heeft de waarde **JobSubmitted**.

## <a name="process-the-log-data"></a>De logboekgegevens verwerken

Een voorbeeld van een biedt Azure Data Lake Analytics voor het verwerken en analyseren van de logboekgegevens. U vindt het voorbeeld op [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md)
