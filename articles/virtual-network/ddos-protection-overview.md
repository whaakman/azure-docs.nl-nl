---
title: Overzicht van Azure DDoS-bescherming Standard | Microsoft Docs
description: Meer informatie over de service Azure DDoS-bescherming.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2018
ms.author: jdial
ms.openlocfilehash: 705f69f9143e3d2b27a3099f340218aaa12931f8
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
ms.locfileid: "31601896"
---
# <a name="azure-ddos-protection-standard-overview"></a>Overzicht van Azure Standard van DDoS-bescherming

Gedistribueerde denial-of service (DDoS)-aanvallen zijn enkele van de grootste uitdagingen beschikbaarheid en beveiliging gerichte klanten die hun toepassingen naar de cloud wilt verplaatsen. Een DDoS-aanval probeert een toepassing bronnen, waardoor de toepassing niet beschikbaar voor legitieme gebruikers. DDoS-aanvallen kunnen worden gericht op een willekeurig eindpunt dat openbaar bereikbaar is via het internet.

Azure DDoS-bescherming, gecombineerd met aanbevolen procedures voor de toepassing-ontwerp, bieden bescherming tegen DDoS-aanvallen. Azure DDos-bescherming biedt de volgende Servicelagen:

- **Basic**: automatisch ingeschakeld als onderdeel van de Azure-platform, zonder extra kosten. Controle van netwerkverkeer altijd op en realtime risicobeperking algemene op netwerkniveau aanvallen, bieden de dezelfde beveiliging door de Microsoft online services gebruikt. De volledige omvang van wereldwijd netwerk van Azure kan worden gebruikt voor de distributie en het verkleinen van aanvallen verkeer tussen regio's. Beveiliging is beschikbaar voor IPv4 en IPv6 Azure [openbare IP-adressen](virtual-network-public-ip-address.md).
- **Standaard**: biedt mogelijkheden voor extra risicobeperking via de Basic servicelaag die specifiek op Azure Virtual Network-resources zijn afgestemd. DDoS-bescherming standaard is eenvoudig om in te schakelen, en vereist geen wijzigingen in de toepassing. Beleid voor gegevensbeveiliging zijn via specifiek verkeer bewaken en machine learning-algoritmen afgestemd. Beleidsregels worden toegepast op het openbare IP-adressen die zijn gekoppeld aan resources die zijn geïmplementeerd in virtuele netwerken, zoals Azure Load Balancer, Azure Application Gateway en Azure Service Fabric-exemplaren. Realtime telemetrie is beschikbaar via Azure Monitor weergaven tijdens een aanval en voor de geschiedenis. Toepassingsbeveiliging laag kan worden toegevoegd via de [Azure Application Gateway Web Application Firewall](../application-gateway//application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Beveiliging is beschikbaar voor IPv4 Azure [openbare IP-adressen](virtual-network-public-ip-address.md).

![Azure DDoS-bescherming-standaard](./media/ddos-protection-overview/ddospic.png)

## <a name="types-of-ddos-attacks-that-ddos-protection-standard-mitigates"></a>Typen DDoS-aanvallen die DDoS-bescherming standaard vermindert

DDoS-bescherming standaard kunt de volgende typen aanvallen verhelpen:

- **Breng aanvallen**: de aanval doel is om de netwerklaag met een aanzienlijke hoeveelheid verkeer ogenschijnlijk betrouwbare overspoelen. Het bevat UDP-overstromingen, versterkingsfactor overstromingen en andere overstromingen vervalst pakket. DDoS-bescherming standaard vermindert deze potentiële aanvallen van meerdere gigabyte door kunnen opnemen en scrubbing, schaal wereldwijd netwerk van Azure automatisch.
- **Protocol aanvallen**: deze aanvallen genereren een doel niet toegankelijk is, door een zwak punt in de laag 3 en 4 layer protocol-stack. Dit omvat, SYN overstroming aanvallen reflectie-aanvallen en andere aanvallen protocol. DDoS-bescherming standaard vermindert deze aanvallen verschillen tussen verkeer van schadelijke en legitieme, door interactie met de client en het blokkeren van schadelijk verkeer. 
- **Resource (toepassing) laag aanvallen**: deze aanvallen doel web application pakketten om de overdracht van gegevens tussen hosts te verstoren. De aanvallen omvatten HTTP-protocol schendingen, SQL injectie cross-site scripting en andere aanvallen laag 7. Gebruik de Azure [Application Gateway web application firewall](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json), met DDoS-bescherming standaard, ter bescherming tegen deze aanvallen. Er zijn ook van derden web application firewall aanbiedingen beschikbaar in de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?page=1&search=web%20application%20firewall).

DDoS-bescherming standaard beveiligt bronnen in een virtueel netwerk met een openbare IP-adressen die zijn gekoppeld aan virtuele machines, taakverdelers en toepassingsgateways. Wanneer samen met de toepassingsgateway web application firewall, kunnen DDoS-bescherming standaard bieden volledige layer 3 tot 7 risicobeperking mogelijkheid laag.

## <a name="ddos-protection-standard-features"></a>DDoS-bescherming standaardfuncties

![DDoS-functionaliteit](./media/ddos-protection-overview/ddosfeatures.png)

DDoS-bescherming standaard functies:

- **Systeemeigen platform-integratie:** systeemeigen wordt geïntegreerd in Azure. Omvat configuratie via de Azure portal. DDoS-bescherming standaard begrijpt uw resources en de resourceconfiguratie.
- **Directe beveiliging:** vereenvoudigd configuratie beveiligt onmiddellijk alle bronnen op een virtueel netwerk als DDoS-bescherming standaard is ingeschakeld. Er is geen definition interventie of de gebruiker vereist. DDoS-bescherming standaard vermindert onmiddellijk en automatisch de aanval zodra deze wordt gedetecteerd.
- **Controle van netwerkverkeer altijd op:** uw toepassing verkeerspatronen worden bewaakt van 24 uur per dag, 7 dagen per week, zoekt u indicatoren van DDoS-aanvallen. Risicobeperking wordt uitgevoerd wanneer het beleid voor gegevensbeveiliging is overschreden.
- **Adaptieve afstemmen:** Intelligent verkeer profilering leert van verkeer van uw toepassing na verloop van tijd selecteert en het profiel dat het meest geschikt is voor uw service-updates. Het profiel wordt aangepast wanneer verkeer gedurende een bepaalde periode verandert.
- **Laag 3 aan de beveiliging van laag 7:** biedt volledige stack DDoS-bescherming, wanneer gebruikt met web application firewall.
- **Uitgebreide risicobeperking scale:** gedurende 60 aanval verschillende typen kunnen worden opgevangen met globale capaciteit, om u te beschermen tegen de grootste bekende DDoS-aanvallen.
- **Aanvallen van metrische gegevens:** samengevat metrische gegevens van elke aanval zijn toegankelijk via de Azure-Monitor.
- **Aanval waarschuwingen:** waarschuwingen kunnen worden geconfigureerd op het starten en stoppen van een aanval en over de duur van de aanval met ingebouwde aanval metrische gegevens. Waarschuwingen integreren in uw operationele software, zoals Microsoft Azure Log Analytics, Splunk, Azure Storage, e-mailadres en de Azure-portal.
- **Kosten garantie:** overdracht van gegevens en toepassing scale-out servicevergoedingen voor gedocumenteerde DDoS-aanvallen.

## <a name="ddos-protection-standard-mitigation"></a>DDoS-bescherming standaard risicobeperking

DDoS-bescherming standaard gebruik van de werkelijke verkeer bewaakt en vergelijkt die voortdurend met de drempelwaarden die is gedefinieerd in het beleid DDoS. Wanneer de drempel voor het verkeer wordt overschreden, wordt automatisch DDoS risicobeperking gestart. Wanneer verkeer weer onder de drempelwaarde, wordt de vermindering verwijderd.

![Oplossing](./media/ddos-protection-overview/mitigation.png)

Tijdens de risicobeperking, verkeer naar de beveiligde bron verzonden door de service DDoS-bescherming wordt omgeleid en verschillende controles worden uitgevoerd, zoals de volgende controles:

- Zorg ervoor dat pakketten in overeenstemming zijn met internet specificaties en zijn niet onjuist gevormd.
- Interactie met de client om te bepalen of het verkeer mogelijk een vervalste pakket (bijvoorbeeld: SYN Auth of SYN Cookie of door een pakket voor de bron te verzenden waarvoor het slepen en neerzetten).
- Frequentielimiet pakketten, als er geen andere afdwingmethode kunnen worden uitgevoerd.

DDoS-bescherming aanval verkeer wordt geblokkeerd en stuurt het resterende verkeer naar de beoogde bestemming. U wordt gewaarschuwd met Azure Monitor metrische gegevens binnen een paar minuten van aanvalsdetectie. Door logboekregistratie DDoS-bescherming standaard telemetrie te configureren, kunt u de logboeken schrijven naar de beschikbare opties voor toekomstige analyse. Metrische gegevens in de Azure-Monitor voor DDoS-bescherming standaard 30 dagen bewaard.

Microsoft werkt samen met [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) voor het bouwen van een interface waarin u verkeer tegen openbare IP-adressen voor simulaties DDoS-bescherming is ingeschakeld genereren kunt. De simulatie onderbrekingspunt Cloud kunt u:

- Valideren van de manier waarop Microsoft Azure DDoS-bescherming Standard beschermt uw Azure-resources DDoS-aanvallen
- Optimaliseren van uw proces voor respons op incidenten terwijl onder DDoS-aanval
- Document DDoS naleving
- Training van uw netwerk beveiliging teams

## <a name="next-steps"></a>Volgende stappen

- [DDoS-bescherming standaard configureren](manage-ddos-protection.md)