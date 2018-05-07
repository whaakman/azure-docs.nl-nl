---
title: Migratie van de metrische gegevens Azure Storage | Microsoft Docs
description: Meer informatie over het migreren van oudere metrische gegevens met nieuwe metrische gegevens die worden beheerd door de Azure-Monitor.
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
ms.openlocfilehash: da8eb0b9e2e5aba60b61a36d83f525c7ce4a7958
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
---
# <a name="azure-storage-metrics-migration"></a>Migratie van Azure Storage metrische gegevens

Uitgelijnd met de strategie van aaneengeschakelde Monitor ervaring in Azure, Azure Storage op-boards metrische gegevens voor Monitor van de Azure-platform. De service van verouderde metrische gegevens in de toekomst wordt beëindigd met vroege kennisgeving op basis van beleid voor Azure. Als u gebruik maakt van metrische gegevens van oude opslag, moet u voorafgaand aan de einddatum van de service om op peil uw metrische gegevens te migreren.

In dit artikel begeleidt u bij de migratie van verouderde metrische gegevens naar de nieuwe metrische gegevens.

## <a name="understand-legacy-metrics-managed-by-azure-storage"></a>Verouderde metrische gegevens die worden beheerd door Azure Storage begrijpen

Azure Storage verouderde metrische waarden, statistische functies, verzamelt en slaat ze op in de tabellen $Metric binnen hetzelfde opslagaccount. U kunt Azure portal gebruiken voor het instellen van de bewaking van de grafiek en u kunt ook een Azure-opslag-SDK's gebruiken om te lezen van de gegevens uit $Metric tabellen op basis van het schema. Voor meer informatie raadpleegt u dit [Opslaganalyse](./storage-analytics.md).

Verouderde metrische gegevens bieden capaciteitsmetrieken op Blob alleen en transactie metrische gegevens over de Blob-, tabel-, bestands- en wachtrij.

Verouderde metrische gegevens zijn ontworpen in platte-schema. Het ontwerp resulteert in de waarde van nul metrische wanneer u de activering van de metriek verkeerspatronen geen hebt. Bijvoorbeeld: **ServerTimeoutError** is ingesteld op 0 in $Metric tabellen, zelfs wanneer u niet alle server-time-outfouten van het live verkeer naar storage-account ontvangt.

## <a name="understand-new-metrics-managed-by-azure-monitor"></a>Nieuwe metrische gegevens die worden beheerd door Azure Monitor begrijpen

Voor de nieuwe opslag metrische gegevens verzendt Azure Storage de metrische gegevens back-end van Azure-Monitor. Azure biedt een geïntegreerde bewakingservaring, met inbegrip van gegevens vanuit de portal en gegevensopname. Voor meer informatie raadpleegt u dit [artikel](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Nieuwe metrische gegevens bieden capaciteitsmetrieken en transactie metrische gegevens over Blob, Table, bestand, wachtrij en premium-opslag.

Meerdere dimensie is een van de functies van Azure-Monitor. Azure Storage neemt het ontwerp voor het definiëren van nieuwe metrische schema. Voor ondersteunde dimensies op metrische gegevens vindt u informatie in deze [Azure Storage metrische gegevens in Azure Monitor](./storage-metrics-in-azure-monitor.md). Meerdere dimensie ontwerp biedt rentabiliteit voor beide bandbreedte opname en capaciteit van de metrische gegevens opslaan. Dus als uw verkeer is niet geactiveerd voor verwante metrische gegevens, wordt de verwante metrische gegevens niet gegenereerd. Bijvoorbeeld, als uw verkeer is niet geactiveerd voor elke server time-outfouten, Azure Monitor retourneert geen gegevens als u de waarde van metrische gegevens van een query **transacties** met dimensie **ResponseType** gelijk zijn aan **ServerTimeoutError**.

## <a name="metrics-mapping-between-legacy-metrics-and-new-metrics"></a>Toewijzing van de metrische gegevens tussen verouderde metrische gegevens en nieuwe metrische gegevens

Als u programmatisch metrische gegevens lezen, moet u vaststellen van het nieuwe metrische schema in uw programma's. Voor meer informatie over de wijzigingen, raadpleegt u de toewijzing in de volgende tabel weergegeven:

**Capaciteit metrische gegevens**

| Verouderde metrische gegevens | Nieuwe metrische gegevens |
| ------------------- | ----------------- |
| Capaciteit            | BlobCapacity met dimensie BlobType gelijk is aan BlockBlob of PageBlob |
| ObjectCount         | BlobCount met dimensie BlobType gelijk is aan BlockBlob of PageBlob |
| ContainerCount      | ContainerCount |

De volgende metrische gegevens zijn nieuwe aanbiedingen die niet, verouderde metrische gegevens ondersteunen:
* TableCapacity
* TableCount
* TableEntityCount
* QueueCapacity
* QueueCount
* QueueMessageCount
* FileCapacity
* FileCount
* FileShareCount
* UsedCapacity

**Transactie metrische gegevens**

| Verouderde metrische gegevens | Nieuwe metrische gegevens |
| ------------------- | ----------------- |
| AnonymousAuthorizationError | Transacties met dimensie ResponseType gelijk zijn aan AuthorizationError |
| AnonymousClientOtherError | Transacties met dimensie ResponseType gelijk zijn aan ClientOtherError |
| AnonymousClientTimeoutError | Transacties met dimensie ResponseType gelijk zijn aan ClientTimeoutError |
| AnonymousNetworkError | Transacties met dimensie ResponseType gelijk zijn aan NetworkError |
| AnonymousServerOtherError | Transacties met dimensie ResponseType gelijk zijn aan ServerOtherError |
| AnonymousServerTimeoutError | Transacties met dimensie ResponseType gelijk zijn aan ServerTimeoutError |
| AnonymousSuccess | Transacties met dimensie ResponseType gelijk zijn aan geslaagd |
| AnonymousThrottlingError | Transacties met dimensie ResponseType gelijk is aan ClientThrottlingError of ServerBusyError |
| AuthorizationError | Transacties met dimensie ResponseType gelijk zijn aan AuthorizationError |
| Beschikbaarheid | Beschikbaarheid |
| AverageE2ELatency | SuccessE2ELatency |
| AverageServerLatency | SuccessServerLatency |
| ClientOtherError | Transacties met dimensie ResponseType gelijk zijn aan ClientOtherError |
| ClientTimeoutError | Transacties met dimensie ResponseType gelijk zijn aan ClientTimeoutError |
| NetworkError | Transacties met dimensie ResponseType gelijk zijn aan NetworkError |
| PercentAuthorizationError | Transacties met dimensie ResponseType gelijk zijn aan AuthorizationError |
| PercentClientOtherError | Transacties met dimensie ResponseType gelijk zijn aan ClientOtherError |
| PercentNetworkError | Transacties met dimensie ResponseType gelijk zijn aan NetworkError |
| PercentServerOtherError | Transacties met dimensie ResponseType gelijk zijn aan ServerOtherError |
| PercentSuccess | Transacties met dimensie ResponseType gelijk zijn aan geslaagd |
| PercentThrottlingError | Transacties met dimensie ResponseType gelijk is aan ClientThrottlingError of ServerBusyError |
| PercentTimeoutError | Transacties met dimensie ResponseType gelijk zijn aan ServerTimeoutError of gelijk zijn aan ClientTimeoutError ResponseType|
| SASAuthorizationError | Transacties met dimensie ResponseType gelijk zijn aan AuthorizationError |
| SASClientOtherError | Transacties met dimensie ResponseType gelijk zijn aan ClientOtherError |
| SASClientTimeoutError | Transacties met dimensie ResponseType gelijk zijn aan ClientTimeoutError |
| SASNetworkError | Transacties met dimensie ResponseType gelijk zijn aan NetworkError |
| SASServerOtherError | Transacties met dimensie ResponseType gelijk zijn aan ServerOtherError |
| SASServerTimeoutError | Transacties met dimensie ResponseType gelijk zijn aan ServerTimeoutError |
| SASSuccess | Transacties met dimensie ResponseType gelijk zijn aan geslaagd |
| SASThrottlingError | Transacties met dimensie ResponseType gelijk is aan ClientThrottlingError of ServerBusyError |
| ServerOtherError | Transacties met dimensie ResponseType gelijk zijn aan ServerOtherError |
| ServerTimeoutError | Transacties met dimensie ResponseType gelijk zijn aan ServerTimeoutError |
| Geslaagd | Transacties met dimensie ResponseType gelijk zijn aan geslaagd |
| ThrottlingError | Transacties met dimensie ResponseType gelijk is aan ClientThrottlingError of ServerBusyError |
| TotalBillableRequests | Transacties |
| TotalEgress | Uitgaand verkeer |
| TotalIngress | Binnenkomend |
| TotalRequests | Transacties |

## <a name="faq"></a>Veelgestelde vragen

* Hoe moet ik bestaande waarschuwingsregels migreren?

A: als u de klassieke waarschuwing regels op basis van de metrische gegevens van oude opslagruimte hebt gemaakt, moet u nieuwe waarschuwing regels op basis van nieuwe metrische schema maken.

* Nieuwe metrische gegevens die worden standaard opgeslagen in hetzelfde opslagaccount maakt?

A: Nee. Als u archiveren van de metrische gegevens naar storage-account wilt, kunt u [Azure Monitor diagnostische instelling API](https://docs.microsoft.com/en-us/rest/api/monitor/diagnosticsettings/createorupdate)

## <a name="next-steps"></a>Volgende stappen

* [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview.md)
* [Metrische gegevens in de Monitor van Azure Storage](./storage-metrics-in-azure-monitor.md)
