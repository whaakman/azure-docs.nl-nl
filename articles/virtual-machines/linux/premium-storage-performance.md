---
title: "Azure Premium Storage: Ontwerp voor prestaties op Windows-VM's | Microsoft Docs"
description: Ontwerp krachtige toepassingen met behulp van Azure Premium Storage. Premium Storage biedt ondersteuning voor schijven voor hoge prestaties en lage latentie voor I/O-intensieve werkbelastingen op Azure Virtual Machines.
services: virtual-machines-linux,storage
author: roygara
ms.service: virtual-machines-linux
ms.tgt_pltfrm: linux
ms.topic: article
ms.date: 06/27/2017
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: c6de3144a87a5bfad38e1b33148f292b26c0f181
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67658246"
---
[!INCLUDE [virtual-machines-common-premium-storage-introduction](../../../includes/virtual-machines-common-premium-storage-introduction.md)]

> [!NOTE]
> Soms is lijkt te zijn van een prestatieprobleem schijf eigenlijk een knelpunt netwerk. In deze situaties moet u optimaliseren uw [netwerkprestaties](../../virtual-network/virtual-network-optimize-network-bandwidth.md).
>
> Als u wilt het benchmarken van de schijf, Zie dan het artikel op [benchmarks van een schijf](disks-benchmarks.md).
>
> Als uw virtuele machine versnelde netwerken ondersteunt, moet u ervoor zorgen dat deze is ingeschakeld. Als deze niet is ingeschakeld, kunt u het inschakelen op reeds geïmplementeerde VM's op zowel [Windows](../../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms) en [Linux](../../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms).

Voordat u begint, als u niet bekend bent met Premium Storage, lees eerst de [Selecteer een type Azure-schijf voor IaaS-VM's](disks-types.md) en [schaalbaarheids- en prestatiedoelen voor Azure Storage voor storage-accounts](../../storage/common/storage-scalability-targets.md).


[!INCLUDE [virtual-machines-common-premium-storage-performance.md](../../../includes/virtual-machines-common-premium-storage-performance.md)]
