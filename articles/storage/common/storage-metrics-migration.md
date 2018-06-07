---
title: Migratie van de metrische gegevens Azure Storage | Microsoft Docs
description: Informatie over het oude metrische gegevens migreren naar nieuwe metrische gegevens die worden beheerd door de Azure-Monitor.
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: fhryo-msft
ms.assetid: ''
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 03/30/2018
ms.author: fryu
ms.openlocfilehash: c64061aee94e8c08a3f6bcae78cffca0b4172d97
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34650669"
---
# <a name="azure-storage-metrics-migration"></a>Migratie van Azure Storage metrische gegevens

Azure Storage wordt uitgelijnd met de strategie van de werking van de monitor in Azure, geïntegreerd metrische gegevens voor het bewaken van de Azure-platform. De service van de oude metrische gegevens worden in de toekomst eindigen met een vroegtijdig op basis van beleid voor Azure. Als u gebruik maakt van de oude opslag metrische gegevens, moet u voorafgaand aan de einddatum van de service om op peil uw metrische gegevens te migreren.

In dit artikel leest u hoe van de oude metrische gegevens naar de nieuwe metrische gegevens te migreren.

## <a name="understand-old-metrics-that-are-managed-by-azure-storage"></a>Oude metrische gegevens die worden beheerd door Azure Storage begrijpen

Azure Storage oude metrische waarden, verzamelt en samenvoegt en slaat ze op in de tabellen $Metric binnen hetzelfde opslagaccount. De Azure-portal kunt u een controle grafiek instellen. U kunt ook de Azure-opslag-SDK's gebruiken de gegevens lezen uit $Metric tabellen die zijn gebaseerd op het schema. Zie voor meer informatie [Opslaganalyse](./storage-analytics.md).

Oude metrische gegevens bieden capaciteitsmetrieken alleen op Azure Blob-opslag. Oude metrische gegevens verstrekken transactie metrische gegevens over Blob storage, Table storage, Azure-bestanden en Queue storage. 

Oude metrische gegevens zijn ontworpen in een platte schema. Het ontwerp resulteert in de waarde van nul metrische wanneer u de activering van de metriek verkeerspatronen geen hebt. Bijvoorbeeld, de **ServerTimeoutError** waarde is ingesteld op 0 in $Metric tabellen, zelfs wanneer u niet alle server-time-outfouten van het live verkeer naar een opslagaccount ontvangt.

## <a name="understand-new-metrics-managed-by-azure-monitor"></a>Nieuwe metrische gegevens die worden beheerd door Azure Monitor begrijpen

Voor de nieuwe opslag metrische gegevens verzendt Azure Storage de metrische gegevens naar de back-end van Azure-Monitor. Azure biedt een geïntegreerde bewakingservaring, met inbegrip van gegevens vanuit de portal en gegevensopname. Voor meer informatie raadpleegt u dit [artikel](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Nieuwe metrische gegevens bieden capaciteitsmetrieken en transactie metrische gegevens over Blob, Table, bestand, wachtrij en premium-opslag.

Meerdere dimensie is een van de functies die Azure biedt. Azure Storage neemt het ontwerp voor het definiëren van nieuwe metrische schema. Voor ondersteunde dimensies op metrische gegevens vindt u informatie in [Azure Storage metrische gegevens in Azure Monitor](./storage-metrics-in-azure-monitor.md). Meerdere dimensie ontwerp biedt rentabiliteit voor beide bandbreedte opname en capaciteit van de metrische gegevens opslaan. Dus als uw verkeer is niet geactiveerd voor verwante metrische gegevens, wordt de verwante metrische gegevens niet gegenereerd. Bijvoorbeeld als verkeer is niet geactiveerd voor elke server time-outfouten, Azure Monitor geen gegevens geretourneerd als u de waarde van metrische gegevens van een query **transacties** met dimensie **ResponseType** gelijk aan **ServerTimeoutError**.

## <a name="metrics-mapping-between-old-metrics-and-new-metrics"></a>Toewijzing van de metrische gegevens tussen oude metrische gegevens en nieuwe metrische gegevens

Als u programmatisch metrische gegevens lezen, moet u vaststellen van het nieuwe metrische schema in uw programma's. Voor meer informatie over de wijzigingen, raadpleegt u de toewijzing in de volgende tabel weergegeven:

**Capaciteit metrische gegevens**

| Oude metrische gegevens | Nieuwe metrische gegevens |
| ------------------- | ----------------- |
| **Capaciteit**            | **BlobCapacity** met de dimensie **BlobType** gelijk zijn aan **BlockBlob** of **PageBlob** |
| **ObjectCount**        | **BlobCount** met de dimensie **BlobType** gelijk zijn aan **BlockBlob** of **PageBlob** |
| **ContainerCount**      | **ContainerCount** |

De volgende metrische gegevens zijn nieuwe aanbiedingen die niet, de oude metrische gegevens ondersteunen:
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

**Transactie metrische gegevens**

| Oude metrische gegevens | Nieuwe metrische gegevens |
| ------------------- | ----------------- |
| **AnonymousAuthorizationError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **AuthorizationError** |
| **AnonymousClientOtherError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientOtherError** |
| **AnonymousClientTimeoutError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientTimeoutError** |
| **AnonymousNetworkError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **NetworkError** |
| **AnonymousServerOtherError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ServerOtherError** |
| **AnonymousServerTimeoutError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ServerTimeoutError** |
| **AnonymousSuccess** | Transacties met de dimensie **ResponseType** gelijk zijn aan **geslaagd** |
| **AnonymousThrottlingError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientThrottlingError** of **ServerBusyError** |
| **AuthorizationError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **AuthorizationError** |
| **Beschikbaarheid** | **Beschikbaarheid** |
| **AverageE2ELatency** | **SuccessE2ELatency** |
| **AverageServerLatency** | **SuccessServerLatency** |
| **ClientOtherError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientOtherError** |
| **ClientTimeoutError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientTimeoutError** |
| **NetworkError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **NetworkError** |
| **PercentAuthorizationError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **AuthorizationError** |
| **PercentClientOtherError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientOtherError** |
| **PercentNetworkError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **NetworkError** |
| **PercentServerOtherError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ServerOtherError** |
| **PercentSuccess** | Transacties met de dimensie **ResponseType** gelijk zijn aan **geslaagd** |
| **PercentThrottlingError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientThrottlingError** of **ServerBusyError** |
| **PercentTimeoutError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ServerTimeoutError** of **ResponseType** gelijk zijn aan **ClientTimeoutError** |
| **SASAuthorizationError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **AuthorizationError** |
| **SASClientOtherError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientOtherError** |
| **SASClientTimeoutError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientTimeoutError** |
| **SASNetworkError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **NetworkError** |
| **SASServerOtherError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ServerOtherError** |
| **SASServerTimeoutError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ServerTimeoutError** |
| **SASSuccess** | Transacties met de dimensie **ResponseType** gelijk zijn aan **geslaagd** |
| **SASThrottlingError** | Transacties met de dimensie **ResponseType** gelijk zijn aan **ClientThrottlingError** of **ServerBusyError** |
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

Als u regels voor klassieke waarschuwingen op basis van de oude opslag metrische gegevens hebt gemaakt, moet u de nieuwe waarschuwing regels op basis van het nieuwe metrische schema maken.

### <a name="is-new-metric-data-stored-in-the-same-storage-account-by-default"></a>Nieuwe metrische gegevens worden opgeslagen in hetzelfde opslagaccount standaard?

Nee. Als u wilt archiveren van de metrische gegevens naar een opslagaccount, gebruiken de [Azure Monitor diagnostische instelling API](https://docs.microsoft.com/en-us/rest/api/monitor/diagnosticsettings/createorupdate).

## <a name="next-steps"></a>Volgende stappen

* [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview.md)
* [Metrische gegevens in de Monitor van Azure Storage](./storage-metrics-in-azure-monitor.md)
