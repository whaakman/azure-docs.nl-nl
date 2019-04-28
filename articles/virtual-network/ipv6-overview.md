---
title: Overzicht van IPv6 voor Azure-netwerk (Preview)
titlesuffix: Azure Virtual Network
description: IPv6-beschrijving van IPv6-eindpunten en gegevens in een Azure-netwerk.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.workload: infrastructure-services
ms.date: 04/22/2019
ms.author: kumud
ms.openlocfilehash: 0ec650880a45f6383b24b5ac810fc2ee745806b7
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130937"
---
# <a name="what-is-ipv6-for-azure-virtual-network-preview"></a>Wat is IPv6 voor Azure Virtual Network? (Preview)

IPv6 voor Azure Virtual Network (VNET) kunt u hosting van toepassingen in Azure met IPv6- en IPv4-connectiviteit, zowel binnen een virtueel netwerk en naar en vanaf het Internet. Nieuwe netwerken voor mobiliteit en een Internet of Things (IoT) zijn vanwege de uitputting van openbare IPv4-adressen, vaak gebaseerd op IPv6. Even lang tot stand gebracht ISP- en mobiele netwerken zijn die wordt omgezet naar IPv6. IPv4-services kunnen zelf vinden op een echte nadeel in bestaande en nieuwe markten. Dual-stack IPv4/IPv6-connectiviteit kan Azure gehoste services deze technologie tussenruimte met wereldwijd beschikbaar, dual-gestapeld services die direct verbinding met zowel de bestaande IPv4- en deze nieuwe IPv6-apparaten en netwerken maken passeren.

Het oorspronkelijke IPv6-connectiviteit van Azure kunt eenvoudig dual stack (IPv4/IPv6) internetverbinding bieden voor toepassingen die worden gehost in Azure. Hierdoor kan voor een eenvoudige implementatie van virtuele machines met gelijke IPv6-connectiviteit voor zowel binnenkomende als uitgaande verbindingen. Deze functie is nog steeds beschikbaar en meer informatie vindt u [hier](../load-balancer/load-balancer-ipv6-overview.md).
IPv6 voor Azure-netwerk is veel meer volledige aanbevolen-volledige IPv6-architecturen worden geïmplementeerd in Azure inschakelen.

> [!Important]
> IPv6 voor Azure Virtual Network is momenteel in openbare preview. Deze preview wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Het volgende diagram ziet u een eenvoudige dual stack (IPv4/IPv6)-implementatie in Azure:

![Implementatie van IPv6-netwerkdiagram](./media/ipv6-support-overview/ipv6-sample-diagram.png)

## <a name="benefits"></a>Voordelen

Azure Virtual Network-IPv6-voordelen:

- Helpt vergroot het bereik van uw Azure gehoste toepassingen in de groeiende mobiele en Internet of Things markten.
- Dubbele gestapelde IPv4/IPv6-virtuele machines hebben de flexibiliteit van de maximale service-implementatie. Een enkele service-exemplaar kunt verbinden met zowel IPv4 als IPv6-compatibele Internet-clients.
- Bouwt voort op de lange-tot stand gebracht en stabiele Azure VM met Internet IPv6-connectiviteit.
- Beveilig standaard omdat IPv6-verbinding met Internet wordt alleen ingesteld als u deze expliciet in uw implementatie aanvragen.

## <a name="capabilities"></a>Functionaliteit

IPv6-ondersteuning voor virtuele machines bevat de volgende mogelijkheden:

- Azure-klanten kunnen hun eigen IPv6-adresruimte van de virtuele netwerk om te voldoen aan de behoeften van hun toepassingen, klanten, of naadloos integreren in hun on-premises IP-adresruimte definiëren.
- Dual-stack (IPv4 en IPv6) virtuele netwerken met dual-stack subnetten inschakelen toepassingen verbinding kunnen maken met zowel IPv4 als IPv6-resources in het virtuele netwerk of -Internet.
- Uw resources beveiligen met IPv6-regels voor Netwerkbeveiligingsgroepen
- Pas de routering van IPv6-verkeer in uw virtuele netwerk met door de gebruiker gedefinieerde Routes - met name bij het gebruik van virtuele netwerkapparaten uitbreiden van uw toepassing.
- IPv6-Load Balancer-ondersteuning om robuuste, schaalbare toepassingen te maken met Azure DNS-ondersteuning voor AAAA-records voor IPv6-openbare IP-adressen.
- IPv6-connectiviteit eenvoudig toevoegen aan bestaande IPv4-implementaties met een Upgrade ter plekke.
- Dual-stack-toepassingen die automatisch omhoog naar uw belasting met virtuele-machineschaalsets schalen maken.

## <a name="limitations"></a>Beperkingen
De preview-versie van IPv6 voor Azure-netwerk heeft de volgende beperkingen:
- IPv6 voor Azure-netwerk (Preview) is beschikbaar in alle globale Azure-regio's, maar alleen in globale Azure, niet de overheidsclouds bevindt.   
- Portal ondersteuning voor de Preview-versie is beperkt tot alleen voor IPv6-configuratie, veel, maar niet alle, bekijken, maar IPv6 voor het virtuele netwerk heeft volledige ondersteuning en documentatie (met voorbeelden) voor IPv6-implementatie met behulp van Azure Powershell en Command Line Interface (CLI).
- Network Watcher-ondersteuning voor de Preview-versie is beperkt tot NSG-stroomlogboeken en netwerkpakketopnames.
- Load Balancing-ondersteuning voor de Preview-versie is oorspronkelijk beperkt tot Basic Load Balancer.
- Op exemplaarniveau openbare IP-adressen (openbare IP's rechtstreeks op een virtuele machine) wordt niet ondersteund in de Preview-versie.  
- Peering op virtueel netwerk wordt (regionaal of wereldwijd) niet ondersteund in de Preview-versie. 

## <a name="pricing"></a>Prijzen

IPv6-Azure-resources en bandbreedte worden in rekening gebracht op basis van de dezelfde tarieven als voor IPv4. Er zijn geen aanvullende of andere kosten voor IPv6. U vindt meer informatie over de prijzen voor [openbare IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses/), [netwerkbandbreedte](https://azure.microsoft.com/pricing/details/bandwidth/), of [Load Balancer](https://azure.microsoft.com/pricing/details/load-balancer/).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [implementeren van een IPv6 dual stack-toepassing met behulp van Azure PowerShell](virtual-network-ipv4-ipv6-dual-stack-powershell.md).
- Meer informatie over het [implementeren van een IPv6 dual stack-toepassing met behulp van Azure CLI](virtual-network-ipv4-ipv6-dual-stack-cli.md).
