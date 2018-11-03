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
ms.date: 10/23/2018
ms.author: cynthn
ms.openlocfilehash: 373003eb8be0482ed96d7d11ecd879237f69b47a
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50957162"
---
# <a name="set-up-endpoints-on-a-windows-virtual-machine-by-using-the-classic-deployment-model"></a>Eindpunten instellen op een Windows-machine met behulp van het klassieke implementatiemodel
Windows virtuele machines (VM's) die u in Azure maakt met behulp van het klassieke implementatiemodel kunnen automatisch communiceren via een privénetwerkkanaal met andere virtuele machines in de dezelfde cloudservice of een virtueel netwerk. Computers op het internet of andere virtuele netwerken moeten echter eindpunten om te leiden van het binnenkomende netwerkverkeer op een virtuele machine. 

U kunt ook eindpunten instellen op [virtuele Linux-machines](../../linux/classic/setup-endpoints.md).

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../../../resource-manager-deployment-model.md). Dit artikel is van toepassing op het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.  
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

In de **Resource Manager** implementatiemodel eindpunten zijn geconfigureerd met behulp van **Netwerkbeveiligingsgroepen (nsg's)**. Zie voor meer informatie, [toestaan van externe toegang tot uw virtuele machine met behulp van de Azure-portal](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Wanneer u een virtuele Windows-machine in Azure portal maakt, zijn algemene eindpunten, zoals eindpunten voor extern bureaublad- en Windows PowerShell voor externe toegang, meestal automatisch voor u gemaakt. U kunt later naar behoefte extra eindpunten configureren.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Volgende stappen
* Zie voor het gebruik van een Azure PowerShell-cmdlet voor het instellen van een VM-eindpunt, [Add-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).
* Zie voor het gebruik van een Azure PowerShell-cmdlet voor het beheren van een ACL op een eindpunt, [beheren toegangsbeheerlijsten (ACL's) voor eindpunten met behulp van PowerShell](../../../virtual-network/virtual-networks-acl-powershell.md).
* Als u een virtuele machine hebt gemaakt in het Resource Manager-implementatiemodel, kunt u Azure PowerShell te gebruiken [netwerkbeveiligingsgroepen maken](../../../virtual-network/tutorial-filter-network-traffic.md) naar verkeer beheren met de virtuele machine.
