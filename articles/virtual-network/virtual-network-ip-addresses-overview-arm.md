---
title: IP-adrestypen in Azure | Microsoft Docs
description: "Meer informatie over openbare en privé-IP-adressen in Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-resource-manager
ms.assetid: 610b911c-f358-4cfe-ad82-8b61b87c3b7e
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 144f4ea213b8ed0a3530495e185f489155c474c9
ms.contentlocale: nl-nl
ms.lasthandoff: 08/21/2017

---
# <a name="ip-address-types-and-allocation-methods-in-azure"></a>IP-adrestypen en toewijzingsmethoden in Azure
U kunt IP-adressen toewijzen aan Azure-resources om te communiceren met andere Azure-resources, uw on-premises netwerk en internet. Er zijn twee typen IP-adressen die u in Azure kunt gebruiken:

* **Openbare IP-adressen**: deze worden gebruikt voor communicatie met internet, met inbegrip van openbare Azure-services
* **Privé-IP-adressen**: deze worden gebruikt voor communicatie in een virtueel Azure-netwerk (VNet) en uw on-premises netwerk wanneer u een VPN-gateway of ExpressRoute-circuit gebruikt om uw netwerk uit te breiden naar Azure.

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../resource-manager-deployment-model.md).  Dit artikel bevat informatie over het Resource Manager-implementatiemodel, dat door Microsoft wordt aanbevolen voor de meeste nieuwe implementaties in plaats van het [klassieke implementatiemodel](virtual-network-ip-addresses-overview-classic.md).
> 

Als u het klassieke implementatiemodel kent, kunt u [hier lezen wat de verschillen zijn in IP-adressering tussen het klassieke model en het Resource Manager-model](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Openbare IP-adressen
Openbare IP-adressen worden gebruikt om Azure-resources te laten communiceren met internet en andere openbare Azure-services, zoals [Azure Redis Cache](https://azure.microsoft.com/services/cache/), [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL databases](../sql-database/sql-database-technical-overview.md) en [Azure Storage](../storage/common/storage-introduction.md).

In Azure Resource Manager is een [openbaar IP](resource-groups-networking.md#public-ip-address)-adres een resource die zijn eigen eigenschappen heeft. U kunt een openbare IP-adresresource koppelen aan elk van de volgende resources:

* Virtuele machines (VM's)
* Internetgerichte load balancers
* VPN-gateways
* Toepassingsgateways

### <a name="allocation-method"></a>Toewijzingsmethode
Er zijn twee methoden voor het toewijzen van een IP-adres aan een *openbare* IP-resource: *dynamisch* en *statisch*. De standaardtoewijzingsmethode is *dynamisch*, waarbij het IP-adres **niet** wordt toegewezen op het tijdstip van aanmaak. In plaats daarvan wordt het openbare IP-adres pas toegewezen wanneer u de gekoppelde resource (zoals een virtuele machine of load balancer) start (of maakt). Het IP-adres wordt weer vrijgegeven wanneer u de resource stopt (of verwijdert). Hierdoor verandert het IP-adres wanneer u een resource stopt en start.

Als u wilt dat het IP-adres voor de gekoppelde resource hetzelfde blijft, kunt u de toewijzingsmethode expliciet instellen op *statisch*. In dat geval wordt er onmiddellijk een IP-adres toegewezen. Het IP-adres wordt alleen vrijgegeven wanneer u de resource verwijdert of de toewijzingsmethode wijzigt in *dynamisch*.

> [!NOTE]
> Ook als u de toewijzingsmethode instelt op *statisch*, kunt u het IP-adres dat aan de *openbare IP-resource* wordt toegewezen, echter niet zelf opgeven. In plaats daarvan wordt het toegewezen vanuit een pool van beschikbare IP-adressen op de Azure-locatie waarin de resource is gemaakt.
>

Statische openbare IP-adressen worden vaak gebruikt in de volgende scenario's:

* Eindgebruikers moeten firewallregels bijwerken om te communiceren met uw Azure-resources.
* U gebruikt een DNS-naamomzetting waarbij een wijziging in het IP-adres het bijwerken van A-records vereist.
* Uw Azure-resources communiceren met andere apps of services die een op IP-adressen gebaseerd beveiligingsmodel gebruiken.
* U gebruikt SSL-certificaten die zijn gekoppeld aan een IP-adres.

> [!NOTE]
> De lijst met IP-adresbereiken waaruit openbare IP-adressen (dynamisch/statisch) worden toegewezen aan Azure-resources, wordt gepubliceerd op [IP-adresbereiken Azure Datacenter](https://www.microsoft.com/download/details.aspx?id=41653).
>

### <a name="dns-hostname-resolution"></a>DNS-hostnaamomzetting
U kunt voor een openbare IP-resource een DNS-domeinnaamlabel opgeven, zodat *domeinnaamlabel*.*locatie*. cloudapp.azure.com verwijst naar het openbare IP-adres op de door Azure beheerde DNS-servers. Als u bijvoorbeeld een openbare IP-resource maakt met **contoso** als *domeinnaamlabel* op de Azure-*locatie* **VS - west**, wordt de FQDN-naam (Fully Qualified Domain Name) **contoso.westus.cloudapp.azure.com** omgezet in het openbare IP-adres van de resource. U kunt deze FDQN gebruiken voor het maken van een aangepaste domein-CNAME-record die verwijst naar het openbare IP-adres in Azure.

> [!IMPORTANT]
> Elk domeinnaamlabel dat wordt gemaakt, moet uniek zijn binnen de Azure-locatie.  
>

### <a name="virtual-machines"></a>Virtuele machines
U kunt een openbaar IP-adres koppelen aan een virtuele [Windows](../virtual-machines/windows/overview.md)- of [Linux](../virtual-machines/virtual-machines-linux-about.md)-machine door het toe te wijzen aan de **netwerkinterface**. In het geval van een virtuele machine met meerdere netwerkinterfaces kunt u het adres alleen toewijzen aan de *primaire* netwerkinterface. U kunt een dynamisch of statisch openbaar IP-adres toewijzen aan een virtuele machine.

### <a name="internet-facing-load-balancers"></a>Internetgerichte load balancers
U kunt een openbaar IP-adres koppelen aan een [Azure Load Balancer](../load-balancer/load-balancer-overview.md) door het toe te wijzen aan de **frontend**-configuratie van de load balancer. Dit openbare IP-adres doet dienst als een virtueel IP-adres (VIP) met taakverdeling. U kunt een dynamisch of statisch openbaar IP-adres toewijzen aan de front-end van een load balancer. U kunt ook meerdere openbare IP-adressen toewijzen aan de front-end van een load balancer, zodat [multi-VIP](../load-balancer/load-balancer-multivip.md)-scenario's, zoals een multitenant-omgeving met op SSL gebaseerde websites, mogelijk zijn.

### <a name="vpn-gateways"></a>VPN-gateways
[Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) wordt gebruikt voor het koppelen van een virtueel Azure-netwerk (VNet) aan andere Azure-VNets of aan een on-premises netwerk. U moet een openbaar IP-adres toewijzen aan de **IP-configuratie** om communicatie met het externe netwerk mogelijk te maken. Op dit moment kunt u alleen een *dynamisch* openbaar IP-adres toewijzen aan een VPN-gateway.

### <a name="application-gateways"></a>Toepassingsgateways
U kunt een openbaar IP-adres koppelen aan een Azure-[toepassingsgateway](../application-gateway/application-gateway-introduction.md) door het toe te wijzen aan de **front-end**-configuratie van de gateway. Dit openbare IP-adres doet dienst als een VIP met taakverdeling. Op dit moment kunt u alleen een *dynamisch* openbaar IP-adres toewijzen aan de front-end-configuratie van een toepassingsgateway.

### <a name="at-a-glance"></a>In een oogopslag
De volgende tabel toont de specifieke eigenschap waarmee een openbaar IP-adres kan worden gekoppeld aan een resource op het hoogste niveau, evenals de mogelijke toewijzingsmethoden (dynamisch of statisch) die kunnen worden gebruikt.

| Resource op het hoogste niveau | IP-adreskoppeling | Dynamisch | Statisch |
| --- | --- | --- | --- |
| Virtuele machine |Netwerkinterface |Ja |Ja |
| Load balancer |Front-end-configuratie |Ja |Ja |
| VPN-gateway |Gateway-IP-configuratie |Ja |Nee |
| Toepassingsgateway |Front-end-configuratie |Ja |Nee |

## <a name="private-ip-addresses"></a>Privé-IP-adressen
Privé-IP-adressen stellen Azure-resources in staat om via een VPN-gateway of een ExpressRoute-circuit te communiceren met andere resources in een [virtueel netwerk](virtual-networks-overview.md) of een on-premises netwerk, zonder gebruik te maken van een via internet bereikbaar IP-adres.

In het Azure Resource Manager-implementatiemodel is een privé-IP-adres gekoppeld aan de volgende typen Azure-resources:

* VM's
* Interne load balancers (ILB's)
* Toepassingsgateways

### <a name="allocation-method"></a>Toewijzingsmethode
Een privé-IP-adres wordt toegewezen vanuit het adresbereik van het subnet waaraan de resource is gekoppeld. Het adresbereik van het subnet zelf is een onderdeel van het VNet-adresbereik.

Er zijn twee methoden voor het toewijzen van een privé-IP-adres: *dynamisch* en *statisch*. De standaardtoewijzingsmethode is *dynamisch*, waarbij het IP-adres automatisch wordt toegewezen vanuit het subnet van de resource (via DHCP). Dit IP-adres kan veranderen wanneer u de resource stopt en start.

Stel de toewijzingsmethode in op *statisch* als u wilt dat het IP-adres altijd hetzelfde blijft. In dat geval moet u ook een geldig IP-adres opgeven dat deel uitmaakt van het subnet van de resource.

Statische privé-IP-adressen worden vaak gebruikt voor:

* Virtuele machines die fungeren als domeincontrollers of DNS-servers.
* Resources die firewallregels gebruiken die zijn gebaseerd op IP-adressen.
* Resources die worden gebruikt door andere apps/resources via een IP-adres.

### <a name="virtual-machines"></a>Virtuele machines
Een privé-IP-adres wordt toegewezen aan de **netwerkinterface** van een virtuele [Windows](../virtual-machines/windows/overview.md)- of [Linux](../virtual-machines/virtual-machines-linux-about.md)-machine. In geval van een VM met een multi-netwerkinterface, wordt aan elke interface een privé-IP-adres toegewezen. Voor een netwerkinterface kunt u de toewijzingsmethode opgeven als dynamisch of statisch.

#### <a name="internal-dns-hostname-resolution-for-vms"></a>Interne DNS-hostnaamomzetting (voor VM's)
Alle Azure-VM's zijn standaard geconfigureerd met door [Azure beheerde DNS-servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution), tenzij u expliciet aangepaste DNS-servers configureert. Deze DNS-servers bieden een interne naamomzetting voor virtuele machines die zich in hetzelfde VNet bevinden.

Wanneer u een virtuele machine maakt, wordt er een toewijzing van de hostnaam aan het bijbehorende privé-IP-adres toegevoegd aan de via Azure beheerde DNS-servers. In geval van een VM met een multi-netwerkinterface wordt de hostnaam toegewezen aan het privé-IP-adres van de primaire netwerkinterface.

Virtuele machines die zijn geconfigureerd met via Azure beheerde DNS-servers, kunnen de hostnamen van alle virtuele machines binnen het betreffende VNet omzetten in hun privé-IP-adressen.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interne load balancers (ILB) en toepassingsgateways
U kunt een privé-IP-adres toewijzen aan de **front-end**-configuratie van een [interne Azure Load Balancer](../load-balancer/load-balancer-internal-overview.md) (ILB) of een [Azure Application Gateway](../application-gateway/application-gateway-introduction.md). Dit privé-IP-adres fungeert als een intern eindpunt dat alleen toegankelijk is voor de resources binnen het virtuele netwerk (VNet) en de externe netwerken die met het VNet zijn verbonden. U kunt een dynamisch of statisch privé-IP-adres toewijzen aan de front-end-configuratie.

### <a name="at-a-glance"></a>In een oogopslag
De volgende tabel toont de specifieke eigenschap waarmee een privé-IP-adres kan worden gekoppeld aan een resource op het hoogste niveau, evenals de mogelijke toewijzingsmethoden (dynamisch of statisch) die kunnen worden gebruikt.

| Resource op het hoogste niveau | IP-adreskoppeling | Dynamisch | Statisch |
| --- | --- | --- | --- |
| Virtuele machine |Netwerkinterface |Ja |Ja |
| Load balancer |Front-end-configuratie |Ja |Ja |
| Toepassingsgateway |Front-end-configuratie |Ja |Ja |

## <a name="limits"></a>Limieten
De limieten die zijn opgelegd voor IP-adressen, vindt u in de volledige set [limieten voor netwerken](../azure-subscription-service-limits.md#networking-limits) in Azure. Deze limieten gelden per regio en per abonnement. U kunt [contact opnemen met ondersteuning](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als u op basis van uw bedrijfsbehoeften de standaardlimieten wilt verhogen tot de maximumlimieten.

## <a name="pricing"></a>Prijzen
Openbare IP-adressen kunnen een kostprijs hebben. Voor meer informatie over prijzen voor IP-adressen in Azure raadpleegt u de pagina [Prijzen van IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses).

## <a name="next-steps"></a>Volgende stappen
* [Een virtuele machine met een statisch openbaar IP-adres implementeren via Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
* [Een virtuele machine met een statisch openbaar IP-adres implementeren met een sjabloon](virtual-network-deploy-static-pip-arm-template.md)
* [Een virtuele machine met een statisch privé-IP-adres implementeren via Azure Portal](virtual-networks-static-private-ip-arm-pportal.md)
