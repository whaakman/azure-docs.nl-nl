---
title: Eindpunten instellen op een klassieke Linux VM | Microsoft Docs
description: Meer informatie over het instellen van eindpunten voor een Linux-VM in Azure portal voor de communicatie met een virtuele Linux-machine in Azure
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: f3749738-1109-4a1d-8635-40e4bd220e91
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: 03cb90dcdcc7a7407898ddd8cbc30f1af0833676
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38295684"
---
# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Over het instellen van eindpunten op een Linux klassieke virtuele machine in Azure
Alle Linux virtuele machines die u maakt in Azure met behulp van het klassieke implementatiemodel kunnen automatisch communiceren via een privénetwerkkanaal met andere virtuele machines in de dezelfde cloudservice of een virtueel netwerk. Computers op het Internet of andere virtuele netwerken moeten echter eindpunten om te leiden van het binnenkomende netwerkverkeer op een virtuele machine. In dit artikel is ook beschikbaar voor [Windows virtuele machines](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

In de **Resource Manager** implementatiemodel-eindpunten worden geconfigureerd met behulp van **Netwerkbeveiligingsgroepen (nsg's)**. Zie voor meer informatie, [poorten en eindpunten openen](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Wanneer u een virtuele Linux-machine in Azure portal maakt, wordt een eindpunt voor Secure Shell (SSH) doorgaans automatisch voor u gemaakt. U kunt extra eindpunten configureren tijdens het maken van de virtuele machine of daarna indien nodig.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Volgende stappen
* U kunt ook een VM-eindpunt maken met behulp van de [Azure Command-Line Interface](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). Voer de **azure vm-eindpunt maken** opdracht.
* Als u een virtuele machine hebt gemaakt in het Resource Manager-implementatiemodel, kunt u de Azure CLI in Resource Manager-modus voor [netwerkbeveiligingsgroepen maken](../../../virtual-network/tutorial-filter-network-traffic-cli.md) naar verkeer beheren met de virtuele machine.
