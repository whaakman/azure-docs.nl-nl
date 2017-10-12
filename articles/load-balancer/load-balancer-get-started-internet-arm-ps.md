---
title: Een internetgerichte load balancer maken - PowerShell | Microsoft Docs
description: Meer informatie over hoe u met behulp van PowerShell een internetgerichte load balancer maakt in Resource Manager
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
tags: azure-resource-manager
ms.assetid: 8257f548-7019-417f-b15f-d004a1eec826
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 0347e90fa97a083865e8b783c96034d30a859031
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started"></a>Een internetgerichte load balancer maken in Resource Manager met behulp van PowerShell

> [!div class="op_single_selector"]
> * [Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Sjabloon](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het Resource Manager-implementatiemodel. Hier vindt u [meer informatie over hoe u een internetgerichte load balancer maakt met het klassieke implementatiemodel](load-balancer-get-started-internet-classic-cli.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-by-using-azure-powershell"></a>De oplossing implementeren met behulp van Azure PowerShell

De volgende procedures laten zien hoe u Azure Resource Manager gebruikt om een internetgerichte load balancer te maken met behulp van PowerShell. Met Azure Resource Manager wordt elke resource afzonderlijk gemaakt en geconfigureerd, en vervolgens samengevoegd om een load balancer te maken.

U moet de volgende objecten maken en configureren om een load balancer te implementeren:

* Front-end-IP-configuratie: bevat openbare IP-adressen (PIP) voor inkomend netwerkverkeer.
* Back-endadresgroep: bevat netwerkinterfaces (NIC's) waardoor de virtuele machines netwerkverkeer kunnen ontvangen van de load balancer.
* Regels voor taakverdeling: bevat regels die een openbare poort op de load balancer toewijst aan een poort in de back-endadresgroep.
* Inkomende NAT-regels: bevat regels die een openbare poort op de load balancer toewijst aan een poort voor een specifieke virtuele machine in de back-endadresgroep.
* Tests: bevat statustests die worden gebruikt om de beschikbaarheid van exemplaren van virtuele machines in de back-endadresgroep te controleren.

Zie [Azure Resource Manager-ondersteuning voor load balancer](load-balancer-arm.md) voor meer informatie.

## <a name="set-up-powershell-to-use-resource-manager"></a>PowerShell instellen voor het gebruik van Resource Manager

Zorg ervoor dat u de nieuwste productieversie van de Azure Resource Manager-module voor PowerShell hebt:

1. Meld u aan bij Azure.

    ```powershell
    Login-AzureRmAccount
    ```

    Voer uw referenties in wanneer dit wordt gevraagd.

2. Controleer de abonnementen voor het account.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Kies welk Azure-abonnement u wilt gebruiken.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. Maak een resourcegroep. (Sla deze stap over als u een bestaande resourcegroep gebruikt.)

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Een virtueel netwerk en een openbaar IP-adres voor de front-end-IP-adresgroep maken

1. Maak een subnet en een virtueel netwerk.

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. Maak een openbare IP-adresresource voor Azure met de naam **PublicIP** dat moet worden gebruikt door een front-end-IP-adresgroep met de DNS-naam **loadbalancernrp.westus.cloudapp.azure.com**. De volgende opdracht maakt gebruik van het statische toewijzingstype.

    ```powershell
    $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -DomainNameLabel loadbalancernrp
    ```

   > [!IMPORTANT]
   > De load balancer gebruikt het domeinlabel van het openbare IP als voorvoegsel voor de FQDN. Dit wijkt af van het klassieke implementatiemodel, waarbij de cloudservice wordt gebruikt als de FQDN voor de load balancer.
   > In dit voorbeeld is de FQDN **loadbalancernrp.westus.cloudapp.azure.com**.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>Een front-end-IP-adresgroep en back-endadresgroep maken

1. Maak een front-end-IP-adresgroep met de naam **LB-Frontend** die gebruikmaakt van de resource **PublicIp**.

    ```powershell
    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP
    ```

2. Maak een back-endadresgroep met de naam **LB-backend**.

    ```powershell
    $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend
    ```

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>NAT-regels, een load balancer-regel, een test en een load balancer maken

In dit voorbeeld worden de volgende items gemaakt:

* Een NAT-regel om al het verkeer dat binnenkomt op poort 3441, om te zetten naar poort 3389
* Een NAT-regel om al het verkeer dat binnenkomt op poort 3442, om te zetten naar poort 3389
* Een testregel om de integriteitsstatus te testen op een pagina met de naam **HealthProbe.aspx**
* Een load balancer-regel om al het verkeer dat binnenkomt op poort 80, gelijk te verdelen naar poort 80 op de adressen in de back-endgroep
* Een load balancer die gebruikmaakt van al deze objecten

Volg deze stappen:

1. Maak de NAT-regels.

    ```powershell
    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389
    ```

2. Maak een statustest. Er zijn twee manieren om een steekproef te configureren:

    HTTP-test

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    TPC-test

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

3. Maak een load balancer-regel.

    ```powershell
    $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

4. Maak de load balancer met behulp van de eerder gemaakte objecten.

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

## <a name="create-nics"></a>NIC's maken

Maak netwerkinterfaces (of wijzig bestaande interfaces) en koppel deze aan NAT-regels, load balancer-regels en tests:

1. Ga naar het virtuele netwerk en een subnet van een virtueel netwerk waarop de NIC's moeten worden gemaakt.

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. Maak een NIC met de naam **lb-nic1-be**, en koppel deze aan de eerste NAT-regel en de eerste (en enige) back-endadresgroep.

    ```powershell
    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
    ```

3. Maak een NIC met de naam **lb-nic2-be**, en koppel deze aan de tweede NAT-regel en de eerste (en enige) back-endadresgroep.

    ```powershell
    $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
    ```

4. Controleer de NIC's.

        $backendnic1

    Verwachte uitvoer:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
                            "PublicIpAddress": {
                                "Id": null
                            },
                            "LoadBalancerBackendAddressPools": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                                }
                            ],
                            "LoadBalancerInboundNatRules": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                                }
                            ],
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. Gebruik de cmdlet `Add-AzureRmVMNetworkInterface` om de NIC's toe te wijzen aan verschillende VM’s.

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Zie voor instructies voor het maken van een virtuele machine en het toewijzen van een NIC [Een Azure-VM maken met behulp van PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

## <a name="add-the-network-interface-to-the-load-balancer"></a>De netwerkinterface toevoegen aan de load balancer

1. Haal de load balancer op uit Azure.

    Laad de load balancer-resource in een variabele (als u dat nog niet hebt gedaan). De variabele heet **$lb**. Gebruik de namen van de load balancer-resource die u eerder hebt gemaakt.

    ```powershell
    $lb= get-azurermloadbalancer -name NRP-LB -resourcegroupname NRP-RG
    ```

2. Laad de back-endconfiguratie in een variabele.

    ```powershell
    $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name LB-backend -LoadBalancer $lb
    ```

3. Laad de gemaakte netwerkinterface in een variabele. De variabele heet **$nic**. De netwerkinterfacenaam is hetzelfde als in bovenstaand voorbeeld.

    ```powershell
    $nic =get-azurermnetworkinterface -name lb-nic1-be -resourcegroupname NRP-RG
    ```

4. Wijzig de back-endconfiguratie op de netwerkinterface.

    ```powershell
    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
    ```

5. Sla het netwerkinterfaceobject op.

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```

    Zodra er een netwerkinterface is toegevoegd aan de back-endgroep van de load balancer, begint deze netwerkverkeer te ontvangen op basis van de taakverdelingsregels voor die load balancer-resource.

## <a name="update-an-existing-load-balancer"></a>Een bestaande load balancer bijwerken

1. Met behulp van de load balancer uit het bovenstaande voorbeeld wijst u load balancer-object toe aan de variabele **$slb** met `Get-AzureLoadBalancer`.

    ```powershell
    $slb = get-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
    ```

2. In het volgende voorbeeld voegt u aan een bestaande load balancer een NAT-regel voor binnenkomende verbindingen toe met poort 81 in de front-end en poort 8181 voor de back-endgroep.

    ```powershell
    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP
    ```

3. Sla de nieuwe configuratie op met behulp van `Set-AzureLoadBalancer`.

    ```powershell
    $slb | Set-AzureRmLoadBalancer
    ```

## <a name="remove-a-load-balancer"></a>Een load balancer verwijderen

Gebruik de opdracht `Remove-AzureLoadBalancer` om een eerder gemaakte load balancer met de naam **NRP-LB** te verwijderen uit de resourcegroep **NRP RG**.

```powershell
Remove-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
```

> [!NOTE]
> Met de optionele schakeloptie **-Force** kunt u de prompt voor verwijdering uitschakelen.

## <a name="next-steps"></a>Volgende stappen

[Aan de slag met het configureren van een interne load balancer](load-balancer-get-started-ilb-arm-ps.md)

[Een distributiemodus voor de load balancer configureren](load-balancer-distribution-mode.md)

[TCP-time-outinstellingen voor inactiviteit voor de load balancer configureren](load-balancer-tcp-idle-timeout.md)
