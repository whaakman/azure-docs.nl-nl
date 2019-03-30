---
title: IP-adrestypen in Azure (klassiek)
titlesuffix: Azure Virtual Network
description: Meer informatie over openbare en privé IP-adressen (klassiek) in Azure.
services: virtual-network
documentationcenter: na
author: genlin
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: genli
ms.openlocfilehash: 9e7a5772dd1e10abf43eddf0548833d625ecfb24
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652164"
---
# <a name="ip-address-types-and-allocation-methods-classic-in-azure"></a>IP-adrestypen en toewijzingsmethoden (klassiek) in Azure
U kunt IP-adressen toewijzen aan Azure-resources om te communiceren met andere Azure-resources, uw on-premises netwerk en internet. Er zijn twee soorten IP-adressen die u in Azure gebruiken kunt: openbare en privéclouds.

Openbare IP-adressen worden gebruikt voor communicatie met Internet, met inbegrip van openbare Azure services.

Privé-IP-adressen worden gebruikt voor communicatie in een Azure-netwerk (VNet), een service in de cloud en uw on-premises netwerk wanneer u een VPN-gateway of ExpressRoute-circuit met uw netwerk uitbreiden naar Azure.

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources:  [Resource Manager en het klassieke model](../resource-manager-deployment-model.md).  Dit artikel gaat over het gebruik van het klassieke implementatiemodel. Microsoft raadt aan dat de meeste nieuwe implementaties Resource Manager gebruiken. Meer informatie over IP-adressen in Resource Manager met het lezen van de [IP-adressen](virtual-network-ip-addresses-overview-arm.md) artikel.

## <a name="public-ip-addresses"></a>Openbare IP-adressen
Openbare IP-adressen gebruikt om Azure-resources om te communiceren met Internet en Azure openbare services zoals [Azure Cache voor Redis](https://azure.microsoft.com/services/cache/), [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL-databases](../sql-database/sql-database-technical-overview.md), en [Azure storage](../storage/common/storage-introduction.md).

Een openbaar IP-adres is gekoppeld aan de volgende resourcetypen:

* Cloud services
* IaaS virtuele Machines (VM's)
* PaaS-rolexemplaren
* VPN-gateways
* Toepassingsgateways

### <a name="allocation-method"></a>Toewijzingsmethode
Wanneer u een openbaar IP-adres moet worden toegewezen aan een Azure-resource, is het *dynamisch* toegewezen vanuit een pool van beschikbare openbare IP-adres binnen de locatie van de resource is gemaakt. Dit IP-adres wordt vrijgegeven wanneer de resource is gestopt. Met cloudservice, gebeurt dit wanneer alle rolinstanties zijn gestopt, die kunnen worden voorkomen met behulp van een *statische* (gereserveerde) IP-adres (Zie [Cloudservices](#cloud-services)).

> [!NOTE]
> De lijst met IP-adresbereiken waaruit openbare IP-adressen worden toegewezen aan Azure-resources wordt gepubliceerd op [Azure Datacenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653).
> 
> 

### <a name="dns-hostname-resolution"></a>DNS-hostnaamomzetting
Wanneer u een cloudservice of een IaaS-VM maakt, moet u een cloud service DNS-naam die uniek is voor alle resources in Azure op te geven. Hiermee maakt u een toewijzing in de Azure beheerde DNS-servers voor *dnsname*. cloudapp.net naar het openbare IP-adres van de resource. Bijvoorbeeld, als u een cloudservice maakt met een cloud service DNS-naam van **contoso**, de volledig gekwalificeerde domeinnaam (FQDN) **contoso.cloudapp.net** omgezet in een openbare IP-adres (VIP) van de cloud de service. U kunt deze FDQN gebruiken voor het maken van een aangepaste domein-CNAME-record die verwijst naar het openbare IP-adres in Azure.

### <a name="cloud-services"></a>Cloud services
Een cloudservice heeft altijd een openbaar IP-adres aangeduid als een virtueel IP-adres (VIP). U kunt eindpunten maken in een cloudservice om te koppelen van verschillende poorten in het VIP naar interne poorten op VM's en rolexemplaren in de cloudservice. 

Een service in de cloud kan meerdere IaaS-VM's of PaaS-rolexemplaren, allemaal weergegeven door de dezelfde cloud service-VIP bevatten. U kunt ook toewijzen [meerdere VIP's aan een cloudservice](../load-balancer/load-balancer-multivip.md), waarmee een multi-VIP-scenario's zoals multitenant-omgeving met SSL gebaseerde websites.

U kunt controleren of het openbare IP-adres van een cloudservice blijft hetzelfde, zelfs wanneer alle rolinstanties zijn gestopt, met behulp van een *statische* openbaar IP-adres, aangeduid als [gereserveerde IP-adres](virtual-networks-reserved-public-ip.md). U kunt een statisch (gereserveerd) IP-resource in een specifieke locatie maken en deze toewijzen aan elke cloudservice op die locatie. U kunt het IP-adres opgeven voor het gereserveerde IP-adres, dit wordt toegewezen uit een pool van beschikbare IP-adressen op de locatie die wordt gemaakt. Dit IP-adres wordt pas vrijgegeven wanneer u deze expliciet verwijderen.

Statisch (gereserveerd), openbare IP-adressen worden vaak gebruikt in de scenario's waarin een service in de cloud:

* firewall-regels worden ingesteld door eindgebruikers vereist.
* afhankelijk van externe DNS-naamomzetting, en een dynamische IP-adres moet bij het bijwerken van de A-records.
* externe webservices die gebruikmaken van IP-gebaseerd beveiligingsmodel verbruikt.
* maakt gebruik van SSL-certificaten die zijn gekoppeld aan een IP-adres.

> [!NOTE]
> Wanneer u een klassieke virtuele machine, een container maakt *cloudservice* wordt gemaakt door Azure, met een virtueel IP-adres (VIP). Wanneer het maken van de vindt plaats via de portal, een standaard RDP of SSH *eindpunt* wordt geconfigureerd via de portal, zodat u verbinding met de virtuele machine via het VIP van cloud-service maken kunt. Deze VIP van cloud-service kan worden gereserveerd, waarmee daadwerkelijk een gereserveerd IP-adres verbinding maken met de virtuele machine. U kunt aanvullende poorten openen door te meer eindpunten configureren.
> 
> 

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS-VM's en PaaS-rolexemplaren
U kunt een openbaar IP-adres toewijzen rechtstreeks aan een IaaS [VM](../virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of PaaS-rolinstantie binnen een cloudservice. Dit wordt aangeduid als een openbaar IP-adres van op exemplaarniveau ([ILPIP](virtual-networks-instance-level-public-ip.md)). Dit openbare IP-adres kan alleen worden dynamisch.

> [!NOTE]
> Dit wijkt af van het VIP van de cloudservice, die een container voor IaaS-VM's of PaaS rolinstanties, omdat een service in de cloud kan bevatten meerdere IaaS-VM's of PaaS-rolexemplaren, alle die toegankelijk is via de dezelfde cloud service VIP is.
> 
> 

### <a name="vpn-gateways"></a>VPN-gateways
Een [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) kan worden gebruikt om een Azure VNet verbinden met andere Azure-VNets of on-premises netwerken. Een VPN-gateway is een openbaar IP-adres toegewezen *dynamisch*, die communicatie met het externe netwerk mogelijk maakt.

### <a name="application-gateways"></a>Toepassingsgateways
Een Azure [Application gateway](../application-gateway/application-gateway-introduction.md) kan worden gebruikt voor Layer7-taakverdeling van netwerkverkeer op basis van HTTP. Application gateway is een openbaar IP-adres toegewezen *dynamisch*, die fungeert als het VIP met taakverdeling.

### <a name="at-a-glance"></a>Een overzicht
De onderstaande tabel ziet u elk resourcetype met de mogelijke toewijzingsmethoden (dynamisch/statisch), en de mogelijkheid om meerdere openbare IP-adressen toewijzen.

| Resource | Dynamisch | Statisch | Meerdere IP-adressen |
| --- | --- | --- | --- |
| Cloudservice |Ja |Ja |Ja |
| IaaS-VM of PaaS-rolinstantie |Ja |Nee |Nee |
| VPN-gateway |Ja |Nee |Nee |
| Toepassingsgateway |Ja |Nee |Nee |

## <a name="private-ip-addresses"></a>Privé-IP-adressen
Privé-IP-adressen worden gebruikt om Azure-resources om te communiceren met andere resources in een cloudservice of een [virtueel netwerk](virtual-networks-overview.md)(VNet), of on-premises netwerk (via een VPN-gateway of ExpressRoute-circuit), zonder gebruik van een Via Internet bereikbaar IP-adres.

In de Azure-klassieke implementatiemodel, kunt een privé IP-adres worden toegewezen aan de volgende Azure-resources:

* IaaS-VM's en PaaS-rolexemplaren
* Interne load balancer
* Toepassingsgateway

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS-VM's en PaaS-rolexemplaren
Virtuele machines (VM's) die zijn gemaakt met het klassieke implementatiemodel worden altijd in een cloudservice, die vergelijkbaar is met PaaS-rolexemplaren geplaatst. Het gedrag van privé-IP-adressen is dus vergelijkbaar voor deze resources.

Het is belangrijk te weten dat een service in de cloud kan worden geïmplementeerd op twee manieren:

* Als een *zelfstandige* -service, waar het is niet binnen een virtueel netwerk in de cloud.
* Als onderdeel van een virtueel netwerk.

#### <a name="allocation-method"></a>Toewijzingsmethode
In geval van een *zelfstandige* cloudservice, een privé IP-adres toegewezen resources-get *dynamisch* van Azure-datacenter voor een privé-IP-adresbereik. Het kan alleen worden gebruikt voor communicatie met andere VM's binnen dezelfde cloudservice. Dit IP-adres kunt wijzigen wanneer de resource wordt gestopt en gestart.

Resources ophalen in het geval van een cloudservice die is geïmplementeerd in een virtueel netwerk, persoonlijke IP-adressen uit het adresbereik van de gekoppelde subnetten (zoals aangegeven in de netwerkconfiguratie) toegewezen. Deze persoonlijke IP-adres kan worden gebruikt voor communicatie tussen alle virtuele machines binnen het VNet.

Bovendien een privé IP-adres is toegewezen in het geval van cloudservices binnen een VNet, *dynamisch* (via DHCP) standaard. Deze kunt wijzigen wanneer de resource wordt gestopt en gestart. Om te controleren of het IP-adres blijft hetzelfde, moet u de toewijzingsmethode instelt op *statische*, en geef een geldig IP-adres binnen het bijbehorende adresbereik.

Statische privé-IP-adressen worden vaak gebruikt voor:

* Virtuele machines die fungeren als domeincontrollers of DNS-servers.
* Virtuele machines waarvoor firewallregels met behulp van IP-adressen.
* Virtuele machines met services die toegankelijk zijn via andere apps via een IP-adres.

#### <a name="internal-dns-hostname-resolution"></a>Interne DNS-hostnaamomzetting
Alle Azure-VM's en PaaS-rolinstanties zijn geconfigureerd met [Azure beheerde DNS-servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) standaard, tenzij u expliciet aangepaste DNS-servers configureert. Deze DNS-servers bieden een interne naamomzetting voor virtuele machines en rolinstanties die zich in dezelfde service VNet of in de cloud bevinden.

Wanneer u een virtuele machine maakt, wordt er een toewijzing van de hostnaam aan het bijbehorende privé-IP-adres toegevoegd aan de via Azure beheerde DNS-servers. Met virtuele machine meerdere NIC's, de hostnaam toegewezen aan het privé IP-adres van de primaire NIC Deze toewijzingsinformatie is echter beperkt tot de resources binnen de dezelfde cloudservice of de VNet.

In geval van een *zelfstandige* cloudservice, kunt u zich kunnen omzetten van hostnamen van alle VM's / rolexemplaren binnen dezelfde cloudservice alleen. In het geval van een cloudservice die binnen een VNet zich kunt u kunnen omzetten van hostnamen van alle exemplaren van virtuele machines/rol binnen het VNet.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interne load balancers (ILB) en toepassingsgateways
U kunt een privé-IP-adres toewijzen aan de **front-end**-configuratie van een [interne Azure Load Balancer](../load-balancer/load-balancer-internal-overview.md) (ILB) of een [Azure Application Gateway](../application-gateway/application-gateway-introduction.md). Dit privé-IP-adres fungeert als een intern eindpunt dat alleen toegankelijk is voor de resources binnen het virtuele netwerk (VNet) en de externe netwerken die met het VNet zijn verbonden. U kunt een dynamisch of statisch privé-IP-adres toewijzen aan de front-end-configuratie. U kunt ook meerdere privé IP-adressen inschakelen van multi-vip-scenario's toewijzen.

### <a name="at-a-glance"></a>Een overzicht
De onderstaande tabel ziet u elk resourcetype met de mogelijke toewijzingsmethoden (dynamisch/statisch), en de mogelijkheid om meerdere privé IP-adressen toewijzen.

| Resource | Dynamisch | Statisch | Meerdere IP-adressen |
| --- | --- | --- | --- |
| Virtuele machine (in een *zelfstandige* cloudservice of VNet) |Ja |Ja |Ja |
| PaaS-rolinstantie (in een *zelfstandige* cloudservice of VNet) |Ja |Nee |Nee |
| Interne load balancer-front-end |Ja |Ja |Ja |
| Application gateway-front-end |Ja |Ja |Ja |

## <a name="limits"></a>Limieten
De onderstaande tabel bevat de limieten die zijn opgelegd voor IP-adressen in Azure per abonnement. U kunt [contact opnemen met ondersteuning](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als u op basis van uw bedrijfsbehoeften de standaardlimieten wilt verhogen tot de maximumlimieten.

|  | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| Openbare IP-adressen (dynamisch) |5 |contact met ondersteuning |
| Gereserveerde openbare IP-adressen |20 |contact met ondersteuning |
| Openbare VIP per implementatie (cloudservice) |5 |contact met ondersteuning |
| Persoonlijke VIP (ILB) per implementatie (cloudservice) |1 |1 |

Zorg ervoor dat u de volledige set lezen [limieten voor netwerken](../azure-subscription-service-limits.md#networking-limits) in Azure.

## <a name="pricing"></a>Prijzen
In de meeste gevallen zijn openbare IP-adressen zijn gratis. Er is een nominale kosten extra en/of statische openbare IP-adressen gebruiken. Zorg ervoor dat de [prijsstructuur voor openbare IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="differences-between-resource-manager-and-classic-deployments"></a>Verschillen tussen Resource Manager en klassieke implementaties
Hieronder volgt een vergelijking van de IP-adressering functies in Resource Manager en het klassieke implementatiemodel.

|  | Resource | Klassiek | Resource Manager |
| --- | --- | --- | --- |
| **Openbare IP-adres** |***VM*** |Aangeduid als een ILPIP (alleen dynamisch) |Aangeduid als een openbaar IP-adres (dynamisch of statisch) |
|  ||Toegewezen aan een IaaS-VM of een PaaS-rolinstantie |Dat is gekoppeld aan van de VM NIC |
|  |***Internetgerichte load balancer*** |Wel VIP (dynamische) of gereserveerd IP-adres (statisch) |Aangeduid als een openbaar IP-adres (dynamisch of statisch) |
|  ||Toegewezen aan een cloudservice |Dat is gekoppeld aan de load balancer-front-end-configuratie |
|  | | | |
| **Privé-IP-adres** |***VM*** |Aangeduid als een DIP |Aangeduid als een privé IP-adres |
|  ||Toegewezen aan een IaaS-VM of een PaaS-rolinstantie |Toegewezen aan van de VM NIC |
|  |***Interne load balancer (ILB)*** |Toegewezen aan de ILB (dynamisch of statisch) |Toegewezen aan van de ILB-front-end-configuratie (dynamisch of statisch) |

## <a name="next-steps"></a>Volgende stappen
* [Een VM implementeren met een statisch privé IP-adres](virtual-networks-static-private-ip-classic-pportal.md) met behulp van de Azure portal.

