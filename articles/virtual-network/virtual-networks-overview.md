---
title: Azure Virtual Network | Microsoft Docs
description: Meer informatie over Azure Virtual Network-concepten en functies.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/23/2017
ms.author: jdial
ms.openlocfilehash: 6d6afd2b9b956138ed400fbd6cabd3b480fde0f0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-virtual-network"></a>Azure Virtual Network

De service Azure Virtual Network kunt u veilig Azure-resources met elkaar te verbinden met virtuele netwerken (vnet's). Een VNet is een weergave van uw eigen netwerk in de cloud. Een VNet is een logische isolatie van de Azure-cloud toegewezen aan uw abonnement. U kunt de VNets ook verbinding maken met uw on-premises netwerk. De volgende afbeelding ziet u de mogelijkheden van de service Azure Virtual Network:

![Netwerkdiagram](./media/virtual-networks-overview/virtual-network-overview.png)

Klik op de mogelijkheid voor meer informatie over de volgende mogelijkheden van Azure Virtual Network:
- **[Isolatie:](#isolation)**  VNets die zijn geïsoleerd van elkaar. U kunt afzonderlijke VNets voor ontwikkeling, testen en productie die gebruikmaken van dezelfde CIDR-adresblokken maken. Als u daarentegen, kunt u meerdere VNets die gebruikmaken van verschillende CIDR-adresblokken en netwerken met elkaar verbinden. U kunt een VNet segmenteren in meerdere subnetten. Azure biedt interne naamomzetting voor VM's en Cloudservices rolinstanties zijn verbonden met een VNet. U kunt desgewenst een VNet voor het gebruik van uw eigen DNS-servers, in plaats van Azure interne naamomzetting configureren.
- **[Verbinding met Internet:](#internet)**  alle Azure virtuele Machines (VM) en Cloud Services-rolexemplaren verbonden met een VNet hebben toegang tot het Internet standaard. U kunt binnenkomende toegang tot specifieke bronnen, indien nodig.
- **[Azure-resource connectiviteit:](#within-vnet)**  Azure-resources zoals Cloudservices en virtuele machines kunnen worden verbonden met hetzelfde VNet. De resources kunnen verbinden met elkaar met behulp van privé IP-adressen, zelfs als ze zich in verschillende subnetten. Azure biedt standaardroutering tussen subnetten, VNets en on-premises netwerken, zodat u niet hoeft te configureren en beheren van routes.
- **[VNet-connectiviteit:](#connect-vnets)**  vnet's kunnen worden verbonden met elkaar, het inschakelen van de resources die zijn verbonden met een VNet om te communiceren met alle bronnen op eventuele andere VNet.
- **[Lokale connectiviteit:](#connect-on-premises)**  vnet's kunnen worden verbonden met on-premises netwerken via persoonlijke netwerkverbindingen tussen uw netwerk en Azure of een site-naar-site VPN-verbinding via Internet.
- **[Wordt verkeer gefilterd:](#filtering)**  VM en Cloud Services-rol exemplaren netwerkverkeer kan worden gefilterd binnenkomend en uitgaand op bron-IP-adres en poort, doel-IP-adres en poort en protocol.
- **[Routering:](#routing)**  kunt u eventueel Azure standaard routering met BGP-routes via een netwerkgateway of configureren van uw eigen routes overschrijven.

## <a name = "isolation"></a>Netwerkisolatie en segmentering

U kunt meerdere VNets binnen elke Azure implementeren [abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) en Azure [regio](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region). Elke VNet is geïsoleerd van andere vnet's. U kunt voor elk VNet:
- Geef een aangepaste persoonlijke IP-adresruimte met behulp van openbare en persoonlijke (RFC 1918)-adressen. Resources in Azure wordt toegewezen en die is verbonden met het VNet een particulier IP-adres van de adresruimte die u toewijst.
- Het VNet segmenteren in een of meer subnetten en een deel van de VNet-adresruimte voor elk subnet toegewezen.
- Azure verschafte naamomzetting gebruiken of geef uw eigen DNS-server voor gebruik door resources die zijn verbonden met een VNet. Lees voor meer informatie over naamomzetting in vnet's, de [naamomzetting voor VM's en Cloudservices](virtual-networks-name-resolution-for-vms-and-role-instances.md) artikel.

## <a name = "internet"></a>Verbinding maken met Internet
Uitgaande verbinding met Internet hebben alle resources die zijn verbonden met een VNet standaard. Het privé IP-adres van de resource is Bronnetwerk adres vertaald (snat omzetten) naar een openbare IP-adres door de Azure-infrastructuur. Lees voor meer informatie over uitgaande internetverbinding, de [inzicht in uitgaande verbindingen in Azure](..\load-balancer\load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json#standalone-vm-with-no-instance-level-public-ip-address) artikel. U kunt de standaard-connectiviteit wijzigen door het implementeren van aangepaste Routering en het filteren van verkeer.

Inkomende communicatie voor Azure-resources van het Internet of om te communiceren met Internet, zonder snat omzetten uitgaand, moet een resource een openbaar IP-adres worden toegewezen. Lees voor meer informatie over openbare IP-adressen, de [openbare IP-adressen](virtual-network-public-ip-address.md) artikel.

## <a name="within-vnet"></a>Verbinding maken met Azure-resources
U kunt verschillende Azure-resources verbinden met een VNet, zoals virtuele Machines (VM), Cloud Services-App Service-omgevingen en virtuele-Machineschaalsets. Virtuele machines verbinding maken met een subnet binnen een VNet via een netwerkinterface (NIC). Lees voor meer informatie over NIC's, de [netwerkinterfaces](virtual-network-network-interface.md) artikel.

## <a name="connect-vnets"></a>Verbinding maken met virtuele netwerken

Kunt u VNets met elkaar, het inschakelen van de resources die zijn verbonden met een VNet-naar-tussen VNets met elkaar communiceren. U kunt een of beide van de volgende opties VNets met elkaar verbinden:
- **Peering:** kunnen resources die zijn verbonden met andere Azure VNets binnen dezelfde Azure-locatie met elkaar communiceren. De bandbreedte en de latentie tussen de VNets is hetzelfde als wanneer de resources zijn verbonden met hetzelfde VNet is. Lees voor meer informatie over de peering, de [virtuele netwerk peering](virtual-network-peering-overview.md) artikel.
- **VNet-naar-VNet-verbinding:** kunnen resources die zijn verbonden met andere Azure-VNet binnen de dezelfde of verschillende Azure-locaties. In tegenstelling tot de peering, is bandbreedte tussen VNets beperkt, omdat het verkeer door een Azure VPN-Gateway moet lopen. Lees meer informatie over het VNets verbinden met een VNet-naar-VNet-verbinding, de [een VNet-naar-VNet-verbinding configureren](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.

## <a name="connect-on-premises"></a>Verbinding maken met een on-premises netwerk

U kunt uw on-premises netwerk verbinden met een VNet met elke combinatie van de volgende opties:
- **Punt-naar-site virtueel particulier netwerk (VPN):** tot stand gebracht tussen één PC verbonden met uw netwerk en het VNet. Dit verbindingstype is handig als u alleen aan de slag met Azure of voor ontwikkelaars, omdat hiervoor weinig of geen wijzigingen in uw bestaande netwerk. De verbinding wordt de SSTP-protocol gebruikt voor gecodeerde communicatie via Internet tussen de PC en het VNet. De latentie voor een punt-naar-site VPN-verbinding is onvoorspelbaar zijn, aangezien het verkeer over het Internet wordt verzonden.
- **Site-naar-site-VPN:** tot stand gebracht tussen uw VPN-apparaat en een Azure VPN-Gateway. Dit verbindingstype kunt een on-premises-resource die geeft u toestemming voor toegang tot een VNet. De verbinding is een VPN IPSec/IKE waarmee gecodeerde communicatie via Internet tussen uw on-premises-apparaat en de Azure VPN-gateway. De latentie voor een site-naar-site-verbinding is onvoorspelbaar zijn, aangezien het verkeer over het Internet wordt verzonden.
- **Azure ExpressRoute:** tot stand gebracht tussen uw netwerk en Azure, via een ExpressRoute-partner. Deze verbinding is een privéverbinding. Verkeer niet via Internet verloopt. De latentie voor een ExpressRoute-verbinding is betrouwbaar, aangezien het verkeer niet via Internet verloopt.

Lees voor meer informatie over de vorige verbindingsopties, de [gatewayverbindingsdiagrammen topologie](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams) artikel.

## <a name="filtering"></a>Filteren van netwerkverkeer
U kunt filteren netwerkverkeer tussen subnetten op met behulp van een of beide van de volgende opties:
- **Netwerkbeveiligingsgroepen (NSG):** elke NSG kan meerdere binnenkomende en uitgaande beveiligingsregels voor verbindingen waarmee u verkeer filteren door de bron en doel-IP-adres, poort en protocol bevatten. U kunt een NSG toepassen op elke NIC op een virtuele machine. U kunt ook een NSG toepassen op het subnet een NIC of andere Azure-resource is verbonden met. Lees voor meer informatie over Nsg de [Netwerkbeveiligingsgroepen](virtual-networks-nsg.md) artikel.
- **Virtuele apparaten (NVA):** een NVA is een virtuele machine uitvoeren van software waarmee een netwerkfunctie, zoals een firewall. Een lijst met beschikbare NVAs in de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). NVAs zijn ook beschikbaar met WAN-optimalisatie en andere netwerkapparaten verkeer functies. NVAs worden doorgaans gebruikt voor de gebruiker gedefinieerde of BGP-routes. U kunt ook een NVA gebruiken voor het filteren van verkeer tussen VNets.

## <a name="routing"></a>Doorsturen van netwerkverkeer

Azure maakt routetabellen waarmee bronnen die zijn verbonden met een enkel subnet in een VNet om te communiceren met elkaar, standaard. U kunt een of beide van de volgende opties voor het onderdrukken van de Azure maakt standaardroutes implementeren:
- **Gebruiker gedefinieerde routes:** kunt u aangepaste routetabellen met routes die bepalen waar verkeer wordt doorgestuurd naar voor elk subnet. Lees het artikel [User-defined routes](virtual-networks-udr-overview.md) (Door de gebruiker gedefinieerde routes) voor meer informatie over door de gebruiker gedefinieerde routes.
- **BGP-routes:** als u uw VNet verbinding met uw on-premises netwerk via een Azure VPN-Gateway of ExpressRoute-verbinding, kunt u BGP-routes doorgegeven aan uw vnet's.

## <a name="pricing"></a>Prijzen

Er zijn geen kosten voor virtuele netwerken, subnetten, routetabellen of netwerk-beveiligingsgroepen. Uitgaande bandbreedtegebruik van Internet, openbare IP-adressen, virtueel netwerk peering, VPN-Gateways en ExpressRoute elke, hebben hun eigen structuren prijzen. Weergave de [virtueel netwerk](https://azure.microsoft.com/pricing/details/virtual-network), [VPN-Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway), en [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) prijzen van pagina's voor meer informatie.

## <a name="faq"></a>Veelgestelde vragen

Veelgestelde vragen over Virtual Network, Zie de [Veelgestelde vragen over het virtuele netwerk](virtual-networks-faq.md) artikel.


## <a name="next-steps"></a>Volgende stappen

- Maken van uw eerste VNet en verbinding maken met een paar virtuele machines, via de stappen in de [maken van uw eerste virtuele netwerk](virtual-network-get-started-vnet-subnet.md) artikel.
- Een punt-naar-site verbinding maken met een VNet met de stappen in de [een punt-naar-site-verbinding configureren](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.
- Informatie over een aantal van de andere sleutel [mogelijkheden netwerk](../networking/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) van Azure.
