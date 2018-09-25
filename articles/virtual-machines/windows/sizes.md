---
title: Windows-VM-grootten in Azure | Microsoft Docs
description: Geeft een lijst van de verschillende grootten beschikbaar voor Windows virtuele machines in Azure.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: aabf0d30-04eb-4d34-b44a-69f8bfb84f22
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: jonbeck
ms.openlocfilehash: 47253fd05cb1df96841b30357ac6e7cfe75c12c5
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039681"
---
# <a name="sizes-for-windows-virtual-machines-in-azure"></a>Grootten voor Windows virtuele machines in Azure

Dit artikel beschrijft de beschikbare grootten en opties voor de virtuele machines van Azure die kunt u uw Windows-apps en workloads uit te voeren. Het biedt ook overwegingen voor de implementatie moet letten wanneer u van plan bent om deze resources te gebruiken.  In dit artikel is ook beschikbaar voor [virtuele Linux-machines](../linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


| Type                     | Grootten           |    Beschrijving       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Algemeen doel](sizes-general.md)          | B, Dsv3, Dv3, DSv2, Dv2, Av2 | Evenwichtige CPU-geheugenverhouding. Ideaal voor testen en ontwikkelen, kleine tot middelgrote databases en webservers met weinig of gemiddeld verkeer. |
| [Geoptimaliseerde rekenkracht](sizes-compute.md)        | Fsv2, Fs-F             | Hoge CPU-geheugenverhouding. Goed voor webservers met gemiddeld verkeer, netwerkapparatuur, batchprocessen en toepassingsservers.        |
| [Geoptimaliseerd geheugen](../virtual-machines-windows-sizes-memory.md)         | Esv3, Ev3, M, GS, G, DSv2-, Dv2  | Hoge geheugen-naar-CPU-verhouding. Uiterst geschikt voor relationele-databaseservers, middelgrote tot grote caches en analysefuncties in het geheugen.                 |
| [Geoptimaliseerde opslag](../virtual-machines-windows-sizes-storage.md)        | Ls                | Snelle doorvoer van schijfgegevens en IO. Ideaal voor big data-, SQL- en NoSQL-databases.                                                         |
| [GPU](sizes-gpu.md)            | NV, NVv2, NC, NCv2, NCv3, ND            | Gespecialiseerde virtuele machines bedoeld voor intensieve grafische rendering en videobewerking, evenals model trainings- en inferentietaken (ND) met deep learning. Beschikbaar met één of meerdere GPU's.       |
| [Krachtig rekenvermogen](sizes-hpc.md) | H       | Onze snelste en krachtigste CPU-virtuele machines met optionele netwerkinterfaces (RDMA) voor hoge doorvoer. 


<br> 

- Zie voor meer informatie over de prijzen van de verschillende grootten [prijzen van virtuele Machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows). 
- Zie voor algemene limieten op Azure Virtual machines [Azure-abonnement en Servicelimieten, quotums en beperkingen](../../azure-subscription-service-limits.md).
- Opslagkosten worden afzonderlijk berekend op basis van het aantal pagina's dat in het opslagaccount is gebruikt. Voor meer informatie, [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/).
- Meer informatie over hoe u [Azure compute units (ACU)](acu.md) kunt u de prestaties van Azure-SKU's met elkaar vergelijken.


## <a name="rest-api"></a>REST-API

Zie de volgende onderwerpen voor meer informatie over het gebruik van de REST-API voor query's voor VM-grootten:

- [Grootten van de lijst met beschikbare virtuele machine voor het vergroten of verkleinen](https://docs.microsoft.com/rest/api/compute/virtualmachines/listavailablesizes)
- [Lijst met beschikbare VM-grootten voor een abonnement](https://docs.microsoft.com/rest/api/compute/virtualmachinesizes/list)
- [Grootte van de lijst met beschikbare virtuele machine in een beschikbaarheidsset](https://docs.microsoft.com/rest/api/compute/availabilitysets/listavailablesizes)

## <a name="acu"></a>ACU

Meer informatie over hoe u [Azure compute units (ACU)](acu.md) kunt u de prestaties van Azure-SKU's met elkaar vergelijken.

## <a name="benchmark-scores"></a>Benchmarkscores

Meer informatie over compute-prestaties voor het gebruik van Windows-VM's de [CoreMark benchmarkscores](compute-benchmark-scores.md).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de verschillende VM-grootten die beschikbaar zijn:
- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerde rekenkracht](sizes-compute.md)
- [Geoptimaliseerd geheugen](../virtual-machines-windows-sizes-memory.md)
- [Geoptimaliseerde opslag](../virtual-machines-windows-sizes-storage.md)
- [Geoptimaliseerde GPU](sizes-gpu.md)
- [Krachtig rekenvermogen](sizes-hpc.md)
- Controleer de [vorige generatie](sizes-previous-gen.md) -pagina voor A Standard, Dv1 (D1-4 en D11-14 v1), en A8-A11-serie




