---
title: Overzicht van IPv6 voor Azure Load Balancer | Microsoft Docs
description: Understanding IPv6-ondersteuning voor Azure Load Balancer en VM's met load balancing.
services: load-balancer
documentationcenter: na
author: KumudD
keywords: IPv6-, azure-load balancer, dual-stack, openbaar IP-adres, systeemeigen IPv6-, mobiele, iot
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/24/2018
ms.author: kumud
ms.openlocfilehash: 379407c8d9e5c6a5a0476fd88ea2dda6e7994e0f
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50739325"
---
# <a name="overview-of-ipv6-for-azure-load-balancer"></a>Overzicht van IPv6 voor Azure Load Balancer


>[!NOTE] 
>Azure Load Balancer ondersteunt twee verschillende typen: Basic en Standard. In dit artikel wordt Basic Load Balancer beschreven. Zie voor meer informatie over Standard Load Balancer [overzicht van Standard Load Balancer](load-balancer-standard-overview.md).

Internetgerichte load balancers kunnen worden geïmplementeerd met een IPv6-adres. Naast het IPv4-connectiviteit kunt hiermee de volgende mogelijkheden:

* Systeemeigen end-to-end IPv6-connectiviteit tussen het openbare Internet-clients en Azure Virtual Machines (VM's) via de load balancer.
* Systeemeigen end-to-end IPv6 uitgaande connectiviteit tussen virtuele machines en openbare clients Internet IPv6 is ingeschakeld.

De volgende afbeelding ziet u de functionaliteit voor IPv6 voor Azure Load Balancer.

![Azure Load Balancer met IPv6](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

Zodra geïmplementeerd, betekent dit dat een client IPv4 of IPv6-functionaliteit Internet zich van de internetgerichte Load Balancer kan communiceren met de openbare IPv4 of IPv6-adressen (of hostnamen). De load balancer routeert het IPv6-pakketten naar de privé-IPv6-adressen van de virtuele machines met network address translation (NAT). De IPv6-Internet-client kan niet communiceren rechtstreeks met het IPv6-adres van de virtuele machines.

## <a name="features"></a>Functies

Systeemeigen IPv6-ondersteuning voor virtuele machines die zijn geïmplementeerd via Azure Resource Manager biedt:

1. IPv6-services met Load Balancing voor IPv6-clients op Internet
2. Systeemeigen IPv6- en IPv4-eindpunten op VM's ('dual gestapeld")
3. Inkomende en uitgaande geïnitieerde systeemeigen IPv6-verbindingen
4. Ondersteunde protocollen, zoals TCP, UDP en HTTP (S) inschakelen een uitgebreid scala aan service-architecturen

## <a name="benefits"></a>Voordelen

Met deze functionaliteit kunnen de volgende belangrijke voordelen:

* Voldoen aan wettelijke voorschriften vereisen dat nieuwe toepassingen toegankelijk voor IPv6-clients
* Schakel mobiele en Internet of things (IOT)-ontwikkelaars dual-gestapeld (IPv4 + IPv6) Azure Virtual Machines gebruiken om op te lossen de groeiende mobiele en IOT-markten

## <a name="details-and-limitations"></a>Meer informatie en beperkingen

Details

* De Azure DNS-service bevat zowel een IPv4 en IPv6-AAAA naamrecords en reageert met beide records voor de load balancer. De client kiezen welk adres (IPv4 of IPv6) om te communiceren met.
* Wanneer een virtuele machine maken van verbinding met een openbaar IPv6 met Internet verbonden apparaat, is de bron van de virtuele machine IPv6-adres netwerkadres vertaald (NAT) naar het openbare IPv6-adres van de load balancer.
* Virtuele machines met het Linux-besturingssysteem moeten worden geconfigureerd voor het ontvangen van een IPv6 IP-adres via DHCP. Veel van de Linux-installatiekopieën in de Azure-galerie zijn al geconfigureerd voor ondersteuning voor IPv6 zonder aanpassingen. Zie voor meer informatie, [DHCPv6 configureren voor virtuele Linux-machines](load-balancer-ipv6-for-linux.md)
* Als u het gebruik van een statustest met de load balancer kiest, maakt u een IPv4-test en deze gebruiken met zowel de IPv4 en IPv6-eindpunten. Als de service op de virtuele machine uitgeschakeld wordt, worden zowel de IPv4 en IPv6-eindpunten uit rotatie gehaald.

Beperkingen

* U kunt geen IPv6-taakverdelingsregels toevoegen in Azure portal. De regels kunnen alleen worden gemaakt via de sjabloon, CLI, PowerShell.
* U kunt geen upgrade van bestaande VM's voor het gebruik van IPv6-adressen. U moet nieuwe VM's implementeren.
* Één IPv6-adres kan worden toegewezen aan één netwerkinterface in elke virtuele machine.
* De openbare IPv6-adressen kunnen niet worden toegewezen aan een virtuele machine. Ze kunnen alleen worden toegewezen aan een load balancer.
* U kunt de omgekeerde DNS-zoekactie niet configureren voor uw openbare IPv6-adressen.
* De virtuele machines met het IPv6-adressen kunnen niet worden leden van een Azure-Cloudservice. Ze kunnen worden verbonden met een Azure-netwerk (VNet) en communiceren met elkaar via hun IPv4-adressen.
* Privé-IPv6-adressen kunnen worden geïmplementeerd op afzonderlijke virtuele machines in een resourcegroep gemaakt maar kunnen niet worden geïmplementeerd in een resourcegroep via Scale Sets.
* Azure-VM's kunnen geen verbinding maken via IPv6 op andere virtuele machines, andere Azure-services of on-premises apparaten. Ze kunnen alleen communiceren met de Azure load balancer via IPv6. Ze kunnen echter communiceren met deze andere resources met behulp van IPv4.
* Beveiliging van de Netwerkbeveiligingsgroep (NSG) voor IPv4 wordt ondersteund in implementaties van dual stack (IPv4 + IPv6). Nsg's niet van toepassing op het IPv6-eindpunten.
* Het IPv6-eindpunt op de virtuele machine is niet rechtstreeks blootgesteld aan internet. Deze bevindt zich achter een load balancer. Alleen de poorten die zijn opgegeven in de load balancer-regels zijn toegankelijk via IPv6.
* Wijzigen van de parameter IdleTimeout voor IPv6 is **momenteel niet ondersteund**. De standaardwaarde is vier minuten.
* Wijzigen van de parameter loadDistributionMethod voor IPv6 is **momenteel niet ondersteund**.
* IPv6-IP-adressen gereserveerd (waarbij IPAllocationMethod = statisch) worden **momenteel niet ondersteund**.
* NAT64 (vertaling van IPv6 naar IPv4) wordt niet ondersteund.

## <a name="next-steps"></a>Volgende stappen

Informatie over het implementeren van een load balancer met IPv6.

* [Beschikbaarheid van IPv6 per regio](https://go.microsoft.com/fwlink/?linkid=828357)
* [Een load balancer met IPv6 met behulp van een sjabloon implementeren](load-balancer-ipv6-internet-template.md)
* [Een load balancer implementeren met IPv6 met Azure PowerShell](load-balancer-ipv6-internet-ps.md)
* [Een load balancer implementeren met IPv6 met Azure CLI](load-balancer-ipv6-internet-cli.md)
