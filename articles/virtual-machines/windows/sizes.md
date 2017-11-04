---
title: Grootten van virtuele Windows-machine in Azure | Microsoft Docs
description: Hier worden de verschillende grootten beschikbaar voor Windows virtuele machines in Azure.
services: virtual-machines-windows
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: aabf0d30-04eb-4d34-b44a-69f8bfb84f22
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/07/2017
ms.author: jonbeck
ms.openlocfilehash: 234d32a7f690b3bfd79a30f1cd0fd355da8fd7b3
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="sizes-for-windows-virtual-machines-in-azure"></a>Grootten voor Windows virtuele machines in Azure

In dit artikel beschrijft de beschikbare grootten en opties voor de virtuele machines van Azure die kunt u uw Windows-apps en werkbelastingen worden uitgevoerd. Het biedt tevens overwegingen voor de implementatie moet letten wanneer u van plan bent om deze bronnen te gebruiken.  In dit artikel is ook beschikbaar voor [virtuele Linux-machines](../linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


| Type                     | Grootten           |    Beschrijving       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Algemeen doel](sizes-general.md)          | B (Preview), Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0 7 | Evenwichtige CPU-geheugenverhouding. Ideaal voor testen en ontwikkelen, kleine tot middelgrote databases en webservers met weinig of gemiddeld verkeer. |
| [Geoptimaliseerde rekenkracht](sizes-compute.md)        | Fsv2, Fs, F             | Hoge CPU-geheugenverhouding. Goed voor webservers met gemiddeld verkeer, netwerkapparatuur, batchprocessen en toepassingsservers.        |
| [Geoptimaliseerd geheugen](../virtual-machines-windows-sizes-memory.md)         | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Verhouding tussen het hoge geheugen aan CPU. Uiterst geschikt voor relationele-databaseservers, middelgrote tot grote caches en analysefuncties in het geheugen.                 |
| [Geoptimaliseerde opslag](../virtual-machines-windows-sizes-storage.md)        | Ls                | Snelle doorvoer van schijfgegevens en IO. Ideaal voor big data-, SQL- en NoSQL-databases.                                                         |
| [GPU](sizes-gpu.md)            | NV, NC            | Gespecialiseerde virtuele machines gericht voor zware grafische weergave en het bewerken van video's. Beschikbaar met één of meerdere GPU's.       |
| [Krachtig rekenvermogen](sizes-hpc.md) | H, A8 11          | Onze snelste en krachtigste CPU-virtuele machines met optionele netwerkinterfaces (RDMA) voor hoge doorvoer. 

<br> 

- Zie voor meer informatie over prijzen van de verschillende grootten [prijzen van virtuele Machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows). 
- Zie voor algemene limieten op Azure Virtual machines [Azure-abonnement en Servicelimieten, quota's en beperkingen](../../azure-subscription-service-limits.md).
- Opslagkosten worden afzonderlijk berekend op basis van het aantal pagina's dat in het opslagaccount is gebruikt. Voor meer informatie [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/).
- Meer informatie over het [Azure compute-eenheden (ACU)](acu.md) kunt u de prestaties van Azure-SKU's met elkaar vergelijken.



## <a name="rest-api"></a>Rest-API

Zie de volgende onderwerpen voor informatie over het gebruik van de REST-API aan query voor VM-grootten:

- [Lijst met beschikbare virtuele machine grootten voor het vergroten of verkleinen](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-for-resizing)
- [Lijst met beschikbare virtuele machine grootten voor een abonnement](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region)
- [Grootte van de lijst met beschikbare virtuele machines in een beschikbaarheidsset](
https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-availability-set)

## <a name="acu"></a>ACU

Meer informatie over het [Azure compute-eenheden (ACU)](acu.md) kunt u de prestaties van Azure-SKU's met elkaar vergelijken.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de verschillende grootten voor virtuele machine die beschikbaar zijn:
- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerde rekenkracht](sizes-compute.md)
- [Geoptimaliseerd geheugen](../virtual-machines-windows-sizes-memory.md)
- [Geoptimaliseerde opslag](../virtual-machines-windows-sizes-storage.md)
- [Geoptimaliseerde GPU](sizes-gpu.md)
- [Krachtig rekenvermogen](sizes-hpc.md)



