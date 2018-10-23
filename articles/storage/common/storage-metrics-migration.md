---
title: Azure Storage metrics-migratie | Microsoft Docs
description: Informatie over het oude metrische gegevens migreren naar nieuwe metrische gegevens die worden beheerd door Azure Monitor.
services: storage
author: fhryo-msft
ms.service: storage
ms.topic: article
ms.date: 03/30/2018
ms.author: fryu
ms.component: common
ms.openlocfilehash: 3f2ebb82f5affa3c41f237edcc039eb6214c7a4c
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49649292"
---
# <a name="azure-storage-metrics-migration"></a>Azure Storage-metrische gegevens over migratie

Uitgelijnd met de strategie van de werking van de monitor in Azure, Azure Storage kan worden geïntegreerd metrische gegevens voor het bewaken van de Azure-platform. De service van de oude metrische gegevens worden in de toekomst, eindigen met een vroegtijdig op basis van Azure policy. Als u zich op oude opslag metrische gegevens baseert, die u wilt migreren vóór de einddatum van de service om toegang te houden van uw metrische gegevens.

Dit artikel leest u hoe het migreren van de oude metrische gegevens naar de nieuwe metrische gegevens.

## <a name="understand-old-metrics-that-are-managed-by-azure-storage"></a>Inzicht in de oude metrische gegevens die worden beheerd door Azure Storage

Azure Storage oude metrische waarden, verzamelt en maakt een aggregatie en slaat ze op in tabellen $Metric binnen hetzelfde opslagaccount. U kunt de Azure-portal gebruiken voor het instellen van een controle grafiek. U kunt ook de Azure Storage-SDK's gebruiken om te lezen van de gegevens uit $Metric tabellen die zijn gebaseerd op het schema. Zie voor meer informatie, [Opslaganalyse](./storage-analytics.md).

Metrische gegevens over capaciteit bieden oude metrische gegevens alleen op Azure Blob-opslag. Oude metrische gegevens bieden transactie metrische gegevens over Blob storage, Table storage, Azure Files en Queue storage.

Oude metrische gegevens zijn in een vast schema ontworpen. Het ontwerp resulteert in de metrische waarde nul wanneer u niet over de verkeerspatronen van de metrische gegevens wordt geactiveerd. Bijvoorbeeld, de **ServerTimeoutError** waarde is ingesteld op 0 in $Metric tabellen, zelfs wanneer u geen serverfouten voor time-out voor van de live-verkeer naar een storage-account ontvangen.

## <a name="understand-new-metrics-managed-by-azure-monitor"></a>Informatie over nieuwe metrische gegevens die worden beheerd door Azure Monitor

Voor nieuwe metrische opslaggegevens verzendt Azure Storage de metrische gegevens naar de back-end van Azure Monitor. Azure Monitor biedt een uniforme bewakingservaring, met inbegrip van gegevens vanuit de portal en de opname van gegevens. Voor meer informatie kunt u verwijzen naar dit [artikel](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Nieuwe metrische gegevens bieden metrische gegevens over capaciteit en metrische gegevens die transactie op Blob-, tabel-, bestands-, wachtrij- en premium-opslag.

Multidimensionale is een van de functies van Azure Monitor. Azure Storage neemt het ontwerp op een nieuwe metrische schema definiëren. Voor ondersteunde dimensies over metrische gegevens, vindt u informatie in [metrische gegevens van Azure Storage in Azure Monitor](./storage-metrics-in-azure-monitor.md). Multidimensionale ontwerp biedt kostenefficiëntie die op beide bandbreedte van gegevensopname en -capaciteit kunnen geen metrische gegevens opslaan. Als gevolg hiervan als uw verkeer is niet geactiveerd voor verwante metrische gegevens, wordt de verwante metrische gegevens niet gegenereerd. Bijvoorbeeld, als uw verkeer heeft serverfouten time-out niet geactiveerd, Azure Monitor biedt geen gegevens geretourneerd wanneer u de waarde van metrische gegevens van een query **transacties** met dimensie **ResponseType** gelijk zijn aan **ServerTimeoutError**.

## <a name="metrics-mapping-between-old-metrics-and-new-metrics"></a>Metrische gegevens toewijzing tussen de oude metrische gegevens en nieuwe metrische gegevens

Als u metrische gegevens via een programma hebt gelezen, moet u vaststellen van het schema voor nieuwe metrische gegevens in uw programma's. Voor meer informatie over de wijzigingen, kunt u verwijzen naar de toewijzing die worden vermeld in de volgende tabel:

**Capaciteit metrische gegevens**

| Oude metrische gegevens | Nieuwe metrische gegevens |
| ------------------- | ----------------- |
| **Capaciteit**            | **BlobCapacity** met de dimensie **BlobType** gelijk zijn aan **BlockBlob** of **PageBlob** |
| **ObjectCount**        | **BlobCount** met de dimensie **BlobType** gelijk zijn aan **BlockBlob** of **PageBlob** |
| **ContainerCount**      | **ContainerCount** |

De volgende metrische gegevens zijn nieuwe aanbiedingen, bieden geen ondersteuning voor de oude metrische gegevens:
* **TableCapacity**
* **TableCount**
* **TableEntityCount**
* **QueueCapacity**
* **QueueCount**
* **QueueMessageCount**
* **FileCapacity**
* **FileCount**
* **FileShareCount**
* **UsedCapacity**

**Metrische gegevens de transactie**

| Oude metrische gegevens | Nieuwe metrische gegevens |
| ------------------- | ----------------- |
| **AnonymousAuthorizationError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **AuthorizationError** - en dimensietabellen **verificatie** gelijk zijn aan **anonieme verificatie** |
| **AnonymousClientOtherError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientOtherError** - en dimensietabellen **verificatie** gelijk zijn aan **anonieme verificatie** |
| **AnonymousClientTimeoutError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientTimeoutError** - en dimensietabellen **verificatie** gelijk zijn aan **anonieme verificatie** |
| **AnonymousNetworkError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **NetworkError** - en dimensietabellen **verificatie** gelijk zijn aan **anonieme verificatie** |
| **AnonymousServerOtherError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ServerOtherError** - en dimensietabellen **verificatie** gelijk zijn aan **anonieme verificatie** |
| **AnonymousServerTimeoutError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ServerTimeoutError** - en dimensietabellen **verificatie** gelijk zijn aan **anonieme verificatie** |
| **AnonymousSuccess** | Transacties met de dimensie **ResponseType** gelijk zijn aan **succes** - en dimensietabellen **verificatie** gelijk zijn aan **anonieme verificatie** |
| **AnonymousThrottlingError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientThrottlingError** of **ServerBusyError** - en dimensietabellen **verificatie** gelijk zijn aan **anonieme verificatie** |
| **AuthorizationError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **AuthorizationError** |
| **Beschikbaarheid** | **Beschikbaarheid** |
| **AverageE2ELatency** | **SuccessE2ELatency** |
| **Averageserverlatency aan** | **SuccessServerLatency** |
| **ClientOtherError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientOtherError** |
| **ClientTimeoutError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientTimeoutError** |
| **NetworkError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **NetworkError** |
| **PercentAuthorizationError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **AuthorizationError** |
| **PercentClientOtherError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientOtherError** |
| **Percentnetworkerror aan** | Transacties met de dimensie **ResponseType** gelijk zijn aan **NetworkError** |
| **PercentServerOtherError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ServerOtherError** |
| **PercentSuccess** | Transacties met de dimensie **ResponseType** gelijk zijn aan **geslaagd** |
| **Percentthrottlingerror aan** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientThrottlingError** of **ServerBusyError** |
| **Percenttimeouterror aan** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ServerTimeoutError** of **ResponseType** gelijk zijn aan **ClientTimeoutError** |
| **SASAuthorizationError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **AuthorizationError** - en dimensietabellen **verificatie** gelijk zijn aan **SAS** |
| **SASClientOtherError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientOtherError** - en dimensietabellen **verificatie** gelijk zijn aan **SAS** |
| **SASClientTimeoutError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientTimeoutError** - en dimensietabellen **verificatie** gelijk zijn aan **SAS** |
| **SASNetworkError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **NetworkError** - en dimensietabellen **verificatie** gelijk zijn aan **SAS** |
| **SASServerOtherError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ServerOtherError** - en dimensietabellen **verificatie** gelijk zijn aan **SAS** |
| **SASServerTimeoutError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ServerTimeoutError** - en dimensietabellen **verificatie** gelijk zijn aan **SAS** |
| **SASSuccess** | Transacties met de dimensie **ResponseType** gelijk zijn aan **succes** - en dimensietabellen **verificatie** gelijk zijn aan **SAS** |
| **SASThrottlingError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientThrottlingError** of **ServerBusyError** - en dimensietabellen **verificatie** gelijk zijn aan **SAS** |
| **ServerOtherError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ServerOtherError** |
| **ServerTimeoutError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ServerTimeoutError** |
| **Geslaagd** | Transacties met de dimensie **ResponseType** gelijk zijn aan **geslaagd** |
| **ThrottlingError** | **Transacties** met de dimensie **ResponseType** gelijk zijn aan **ClientThrottlingError** of **ServerBusyError**|
| **TotalBillableRequests** | **Transacties** |
| **TotalEgress** | **Egress** |
| **TotalIngress** | **Ingress** |
| **TotalRequests** | **Transacties** |

## <a name="faq"></a>Veelgestelde vragen

### <a name="how-should-i-migrate-existing-alert-rules"></a>Hoe moet ik bestaande waarschuwingsregels migreren?

Als u regels voor klassieke waarschuwingen op basis van metrische gegevens van oude storage hebt gemaakt, moet u de nieuwe waarschuwing regels op basis van het schema voor nieuwe metrische gegevens maken.

### <a name="is-new-metric-data-stored-in-the-same-storage-account-by-default"></a>Nieuwe metrische gegevens in hetzelfde opslagaccount standaard opgeslagen?

Nee. Als u wilt de metrische gegevens naar een opslagaccount archiveren, gebruikt u de [diagnostische instelling API van Azure Monitor](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings/createorupdate).

## <a name="next-steps"></a>Volgende stappen

* [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview.md)
* [Metrische gegevens van Storage in Azure Monitor](./storage-metrics-in-azure-monitor.md)
