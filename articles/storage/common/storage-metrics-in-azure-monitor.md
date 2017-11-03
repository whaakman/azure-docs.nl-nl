---
title: Metrische gegevens van Azure Storage in Azure Monitor | Microsoft Docs
description: Meer informatie over de nieuwe metrische gegevens die door Azure Monitor wordt aangeboden.
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: fhryo-msft
ms.assetid: 
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 09/05/2017
ms.author: fryu
ms.openlocfilehash: d30a99044e335723e5d2c4bbd71fab7e4fd51145
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-storage-metrics-in-azure-monitor-preview"></a>Metrische gegevens van Azure Storage in de Azure-Monitor (preview)

U kunt met metrische gegevens op Azure Storage trends in gebruik, trace-aanvragen, analyseren en problemen diagnosticeren met uw opslagaccount.

Azure biedt een uniforme gebruikersinterfaces voor het bewaken van alle andere Azure-services. Zie voor meer informatie [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview.md). Azure-opslag integreert Azure Monitor door metrische gegevens te verzenden naar de Monitor van de Azure-platform.

## <a name="access-metrics"></a>Toegang tot metrische gegevens

Azure biedt verschillende manieren met toegang tot metrische gegevens. U kunt krijgen van de [Azure-portal](https://portal.azure.com), de Azure-Monitor API's (REST en .net) en analyse-oplossingen zoals bewerking Management Suite en Event Hub. Zie voor meer informatie [Azure Monitor metrische gegevens](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Metrische gegevens zijn standaard ingeschakeld en u hebt toegang tot de meest recente 30 dagen aan gegevens. Als u gegevens wilt behouden voor een langere periode nodig hebt, kunt u metrische gegevens om een Azure Storage-account te archiveren. Dit is geconfigureerd in [diagnostische instellingen](../../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) in de Azure-Monitor.

### <a name="access-metrics-in-the-azure-portal"></a>Toegang tot metrische gegevens in de Azure portal

U kunt metrische gegevens controleren na verloop van tijd in de Azure portal. Het volgende is een voorbeeld weergeven van **UsedCapacity** op niveau van de account.

![Schermafbeelding van de toegang tot metrische gegevens in de Azure portal](./media/storage-metrics-in-azure-monitor/access-metrics-in-portal.png)

Voor de metrische gegevens voor ondersteuning van dimensies, moet u met de waarde van de gewenste dimensie filteren. Het volgende is een voorbeeld weergeven van **transacties** op niveau van een account met **geslaagd** antwoordtype.

![Schermafbeelding van de toegang tot metrische gegevens met dimensie in de Azure portal](./media/storage-metrics-in-azure-monitor/access-metrics-in-portal-with-dimension.png)

### <a name="access-metrics-with-the-rest-api"></a>Toegang tot metrische gegevens met de REST-API

Azure biedt [REST-API's](/rest/api/monitor/) metrische definitie en waarden worden gelezen. In deze sectie leest u hoe de opslag metrische gegevens te lezen. De resource-ID wordt gebruikt in alle REST-API's. Lees voor meer informatie [inzicht in resource-ID voor services in de opslag](#understanding-resource-id-for-services-in-storage).

Het volgende voorbeeld ziet u hoe u [ArmClient](https://github.com/projectkudu/ARMClient) op de opdrachtregel voor het vereenvoudigen van testen met de REST-API.

#### <a name="list-account-level-metric-definition-with-the-rest-api"></a>Lijst met niveau metrische gebruikersaccount met de REST-API

Het volgende voorbeeld ziet u hoe u metrische definitie op niveau van de account:

```
# Login to Azure and enter your credentials when prompted.
> armclient login

> armclient GET /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metricdefinitions?api-version=2017-05-01-preview

```

Als u weergeven van de metrische definities voor blob, table, bestand of wachtrij wilt, moet u verschillende bron-id's voor elke service opgeven met de API.

Het antwoord bevat de definitie van de metrische in JSON-indeling:

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

#### <a name="read-account-level-metric-values-with-the-rest-api"></a>Metrische waarden met de REST-API-niveau lezen

Het volgende voorbeeld ziet u hoe metrische gegevens op het niveau van de account lezen:

```
> armclient GET "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metrics?metric=Availability&api-version=2017-05-01-preview&aggregation=Average&interval=PT1H"

```

In bovenstaande bijvoorbeeld moet als u wilt lezen metrische waarden voor blob, table, bestand of wachtrij, u verschillende bron-id's voor elke service met de API.

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

## <a name="billing-for-metrics"></a>Facturering voor metrische gegevens

Met metrische gegevens in de Azure-Monitor is die momenteel beschikbaar is. Echter, als u aanvullende oplossingen voor het opnemen van metrische gegevens gebruikt, u mogelijk worden gefactureerd door deze oplossingen. U wordt bijvoorbeeld gefactureerd door Azure Storage als archiveren van metrische gegevens aan een Azure Storage-account. Of u kunt door opnieuw Management Suite (OMS) worden gefactureerd als stream van metrische gegevens aan OMS voor geavanceerde analyse.

## <a name="understanding-resource-id-for-services-in-azure-storage"></a>Understanding resource-ID voor de services in Azure Storage

Resource-ID is een unieke id van een resource in Azure. Wanneer u de REST-API van Azure Monitor gebruikt om metrische definities of waarden te lezen, moet u de resource-ID voor de resource waarop u van plan bent om te werken. De resource-ID sjabloon volgt deze indeling:

`
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
`

Opslag voorziet in maatstaven op zowel de storage-account niveau en de service met Azure-Monitor. U kunt bijvoorbeeld de metrische gegevens voor NET Blob storage ophalen. Elk niveau heeft een eigen resource-ID wordt gebruikt voor het ophalen van de metrische gegevens voor alleen dat niveau.

### <a name="resource-id-for-a-storage-account"></a>Resource-ID voor een opslagaccount

Hieronder ziet u de indeling voor het opgeven van de Resource-ID voor een opslagaccount.

`
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}
`

### <a name="resource-id-for-the-storage-services"></a>Resource-ID voor de storage-services

Hieronder ziet u de indeling voor het opgeven van de Resource-ID voor elk van de storage-services.

* Resource-ID van BLOB-service`
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/blobServices/default
`
* Resource-ID van tabel-service`
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/tableServices/default
`
* Wachtrij-service resource-ID`
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/queueServices/default
`
* Resource-ID File-service`
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/fileServices/default
`

### <a name="resource-id-in-azure-monitor-rest-api"></a>Bron-ID in Azure Monitor REST-API

Hieronder ziet u het patroon dat wordt gebruikt bij het aanroepen van de Monitor REST-API van Azure.

`
GET {resourceId}/providers/microsoft.insights/metrics?{parameters}
`

## <a name="capacity-metrics"></a>Capaciteit metrische gegevens

Capaciteit metrische waarden worden verzonden naar Azure Monitor om het uur. De waarde worden dagelijks vernieuwd. De tijdgranulariteit definieert het tijdsinterval waarvoor metrische waarden worden weergegeven. De ondersteunde tijdgranulariteit voor alle capaciteitsmetrieken is een uur (PT1H).

Azure Storage biedt de volgende capaciteit metrische gegevens in de Azure-Monitor.

### <a name="account-level"></a>Niveau

| Metrische naam | Beschrijving |
| ------------------- | ----------------- |
| UsedCapacity | De hoeveelheid opslagruimte die wordt gebruikt door de storage-account. Voor standard-opslag-accounts is de som van de capaciteit die wordt gebruikt door de blob-, tabel-, bestands- en wachtrij. Voor premium storage-accounts en Blob storage-accounts is dit hetzelfde is als BlobCapacity. <br/><br/> Eenheid: Bytes <br/> Samenvoegingstype: gemiddelde <br/> Voorbeeld van een waarde: 1024 |

### <a name="blob-storage"></a>Blob Storage

| Metrische naam | Beschrijving |
| ------------------- | ----------------- |
| BlobCapacity | Het totaal van Blob storage gebruikt in het opslagaccount. <br/><br/> Eenheid: Bytes <br/> Samenvoegingstype: gemiddelde <br/> Voorbeeld van een waarde: 1024 <br/> Dimensie: BlobType ([definitie](#metrics-dimensions)) |
| BlobCount    | Het aantal blob-objecten die zijn opgeslagen in het opslagaccount. <br/><br/> Eenheid: aantal <br/> Samenvoegingstype: gemiddelde <br/> Voorbeeld van een waarde: 1024 <br/> Dimensie: BlobType ([definitie](#metrics-dimensions)) |
| ContainerCount    | Het aantal containers in het opslagaccount. <br/><br/> Eenheid: aantal <br/> Samenvoegingstype: gemiddelde <br/> Voorbeeld van een waarde: 1024 |

### <a name="table-storage"></a>Table Storage

| Metrische naam | Beschrijving |
| ------------------- | ----------------- |
| TableCapacity | De hoeveelheid Table storage gebruikt door de storage-account. <br/><br/> Eenheid: Bytes <br/> Samenvoegingstype: gemiddelde <br/> Voorbeeld van een waarde: 1024 |
| TableCount   | Het aantal tabellen in het opslagaccount. <br/><br/> Eenheid: aantal <br/> Samenvoegingstype: gemiddelde <br/> Voorbeeld van een waarde: 1024 |
| TableEntityCount | Het aantal tabelentiteiten in het opslagaccount. <br/><br/> Eenheid: aantal <br/> Samenvoegingstype: gemiddelde <br/> Voorbeeld van een waarde: 1024 |

### <a name="queue-storage"></a>Queue Storage

| Metrische naam | Beschrijving |
| ------------------- | ----------------- |
| QueueCapacity | De hoeveelheid Queue storage gebruikt door de storage-account. <br/><br/> Eenheid: Bytes <br/> Samenvoegingstype: gemiddelde <br/> Voorbeeld van een waarde: 1024 |
| QueueCount   | Het aantal wachtrijen in het opslagaccount. <br/><br/> Eenheid: aantal <br/> Samenvoegingstype: gemiddelde <br/> Voorbeeld van een waarde: 1024 |
| QueueMessageCount | Het aantal lopende berichtenwachtrij-berichten in het opslagaccount. <br/><br/>Eenheid: aantal <br/> Samenvoegingstype: gemiddelde <br/> Voorbeeld van een waarde: 1024 |

### <a name="file-storage"></a>File Storage

| Metrische naam | Beschrijving |
| ------------------- | ----------------- |
| FileCapacity | De hoeveelheid opslag van bestanden die worden gebruikt door de storage-account. <br/><br/> Eenheid: Bytes <br/> Samenvoegingstype: gemiddelde <br/> Voorbeeld van een waarde: 1024 |
| FileCount   | Het aantal bestanden in de storage-account. <br/><br/> Eenheid: aantal <br/> Samenvoegingstype: gemiddelde <br/> Voorbeeld van een waarde: 1024 |
| FileShareCount | Het aantal bestand deelt in het opslagaccount. <br/><br/> Eenheid: aantal <br/> Samenvoegingstype: gemiddelde <br/> Voorbeeld van een waarde: 1024 |

## <a name="transaction-metrics"></a>Transactie metrische gegevens

Transactie metrische gegevens worden verzonden van Azure-opslag naar Azure Monitor elke minuut. Alle transactie metrische gegevens zijn beschikbaar op zowel account en het serviceniveau (Blob storage, Table storage, Azure-bestanden en Queue storage). De tijdgranulariteit definieert de tijdsinterval die metrische waarden worden aangeboden. De ondersteunde tijd korrels voor alle transactie metrische gegevens zijn PT1H en PT1M.

Azure Storage biedt de volgende transactie metrische gegevens in de Azure-Monitor.

| Metrische naam | Beschrijving |
| ------------------- | ----------------- |
| Transacties | Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen als aanvragen die fouten heeft geproduceerd. <br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Toepasselijke dimensies: ResponseType, GeoType, ApiName ([definitie](#metrics-dimensions))<br/> Voorbeeld van een waarde: 1024 |
| Binnenkomend | De hoeveelheid inkomende gegevens. Dit is inclusief de inkomende gegevens via een externe client in Azure Storage, evenals een inkomend binnen Azure. <br/><br/> Eenheid: Bytes <br/> Samenvoegingstype: totaal <br/> Toepasselijke dimensies: GeoType, ApiName ([definitie](#metrics-dimensions)) <br/> Voorbeeld van een waarde: 1024 |
| Uitgaande | De hoeveelheid uitgaande gegevens. Dit is inclusief uitgaande van een externe client in Azure Storage, evenals een uitgaande binnen Azure. Dit nummer is daardoor niet factureerbare uitgaande doorgevoerd. <br/><br/> Eenheid: Bytes <br/> Samenvoegingstype: totaal <br/> Toepasselijke dimensies: GeoType, ApiName ([definitie](#metrics-dimensions)) <br/> Voorbeeld van een waarde: 1024 |
| SuccessServerLatency | De gemiddelde tijd die wordt gebruikt voor het verwerken van de aanvraag is gelukt door Azure Storage. Deze waarde is niet de netwerklatentie die is opgegeven in SuccessE2ELatency opgenomen. <br/><br/> Eenheid: milliseconden <br/> Samenvoegingstype: gemiddelde <br/> Toepasselijke dimensies: GeoType, ApiName ([definitie](#metrics-dimensions)) <br/> Voorbeeld van een waarde: 1024 |
| SuccessE2ELatency | De gemiddelde end-to-end-latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage te lezen van de aanvraag, antwoord verzenden en ontvangen van de bevestiging van het antwoord. <br/><br/> Eenheid: milliseconden <br/> Samenvoegingstype: gemiddelde <br/> Toepasselijke dimensies: GeoType, ApiName ([definitie](#metrics-dimensions)) <br/> Voorbeeld van een waarde: 1024 |
| Beschikbaarheid | Het percentage van de beschikbaarheid van de storage-service of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door te nemen van de waarde van de totale factureerbare aanvragen en deze te delen door het aantal van toepassing aanvragen, met inbegrip van aanvragen die geproduceerd onverwachte fouten. Alle onverwachte fouten leiden tot beperkte beschikbaarheid voor de storage-service of de opgegeven API-bewerking. <br/><br/> Eenheid: procent <br/> Samenvoegingstype: gemiddelde <br/> Toepasselijke dimensies: GeoType, ApiName ([definitie](#metrics-dimensions)) <br/> Voorbeeld van een waarde: 99,99 |

## <a name="metrics-dimensions"></a>Metrische gegevens dimensies

Azure Storage ondersteunt volgende dimensies voor metrische gegevens in Azure-Monitor.

| De dimensienaam van de | Beschrijving |
| ------------------- | ----------------- |
| BlobType | Het type van de blob voor alleen Blob metrische gegevens. De ondersteunde waarden zijn **BlockBlob** en **PageBlob**. Toevoeg-Blob is opgenomen in BlockBlob. |
| ResponseType | Transactietype antwoord. De beschikbare waarden zijn onder andere: <br/><br/> <li>ServerOtherError: Alle andere serverzijde fouten behalve beschreven die zijn </li> <li> ServerBusyError: Geverifieerde aanvraag die een HTTP 503-statuscode geretourneerd. (Nog niet ondersteund) </li> <li> ServerTimeoutError: Time-out geverifieerde aanvraag die een HTTP 500-statuscode geretourneerd. De time-out is opgetreden vanwege een serverfout. </li> <li> ThrottlingError: De som van de clientzijde en serverzijde bandbreedteregeling fout (deze wordt verwijderd zodra ServerBusyError en ClientThrottlingError worden ondersteund) </li> <li> AuthorizationError: Geverifieerde aanvraag die is mislukt vanwege niet-geautoriseerde toegang tot gegevens of een Autorisatiefout. </li> <li> NetworkError: Geverifieerde aanvraag die is mislukt vanwege netwerkfouten. Treedt meestal op wanneer een client een verbinding voor de vervaldatum van de time-out voor de voortijdig wordt gesloten. </li> <li>  ClientThrottlingError: Clientzijde bandbreedteregeling fout (nog niet ondersteund) </li> <li> ClientTimeoutError: Time-out geverifieerde aanvraag die een HTTP 500-statuscode geretourneerd. Als de client netwerktime-out- of time-out van de aanvraag is ingesteld op een lagere waarde dan werd verwacht door de storage-service, is een time-out van de verwachte. Anders wordt deze gerapporteerd als een ServerTimeoutError. </li> <li> ClientOtherError: Beschreven die zijn met uitzondering van alle andere client-side '-fouten. </li> <li> Voltooid: Aanvraag is gelukt|
| GeoType | De transactie uit cluster primair of secundair. De beschikbare waarden zijn primair en secundair. Dit geldt voor leestoegang geografisch redundante Storage(RA-GRS) bij het lezen van objecten van secundaire tenant. |
| apiName | De naam van de bewerking. Bijvoorbeeld: <br/> <li>CreateContainer</li> <li>DeleteBlob</li> <li>GetBlob</li> Zie voor alle namen van de bewerking, [document](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages#logged-operations.md). |

Voor de metrische gegevens ondersteunende dimensies moet u de dimensiewaarde om te zien van de bijbehorende waarden van de metrische gegevens op te geven. Als u bijvoorbeeld **transacties** waarde voor geslaagde antwoorden die u wilt filteren de **ResponseType** dimensie met **geslaagd**. Of als u bekijkt **BlobCount** waarde voor blok-Blob, moet u voor het filteren van de **BlobType** dimensie met **BlockBlob**.

## <a name="service-continuity-of-legacy-metrics"></a>Continuïteit van de service van verouderde metrische gegevens

Verouderde metrische gegevens zijn parallel met Azure Monitor beheerd metrische gegevens beschikbaar. De ondersteuning blijft hetzelfde tot Azure Storage is beëindigd door de service op verouderde metrische gegevens. We zullen het eind plan aankondigen nadat we Azure Monitor beheerd metrische gegevens officiële release.

## <a name="see-also"></a>Zie ook

* [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview.md)
