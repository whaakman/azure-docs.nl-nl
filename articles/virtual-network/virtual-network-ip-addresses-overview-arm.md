---
title: IP-adrestypen in Azure | Microsoft Docs
description: "Meer informatie over openbare en privé-IP-adressen in Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 610b911c-f358-4cfe-ad82-8b61b87c3b7e
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/16/2017
ms.author: jdial
ms.openlocfilehash: e3baedba814cf6ac19df72c49e1c95ea2cd5cf73
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="ip-address-types-and-allocation-methods-in-azure"></a>IP-adrestypen en toewijzingsmethoden in Azure

U kunt IP-adressen toewijzen aan Azure-resources om te communiceren met andere Azure-resources, uw on-premises netwerk en internet. Er zijn twee typen IP-adressen die u in Azure kunt gebruiken:

* **Openbare IP-adressen**: deze worden gebruikt voor communicatie met internet, met inbegrip van openbare Azure-services.
* **Privé-IP-adressen**: deze worden gebruikt voor communicatie in een virtueel Azure-netwerk (VNet) en uw on-premises netwerk wanneer u een VPN-gateway of ExpressRoute-circuit gebruikt om uw netwerk uit te breiden naar Azure.

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  Dit artikel bevat informatie over het Resource Manager-implementatiemodel, dat door Microsoft wordt aanbevolen voor de meeste nieuwe implementaties in plaats van het [klassieke implementatiemodel](virtual-network-ip-addresses-overview-classic.md).
> 

Als u het klassieke implementatiemodel kent, kunt u [hier lezen wat de verschillen zijn in IP-adressering tussen het klassieke model en het Resource Manager-model](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Openbare IP-adressen

Door openbare IP-adressen te gebruiken, zijn internetbronnen in staat inkomende communicatie voor Azure-resources te verwerken. Openbare IP-adressen maken het ook mogelijk dat Azure-resources uitgaande communicatie naar internet en openbare Azure-services kunnen afhandelen via een IP-adres dat is toegewezen aan de resource. Het adres blijft toegewezen aan de resource totdat u deze toewijzing ongedaan maakt. Als een openbaar IP-adres niet aan een resource is toegewezen, kan de resource nog steeds de uitgaande communicatie met internet afhandelen, maar wijst Azure dynamisch een beschikbaar IP-adres toe dat niet aan de resource is toegewezen. Zie [Uitleg over uitgaande verbindingen](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie over uitgaande verbindingen in Azure.

In Azure Resource Manager is een [openbaar IP](virtual-network-public-ip-address.md)-adres een resource die zijn eigen eigenschappen heeft. U kunt sommige resources aan een openbare IP-adresresource koppelen, zoals:

* Netwerkinterfaces van virtuele machines
* Internetgerichte load balancers
* VPN-gateways
* Toepassingsgateways

### <a name="ip-address-version"></a>IP-adresversie

Openbare IP-adressen worden gemaakt met een IPv4- of IPv6-adres. Openbare IPv6-adressen kunnen alleen worden toegewezen aan internetgerichte load balancers.

### <a name="sku"></a>SKU

Openbare IP-adressen worden gemaakt met een van de volgende SKU's:

#### <a name="basic"></a>Basic

Alle openbare IP-adressen die zijn gemaakt vóór de introductie van SKU's zijn openbare IP-adressen van de basis-SKU. Door de introductie van SKU's kunt u opgeven welke SKU het openbare IP-adres is. Basis-SKU-adressen:

- Worden toegewezen met de statische of dynamische toewijzingsmethode.
- Worden toegewezen aan een Azure-resource waaraan een openbaar IP-adres kan worden toegewezen, zoals netwerkinterfaces, VPN-gateways, toepassingsgateways en internetgerichte load balancers.
- Kunnen worden toegewezen aan een specifieke zone.
- Zijn niet zone-redundant. Zie [Overzicht van beschikbaarheidszones](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie over beschikbaarheidszones.

#### <a name="standard"></a>Standard

Openbare IP-adressen van de standaard-SKU:

- Worden uitsluitend toegewezen met de statische toewijzingsmethode.
- Worden toegewezen aan netwerkinterfaces of standaard internetgerichte load balancers. Zie [Standaard-SKU's van Azure Load Balancer](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie over SKU's van Azure Load Balancer.
- Zijn standaard zone-redundant. Kunnen zonegebonden worden gemaakt en gegarandeerd in een bepaalde beschikbaarheidszone.  Zie [Overzicht van beschikbaarheidszones](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie over beschikbaarheidszones.
 
> [!NOTE]
> Als u een openbaar IP-adres van een standaard-SKU toewijst aan een netwerkinterface van een virtuele machine, moet u het bedoelde verkeer expliciet toestaan met een [netwerkbeveiligingsgroep](security-overview.md#network-security-groups).  Communicatie met de resource mislukt totdat u een netwerkbeveiligingsgroep maakt en koppelt en het gewenste verkeer expliciet toestaat.

De standaard-SKU zit in de evaluatieversie. Voordat u een openbaar IP-adres van een standaard-SKU maakt, moet u zich eerst registreren voor de evaluatieversie en het adres in een ondersteunde locatie maken. Zie [Registreren voor de evaluatieversie van de standaard-SKU](virtual-network-public-ip-address.md#register-for-the-standard-sku-preview) om u te registreren voor de evaluatieversie. Zie [Beschikbaarheid in regio's](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region-availability) voor een lijst met ondersteunde locaties (regio's) en controleer de pagina [Azure Virtual Network-updates](https://azure.microsoft.com/updates/?product=virtual-network) voor aanvullende ondersteuning voor regio's.


### <a name="allocation-method"></a>Toewijzingsmethode

Er zijn twee methoden voor het toewijzen van een IP-adres aan een openbare IP-adresresource: *dynamisch* en *statisch*. De standaardtoewijzingsmethode is *dynamisch*, waarbij het IP-adres **niet** wordt toegewezen op het tijdstip van aanmaak. In plaats daarvan wordt het openbare IP-adres pas toegewezen wanneer u de gekoppelde resource (zoals een virtuele machine of load balancer) start (of maakt). Het IP-adres wordt weer vrijgegeven wanneer u de resource stopt (of verwijdert). Nadat het IP-adres vanaf resource A is vrijgegeven, kan het worden toegewezen aan een andere resource. Als het IP-adres wordt toegewezen aan een andere resource terwijl resource A is gestopt, wordt een ander IP-adres toegewezen als u resource A opnieuw start.

Als u wilt dat het IP-adres voor de gekoppelde resource hetzelfde blijft, kunt u de toewijzingsmethode expliciet instellen op *statisch*. Een statisch IP-adres wordt onmiddellijk toegewezen. Het adres wordt alleen vrijgegeven wanneer u de resource verwijdert of de toewijzingsmethode wijzigt in *dynamisch*.

> [!NOTE]
> Ook als u de toewijzingsmethode instelt op *statisch*, kunt u het IP-adres dat aan de openbare IP-adresresource wordt toegewezen, echter niet zelf opgeven. Azure wijst het IP-adres toe vanuit een pool van beschikbare IP-adressen op de Azure-locatie waarin de resource is gemaakt.
>

Statische openbare IP-adressen worden vaak gebruikt in de volgende scenario's:

* U moet firewallregels bijwerken om te communiceren met uw Azure-resources.
* U gebruikt een DNS-naamomzetting waarbij een wijziging in het IP-adres het bijwerken van A-records vereist.
* Uw Azure-resources communiceren met andere apps of services die een op IP-adressen gebaseerd beveiligingsmodel gebruiken.
* U gebruikt SSL-certificaten die zijn gekoppeld aan een IP-adres.

> [!NOTE]
> Azure wijst openbare IP-adressen toe uit een bereik dat uniek is voor elke Azure-regio. Zie [IP-adresbereiken Azure Datacenter](https://www.microsoft.com/download/details.aspx?id=41653) voor meer informatie.
>

### <a name="dns-hostname-resolution"></a>DNS-hostnaamomzetting
U kunt voor een openbare IP-resource een DNS-domeinnaamlabel opgeven, zodat *domeinnaamlabel*.*locatie*. cloudapp.azure.com verwijst naar het openbare IP-adres op de door Azure beheerde DNS-servers. Als u bijvoorbeeld een openbare IP-resource maakt met **contoso** als *domeinnaamlabel* op de Azure*-locatie* **VS - west**, wordt de FQDN-naam (Fully Qualified Domain Name) **contoso.westus.cloudapp.azure.com** omgezet in het openbare IP-adres van de resource. U kunt de FDQN gebruiken voor het maken van een aangepaste domein-CNAME-record die verwijst naar het openbare IP-adres in Azure. In plaats van of naast het gebruik van het DNS-naamlabel met het standaardachtervoegsel, kunt u de Azure DNS-service gebruiken om een DNS-naam met een aangepast achtervoegsel te configureren dat wordt omgezet naar het openbare IP-adres. Zie [Azure DNS gebruiken met een openbaar IP-adres van Azure](../dns/dns-custom-domain.md?toc=%2fazure%2fvirtual-network%2ftoc.json#public-ip-address) voor meer informatie.

> [!IMPORTANT]
> Elk domeinnaamlabel dat wordt gemaakt, moet uniek zijn binnen de Azure-locatie.  
>

### <a name="virtual-machines"></a>Virtuele machines

U kunt een openbaar IP-adres koppelen aan een virtuele [Windows](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)- of [Linux](../virtual-machines/linux/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-machine door het toe te wijzen aan de **netwerkinterface**. U kunt een dynamisch of statisch openbaar IP-adres toewijzen aan een virtuele machine. Meer informatie over [IP-adressen toewijzen aan netwerkinterfaces](virtual-network-network-interface-addresses.md).

### <a name="internet-facing-load-balancers"></a>Internetgerichte load balancers

U kunt een openbaar IP-adres dat met een willekeurige [SKU](#SKU) is gemaakt koppelen aan een [Azure Load Balancer](../load-balancer/load-balancer-overview.md) door het toe te wijzen aan de **front-end**-configuratie van de load balancer. Het openbare IP-adres doet dienst als een virtueel IP-adres (VIP) met taakverdeling. U kunt een dynamisch of statisch openbaar IP-adres toewijzen aan de front-end van een load balancer. U kunt ook meerdere openbare IP-adressen toewijzen aan de front-end van een load balancer, zodat [multi-VIP](../load-balancer/load-balancer-multivip-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-scenario's, zoals een multitenant-omgeving met op SSL gebaseerde websites, mogelijk zijn. Zie [Standaard-SKU's van Azure Load Balancer](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie over SKU's van Azure Load Balancer.

### <a name="vpn-gateways"></a>VPN-gateways

Een [Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) koppelt een virtueel Azure-netwerk aan andere virtuele Azure-netwerken of aan een on-premises netwerk. Een openbaar IP-adres wordt toegewezen aan de VPN-gateway om communicatie met het externe netwerk mogelijk te maken. U kunt alleen een *dynamisch* openbaar IP-adres toewijzen aan een VPN-gateway.

### <a name="application-gateways"></a>Toepassingsgateways

U kunt een openbaar IP-adres koppelen aan een Azure-[toepassingsgateway](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) door het toe te wijzen aan de **front-end**-configuratie van de gateway. Dit openbare IP-adres doet dienst als een VIP met taakverdeling. U kunt alleen een *dynamisch* openbaar IP-adres toewijzen aan de front-end-configuratie van een toepassingsgateway.

### <a name="at-a-glance"></a>In een oogopslag
De volgende tabel toont de specifieke eigenschap waarmee een openbaar IP-adres kan worden gekoppeld aan een resource op het hoogste niveau, evenals de mogelijke toewijzingsmethoden (dynamisch of statisch) die kunnen worden gebruikt.

| Resource op het hoogste niveau | IP-adreskoppeling | Dynamisch | Statisch |
| --- | --- | --- | --- |
| Virtuele machine |Netwerkinterface |Ja |Ja |
| Internetgerichte load balancer |Front-end-configuratie |Ja |Ja |
| VPN-gateway |Gateway-IP-configuratie |Ja |Nee |
| Toepassingsgateway |Front-end-configuratie |Ja |Nee |

## <a name="private-ip-addresses"></a>Privé-IP-adressen
Privé-IP-adressen stellen Azure-resources in staat om via een VPN-gateway of een ExpressRoute-circuit te communiceren met andere resources in een [virtueel netwerk](virtual-networks-overview.md) of een on-premises netwerk, zonder gebruik te maken van een via internet bereikbaar IP-adres.

In het Azure Resource Manager-implementatiemodel is een privé-IP-adres gekoppeld aan de volgende typen Azure-resources:

* Netwerkinterfaces van virtuele machines
* Interne load balancers (ILB's)
* Toepassingsgateways

### <a name="ip-address-version"></a>IP-adresversie

Privé-IP-adressen worden gemaakt met een IPv4- of IPv6-adres. Privé-IPv6-adressen kunnen alleen worden toegewezen met de dynamische toewijzingsmethode. U kunt niet communiceren tussen privé-IPv6-adressen op een virtueel netwerk. U kunt inkomend communiceren met een privé-IPv6-adres van het internet via een internetgerichte load balancer. Zie [Een internetgerichte load balancer maken met IPv6](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie.

### <a name="allocation-method"></a>Toewijzingsmethode

Een privé-IP-adres wordt in toegewezen in het adresbereik van het subnet van het virtuele netwerk waarin een resource wordt geïmplementeerd. Azure reserveert de eerste vier adressen in het adresbereik van elk subnet, zodat die adressen niet aan resources kunnen worden toegewezen. Als het adresbereik van het subnet bijvoorbeeld 10.0.0.0/16 is, kunnen de adressen van 10.0.0.0 tot en met-10.0.0.3 niet aan resources worden toegewezen. IP-adressen binnen het adresbereik van het subnet kunnen slechts aan één resource tegelijkertijd worden toegewezen. 

Er zijn twee methoden voor het toewijzen van een privé-IP-adres:

- **Dynamisch**: Azure wijst het volgende beschikbare niet-toegewezen of niet-gereserveerde IP-adres in het adresbereik van het subnet toe. Azure wijst 10.0.0.10 bijvoorbeeld toe aan een nieuwe resource als de adressen van 10.0.0.4 tot en met 10.0.0.9 al aan andere bronnen zijn toegewezen. Dynamisch is de standaardmethode voor toewijzing. Nadat dynamische IP-adressen zijn toegewezen, worden ze alleen vrijgegeven als een netwerkinterface wordt verwijderd of wordt toegewezen aan een ander subnet binnen hetzelfde virtuele netwerk of als de toewijzingsmethode wordt gewijzigd in statisch en een ander IP-adres wordt opgegeven. Standaard wijst Azure het vorige dynamisch toegewezen adres toe als het statische adres wanneer u de toewijzingsmethode van dynamisch wijzigt in statisch.
- **Statisch**: u selecteert elk beschikbaar niet-toegewezen of niet-gereserveerde IP-adres in het adresbereik van het subnet en wijst dit toe. Als het adresbereik van een subnet bijvoorbeeld 10.0.0.0/16 is en de adressen van 10.0.0.4 tot en met 10.0.0.9 al zijn toegewezen aan andere resources, kunt u elk adres tussen 10.0.0.10 en 10.0.255.254 toewijzen. Statische adressen worden alleen vrijgegeven als een netwerkinterface wordt verwijderd. Als u de toewijzingsmethode wijzigt in dynamisch, wijst Azure het eerder toegewezen statische IP-adres toe als dynamisch adres, zelfs als dat adres niet het eerstvolgende beschikbare adres in het adresbereik van het subnet is. Het adres verandert ook als de netwerkinterface wordt toegewezen aan een ander subnet binnen hetzelfde virtuele netwerk, maar als u de netwerkinterface wilt toewijzen aan een ander subnet, moet u eerst de toewijzingsmethode wijzigen van statisch in dynamisch. Nadat u de netwerkinterface hebt toegewezen aan een ander subnet, kunt u de toewijzingsmethode weer wijzigen in statisch en een IP-adres uit het adresbereik van het nieuwe subnet toewijzen.

### <a name="virtual-machines"></a>Virtuele machines

Een of meer privé-IP-adressen worden toegewezen aan een of meer **netwerkinterfaces** van een virtuele [Windows](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)- of [Linux](../virtual-machines/linux/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-machine. Voor elk privé-IP-adres kunt u de toewijzingsmethode opgeven als dynamisch of statisch.

#### <a name="internal-dns-hostname-resolution-for-virtual-machines"></a>Interne DNS-hostnaamomzetting (voor virtuele machines)

Alle virtuele machines van Azure zijn standaard geconfigureerd met door [Azure beheerde DNS-servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution), tenzij u expliciet aangepaste DNS-servers configureert. Deze DNS-servers bieden een interne naamomzetting voor virtuele machines die zich in hetzelfde virtuele netwerk bevinden.

Wanneer u een virtuele machine maakt, wordt er een toewijzing van de hostnaam aan het bijbehorende privé-IP-adres toegevoegd aan de via Azure beheerde DNS-servers. Als een virtuele machine meerdere netwerkinterfaces of meerdere IP-configuraties voor een netwerkinterface heeft, wordt de hostnaam toegewezen aan het privé-IP-adres van de primaire IP-configuratie van de primaire netwerkinterface.

Virtuele machines die zijn geconfigureerd met via Azure beheerde DNS-servers, kunnen de hostnamen van alle virtuele machines binnen hetzelfde virtuele netwerk omzetten in hun privé-IP-adressen. Voor het omzetten van hostnamen van virtuele machines in de verbonden virtuele netwerken moet u een aangepaste DNS-server gebruiken.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interne load balancers (ILB) en toepassingsgateways

U kunt een privé-IP-adres toewijzen aan de **front-end**-configuratie van een [interne Azure Load Balancer](../load-balancer/load-balancer-internal-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (ILB) of een [Azure Application Gateway](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dit privé-IP-adres fungeert als een intern eindpunt dat alleen toegankelijk is voor de resources binnen het virtuele netwerk en de externe netwerken die met het virtuele netwerk zijn verbonden. U kunt een dynamisch of statisch privé-IP-adres toewijzen aan de front-end-configuratie.

### <a name="at-a-glance"></a>In een oogopslag
De volgende tabel toont de specifieke eigenschap waarmee een privé-IP-adres kan worden gekoppeld aan een resource op het hoogste niveau, evenals de mogelijke toewijzingsmethoden (dynamisch of statisch) die kunnen worden gebruikt.

| Resource op het hoogste niveau | IP-adreskoppeling | Dynamisch | Statisch |
| --- | --- | --- | --- |
| Virtuele machine |Netwerkinterface |Ja |Ja |
| Load balancer |Front-end-configuratie |Ja |Ja |
| Toepassingsgateway |Front-end-configuratie |Ja |Ja |

## <a name="limits"></a>Limieten
De limieten die zijn opgelegd voor IP-adressen, vindt u in de volledige set [limieten voor netwerken](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) in Azure. De limieten gelden per regio en per abonnement. U kunt [contact opnemen met ondersteuning](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als u op basis van uw bedrijfsbehoeften de standaardlimieten wilt verhogen tot de maximumlimieten.

## <a name="pricing"></a>Prijzen
Openbare IP-adressen kunnen een kostprijs hebben. Voor meer informatie over prijzen voor IP-adressen in Azure raadpleegt u de pagina [Prijzen van IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses).

## <a name="next-steps"></a>Volgende stappen
* [Een virtuele machine met een statisch openbaar IP-adres implementeren via Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
* [Een virtuele machine met een statisch openbaar IP-adres implementeren met een sjabloon](virtual-network-deploy-static-pip-arm-template.md)
* [Een virtuele machine met een statisch privé-IP-adres implementeren via Azure Portal](virtual-networks-static-private-ip-arm-pportal.md)
