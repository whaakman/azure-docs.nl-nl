---
title: Azure Linux VM-groottes - GPU | Microsoft Docs
description: Een lijst met de verschillende GPU geoptimaliseerd grootten beschikbaar voor virtuele Linux-machines in Azure. Bevat informatie over het aantal Vcpu, gegevensschijven en NIC's, evenals opslag doorvoer en bandbreedte voor de grootte van deze reeks.
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jonbeck
ms.openlocfilehash: a1057cd901725f2c8ad209db507a5bb9fdf33e77
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2018
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>GPU geoptimaliseerd grootten van virtuele machines

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

Zie voor stuurprogramma-installatie- en verificatiestappen stappen [N-reeks stuurprogramma-instellingen voor Linux](n-series-driver-setup.md).

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* U mag niet X installeren server of andere systemen die gebruikmaken van de `Nouveau` stuurprogramma op Ubuntu NC-virtuele machines. Voordat u NVIDIA GPU-stuurprogramma's installeert, moet u uitschakelen de `Nouveau` stuurprogramma.  

## <a name="other-sizes"></a>Andere grootten
- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerde rekenkracht](sizes-compute.md)
- [Geoptimaliseerd geheugen](sizes-memory.md)
- [Geoptimaliseerde opslag](sizes-storage.md)
- [Krachtig rekenvermogen](sizes-hpc.md)

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [Azure compute-eenheden (ACU)](acu.md) kunt u de prestaties van Azure-SKU's met elkaar vergelijken.