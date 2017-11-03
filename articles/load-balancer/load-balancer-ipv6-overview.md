---
title: Overzicht van IPv6 voor Azure Load Balancer | Microsoft Docs
description: Understanding IPv6-ondersteuning voor Azure Load Balancer en taakverdeling virtuele machines.
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
keywords: IPv6-, azure load balancer, dual-stack, openbare IP-adres, systeemeigen ipv6, mobiele, iot
ms.assetid: 6a1d583f-a305-40fd-a94b-fa42e1943bbb
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 1902475c81c4f83f8ba69a05f9564bc65a5de833
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-ipv6-for-azure-load-balancer"></a>Overzicht van IPv6 voor Azure Load Balancer


[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Internet gerichte load balancers kunnen worden geïmplementeerd met een IPv6-adres. Dit biedt de volgende mogelijkheden naast IPv4-verbindingen:

* Oorspronkelijke end-to-end IPv6-connectiviteit tussen het openbare Internet-clients en Azure Virtual Machines (VM's) via de load balancer.
* Systeemeigen end-to-end IPv6 uitgaande connectiviteit tussen virtuele machines en openbare clients voor Internet IPv6 is ingeschakeld.

De volgende afbeelding ziet u de IPv6-functionaliteit voor Azure Load Balancer.

![Azure Load Balancer met IPv6](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

Zodra geïmplementeerd, kan een client IPv4 of IPv6-functionaliteit Internet communiceren met de openbare IPv4- of IPv6-adressen (of hostnamen) van de Azure Internet gerichte Load Balancer. De load balancer stuurt de IPv6-pakketten naar de persoonlijke IPv6-adressen van de virtuele machines met network address translation (NAT). De IPv6-Internet-client communiceren niet rechtstreeks met het IPv6-adres van de virtuele machines.

## <a name="features"></a>Functies

Systeemeigen IPv6-ondersteuning voor virtuele machines die zijn geïmplementeerd via Azure Resource Manager biedt:

1. Taakverdeling IPv6-services voor IPv6-clients op Internet
2. Systeemeigen IPv6- en IPv4-eindpunten op virtuele machines ('dual gestapelde')
3. Binnenkomende en uitgaande geïnitieerde systeemeigen IPv6-verbindingen
4. Ondersteunde protocollen zoals TCP, UDP- en HTTP (S) inschakelen een volledige reeks architecturen service

## <a name="benefits"></a>Voordelen

Deze functionaliteit kunnen de volgende voordelen:

* Voldoen aan wettelijke voorschriften vereisen dat de nieuwe toepassingen toegankelijk voor clients met alleen IPv6 zijn
* Schakel mobiele en Internet der dingen (IOT)-ontwikkelaars kunnen dual gestapelde (IPv4 + IPv6) Azure Virtual Machines gebruiken voor het oplossen van de groeiende mobile & IOT markten

## <a name="details-and-limitations"></a>Meer informatie en beperkingen

Details

* De Azure DNS-service bevat zowel IPv4 A en AAAA IPv6-records van naam en reageert met beide records voor de load balancer. De client kiezen welk adres (IPv4 of IPv6) om te communiceren met.
* Wanneer een virtuele machine een verbinding met een openbaar Internet IPv6 verbonden apparaat initieert, is IPv6-adres van de VM-bron netwerkadres vertaald (NAT) naar het openbare IPv6-adres van de load balancer.
* Virtuele machines met Linux-besturingssysteem moeten worden geconfigureerd voor het ontvangen van een IPv6-IP-adres via DHCP. Veel van de Linux-afbeeldingen in de galerie van Azure zijn al geconfigureerd voor ondersteuning voor IPv6 zonder aanpassing. Zie voor meer informatie [DHCPv6 configureren voor virtuele Linux-machines](load-balancer-ipv6-for-linux.md)
* Als u kiest voor het gebruik van een health test met de load balancer, een IPv4-test maken en deze gebruiken met de IPv4- als IPv6-eindpunten. Als de service op de virtuele machine uitgeschakeld wordt, worden zowel de IPv4- als IPv6-eindpunten buiten rotatie genomen.

Beperkingen

* U kunt geen regels voor taakverdeling IPv6 toevoegen in de Azure portal. De regels kunnen alleen worden gemaakt via de sjabloon, CLI, PowerShell.
* U kunt geen upgrade van bestaande virtuele machines voor het gebruik van IPv6-adressen. U kunt nieuwe virtuele machines moet implementeren.
* Één IPv6-adres kan worden toegewezen aan één netwerkinterface in elke virtuele machine.
* De openbare IPv6-adressen kunnen niet worden toegewezen aan een virtuele machine. Ze kunnen alleen worden toegewezen aan een load balancer.
* U kunt de reverse DNS-lookup niet configureren voor uw openbare IPv6-adressen.
* De virtuele machines met het IPv6-adressen kunnen niet lid zijn van een Azure Cloud Service. Ze kunnen worden verbonden met een Azure-netwerk (VNet) en communiceren met elkaar via hun IPv4-adressen.
* Persoonlijke IPv6-adressen kunnen worden geïmplementeerd op afzonderlijke virtuele machines in een resourcegroep, maar kunnen niet worden geïmplementeerd in een resourcegroep via-Schaalsets.
* Virtuele machines in Azure kunnen geen verbinding maken via IPv6 op andere virtuele machines, andere Azure-services of on-premises apparaten. Ze kunnen alleen de Azure load balancer communiceren via IPv6. Ze kunnen echter communiceren met deze andere resources met behulp van IPv4.
* Beveiliging van de Netwerkbeveiligingsgroep (NSG) voor IPv4 wordt ondersteund in implementaties van dual stack (IPv4 + IPv6). Nsg's niet van toepassing op de IPv6-eindpunten.
* Het IPv6-eindpunt op de virtuele machine is niet rechtstreeks blootgesteld aan internet. Het is achter een load balancer. Alleen de poorten die zijn opgegeven in de load balancer-regels zijn toegankelijk via IPv6.
* Wijzigen van de parameter IdleTimeout voor IPv6 **momenteel niet ondersteund**. De standaardwaarde is vier minuten.
* Wijzigen van de parameter loadDistributionMethod voor IPv6 **momenteel niet ondersteund**.
* Gereserveerd IP-IPv6-adressen (waarbij IPAllocationMethod = statisch) zijn **momenteel niet ondersteund**.

## <a name="next-steps"></a>Volgende stappen

Informatie over het implementeren van een load balancer met IPv6.

* [Beschikbaarheid van IPv6 per regio](https://go.microsoft.com/fwlink/?linkid=828357)
* [Een load balancer met IPv6 met een sjabloon implementeren](load-balancer-ipv6-internet-template.md)
* [Implementeren van een load balancer met IPv6 met Azure PowerShell](load-balancer-ipv6-internet-ps.md)
* [Implementeren van een load balancer met IPv6 met Azure CLI](load-balancer-ipv6-internet-cli.md)
