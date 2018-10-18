---
title: 'Azure PowerShell-voorbeeldscript: een Azure Firewall-testomgeving maken'
description: 'Azure PowerShell-voorbeeldscript: een Azure Firewall-testomgeving maken.'
services: virtual-network
author: vhorne
ms.service: firewall
ms.devlang: powershell
ms.topic: sample
ms.date: 8/13/2018
ms.author: victorh
ms.openlocfilehash: 7f1986a9a59087d084577e980233ff87360a17e0
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49390105"
---
# <a name="create-an-azure-firewall-test-environment"></a>Een Azure Firewall-testomgeving maken

In dit voorbeeldscript wordt een firewall en een testnetwerkomgeving gemaakt. Het netwerk heeft één VNet met drie subnetten: een *AzureFirewallSubnet*, een *ServersSubnet* en een *JumpboxSubnet*. Het ServersSubnet en het JumpboxSubnet hebben elk één Windows Server met twee kernen.

De firewall bevindt zich in het AzureFirewallSubnet en is geconfigureerd met een toepassingsregelverzameling met één regel die toegang geeft tot www.microsoft.com.

Er wordt een gebruikergedefinieerde route gemaakt die het netwerkverkeer vanuit het ServersSubnet door de firewall leidt, waar de firewallregels worden toegepast.

U kunt het script uitvoeren vanuit de Azure [Cloud Shell](https://shell.azure.com/powershell) of vanuit een lokale installatie van PowerShell. 

Als u PowerShell lokaal uitvoert, vereist dit script de nieuwste versie van de AzureRM PowerShell-module (6.9.0 of hoger). Voer `Get-Module -ListAvailable AzureRM` uit om na te gaan welke versie er is geïnstalleerd. 

Als u moet upgraden, kunt u `PowerShellGet` gebruiken, wat in Windows 10 en Windows Server 2016 is ingebouwd.

> [!NOTE]
>Voor andere Windows-versies moet u `PowerShellGet` installeren voordat u het kunt gebruiken. U kunt `Get-Module -Name PowerShellGet -ListAvailable | Select-Object -Property Name,Version,Path` uitvoeren om te bepalen of het op uw systeem is geïnstalleerd. Als de uitvoer leeg is, moet u het nieuwste [Windows Management Framework](https://www.microsoft.com/download/details.aspx?id=54616) installeren.

Zie [Azure PowerShell in Windows installeren met PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.4.0) voor meer informatie.

Een bestaande Azure PowerShell-installatie die met het webplatforminstallatieprogramma is uitgevoerd, conflicteert met de PowerShellGet-installatie en moet worden verwijderd.

Vergeet niet dat als u PowerShell lokaal uitvoert, u ook `Connect-AzureRmAccount` moet uitvoeren om verbinding te kunnen maken met Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript


[!code-azurepowershell-interactive[main](../../../powershell_scripts/firewall/create-fw-test.ps1  "Create a firewall test environment")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht uit om de resourcegroep, VM en alle gerelateerde resources te verwijderen:

```powershell
Remove-AzureRmResourceGroup -Name AzfwSampleScriptEastUS -Force
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt voor het maken van een resourcegroep, een virtueel netwerk en netwerkbeveiligingsgroepen. Elke opdracht in de volgende tabel is een koppeling naar opdrachtspecifieke documentatie:

| Opdracht | Opmerkingen |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Hiermee maakt u een subnetconfiguratie |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Hiermee maakt u een virtueel Azure-netwerk en front-end-subnet. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Hiermee maakt u beveiligingsregels voor verbindingen die worden toegewezen aan een netwerkbeveiligingsgroep. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) |Hiermee maakt u NSG-regels die bepaalde poorten tot specifieke subnetten blokkeren of toestaan. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | Hiermee koppelt u NSG's aan subnetten. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Hiermee maakt u een openbaar IP-adres voor toegang tot de VM via internet. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Hiermee maakt u virtuele netwerkinterfaces en koppelt u ze aan de front-end- en back-end-subnetten van het virtuele netwerk. |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Hiermee maakt u een VM-configuratie. Deze configuratie bevat informatie zoals de naam, het besturingssysteem en de beheerdersreferenties van de virtuele machine. De configuratie wordt gebruikt tijdens het maken van de virtuele machine. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Hiermee maakt u een virtuele machine. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep en alle daarin opgenomen resources. |
|[New-AzureRmFirewall](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermfirewall?view=azurermps-6.9.0)| Hiermee wordt een nieuwe Azure Firewall gemaakt.|
|[Get-AzureRmFirewall](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermfirewall?view=azurermps-6.9.0)|Hiermee wordt een Azure Firewall-object opgehaald.|
|[New-AzureRmFirewallApplicationRule](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermfirewallapplicationrule?view=azurermps-6.9.0)|Hiermee wordt een regel voor een Azure Firewall-toepassing gemaakt.|
|[Set-AzureRmFirewall](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermfirewall?view=azurermps-6.9.0)|Hiermee worden wijzigingen aan het Azure Firewall-object doorgevoerd.|


## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](/powershell/azure/overview) voor meer informatie over Azure PowerShell.

