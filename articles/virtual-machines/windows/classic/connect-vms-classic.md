---
title: Verbinding maken met Windows-VM's in een cloudservice | Microsoft Docs
description: Verbinding maken met Windows virtuele machines is gemaakt met het klassieke implementatiemodel met een Azure-cloudservice of virtuele netwerk.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: c1cbc802-4352-4d2e-9e49-4ccbd955324b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: cynthn
ms.openlocfilehash: f30c111afa31db9bec16ba5a9ad46ea20d64f285
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="connect-windows-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Verbinding maken met Windows virtuele machines is gemaakt met het klassieke implementatiemodel met een virtueel netwerk of cloud-service
> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

Windows virtuele machines is gemaakt met het klassieke implementatiemodel worden altijd geplaatst in een cloudservice. De cloudservice fungeert als een container en biedt een unieke openbare DNS-naam, een openbare IP-adres en een set eindpunten voor toegang tot de virtuele machine via het Internet. De cloudservice kan zich in een virtueel netwerk, maar is geen vereiste.

Als een cloudservice niet in een virtueel netwerk, er sprake een *zelfstandige* cloudservice. De virtuele machines in een zelfstandige cloudservice communiceren met andere virtuele machines met behulp van de andere virtuele machines de openbare DNS-namen, en het verkeer wordt verzonden via Internet. Als een service in de cloud zich in een virtueel netwerk, kan de virtuele machines in de cloudservice zonder verkeer verzenden via Internet communiceren met andere virtuele machines in het virtuele netwerk.

Als u uw virtuele machines in dezelfde zelfstandige cloudservice plaatst, kunt u nog steeds gebruiken voor taakverdeling en beschikbaarheidssets. Zie voor meer informatie [taakverdeling van virtuele machines](../../virtual-machines-windows-load-balance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) en [de beschikbaarheid van virtuele machines beheren](../../virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). U kan echter ordenen van de virtuele machines op subnetten of een zelfstandige cloudservice verbinding met uw on-premises netwerk. Hier volgt een voorbeeld:

[!INCLUDE [virtual-machines-common-classic-connect-vms](../../../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Volgende stappen
Nadat u een virtuele machine maakt, is het een goed idee om [een gegevensschijf toevoegen](attach-disk.md) zodat uw services en werkbelastingen hebt een locatie voor het opslaan van gegevens.
