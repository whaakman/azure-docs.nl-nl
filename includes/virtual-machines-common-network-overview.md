---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 11/01/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: cd3b7d6cc75afc5d83ff02a15b920d9f8b05f608
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66391374"
---
Wanneer u een virtuele Azure-machine maakt, moet u een [virtueel netwerk](../articles/virtual-network/virtual-networks-overview.md) (VNet) maken of een bestaand VNet gebruiken. U moet ook bepalen hoe uw virtuele machines kunnen worden geopend via de VNet. Het is belangrijk om [een planning te maken voordat u resources maakt](../articles/virtual-network/virtual-network-vnet-plan-design-arm.md) en ervoor te zorgen dat u op de hoogte bent van de [limieten van netwerkresources](../articles/azure-subscription-service-limits.md#networking-limits).

Op de volgende afbeelding worden virtuele machines weergegeven als webservers en databaseservers. Elke set van virtuele machines is toegewezen aan afzonderlijke subnetten in de VNet.

![Azure Virtual Network](./media/virtual-machines-common-network-overview/vnetoverview.png)

U kunt een VNet maken voordat u een virtuele machine maken of u kunt bij het maken van een virtuele machine. U maakt deze resources ter ondersteuning van de communicatie met een virtuele machine:

- Netwerkinterfaces
- IP-adressen
- Virtueel netwerk en subnetten

Naast deze basisresources, moet u ook deze optionele resources overwegen:

- Netwerkbeveiligingsgroepen
- Load balancers 

[!INCLUDE [updated-for-az](./updated-for-az.md)]

## <a name="network-interfaces"></a>Netwerkinterfaces

Een [netwerkinterface (NIC)](../articles/virtual-network/virtual-network-network-interface.md) is de onderlinge verbinding tussen een virtuele machine en een virtueel netwerk (VNet). Een virtuele machine moet ten minste één NIC hebben, maar kan er meer dan een hebben, afhankelijk van de grootte van de virtuele machine die u maakt. Meer informatie over het aantal NIC's van elke VM-grootte ondersteunt voor [Windows](../articles/virtual-machines/windows/sizes.md) of [Linux](../articles/virtual-machines/linux/sizes.md).

U kunt een virtuele machine maken met meerdere NIC's, en u kunt ook toevoegen of verwijderen van NIC's gedurende de levensduur van een virtuele machine. Meerdere NIC's kunnen een virtuele machine verbinding maken met verschillende subnetten en verzenden of ontvangen van verkeer via de meest geschikte interface. Virtuele machines met een willekeurig aantal netwerkinterfaces kunnen bestaan in dezelfde beschikbaarheidsset, tot aan het aantal dat wordt ondersteund door de VM-grootte. 

Elke NIC die is gekoppeld aan een virtuele machine moet bestaan op dezelfde locatie en in hetzelfde abonnement als de virtuele machine. Elke NIC moet zijn verbonden met een VNet die bestaat op dezelfde Azure-locatie en in hetzelfde abonnement als de NIC. Kunt u het subnet dat een virtuele machine is verbonden met nadat deze gemaakt, maar u kunt het VNet niet wijzigen. Aan elke NIC die is gekoppeld aan een virtuele machine wordt een MAC-adres toegewezen dat niet wordt gewijzigd totdat de virtuele machine wordt verwijderd.

Deze tabel bevat de methoden die u kunt gebruiken voor het maken van een netwerkinterface.

| Methode | Description |
| ------ | ----------- |
| Azure Portal | Wanneer u een virtuele machine in Azure Portal maakt, wordt automatisch een netwerkinterface voor u gemaakt (u kunt niet een NIC gebruiken die u afzonderlijk hebt gemaakt). De portal maakt een virtuele machine met slechts één NIC. Als u een virtuele machine met meer dan één NIC wilt maken, moet u deze met een andere methode maken. |
| [Azure PowerShell](../articles/virtual-machines/windows/multiple-nics.md) | Gebruik [New-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) met de **- PublicIpAddressId** parameter op te geven van de id van het openbare IP-adres dat u eerder hebt gemaakt. |
| [Azure-CLI](../articles/virtual-machines/linux/multiple-nics.md) | Voor de id van het openbare IP-adres dat u eerder hebt gemaakt, gebruikt u [az network nic maken](https://docs.microsoft.com/cli/azure/network/nic) met de **--public-ip-adres** parameter. |
| [Sjabloon](../articles/virtual-network/template-samples.md) | Gebruik [Netwerkinterface in een virtueel netwerk met openbaar IP-adres](https://github.com/Azure/azure-quickstart-templates/tree/master/101-nic-publicip-dns-vnet) als richtlijn voor het implementeren van een netwerkinterface met behulp van een sjabloon. |

## <a name="ip-addresses"></a>IP-adressen 

U kunt deze soorten [IP-adressen](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md) toewijzen aan een NIC in Azure:

- **Openbare IP-adressen**: worden gebruikt om inkomend en uitgaand te communiceren (zonder NAT (netwerkadresomzetting)) met internet en andere Azure-resources die niet zijn verbonden met een VNet. Het toewijzen van een openbaar IP-adres aan een NIC is optioneel. Openbare IP-adressen hebben geringe kosten en er is een maximum aantal dat per abonnement kan worden gebruikt.
- **Privé IP-adressen**: worden gebruikt voor communicatie binnen een VNet, uw on-premises netwerk en internet (met NAT). U moet ten minste één privé IP-adres toewijzen aan een virtuele machine. Lees [Uitgaande verbindingen in Azure](../articles/load-balancer/load-balancer-outbound-connections.md) voor meer informatie over NAT in Azure.

U kunt openbare IP-adressen toewijzen aan virtuele machines of internetgerichte load balancers. U kunt privé IP-adressen toewijzen aan virtuele machines en interne load balancers. U kunt IP-adressen toewijzen aan een virtuele machine met behulp van een netwerkinterface.

Er zijn twee methoden voor het toewijzen van een IP-adres aan een resource: dynamisch en statisch. De standaardtoewijzingsmethode is dynamisch, waarbij het IP-adres niet wordt toegewezen wanneer het wordt gemaakt. In plaats daarvan wordt het IP-adres toegewezen wanneer u een virtuele machine maakt of een gestopte virtuele machine start. Het IP-adres wordt weer vrijgegeven wanneer u de virtuele machine stopt of verwijdert. 

Als u wilt dat het IP-adres voor de virtuele machine hetzelfde blijft, kunt u de toewijzingsmethode expliciet instellen op statisch. In dat geval wordt er onmiddellijk een IP-adres toegewezen. Het IP-adres wordt alleen vrijgegeven wanneer u de virtuele machine verwijdert of de toewijzingsmethode wijzigt in dynamisch.
    
Deze tabel bevat de methoden die u kunt gebruiken voor het maken van een IP-adres.

| Methode | Description |
| ------ | ----------- |
| [Azure Portal](../articles/virtual-network/virtual-network-deploy-static-pip-arm-portal.md) | Standaard zijn openbare IP-adressen dynamisch, en het adres dat eraan is gekoppeld kan worden gewijzigd wanneer de virtuele machine wordt gestopt of verwijderd. Om ervoor te zorgen dat de virtuele machine altijd hetzelfde openbare IP-adres gebruikt, maakt u een statisch openbaar IP-adres. De portal wijst standaard een dynamisch privé IP-adres toe aan een NIC bij het maken van een virtuele machine. U kunt dit IP-adres wijzigen in statisch nadat de virtuele machine is gemaakt.|
| [Azure PowerShell](../articles/virtual-network/virtual-network-deploy-static-pip-arm-ps.md) | U gebruikt [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) met de **- AllocationMethod** parameter als dynamisch of statisch. |
| [Azure-CLI](../articles/virtual-network/virtual-network-deploy-static-pip-arm-cli.md) | U gebruikt [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip) met de parameter **--allocation-method** als dynamisch of statisch. |
| [Sjabloon](../articles/virtual-network/template-samples.md) | Gebruik [Netwerkinterface in een virtueel netwerk met openbaar IP-adres](https://github.com/Azure/azure-quickstart-templates/tree/master/101-nic-publicip-dns-vnet) als richtlijn voor het implementeren van een openbaar IP-adres met behulp van een sjabloon. |

Nadat u een openbaar IP-adres hebt gemaakt, kunt u dit aan een virtuele machine koppelen door het aan een NIC toe te wijzen.

## <a name="virtual-network-and-subnets"></a>Virtueel netwerk en subnetten

Een subnet is een bereik met IP-adressen in de VNet. U kunt u een VNET onderverdelen in meerdere subnetten voor organisatie- en beveiligingsdoeleinden. Elke NIC in een virtuele machine is verbonden met één subnet in één VNet. NIC’s die verbonden zijn met (dezelfde of verschillende) subnetten binnen een VNET kunnen met elkaar communiceren zonder extra configuratie.

Wanneer u een VNet instelt, geeft u de topologie op, met inbegrip van de beschikbare adresruimten en subnetten. Als de VNet moet worden verbonden met andere VNets of on-premises netwerken, moet u adresbereiken selecteren die elkaar niet overlappen. De IP-adressen zijn privé en kunnen niet worden geopend via Internet, die alleen voor de niet-routeerbare IP-adressen zoals 10.0.0.0/8, 172.16.0.0/12 of 192.168.0.0/16. Azure behandelt elk adresbereik nu als onderdeel van de privé VNet IP-adresruimte die alleen bereikbaar is binnen de VNet, binnen onderling verbonden VNets en vanaf uw on-premises locatie. 

Als u binnen een organisatie werkt waarin iemand anders verantwoordelijk is voor de interne netwerken, moet u voordat u uw adresruimte selecteert met die persoon overleggen. Zorg ervoor dat er geen overlap is en laat deze persoon weten welke ruimte u wilt gebruiken, zodat deze niet probeert hetzelfde bereik met IP-adressen te gebruiken. 

Standaard zijn er geen beveiligingsgrenzen tussen subnetten, zodat virtuele machines in elk van deze subnetten met elkaar kunnen communiceren. U kunt echter netwerkbeveiligingsgroepen (NSG's) instellen, waarmee u het netwerkverkeer naar en van subnetten en naar en van virtuele machines kunt beheren. 

Deze tabel bevat de methoden die u kunt gebruiken voor het maken van een VNet en subnetten. 

| Methode | Description |
| ------ | ----------- |
| [Azure Portal](../articles/virtual-network/quick-create-portal.md) | Als u een VNet laat maken door Azure wanneer u een virtuele machine maakt, is de naam een combinatie van de naam van de resourcegroep die de VNet bevat en **-vnet**. De adresruimte is 10.0.0.0/24, de vereiste subnetnaam is **default** en het adresbereik van het subnet is 10.0.0.0/24. |
| [Azure PowerShell](../articles/virtual-network/quick-create-powershell.md) | U gebruikt [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworkSubnetConfig) en [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork) om een subnet en een VNet te maken. U kunt ook [toevoegen AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/Az.Network/Add-AzVirtualNetworkSubnetConfig) een subnet toevoegen aan een bestaand VNet. |
| [Azure-CLI](../articles/virtual-network/quick-create-cli.md) | Het subnet en de VNet worden op hetzelfde moment gemaakt. Geef een **--subnet-name**-parameter op voor [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) met de subnetnaam. |
| Template | De eenvoudigste manier om een VNet en subnetten te maken is voor het downloaden van een bestaande sjabloon, zoals [Virtueelnetwerk met twee subnetten](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets), en voor uw behoeften te wijzigen. |

## <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen

Een [netwerkbeveiligingsgroep (NSG)](../articles/virtual-network/virtual-network-vnet-plan-design-arm.md) bevat een lijst met ACL-regels (Access Control List, toegangsbeheerlijst) waarmee netwerkverkeer naar subnetten, NIC’s of beide wordt toegestaan of geweigerd. NSG's kunnen worden gekoppeld aan subnetten of afzonderlijke NIC’s die zijn verbonden met een subnet. Als een NSG is gekoppeld aan een subnet, zijn de ACL-regels van toepassing op alle virtuele machines in dat subnet. U kunt het verkeer naar een afzonderlijke NIC beperken door een NSG rechtstreeks aan een NIC te koppelen.

NSG's bevatten twee sets met regels: een set met regels voor binnenkomend verkeer en een set met regels voor uitgaand verkeer. De prioriteit voor een regel moet uniek zijn binnen elke set. Elke regel heeft eigenschappen voor protocol, bron- en doelpoortbereik, adresvoorvoegsels, richting van verkeer, prioriteit en toegangstype. 

Alle NSG's bevatten een set met standaardregels. De standaardregels kunnen niet worden verwijderd, maar omdat ze de laagste prioriteit hebben, kunnen ze worden overschreven door de regels die u maakt. 

Als u een NSG aan een NIC koppelt, worden de netwerktoegangsregels in de NSG alleen toegepast op die NIC. Als een NSG wordt toegepast op één NIC op een virtuele machine met meerdere NIC’s, heeft dat geen invloed op verkeer naar de andere NIC's. U kunt verschillende NSG's koppelen aan een NIC (of virtuele machine, afhankelijk van het implementatiemodel) en het subnet dat met een NIC of VM is verbonden. Prioriteit wordt verleend op basis van de richting van het verkeer.

Zorg ervoor dat u uw NSG’s [plant](../articles/virtual-network/virtual-network-vnet-plan-design-arm.md) wanneer u uw virtuele machines en VNet aan het plannen bent.

Deze tabel bevat de methoden die u kunt gebruiken voor het maken van een netwerkbeveiligingsgroep.

| Methode | Description |
| ------ | ----------- |
| [Azure Portal](../articles/virtual-network/tutorial-filter-network-traffic.md) | Wanneer u een virtuele machine in Azure Portal maakt, wordt een NSG automatisch gemaakt en gekoppeld aan de NIC die door de Portal wordt gemaakt. De naam van de NSG is een combinatie van de naam van de virtuele machine en **-nsg**. Deze NSG bevat een regel voor inkomende verbindingen met een prioriteit van 1000, de service ingesteld op RDP, het protocol ingesteld op TCP, de poort ingesteld op 3389 en de actie ingesteld op Toestaan. Als u ander inkomend verkeer op de virtuele machine wilt toestaan, moet u extra regels toevoegen aan de NSG. |
| [Azure PowerShell](../articles/virtual-network/tutorial-filter-network-traffic.md) | Gebruik [New-AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig) en geef de vereiste regelinformatie. Gebruik [New-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup) te maken van de NSG. Gebruik [Set AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/set-azvirtualnetworksubnetconfig) het configureren van de Netwerkbeveiligingsgroep voor het subnet. Gebruik [Set AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/set-azvirtualnetwork) de NSG toevoegt aan het VNet. |
| [Azure-CLI](../articles/virtual-network/tutorial-filter-network-traffic-cli.md) | Gebruik [az network nsg create](https://docs.microsoft.com/cli/azure/network/nsg) om de NSG te maken. Gebruik [az network nsg rule create](https://docs.microsoft.com/cli/azure/network/nsg/rule) om regels toe te voegen aan de NSG. Gebruik [az network vnet subnet update](https://docs.microsoft.com/cli/azure/network/vnet/subnet) om de NSG toe te voegen aan het subnet. |
| [Sjabloon](../articles/virtual-network/template-samples.md) | Gebruik [Een netwerkbeveiligingsgroep maken](https://github.com/Azure/azure-quickstart-templates/tree/master/101-security-group-create) als richtlijn voor het implementeren van een netwerkbeveiligingsgroep met behulp van een sjabloon. |

## <a name="load-balancers"></a>Load balancers

[Azure Load Balancer](../articles/load-balancer/load-balancer-overview.md) zorgt dat uw toepassingen een hoge beschikbaarheid hebben en goede netwerkprestaties leveren. Een load balancer kan worden geconfigureerd om [inkomend internetverkeer te reguleren](../articles/load-balancer/load-balancer-internet-overview.md) voor virtuele machines of [verkeer tussen virtuele machines in een VNet te reguleren](../articles/load-balancer/load-balancer-internal-overview.md). Een load balancer kan ook verkeer tussen on-premises computers en virtuele machines in een cross-premises netwerk reguleren, of extern verkeer doorsturen naar een specifieke virtuele machine.

De load balancer wijst inkomend en uitgaand verkeer toe tussen het openbare IP-adres en de poort van de load balancer en het privé IP-adres en poort van de virtuele machine.

Wanneer u een load balancer maakt, moet u ook rekening houden met deze configuratie-elementen:

- **Front-end IP-adresconfiguratie**: een load balancer kan een of meer front-end IP-adressen bevatten, ook wel bekend als virtuele IP-adressen (VIP's). Deze IP-adressen fungeren als ingang voor het verkeer.
- **Back-end-adrespool**: IP-adressen die zijn gekoppeld aan de NIC waarop de taakverdeling wordt gedistribueerd.
- **NAT-regels**: definieert hoe inkomend verkeer via de front-end IP gaat en naar het back-end IP-adres wordt gedistribueerd.
- **Load balancer-regels**: wijst een bepaalde combinatie van front-end IP en poort toe aan een combinatie van een set van back-end IP-adressen en poort. Een enkele load balancer kan meerdere regels voor taakverdeling bevatten. Elke regel is een combinatie van een front-end IP en poort en back-end IP en poort die is gekoppeld aan virtuele machines.
- **[Tests](../articles/load-balancer/load-balancer-custom-probe-overview.md)** : controleert de status van virtuele machines. Wanneer een test niet reageert, stopt de load balancer met het verzenden van nieuwe verbindingen naar de slechte virtuele machine. De bestaande verbindingen worden hierdoor niet beïnvloed en nieuwe verbindingen worden naar goede virtuele machines verzonden.

Deze tabel bevat de methoden die u kunt gebruiken voor het maken van een internetgerichte load balancer.

| Methode | Description |
| ------ | ----------- |
| Azure Portal |  U kunt [taakverdeling instellen voor internetverkeer virtuele machines met behulp van de Azure-portal](../articles/load-balancer/tutorial-load-balancer-standard-manage-portal.md). |
| [Azure PowerShell](../articles/load-balancer/load-balancer-get-started-internet-arm-ps.md) | Voor de id van het openbare IP-adres dat u eerder hebt gemaakt, gebruikt u [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig) met de **- PublicIpAddress** parameter. Gebruik [New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig) om de configuratie van de back-end-adresgroep te maken. Gebruik [New-AzLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig) te maken van inkomende NAT-regels die zijn gekoppeld aan de front-end-IP-configuratie die u hebt gemaakt. Gebruik [New-AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerprobeconfig) te maken van de tests die u nodig hebt. Gebruik [New-AzLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerruleconfig) te maken van de load balancer-configuratie. Gebruik [New-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancer) te maken van de load balancer.|
| [Azure-CLI](../articles/load-balancer/load-balancer-get-started-internet-arm-cli.md) | Gebruik [az network lb create](https://docs.microsoft.com/cli/azure/network/lb) voor het maken van de eerste load balancer-configuratie. Gebruik [az network lb frontend-ip create](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip) om het openbare IP-adres dat u eerder hebt gemaakt toe te voegen. Gebruik [az network lb address-pool create](https://docs.microsoft.com/cli/azure/network/lb/address-pool) om de configuratie van de back-end-adrespool toe te voegen. Gebruik [az network lb inbound-nat-rule create](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule) om NAT-regels toe te voegen. Gebruik [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule) om de load balancer-regels toe te voegen. Gebruik [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe) om de tests toe te voegen. |
| [Sjabloon](../articles/load-balancer/load-balancer-get-started-internet-arm-template.md) | Gebruik [2 virtuele machines in een load balancer en NAT-regels configureren op de LB](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-natrules) als richtlijn voor het implementeren van een load balancer met behulp van een sjabloon. |
    
Deze tabel bevat de methoden die u kunt gebruiken voor het maken van een interne load balancer.

| Methode | Description |
| ------ | ----------- |
| Azure Portal | U kunt [saldo interne verkeer load met een basisversie van load balancer in Azure portal](../articles/load-balancer/tutorial-load-balancer-basic-internal-portal.md). |
| [Azure PowerShell](../articles/load-balancer/load-balancer-get-started-ilb-arm-ps.md) | Voor een privé IP-adres in het subnet van het netwerk, gebruikt u [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig) met de **- PrivateIpAddress** parameter. Gebruik [New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig) om de configuratie van de back-end-adresgroep te maken. Gebruik [New-AzLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig) te maken van inkomende NAT-regels die zijn gekoppeld aan de front-end-IP-configuratie die u hebt gemaakt. Gebruik [New-AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerprobeconfig) te maken van de tests die u nodig hebt. Gebruik [New-AzLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerruleconfig) te maken van de load balancer-configuratie. Gebruik [New-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancer) te maken van de load balancer.|
| [Azure-CLI](../articles/load-balancer/load-balancer-get-started-ilb-arm-cli.md) | Gebruik de opdracht [az network lb create](https://docs.microsoft.com/cli/azure/network/lb) voor het maken van de eerste load balancer-configuratie. Gebruik [az network lb frontend-ip create](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip) met de parameter **--private-ip-address** voor het definiëren van het privé IP-adres. Gebruik [az network lb address-pool create](https://docs.microsoft.com/cli/azure/network/lb/address-pool) om de configuratie van de back-end-adrespool toe te voegen. Gebruik [az network lb inbound-nat-rule create](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule) om NAT-regels toe te voegen. Gebruik [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule) om de load balancer-regels toe te voegen. Gebruik [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe) om de tests toe te voegen.|
| [Sjabloon](../articles/load-balancer/load-balancer-get-started-ilb-arm-template.md) | Gebruik [2 virtuele machines in een load balancer en NAT-regels configureren op de LB](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer) als richtlijn voor het implementeren van een load balancer met behulp van een sjabloon. |

## <a name="vms"></a>VM's

Virtuele machines kunnen worden gemaakt in dezelfde VNet en ze kunnen verbinding met elkaar maken met behulp van privé IP-adressen. Ze kunnen zelfs verbinding maken als ze zich in verschillende subnetten bevinden zonder de noodzaak een gateway te configureren of openbare IP-adressen te gebruiken. Als u virtuele machines in een VNet wilt plaatsen, maakt u de VNet, en als u elke virtuele machine maakt, wijst u deze toe aan de VNet en het subnet. VM's verkrijgen hun netwerkinstellingen tijdens de implementatie of tijdens het opstarten.  

Aan virtuele machines wordt een IP-adres toegewezen wanneer deze worden geïmplementeerd. Als u meerdere virtuele machines in een VNet of subnet implementeert, worden er IP-adressen aan toegewezen als ze opstarten. U kunt ook een statisch IP-adres toewijzen aan een virtuele machine. Als u een statisch IP-adres toewijst, moet u overwegen een specifiek subnet om te voorkomen dat per ongeluk een statisch IP-adres voor een andere virtuele machine.  

Als u een virtuele machine maakt en deze later wilt migreren naar een VNet, is het niet eenvoudig om de configuratie aan te passen. U moet de virtuele machine opnieuw implementeren in de VNet. De makkelijkste manier om de virtuele machine opnieuw te implementeren, is de virtuele machine te verwijderen, maar niet alle gekoppelde schijven te verwijderen, en vervolgens de virtuele machine opnieuw te maken met behulp van de oorspronkelijke schijven in de VNet. 

Deze tabel bevat de methoden die u kunt gebruiken voor het maken van een virtuele machine in een VNet.

| Methode | Description |
| ------ | ----------- |
| [Azure Portal](../articles/virtual-machines/windows/quick-create-portal.md) | Gebruikt de standaardinstellingen van het netwerk die eerder zijn genoemd om een virtuele machine te maken met een enkele NIC. Voor het maken van een virtuele machine met meerdere NIC's, moet u een andere methode gebruiken. |
| [Azure PowerShell](../articles/virtual-machines/windows/tutorial-manage-vm.md) | Omvat het gebruik van [toevoegen AzVMNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/add-azvmnetworkinterface) de NIC die u eerder hebt gemaakt aan de VM-configuratie toevoegen. |
| [Azure-CLI](../articles/virtual-machines/linux/create-cli-complete.md) | Maken en een virtuele machine verbinden met een Vnet, subnet en NIC die bouwen als afzonderlijke stappen. |
| [Sjabloon](../articles/virtual-machines/windows/ps-template.md) | Gebruik [Zeer eenvoudige implementatie van een virtuele Windows-machine](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) als richtlijn voor het implementeren van een virtuele machine met behulp van een sjabloon. |

## <a name="next-steps"></a>Volgende stappen
Zie voor VM-specifieke stappen over het beheren van virtuele Azure-netwerken voor virtuele machines, de [Windows](../articles/virtual-machines/windows/tutorial-virtual-network.md) of [Linux](../articles/virtual-machines/linux/tutorial-virtual-network.md) zelfstudies.

Er zijn ook zelfstudies over hoe u taakverdeling tussen VM's en maak maximaal beschikbare toepassingen voor [Windows](../articles/virtual-machines/windows/tutorial-load-balancer.md) of [Linux](../articles/virtual-machines/linux/tutorial-load-balancer.md).

- Het configureren van [door de gebruiker gedefinieerde routes en doorsturen via IP](../articles/virtual-network/virtual-networks-udr-overview.md). 
- Het configureren van [VNet-naar-VNet-verbindingen](../articles/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).
- [Problemen met routes oplossen](../articles/virtual-network/diagnose-network-routing-problem.md).
