---
title: "Azure Premium Storage: Ontwerpen voor prestaties op Windows-Vm's | Microsoft Docs"
description: Ontwerp krachtige toepassingen met Azure Premium Storage. Premium Storage biedt schijf ondersteuning met hoge prestaties en lage latentie voor I/O-intensieve workloads die worden uitgevoerd op Azure Virtual Machines.
author: roygara
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.date: 06/27/2017
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 70da3509ce44fe260f8010ccf6eb1d2192ca6e73
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68695501"
---
[!INCLUDE [virtual-machines-common-premium-storage-introduction](../../../includes/virtual-machines-common-premium-storage-introduction.md)]

> [!NOTE]
> Soms is het een probleem met de prestaties van een schijf. In deze situaties moet u de [netwerk prestaties](../../virtual-network/virtual-network-optimize-network-bandwidth.md)optimaliseren.
>
> Als u een bench Mark-schijf wilt maken, raadpleegt u ons artikel over de benchmarking van [een schijf](disks-benchmarks.md).
>
> Als uw virtuele machine versneld netwerken ondersteunt, moet u ervoor zorgen dat deze is ingeschakeld. Als deze optie niet is ingeschakeld, kunt u deze inschakelen op al geïmplementeerde Vm's in [Windows](../../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms) en [Linux](../../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms).

Voordat u begint Premium Storage, lees dan eerst de [Select a Azure-schijf type voor IaaS vm's](disks-types.md) en [Azure Storage schaal baarheid en prestatie doelen voor opslag accounts](../../storage/common/storage-scalability-targets.md).


[!INCLUDE [virtual-machines-common-premium-storage-performance.md](../../../includes/virtual-machines-common-premium-storage-performance.md)]
