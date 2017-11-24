---
title: Azure Storage Scalability and Performance Targets | Microsoft Docs
description: Meer informatie over de schaalbaarheids- en prestatiedoelen voor Azure Storage, met inbegrip van capaciteit en snelheid van aanvragen voor binnenkomende en uitgaande bandbreedte voor beide standard en premium storage-accounts. Begrijpen prestatiedoelen voor partities in elk van de Azure Storage-services.
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: be721bd3-159f-40a1-88c1-96418537fe75
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 10/24/2017
ms.author: tamram
ms.openlocfilehash: f62f2020d40e473886cb679cdfe1c164b95f7114
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="azure-storage-scalability-and-performance-targets"></a>Schaalbaarheids- en prestatiedoelen van Azure Storage
## <a name="overview"></a>Overzicht
Dit artikel wordt beschreven in de onderwerpen schaalbaarheid en prestaties voor Azure Storage. Zie voor een overzicht van andere Azure limieten [Azure-abonnement en Service-limieten, quota's en beperkingen](../../azure-subscription-service-limits.md).

> [!NOTE]
> Alle opslagaccounts worden uitgevoerd op de nieuwe platte netwerktopologie en ondersteunen de schaalbaarheids- en prestatiedoelen die worden beschreven in dit artikel, ongeacht wanneer ze zijn gemaakt. Zie voor meer informatie over de Azure Storage platte netwerkarchitectuur en schaalbaarheid [Microsoft Azure Storage: een maximaal beschikbare Cloudopslagservice met sterke consistentie](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).
> 

> [!IMPORTANT]
> De schaalbaarheids- en prestatiedoelen hier vermeld zijn geavanceerde doelen, maar worden uitgevoerd. In alle gevallen, de frequentie van aanvragen en bandbreedte door uw opslag account is afhankelijk van de grootte van de objecten die zijn opgeslagen, de toegangspatronen gebruikt, en het type werkbelasting van uw toepassing uitvoert. Zorg ervoor dat voor het testen van uw service om te bepalen of de prestaties aan uw vereisten voldoet. Indien mogelijk plotselinge pieken in de frequentie van verkeer te voorkomen en ervoor te zorgen dat verkeer goed gedistribueerde meerdere partities.
> 
> Wanneer uw toepassing heeft de limiet van wat een partitie voor uw workload verwerken kan bereikt, begint Azure Storage-foutcode 503 (Server bezet) of foutcode 500 (time-out voor de bewerking) antwoorden retourneren. Als deze fouten voordoen zich, kunnen uw toepassing moet een exponentieel uitstel-beleid gebruiken voor nieuwe pogingen. De exponentieel uitstel kan de belasting op de partitie te verkleinen en te vereenvoudigen uit pieken in het verkeer voor deze partitie.
> 
> 

Als de behoeften van uw toepassing de schaalbaarheidsdoelen van een enkele storage-account overschrijdt, kunt u uw toepassing gebruik van meerdere storage-accounts maken. U kunt vervolgens uw gegevensobjecten partitioneren over de storage-accounts. Zie [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/) voor meer informatie over Volumeprijzen.

## <a name="scalability-targets-for-a-storage-account"></a>Schaalbaarheidsdoelen voor een opslagaccount
[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Azure Blob storage scale doelen
[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Azure bestanden scale doelen
Zie voor meer informatie over de scale- en prestatiedoelen voor Azure-bestanden en Azure bestand Sync [Azure Files schaalbaarheids- en prestatiedoelen](../files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="azure-file-sync-scale-targets"></a>Azure bestand Sync scale doelen
[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="azure-queue-storage-scale-targets"></a>Azure Queue storage scale doelen
[!INCLUDE [storage-queues-scale-targets](../../../includes/storage-queues-scale-targets.md)]

## <a name="azure-table-storage-scale-targets"></a>Azure Table storage scale doelen
[!INCLUDE [storage-table-scale-targets](../../../includes/storage-tables-scale-targets.md)]

## <a name="see-also"></a>Zie ook
* [Opslag prijsgegevens](https://azure.microsoft.com/pricing/details/storage/)
* [Azure-abonnement en Servicelimieten, quota's en beperkingen](../../azure-subscription-service-limits.md)
* [Azure Storage-replicatie](../storage-redundancy.md)
* [Microsoft Azure Storage prestaties en schaalbaarheid controlelijst](../storage-performance-checklist.md)
* [Microsoft Azure Storage: Een maximaal beschikbare Cloudopslagservice met sterke consistentie](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

