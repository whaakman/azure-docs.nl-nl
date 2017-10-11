---
title: Eindpunten van een klassieke Linux VM instellen | Microsoft Docs
description: Informatie over het instellen van eindpunten voor een Linux-VM in de klassieke Azure portal voor de communicatie met een virtuele Linux-machine in Azure
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: f3749738-1109-4a1d-8635-40e4bd220e91
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: 4fd8b847b0f60648d1661ce5a8667c641e616ed4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Het instellen van eindpunten op een Linux klassieke virtuele machine in Azure
Alle Linux virtuele machines die u maakt in Azure met behulp van het klassieke implementatiemodel kunnen automatisch via een particulier netwerkkanaal met andere virtuele machines in dezelfde cloudservice- of virtueel netwerk communiceren. Computers op Internet of een andere virtuele netwerken vereisen echter eindpunten het binnenkomend netwerkverkeer naar een virtuele machine wordt omgeleid. In dit artikel is ook beschikbaar voor [Windows virtuele machines](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

In de **Resource Manager** implementatiemodel eindpunten zijn geconfigureerd met behulp van **Netwerkbeveiligingsgroepen (nsg's)**. Zie voor meer informatie [openen van poorten en eindpunten](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Wanneer u een virtuele Linux-machine in de Azure portal maakt, wordt een eindpunt voor Secure Shell (SSH) doorgaans automatisch voor u gemaakt. Tijdens het maken van de virtuele machine of daarna indien nodig, kunt u extra eindpunten configureren.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Volgende stappen
* U kunt ook een VM-eindpunt maken met behulp van de [Azure-opdrachtregelinterface](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). Voer de **azure vm-eindpunt maken** opdracht.
* Als u een virtuele machine hebt gemaakt in het Resource Manager-implementatiemodel, kunt u de Azure CLI gebruiken in de modus Resource Manager voor [netwerk beveiligingsgroepen maken](../../../virtual-network/virtual-networks-create-nsg-arm-cli.md) aan verkeer voor beheer met de virtuele machine.
