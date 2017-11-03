---
title: "Selecteer Windows VM-installatiekopieën in Azure | Microsoft Docs"
description: "Informatie over het gebruik van Azure PowerSHell om te bepalen van de uitgever, aanbieding, SKU en versie voor Marketplace-VM-installatiekopieën."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/12/2017
ms.author: danlep
ms.openlocfilehash: c9b35ff5f3fbd33639805b5a4f105df32562a691
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a>Het zoeken van installatiekopieën van virtuele Windows-machine in Azure Marketplace met Azure PowerShell

In dit onderwerp wordt beschreven hoe u met Azure PowerShell VM-installatiekopieën vinden in Azure Marketplace. Deze informatie gebruiken om op te geven van een Marketplace-installatiekopie bij het maken van een virtuele machine van Windows.

Zorg ervoor dat u geïnstalleerd en geconfigureerd, de meest recente [Azure PowerShell-module](/powershell/azure/install-azurerm-ps).



## <a name="table-of-commonly-used-windows-images"></a>Tabel met veelgebruikte Windows-installatiekopieën
| PublisherName | Aanbieding | Sku |
|:--- |:--- |:--- |:--- |
| MicrosoftWindowsServer |WindowsServer |2016 Datacenter |
| MicrosoftWindowsServer |WindowsServer |2016 Datacenter-Server Core |
| MicrosoftWindowsServer |WindowsServer |2016 Datacenter met Containers |
| MicrosoftWindowsServer |WindowsServer |2016-Nano-Server |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2008 R2 SP1 |
| MicrosoftDynamicsNAV |DynamicsNAV |2017 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2016 |
| MicrosoftSQLServer |SQL2016 WS2016 |Enterprise |
| MicrosoftSQLServer |SQL2014SP2 WS2012R2 |Enterprise |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |

## <a name="find-specific-images"></a>Specifieke installatiekopieën zoeken


Bij het maken van een nieuwe virtuele machine met Azure Resource Manager geeft u in bepaalde gevallen een installatiekopie op in de vorm van een combinatie van de volgende installatiekopie-eigenschappen:

* Uitgever
* Aanbieding
* SKU

Gebruik bijvoorbeeld deze waarden met de [Set AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) PowerShell-cmdlet of met een resource groep sjabloon waarin moet u het type van de virtuele machine moet worden gemaakt.

Als u deze waarden bepalen moet, kunt u uitvoeren de [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), en [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) cmdlets om de afbeeldingen te bladeren. U bepalen deze waarden:

1. Geef de uitgevers van installatiekopieën weer.
2. Geef de aanbiedingen voor een bepaalde uitgever weer.
3. Geef de SKU's voor een bepaalde aanbieding weer.

Geef eerst de uitgevers weer met de volgende opdrachten:

```azurepowershell-interactive
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Vul de naam van de gewenste uitgever in en voer de volgende opdrachten uit:

```azurepowershell-interactive
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Vul de naam van de gewenste aanbieding in en voer de volgende opdrachten uit:

```azurepowershell-interactive
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Vanuit de uitvoer van de `Get-AzureRMVMImageSku` opdracht, hebt u alle informatie die u nodig hebt om op te geven van de afbeelding voor een nieuwe virtuele machine.

Hier volgt een compleet voorbeeld:

```azurepowershell-interactive
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

Uitvoer:

```
PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

Voor de uitgever MicrosoftWindowsServer:

```azurepowershell-interactive
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Uitvoer:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServer-HUB
```

Voor de aanbieding WindowsServer:

```azurepowershell-interactive
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Uitvoer:

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Nano-Server
```

Kopieer de naam van de gewenste SKU uit deze lijst. Nu hebt u alle benodigde gegevens voor de PowerShell-cmdlet `Set-AzureRMVMSourceImage` of voor een resourcegroepsjabloon.

## <a name="next-steps"></a>Volgende stappen
Nu kunt u precies de installatiekopie kiezen die u wilt gebruiken. Voor het maken van een virtuele machine snel met behulp van de installatiekopie-informatie, die u zojuist hebt gevonden, Zie [virtuele Windows-machine maken met PowerShell](quick-create-powershell.md).
