---
title: Azure aangepaste routes gebruiken voor het inschakelen van KMS-activering met geforceerde tunnels | Microsoft Docs
description: Laat zien hoe Azure aangepaste routes gebruiken voor het inschakelen van KMS-activering met geforceerde tunnels in Azure.
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
ms.openlocfilehash: f1e2ab6a954361a7807d78dc2baf5d24af52a679
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53798003"
---
# <a name="windows-activation-fails-in-forced-tunneling-scenario"></a>Windows-activering mislukt in geval van geforceerde tunneling

In dit artikel wordt beschreven hoe u kunt oplossen door de KMS-activering probleem die u tegenkomen kunt bij het inschakelen van geforceerde tunneling in site-naar-site VPN-verbinding of ExpressRoute-scenario's.

## <a name="symptom"></a>Symptoom

U hebt ingeschakeld [geforceerde tunneling](../../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) op de Azure back-subnetten van het virtuele netwerk om te leiden van alle internetverkeer naar uw on-premises netwerk. In dit scenario kan de Azure virtual machines (VM) met Windows Server 2012 R2 of hoger is de Windows activeren. De VM's waarop een eerdere versie van Windows mislukt echter de Windows activeren. 

## <a name="cause"></a>Oorzaak

De Windows Azure-VM's moet verbinding maken met de Azure-KMS-server voor Windows-activering. De activering is vereist dat de activeringsaanvraag afkomstig van een openbaar IP-adres van Azure zijn moet. In het scenario voor geforceerde tunneling mislukt de activering omdat de aanvraag voor activering van uw on-premises netwerk in plaats van vanaf een Azure openbaar IP-adres. 

## <a name="solution"></a>Oplossing

U lost dit probleem, gebruikt u het verkeer Azure aangepaste route-route-activering naar de Azure-KMS-server (23.102.135.246). 

Het IP-adres 23.102.135.246 is het IP-adres van de KMS-server voor de globale Azure-cloud. De DNS-naam is kms.core.windows.net. Als u andere Azure-platform, zoals Azure Duitsland gebruikt, moet u het IP-adres van de KMS-server overeenkomen. Zie de volgende tabel voor meer informatie:

|Platform| KMS-DNS|KMS-IP|
|------|-------|-------|
|Azure wereldwijd|KMS.Core.Windows.NET|23.102.135.246|
|Azure Duitsland|KMS.Core.cloudapi.de|51.4.143.248|
|Azure van de Amerikaanse overheid|KMS.Core.usgovcloudapi.NET|23.97.0.13|
|Azure China 21Vianet|KMS.Core.chinacloudapi.CN|42.159.7.249|


Als u wilt toevoegen de aangepaste route, de volgende stappen uit:

### <a name="for-resource-manager-vms"></a>Voor virtuele machines Resource Manager

1. Open Azure PowerShell, en vervolgens [aanmelden bij uw Azure-abonnement](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Voer de volgende opdrachten uit:

    ```powershell
    # First, we will get the virtual network hosts the VMs that has activation problems. In this case, I get virtual network ArmVNet-DM in Resource Group ArmVNet-DM

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "ArmVNet-DM" -Name "ArmVNet-DM"

    # Next, we create a route table and specify that traffic bound to the KMS IP (23.102.135.246) will go directly out

    $RouteTable = New-AzureRmRouteTable -Name "ArmVNet-DM-KmsDirectRoute" -ResourceGroupName "ArmVNet-DM" -Location "centralus"

    Add-AzureRmRouteConfig -Name "DirectRouteToKMS" -AddressPrefix 23.102.135.246/32 -NextHopType Internet -RouteTable $RouteTable

    Set-AzureRmRouteTable -RouteTable $RouteTable
    ```
3. Ga naar de virtuele machine die activering probleem is, gebruikt u [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) om te controleren of het KMS-server kan worden bereikt:

        psping kms.core.windows.net:1688

4. Probeer Windows activeren en zien als het probleem opgelost is.

### <a name="for-classic-vms"></a>Voor klassieke VM 's

1. Open Azure PowerShell, en vervolgens [aanmelden bij uw Azure-abonnement](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Voer de volgende opdrachten uit:

    ```powershell
    # First, we will create a new route table
    New-AzureRouteTable -Name "VNet-DM-KmsRouteGroup" -Label "Route table for KMS" -Location "Central US"

    # Next, get the routetable that was created
    $rt = Get-AzureRouteTable -Name "VNet-DM-KmsRouteTable"

    # Next, create a route
    Set-AzureRoute -RouteTable $rt -RouteName "AzureKMS" -AddressPrefix "23.102.135.246/32" -NextHopType Internet

    # Apply KMS route table to the subnet that host the problem VMs (in this case, I will apply it to the subnet named Subnet-1)
    Set-AzureSubnetRouteTable -VirtualNetworkName "VNet-DM" -SubnetName "Subnet-1" 
    -RouteTableName "VNet-DM-KmsRouteTable"
    ```

3. Ga naar de virtuele machine die activering probleem is, gebruikt u [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) om te controleren of het KMS-server kan worden bereikt:

        psping kms.core.windows.net:1688

4. Probeer Windows activeren en zien als het probleem opgelost is.

## <a name="next-steps"></a>Volgende stappen

- [Installatiecodes voor KMS-Client](https://docs.microsoft.com/windows-server/get-started/kmsclientkeys
)
- [Bekijken en selecteer activeringsmethoden](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134256(v=ws.11)
)