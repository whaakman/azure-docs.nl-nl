---
title: Maken van een Azure virtuele-machineschaalset | Microsoft Docs
description: Maken en implementeren van een Linux- of Windows Azure virtuele-machineschaalset ingesteld met Azure CLI, PowerShell, een sjabloon of Visual Studio.
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 07/21/2017
ms.author: adegeo
ms.openlocfilehash: 32af01aa545c541688128a7ae6bbb82a0e046f2d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-deploy-a-virtual-machine-scale-set"></a>Maken en implementeren van een virtuele-machineschaalset
Virtuele-machineschaalsets kunnen gemakkelijk te implementeren en beheren van identieke virtuele machines als een set. Schaalsets bieden een uiterst schaalbare en aanpasbare berekeningslaag voor toepassingen die grootschalig en installatiekopieën voor Windows-platform, installatiekopieën van virtuele Linux-platform, aangepaste installatiekopieën en -extensies ondersteunen. Zie voor meer informatie over schaalsets [virtuele-machineschaalsets](virtual-machine-scale-sets-overview.md).

Deze zelfstudie ziet u het maken van een virtuele-machineschaalset **zonder** met de Azure portal. Zie voor meer informatie over het gebruik van de Azure-portal [het maken van een virtuele-machineschaalset ingesteld met de Azure-portal](virtual-machine-scale-sets-portal-create.md).

>[!NOTE]
>Zie voor meer informatie over Azure Resource Manager-resources [Azure Resource Manager versus klassieke implementatie](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Als u Azure PowerShell of Azure CLI 2.0 een schaalset maken, moet u eerst aan te melden bij uw abonnement.

Zie voor meer informatie over het installeren, het instellen en het aanmelden bij Azure met Azure CLI of PowerShell [aan de slag met Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) of [aan de slag met Azure PowerShell-cmdlets](/powershell/azure/overview).

```azurecli
az login
```

```powershell
Login-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

U moet eerst een resourcegroep die betrekking heeft op de virtuele-machineschaalset maken.

```azurecli
az group create --location westus2 --name MyResourceGroup1
```

```powershell
New-AzureRmResourceGroup -Location westus2 -Name MyResourceGroup1
```

## <a name="create-from-azure-cli"></a>Maken van Azure CLI

U kunt een virtuele-machineschaalset ingesteld met een minimale inspanning maken met Azure CLI. Als u standaardwaarden weglaat, zijn ze beschikbaar voor u. Als u de informatie van een virtueel netwerk niet opgeeft, wordt bijvoorbeeld een virtueel netwerk voor u gemaakt. Als u de volgende onderdelen weglaat, wordt deze voor u gemaakt: 
- Een load balancer
- Een virtueel netwerk
- Een openbaar IP-adres

Als u de installatiekopie van de virtuele machine die u wilt gebruiken op de virtuele-machineschaalset, hebt u enkele mogelijkheden:

- URN  
De id van een resource:  
**Win2012R2Datacenter**

- URN alias  
De beschrijvende naam van een URN:  
**MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest**

- Aangepaste resource-id  
Het pad naar een Azure-resource:  
**/Subscriptions/Subscription-GUID/resourceGroups/MyResourceGroup/providers/Microsoft.COMPUTE/images/MyImage**

- Webbron  
Het pad naar een HTTP-URI:  
**http://contoso.BLOB.Core.Windows.NET/vhds/osdiskimage.VHD**

>[!TIP]
>U kunt een lijst met beschikbare installatiekopieën met opvragen `az vm image list`.

Voor het maken van een virtuele-machineschaalset, moet u het volgende opgeven:

- Resourcegroep 
- Naam
- Installatiekopie van besturingssysteem
- Verificatie-informatie 
 
Het volgende voorbeeld wordt een eenvoudige virtuele-machineschaalset (deze stap kan enkele minuten duren).

```azurecli
az vmss create --resource-group MyResourceGroup1 --name MyScaleSet --image UbuntuLTS --authentication-type password --admin-username azureuser --admin-password P@ssw0rd!
```

Zodra de opdracht is voltooid hebt u nu uw virtuele-machineschaalset gemaakt ingesteld. Mogelijk moet u het IP-adres van de virtuele machine zodat u verbinding mee kan maken. U kunt een groot aantal verschillende gegevens over de virtuele machine (inclusief het IP-adres) met de volgende opdracht ophalen. 

```azurecli
az vmss list-instance-connection-info --resource-group MyResourceGroup1 --name MyScaleSet
```

## <a name="create-from-powershell"></a>Maken van PowerShell

PowerShell is gecompliceerdere te gebruiken dan de Azure CLI. Terwijl Azure CLI bevat standaardinstellingen voor netwerkgerelateerde bronnen (zoals load balancers, IP-adressen en virtuele netwerken), PowerShell niet het geval. Verwijst naar een afbeelding met PowerShell is een iets gecompliceerder te. U kunt afbeeldingen met de volgende cmdlets krijgen:

1. Get-AzureRMVMImagePublisher
2. Get-AzureRMVMImageOffer
3. Get-AzureRmVMImageSku

Het werk cmdlets kunt u de eigenschappen in de reeks. Hier volgt een voorbeeld van hoe u alle installatiekopieën voor de **VS-West 2** regio met een uitgever met de naam **microsoft** erin.

```powershell
Get-AzureRMVMImagePublisher -Location WestUS2 | Where-Object PublisherName -Like *microsoft* | Get-AzureRMVMImageOffer | Get-AzureRmVMImageSku | Select-Object PublisherName, Offer, Skus
```

```
PublisherName              Offer                    Skus
-------------              -----                    ----
microsoft-ads              linux-data-science-vm    linuxdsvm
microsoft-ads              standard-data-science-vm standard-data-science-vm
MicrosoftAzureSiteRecovery Process-Server           Windows-2012-R2-Datacenter
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Enterprise
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Standard
MicrosoftBizTalkServer     BizTalk-Server           2016-Developer
MicrosoftBizTalkServer     BizTalk-Server           2016-Enterprise
...
```

De werkstroom voor het maken van een virtuele-machineschaalset is als volgt:

1. Een configuratie-object met informatie over de schaalaanpassingsset maken.
2. Verwijzen naar de basisinstallatiekopie van het besturingssysteem.
3. Configureer de instellingen van besturingssysteem: verificatie, VM-voorvoegsel en pass-gebruiker.
4. Netwerken configureren.
5. Maak de schaal.

In dit voorbeeld maakt een eenvoudige twee exemplaar schaal instellen voor een computer met Windows Server 2016 is geïnstalleerd.

```powershell
# Resource group name from above
$rg = "MyResourceGroup1"
$location = "WestUS2"

# Create a config object
$vmssConfig = New-AzureRmVmssConfig -Location $location -SkuCapacity 2 -SkuName Standard_A0  -UpgradePolicyMode Automatic

# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig -ImageReferencePublisher MicrosoftWindowsServer -ImageReferenceOffer WindowsServer -ImageReferenceSku 2016-Datacenter -ImageReferenceVersion latest

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig -AdminUsername azureuser -AdminPassword P@ssw0rd! -ComputerNamePrefix myvmssvm

# Create the virtual network resources

## Basics
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name "my-subnet" -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork -Name "my-network" -ResourceGroupName $rg -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $subnet

## Load balancer
$publicIP = New-AzureRmPublicIpAddress -Name "PublicIP" -ResourceGroupName $rg -Location $location -AllocationMethod Static -DomainNameLabel "myuniquedomain"
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontend" -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
$probe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
$inboundNATRule1= New-AzureRmLoadBalancerRuleConfig -Name "webserver" -FrontendIpConfiguration $frontendIP -Protocol Tcp -FrontendPort 80 -BackendPort 80 -IdleTimeoutInMinutes 15 -Probe $probe -BackendAddressPool $backendPool
$inboundNATPool1 = New-AzureRmLoadBalancerInboundNatPoolConfig -Name "RDP" -FrontendIpConfigurationId $frontendIP.Id -Protocol TCP -FrontendPortRangeStart 53380 -FrontendPortRangeEnd 53390 -BackendPort 3389

New-AzureRmLoadBalancer -ResourceGroupName $rg -Name "LB1" -Location $location -FrontendIpConfiguration $frontendIP -LoadBalancingRule $inboundNATRule1 -InboundNatPool $inboundNATPool1 -BackendAddressPool $backendPool -Probe $probe

## IP address config
$ipConfig = New-AzureRmVmssIpConfig -Name "my-ipaddress" -LoadBalancerBackendAddressPoolsId $backendPool.Id -SubnetId $vnet.Subnets[0].Id -LoadBalancerInboundNatPoolsId $inboundNATPool1.Id

# Attach the virtual network to the IP object
Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmssConfig -Name "network-config" -Primary $true -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss -ResourceGroupName $rg -Name "MyScaleSet1" -VirtualMachineScaleSet $vmssConfig
```

### <a name="using-a-custom-virtual-machine-image"></a>Met behulp van de installatiekopie van een aangepaste virtuele machine
Als u een schaal ingesteld op basis van uw eigen aangepaste installatiekopie, in plaats van die verwijzen naar de installatiekopie van een virtuele machine uit de galerie maakt de _Set AzureRmVmssStorageProfile_ opdracht er als volgt:
```PowerShell
Set-AzureRmVmssStorageProfile -OsDiskCreateOption FromImage -ManagedDisk PremiumLRS -OsDiskCaching "None" -OsDiskOsType Linux -ImageReferenceId (Get-AzureRmImage -ImageName $VMImage -ResourceGroupName $rg).id
```

## <a name="create-from-a-template"></a>Op basis van een sjabloon maken

U kunt een virtuele-machineschaalset ingesteld met behulp van een Azure Resource Manager-sjabloon implementeren. U kunt uw eigen sjabloon maken of gebruik een van de [sjabloon opslagplaats](https://azure.microsoft.com/resources/templates/?term=vmss). Deze sjablonen kunnen rechtstreeks worden geïmplementeerd in uw Azure-abonnement.

>[!NOTE]
>Als u wilt uw eigen sjabloon maakt, moet u een JSON-tekstbestand maken. Raadpleeg voor algemene informatie over het maken en aanpassen van een sjabloon [Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md).

Er is een voorbeeldsjabloon beschikbaar [op GitHub](https://github.com/gatneil/mvss/tree/minimum-viable-scale-set). Zie voor meer informatie over het maken en gebruiken dat voorbeeld [Minimum levensvatbaar schaalset](.\virtual-machine-scale-sets-mvss-start.md).

## <a name="create-from-visual-studio"></a>Maak vanuit Visual Studio

U kunt met Visual Studio een Azure-resourcegroepproject maken en toevoegen van dat een virtuele-machineschaalset sjabloon ingesteld op het. U kunt kiezen of u wilt importeren in GitHub of de galerie van Azure Web Application. Een PowerShell-script-implementatie wordt ook voor u gegenereerd. Zie voor meer informatie [het maken van een virtuele-machineschaalset instellen met Visual Studio](virtual-machine-scale-sets-vs-create.md).

## <a name="create-from-the-azure-portal"></a>Maken van de Azure-portal

De Azure portal biedt een handige manier om snel een scale-set maken. Zie voor meer informatie [het maken van een virtuele-machineschaalset ingesteld met de Azure-portal](virtual-machine-scale-sets-portal-create.md).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [gegevensschijven](virtual-machine-scale-sets-attached-disks.md).

Meer informatie over hoe [beheren van uw apps](virtual-machine-scale-sets-deploy-app.md).
