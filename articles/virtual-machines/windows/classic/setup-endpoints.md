---
title: Eindpunten instellen op een klassieke Windows-VM | Microsoft Docs
description: Meer informatie over het instellen van eindpunten voor een klassieke Windows-VM in Azure portal voor de communicatie met een Windows-machine in Azure.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 8afc21c2-d3fb-43a3-acce-aa06be448bb6
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: cca9adb40557cf7bf9e1d4129fc6bd61cbf0df4f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38618236"
---
# <a name="how-to-set-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Over het instellen van eindpunten op een klassieke Windows-machine in Azure
Alle Windows virtuele machines die u maakt in Azure met behulp van het klassieke implementatiemodel automatisch kunnen communiceren via een particulier netwerkkanaal met andere virtuele machines in de dezelfde cloudservice of een virtueel netwerk. Computers op het Internet of andere virtuele netwerken moeten echter eindpunten om te leiden van het binnenkomende netwerkverkeer op een virtuele machine. In dit artikel is ook beschikbaar voor [virtuele Linux-machines](../../linux/classic/setup-endpoints.md).

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

In de **Resource Manager** implementatiemodel-eindpunten worden geconfigureerd met behulp van **Netwerkbeveiligingsgroepen (nsg's)**. Zie voor meer informatie, [toestaan van externe toegang tot uw virtuele machine via de Azure-portal](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Wanneer u een Windows-machine in Azure portal maakt, zijn algemene eindpunten, zoals die voor extern bureaublad- en Windows PowerShell voor externe toegang meestal automatisch voor u gemaakt. U kunt extra eindpunten configureren tijdens het maken van de virtuele machine of daarna indien nodig.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Volgende stappen
* Zie voor het gebruik van een Azure PowerShell-cmdlet voor het instellen van een VM-eindpunt, [Add-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).
* Zie voor het gebruik van een Azure PowerShell-cmdlet voor het beheren van een ACL op een eindpunt, [beheren toegangsbeheerlijsten (ACL's) voor eindpunten met behulp van PowerShell](../../../virtual-network/virtual-networks-acl-powershell.md).
* Als u een virtuele machine hebt gemaakt in het Resource Manager-implementatiemodel, kunt u Azure PowerShell te gebruiken [netwerkbeveiligingsgroepen maken](../../../virtual-network/tutorial-filter-network-traffic.md) naar verkeer beheren met de virtuele machine.
