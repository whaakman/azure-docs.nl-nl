---
title: Een klassieke Linux virtuele machine maken met de Azure CLI 1.0 | Microsoft Docs
description: Informatie over het maken van een virtuele Linux-machine met de Azure CLI 1.0 met het klassieke implementatiemodel
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: f8071a2e-ed91-4f77-87d9-519a37e5364f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 6d3f0dd0c82ad32df4d6e17058d9b1bea57c301f
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
ms.locfileid: "30841432"
---
# <a name="how-to-create-a-classic-linux-vm-with-the-azure-cli-10"></a>Een klassieke virtuele Linux-machine maken met de Azure CLI 1.0
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Zie voor de versie van het Resource Manager, [hier](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Dit onderwerp beschrijft het maken van virtuele Linux-machine (VM) met de Azure CLI 1.0 met het klassieke implementatiemodel. We gebruiken een Linux-installatiekopie uit de beschikbare **INSTALLATIEKOPIEËN** op Azure. De Azure CLI 1.0-opdrachten geven de volgende configuratieopties, onder andere:

* De virtuele machine verbinden met een virtueel netwerk
* De virtuele machine toe te voegen aan een bestaande cloudservice
* De virtuele machine toe te voegen aan een bestaand opslagaccount
* De virtuele machine toe te voegen aan een beschikbaarheidsset of locatie

> [!IMPORTANT]
> Als u uw VM wilt voor gebruik van een virtueel netwerk, zodat u verbinding met het rechtstreeks door de hostnaam of het instellen van de cross-premises verbindingen maken kunt, zorg er dan voor dat u het virtuele netwerk opgeven wanneer u de virtuele machine maken. Een virtuele machine kan worden geconfigureerd voor een virtueel netwerk toe te voegen alleen als u de virtuele machine maken. Zie voor meer informatie over virtuele netwerken, [Azure Virtual Network-overzicht](http://go.microsoft.com/fwlink/p/?LinkID=294063).
> 
> 

## <a name="how-to-create-a-linux-vm-using-the-classic-deployment-model"></a>Het maken van een Linux-VM met het klassieke implementatiemodel
[!INCLUDE [virtual-machines-create-LinuxVM](../../../../includes/virtual-machines-create-linuxvm.md)]

