---
title: Metrische gegevens van Azure Storage in Azure Monitor | Microsoft Docs
description: Meer informatie over de nieuwe metrische gegevens die wordt aangeboden via Azure Monitor.
services: storage
author: fhryo-msft
ms.service: storage
ms.topic: article
ms.date: 09/05/2017
ms.author: fryu
ms.subservice: common
ms.openlocfilehash: 4be52fbc6d9fb01ac3cd3c0954042c35b45bbf23
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58884360"
---
# <a name="azure-storage-metrics-in-azure-monitor"></a>Metrische gegevens van Azure Storage in Azure Monitor

Met metrische gegevens over Azure Storage, kunt u trends in gebruik, trace-aanvragen, analyseren en problemen met uw storage-account.

Azure Monitor biedt een uniforme gebruikersinterfaces voor het bewaken van alle andere Azure-services. Zie voor meer informatie, [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview.md). Azure Storage kan Azure Monitor door metrische gegevens te verzenden naar het platform van Azure Monitor worden geïntegreerd.

## <a name="access-metrics"></a>Toegang tot metrische gegevens

Azure Monitor biedt meerdere manieren voor toegang tot metrische gegevens. U kunt krijgen van de [Azure-portal](https://portal.azure.com), de Azure Monitor API's (REST en .NET) en analyse-oplossingen, zoals Event Hubs. Zie voor meer informatie, [Azure Monitor Metrics](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Metrische gegevens zijn standaard ingeschakeld en u hebt toegang tot gegevens van de afgelopen 93 dagen. Als u behouden van gegevens voor een langere periode wilt, kunt u metrische gegevens om een Azure Storage-account te archiveren. Dit is geconfigureerd in [diagnostische instellingen](../../azure-monitor/platform/diagnostic-logs-overview.md) in Azure Monitor.

### <a name="access-metrics-in-the-azure-portal"></a>Toegang tot metrische gegevens in Azure portal

U kunt metrische gegevens controleren na verloop van tijd in de Azure-portal. Het volgende voorbeeld laat zien hoe om weer te geven **transacties** op accountniveau.

![Schermafbeelding van de toegang tot metrische gegevens in Azure portal](./media/storage-metrics-in-azure-monitor/access-metrics-in-portal.png)

Voor metrische gegevens voor ondersteuning van dimensies, kunt u de metrische waarde met de waarde van de gewenste dimensie filteren. Het volgende voorbeeld laat zien hoe om weer te geven **transacties** op accountniveau op een bepaalde bewerking door het selecteren van de waarden voor **API-naam** dimensie.

![Schermafbeelding van de toegang tot metrische gegevens met de dimensie in Azure portal](./media/storage-metrics-in-azure-monitor/access-metrics-in-portal-with-dimension.png)

### <a name="access-metrics-with-the-rest-api"></a>Toegang tot metrische gegevens met de REST-API

Azure Monitor biedt [REST-API's](/rest/api/monitor/) metrische definitie en waarden worden gelezen. Deze sectie leest u over het lezen van de metrische gegevens van storage. Resource-ID wordt gebruikt in alle REST-API's. Lees de informatie over resource-ID voor services in de opslag voor meer informatie.

Het volgende voorbeeld ziet u hoe u [ArmClient](https://github.com/projectkudu/ARMClient) op de opdrachtregel voor het vereenvoudigen van testen met de REST-API.

#### <a name="list-account-level-metric-definition-with-the-rest-api"></a>Lijst met account niveau metrische definitie met de REST-API

Het volgende voorbeeld laat zien hoe om metrische definitie op accountniveau weer te geven:

```
# Login to Azure and enter your credentials when prompted.
> armclient login

> armclient GET /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metricdefinitions?api-version=2018-01-01

```

Als u weergeven van de metrische definities voor de blob, table, bestand of wachtrij wilt, moet u andere resource-id's voor elke service opgeven met de API.

Het antwoord bevat de definitie van de metrische gegevens in JSON-indeling:

```Json
{
  "value": [
    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metricdefinitions/UsedCapacity",
      "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}",
      "category": "Capacity",
      "name": {
        "value": "UsedCapacity",
        "localizedValue": "Used capacity"
      },
      "isDimensionRequired": false,
      "unit": "Bytes",
      "primaryAggregationType": "Average",
      "metricAvailabilities": [
        {
          "timeGrain": "PT1M",
          "retention": "P30D"
        },
        {
          "timeGrain": "PT1H",
          "retention": "P30D"
        }
      ]
    },
    ... next metric definition
  ]
}

```

#### <a name="read-account-level-metric-values-with-the-rest-api"></a>Gelezen metrische waarden van de account op serverniveau met de REST-API

Het volgende voorbeeld laat zien hoe om metrische gegevens op accountniveau te lezen:

```
> armclient GET "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metrics?metricnames=Availability&api-version=2018-01-01&aggregation=Average&interval=PT1H"

```

In bovenstaande voorbeeld moet als u wilt lezen metrische waarden voor de blob, table, bestand of wachtrij, u andere resource-id's voor elke service met de API.

Het volgende antwoord bevat metrische waarden in de JSON-indeling:

```Json
{
  "cost": 0,
  "timespan": "2017-09-07T17:27:41Z/2017-09-07T18:27:41Z",
  "interval": "PT1H",
  "value": [
    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/Microsoft.Insights/metrics/Availability",
      "type": "Microsoft.Insights/metrics",
      "name": {
        "value": "Availability",
        "localizedValue": "Availability"
      },
      "unit": "Percent",
      "timeseries": [
        {
          "metadatavalues": [],
          "data": [
            {
              "timeStamp": "2017-09-07T17:27:00Z",
              "average": 100.0
            }
          ]
        }
      ]
    }
  ]
}

```

### <a name="access-metrics-with-the-net-sdk"></a>Toegang tot metrische gegevens met de .NET SDK

Azure Monitor biedt [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/) metrische definitie en waarden worden gelezen. De [voorbeeldcode](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/) ziet u hoe u de SDK en de verschillende parameters. U moet gebruiken `0.18.0-preview` of hoger voor metrische gegevens van storage. Resource-ID wordt gebruikt in .NET-SDK. Lees de informatie over resource-ID voor services in de opslag voor meer informatie.

Het volgende voorbeeld laat zien hoe Azure Monitor .NET SDK gebruiken om te lezen van metrische opslaggegevens.

#### <a name="list-account-level-metric-definition-with-the-net-sdk"></a>Lijst met account niveau metrische definitie met de .NET SDK

Het volgende voorbeeld laat zien hoe om metrische definitie op accountniveau weer te geven:

```csharp
    public static async Task ListStorageMetricDefinition()
    {
        // Resource ID for storage account
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}";
        var subscriptionId = "{SubscriptionID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "{TenantID}";
        var applicationId = "{ApplicationID}";
        var accessKey = "{AccessKey}";

        // Using metrics in Azure Monitor is currently free. However, if you use additional solutions ingesting metrics data, you may be billed by these solutions. For example, you are billed by Azure Storage if you archive metrics data to an Azure Storage account. Or you are billed by Operation Management Suite (OMS) if you stream metrics data to OMS for advanced analysis.
        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;
        IEnumerable<MetricDefinition> metricDefinitions = await readOnlyClient.MetricDefinitions.ListAsync(resourceUri: resourceId, cancellationToken: new CancellationToken());

        foreach (var metricDefinition in metricDefinitions)
        {
            //Enumrate metric definition:
            //    Id
            //    ResourceId
            //    Name
            //    Unit
            //    MetricAvailabilities
            //    PrimaryAggregationType
            //    Dimensions
            //    IsDimensionRequired
        }
    }

```

Als u weergeven van de metrische definities voor de blob, table, bestand of wachtrij wilt, moet u andere resource-id's voor elke service opgeven met de API.

#### <a name="read-metric-values-with-the-net-sdk"></a>Gelezen metrische waarden met de .NET SDK

Het volgende voorbeeld ziet u hoe `UsedCapacity` gegevens op het accountniveau van:

```csharp
    public static async Task ReadStorageMetricValue()
    {
        // Resource ID for storage account
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}";
        var subscriptionId = "{SubscriptionID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "{TenantID}";
        var applicationId = "{ApplicationID}";
        var accessKey = "{AccessKey}";

        MonitorClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;

        Response = await readOnlyClient.Metrics.ListAsync(
            resourceUri: resourceId,
            timespan: timeSpan,
            interval: System.TimeSpan.FromHours(1),
            metricnames: "UsedCapacity",

            aggregation: "Average",
            resultType: ResultType.Data,
            cancellationToken: CancellationToken.None);

        foreach (var metric in Response.Value)
        {
            //Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

In bovenstaande voorbeeld moet als u wilt lezen metrische waarden voor de blob, table, bestand of wachtrij, u andere resource-id's voor elke service met de API.

#### <a name="read-multi-dimensional-metric-values-with-the-net-sdk"></a>Multi-dimensionale metrische waarden met de .NET SDK

Voor multidimensionale metrische gegevens moet u metagegevens gegevens om filter te definiëren als u wilt lezen, metrische gegevens van specifieke dimensie-waarde.

Het volgende voorbeeld laat zien hoe om te lezen, metrische gegevens op de metriek multidimensionale ondersteuning:

```csharp
    public static async Task ReadStorageMetricValueTest()
    {
        // Resource ID for blob storage
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/blobServices/default";
        var subscriptionId = "{SubscriptionID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "{TenantID}";
        var applicationId = "{ApplicationID}";
        var accessKey = "{AccessKey}";

        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;
        // It's applicable to define meta data filter when a metric support dimension
        // More conditions can be added with the 'or' and 'and' operators, example: BlobType eq 'BlockBlob' or BlobType eq 'PageBlob'
        ODataQuery<MetadataValue> odataFilterMetrics = new ODataQuery<MetadataValue>(
            string.Format("BlobType eq '{0}'", "BlockBlob"));

        Response = readOnlyClient.Metrics.List(
                        resourceUri: resourceId,
                        timespan: timeSpan,
                        interval: System.TimeSpan.FromHours(1),
                        metricnames: "BlobCapacity",
                        odataQuery: odataFilterMetrics,
                        aggregation: "Average",
                        resultType: ResultType.Data);

        foreach (var metric in Response.Value)
        {
            //Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

## <a name="understanding-resource-id-for-services-in-azure-storage"></a>Wat is resource-ID voor services in Azure Storage?

Resource-ID is een unieke id van een resource in Azure. Wanneer u de Azure Monitor REST API gebruikt definities voor metrische gegevens of waarden wilt lezen, moet u de resource-ID gebruiken voor de resource waarop u van plan bent om te werken. De resource-ID sjabloon met de volgende notatie:

```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

Storage beschikt over metrische gegevens in zowel het niveau van de storage-account en het serviceniveau met Azure Monitor. Bijvoorbeeld, kunt u metrische gegevens voor alleen Blob-opslag ophalen. Elk niveau heeft een eigen resource-ID, die wordt gebruikt om op te halen van de metrische gegevens voor alleen dat niveau.

### <a name="resource-id-for-a-storage-account"></a>Resource-ID voor een opslagaccount

Hieronder ziet u de indeling voor het opgeven van de Resource-ID voor een opslagaccount.

```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}
```

### <a name="resource-id-for-the-storage-services"></a>Resource-ID voor de storage-services

Hieronder ziet u de indeling voor het opgeven van de Resource-ID voor elk van de storage-services.

* BLOB-service resource-ID
```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/blobServices/default
```
* Tabel-service resource-ID
```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/tableServices/default
```
* Queue-service resource-ID
```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/queueServices/default
```
* Resource-ID File-service
```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/fileServices/default
```

### <a name="resource-id-in-azure-monitor-rest-api"></a>Resource-ID in Azure Monitor REST-API

Hieronder ziet u het patroon dat wordt gebruikt bij het aanroepen van de Azure Monitor REST API.

```
GET {resourceId}/providers/microsoft.insights/metrics?{parameters}
```

## <a name="capacity-metrics"></a>Capaciteit metrische gegevens
Capaciteit metrische waarden worden verzonden naar Azure Monitor om het uur. De waarden worden dagelijks vernieuwd. Het tijdsinterval definieert het tijdsinterval waarvoor metrische waarden worden weergegeven. Het ondersteunde tijdsinterval voor alle metrische gegevens over capaciteit is een uur (PT1H).

Azure Storage biedt de volgende metrische gegevens over capaciteit in Azure Monitor.

### <a name="account-level"></a>Accountniveau

| Naam van meetwaarde | Beschrijving |
| ------------------- | ----------------- |
| UsedCapacity | De hoeveelheid opslag die wordt gebruikt door de storage-account. Voor standard storage-accounts is het de som van de capaciteit die wordt gebruikt door de blob-, tabel-, bestands- en wachtrij. Voor premium-opslagaccounts en Blob storage-accounts is het hetzelfde als BlobCapacity. <br/><br/> Eenheid: Bytes <br/> Aggregatietype: Gemiddeld <br/> Voorbeeld van een waarde: 1024 |

### <a name="blob-storage"></a>Blob Storage

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
| BlobCapacity | Het totaal van Blob-opslag die wordt gebruikt in de storage-account. <br/><br/> Eenheid: Bytes <br/> Aggregatietype: Gemiddeld <br/> Voorbeeld van een waarde: 1024 <br/> Dimensie: BlobType ([definitie](#metrics-dimensions)) |
| BlobCount    | Het aantal blob-objecten die zijn opgeslagen in de storage-account. <br/><br/> Eenheid: Count <br/> Aggregatietype: Gemiddeld <br/> Voorbeeld van een waarde: 1024 <br/> Dimensie: BlobType ([definitie](#metrics-dimensions)) |
| ContainerCount    | Het aantal containers in het opslagaccount. <br/><br/> Eenheid: Count <br/> Aggregatietype: Gemiddeld <br/> Voorbeeld van een waarde: 1024 |
| IndexCapacity     | De hoeveelheid opslag die wordt gebruikt door ADLS Gen2 hiërarchische Index <br/><br/> Eenheid: Bytes <br/> Aggregatietype: Gemiddeld <br/> Voorbeeld van een waarde: 1024 |

### <a name="table-storage"></a>Table Storage

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
| TableCapacity | De hoeveelheid van Table storage die worden gebruikt door de storage-account. <br/><br/> Eenheid: Bytes <br/> Aggregatietype: Gemiddeld <br/> Voorbeeld van een waarde: 1024 |
| TableCount   | Het aantal tabellen in de storage-account. <br/><br/> Eenheid: Count <br/> Aggregatietype: Gemiddeld <br/> Voorbeeld van een waarde: 1024 |
| TableEntityCount | Het aantal tabelentiteiten in de storage-account. <br/><br/> Eenheid: Count <br/> Aggregatietype: Gemiddeld <br/> Voorbeeld van een waarde: 1024 |

### <a name="queue-storage"></a>Queue Storage

| Naam van meetwaarde | Beschrijving |
| ------------------- | ----------------- |
| QueueCapacity | De hoeveelheid van Queue storage die worden gebruikt door de storage-account. <br/><br/> Eenheid: Bytes <br/> Aggregatietype: Gemiddeld <br/> Voorbeeld van een waarde: 1024 |
| QueueCount   | Het aantal wachtrijen in de storage-account. <br/><br/> Eenheid: Count <br/> Aggregatietype: Gemiddeld <br/> Voorbeeld van een waarde: 1024 |
| QueueMessageCount | Het aantal niet-verlopen Wachtrijberichten in de storage-account. <br/><br/>Eenheid: Count <br/> Aggregatietype: Gemiddeld <br/> Voorbeeld van een waarde: 1024 |

### <a name="file-storage"></a>File Storage

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
| FileCapacity | De hoeveelheid opslag van bestanden die worden gebruikt door de storage-account. <br/><br/> Eenheid: Bytes <br/> Aggregatietype: Gemiddeld <br/> Voorbeeld van een waarde: 1024 |
| FileCount   | Het aantal bestanden in de storage-account. <br/><br/> Eenheid: Count <br/> Aggregatietype: Gemiddeld <br/> Voorbeeld van een waarde: 1024 |
| FileShareCount | Het aantal bestandsshares in de storage-account. <br/><br/> Eenheid: Count <br/> Aggregatietype: Gemiddeld <br/> Voorbeeld van een waarde: 1024 |

## <a name="transaction-metrics"></a>Metrische gegevens de transactie

Metrische gegevens die transactie zijn bij elke aanvraag verzonden naar een opslagaccount van Azure-opslag naar Azure Monitor. In het geval van geen activiteit in uw storage-account worden er geen gegevens over transactie metrische gegevens in de periode. Alle metrische gegevens voor de transactie zijn beschikbaar op zowel account- en serviceniveau (Blob storage, Table storage, Azure Files en Queue storage). Het tijdsinterval definieert het tijdsinterval dat metrische waarden worden weergegeven. De ondersteunde tijd korrels voor alle metrische gegevens voor de transactie zijn PT1H en PT1M.

Azure Storage biedt de volgende metrische gegevens van de transactie uit in Azure Monitor.

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
| Transacties | Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen, evenals aanvragen waarbij fouten zijn opgetreden. <br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Van toepassing afmetingen: De ResponseType, GeoType ApiName en -verificatie ([definitie](#metrics-dimensions))<br/> Voorbeeld van een waarde: 1024 |
| Inkomend verkeer | De hoeveelheid inkomende gegevens. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure. <br/><br/> Eenheid: Bytes <br/> Aggregatietype: Totaal <br/> Van toepassing afmetingen: GeoType, ApiName en -verificatie ([definitie](#metrics-dimensions)) <br/> Voorbeeld van een waarde: 1024 |
| Uitgaand verkeer | De hoeveelheid uitgaande gegevens. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer. <br/><br/> Eenheid: Bytes <br/> Aggregatietype: Totaal <br/> Van toepassing afmetingen: GeoType, ApiName en -verificatie ([definitie](#metrics-dimensions)) <br/> Voorbeeld van een waarde: 1024 |
| SuccessServerLatency | De gemiddelde tijd die nodig is om een aanvraag door Azure Storage te verwerken. Deze waarde bevat niet de netwerklatentie die is opgegeven in SuccessE2ELatency. <br/><br/> Eenheid: Milliseconden <br/> Aggregatietype: Gemiddeld <br/> Van toepassing afmetingen: GeoType, ApiName en -verificatie ([definitie](#metrics-dimensions)) <br/> Voorbeeld van een waarde: 1024 |
| SuccessE2ELatency | De gemiddelde end-to-end-latentie van geslaagde aanvragen aan een opslagservice of de opgegeven API-bewerking. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen. <br/><br/> Eenheid: Milliseconden <br/> Aggregatietype: Gemiddeld <br/> Van toepassing afmetingen: GeoType, ApiName en -verificatie ([definitie](#metrics-dimensions)) <br/> Voorbeeld van een waarde: 1024 |
| Beschikbaarheid | Het percentage van de beschikbaarheid van de opslagservice of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door te nemen van de waarde van het totale aantal factureerbare aanvragen en delen door het aantal toepasselijke aanvragen, met inbegrip van aanvragen die onverwachte fouten zijn opgetreden. Alle onverwachte fouten leiden tot een afgenomen beschikbaarheid voor de opslagservice of de opgegeven API-bewerking. <br/><br/> Eenheid: Procent <br/> Aggregatietype: Gemiddeld <br/> Van toepassing afmetingen: GeoType, ApiName en -verificatie ([definitie](#metrics-dimensions)) <br/> Voorbeeld van een waarde: 99.99 |

## <a name="metrics-dimensions"></a>Metrische gegevens over dimensies

Azure Storage ondersteunt de volgende dimensies voor metrische gegevens in Azure Monitor.

| Dimensienaam | Description |
| ------------------- | ----------------- |
| BlobType | Het type blob voor alleen Blob-metrische gegevens. De ondersteunde waarden zijn **BlockBlob** en **PageBlob**. Toevoeg-Blob is opgenomen in BlockBlob. |
| ResponseType | Transactietype antwoord. De beschikbare waarden zijn onder meer: <br/><br/> <li>ServerOtherError: alle overige serverfouten, behalve diegene die zijn beschreven </li> <li> ServerBusyError: geverifieerde aanvraag waardoor een HTTP 503-statuscode is geretourneerd. </li> <li> ServerTimeoutError: geverifieerde aanvraag met time-out waardoor een HTTP 500-statuscode is geretourneerd. De time-out is opgetreden vanwege een serverfout. </li> <li> AuthorizationError: geverifieerde aanvraag die is mislukt vanwege niet-geautoriseerde toegang tot gegevens of een autorisatiefout. </li> <li> NetworkError: geverifieerde aanvraag die is mislukt vanwege netwerkfouten. Treedt meestal op als dooreen client voortijdig een verbinding wordt verbroken voordat de time-out voorbij is. </li> <li>    ClientThrottlingError: Beperkingsfout aan de client-zijde. </li> <li> ClientTimeoutError: geverifieerde aanvraag met time-out waardoor een HTTP 500-statuscode is geretourneerd. Als de time-out van het clientnetwerk of van de aanvraag is ingesteld op een lagere waarde dan door de opslagservice wordt verwacht, is er sprake van een verwachte time-out. Anders wordt deze als ServerTimeoutError gerapporteerd. </li> <li> ClientOtherError: alle overige fouten aan de clientzijde, behalve diegene die zijn beschreven. </li> <li> Geslaagd: succesvolle aanvraag|
| GeoType | De transactie van primaire of secundaire cluster. De beschikbare waarden zijn onder andere primaire en secundaire database. Dit geldt voor Read Access-Geo Redundant Storage(RA-GRS) bij het lezen van objecten van secundaire tenant. |
| ApiName | De naam van bewerking. Bijvoorbeeld: <br/> <li>CreateContainer</li> <li>DeleteBlob</li> <li>GetBlob</li> Zie voor alle namen van de bewerking, [document](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages). |
| Authentication | Verificatietype dat wordt gebruikt in transacties. De beschikbare waarden zijn onder meer: <br/> <li>AccountKey: De transactie is geverifieerd met de sleutel van het opslagaccount.</li> <li>SAS: De transactie wordt geverifieerd met handtekeningen voor gedeelde toegang.</li> <li>OAuth: De transactie wordt geverifieerd met OAuth-toegangstokens.</li> <li>Anoniem: De transactie is anoniem aangevraagd. Deze bevat geen voorbereidende aanvragen.</li> <li>AnonymousPreflight: De transactie is voorbereidende aanvraag.</li> |

Voor de metrische gegevens over ondersteunende dimensies moet u de dimensiewaarde om te zien van de bijbehorende metrische waarden op te geven. Bijvoorbeeld, als u bekijkt **transacties** waarde voor gelukt-antwoorden, moet u voor het filteren van de **ResponseType** dimensie met **succes**. Of als u bekijkt **BlobCount** waarde voor blok-Blob, moet u voor het filteren van de **BlobType** dimensie met **BlockBlob**.

## <a name="service-continuity-of-legacy-metrics"></a>Continuïteit van de service van verouderde metrische gegevens

Verouderde metrische gegevens zijn beschikbaar in combinatie met Azure Monitor beheerd metrische gegevens. De ondersteuning blijft hetzelfde tot Azure Storage is beëindigd door de service op verouderde metrische gegevens.

## <a name="faq"></a>Veelgestelde vragen

**Nieuwe metrische gegevens biedt ondersteuning voor klassieke opslagaccount?**

Nee, nieuwe metrische gegevens in Azure Monitor alleen ondersteuning voor Azure resourcemanager-opslagaccounts. Als u wilt de metrische gegevens op Storage-accounts gebruiken, moet u voor het migreren naar Azure Resource Manager-opslagaccount. Zie [migreren naar Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview).

**Biedt Azure Storage ondersteuning voor de metrische gegevens voor Managed Disks of niet-beheerde schijven?**

Nee, Azure Compute biedt ondersteuning voor de metrische gegevens op schijven. Zie [artikel](https://azure.microsoft.com/blog/per-disk-metrics-managed-disks/) voor meer informatie.

**Over het toewijzen en migreren van klassieke metrische gegevens met nieuwe metrische gegevens?**

U vindt gedetailleerde toewijzing tussen klassieke metrische gegevens en nieuwe metrische gegevens in [Azure Storage metrics migratie](./storage-metrics-migration.md).

## <a name="next-steps"></a>Volgende stappen

* [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview.md)
