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
ms.date: 11/08/2017
ms.author: jonbeck
ms.openlocfilehash: 6650bb2620f43053836a49f39337852d94d8dbc5
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
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