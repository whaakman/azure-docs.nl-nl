---
title: Azure Virtual Network | Microsoft Docs
description: Meer informatie over concepten en functies van Azure Virtual Network.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: As someone with a basic network background that is new to Azure, I want to understand the capabilities of Azure Virtual Network, so that my Azure resources such as VMs, can securely communicate with each other, the internet, and my on-premises resources.
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/23/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: 072a4a483cb39a6f2827b6d5973ec544fd58d09c
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="what-is-azure-virtual-network"></a>Wat is Azure Virtual Network?

Via Azure Virtual Network kunnen veel soorten Azure-resources, zoals virtuele Azure-machines, veilig communiceren met elkaar, internet en on-premises netwerken. Azure Virtual Network bevat de volgende kernmogelijkheden: 

## <a name="isolation-and-segmentation"></a>Isolatie en segmentatie

U kunt meerdere virtuele netwerken binnen elk Azure-[abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) en elke Azure-[regio](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region) implementeren. Elk virtuele netwerk is geïsoleerd van andere virtuele netwerken. Voor elke virtueel netwerk kunt u het volgende:
- Een aangepaste persoonlijke IP-adresruimte opgeven met behulp van openbare en persoonlijke adressen (RFC 1918). Azure wijst resources in een virtueel netwerk een persoonlijk IP-adres toe op basis van de adresruimte die u toewijst.
- Het virtuele netwerk segmenteren in een of meer subnetten en een deel van de adresruimte van het virtuele netwerk aan elk subnet toewijzen.
- Door Azure geleverd naamomzetting gebruiken of uw eigen DNS-server opgeven, voor gebruik door resources in een virtueel netwerk.

## <a name="communicate-with-the-internet"></a>Communiceren met internet

Alle resources in een virtueel netwerk kunnen standaard uitgaand communiceren met internet. U kunt binnenkomend communiceren met een resource door er een openbaar IP-adres aan toe te wijzen. Zie [Openbare IP-adressen](virtual-network-public-ip-address.md) voor meer informatie.

## <a name="communicate-between-azure-resources"></a>Communicatie tussen Azure-resources

Azure-resources communiceren veilig met elkaar op een van de volgende manieren:

- **Via een virtueel netwerk**: u kunt virtuele machines en diverse andere soorten Azure-resources implementeren op een virtueel netwerk, zoals Azure App Service-omgevingen en Microsoft Azure Virtual Machine Scale Sets. Zie [Integratie van virtuele netwerkservices](virtual-network-for-azure-services.md) voor een volledige lijst met Azure-resources die u in een virtueel netwerk kunt implementeren. 
- **Via een service-eindpunt van een virtueel netwerk**: breid de openbare adresruimte van uw virtuele netwerk en de identiteit van uw virtuele netwerk uit naar Azure-serviceresources, zoals Azure Storage-accounts en Azure SQL Databases, via een directe verbinding. Met service-eindpunten kunt u uw kritieke Azure-serviceresources alleen beveiligen naar een virtueel netwerk. Zie [Overzicht van service-eindpunten voor virtuele netwerken](virtual-network-service-endpoints-overview.md) voor meer informatie.
 
## <a name="communicate-with-on-premises-resources"></a>Communiceren met on-premises resources

U kunt uw on-premises computers en netwerken verbinden met een virtueel netwerk via een combinatie van de volgende opties:

- **Point-to-site virtueel particulier netwerk (VPN):** wordt tot stand gebracht tussen een virtueel netwerk en een enkele computer in uw netwerk. Elke computer die verbinding wil met een virtueel netwerk moet de verbinding hiervoor configureren. Dit verbindingstype is handig als u net aan de slag gaat met Azure of voor ontwikkelaars, omdat hiervoor weinig of geen wijzigingen in uw bestaande netwerk nodig zijn. De communicatie tussen uw computer en een virtueel netwerk wordt verzonden via een gecodeerde tunnel via internet. Zie [Point-to-site VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#P2S) voor meer informatie.
- **Site-to-site VPN:** wordt tot stand gebracht tussen uw on-premises VPN-apparaat en een Azure VPN Gateway die is geïmplementeerd in een virtueel netwerk. Met dit verbindingstype krijgen alle on-premises resource die u toestemming geeft toegang tot een virtueel netwerk. De communicatie tussen uw on-premises VPN-apparaat en een Azure VPN Gateway wordt verzonden via een gecodeerde tunnel via internet. Zie [Site-to-site VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) voor meer informatie.
- **Azure ExpressRoute:** wordt tot stand gebracht tussen uw netwerk en Azure, via een ExpressRoute-partner. Deze verbinding is een privéverbinding. Verkeer gaat niet via internet. Zie [ExpressRoute](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#ExpressRoute) voor meer informatie.

## <a name="filter-network-traffic"></a>Netwerkverkeer filteren
U kunt netwerkverkeer filteren tussen subnetten met behulp van een of beide van de volgende opties:
- **Netwerkbeveiligingsgroepen:** een netwerkbeveiligingsgroep kan meerdere binnenkomende en uitgaande beveiligingsregels bevatten waarmee u verkeer van en naar resources kunt filteren op bron- en doel-IP-adres, poort en protocol. Zie [Netwerkbeveiligingsgroepen](security-overview.md#network-security-groups) voor meer informatie.
- **Virtuele apparaten:** een virtueel netwerkapparaat is een virtuele machine die een netwerkfunctie uitvoert, zoals een firewall, WAN-optimalisatie of een andere netwerkfunctie. Zie [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances) voor meer informatie over het bekijken van een lijst met beschikbare virtuele netwerkapparaten die u in een virtueel netwerk kunt implementeren.

## <a name="route-network-traffic"></a>Netwerkverkeer routeren

Azure routeert verkeer standaard tussen subnetten, verbonden virtuele netwerken, on-premises netwerken en internet. U kunt een of beide van de volgende opties implementeren om de standaardroutes die Azure maakt te onderdrukken:
- **Routetabellen:** u kunt aangepaste routetabellen maken met routes die bepalen waar verkeer naartoe wordt doorgestuurd voor elk subnet. Meer informatie over [routetabellen](virtual-networks-udr-overview.md#user-defined).
- **BGP-routes (Border Gateway Protocol):** als u uw virtuele netwerk verbindt met uw on-premises netwerk via een Azure VPN Gateway- of ExpressRoute-verbinding, kunt u uw lokale BGP-routes doorgegeven aan uw virtuele netwerken. Lees meer over het gebruik van BGP met [Azure VPN Gateway](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en [ExpressRoute](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#dynamic-route-exchange).

## <a name="connect-virtual-networks"></a>Virtuele netwerken verbinden

U kunt virtuele netwerken met elkaar verbinden, zodat resources in beide virtuele netwerken met elkaar kunnen communiceren, met behulp van peering voor virtuele netwerken. De virtuele netwerken die u met elkaar verbindt, kunnen zich in dezelfde of verschillende Azure-regio's bevinden. Zie [Peering van virtuele netwerken](virtual-network-peering-overview.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

U hebt nu een overzicht van Azure Virtual Network. Als u aan de slag wilt gaan met een virtueel netwerk, maakt u een virtueel netwerk, implementeert u een paar VM's en laat u de virtuele machines met elkaar communiceren. Zie de snelstart [Een virtueel netwerk maken](quick-create-portal.md) voor meer informatie.