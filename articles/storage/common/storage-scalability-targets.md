---
title: Azure Storage Scalability and Performance Targets | Microsoft Docs
description: Meer informatie over de schaalbaarheids- en prestatiedoelen voor Azure Storage, met inbegrip van capaciteit en snelheid van aanvragen voor binnenkomende en uitgaande bandbreedte voor beide standard en premium storage-accounts. Krijg inzicht in de prestatiedoelen voor partities binnen elk van de Azure Storage-services.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 10/24/2017
ms.author: rogarana
ms.component: common
ms.openlocfilehash: 248ec73b09c25c162c5bdc77ceb18e610d604b77
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44052170"
---
# <a name="azure-storage-scalability-and-performance-targets"></a>Schaalbaarheids- en prestatiedoelen van Azure Storage
## <a name="overview"></a>Overzicht
Dit artikel beschrijft de schaalbaarheids- en -onderwerpen voor Azure Storage. Zie voor een overzicht van andere Azure-limieten, [Azure-abonnement en Servicelimieten, Quotums en beperkingen](../../azure-subscription-service-limits.md).

> [!NOTE]
> Alle opslagaccounts worden uitgevoerd op de nieuwe platte netwerktopologie en ondersteuning voor de schaalbaarheids- en prestatiedoelen die worden beschreven in dit artikel, ongeacht wanneer ze zijn gemaakt. Zie voor meer informatie over de platte netwerkarchitectuur van Azure Storage en schaalbaarheid [Microsoft Azure Storage: een maximaal beschikbare Cloudopslagservice met sterke consistentie](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).
> 

> [!IMPORTANT]
> De schaalbaarheids- en prestatiedoelen die hier worden vermeld geavanceerde doelen zijn, maar worden uitgevoerd. In alle gevallen, de snelheid van aanvragen en bandbreedte met uw opslag account is afhankelijk van de grootte van de objecten die zijn opgeslagen, de patronen voor databasetoegang gebruikt, en het type werkbelasting van uw toepassing uitvoert. Zorg ervoor dat uw service om te bepalen of de prestaties, voldoet aan uw vereisten testen. Indien mogelijk, te voorkomen dat plotselinge pieken in de snelheid van verkeer en zorg ervoor dat verkeer goed gedistribueerd over meerdere partities.
> 
> Wanneer uw toepassing de limiet bereikt van wat een partitie voor uw werkbelasting kan verwerken, begint de Azure Storage om terug te keren foutcode 503 (Server bezet) of foutcode antwoorden van 500 (time-out van de bewerking). Als deze fouten voordoen zich, kunnen uw toepassing moet een beleid voor exponentieel uitstel gebruiken voor nieuwe pogingen. De exponentieel uitstel kan de belasting op de partitie te verlagen en om te vergemakkelijken van pieken in verkeer naar de betreffende partitie.
> 
> 

Als de behoeften van uw toepassing de schaalbaarheidsdoelen van een enkel opslagaccount overschrijdt, kunt u uw toepassing voor het gebruik van meerdere opslagaccounts maken. U kunt vervolgens uw gegevensobjecten partitioneren voor deze opslagaccounts. Zie [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/) voor meer informatie over Volumeprijzen.

## <a name="scalability-targets-for-a-storage-account"></a>Schaalbaarheidsdoelen voor een storage-account
[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Schaal prestatiedoelen voor Azure Blob storage
[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Azure bestanden schaal doelen
Zie voor meer informatie over de schaal- en prestatiedoelen voor Azure Files en Azure File Sync, [schaalbaarheids- en prestatiedoelen van Azure Files](../files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="azure-file-sync-scale-targets"></a>Azure File Sync schaal doelen
Met Azure File Sync, dat we hebben geprobeerd zo veel mogelijk te ontwerpen voor onbeperkt gebruik, maar dit niet altijd mogelijk is. De onderstaande tabel geeft aan dat de grenzen van onze tests en welke doelen daadwerkelijk vaste limieten zijn:

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
* [Microsoft Azure Storage: Een maximaal beschikbare Cloudopslagservice met sterke consistentie](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

