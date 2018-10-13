---
title: Overzicht van Azure DDoS Protection Standard | Microsoft Docs
description: Meer informatie over de service Azure DDoS Protection.
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
ms.date: 10/13/2018
ms.author: jdial
ms.openlocfilehash: ae1a77921cdb3eba003ff6dc8b4885866f2fbe5e
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2018
ms.locfileid: "49310350"
---
# <a name="azure-ddos-protection-standard-overview"></a>Overzicht van Azure DDoS Protection Standard

Distributed denial of service (DDoS)-aanvallen zijn enkele van de grootste problemen beschikbaarheid en beveiliging naar klanten die hun toepassingen naar de cloud wilt verplaatsen. Een DDoS-aanval wil de bronnen van een toepassing, waardoor de toepassing niet beschikbaar voor legitieme gebruikers. DDoS-aanvallen kunnen worden gericht op een willekeurig eindpunt dat openbaar bereikbaar is via het internet.

Azure DDoS protection, gecombineerd met aanbevolen procedures voor de toepassing-ontwerp, bieden beveiliging tegen DDoS-aanvallen. Azure DDoS protection biedt de volgende Servicelagen:

- **Basic**: automatisch ingeschakeld als onderdeel van het Azure-platform. Verkeer altijd controleren en realtime oplossing voor veelvoorkomende aanvallen op netwerkniveau, bieden de dezelfde beveiliging die door Microsoft online services wordt gebruikt. De volledige omvang van het wereldwijde netwerk van Azure kan worden gebruikt om te distribueren en te verhelpen aanval verkeer tussen regio's. Beveiliging is beschikbaar voor IPv4 en IPv6-Azure [openbare IP-adressen](virtual-network-public-ip-address.md).
- **Standard**: biedt u aanvullende risicobeperkende mogelijkheden ten opzichte van de basis-servicelaag die geschikt zijn specifiek voor Azure Virtual Network-resources. DDoS Protection Standard is eenvoudig om in te schakelen, en vereist geen wijzigingen in de toepassing. Beveiligingsbeleid voor apps zijn afgestemd op door middel van controle van toegewezen netwerkverkeer en machine learning-algoritmen. Beleid wordt toegepast op het openbare IP-adressen die zijn gekoppeld aan resources die zijn geïmplementeerd in virtuele netwerken, zoals Azure Load Balancer, Azure Application Gateway en Azure Service Fabric-exemplaren, maar deze beveiliging is niet van toepassing op App Service-omgevingen. Realtime telemetrie is beschikbaar via Azure Monitor weergaven tijdens een aanval en voor de geschiedenis. Uitgebreide aanval risicobeperking analytics zijn beschikbaar via diagnostische instellingen. Beveiliging van de toepassingslaag kan worden toegevoegd via de [Azure Application Gateway Web Application Firewall](../application-gateway//application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of door een 3e partij firewall installeren vanuit Azure Marketplace. Beveiliging is van toepassing op IPv4 Azure [openbare IP-adressen](virtual-network-public-ip-address.md).

![Azure DDoS Protection Basic vs. Standard](./media/ddos-protection-overview/ddoscomparison.png)

## <a name="types-of-ddos-attacks-that-ddos-protection-standard-mitigates"></a>Typen DDoS-aanvallen die DDoS Protection standaard vermindert

DDoS Protection standaard kan het risico van de volgende typen aanvallen:

- **Breng aanvallen**: van de aanval doel is om de netwerklaag met een aanzienlijke hoeveelheid schijnbaar legitieme verkeer overspoelen. Het bevat UDP-overstromingen, versterking overstromingen en andere overstromingen vervalst pakket. DDoS Protection standaard vermindert deze potentiële meerdere gigabyte aanvallen door opvangen en scrubbing, schaal van de wereldwijde netwerk van Azure, automatisch.
- **Protocol aanvallen**: deze aanvallen weer een doel niet toegankelijk is, door een zwak punt in de laag-3 en laag 4-protocolstack. Hiertoe behoren, SYN overstroming aanvallen, reflectie-aanvallen en andere aanvallen protocol. DDoS Protection standaard vermindert deze aanvallen verschillen tussen schadelijke en legitieme, door interactie met de client en het blokkeren van schadelijk verkeer. 
- **Resource (toepassing) aanvallen op toepassingslagen**: deze aanvallen doel web application pakketten, als u wilt de overdracht van gegevens tussen hosts te verstoren. De aanvallen zijn HTTP-protocolschendingen, SQL-injectie, cross-site scripting en andere aanvallen laag 7. Gebruik de Azure [Application Gateway web application firewall](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json), met DDoS Protection standaard, voor beveiliging tegen deze aanvallen. Er zijn ook van derden web application firewall aanbiedingen beschikbaar in de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?page=1&search=web%20application%20firewall).

DDoS Protection standaard beveiligt bronnen in een virtueel netwerk met inbegrip van openbare IP-adressen die zijn gekoppeld aan virtuele machines, load balancers en toepassingsgateways. Wanneer in combinatie met de Application Gateway web application firewall, krijgt DDoS Protection Standard u een volledige laag-3 aan laag 7 risicobeperking mogelijkheid.

## <a name="ddos-protection-standard-features"></a>DDoS Protection Standard-functies

![DDoS-functionaliteit](./media/ddos-protection-overview/ddosfeatures.png)

DDoS Protection Standard-functies zijn onder andere:

- **Systeemeigen platform-integratie:** systeemeigen geïntegreerd in Azure. Bevat configuratie via de Azure-portal. DDoS Protection standaard begrijpt dat uw resources en resourceconfiguratie.
- **Ingebouwde beveiliging:** vereenvoudigde configuratie beschermt meteen alle resources in een virtueel netwerk zodra DDoS Protection standaard is ingeschakeld. Er is geen tussenkomst of gebruiker definitie is vereist. DDoS Protection standaard vermindert meteen en automatisch de aanval, zodra deze wordt gedetecteerd.
- **Verkeer altijd controleren:** verkeerspatronen van uw toepassingen worden bewaakt van 24 uur per dag, 7 dagen per week, op zoek naar indicatoren van DDoS-aanvallen. Risicobeperking wordt uitgevoerd wanneer het beveiligingsbeleid voor apps worden overschreden.
- **Adaptieve afstemming:** intelligente verkeer profilering leert van verkeer van uw toepassing gedurende een periode, en selecteert en het profiel dat het meest geschikt is voor uw service-updates. Het profiel wordt aangepast wanneer verkeer na verloop van tijd verandert.
- **Meerlaagse beveiliging:** biedt volledige stack DDoS protection, gebruikt in combinatie met een web application firewall.
- **Uitgebreide risicobeperking schaal:** gedurende 60 verschillende aanval typen kunnen worden verholpen met globale capaciteit, om te beveiligen tegen het grootste bekende DDoS-aanvallen.
- **Aanvallen analytics:** gedetailleerde rapporten ophalen in vijf minuten durende stappen tijdens een aanval en een volledig overzicht na het einde van de aanval. Stream risicobeperking stroom vastgelegd in een offline security information en event management (SIEM)-systeem voor near realtime controle tijdens een aanval.
- **Metrische gegevens over aanvallen:** samengevat metrische gegevens van elke aanval, toegankelijk zijn via Azure Monitor.
- **Waarschuwingen van de aanval:** waarschuwingen kunnen worden geconfigureerd op het starten en stoppen van een aanval en over de duur van de aanval, met ingebouwde aanval metrische gegevens. Waarschuwingen integreren in uw operationele software, zoals Microsoft Azure Log Analytics, Splunk, Azure Storage, e-mailadres en de Azure-portal.
- **Kosten garantie:** overdracht van gegevens en toepassingen scale-out servicetegoeden voor gedocumenteerde DDoS-aanvallen.

## <a name="ddos-protection-standard-mitigation"></a>DDoS Protection standaard risicobeperking

DDoS Protection standaard gebruik van de werkelijke hoeveelheid verkeer bewaakt en vergelijkt die voortdurend met de drempelwaarden gedefinieerd in het DDoS-beleid op te geven. Wanneer de drempelwaarde voor foutverkeer wordt overschreden, wordt automatisch DDoS-risicobeperking gestart. Wanneer het verkeer wordt lager dan de drempelwaarde, wordt de oplossing wordt verwijderd.

![Oplossing](./media/ddos-protection-overview/mitigation.png)

Verkeer dat wordt verzonden naar de beveiligde resource wordt doorgestuurd door de DDoS protection-service tijdens risicobeperking, en verschillende controles worden uitgevoerd, zoals de volgende controles:

- Zorg ervoor dat pakketten voldoen volgens de specificaties van internet en niet zijn onjuist gevormd.
- Interactie met de client om te bepalen of het verkeer mogelijk een vervalste-pakket (bijvoorbeeld: SYN Auth of SYN Cookie of door een pakket voor de bron te vragen deze te slepen en neerzetten).
- Frequentielimiet pakketten, als er geen andere methode voor het afdwingen kunnen worden uitgevoerd.

DDoS protection-aanval verkeer wordt geblokkeerd en verzendt het resterende verkeer naar de gewenste bestemming. Binnen een paar minuten van de aanvalsdetectie van de, u krijgt een melding met Azure Monitor metrische gegevens. Door het configureren van DDoS Protection standaardtelemetrie voor aanmelden, kunt u de logboeken kunt schrijven naar de beschikbare opties voor toekomstig gebruik. Metrische gegevens in Azure Monitor voor DDoS Protection Standard is gedurende 30 dagen bewaard.

Microsoft is een partnerschap aangegaan met [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) aan het bouwen van een interface waarin u verkeer op basis van openbare IP-adressen voor simulaties DDoS Protection is ingeschakeld kunt genereren. De simulatie onderbrekingspunt Cloud kunt u:

- Valideren van de manier waarop Microsoft Azure DDoS Protection Standard beschermt uw Azure-resources van DDoS-aanvallen
- Optimaliseer het incidentreactieproces terwijl onder DDoS-aanval
- Document DDoS-naleving
- Uw netwerk beveiligingsteams trainen

## <a name="next-steps"></a>Volgende stappen

- [DDoS-bescherming standaard configureren](manage-ddos-protection.md)
