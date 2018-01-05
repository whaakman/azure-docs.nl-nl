---
title: De Load Balancer op meerdere IP-configuraties in Azure | Microsoft Docs
description: Taakverdeling over de primaire en secundaire IP-configuraties.
services: load-balancer
documentationcenter: na
author: anavinahar
manager: narayan
editor: na
ms.assetid: 244907cd-b275-4494-aaf7-dcfc4d93edfe
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: annahar
ms.openlocfilehash: 2235d007434dabde1639ab19bc6813c818ed5ed7
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="load-balancing-on-multiple-ip-configurations-using-powershell"></a>De Load Balancer op meerdere IP-configuraties met behulp van PowerShell

> [!div class="op_single_selector"]
> * [Portal](load-balancer-multiple-ip.md)
> * [CLI](load-balancer-multiple-ip-cli.md)
> * [PowerShell](load-balancer-multiple-ip-powershell.md)

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Dit artikel wordt beschreven hoe u Azure Load Balancer met meerdere IP-adressen op een secundaire netwerkinterface (NIC). In dit scenario hebben we twee virtuele machines met Windows, elk met een primaire en een secundaire NIC. Elk van de secundaire NIC's heeft twee IP-configuraties. Elke virtuele machine fungeert als host van websites contoso.com en fabrikam.com. Elke website is gebonden aan een van de IP-configuraties voor de secundaire NIC. We Azure Load Balancer gebruiken om twee IP-adressen voor frontend, één voor elke website, voor het distribueren van het verkeer naar de respectieve IP-configuratie voor de website weer te geven. Dit scenario gebruikt hetzelfde poortnummer op zowel frontends, evenals het IP-adressen van zowel back-end-groep.

![De installatiekopie van de Load Balancer-scenario](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a>Stappen om taken te verdelen over meerdere IP-configuraties

Volg onderstaande stappen voor het bereiken van het scenario in dit artikel wordt beschreven:

1. Installeer Azure PowerShell. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor informatie over het installeren van de nieuwste versie van Azure PowerShell, het selecteren van het abonnement en het aanmelden bij uw account.
2. Maak een resourcegroep met de volgende instellingen:

    ```powershell
    $location = "westcentralus".
    $myResourceGroup = "contosofabrikam"
    ```

    Zie voor meer informatie stap 2 van [een resourcegroep maken](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

3. [Maken van een Beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md?toc=%2fazure%2fload-balancer%2ftoc.json) uw virtuele machines bevatten. In dit scenario moet u de volgende opdracht gebruiken:

    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset" -Location "West Central US"
    ```

4. Volg de instructies stappen 3 tot en met 5 in [maken van een virtuele machine van Windows](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) artikel voor het voorbereiden van het maken van een virtuele machine met een enkele netwerkinterfacekaart. 6.1 stap uit te voeren en de volgende opdracht gebruiken in plaats van stap 6.2:

    ```powershell
    $availset = Get-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset"
    New-AzureRmVMConfig -VMName "VM1" -VMSize "Standard_DS1_v2" -AvailabilitySetId $availset.Id
    ```

    Voltooi [maken van een virtuele machine van Windows](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) 6.3 via 6,8 stappen.

5. Een tweede IP-configuratie toevoegen aan elk van de virtuele machines. Volg de instructies in [meerdere IP-adressen toewijzen aan virtuele machines](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md#add) artikel. Gebruik de volgende configuratieinstellingen:

    ```powershell
    $NicName = "VM1-NIC2"
    $RgName = "contosofabrikam"
    $NicLocation = "West Central US"
    $IPConfigName4 = "VM1-ipconfig2"
    $Subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -VirtualNetwork $myVnet
    ```

    U hoeft niet te koppelen van de secundaire IP-configuraties met openbare IP-adressen voor deze zelfstudie. Bewerk de opdracht voor het verwijderen van het openbare deel van de IP-koppeling.

6. Voltooi de stappen 4 tot en met 6 van dit artikel opnieuw voor VM2. Zorg ervoor dat de naam van de VM naar VM2 vervangen wanneer dit te doen. Houd er rekening mee dat u niet wilt maken van een virtueel netwerk voor de tweede VM. U kunt of kan een nieuw subnet op basis van uw gebruiksvoorbeeld niet maken.

7. Twee openbare IP-adressen maken en deze opslaan op de juiste variabelen, zoals wordt weergegeven:

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

9. Uw back-end-adresgroepen, een test en de load-balancingregels maken:

    ```powershell
    $beaddresspool1 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name contosopool
    $beaddresspool2 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HTTP -RequestPath 'index.html' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    $lbrule1 = New-AzureRmLoadBalancerRuleConfig -Name HTTPc -FrontendIpConfiguration $frontendIP1 -BackendAddressPool $beaddresspool1 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    $lbrule2 = New-AzureRmLoadBalancerRuleConfig -Name HTTPf -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

10. Zodra u deze resources die zijn gemaakt hebt, maakt u de load balancer:

    ```powershell
    $mylb = New-AzureRmLoadBalancer -ResourceGroupName contosofabrikam -Name mylb -Location 'West Central US' -FrontendIpConfiguration $frontendIP1 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

11. De tweede back-end-pool en frontend IP-adresconfiguratie toevoegen aan de zojuist gemaakte load balancer:

    ```powershell
    $mylb = Get-AzureRmLoadBalancer -Name "mylb" -ResourceGroupName $myResourceGroup | Add-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool | Set-AzureRmLoadBalancer

    $mylb | Add-AzureRmLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2 | Set-AzureRmLoadBalancer
    
    Add-AzureRmLoadBalancerRuleConfig -Name HTTP -LoadBalancer $mylb -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80 | Set-AzureRmLoadBalancer
    ```

12. De onderstaande opdrachten de NIC's ophalen en vervolgens beide IP-configuraties van elke secundaire NIC toe te voegen aan de back-end-adresgroep van de load balancer:

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

13. Tot slot moet u DNS-bronrecords om te verwijzen naar de respectieve frontend-IP-adres van de Load Balancer configureren. U kunt uw domeinen in Azure DNS hosten. Zie voor meer informatie over het gebruik van Azure DNS met Load Balancer [met behulp van Azure DNS met andere Azure-services](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het combineren van de load balancer-services in Azure in [met gelijke taakverdeling van services in Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).
- Meer informatie over hoe u verschillende typen logboeken kunt gebruiken in Azure te beheren en oplossen van de load balancer in [analytics aanmelden voor Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md).
