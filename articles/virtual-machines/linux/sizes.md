---
title: Linux-VM-groottes in Azure | Microsoft Docs
description: Hier worden de verschillende grootten beschikbaar voor virtuele Linux-machines in Azure.
services: virtual-machines-linux
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: da681171-f045-4c80-a5a9-d8bd47964673
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jonbeck
ms.openlocfilehash: 2480a48670646d13f55b405d2594c5937d1b61ba
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a>Grootten voor virtuele Linux-machines in Azure
In dit artikel beschrijft de beschikbare grootten en opties voor de virtuele machines van Azure die kunt u uw Linux-apps en werkbelastingen worden uitgevoerd. Het biedt tevens overwegingen voor de implementatie moet letten wanneer u van plan bent om deze bronnen te gebruiken. In dit artikel is ook beschikbaar voor [Windows virtuele machines](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


| Type                     | Grootten           |    Beschrijving       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Algemeen doel](sizes-general.md)          | B, Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7  | Evenwichtige CPU-geheugenverhouding. Ideaal voor testen en ontwikkelen, kleine tot middelgrote databases en webservers met weinig of gemiddeld verkeer. |
| [Geoptimaliseerde rekenkracht](sizes-compute.md)        | Fsv2, Fs, F             | Hoge CPU-geheugenverhouding. Goed voor webservers met gemiddeld verkeer, netwerkapparatuur, batchprocessen en toepassingsservers.        |
| [Geoptimaliseerd geheugen](sizes-memory.md)         | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Verhouding tussen het hoge geheugen aan CPU. Uiterst geschikt voor relationele-databaseservers, middelgrote tot grote caches en analysefuncties in het geheugen.                 |
| [Geoptimaliseerde opslag](sizes-storage.md)        | Ls                | Snelle doorvoer van schijfgegevens en IO. Ideaal voor big data-, SQL- en NoSQL-databases.                                                         |
| [GPU](sizes-gpu.md)            | NV, NC, NCv2, NCv3, ND            | Gespecialiseerde virtuele machines gericht voor zware grafische weergave en het bewerken van video's, evenals model training en inferencing (ND) met grondige learning. Beschikbaar met één of meerdere GPU's.       |
| [Krachtig rekenvermogen](sizes-hpc.md) | H, A8-11          | Onze snelste en krachtigste CPU-virtuele machines met optionele netwerkinterfaces (RDMA) voor hoge doorvoer. 

<br>

- Zie voor meer informatie over prijzen van de verschillende grootten [prijzen van virtuele Machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux). 
- Zie voor de beschikbaarheid van VM-formaten in Azure-regio's, [producten die beschikbaar zijn in elke regio](https://azure.microsoft.com/regions/services/).
- Zie voor algemene limieten op Azure Virtual machines [Azure-abonnement en Servicelimieten, quota's en beperkingen](../../azure-subscription-service-limits.md).
- Meer informatie over het [Azure compute-eenheden (ACU)](acu.md) kunt u de prestaties van Azure-SKU's met elkaar vergelijken.


## <a name="rest-api"></a>REST-API

Zie de volgende onderwerpen voor informatie over het gebruik van de REST-API aan query voor VM-grootten:

- [Lijst met beschikbare virtuele machine grootten voor het vergroten of verkleinen](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-for-resizing)
- [Lijst met beschikbare virtuele machine grootten voor een abonnement](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region)
- [Grootte van de lijst met beschikbare virtuele machines in een beschikbaarheidsset](
https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-availability-set)

## <a name="acu"></a>ACU

Meer informatie over het [Azure compute-eenheden (ACU)](acu.md) kunt u de prestaties van Azure-SKU's met elkaar vergelijken.

## <a name="benchmark-scores"></a>Benchmarkscores

Meer informatie over de prestaties voor het gebruik van virtuele Linux-machines compute de [CoreMark benchmark scores](compute-benchmark-scores.md).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de verschillende grootten voor virtuele machine die beschikbaar zijn:
- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerde rekenkracht](sizes-compute.md)
- [Geoptimaliseerd geheugen](sizes-memory.md)
- [Geoptimaliseerde opslag](sizes-storage.md)
- [GPU](sizes-gpu.md)
- [Krachtig rekenvermogen](sizes-hpc.md)



