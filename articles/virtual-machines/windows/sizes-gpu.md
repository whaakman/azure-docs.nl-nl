---
title: Azure Windows VM-grootten - GPU | Microsoft Docs
description: Een lijst met de verschillende GPU geoptimaliseerd grootten beschikbaar voor Windows virtuele machines in Azure. Bevat informatie over het aantal vcpu's, gegevensschijven en NIC's, evenals opslag en doorvoer netwerkbandbreedte voor grootten die in deze reeks.
services: virtual-machines-windows
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/06/2018
ms.author: jonbeck
ms.openlocfilehash: ab3ccd981514225a003b07bafbb73db11aaa21e2
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37903727"
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>GPU-geoptimaliseerde VM-grootten

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Ondersteunde besturingssystemen en stuurprogramma’s

Als u wilt profiteren van de GPU-mogelijkheden van Azure uit de N-serie VM's waarop Windows wordt uitgevoerd, moeten de NVIDIA GPU-stuurprogramma's worden geïnstalleerd. De [NVIDIA GPU-stuurprogramma extensie](../extensions/hpccompute-gpu-windows.md) juiste NVIDIA CUDA- of GRID stuurprogramma's worden geïnstalleerd op een VM uit de N-serie. Installeren of beheren van de extensie met de Azure portal of hulpprogramma's, zoals Azure PowerShell of Azure Resource Manager-sjablonen. Zie de [NVIDIA GPU-stuurprogramma extensie documentatie](../extensions/hpccompute-gpu-windows.md) voor ondersteunde besturingssystemen en implementatiestappen. Raadpleeg voor algemene informatie over VM-extensies [virtuele machine van Azure-extensies en functies](../extensions/overview.md).

Als u kiest voor het handmatig installeren van NVIDIA GPU-stuurprogramma's, Zie [N-serie GPU-stuurprogramma-instellingen voor Windows](n-series-driver-setup.md) voor ondersteunde besturingssystemen, stuurprogramma's en installatie-en verificatiestappen.

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

## <a name="other-sizes"></a>Andere grootten
- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerde rekenkracht](sizes-compute.md)
- [Krachtig rekenvermogen](sizes-hpc.md)
- [Geoptimaliseerd geheugen](sizes-memory.md)
- [Geoptimaliseerde opslag](sizes-storage.md)
- [Vorige generaties](sizes-previous-gen.md)

## <a name="next-steps"></a>Volgende stappen
Meer informatie over hoe u [Azure compute units (ACU)](acu.md) kunt u de prestaties van Azure-SKU's met elkaar vergelijken.

