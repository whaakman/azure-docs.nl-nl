---
title: 'Zelfstudie: Diagnostische gegevens en activiteitenlogboekgegevens in Azure Data Explorer opnemen zonder één regel code'
description: In deze zelfstudie leert u hoe u zonder één regel code gegevens kunt opnemen in Azure Data Explorer en query's op die gegevens kunt uitvoeren.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: tutorial
ms.date: 3/14/2019
ms.openlocfilehash: 422813c1ddb77aa11195d3021484744839c4e3bf
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57994341"
---
# <a name="tutorial-ingest-data-in-azure-data-explorer-without-one-line-of-code"></a>Zelfstudie: Gegevens opnemen in Azure Data Explorer zonder één regel code

In deze zelfstudie leert u hoe u zonder code te schrijven gegevens uit diagnostische logboeken en activiteitenlogboeken in een Azure Data Explorer-cluster kunt opnemen. Met deze eenvoudige methode voor gegevensopname kunt u snel beginnen met het uitvoeren van query's voor Azure Data Explorer om gegevens te analyseren.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak tabellen en toewijzing van opname in een Azure Data Explorer-database.
> * Maak de opgenomen gegevens op met behulp van updatebeleid.
> * Maak een [event hub](/azure/event-hubs/event-hubs-about) en koppel deze aan Azure Data Explorer.
> * Stream gegevens naar een event hub vanuit [diagnostische logboeken van Azure Monitor](/azure/azure-monitor/platform/diagnostic-logs-overview) en [activiteitenlogboeken van Azure Monitor](/azure/azure-monitor/platform/activity-logs-overview).
> * Vraag de opgenomen gegevens op met behulp van Azure Data Explorer.

> [!NOTE]
> Maak alle resources in dezelfde Azure-locatie of -regio. Dit is een vereiste voor diagnostische logboeken van Azure Monitor.

## <a name="prerequisites"></a>Vereisten

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis Azure-account](https://azure.microsoft.com/free/) aan voordat u begint.
* [Een Azure Data Explorer-cluster en -database](create-cluster-database-portal.md). In deze zelfstudie is *TestDatabase* de databasenaam.

## <a name="azure-monitor-data-provider-diagnostic-and-activity-logs"></a>Azure Monitor-gegevensprovider - diagnostische logboeken en activiteitenlogboeken

Bekijken en te begrijpen van de gegevens van de Azure Monitor diagnostische logboeken en het activiteit hieronder. We gaan een opnamepijplijn maken op basis van deze gegevensschema's. Houd er rekening mee dat elke gebeurtenis in een logboek een matrix van records heeft. Deze matrix van records gesplitst later in de zelfstudie.

### <a name="diagnostic-logs-example"></a>Voorbeeld van diagnostische logboeken

Diagnostische logboeken van Azure bevatten metrische gegevens afkomstig van een Azure-service die gegevens levert over de werking van die service. Gegevens worden samengevoegd met een tijdsinterval van 1 minuut. Hier volgt een voorbeeld van een metrisch gebeurtenisschema voor Azure Data Explorer, op basis van queryduur:

```json
{
    "records": [
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
    },
    {
        "count": 12,
        "total": 0,
        "minimum": 0,
        "maximum": 0,
        "average": 0,
        "resourceId": "/SUBSCRIPTIONS/F3101802-8C4F-4E6E-819C-A3B5794D33DD/RESOURCEGROUPS/KEDAMARI/PROVIDERS/MICROSOFT.KUSTO/CLUSTERS/KEREN",
        "time": "2018-12-21T17:00:00.0000000Z",
        "metricName": "QueryDuration",
        "timeGrain": "PT1M"
    }
    ]
}
```

### <a name="activity-logs-example"></a>Voorbeeld van activiteitenlogboeken

Azure-activiteitenlogboeken zijn abonnementsniveau logboeken die inzicht geven in de bewerkingen die worden uitgevoerd op resources in uw abonnement. Hier volgt een voorbeeld van een gebeurtenis in een activiteitenlogboek voor het controleren van toegang:

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

## <a name="set-up-an-ingestion-pipeline-in-azure-data-explorer"></a>Een opnamepijplijn instellen in Azure Data Explorer

Het instellen van een Azure Data Explorer-pijplijn omvat verschillende stappen, zoals [het maken van een tabel en gegevensopname](/azure/data-explorer/ingest-sample-data#ingest-data). U kunt de gegevens ook bewerken, toewijzen en bijwerken.

### <a name="connect-to-the-azure-data-explorer-web-ui"></a>Verbinding maken met de web-UI van Azure Data Explorer

Selecteer **Query** in uw *TestDatabase*-database van Azure Data Explorer om de web-UI van Azure Data Explorer te openen.

![Querypagina](media/ingest-data-no-code/query-database.png)

### <a name="create-the-target-tables"></a>De doeltabellen maken

De structuur van de logboeken van Azure Monitor is niet in tabelvorm. U de gegevens te manipuleren en elke gebeurtenis worden uitgebreid tot een of meer records. De onbewerkte gegevens die worden zal worden opgenomen in een tussenliggende tabel met de naam *ActivityLogsRawRecords* voor activiteitenlogboeken en *DiagnosticLogsRawRecords* voor diagnostische logboeken. Dat is het moment waarop de gegevens worden bewerkt en uitgebreid. Met behulp van een updatebeleid, de uitgebreide gegevens wordt vervolgens worden opgenomen in de *ActivityLogsRecords* tabel voor activiteitenlogboeken en *DiagnosticLogsRecords* voor diagnostische logboeken. Dit betekent dat u moet twee aparte tabellen voor het opnemen van activiteitenlogboeken en worden twee afzonderlijke tabellen voor het opnemen van diagnostische logboeken te maken.

Gebruik de web-UI van Azure Data Explorer om de doeltabellen te maken in de Azure Data Explorer-database.

#### <a name="the-diagnostic-logs-table"></a>De tabel met diagnostische logboeken

1. Maak in de *TestDatabase*-database een tabel met de naam *DiagnosticLogsRecords* om de diagnostische logboeken op te slaan. Gebruik de volgende `.create table`-besturingselementopdracht:

    ```kusto
    .create table DiagnosticLogsRecords (Timestamp:datetime, ResourceId:string, MetricName:string, Count:int, Total:double, Minimum:double, Maximum:double, Average:double, TimeGrain:string)
    ```

1. Selecteer **Uitvoeren** om de tabel te maken.

    ![Query uitvoeren](media/ingest-data-no-code/run-query.png)

1. De tabel van de tussentijdse gegevens met de naam *DiagnosticLogsRawRecords* in de *TestDatabase* database voor het manipuleren van gegevens met behulp van de volgende query uit. Selecteer **Uitvoeren** om de tabel te maken.

    ```kusto
    .create table DiagnosticLogsRawRecords (Records:dynamic)
    ```

#### <a name="the-activity-logs-tables"></a>De tabellen voor activiteitenlogboeken

1. Maak een tabel met de naam *ActivityLogsRecords* in de *TestDatabase*-database om records voor activiteitenlogboeken te ontvangen. Voer de volgende Azure Data Explorer-query uit om de tabel te maken:

    ```kusto
    .create table ActivityLogsRecords (Timestamp:datetime, ResourceId:string, OperationName:string, Category:string, ResultType:string, ResultSignature:string, DurationMs:int, IdentityAuthorization:dynamic, IdentityClaims:dynamic, Location:string, Level:string)
    ```

1. Maak de tussenliggende gegevenstabel met de naam *ActivityLogsRawRecords* in de *TestDatabase*-database voor het manipuleren van gegevens:

    ```kusto
    .create table ActivityLogsRawRecords (Records:dynamic)
    ```

<!--
     ```kusto
     .alter-merge table ActivityLogsRawRecords policy retention softdelete = 0d
    <[Retention](/azure/kusto/management/retention-policy) for an intermediate data table is set at zero retention policy.
-->

### <a name="create-table-mappings"></a>Tabeltoewijzingen maken

 Omdat de tabelindeling `json` is, is gegevenstoewijzing vereist. Met de `json`-toewijzingen wordt elk json-pad toegewezen aan de naam van een tabelkolom.

#### <a name="table-mapping-for-diagnostic-logs"></a>Tabeltoewijzing voor diagnostische logboeken

Gebruik de volgende query om de gegevens uit de diagnostische logboeken toe te wijzen aan de tabel:

```kusto
.create table DiagnosticLogsRawRecords ingestion json mapping 'DiagnosticLogsRawRecordsMapping' '[{"column":"Records","path":"$.records"}]'
```

#### <a name="table-mapping-for-activity-logs"></a>Tabeltoewijzing voor activiteitenlogboeken

Gebruik de volgende query om de gegevens uit de activiteitenlogboeken toe te wijzen aan de tabel:

```kusto
.create table ActivityLogsRawRecords ingestion json mapping 'ActivityLogsRawRecordsMapping' '[{"column":"Records","path":"$.records"}]'
```

### <a name="create-the-update-policy-for-log-data"></a>Het updatebeleid voor logboekgegevens maken

#### <a name="activity-log-data-update-policy"></a>Gegevens van een activiteitenlogboek beleid bijwerken

1. Maak een [functie](/azure/kusto/management/functions) die de verzameling van records in logboek registreren activiteit wordt uitgebreid, zodat elke waarde in de verzameling een afzonderlijke rij ontvangt. Gebruik de operator [`mvexpand`](/azure/kusto/query/mvexpandoperator):

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
            IdentityAuthorization = events.identity.authorization,
            IdentityClaims = events.identity.claims,
            Location = tostring(events["location"]),
            Level = tostring(events["level"])
    }
    ```

2. Voeg het [updatebeleid](/azure/kusto/concepts/updatepolicy) toe aan de doeltabel. Dit beleid wordt automatisch uitgevoerd op alle nieuw opgenomen gegevens in de tussenliggende gegevenstabel *ActivityLogsRawRecords* en de resultaten worden opgenomen in de tabel *ActivityLogsRecords*:

    ```kusto
    .alter table ActivityLogsRecords policy update @'[{"Source": "ActivityLogsRawRecords", "Query": "ActivityLogRecordsExpand()", "IsEnabled": "True"}]'
    ```

#### <a name="diagnostic-log-data-update-policy"></a>Diagnostische logboekgegevens beleid bijwerken

1. Maak een [functie](/azure/kusto/management/functions) die het verzamelen van diagnostische logboekrecords wordt uitgebreid, zodat elke waarde in de verzameling een afzonderlijke rij ontvangt. Gebruik de operator [`mvexpand`](/azure/kusto/query/mvexpandoperator):
     ```kusto
    .create function DiagnosticLogRecordsExpand() {
        DiagnosticLogsRawRecords
        | mvexpand events = Records
        | project
            Timestamp = todatetime(events["time"]),
            ResourceId = tostring(events["resourceId"]),
            MetricName = tostring(events["metricName"]),
            Count = toint(events["count"]),
            Total = todouble(events["total"]),
            Minimum = todouble(events["minimum"]),
            Maximum = todouble(events["maximum"]),
            Average = todouble(events["average"]),
            TimeGrain = tostring(events["timeGrain"])
    }
    ```

2. Voeg het [updatebeleid](/azure/kusto/concepts/updatepolicy) toe aan de doeltabel. Dit beleid wordt automatisch de query uitvoeren op alle nieuwe opgenomen gegevens in de *DiagnosticLogsRawRecords* tussenliggende ' gegevenstabel ' en nemen de resultaten in de *DiagnosticLogsRecords* tabel:

    ```kusto
    .alter table DiagnosticLogsRecords policy update @'[{"Source": "DiagnosticLogsRawRecords", "Query": "DiagnosticLogRecordsExpand()", "IsEnabled": "True"}]'
    ```

## <a name="create-an-azure-event-hubs-namespace"></a>Een Azure Event Hubs-naamruimte maken

Diagnostische logboeken in Azure maken het exporteren van metrische gegevens naar een opslagaccount of een event hub mogelijk. In deze zelfstudie sturen we de metrische gegevens door via een event hub. In de volgen stappen gaat u een Event Hubs-naamruimte en een event hub voor diagnostische logboeken maken. Azure Monitor maakt de event hub *insights-operational-logs* voor de activiteitenlogboeken.

1. Maak een event hub met behulp van een Azure Resource Manager-sjabloon in de Microsoft Azure-portal. Klik met de rechtermuisknop op **Implementeren in Azure** en selecteer **In nieuw venster openen** om de rest van de stappen in dit artikel te volgen. Klik op de knop **Implementeren in Azure** om de Azure-portal te openen.

    [![De knop Implementeren in Azure](media/ingest-data-no-code/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

1. Maak een Event Hubs-naamruimte en een event hub voor de diagnostische logboeken.

    ![Een event hub maken](media/ingest-data-no-code/event-hub.png)

1. Vul het formulier in met de volgende gegevens. Gebruik de standaardwaarden voor alle instellingen die niet worden vermeld in de volgende tabel.

    **Instelling** | **Voorgestelde waarde** | **Beschrijving**
    |---|---|---|
    | **Abonnement** | *Uw abonnement* | Selecteer het Azure-abonnement dat u wilt gebruiken voor de Event Hub.|
    | **Resourcegroep** | *test-resource-group* | Maak een nieuwe resourcegroep. |
    | **Locatie** | Selecteer de regio die het beste voldoet aan uw behoeften. | Maak de Event Hubs-naamruimte op dezelfde locatie als andere resources.
    | **Naam van naamruimte** | *AzureMonitoringData* | Kies een unieke naam waarmee de naamruimte kan worden geïdentificeerd.
    | **Event hub-naam** | *DiagnosticLogsData* | De Event Hub bevindt zich onder de naamruimte, wat een unieke bereikcontainer biedt. |
    | **Naam van consumentengroep** | *adxpipeline* | Maak een naam voor een consumentengroep. Met consumentengroepen kunnen meerdere gebruikstoepassingen elk een afzonderlijke weergave van de gebeurtenisstroom hebben. |
    | | |

## <a name="connect-azure-monitor-logs-to-your-event-hub"></a>Azure Monitor-logboeken verbinden met uw event hub

U moet nu uw diagnostische logboeken en uw activiteitenlogboeken verbinden met de event hub.

### <a name="connect-diagnostic-logs-to-your-event-hub"></a>Diagnostische logboeken verbinden met uw event hub

Selecteer een resource van waaruit u metrische gegevens wilt exporteren. Verschillende resourcetypen ondersteunen het exporteren van diagnostische logboeken, waaronder de Event Hub-naamruimte, Azure Key Vault, Azure IoT Hub en Azure Data Explorer-clusters. In deze zelfstudie gebruiken we een Azure Data Explorer-cluster als resource.

1. Selecteer uw Kusto-cluster in de Azure-portal.
1. Selecteer **Diagnostische instellingen** en selecteer vervolgens de link **Diagnostische gegevens inschakelen**. 

    ![Diagnostische instellingen](media/ingest-data-no-code/diagnostic-settings.png)

1. Het deelvenster **Diagnostische instellingen** wordt geopend. Voer de volgende stappen uit:
   1. Geef de gegevens uit uw diagnostische logboeken de naam *ADXExportedData*.
   1. Selecteer onder **METRIC** het selectievakje **AllMetrics** (optioneel).
   1. Schakel het selectievakje **Streamen naar een event hub** in.
   1. Selecteer **Configureren**.

      ![Het deelvenster Diagnostische instellingen](media/ingest-data-no-code/diagnostic-settings-window.png)

1. Configureer in het deelvenster **Een event hub selecteren** hoe gegevens uit diagnostisch logboeken moeten worden geëxporteerd naar de event hub die u hebt gemaakt:
    1. Selecteer in de lijst **Naamruimte van de Event Hub selecteren** de optie *AzureMonitoringData*.
    1. Selecteer in de lijst **Naam van de Event Hub selecteren** de optie *diagnosticlogsdata*.
    1. Selecteer in de lijst **Naam van het Event Hub-beleid selecteren** de optie **RootManagerSharedAccessKey**.
    1. Selecteer **OK**.

1. Selecteer **Opslaan**.

### <a name="connect-activity-logs-to-your-event-hub"></a>Activiteitenlogboeken verbinden met uw event hub

1. Selecteer **Activiteitenlogboek** in het linkermenu van de Microsoft Azure-portal.
1. Het venster **Activiteitenlogboek** wordt geopend. Selecteer **Exporteren naar Event Hub**.

    ![Het venster Activiteitenlogboek](media/ingest-data-no-code/activity-log.png)

1. Het venster **Activiteitenlogboek exporteren** wordt geopend:
 
    ![Het venster Activiteitenlogboek exporteren](media/ingest-data-no-code/export-activity-log.png)

1. Voer de volgende stappen uit in het venster **Activiteitenlogboek exporteren**:
      1. Selecteer uw abonnement.
      1. Kies in de lijst **Regio's** de optie **Alles selecteren**.
      1. Selecteer het selectievakje **Exporteren naar een Event Hub**.
      1. Kies **Selecteer een service bus-naamruimte** om het deelvenster **Een event hub selecteren** te openen.
      1. Selecteer uw abonnement in het deelvenster **Event Hub selecteren**.
      1. Selecteer in de lijst **Naamruimte van de Event Hub selecteren** de optie *AzureMonitoringData*.
      1. Selecteer in de lijst **Naam van het Event Hub-beleid selecteren** de naam van het standaardbeleid voor de event hub.
      1. Selecteer **OK**.
      1. Selecteer **Opslaan** in de linkerbovenhoek van het venster.
   Er wordt een event hub met de naam van de *insights-operational-logs* gemaakt.

### <a name="see-data-flowing-to-your-event-hubs"></a>Zie hoe gegevens naar uw event hubs gaan

1. Wacht een paar minuten totdat de verbinding is gedefinieerd en het exporteren van het activiteitenlogboek naar de event hub is voltooid. Ga naar uw Event Hubs-naamruimte om de event hubs te zien die u hebt gemaakt.

    ![Gemaakte event hubs](media/ingest-data-no-code/event-hubs-created.png)

1. Zie hoe gegevens naar uw event hub gaan:

    ![Gegevens van de event hub](media/ingest-data-no-code/event-hubs-data.png)

## <a name="connect-an-event-hub-to-azure-data-explorer"></a>Een event hub verbinden met Azure Data Explorer

Nu moet u de gegevensverbindingen voor uw diagnostische logboeken en activiteitenlogboeken maken.

### <a name="create-the-data-connection-for-diagnostic-logs"></a>De gegevensverbinding maken voor diagnostische logboeken

1. In het Azure Data Explorer-cluster met de naam *kustodocs* selecteert u **Databases** in het menu links.
1. In het venster **Databases** selecteert u uw *TestDatabase*-database.
1. Selecteer **Gegevensopname** in het menu links.
1. Klik in het venster **Gegevensopname** op **+ Gegevensverbinding toevoegen**.
1. Voer in het venster **Gegevensverbinding** de volgende informatie in:

    ![Event hub-gegevensverbinding](media/ingest-data-no-code/event-hub-data-connection.png)

    Gegevensbron:

    **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**
    |---|---|---|
    | **Naam van gegevensverbinding** | *DiagnosticsLogsConnection* | De naam van de verbinding die u wilt maken in Azure Data Explorer.|
    | **Event hub-naamruimte** | *AzureMonitoringData* | De naam die u eerder hebt gekozen om de naamruimte te identificeren. |
    | **Event hub** | *diagnosticlogsdata* | De Event Hub die u hebt gemaakt. |
    | **Consumentengroep** | *adxpipeline* | De consumentengroep die u hebt gedefinieerd in de gemaakte Event Hub. |
    | | |

    Doeltabel:

    Er zijn twee opties voor de routering: *statische* en *dynamische*. Voor deze zelfstudie gebruikt u statische routering (standaardinstelling), waarbij u de tabelnaam, gegevensindeling en toewijzing opgeeft. Laat **Mijn gegevens omvatten routeringsgegevens** uitgeschakeld.

     **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**
    |---|---|---|
    | **Tabel** | *DiagnosticLogsRawRecords* | De tabel die u hebt gemaakt in de *TestDatabase*-database. |
    | **Gegevensindeling** | *JSON* | De indeling die in de tabel wordt gebruikt. |
    | **Toewijzen van kolommen** | *DiagnosticLogsRecordsMapping* | De toewijzing die u hebt gemaakt in de *TestDatabase*-database, waarmee binnenkomende JSON-gegevens worden toegewezen aan de kolomnamen en gegevenstypen van de tabel *DiagnosticLogsRecords*.|
    | | |

1. Selecteer **Maken**.  

### <a name="create-the-data-connection-for-activity-logs"></a>De gegevensverbinding maken voor activiteitenlogboeken

Herhaal de stappen in de sectie De gegevensverbinding maken voor diagnostische logboeken om de gegevensverbinding voor uw activiteitenlogboeken te maken.

1. Gebruik de volgende instellingen in het venster **Gegevensverbinding**:

    Gegevensbron:

    **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**
    |---|---|---|
    | **Naam van gegevensverbinding** | *ActivityLogsConnection* | De naam van de verbinding die u wilt maken in Azure Data Explorer.|
    | **Event hub-naamruimte** | *AzureMonitoringData* | De naam die u eerder hebt gekozen om de naamruimte te identificeren. |
    | **Event hub** | *insights-operational-logs* | De Event Hub die u hebt gemaakt. |
    | **Consumentengroep** | *$Default* | De standaard consumentengroep. Zo nodig kunt u een andere consumentengroep maken. |
    | | |

    Doeltabel:

    Er zijn twee opties voor de routering: *statische* en *dynamische*. Voor deze zelfstudie gebruikt u statische routering (standaardinstelling), waarbij u de tabelnaam, gegevensindeling en toewijzing opgeeft. Laat **Mijn gegevens omvatten routeringsgegevens** uitgeschakeld.

     **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**
    |---|---|---|
    | **Tabel** | *ActivityLogsRawRecords* | De tabel die u hebt gemaakt in de *TestDatabase*-database. |
    | **Gegevensindeling** | *JSON* | De indeling die in de tabel wordt gebruikt. |
    | **Toewijzen van kolommen** | *ActivityLogsRawRecordsMapping* | De toewijzing die u hebt gemaakt in de *TestDatabase*-database, waarmee binnenkomende JSON-gegevens worden toegewezen aan de kolomnamen en gegevenstypen van de tabel *ActivityLogsRawRecords*.|
    | | |

1. Selecteer **Maken**.  

## <a name="query-the-new-tables"></a>Query uitvoeren op de nieuwe tabellen

U hebt nu een pijplijn met een gegevensstroom. Opname via het cluster duurt standaard 5 minuten. Laat de gegevensstroom daarom een paar minuten zijn gang gaan voordat u een query gaat uitvoeren.

### <a name="an-example-of-querying-the-diagnostic-logs-table"></a>Een voorbeeld van het uitvoeren van query's in de tabel met diagnostische logboeken

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

### <a name="an-example-of-querying-the-activity-logs-table"></a>Een voorbeeld van het uitvoeren van query's in de tabel met activiteitenlogboeken

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
