---
title: Schaalbaarheid en de prestatiedoelen voor Azure Storage
description: Meer informatie over de schaalbaarheids- en prestatiedoelen, met inbegrip van capaciteit en snelheid van aanvragen voor binnenkomende en uitgaande bandbreedte, voor standard Azure storage-accounts.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 11/08/2018
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: f8227495d7ca2e1d43b74066d359d0d59e456263
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55451203"
---
# <a name="azure-storage-scalability-and-performance-targets-for-standard-storage-accounts"></a>Azure Storage-schaalbaarheids- en prestatiedoelen voor standard storage-accounts

Dit artikel worden de schaalbaarheids- en prestatiedoelen voor standaard Azure storage-accounts. De schaalbaarheids- en prestatiedoelen die hier worden vermeld geavanceerde doelen zijn, maar worden uitgevoerd. In alle gevallen, de snelheid van aanvragen en bandbreedte met uw opslag account is afhankelijk van de grootte van de objecten die zijn opgeslagen, de patronen voor databasetoegang gebruikt, en het type werkbelasting van uw toepassing uitvoert. 

Zorg ervoor dat uw service om te bepalen of de prestaties, voldoet aan uw vereisten testen. Indien mogelijk, te voorkomen dat plotselinge pieken in de snelheid van verkeer en zorg ervoor dat verkeer goed gedistribueerd over meerdere partities.

Wanneer uw toepassing de limiet bereikt van wat een partitie voor uw werkbelasting kan verwerken, begint de Azure Storage om terug te keren foutcode 503 (Server bezet) of foutcode antwoorden van 500 (time-out van de bewerking). Als 503-fouten optreden, kunt u overwegen uw toepassing met een beleid voor exponentieel uitstel voor nieuwe pogingen wijzigen. De exponentieel uitstel kan de belasting op de partitie te verlagen en om te vergemakkelijken van pieken in verkeer naar de betreffende partitie.

## <a name="standard-storage-account-scale-limits"></a>Schaallimieten voor Standard storage-account
[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

## <a name="storage-resource-provider-scale-limits"></a>Schaallimieten voor Storage resource provider 

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Schaal prestatiedoelen voor Azure Blob storage
[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Azure bestanden schaal doelen
Zie voor meer informatie over de schaal- en prestatiedoelen voor Azure Files en Azure File Sync, [schaalbaarheids- en prestatiedoelen van Azure Files](../files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="azure-file-sync-scale-targets"></a>Azure File Sync schaal doelen
Azure File Sync is ontworpen met het doel van onbeperkt gebruik, maar onbeperkte gebruik is niet altijd mogelijk. De volgende tabel geeft aan dat de grenzen van het testen van Microsoft en geeft ook aan welke doelen vaste limieten zijn:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="azure-queue-storage-scale-targets"></a>Schaal prestatiedoelen voor Azure Queue storage
[!INCLUDE [storage-queues-scale-targets](../../../includes/storage-queues-scale-targets.md)]

## <a name="azure-table-storage-scale-targets"></a>Schaal prestatiedoelen voor Azure Table storage
[!INCLUDE [storage-table-scale-targets](../../../includes/storage-tables-scale-targets.md)]

## <a name="see-also"></a>Zie ook
* [Prijsinformatie voor opslag](https://azure.microsoft.com/pricing/details/storage/)
* [Azure-abonnement en Servicelimieten, Quotums en beperkingen](../../azure-subscription-service-limits.md)
* [Azure Storage-replicatie](../storage-redundancy.md)
* [Controlelijst voor prestaties en schaalbaarheid van Microsoft Azure Storage](../storage-performance-checklist.md)

