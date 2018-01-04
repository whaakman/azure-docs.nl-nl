---
title: Azure Virtual Network | Microsoft Docs
description: Meer informatie over Azure Virtual Network-concepten en functies.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2017
ms.author: jdial
ms.openlocfilehash: 6cc7035e798ef72f69958a7536a741f80939d4fe
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="azure-virtual-network"></a>Azure Virtual Network

De Microsoft Azure Virtual Network-service biedt Azure-bronnen te veilig te communiceren met elkaar in een virtueel netwerk. Een virtueel netwerk is een logische isolatie van de Azure-cloud toegewezen aan uw abonnement. U kunt virtuele netwerken verbinding maken met andere virtuele netwerken, of uw on-premises netwerk. De volgende afbeelding ziet u de mogelijkheden van de service Azure Virtual Network:

![Netwerkdiagram](./media/virtual-networks-overview/virtual-network-overview.png)

Klik op de mogelijkheid voor meer informatie over de volgende mogelijkheden van Azure Virtual Network:
- **[Isolatie:](#isolation)**  virtuele netwerken zijn geïsoleerd van elkaar. Kunt u afzonderlijke virtuele netwerken die gebruikmaken van de dezelfde CIDR (bijvoorbeeld 10.0.0.0/0) voor ontwikkelen, testen en productie adres blokken. Als u daarentegen, kunt u meerdere virtuele netwerken die gebruikmaken van verschillende CIDR-adresblokken en de netwerken met elkaar verbinden. U kunt een virtueel netwerk segmenteren in meerdere subnetten. Azure biedt interne naamomzetting voor resources geïmplementeerd in een virtueel netwerk. Indien nodig, kunt u een virtueel netwerk voor het gebruik van uw eigen DNS-servers, in plaats van Azure interne naamomzetting configureren.
- **[Internetcommunicatie:](#internet)**  Resources, zoals virtuele machines die zijn geïmplementeerd in een virtueel netwerk, hebt u toegang tot het Internet standaard. U kunt binnenkomende toegang tot specifieke bronnen, indien nodig.
- **[Communicatie van de Azure-resource:](#within-vnet)**  Azure-resources geïmplementeerd in een virtueel netwerk kunnen communiceren met elkaar met behulp van privé IP-adressen, zelfs als de bronnen in verschillende subnetten wordt geïmplementeerd. Azure biedt standaardroutering tussen subnetten, verbonden virtuele netwerken en on-premises netwerken, zodat u niet hoeft te configureren en beheren van routes. Indien gewenst, kunt u de Azure routering kunt aanpassen.
- **[Virtuele netwerkverbinding:](#connect-vnets)**  virtuele netwerken kunnen worden verbonden met elkaar, waardoor bronnen in een virtueel netwerk om te communiceren met resources in een virtueel netwerk.
- **[Lokale connectiviteit:](#connect-on-premises)**  een virtueel netwerk kan worden verbonden met een on-premises netwerk, waardoor bronnen voor de communicatie tussen elkaar.
- **[Wordt verkeer gefilterd:](#filtering)**  kunt u netwerkverkeer naar en van resources in een virtueel netwerk door IP-adres en poort, doel-IP-adres en poort en protocol filteren.
- **[Routering:](#routing)**  u kunt eventueel Azure standaard routering door het configureren van uw eigen routes overschrijven of doorgeven van BGP routes via een netwerkgateway.

## <a name = "isolation"></a>Netwerkisolatie en segmentering

U kunt meerdere virtuele netwerken in elke Azure implementeren [abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) en Azure [regio](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region). Elk virtuele netwerk is geïsoleerd van andere virtuele netwerken. U kunt voor elke virtuele netwerk:
- Geef een aangepaste persoonlijke IP-adresruimte met behulp van openbare en persoonlijke (RFC 1918)-adressen. Azure wijst bronnen in een virtueel netwerk een particulier IP-adres van de adresruimte die u toewijst.
- Het virtuele netwerk segmenteren in een of meer subnetten en een deel van de adresruimte van het virtuele netwerk op elk subnet toegewezen.
- Azure verschafte naamomzetting gebruiken of uw eigen DNS-server, voor gebruik door de bronnen in een virtueel netwerk opgeven. Zie voor meer informatie over naamomzetting in virtuele netwerken, [naamomzetting voor resources in virtuele netwerken](virtual-networks-name-resolution-for-vms-and-role-instances.md) artikel.

## <a name = "internet"></a>Internetcommunicatie
Alle resources in een virtueel netwerk kunnen communiceren uitgaand naar het Internet. Het privé IP-adres van de resource is standaard Bronnetwerk adres vertaald (snat omzetten) naar een openbare IP-adres geselecteerd door de Azure-infrastructuur. Lees voor meer informatie over uitgaande internetverbinding, de [inzicht in uitgaande verbindingen in Azure](..\load-balancer\load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json#standalone-vm-with-no-instance-level-public-ip-address) artikel. Om te voorkomen dat een uitgaande internetverbinding, kunt u aangepaste routes of filteren van verkeer te implementeren.

Inkomende communicatie voor Azure-resources van het Internet of om te communiceren met Internet, zonder snat omzetten uitgaand, moet een resource een openbaar IP-adres worden toegewezen. Lees voor meer informatie over openbare IP-adressen, de [openbare IP-adressen](virtual-network-public-ip-address.md) artikel.

## <a name="within-vnet"></a>Veilige communicatie tussen Azure-resources

U kunt virtuele machines binnen een virtueel netwerk kunt implementeren. Virtuele machines communiceren met andere resources in een virtueel netwerk via een netwerkinterface. Zie voor meer informatie over netwerkinterfaces, [netwerkinterfaces](virtual-network-network-interface.md).

U kunt ook diverse andere typen van Azure-resources implementeren met een virtueel netwerk, zoals Azure App Service-omgevingen en Azure virtuele Machine-Schaalsets. Zie voor een volledige lijst van Azure-resources u in een virtueel netwerk implementeren kunt [virtuele netwerk-integratie voor Azure-services](virtual-network-for-azure-services.md).

Sommige resources kunnen niet worden geïmplementeerd in een virtueel netwerk, maar u communicatie op resources binnen een virtueel netwerk alleen beperken inschakelen. Zie voor meer informatie over het beperken van toegang tot bronnen, [Virtual network service-eindpunten](virtual-network-service-endpoints-overview.md). 

## <a name="connect-vnets"></a>Verbinding maken met virtuele netwerken

U kunt virtuele netwerken met elkaar verbinden resources in een virtueel netwerk om te communiceren met elkaar met behulp van virtueel netwerk peering inschakelen. De bandbreedte en de latentie van communicatie tussen bronnen in verschillende virtuele netwerken is hetzelfde als de bronnen zijn in hetzelfde virtuele netwerk is. Lees voor meer informatie over de peering, de [virtuele netwerk peering](virtual-network-peering-overview.md) artikel.

## <a name="connect-on-premises"></a>Verbinding maken met een on-premises netwerk

U kunt uw on-premises netwerk verbinden met een virtueel netwerk met een combinatie van de volgende opties:
- **Punt-naar-site virtueel particulier netwerk (VPN):** tot stand gebracht tussen een virtueel netwerk en één computer in uw netwerk. Elke computer die verbinding met een virtueel netwerk tot stand te wil moet de verbinding afzonderlijk configureren. Dit verbindingstype is handig als u alleen aan de slag met Azure of voor ontwikkelaars, omdat hiervoor weinig of geen wijzigingen in uw bestaande netwerk. De verbinding wordt de SSTP-protocol gebruikt voor gecodeerde communicatie via Internet tussen de PC en een virtueel netwerk. De latentie voor een punt-naar-site VPN-verbinding is onvoorspelbaar zijn, aangezien het verkeer over het Internet wordt verzonden.
- **Site-naar-site-VPN:** tot stand gebracht tussen uw VPN-apparaat en een Azure VPN-Gateway is geïmplementeerd in een virtueel netwerk. Dit verbindingstype kunt een on-premises-resource die geeft u toestemming voor toegang tot een virtueel netwerk. De verbinding is een VPN IPSec/IKE waarmee gecodeerde communicatie via Internet tussen uw on-premises-apparaat en de Azure VPN-gateway. De latentie voor een site-naar-site-verbinding is onvoorspelbaar zijn, aangezien het verkeer over het Internet wordt verzonden.
- **Azure ExpressRoute:** tot stand gebracht tussen uw netwerk en Azure, via een ExpressRoute-partner. Deze verbinding is een privéverbinding. Verkeer niet via Internet verloopt. De latentie voor een ExpressRoute-verbinding is betrouwbaar, aangezien het verkeer niet via Internet verloopt.

Zie voor meer informatie over de vorige verbindingsopties, [gatewayverbindingsdiagrammen topologie](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams).

## <a name="filtering"></a>Filteren van netwerkverkeer
U kunt filteren netwerkverkeer tussen subnetten op met behulp van een of beide van de volgende opties:
- **Netwerkbeveiligingsgroepen:** een netwerkbeveiligingsgroep kan meerdere binnenkomende en uitgaande beveiligingsregels voor verbindingen waarmee u verkeer filteren door de bron en doel-IP-adres, poort en protocol bevatten. U kunt een netwerkbeveiligingsgroep toepassen op elke netwerkinterface in een virtuele machine. U kunt ook een netwerkbeveiligingsgroep toepassen op het subnet een netwerkinterface of andere Azure-resource is in. Zie voor meer informatie over netwerkbeveiligingsgroepen, [Netwerkbeveiligingsgroepen](security-overview.md#network-security-groups).
- **Virtuele apparaten:** een virtueel netwerk-apparaat is een virtuele machine met software waarmee een netwerkfunctie, zoals een firewall. Een lijst met beschikbare netwerken virtuele apparaten in de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). Virtuele netwerkapparaten zijn ook beschikbaar die bieden WAN-optimalisatie en andere netwerkapparaten verkeer functies. Virtuele netwerkapparaten worden doorgaans gebruikt voor de gebruiker gedefinieerde of BGP-routes. U kunt ook een virtueel netwerkapparaat gebruiken voor het filteren van verkeer tussen virtuele netwerken.

## <a name="routing"></a>Doorsturen van netwerkverkeer

Azure maakt routetabellen waarmee bronnen die standaard op enkel subnet in een virtueel netwerk om te communiceren met elkaar en het Internet zijn aangesloten. U kunt een of beide van de volgende opties voor het onderdrukken van de Azure maakt standaardroutes implementeren:
- **Gebruiker gedefinieerde routes:** kunt u aangepaste routetabellen met routes die bepalen waar verkeer wordt doorgestuurd naar voor elk subnet. Zie voor meer informatie over de gebruiker gedefinieerde routes, [gebruiker gedefinieerde routes](virtual-networks-udr-overview.md#user-defined).
- **BGP-routes:** als u het virtuele netwerk verbinding met uw on-premises netwerk via een Azure VPN-Gateway of ExpressRoute-verbinding, kunt u BGP-routes doorgegeven aan uw virtuele netwerken.

## <a name="pricing"></a>Prijzen

Er zijn geen kosten voor virtuele netwerken, subnetten, routetabellen of netwerk-beveiligingsgroepen. Uitgaande bandbreedtegebruik van Internet, openbare IP-adressen, virtueel netwerk peering, VPN-Gateways en ExpressRoute elke, hebben hun eigen structuren prijzen. Weergave de [virtueel netwerk](https://azure.microsoft.com/pricing/details/virtual-network), [VPN-Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway), en [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) prijzen van pagina's voor meer informatie.

## <a name="faq"></a>Veelgestelde vragen

Veelgestelde vragen over Azure Virtual Network, Zie de [virtueel netwerk Veelgestelde vragen over](virtual-networks-faq.md) artikel.

## <a name="next-steps"></a>Volgende stappen

- Uw eerste virtuele netwerk maken en implementeren van een paar virtuele machines in de App, via de stappen in [maken van uw eerste virtuele netwerk](virtual-network-get-started-vnet-subnet.md).
- Een punt-naar-site verbinding maken met een virtueel netwerk met de stappen in [een punt-naar-site-verbinding configureren](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Informatie over een aantal van de andere sleutel [mogelijkheden netwerk](../networking/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) van Azure.
