---
title: De taakverdeling op meerdere IP-configuraties in Azure
titlesuffix: Azure Load Balancer
description: Taakverdeling over de primaire en secundaire IP-configuraties.
services: load-balancer
documentationcenter: na
author: anavinahar
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: annahar
ms.openlocfilehash: cdfb374bf9245d055c86974debc9a6eb8f0fefd6
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53164208"
---
# <a name="load-balancing-on-multiple-ip-configurations-using-powershell"></a>De taakverdeling op meerdere IP-configuraties met behulp van PowerShell

> [!div class="op_single_selector"]
> * [Portal](load-balancer-multiple-ip.md)
> * [CLI](load-balancer-multiple-ip-cli.md)
> * [PowerShell](load-balancer-multiple-ip-powershell.md)


Dit artikel wordt beschreven hoe u Azure Load Balancer met meerdere IP-adressen op een secundaire netwerkinterface (NIC). Voor dit scenario hebben we twee virtuele machines met Windows, elk met een primaire en een secundaire NIC. Elk van de secundaire NIC's heeft twee IP-configuraties. Elke virtuele machine als host fungeert voor websites contoso.com en fabrikam.com. Elke website is gebonden aan een van de IP-configuraties voor de secundaire NIC. We gebruiken Azure Load Balancer om twee IP-adressen voor front-end, één voor elke website, voor het distribueren van verkeer naar de respectieve IP-configuratie voor de website zichtbaar te maken. In dit scenario maakt gebruik van hetzelfde poortnummer in beide front-ends, evenals het IP-adressen van beide back-end-groep.

![Afbeelding van LB-scenario](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a>Stappen voor het verdelen van meerdere IP-configuraties

Volg onderstaande stappen voor het bereiken van het scenario in dit artikel beschreven:

1. Installeer Azure PowerShell. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor informatie over het installeren van de nieuwste versie van Azure PowerShell, het selecteren van het abonnement en het aanmelden bij uw account.
2. Maak een resourcegroep met de volgende instellingen:

    ```powershell
    $location = "westcentralus".
    $myResourceGroup = "contosofabrikam"
    ```

    Zie voor meer informatie stap 2 van [een resourcegroep maken](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

3. [Maak een Beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md?toc=%2fazure%2fload-balancer%2ftoc.json) aan uw virtuele machines bevatten. Gebruik de volgende opdracht in dit scenario:

    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset" -Location "West Central US"
    ```

4. Volg de instructies stappen 3 t/m 5 in [maken van een Windows-VM](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) artikel voor het voorbereiden van het maken van een virtuele machine met een enkele NIC. Stap 6.1 uitvoeren en gebruik de volgende informatie in plaats van stap 6.2:

    ```powershell
    $availset = Get-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset"
    New-AzureRmVMConfig -VMName "VM1" -VMSize "Standard_DS1_v2" -AvailabilitySetId $availset.Id
    ```

    Voltooi [maken van een Windows-VM](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) 6.3 via 6,8 stappen.

5. Een tweede IP-configuratie toevoegen aan elk van de virtuele machines. Volg de instructies in [meerdere IP-adressen toewijzen aan virtuele machines](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md#add) artikel. Gebruik de volgende configuratieinstellingen:

    ```powershell
    $NicName = "VM1-NIC2"
    $RgName = "contosofabrikam"
    $NicLocation = "West Central US"
    $IPConfigName4 = "VM1-ipconfig2"
    $Subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -VirtualNetwork $myVnet
    ```

    U hoeft niet te koppelen van de secundaire IP-configuraties met openbare IP-adressen voor deze zelfstudie. Bewerk de opdracht voor het verwijderen van het openbare deel van de IP-koppeling.

6. Volg de stappen 4 t / m 6 van dit artikel nogmaals voor VM2. Zorg ervoor dat de naam van de virtuele machine voor VM2 vervangen bij het uitvoeren van deze. Houd er rekening mee dat u niet wilt maken van een virtueel netwerk voor de tweede virtuele machine. U kan of kan niet maken voor een nieuw subnet op basis van uw situatie.

7. Maak twee openbare IP-adressen en deze opslaan in de betreffende variabelen, zoals wordt weergegeven:

    ```powershell
    $publicIP1 = New-AzureRmPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel contoso
    $publicIP2 = New-AzureRmPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel fabrikam

    $publicIP1 = Get-AzureRmPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam
    $publicIP2 = Get-AzureRmPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam
    ```

8. Maak twee frontend-IP-configuraties:

    ```powershell
    $frontendIP1 = New-AzureRmLoadBalancerFrontendIpConfig -Name contosofe -PublicIpAddress $publicIP1
    $frontendIP2 = New-AzureRmLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2
    ```

9. Uw back-end-adresgroepen, een test en de load balancer-regels maken:

    ```powershell
    $beaddresspool1 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name contosopool
    $beaddresspool2 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HTTP -RequestPath 'index.html' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    $lbrule1 = New-AzureRmLoadBalancerRuleConfig -Name HTTPc -FrontendIpConfiguration $frontendIP1 -BackendAddressPool $beaddresspool1 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    $lbrule2 = New-AzureRmLoadBalancerRuleConfig -Name HTTPf -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

10. Wanneer u deze resources die zijn gemaakt hebt, maakt u de load balancer:

    ```powershell
    $mylb = New-AzureRmLoadBalancer -ResourceGroupName contosofabrikam -Name mylb -Location 'West Central US' -FrontendIpConfiguration $frontendIP1 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

11. De tweede back-end-pool en front-end IP-adresconfiguratie toevoegen aan uw zojuist gemaakte load balancer:

    ```powershell
    $mylb = Get-AzureRmLoadBalancer -Name "mylb" -ResourceGroupName $myResourceGroup | Add-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool | Set-AzureRmLoadBalancer

    $mylb | Add-AzureRmLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2 | Set-AzureRmLoadBalancer
    
    Add-AzureRmLoadBalancerRuleConfig -Name HTTP -LoadBalancer $mylb -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80 | Set-AzureRmLoadBalancer
    ```

12. De onderstaande opdrachten de NIC's ophalen en vervolgens beide IP-configuraties van elke secundaire NIC toevoegen aan de back-endadresgroep van de load balancer:

    ```powershell
    $nic1 = Get-AzureRmNetworkInterface -Name "VM1-NIC2" -ResourceGroupName "MyResourcegroup";
    $nic2 = Get-AzureRmNetworkInterface -Name "VM2-NIC2" -ResourceGroupName "MyResourcegroup";

    $nic1.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic1.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);
    $nic2.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic2.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);

    $mylb = $mylb | Set-AzureRmLoadBalancer

    $nic1 | Set-AzureRmNetworkInterface
    $nic2 | Set-AzureRmNetworkInterface
    ```

13. Tot slot moet u DNS-resourcerecords om te verwijzen naar de respectieve frontend-IP-adres van de Load Balancer configureren. U kunt uw domeinen in Azure DNS hosten. Zie voor meer informatie over het gebruik van Azure DNS met Load Balancer [met behulp van Azure DNS met andere Azure-services](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het combineren van load balancing-services in Azure in [met load balancing-services in Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).
- Lees hoe u verschillende soorten logboeken in Azure kunt gebruiken om te beheren en problemen oplossen van load balancer in het [Log analytics voor Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md).
