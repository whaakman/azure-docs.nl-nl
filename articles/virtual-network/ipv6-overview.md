---
title: Overzicht van IPv6 voor Azure Virtual Network (preview-versie)
titlesuffix: Azure Virtual Network
description: IPv6-beschrijving van IPv6-eind punten en gegevens paden in een virtueel Azure-netwerk.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.workload: infrastructure-services
ms.date: 07/15/2019
ms.author: kumud
ms.openlocfilehash: 5093b74484cd04a0c0c7afed8e2ebc725af033f5
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68249839"
---
# <a name="what-is-ipv6-for-azure-virtual-network-preview"></a>Wat is IPv6 voor Azure Virtual Network? (Preview)

Met IPv6 voor Azure Virtual Network (VNet) kunt u toepassingen in azure hosten met IPv6-en IPv4-connectiviteit, zowel binnen een virtueel netwerk als vanaf het internet. Als gevolg van de uitputting van open bare IPv4-adressen, zijn nieuwe netwerken voor Mobility en Internet of Things (IoT) vaak gebaseerd op IPv6. Zelfs lange en mobiele netwerken worden naar IPv6 getransformeerd. Alleen IPv4-Services kunnen zichzelf tegen een echt nadeel vinden in zowel de bestaande als opkomende markten. Dankzij de dual-stack IPv4/IPv6-connectiviteit kunnen door Azure gehoste services dit technologie-hiaat door lopen met wereld wijd beschik bare, dubbele gestapelde services waarmee u eenvoudig verbinding kunt maken met zowel de bestaande IPv4-als deze nieuwe IPv6-apparaten en-netwerken.

Met de oorspronkelijke IPv6-connectiviteit van Azure kunt u eenvoudig een dual stack (IPv4/IPv6) Internet connectiviteit bieden voor toepassingen die worden gehost in Azure. Hiermee kan een eenvoudige implementatie van Vm's met IPv6-connectiviteit met gelijke taak verdeling voor zowel binnenkomende als uitgaande verbindingen worden gestart. Deze functie is nog steeds beschikbaar en u vindt [hier](../load-balancer/load-balancer-ipv6-overview.md)meer informatie.
IPv6 voor Azure Virtual Network is veel meer aanbevolen, waardoor volledige IPv6-oplossings architecturen in azure kunnen worden geïmplementeerd.

> [!Important]
> IPv6 voor Azure Virtual Network is momenteel beschikbaar als open bare preview. Deze preview wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In het volgende diagram ziet u een eenvoudige implementatie van een dubbele stack (IPv4/IPv6) in Azure:

![Implementatie diagram van IPv6-netwerk](./media/ipv6-support-overview/ipv6-sample-diagram.png)

## <a name="benefits"></a>Voordelen

Voor delen van Azure Virtual Network IPv6:

- Het bereik van uw door Azure gehoste toepassingen kan worden uitgebreid naar de groeiende mobiele en Internet of Things markten.
- Dubbele, gestapelde IPv4/IPv6-Vm's bieden maximale flexibiliteit voor service-implementatie. Eén service-exemplaar kan verbinding maken met IPv4-en IPv6-compatibele internetclients.
- Bouwt voort op lange, stabiele Azure VM-to-Internet IPv6-connectiviteit.
- Standaard beveiligd omdat de IPv6-verbinding met het internet alleen wordt ingesteld wanneer u deze expliciet aanvraagt in uw implementatie.

## <a name="capabilities"></a>Functionaliteit

IPv6 voor VNet bevat de volgende mogelijkheden:

- Azure-klanten kunnen hun eigen IPv6-adres ruimte voor het virtuele netwerk definiëren om te voldoen aan de behoeften van hun toepassingen, klanten of naadloos integreren in hun on-premises IP-ruimte.
- Met dual stack (IPv4 en IPv6) virtuele netwerken met dual stack-subnetten kunnen toepassingen verbinding maken met zowel IPv4-als IPv6-resources in hun virtuele netwerk of Internet.
- Beveilig uw resources met IPv6-regels voor netwerk beveiligings groepen
- Pas de route ring van IPv6-verkeer in het virtuele netwerk aan met door de gebruiker gedefinieerde routes, met name bij het gebruik van virtuele netwerk apparaten om uw toepassing uit te breiden.
- Laat Internet-clients naadloos toegang krijgen tot uw dual stack-toepassing met behulp van het Protocol van de keuze met Azure DNS ondersteuning voor IPv6-records (AAAA). 
- Open bare standaard IPv6-Load Balancer ondersteuning voor het maken van robuuste, schaal bare toepassingen, waaronder:
    - Optionele IPv6-status test om te bepalen welke back-end-pool instanties status zijn en dus nieuwe stromen kunnen ontvangen. .  
    - Optionele regels voor uitgaande verbindingen die volledige declaratieve controle over uitgaande connectiviteit bieden om deze mogelijkheid te schalen en af te stemmen op uw specifieke behoeften.
    - Optionele meerdere front-end configuraties waarmee één load balancer meerdere open bare IPv6-IP-adressen kan gebruiken. hetzelfde frontend-protocol en dezelfde poort kunnen worden gebruikt voor alle frontend-adressen.
- Openbaar IP-adres op exemplaar niveau biedt rechtstreeks IPv6-Internet connectiviteit voor afzonderlijke Vm's.
- Voeg eenvoudig IPv6-connectiviteit toe aan bestaande IPv4-implementaties met upgrade-in-place.
- Maak dual stack-toepassingen die automatisch worden geschaald naar uw belasting met schaal sets voor virtuele machines.
- Portal ondersteuning voor de preview omvat interactieve maken/bewerken/verwijderen van dual stack (IPv4 + IPv6) virtuele netwerken en subnetten, IPv6-netwerk beveiligings groeps regels, door IPv6 door de gebruiker gedefinieerde routes en open bare IPv6-Ip's.  

## <a name="limitations"></a>Beperkingen
De preview-versie van IPv6 voor het virtuele netwerk van Azure heeft de volgende beperkingen:
- IPv6 voor het virtuele netwerk van Azure (preview) is beschikbaar in alle wereld wijde Azure-regio's, maar alleen in wereld wijde Azure-niet de overheids Clouds.
- Portal ondersteuning voor Standard Load Balancer onderdelen is alleen-lezen.  Volledige ondersteuning en documentatie (met voor beelden) is echter beschikbaar voor Standard Load Balancer implementaties met behulp van Azure Power shell en de opdracht regel interface (CLI).   
- Network Watcher ondersteuning voor de preview is beperkt tot NSG-stroom logboeken en netwerk pakket opnames.
- Peering van virtuele netwerken (regionaal of wereld wijd) wordt niet ondersteund in de preview-versie.
- Bij gebruik van standaard-IPv6-externe Load Balancer gelden de volgende limieten: 
  - Uitgaande regels kunnen verwijzen naar meerdere front-end open bare Ip's, maar kunnen **niet** verwijzen naar een openbaar IPv6-voor voegsel. Het open bare IP-voor voegsel ondersteunt alleen IPv4-voor voegsels.
  - De IPv6-taakverdelings regels kunnen de *zwevende IP* -functie **niet** gebruiken. Het opnieuw gebruiken van een poort op back-end-exemplaren wordt alleen ondersteund met IPv4.
- Het reserveren van een blok met Internet gerichte IPv6-adressen wordt niet ondersteund door de functie voor voegsel van het open bare IP-adres van Azure.

## <a name="pricing"></a>Prijzen

IPv6 Azure-resources en-band breedte worden in rekening gebracht tegen dezelfde tarieven als IPv4. Er zijn geen extra of andere kosten voor IPv6. U kunt details vinden over de prijzen voor [open bare IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses/), [netwerk bandbreedte](https://azure.microsoft.com/pricing/details/bandwidth/)of [Load Balancer](https://azure.microsoft.com/pricing/details/load-balancer/).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [implementeren van een IPv6-toepassing met dubbele stack met Azure PowerShell](virtual-network-ipv4-ipv6-dual-stack-powershell.md).
- Meer informatie over het [implementeren van een IPv6-toepassing met dubbele stacks met behulp van Azure cli](virtual-network-ipv4-ipv6-dual-stack-cli.md).
