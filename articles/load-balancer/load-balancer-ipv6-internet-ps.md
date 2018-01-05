---
title: Maken van een Azure Internet gerichte load balancer met IPv6 - PowerShell | Microsoft Docs
description: Informatie over het maken van een Internetgericht load balancer met IPv6 met behulp van PowerShell voor Resource Manager
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
tags: azure-resource-manager
keywords: IPv6-, azure load balancer, dual-stack, openbare IP-adres, systeemeigen ipv6, mobiele, iot
ms.assetid: d4c649e3-84ad-4343-8b6a-0e89f0b9e518
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: a84fd69c568e26bbd1ff06b699b804c70e0e9c09
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a>Maken van een Internetgericht load balancer met IPv6 met behulp van PowerShell voor Resource Manager aan de slag

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [Azure-CLI](load-balancer-ipv6-internet-cli.md)
> * [Sjabloon](load-balancer-ipv6-internet-template.md)


[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure Load Balancer is een Layer-4 (TCP, UDP) load balancer. De load balancer biedt hoge beschikbaarheid bij het verdelen van inkomend verkeer over gezonde service-exemplaren in cloudservices of virtuele machines in een load balancer-set. Azure Load Balancer kan deze services ook toepassen op meerdere poorten, meerdere IP-adressen of allebei.

## <a name="example-deployment-scenario"></a>Voorbeeldscenario voor implementatie

Het volgende diagram illustreert de oplossing wordt geïmplementeerd in dit artikel voor taakverdeling.

![Load balancer-scenario](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

In dit scenario maakt u de volgende Azure-resources:

* een internetgerichte Load Balancer met een IPv4 en IPv6-openbare IP-adres
* twee taakverdelingsregels het openbare VIP's worden toegewezen aan de persoonlijke eindpunten
* een Beschikbaarheidsset aan die bevat de twee virtuele machines
* twee virtuele machines (VM's)
* de virtuele netwerkinterface voor elke virtuele machine met zowel IPv4 als IPv6-adressen die zijn toegewezen

## <a name="deploying-the-solution-using-the-azure-powershell"></a>Implementatie van de oplossing met behulp van Azure PowerShell

De volgende stappen laten zien hoe een Internetgericht load balancer met Azure Resource Manager met PowerShell maken. Met Azure Resource Manager elke bron wordt gemaakt en geconfigureerd afzonderlijk, klikt u vervolgens put samen om een resource.

Voor het implementeren van een load balancer maken en configureren van de volgende objecten:

* Front-end-IP-configuratie: bevat openbare IP-adressen voor inkomend netwerkverkeer.
* Back-endadresgroep: bevat netwerkinterfaces (NIC's) waardoor de virtuele machines netwerkverkeer kunnen ontvangen van de load balancer.
* Regels voor taakverdeling: bevat regels die een openbare poort op de load balancer toewijzen aan een poort in de back-endadresgroep.
* NAT-regels voor binnenkomende verbindingen: bevat regels die een openbare poort op de load balancer toewijzen aan een poort voor een specifieke virtuele machine in de back-endadresgroep.
* Tests: bevat statustests die worden gebruikt om de beschikbaarheid van exemplaren van virtuele machines in de back-endadresgroep te controleren.

Zie [Azure Resource Manager-ondersteuning voor load balancer](load-balancer-arm.md) voor meer informatie.

## <a name="set-up-powershell-to-use-resource-manager"></a>PowerShell instellen voor het gebruik van Resource Manager

Zorg ervoor dat u hebt de nieuwste productieversie van de module Azure Resource Manager PowerShell.

1. Meld u aan bij Azure

    ```powershell
    Login-AzureRmAccount
    ```

    Voer uw referenties in wanneer dit wordt gevraagd.

2. De abonnementen voor het account controleren

    ```powershell
    Get-AzureRmSubscription
    ```

3. Kies welk Azure-abonnement u wilt gebruiken.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. Maak een resourcegroep (kunt deze stap als u een bestaande resourcegroep overslaan)

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Een virtueel netwerk en een openbaar IP-adres voor de front-end-IP-adresgroep maken

1. Maak een virtueel netwerk met een subnet.

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. Azure openbaar IP-adres (PIP) resources maken voor de front-end-IP-adresgroep.

    ```powershell
    $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
    $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6
    ```

    > [!IMPORTANT]
    > De load balancer gebruikmaakt van het domeinlabel van het openbare IP-adres als voorvoegsel voor de FQDN-naam. In dit voorbeeld wordt de FQDN-namen zijn *lbnrpipv4.westus.cloudapp.azure.com* en *lbnrpipv6.westus.cloudapp.azure.com*.

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a>Een Front-End-IP-configuraties en een Back-End-adresgroep maken

1. Maak adres voor front-end-configuratie die gebruikmaakt van het openbare IP-adressen die u hebt gemaakt.

    ```powershell
    $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
    $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6
    ```

2. Maak back-end-adresgroepen.

    ```powershell
    $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
    $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"
    ```

## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a>LB regels, NAT-regels, een test- en een load balancer maken

In dit voorbeeld worden de volgende items gemaakt:

* een NAT-regel voor alle binnenkomend verkeer op poort 443 op poort 4443 vertalen
* een load balancer-regel om al het verkeer dat binnenkomt op poort 80, gelijk te verdelen naar poort 80 op de adressen in de back-endgroep.
* een load balancer-regel voor het RDP-verbinding met de virtuele machines op poort 3389 toestaan.
* een regel test om te controleren van de status op een pagina met de naam *HealthProbe.aspx* of een service via poort 8080
* een load balancer die gebruikmaakt van alle deze objecten

1. Maak de NAT-regels.

    ```powershell
    $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    ```

2. Maak een statustest. Er zijn twee manieren om een steekproef te configureren:

    HTTP-test

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    of de TCP-controle

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
    $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2
    ```

    In dit voorbeeld gaan we gebruiken het TCP-tests.

3. Maak een load balancer-regel.

    ```powershell
    $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389
    ```

4. De load balancer met de eerder gemaakte objecten maken.

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule
    ```

## <a name="create-nics-for-the-back-end-vms"></a>NIC's voor de back-end virtuele machines maken

1. Lees het virtuele netwerk en het virtuele netwerk, Subnet, waarbij de NIC's moeten worden gemaakt.

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name VNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. IP-configuraties en NIC's maken voor de virtuele machines.

    ```powershell
    $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
    $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
    $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

    $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
    $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
    $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'
    ```

## <a name="create-virtual-machines-and-assign-the-newly-created-nics"></a>Virtuele machines maken en toewijzen van de zojuist gemaakte NIC 's

Zie voor meer informatie over het maken van een virtuele machine [maken en vooraf configureren van een virtuele Windows-computer met Resource Manager en Azure PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)

1. Een Beschikbaarheidsset en Storage-account maken

    ```powershell
    New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
    $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
    New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName "Standard_LRS"
    $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'
    ```

2. Elke virtuele machine maken en toewijzen via de vorige gemaakt NIC 's

    ```powershell
    $mySecureCredentials= Get-Credential -Message "Type the username and password of the local administrator account."

    $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
    $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
    $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
    New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

    $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
    $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
    $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
    New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2
    ```

## <a name="next-steps"></a>Volgende stappen

[Aan de slag met het configureren van een interne load balancer](load-balancer-get-started-ilb-arm-ps.md)

[Een distributiemodus voor de load balancer configureren](load-balancer-distribution-mode.md)

[TCP-time-outinstellingen voor inactiviteit voor de load balancer configureren](load-balancer-tcp-idle-timeout.md)
