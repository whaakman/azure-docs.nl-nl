---
title: Azure Storage schaalbaarheids- en prestatiedoelen - storage-accounts
description: Meer informatie over de schaalbaarheids- en prestatiedoelen, met inbegrip van capaciteit en snelheid van aanvragen voor binnenkomende en uitgaande bandbreedte, voor Azure storage-accounts.
services: storage
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: e3e0e9ae4a1939aad9ab2ae42a1b51b1b00e2462
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/12/2019
ms.locfileid: "59521713"
---
# <a name="azure-storage-scalability-and-performance-targets-for-storage-accounts"></a>Azure Storage-schaalbaarheids- en prestatiedoelen van storage-accounts

Dit artikel worden de schaalbaarheids- en prestatiedoelen voor Azure storage-accounts. De schaalbaarheids- en prestatiedoelen die hier worden vermeld geavanceerde doelen zijn, maar worden uitgevoerd. In alle gevallen, de snelheid van aanvragen en bandbreedte met uw opslag account is afhankelijk van de grootte van de objecten die zijn opgeslagen, de patronen voor databasetoegang gebruikt, en het type werkbelasting van uw toepassing uitvoert.

Zorg ervoor dat uw service om te bepalen of de prestaties, voldoet aan uw vereisten testen. Indien mogelijk, te voorkomen dat plotselinge pieken in de snelheid van verkeer en zorg ervoor dat verkeer goed gedistribueerd over meerdere partities.

Wanneer uw toepassing de limiet bereikt van wat een partitie voor uw werkbelasting kan verwerken, begint de Azure Storage om terug te keren foutcode 503 (Server bezet) of foutcode antwoorden van 500 (time-out van de bewerking). Als 503-fouten optreden, kunt u overwegen uw toepassing met een beleid voor exponentieel uitstel voor nieuwe pogingen wijzigen. De exponentieel uitstel kan de belasting op de partitie te verlagen en om te vergemakkelijken van pieken in verkeer naar de betreffende partitie.

## <a name="storage-account-scale-limits"></a>Schaallimieten voor Storage-account

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

## <a name="premium-performance-storage-account-scale-limits"></a>Premium storage-account schalen prestatielimieten

[!INCLUDE [azure-premium-limits](../../../includes/azure-storage-limits-premium.md)]

## <a name="storage-resource-provider-scale-limits"></a>Schaallimieten voor Storage resource provider

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Schaal prestatiedoelen voor Azure Blob storage

[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Azure bestanden schaal doelen

Zie voor meer informatie over de schaal- en prestatiedoelen voor Azure Files en Azure File Sync, [schaalbaarheids- en prestatiedoelen van Azure Files](../files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="premium-files-scale-targets"></a>Premium-bestanden schalen doelen

Er zijn drie soorten beperkingen van premium-bestanden: storage-accounts, bestandsshares en bestanden.

Bijvoorbeeld: Een afzonderlijke share kan maar liefst 100.000 IOPS en maximaal 5000 IOP's in een enkel bestand kan worden geschaald. Dus als u hebt drie bestanden in één share, het maximum aantal IOP's krijgt u van deze share is bijvoorbeeld 15.000.

#### <a name="premium-file-share-limits"></a>Limieten voor Premium-bestand delen

> [!IMPORTANT]
> Storage-accountlimieten gelden voor alle shares. Schalen tot de maximale waarde voor storage-accounts is alleen mogelijk als er slechts één share per opslagaccount.

|Onderwerp  |Doel  |
|---------|---------|
|Minimale ingerichte grootte                        |100 GiB      |
|Maximale grootte van ingericht                        |100 TiB      |
|Minimale grootte vergroten/verkleinen    |1 GiB      |
|Basislijn IOPS    |1 IOP's per GiB maximaal 100.000|
|IOPS bursting    |3 x IOPS per GiB maximaal 100.000|
|Snelheid van uitgangsgebeurtenissen         |60 MiB/s + 0,06 * ingericht GiB        |
|Gegevens| 40 MiB/s + 0,04 * ingericht GiB |
|Maximum aantal momentopnamen        |200       |

#### <a name="premium-file-limits"></a>Limieten voor Premium-bestand

|Onderwerp  |Doel  |
|---------|---------|
|Grootte                  |1 TiB         |
|Maximale IOPS per bestand     |5.000         |
|Gelijktijdige verwerkt    |2,000         |

### <a name="azure-file-sync-scale-targets"></a>Azure File Sync schaal doelen

Azure File Sync is ontworpen met het doel van onbeperkt gebruik, maar onbeperkte gebruik is niet altijd mogelijk. De volgende tabel geeft aan dat de grenzen van het testen van Microsoft en geeft ook aan welke doelen vaste limieten zijn:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="azure-queue-storage-scale-targets"></a>Schaal prestatiedoelen voor Azure Queue storage

[!INCLUDE [storage-queues-scale-targets](../../../includes/storage-queues-scale-targets.md)]

## <a name="azure-table-storage-scale-targets"></a>Schaal prestatiedoelen voor Azure Table storage

[!INCLUDE [storage-table-scale-targets](../../../includes/storage-tables-scale-targets.md)]

## <a name="see-also"></a>Zie ook

- [Prijsinformatie voor opslag](https://azure.microsoft.com/pricing/details/storage/)
- [Azure-abonnement en Servicelimieten, Quotums en beperkingen](../../azure-subscription-service-limits.md)
- [Azure Storage-replicatie](../storage-redundancy.md)
- [Controlelijst voor prestaties en schaalbaarheid van Microsoft Azure Storage](../storage-performance-checklist.md)