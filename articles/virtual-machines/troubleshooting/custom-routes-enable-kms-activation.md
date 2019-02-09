---
title: Azure aangepaste routes gebruiken voor het inschakelen van KMS-activering met geforceerde tunnels | Microsoft Docs
description: Ziet u hoe u Azure aangepaste routes naar de KMS-activering in te schakelen wanneer u met behulp van geforceerde tunneling in Azure.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 12/20/2018
ms.author: genli
ms.openlocfilehash: b121996530ea0618fc757f1ae12dfafde10ed7bb
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55979374"
---
# <a name="windows-activation-fails-in-forced-tunneling-scenario"></a>Windows-activering mislukt in geval van geforceerde tunneling

In dit artikel wordt beschreven hoe u kunt oplossen door de KMS-activering probleem die u tegenkomen kunt wanneer u inschakelt geforceerde tunneling in site-naar-site VPN-verbinding of ExpressRoute-scenario's.

## <a name="symptom"></a>Symptoom

U hebt ingeschakeld [geforceerde tunneling](../../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) op Azure back-subnetten van het virtuele netwerk om te leiden van alle internetverkeer naar uw on-premises netwerk. In dit scenario kan de Azure virtual machines (VM's) met Windows Server 2012 R2 (of latere versies van Windows) is Windows activeren. Virtuele machines met een eerdere versie van Windows mislukt echter Windows activeren.

## <a name="cause"></a>Oorzaak

De Windows Azure-VM's moet verbinding maken met de Azure-KMS-server voor Windows-activering. De activering is vereist dat de activeringsaanvraag afkomstig zijn van een openbaar IP-adres van Azure. In het scenario voor geforceerde tunneling de activering is mislukt omdat de activeringsaanvraag afkomstig zijn uit uw on-premises netwerk in plaats van vanaf een openbaar IP-adres van Azure.

## <a name="solution"></a>Oplossing

U lost dit probleem, gebruikt u het verkeer Azure aangepaste route-route-activering naar de Azure-KMS-server.

Het IP-adres van de KMS-server voor de globale Azure-cloud is 23.102.135.246. De DNS-naam is kms.core.windows.net. Als u andere Azure-platform, zoals Azure Duitsland gebruikt, moet u het IP-adres van de bijbehorende KMS-server. Zie de volgende tabel voor meer informatie:

|Platform| KMS DNS|KMS IP|
|------|-------|-------|
|Azure Global|kms.core.windows.net|23.102.135.246|
|Azure Duitsland|kms.core.cloudapi.de|51.4.143.248|
|Azure van de Amerikaanse overheid|kms.core.usgovcloudapi.net|23.97.0.13|
|Azure China 21Vianet|kms.core.chinacloudapi.cn|42.159.7.249|


Als u wilt toevoegen de aangepaste route, de volgende stappen uit:

### <a name="for-resource-manager-vms"></a>Voor virtuele machines Resource Manager

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

1. Open Azure PowerShell, en vervolgens [aanmelden bij uw Azure-abonnement](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Voer de volgende opdrachten uit:

    ```powershell
    # First, get the virtual network that hosts the VMs that have activation problems. In this case, we get virtual network ArmVNet-DM in Resource Group ArmVNet-DM:

    $vnet = Get-AzVirtualNetwork -ResourceGroupName "ArmVNet-DM" -Name "ArmVNet-DM"

    # Next, create a route table and specify that traffic bound to the KMS IP (23.102.135.246) will go directly out:

    $RouteTable = New-AzRouteTable -Name "ArmVNet-DM-KmsDirectRoute" -ResourceGroupName "ArmVNet-DM" -Location "centralus"

    Add-AzRouteConfig -Name "DirectRouteToKMS" -AddressPrefix 23.102.135.246/32 -NextHopType Internet -RouteTable $RouteTable

    Set-AzRouteTable -RouteTable $RouteTable
    ```
3. Ga naar de virtuele machine met activeringsproblemen. Gebruik [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) om te controleren of deze de KMS-server kunt bereiken:

        psping kms.core.windows.net:1688

4. Probeer Windows activeren en bekijken als het probleem opgelost is.

### <a name="for-classic-vms"></a>Voor klassieke VM 's

1. Open Azure PowerShell, en vervolgens [aanmelden bij uw Azure-abonnement](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Voer de volgende opdrachten uit:

    ```powershell
    # First, create a new route table:
    New-AzureRouteTable -Name "VNet-DM-KmsRouteGroup" -Label "Route table for KMS" -Location "Central US"

    # Next, get the route table that was created:
    $rt = Get-AzureRouteTable -Name "VNet-DM-KmsRouteTable"

    # Next, create a route:
    Set-AzureRoute -RouteTable $rt -RouteName "AzureKMS" -AddressPrefix "23.102.135.246/32" -NextHopType Internet

    # Apply the KMS route table to the subnet that hosts the problem VMs (in this case, we apply it to the subnet that's named Subnet-1):
    Set-AzureSubnetRouteTable -VirtualNetworkName "VNet-DM" -SubnetName "Subnet-1" 
    -RouteTableName "VNet-DM-KmsRouteTable"
    ```

3. Ga naar de virtuele machine met activeringsproblemen. Gebruik [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) om te controleren of deze de KMS-server kunt bereiken:

        psping kms.core.windows.net:1688

4. Probeer Windows activeren en bekijken als het probleem opgelost is.

## <a name="next-steps"></a>Volgende stappen

- [Installatiecodes voor KMS-Client](https://docs.microsoft.com/windows-server/get-started/kmsclientkeys
)
- [Bekijken en selecteer activeringsmethoden](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134256(v=ws.11)
)
