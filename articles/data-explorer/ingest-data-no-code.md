---
title: 'Zelfstudie: Diagnostische gegevens en activiteitenlogboekgegevens in Azure Data Explorer opnemen zonder één regel code'
description: In deze zelfstudie leert u hoe u zonder één regel code gegevens kunt opnemen in Azure Data Explorer en query's op die gegevens kunt uitvoeren.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: tutorial
ms.date: 2/5/2019
ms.openlocfilehash: 39019c4b11d055aa8f550928bd677e4ce33d6252
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55885218"
---
# <a name="tutorial-ingest-data-in-azure-data-explorer-without-one-line-of-code"></a>Zelfstudie: Gegevens opnemen in Azure Data Explorer zonder één regel code

In deze zelfstudie leert u hoe u zonder één regel code diagnostische gegevens en activiteitenlogboekgegevens in een Azure Data Explorer-cluster kunt opnemen. Met deze eenvoudige methode voor gegevensopname kunt u snel beginnen met het uitvoeren van query's voor Azure Data Explorer om gegevens te analyseren.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Maak tabellen en toewijzing van opname in een Azure Data Explorer-database.
> * Maak de opgenomen gegevens op met behulp van updatebeleid.
> * Maak een [Event Hub](/azure/event-hubs/event-hubs-about) en koppel deze aan Azure Data Explorer.
> * Stream gegevens naar een Event Hub vanuit [diagnostische logboeken van Azure Monitor](/azure/azure-monitor/platform/diagnostic-logs-overview) en [activiteitenlogboeken van Azure Monitor](/azure/azure-monitor/platform/activity-logs-overview).
> * Vraag de opgenomen gegevens op met behulp van Azure Data Explorer.

> [!NOTE]
> Maak alle resources in dezelfde Azure-locatie/regio. Dit is een vereiste voor diagnostische logboeken van Azure Monitor.

## <a name="prerequisites"></a>Vereisten

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis Azure-account](https://azure.microsoft.com/free/) aan voordat u begint.
* [Een Azure Data Explorer-cluster en -database](create-cluster-database-portal.md). In deze zelfstudie is *AzureMonitoring* de databasenaam.

## <a name="azure-monitoring-data-provider---diagnostic-and-activity-logs"></a>Azure Monitoring-gegevensprovider - diagnostische logboeken en activiteitenlogboeken

Bekijk en begrijp de gegevens die worden geleverd door de diagnostische logboeken en activiteitenlogboeken van Azure Monitoring. We gaan een opnamepijplijn maken op basis van deze gegevensschema's.

### <a name="diagnostic-logs-example"></a>Voorbeeld van diagnostische logboeken

Diagnostische logboeken van Azure bevatten metrische gegevens afkomstig van een Azure-service die gegevens levert over de werking van die service. Gegevens worden samengevoegd met een tijdsinterval van 1 minuut. Elke gebeurtenis van diagnostische logboeken bevat één record. Hieronder volgt een voorbeeld van een metrisch gebeurtenisschema voor Azure Data Explorer, op basis van queryduur:

```json
{
    "count": 14,
    "total": 0,
    "minimum": 0,
    "maximum": 0,
    "average": 0,
    "resourceId": "/SUBSCRIPTIONS/F3101802-8C4F-4E6E-819C-A3B5794D33DD/RESOURCEGROUPS/KEDAMARI/PROVIDERS/MICROSOFT.KUSTO/CLUSTERS/KEREN",
    "time": "2018-12-20T17:00:00.0000000Z",
    "metricName": "QueryDuration",
    "timeGrain": "PT1M"
}
```

### <a name="activity-logs-example"></a>Voorbeeld van activiteitenlogboeken

Azure-activiteitenlogboeken zijn aan een abonnement gekoppelde logboeken met een verzameling records. De logboeken bieden inzicht in de bewerkingen die worden uitgevoerd op resources in uw abonnement. In tegenstelling tot diagnostische logboeken bestaat een gebeurtenis in activiteitenlogboeken uit een matrix van records. Verderop in deze zelfstudie gaan we deze matrix van records opsplitsen. Hieronder volgt een voorbeeld van een gebeurtenis in een activiteitenlogboek voor het controleren van toegang:

```json
{
    "records": [
    {
        "time": "2018-12-26T16:23:06.1090193Z",
        "resourceId": "/SUBSCRIPTIONS/F80EB51C-C534-4F0B-80AB-AEBC290C1C19/RESOURCEGROUPS/CLEANUPSERVICE/PROVIDERS/MICROSOFT.WEB/SITES/CLNB5F73B70-DCA2-47C2-BB24-77B1A2CAAB4D/PROVIDERS/MICROSOFT.AUTHORIZATION",
        "operationName": "MICROSOFT.AUTHORIZATION/CHECKACCESS/ACTION",
        "category": "Action",
        "resultType": "Start",
        "resultSignature": "Started.",
        "durationMs": 0,
        "callerIpAddress": "13.66.225.188",
        "correlationId": "0de9f4bc-4adc-4209-a774-1b4f4ae573ed",
        "identity": {
            "authorization": {
                ...
            },
            "claims": {
                ...
            }
        },
        "level": "Information",
        "location": "global",
        "properties": {
            ...
        }
    },
    {
        "time": "2018-12-26T16:23:06.3040244Z",
        "resourceId": "/SUBSCRIPTIONS/F80EB51C-C534-4F0B-80AB-AEBC290C1C19/RESOURCEGROUPS/CLEANUPSERVICE/PROVIDERS/MICROSOFT.WEB/SITES/CLNB5F73B70-DCA2-47C2-BB24-77B1A2CAAB4D/PROVIDERS/MICROSOFT.AUTHORIZATION",
        "operationName": "MICROSOFT.AUTHORIZATION/CHECKACCESS/ACTION",
        "category": "Action",
        "resultType": "Success",
        "resultSignature": "Succeeded.OK",
        "durationMs": 194,
        "callerIpAddress": "13.66.225.188",
        "correlationId": "0de9f4bc-4adc-4209-a774-1b4f4ae573ed",
        "identity": {
            "authorization": {
                ...
            },
            "claims": {
                ...
            }
        },
        "level": "Information",
        "location": "global",
        "properties": {
            "statusCode": "OK",
            "serviceRequestId": "87acdebc-945f-4c0c-b931-03050e085626"
        }
    }]
}
```

## <a name="set-up-ingestion-pipeline-in-azure-data-explorer"></a>Opnamepijplijn instellen in Azure Data Explorer 

Het instellen van een pijplijn in Azure Data Explorer bestaat uit verschillende stappen, waaronder [het maken van een tabel en het opnemen van gegevens](/azure/data-explorer/ingest-sample-data#ingest-data). U kunt de gegevens ook bewerken, toewijzen en bijwerken.

### <a name="connect-to-azure-data-explorer-web-ui"></a>Verbinding maken met de web-UI van Azure Data Explorer

1. Selecteer **Query** in uw *AzureMonitoring*-database van Azure Data Explorer om de web-UI van Azure Data Explorer te openen.

    ![Query’s uitvoeren](media/ingest-data-no-code/query-database.png)

### <a name="create-target-tables"></a>Doeltabellen maken

Gebruik de web-UI van Azure Data Explorer om de doeltabellen te maken in de Azure Data Explorer-database.

#### <a name="diagnostic-logs-table"></a>Tabel met diagnostische logboeken

1. Maak een tabel *DiagnosticLogsRecords* in de *AzureMonitoring*-database die records voor de diagnostische logboeken ontvangt en gebruik daarvoor de opdracht `.create table`:

    ```kusto
    .create table DiagnosticLogsRecords (Timestamp:datetime, ResourceId:string, MetricName:string, Count:int, Total:double, Minimum:double, Maximum:double, Average:double, TimeGrain:string)
    ```

1. Selecteer **Uitvoeren** om de tabel te maken.

    ![Query uitvoeren](media/ingest-data-no-code/run-query.png)

#### <a name="activity-logs-tables"></a>Tabellen voor activiteitenlogboeken

Omdat de structuur van de activiteitenlogboeken geen tabelvorm heeft, moet u de gegevens manipuleren en elke gebeurtenis uitbreiden tot een of meer records. De onbewerkte gegevens worden opgenomen in de tussenliggende tabel *ActivityLogsRawRecords*. Dat is het moment waarop de gegevens worden bewerkt en uitgebreid. De uitgebreide gegevens worden vervolgens opgenomen in de tabel *ActivityLogsRecords* op basis van updatebeleid. Daarom moet u twee afzonderlijke tabellen maken voor het opnemen van activiteitenlogboeken.

1. Maak de tabel *ActivityLogsRecords* in de *AzureMonitoring*-database die records voor activiteitenlogboeken ontvangt. Voer de volgende Azure Data Explorer-query uit om de tabel te maken:

    ```kusto
    .create table ActivityLogsRecords (Timestamp:datetime, ResourceId:string, OperationName:string, Category:string, ResultType:string, ResultSignature:string, DurationMs:int, IdentityAuthorization:dynamic, IdentityClaims:dynamic, Location:string, Level:string)
    ```

1. Maak de tussenliggende gegevenstabel *ActivityLogsRawRecords* in de *AzureMonitoring*-database voor het manipuleren van gegevens:

    ```kusto
    .create table ActivityLogsRawRecords (Records:dynamic)
    ```

<!--
     ```kusto
     .alter-merge table ActivityLogsRawRecords policy retention softdelete = 0d
    <[Retention](/azure/kusto/management/retention-policy) for an intermediate data table is set at zero retention policy.
-->

### <a name="create-table-mappings"></a>Tabeltoewijzingen maken

 Vanwege de tabelindeling `json` is gegevenstoewijzing vereist. Met de `json`-toewijzingen wordt elk json-pad toegewezen aan de naam van een tabelkolom.

#### <a name="diagnostic-logs-table-mapping"></a>Toewijzing van tabel met diagnostische logboeken

Gebruik de volgende query om de gegevens toe te wijzen aan de tabel:

```kusto
.create table DiagnosticLogsRecords ingestion json mapping 'DiagnosticLogsRecordsMapping' '[
{"column":"Timestamp","path":"$.time"},
{"column":"ResourceId","path":"$.resourceId"},
{"column":"MetricName","path":"$.metricName"},
{"column":"Count","path":"$.count"},
{"column":"Total","path":"$.total"},
{"column":"Minimum","path":"$.minimum"},
{"column":"Maximum","path":"$.maximum"},
{"column":"Average","path":"$.average"},
{"column":"TimeGrain","path":"$.timeGrain"}]'
```

#### <a name="activity-logs-table-mapping"></a>Toewijzing van tabel met activiteitenlogboeken

Gebruik de volgende query om de gegevens toe te wijzen aan de tabel:

```kusto
.create table ActivityLogsRawRecords ingestion json mapping 'ActivityLogsRawRecordsMapping' '[
{"column":"Records","path":"$.records"}]'
```

### <a name="create-update-policy"></a>Updatebeleid maken

1. Maak een [functie](/azure/kusto/management/functions) waarmee de verzameling records wordt uitgebreid zodat elke waarde in de verzameling een afzonderlijke rij ontvangt. Gebruik de operator [`mvexpand`](/azure/kusto/query/mvexpandoperator):

    ```kusto
    .create function ActivityLogRecordsExpand() {
        ActivityLogsRawRecords
        | mvexpand events = Records
        | project
            Timestamp = todatetime(events["time"]),
            ResourceId = tostring(events["resourceId"]),
            OperationName = tostring(events["operationName"]),
            Category = tostring(events["category"]),
            ResultType = tostring(events["resultType"]),
            ResultSignature = tostring(events["resultSignature"]),
            DurationMs = toint(events["durationMs"]),
            IdentityAuthorization = events["identity.authorization"],
            IdentityClaims = events["identity.claims"],
            Location = tostring(events["location"]),
            Level = tostring(events["level"])
    }
    ```

2. Voeg [updatebeleid](/azure/kusto/concepts/updatepolicy) toe aan de doeltabel. De query wordt automatisch uitgevoerd op alle nieuw opgenomen gegevens in de tussenliggende gegevenstabel *ActivityLogsRawRecords* en de resultaten worden opgenomen in de tabel *ActivityLogsRecords*:

    ```kusto
    .alter table ActivityLogRecords policy update @'[{"Source": "ActivityLogsRawRecords", "Query": "ActivityLogRecordsExpand()", "IsEnabled": "True"}]'
    ```

## <a name="create-an-event-hub-namespace"></a>Een Event Hub-naamruimte maken

Diagnostische logboeken in Azure maken het exporteren van metrische gegevens naar een opslagaccount of een Event Hub mogelijk. In deze zelfstudie sturen we metrische gegevens door via een Event Hub. In de volgen stappen gaat u een Event Hubs-naamruimte en een Event Hub voor diagnostische logboeken maken. Azure Monitoring maakt de Event Hub *insights-operational-logs* voor activiteitenlogboeken.

1. Maak een Event Hub met behulp van een Azure Resource Manager-sjabloon in de Azure-portal. Gebruik de volgende knop om de implementatie te starten. Klik met de rechtermuisknop en selecteer **In nieuw venster openen**, zodat u de rest van de stappen in dit artikel kunt volgen. Klik op de knop **Implementeren in Azure** om de Azure-portal te openen.

    [![Implementeren in Azure](media/ingest-data-no-code/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

1. Maak een Event Hub-naamruimte en een Event Hub voor de diagnostische logboeken.

    ![Een Event Hub maken](media/ingest-data-no-code/event-hub.png)

    Vul het formulier in met de volgende gegevens. Gebruik de standaardwaarden voor alle instellingen die niet zijn vermeld in de volgende tabel.

    **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**
    |---|---|---|
    | Abonnement | Uw abonnement | Selecteer het Azure-abonnement dat u wilt gebruiken voor de Event Hub.|
    | Resourcegroep | *test-resource-group* | Maak een nieuwe resourcegroep. |
    | Locatie | Selecteer de regio die het beste voldoet aan uw behoeften. | Maak de Event Hub-naamruimte op dezelfde locatie als andere resources.
    | Naam van naamruimte | *AzureMonitoringData* | Kies een unieke naam waarmee de naamruimte kan worden geïdentificeerd.
    | Naam van Event Hub | *DiagnosticLogsData* | De Event Hub bevindt zich onder de naamruimte, wat een unieke bereikcontainer biedt. |
    | Naam van consumentengroep | *adxpipeline* | Maak een naam voor een consumentengroep. Met consumentengroepen kunnen meerdere gebruikstoepassingen elk een afzonderlijke weergave van de gebeurtenisstroom hebben. |
    | | |

## <a name="connect-azure-monitoring-logs-to-event-hub"></a>Azure Monitoring-logboeken verbinden met Event Hub

### <a name="diagnostic-logs-connection-to-event-hub"></a>Verbinding van diagnostische logboeken met Event Hub

Selecteer een resource van waaruit u metrische gegevens wilt exporteren. Er zijn verschillende resourcetypen voor het exporteren van diagnostische logboeken, te weten de Event Hub-naam, KeyVault, IoT-Hub en Azure Data Explorer-cluster. In deze zelfstudie gebruiken we het Azure Data Explorer-cluster als resource.

1. Selecteer uw Kusto-cluster in de Azure-portal.

    ![Diagnostische instellingen](media/ingest-data-no-code/diagnostic-settings.png)

1. Selecteer **Diagnostische instellingen** in het menu links.
1. Klik op de koppeling **Diagnostische gegevens inschakelen**. Het venster **Diagnostische instellingen** wordt geopend.

    ![Venster Diagnostische instellingen](media/ingest-data-no-code/diagnostic-settings-window.png)

1. Ga in het deelvenster **Diagnostische instellingen** als volgt te werk:
    1. Geef uw diagnostische logboekgegevens een naam: *ADXExportedData*
    1. Schakel het selectievakje **AllMetrics** in (optioneel).
    1. Schakel het selectievakje **Streamen naar een event hub** in.
    1. Klik op **Configureren**

1. Configureer in het deelvenster **Een event hub selecteren** het exporteren naar de Event Hub die u hebt gemaakt:
    1. **Selecteer de Event Hub-naamruimte** *AzureMonitoringData* in de vervolgkeuzelijst.
    1. **Selecteer de Event Hub-naamruimte** *diagnosticlogsdata* in de vervolgkeuzelijst.
    1. Selecteer **Naam van het Event Hub-beleid**  in de vervolgkeuzelijst.
    1. Klik op **OK**.

1. Klik op **Opslaan**. De naam en beleidsnaam van de Event hub-naamruimte worden weergegeven in het venster.

    ![Diagnostische instellingen opslaan](media/ingest-data-no-code/save-diagnostic-settings.png)

### <a name="activity-logs-connection-to-event-hub"></a>Verbinding van activiteitenlogboeken met Event Hub

1. Selecteer **Activiteitenlogboek** in het linkermenu van de Azure-portal.
1. Het venster **Activiteitenlogboek** wordt geopend. **Klik op Exporteren naar Event Hub**

    ![Activiteitenlogboek](media/ingest-data-no-code/activity-log.png)

1. Ga in het venster **Activiteitenlogboek exporteren** als volgt te werk:
 
    ![Activiteitenlogboek exporteren](media/ingest-data-no-code/export-activity-log.png)

    1. Selecteer uw abonnement.
    1. Kies in de vervolgkeuzelijst **Regio's** de optie **Alles selecteren**
    1. Schakel het selectievakje **Exporteren naar een event hub** in.
    1. Klik op **Selecteer een service bus-naamruimte** om het deelvenster **Een event hub selecteren** te openen.
    1. Selecteer het volgende in de vervolgkeuzemenu's van het deelvenster **Een event hub selecteren**: uw abonnement, uw Event Hub-naamruimte *AzureMonitoringData* en de naam van het standaard Event Hub-beleid.
    1. Klik op **OK**.
    1. Klik op **Opslaan** rechtsboven in het venster. Er wordt een event hub met de naam van de *insights-operational-logs* gemaakt.

### <a name="see-data-flowing-to-your-event-hubs"></a>U ziet hoe gegevens naar uw event hubs gaan

1. Wacht een paar minuten totdat de verbinding is gedefinieerd en het exporteren van het activiteitenlogboek naar de Event Hub is voltooid. Ga naar uw Event Hubs-naamruimte om de event hubs te zien die u hebt gemaakt.

    ![Gemaakte event hubs](media/ingest-data-no-code/event-hubs-created.png)

1. Zie hoe gegevens naar uw event hub gaan:

    ![Event hubs-gegevens](media/ingest-data-no-code/event-hubs-data.png)

## <a name="connect-event-hub-to-azure-data-explorer"></a>Event Hub verbinden met Azure Data Explorer

### <a name="diagnostic-logs-data-connection"></a>Gegevensverbinding voor diagnostische logboeken

1. In het Azure Data Explorer-cluster *kustodocs* selecteert u **Databases** in het menu links.
1. In het venster **Databases** selecteert u de databasenaam *AzureMonitoring*
1. Selecteer **Gegevensopname** in het menu links.
1. Klik in het venster **Gegevensopname** op **+ Gegevensverbinding toevoegen**
1. Voer in het venster **Gegevensverbinding** de volgende informatie in:

    ![Event Hub-verbinding](media/ingest-data-no-code/event-hub-data-connection.png)

    Gegevensbron:

    **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**
    |---|---|---|
    | Naam van gegevensverbinding | *DiagnosticsLogsConnection* | De naam van de verbinding die u wilt maken in Azure Data Explorer.|
    | Event hub-naamruimte | *AzureMonitoringData* | De naam die u eerder hebt gekozen om de naamruimte te identificeren. |
    | Event Hub | *diagnosticlogsdata* | De Event Hub die u hebt gemaakt. |
    | Consumentengroep | *adxpipeline* | De consumentengroep die u hebt gedefinieerd in de gemaakte Event Hub. |
    | | |

    Doeltabel:

    Er zijn twee opties voor de routering: *statische* en *dynamische*. Voor deze zelfstudie gebruikt u statische routering (standaardinstelling), waarbij u de tabelnaam, bestandsindeling en toewijzing opgeeft. Laat **Mijn gegevens omvatten routeringsgegevens** daarom uitgeschakeld.

     **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**
    |---|---|---|
    | Tabel | *DiagnosticLogsRecords* | De tabel die u hebt gemaakt in de database *AzureMonitoring*. |
    | Gegevensindeling | *JSON* | Opmaak in tabel. |
    | Toewijzen van kolommen | *DiagnosticLogsRecordsMapping* | De toewijzing die u hebt gemaakt in de database *AzureMonitoring* en waarmee die binnenkomende JSON-gegevens worden toegewezen aan de kolomnamen en gegevenstypen van *DiagnosticLogsRecords*.|
    | | |

1. Klik op **Maken**.  

### <a name="activity-logs-data-connection"></a>Gegevensverbinding voor activiteitenlogboeken

Herhaal de stappen in het gedeelte [Gegevensverbinding diagnostische logboeken](#diagnostic-logs-data-connection) om de gegevensverbinding voor activiteitenlogboeken te maken.

1. Voer in het venster **Gegevensverbinding** de volgende instellingen in:

    Gegevensbron:

    **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**
    |---|---|---|
    | Naam van gegevensverbinding | *ActivityLogsConnection* | De naam van de verbinding die u wilt maken in Azure Data Explorer.|
    | Event hub-naamruimte | *AzureMonitoringData* | De naam die u eerder hebt gekozen om de naamruimte te identificeren. |
    | Event Hub | *insights-operational-logs* | De Event Hub die u hebt gemaakt. |
    | Consumentengroep | *$Default* | De standaard consumentengroep. Zo nodig kunt u een andere consumentengroep maken. |
    | | |

    Doeltabel:

    Er zijn twee opties voor de routering: *statische* en *dynamische*. Voor deze zelfstudie gebruikt u statische routering (standaardinstelling), waarbij u de tabelnaam, bestandsindeling en toewijzing opgeeft. Laat **Mijn gegevens omvatten routeringsgegevens** daarom uitgeschakeld.

     **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**
    |---|---|---|
    | Tabel | *ActivityLogsRawRecords* | De tabel die u hebt gemaakt in de database *AzureMonitoring*. |
    | Gegevensindeling | *JSON* | Opmaak in tabel. |
    | Toewijzen van kolommen | *ActivityLogsRawRecordsMapping* | De toewijzing die u hebt gemaakt in de database *AzureMonitoring* en waarmee die binnenkomende JSON-gegevens worden toegewezen aan de kolomnamen en gegevenstypen van *ActivityLogsRawRecords*.|
    | | |

1. Klik op **Maken**.  

## <a name="query-the-new-tables"></a>Query uitvoeren op de nieuwe tabellen

U hebt een pijplijn met een gegevensstroom. Opname via het cluster duurt standaard 5 minuten. Laat de gegevensstroom daarom een paar minuten zijn gang gaan voordat u een query gaat uitvoeren.

### <a name="diagnostic-logs-table-query-example"></a>Voorbeeld van een query voor een tabel met diagnostische logboeken

De volgende query analyseert de queryduur van de gegevens uit records van diagnostisch logboeken in Azure Data Explorer:

```kusto
DiagnosticLogsRecords
| where Timestamp > ago(15m) and MetricName == 'QueryDuration'
| summarize avg(Average)
```

Queryresultaten:

|   |   |
| --- | --- |
|   |  avg_Average |
|   | 00:06.156 |
| | |

### <a name="activity-logs-table-query-example"></a>Voorbeeld van een query voor een tabel met activiteitenlogboeken

De volgende query analyseert gegevens uit records van activiteitenlogboeken in Azure Data Explorer:

```kusto
ActivityLogsRecords
| where OperationName == 'MICROSOFT.EVENTHUB/NAMESPACES/AUTHORIZATIONRULES/LISTKEYS/ACTION'
| where ResultType == 'Success'
| summarize avg(DurationMs)
```

Queryresultaten:
|   |   |
| --- | --- |
|   |  avg(DurationMs) |
|   | 768.333 |
| | |

## <a name="next-steps"></a>Volgende stappen

Leer met behulp van het volgende artikel veel meer query's te schrijven voor de gegevens die u hebt opgehaald uit Azure Data Explorer:

> [!div class="nextstepaction"]
> [Query's schrijven voor Azure Data Explorer](write-queries.md)
