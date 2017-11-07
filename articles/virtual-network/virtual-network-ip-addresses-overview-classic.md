---
title: IP-adrestypen in Azure (klassiek) | Microsoft Docs
description: Meer informatie over openbare en particuliere IP-adressen (klassiek) in Azure.
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 2f8664ab-2daf-43fa-bbeb-be9773efc978
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: jdial
ms.openlocfilehash: d5eea5e4499b9de40002ce2fc6aac39239c41b19
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="ip-address-types-and-allocation-methods-classic-in-azure"></a>IP-adrestypen en toewijzingsmethoden Azure (klassiek)
U kunt IP-adressen toewijzen aan Azure-resources om te communiceren met andere Azure-resources, uw on-premises netwerk en internet. Er zijn twee soorten IP-adressen kunt u in Azure: openbare en persoonlijke.

Openbare IP-adressen worden gebruikt voor communicatie met Internet, met inbegrip van openbare Azure-services.

Privé IP-adressen worden gebruikt voor communicatie binnen een Azure-netwerk (VNet), een cloudservice en uw on-premises netwerk wanneer u een VPN-gateway of ExpressRoute-circuit met uw netwerk uitbreiden naar Azure.

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../resource-manager-deployment-model.md).  Dit artikel gaat over het gebruik van het klassieke implementatiemodel. Microsoft raadt aan dat de meeste nieuwe implementaties Resource Manager gebruiken. Meer informatie over IP-adressen in het Resource Manager door het lezen van de [IP-adressen](virtual-network-ip-addresses-overview-arm.md) artikel.

## <a name="public-ip-addresses"></a>Openbare IP-adressen
Openbare IP-adressen worden gebruikt om Azure-resources te laten communiceren met internet en andere openbare Azure-services, zoals [Azure Redis Cache](https://azure.microsoft.com/services/cache/), [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL databases](../sql-database/sql-database-technical-overview.md) en [Azure Storage](../storage/common/storage-introduction.md).

Een openbaar IP-adres is gekoppeld aan de volgende resourcetypen:

* Cloud services
* IaaS virtuele Machines (VM's)
* PaaS-rolexemplaren
* VPN-gateways
* Toepassingsgateways

### <a name="allocation-method"></a>Toewijzingsmethode
Wanneer een openbaar IP-adres worden toegewezen aan een Azure-resource moet, is het *dynamisch* toegewezen uit een groep met beschikbare openbare IP-adres binnen de locatie van de resource is gemaakt. Dit IP-adres is uitgebracht als de resource is gestopt. Als dit gebeurt wanneer de rolinstanties zijn gestopt van een cloudservice, die kunnen worden voorkomen met behulp van een *statische* (gereserveerd) IP-adres (Zie [Cloudservices](#Cloud-services)).

> [!NOTE]
> De lijst met IP-adresbereiken waarvan openbare IP-adressen zijn toegewezen aan Azure-resources wordt gepubliceerd op [Azure Datacenter IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).
> 
> 

### <a name="dns-hostname-resolution"></a>DNS-hostnaamomzetting
Wanneer u een cloudservice of een IaaS VM maakt, moet u een cloud service DNS-naam die uniek voor alle resources in Azure is te geven. Hiermee maakt u een toewijzing in de Azure-beheerde DNS-servers voor *dnsname*. cloudapp.net het openbare IP-adres van de resource. Bijvoorbeeld, als u een cloudservice maakt met een cloud service DNS-naam van **contoso**, de volledig gekwalificeerde domeinnaam (FQDN) **contoso.cloudapp.net** wordt omgezet in een openbaar IP-adres (VIP) van de cloudservice. U kunt deze FDQN gebruiken voor het maken van een aangepaste domein-CNAME-record die verwijst naar het openbare IP-adres in Azure.

### <a name="cloud-services"></a>Cloud services
Een service in de cloud heeft altijd een openbaar IP-adres aangeduid als een virtueel IP-adres (VIP). U kunt eindpunten maken in een cloudservice om te koppelen van verschillende poorten in het VIP naar interne poorten op VM's en rolinstanties in de cloudservice. 

Een cloudservice kan meerdere IaaS VM's of PaaS-rolexemplaren, alle weergegeven via het dezelfde cloud service-VIP bevatten. U kunt ook toewijzen [meerdere VIP's op een cloudservice](../load-balancer/load-balancer-multivip.md), waardoor meerdere VIP's scenario's zoals multitenant-omgeving met websites op basis van SSL.

U kunt controleren of het openbare IP-adres van een cloudservice blijft hetzelfde, zelfs wanneer de rolinstanties zijn gestopt met behulp van een *statische* openbare IP-adres, aangeduid als [gereserveerde IP-adres](virtual-networks-reserved-public-ip.md). U kunt een statisch (gereserveerd) IP-resource maken in een specifieke locatie en deze toewijzen aan elke cloudservice op die locatie. U kunt de IP-adres voor het gereserveerde IP-adres opgeven, is het toegewezen van groep met beschikbare IP-adressen op de locatie die is gemaakt. Dit IP-adres wordt niet vrijgegeven totdat u het expliciet verwijdert.

Statisch (gereserveerd) openbare IP-adressen worden vaak gebruikt in de scenario's waarbij een cloudservice:

* firewall-regels worden ingesteld door eindgebruikers vereist.
* afhankelijk van externe DNS-naamomzetting en een dynamische IP-adres vereist updaten van A-records.
* verbruikt externe webservices die IP-gebaseerd beveiligingsmodel te gebruiken.
* maakt gebruik van SSL-certificaten die zijn gekoppeld aan een IP-adres.

> [!NOTE]
> Wanneer u een klassieke virtuele machine, een container maakt *cloudservice* wordt gemaakt door Azure, met een virtueel IP-adres (VIP). Wanneer het is gemaakt via de portal, een standaard RDP of SSH *eindpunt* is geconfigureerd door de portal, dus u verbinding met de virtuele machine via het VIP van cloud-service maken kunt. Dit VIP cloud-service kan worden gereserveerd, waarmee u effectief op een gereserveerde IP-adres verbinding maken met de virtuele machine. U kunt extra poorten openen door meer eindpunten configureren.
> 
> 

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS VM's en PaaS-rolexemplaren
U kunt een openbare IP-adres toewijzen rechtstreeks naar een IaaS [VM](../virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of PaaS-rolinstantie binnen een cloudservice. Dit wordt aangeduid als een instantieniveau openbare IP-adres ([ILPIP](virtual-networks-instance-level-public-ip.md)). Dit openbare IP-adres kan alleen dynamisch zijn.

> [!NOTE]
> Dit wijkt af van het VIP van de cloudservice, een container voor IaaS VM's of PaaS-rolexemplaren is, omdat een cloudservice kan bevatten meerdere IaaS VM's of PaaS-rolexemplaren, alle toegankelijk zijn via de dezelfde VIP voor cloud-service.
> 
> 

### <a name="vpn-gateways"></a>VPN-gateways
Een [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) kan worden gebruikt voor een Azure-VNet verbinding met andere Azure VNets of on-premises netwerken. Een VPN-gateway is een openbaar IP-adres toegewezen *dynamisch*, waardoor communicatie met het externe netwerk.

### <a name="application-gateways"></a>Toepassingsgateways
Een Azure [toepassingsgateway](../application-gateway/application-gateway-introduction.md) kan worden gebruikt voor Layer7 taakverdeling om netwerkverkeer te routeren op basis van HTTP. Toepassingsgateway een openbaar IP-adres is toegewezen *dynamisch*, die fungeert als het VIP taakverdeling.

### <a name="at-a-glance"></a>Een overzicht
De onderstaande tabel ziet elk resourcetype met de mogelijke toewijzingsmethoden (dynamische/statisch) en de mogelijkheid meerdere openbare IP-adressen toewijzen.

| Resource | Dynamisch | Statisch | Meerdere IP-adressen |
| --- | --- | --- | --- |
| Cloudservice |Ja |Ja |Ja |
| IaaS VM of PaaS rolinstantie |Ja |Nee |Nee |
| VPN-gateway |Ja |Nee |Nee |
| Toepassingsgateway |Ja |Nee |Nee |

## <a name="private-ip-addresses"></a>Privé-IP-adressen
Privé IP-adressen worden gebruikt om Azure-resources om te communiceren met andere resources in een cloudservice of een [virtueel netwerk](virtual-networks-overview.md)(VNet) of on-premises netwerk (via een VPN-gateway of ExpressRoute-circuit), zonder gebruik van een Internet-bereikbaar IP-adres.

In Azure klassieke implementatiemodel, kan een particulier IP-adres worden toegewezen aan de volgende Azure-resources:

* IaaS VM's en PaaS-rolexemplaren
* Interne load balancer
* Toepassingsgateway

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS VM's en PaaS-rolexemplaren
Virtuele machines (VM's) gemaakt met het klassieke implementatiemodel worden altijd in een cloudservice, vergelijkbaar met het PaaS-rolexemplaren geplaatst. Het gedrag van privé IP-adressen zijn dus vergelijkbaar voor deze resources.

Het is belangrijk te weten dat een cloudservice kan worden geïmplementeerd op twee manieren:

* Als een *zelfstandige* cloudservice waarin het is niet binnen een virtueel netwerk.
* Als onderdeel van een virtueel netwerk.

#### <a name="allocation-method"></a>Toewijzingsmethode
In geval van een *zelfstandige* cloudservice, een particulier IP-adres toegewezen resources-get *dynamisch* van de Azure-datacenter persoonlijke IP-adresbereik. Deze kan alleen worden gebruikt voor communicatie met andere virtuele machines in dezelfde cloudservice. Dit IP-adres kunt wijzigen wanneer de bron wordt gestopt en gestart.

Bronnen ophalen in het geval van een cloudservice die is geïmplementeerd in een virtueel netwerk, persoonlijke IP-adressen uit het adresbereik van de bijbehorende subnetten (zoals opgegeven in de netwerkconfiguratie) toegewezen. Deze persoonlijke IP-adressen kan worden gebruikt voor communicatie tussen alle virtuele machines binnen het VNet.

Bovendien een particulier IP-adres is toegewezen in geval van een cloud-services binnen een VNet, *dynamisch* (met behulp van DHCP) standaard. Deze kunt wijzigen wanneer de bron wordt gestopt en gestart. Om te controleren of het IP-adres hetzelfde blijft, moet u de toewijzingsmethode instellen op *statische*, en geef een geldig IP-adres binnen het bijbehorende adresbereik liggen.

Statische privé-IP-adressen worden vaak gebruikt voor:

* Virtuele machines die fungeren als domeincontrollers of DNS-servers.
* Virtuele machines waarvoor firewallregels IP-adressen gebruikt.
* Virtuele machines met services die worden gebruikt door andere apps via een IP-adres.

#### <a name="internal-dns-hostname-resolution"></a>Interne DNS-hostnaamomzetting
Alle virtuele Azure-machines en PaaS-rolinstanties zijn geconfigureerd met [Azure beheerde DNS-servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) standaard, tenzij u expliciet aangepaste DNS-servers configureert. Deze DNS-servers bieden interne naamomzetting voor VM's en rolexemplaren die zich in dezelfde service VNet of in de cloud bevinden.

Wanneer u een virtuele machine maakt, wordt er een toewijzing van de hostnaam aan het bijbehorende privé-IP-adres toegevoegd aan de via Azure beheerde DNS-servers. In geval van een VM meerdere NIC's, is de hostnaam toegewezen aan de persoonlijke IP-adres van de primaire NIC Deze informatie over de Identiteitstoewijzing is echter beperkt tot resources in dezelfde cloudservice- of VNet.

In geval van een *zelfstandige* cloudservice, kunt u zich kunnen omzetten van hostnamen van alle VM's / rolexemplaren binnen dezelfde cloudservice alleen. In geval van een cloudservice binnen een VNet, kunt u zich kunnen omzetten van hostnamen van alle exemplaren van virtuele machines/rol binnen het VNet.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interne load balancers (ILB) en toepassingsgateways
U kunt een privé-IP-adres toewijzen aan de **front-end**-configuratie van een [interne Azure Load Balancer](../load-balancer/load-balancer-internal-overview.md) (ILB) of een [Azure Application Gateway](../application-gateway/application-gateway-introduction.md). Dit privé-IP-adres fungeert als een intern eindpunt dat alleen toegankelijk is voor de resources binnen het virtuele netwerk (VNet) en de externe netwerken die met het VNet zijn verbonden. U kunt een dynamisch of statisch privé-IP-adres toewijzen aan de front-end-configuratie. U kunt ook meerdere particuliere IP-adressen scenario's voor meerdere VIP's te maken.

### <a name="at-a-glance"></a>Een overzicht
De onderstaande tabel ziet elk resourcetype met de mogelijke toewijzingsmethoden (dynamische/statisch) en de mogelijkheid meerdere persoonlijke IP-adressen toewijzen.

| Resource | Dynamisch | Statisch | Meerdere IP-adressen |
| --- | --- | --- | --- |
| Virtuele machine (in een *zelfstandige* cloud service of VNet) |Ja |Ja |Ja |
| De rolinstantie PaaS (in een *zelfstandige* cloud service of VNet) |Ja |Nee |Nee |
| Interne load balancer-front-end |Ja |Ja |Ja |
| Application gateway-front-end |Ja |Ja |Ja |

## <a name="limits"></a>Limieten
De onderstaande tabel ziet de beperkingen op IP-adressering in Azure per abonnement. U kunt [contact opnemen met ondersteuning](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als u op basis van uw bedrijfsbehoeften de standaardlimieten wilt verhogen tot de maximumlimieten.

|  | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| Openbare IP-adressen (dynamisch) |5 |contact met ondersteuning |
| Gereserveerde openbare IP-adressen |20 |contact met ondersteuning |
| Openbare VIP per implementatie (cloudservice) |5 |contact met ondersteuning |
| Persoonlijke VIP (ILB) per implementatie (cloudservice) |1 |1 |

Zorg ervoor dat u de volledige set lezen [voor netwerken beperkt](../azure-subscription-service-limits.md#networking-limits) in Azure.

## <a name="pricing"></a>Prijzen
In de meeste gevallen zijn openbare IP-adressen beschikbaar. Er is een nominaal gratis extra en/of statische openbare IP-adressen gebruiken. Zorg dat u begrijpt de [structuur prijzen voor openbare IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="differences-between-resource-manager-and-classic-deployments"></a>Verschillen tussen Resource Manager en klassieke implementaties
Hieronder volgt een vergelijking van IP-adressering functies in het Resource Manager en het klassieke implementatiemodel.

|  | Resource | Klassiek | Resource Manager |
| --- | --- | --- | --- |
| **Openbare IP-adres** |***VM*** |Aangeduid als een ILPIP (alleen dynamisch) |Aangeduid als een openbaar IP-adres (dynamische of statische) |
|  ||Toegewezen aan een IaaS-VM of een PaaS-rolinstantie |Gekoppeld aan de VM-NIC | |
|  |***Internet gerichte load balancer*** |VIP (dynamische) of gereserveerd IP-adres (statische) genoemd |Aangeduid als een openbaar IP-adres (dynamische of statische) | |
|  ||Toegewezen aan een cloudservice |Gekoppeld aan de load balancer-front-end configuratie | |
|  | | | |
| **Particulier IP-adres** |***VM*** |Aangeduid als een DIP |Aangeduid als een persoonlijk IP-adres |
|  ||Toegewezen aan een IaaS-VM of een PaaS-rolinstantie |Toegewezen aan de VM-NIC | |
|  |***Interne load balancer (ILB)*** |Toegewezen aan de ILB (dynamische of statische) |Toegewezen aan de ILB front-end configuratie (dynamisch of statisch) | |

## <a name="next-steps"></a>Volgende stappen
* [Een virtuele machine met een statisch privé IP-adres implementeren](virtual-networks-static-private-ip-classic-pportal.md) met de Azure portal.

