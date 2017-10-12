---
title: Een interne load balancer in Azure maken - PowerShell | Microsoft Docs
description: Meer informatie over hoe u met PowerShell een interne load balancer maakt in Resource Manager
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
tags: azure-resource-manager
ms.assetid: c6c98981-df9d-4dd7-a94b-cc7d1dc99369
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 1215ca8ff4d45e3b910b8e0ec0bd6833e4bfc308
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-internal-load-balancer-using-powershell"></a>Een interne load balancer maken met behulp van PowerShell

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Sjabloon](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../azure-resource-manager/resource-manager-deployment-model.md).  Dit artikel bevat informatie over het Resource Manager-implementatiemodel, dat door Microsoft wordt aanbevolen voor de meeste nieuwe implementaties in plaats van het [klassieke implementatiemodel](load-balancer-get-started-ilb-classic-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

In de volgende stappen wordt uitgelegd hoe u Azure Resource Manager gebruikt om een interne load balancer te maken met behulp van PowerShell. Met Azure Resource Manager worden de items die nodig zijn om een interne load balancer te maken afzonderlijk geconfigureerd en vervolgens gecombineerd om een load balancer te maken.

Maak en configureer de volgende objecten om een load balancer te implementeren:

* Front-end-IP-configuratie: hiermee configureert u het particuliere IP-adres voor inkomend netwerkverkeer.
* Back-endadresgroep: hiermee configureert u de netwerkinterfaces die het verkeer met gelijke taakverdeling ontvangen van de front-end-IP-adresgroep.
* Taakverdelingsregels: configuratie van bron- en lokale poort voor de load balancer.
* Test: hiermee configureert u de integriteitsstatustest voor de virtuele machines.
* NAT-regels voor binnenkomende verbindingen: hiermee configureert u de poortregels voor rechtstreekse toegang tot een van de virtuele machines.

Meer informatie over de load balancer-onderdelen in Azure Resource Manager vindt u op [Ondersteuning van Azure Resource Manager voor load balancer](load-balancer-arm.md).

In de volgende stappen wordt uitgelegd hoe u een load balancer tussen twee virtuele machines configureert.

## <a name="set-up-powershell-to-use-resource-manager"></a>PowerShell instellen voor het gebruik van Resource Manager

Zorg ervoor dat u de nieuwste productieversie hebt van de Azure-module voor PowerShell en dat PowerShell correct is geconfigureerd voor toegang tot uw Azure-abonnement.

### <a name="step-1"></a>Stap 1

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>Stap 2

De abonnementen voor het account controleren

```powershell
Get-AzureRmSubscription
```

U wordt gevraagd om u te verifiëren met uw referenties.

### <a name="step-3"></a>Stap 3

Kies welk Azure-abonnement u wilt gebruiken.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="create-resource-group-for-load-balancer"></a>Resourcegroep voor load balancer maken

Een nieuwe resourcegroep maken (sla deze stap over als u een bestaande resourcegroep gebruikt)

```powershell
New-AzureRmResourceGroup -Name NRP-RG -location "West US"
```

Azure Resource Manager vereist dat er voor alle resourcegroepen een locatie wordt opgegeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Zorg ervoor dat alle opdrachten voor het maken van een load balancer dezelfde resourcegroep gebruiken.

In het vorige voorbeeld is er een resourcegroep gemaakt met de naam **NRP-RG** en de locatie **VS - west**.

## <a name="create-a-virtual-network-and-a-private-ip-address-for-a-front-end-ip-pool"></a>Een virtueel netwerk en een particulier IP-adres voor een front-end-IP-adresgroep maken

Hiermee maakt u een subnet voor het virtuele netwerk en wijst u deze toe aan variabele $backendSubnet

```powershell
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
```

Een virtueel netwerk maken:

```powershell
$vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
```

Hiermee maakt u het virtuele netwerk, voegt u het subnet lb-subnet-be toe aan het virtuele netwerk NRPVNet en wijst u dit toe aan variabele $vnet

## <a name="create-a-front-end-ip-pool-and-back-end-address-pool"></a>Een front-end-IP-adresgroep en back-endadresgroep maken

Hiermee stelt u een front-end-IP-adresgroep in voor het binnenkomende netwerkverkeer van de load balancer en stelt u de back-endadresgroep in om het verkeer met gelijke taakverdeling te ontvangen.

### <a name="step-1"></a>Stap 1

Maak een front-end-IP-adresgroep met het particuliere IP-adres 10.0.2.5 voor het subnet 10.0.2.0/24, dat het eindpunt van het binnenkomende netwerkverkeer is.

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id
```

### <a name="step-2"></a>Stap 2

Stel als volgt een back-endadresgroep in die wordt gebruikt om binnenkomend verkeer van de front-end-IP-adresgroep te ontvangen:

```powershell
$beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
```

## <a name="create-load-balancing-rules-nat-rules-probe-and-load-balancer"></a>Taakverdelingsregels, NAT-regels, testen en load balancer maken

Wanneer de front-end-IP-adresgroep en de back-endadresgroep zijn gemaakt, maakt u als volgt de regels van de load balancer-resource:

### <a name="step-1"></a>Stap 1

```powershell
$inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

$inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

$healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

In het vorige voorbeeld worden de volgende items gemaakt:

* NAT-regel waarmee al het verkeer dat binnenkomt bij poort 3441, wordt omgeleid naar poort 3389.
* Een tweede NAT-regel waarmee al het verkeer dat binnenkomt bij poort 3442, wordt omgeleid naar poort 3389.
* Een load balancer-regel die ervoor zorgt dat al het verkeer dat binnenkomt bij openbare poort 80, met gelijke taakverdeling wordt omgeleid naar de lokale poort 80 in de back-endadresgroep.
* Een testregel die de integriteitsstatus test voor het pad HealthProbe.aspx

### <a name="step-2"></a>Stap 2

Maak de load balancer door alle objecten (NAT-regels, load balancer-regels, testconfiguraties) als volgt bij elkaar te voegen:

```powershell
$NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
```

## <a name="create-network-interfaces"></a>Netwerkinterfaces maken

Nadat de interne load balancer is gemaakt, moet u definiëren welke netwerkinterfaces het binnenkomende netwerkverkeer met gelijke taakverdeling, NAT-regels en test kunnen ontvangen. In dit geval is de netwerkinterface afzonderlijk geconfigureerd. Deze kan later worden toegewezen aan een virtuele machine.

### <a name="step-1"></a>Stap 1

Haal als volgt het virtuele netwerk en het subnet van de resource op om netwerkinterfaces maken:

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

$backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
```

Met deze stap maakt u een netwerkinterface die tot de back-endgroep van de load balancer behoort en koppelt u de eerste NAT-regel voor RDP voor deze netwerkinterface:

```powershell
$backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
```

### <a name="step-2"></a>Stap 2

Maak als volgt een tweede netwerkinterface met de naam LB-Nic2-BE:

Met deze stap maakt u een tweede netwerkinterface, wijst u deze toe aan dezelfde back-endgroep van de load balancer en koppelt u de tweede NAT-regel voor RDP:

```powershell
$backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
```

In het eindresultaat ziet u de volgende uitvoer:

    $backendnic1

Verwachte uitvoer:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
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
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>Stap 3

Gebruik de opdracht Add-AzureRmVMNetworkInterface om de NIC toe te wijzen aan een virtuele machine.

U vindt de stapsgewijze instructies voor het maken van een virtuele machine en de toewijzing ervan aan een NIC in de documentatie: [Een virtuele machine in Azure maken met behulp van PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

## <a name="add-the-network-interface"></a>Netwerkinterface toevoegen

Als u al een virtuele machine hebt gemaakt, kunt u de netwerkinterface met de volgende stappen toevoegen:

### <a name="step-1"></a>Stap 1

Laad de load balancer-resource in een variabele (als u dat nog niet hebt gedaan). Gebruik de variabelenaam $lb. Gebruik dezelfde namen uit de load balancer-resource die u in de stappen hiervoor hebt gemaakt.

```powershell
$lb = Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG
```

### <a name="step-2"></a>Stap 2

Laad de back-endconfiguratie in een variabele.

```powershell
$backend = Get-AzureRmLoadBalancerBackendAddressPoolConfig -name LB-backend -LoadBalancer $lb
```

### <a name="step-3"></a>Stap 3

Laad de gemaakte netwerkinterface in een variabele. Gebruik de variabelenaam $nic. Gebruik dezelfde netwerkinterfacenaam als in het vorige voorbeeld.

```powershell
$nic = Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG
```

### <a name="step-4"></a>Stap 4

Wijzig de back-endconfiguratie op de netwerkinterface.

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
```

### <a name="step-5"></a>Stap 5

Sla het netwerkinterfaceobject op.

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Zodra een netwerkinterface is toegevoegd aan de back-endgroep van de load balancer, begint deze netwerkverkeer te ontvangen op basis van de taakverdelingsregels voor die load balancer-resource.

## <a name="update-an-existing-load-balancer"></a>Een bestaande load balancer bijwerken

### <a name="step-1"></a>Stap 1
Met behulp van de load balancer uit het vorige voorbeeld wijst u het load balancer-object met Get-AzureRmLoadBalancer toe aan variabele $slb

```powershell
$slb = Get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

### <a name="step-2"></a>Stap 2

In het volgende voorbeeld voegt u aan een bestaande load balancer een nieuwe NAT-regel voor binnenkomende verbindingen toe met poort 81 in de front-end en poort 8181 voor de back-endgroep

```powershell
$slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp
```

### <a name="step-3"></a>Stap 3

De nieuwe configuratie opslaan met Set-AzureLoadBalancer

```powershell
$slb | Set-AzureRmLoadBalancer
```

## <a name="remove-a-load-balancer"></a>Een load balancer verwijderen

De opdracht Remove-AzureRmLoadBalancer gebruiken om een eerder gemaakte load balancer met de naam NRP-LB te verwijderen uit de resourcegroep NRP RG

```powershell
Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

> [!NOTE]
> Met de optionele schakeloptie -Force kunt u de prompt voor verwijdering uitschakelen.

## <a name="next-steps"></a>Volgende stappen

[Een distributiemodus voor de load balancer configureren](load-balancer-distribution-mode.md)

[TCP-time-outinstellingen voor inactiviteit voor de load balancer configureren](load-balancer-tcp-idle-timeout.md)
