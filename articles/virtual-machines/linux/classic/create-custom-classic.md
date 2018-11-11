---
title: Een klassieke virtuele Linux-machine met de klassieke Azure-CLI maken | Microsoft Docs
description: Meer informatie over het maken van een virtuele Linux-machine met de Azure klassieke CLI met behulp van het klassieke implementatiemodel
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
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
ms.author: cynthn
ms.openlocfilehash: d8e469289f72fe892ea7c3da99972e6326c75eb9
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51242533"
---
# <a name="how-to-create-a-classic-linux-vm-with-the-azure-classic-cli"></a>Een klassieke virtuele Linux-machine maken met de klassieke Azure-CLI
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Zie voor de versie van het Resource Manager, [hier](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

In dit onderwerp beschrijft hoe u een Linux-machine (VM) maken met de Azure klassieke CLI met behulp van het klassieke implementatiemodel. Gebruiken we een Linux-installatiekopie uit de beschikbare **INSTALLATIEKOPIEËN** op Azure. De Azure CLI-opdrachten klassieke geven de volgende configuratie-opties, onder andere:

* De VM wordt verbonden met een virtueel netwerk
* De virtuele machine toe te voegen aan een bestaande cloudservice
* De virtuele machine toe te voegen aan een bestaand opslagaccount
* De virtuele machine toe te voegen aan een beschikbaarheidsset of een locatie

> [!IMPORTANT]
> Als u wilt dat uw virtuele machine gebruik van een virtueel netwerk, zodat u verbinding met deze rechtstreeks door de hostnaam of het instellen van cross-premises verbindingen maken kunt, zorg ervoor dat u het virtuele netwerk opgeeft bij het maken van de virtuele machine. Een virtuele machine kan worden geconfigureerd als u wilt deelnemen aan een virtueel netwerk alleen wanneer u de virtuele machine maakt. Zie voor meer informatie over virtuele netwerken, [Azure Virtual Network Overview](https://go.microsoft.com/fwlink/p/?LinkID=294063).
> 
> 

## <a name="how-to-create-a-linux-vm-using-the-classic-deployment-model"></a>Over het maken van een Linux-VM met behulp van het klassieke implementatiemodel
[!INCLUDE [virtual-machines-create-LinuxVM](../../../../includes/virtual-machines-create-linuxvm.md)]

