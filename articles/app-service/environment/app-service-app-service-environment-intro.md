---
title: Inleiding tot de App Service-omgeving v1
description: Meer informatie over de App Service-omgeving v1-functie met beveiligde, die lid zijn van VNet, toegewezen schaaleenheden voor het uitvoeren van uw apps.
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 78e6d4f5-da46-4eb5-a632-b5fdc17d2394
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: b13f726ada6deba67d5ed484eca769fed354e400
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-app-service-environment-v1"></a>Inleiding tot de App Service-omgeving v1

> [!NOTE]
> In dit artikel gaat over de v1 App Service-omgeving.  Er is een nieuwere versie van de App Service-omgeving die eenvoudiger te gebruiken en wordt uitgevoerd op krachtiger infrastructuur. Voor meer informatie over de nieuwe versie begin met het [Inleiding tot de App-serviceomgeving](intro.md).
> 

## <a name="overview"></a>Overzicht
Een App Service-omgeving is een [Premium] [ PremiumTier] service-plan optie [Azure App Service](../app-service-web-overview.md) die zorgt voor een volledig geïsoleerde en toegewezen omgeving voor het veilig uitvoeren Azure App Service-apps op grote schaal, waaronder Web-Apps, Mobile Apps en API-Apps.  

App Service-omgevingen zijn ideaal voor toepassingsworkloads vereisen:

* Zeer grote schaal
* Isolatie en beveiligde netwerktoegang

Klanten kunnen meerdere App Service-omgevingen binnen één Azure-regio, evenals over meerdere Azure-regio's maken.  Dit maakt App Service-omgevingen ideaal voor horizontaal schalen status minder toepassingslagen ter ondersteuning van werklasten met hoge RPS.

App Service-omgevingen zijn geïsoleerd, zodat slechts één klant toepassingen uitvoeren en altijd in een virtueel netwerk worden geïmplementeerd.  Klanten hebben fijnmazig controle over beide-toepassing voor binnenkomend en uitgaand netwerkverkeer en toepassingen kunnen verbinding maken snelle beveiligde verbindingen via virtuele netwerken met lokale bedrijfsbronnen.

Voor een overzicht van hoe App Service-omgevingen grote schaal inschakelen en beveiligde netwerktoegang, Zie de [AzureCon diepgaand] [ AzureConDeepDive] op App Service-omgevingen.

Voor een dieper in op horizontaal schalen met meerdere App Service-omgevingen Zie het artikel over de installatie een [geografisch verspreide app footprint][GeodistributedAppFootprint].

Hoe de beveiligingsarchitectuur weergegeven in de diepgaand AzureCon is geconfigureerd, Zie het artikel over het implementeren van een [beveiligingsarchitectuur gelaagde](app-service-app-service-environment-layered-security.md) met App Service-omgevingen.

Apps die worden uitgevoerd op App Service-omgevingen kunnen de geregeld door de upstream-apparaten, zoals web application Firewall (WAF) toegang hebben.  Het artikel over [een WAF configureren voor App Service-omgevingen](app-service-app-service-environment-web-application-firewall.md) bevat informatie over dit scenario. 

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="dedicated-compute-resources"></a>Speciale berekenings-Resources
Alle resources in een App-serviceomgeving compute zijn toegewezen aan één abonnement en een App Service-omgeving kan worden geconfigureerd met maximaal vijftig (50) rekenresources voor exclusief gebruik door één toepassing.

Een App Service-omgeving bestaat uit een front-compute-resourcegroep, evenals één tot drie worker compute resourcegroepen. 

De front-groep bevat de rekenresources die verantwoordelijk is voor SSL-beëindiging als ook automatische taakverdeling van app-aanvragen in een App Service-omgeving. 

Elke worker-groep bevat rekenresources die zijn toegewezen aan [App Service-abonnementen][AppServicePlan], die een of meer Azure App Service-apps op hun beurt bevatten.  Omdat er kunnen maximaal drie verschillende werknemersgroepen in een App Service-omgeving, kunt u hebt de flexibiliteit om te kiezen verschillende rekenresources voor elke worker-groep.  

Bijvoorbeeld: Hiermee kunt u één worker-groep maken met minder krachtige rekenresources voor App Service-abonnementen die bestemd zijn voor ontwikkeling of tests apps.  Een tweede (of zelfs derde) werknemersgroep kan krachtiger rekenresources die bestemd zijn voor het App Service-abonnementen met productie-apps gebruiken.

Zie voor meer informatie over de hoeveelheid beschikbaar is op de front- en werkrollen opslaggroepen rekenresources [het configureren van een App-serviceomgeving][HowToConfigureanAppServiceEnvironment].  

Voor meer informatie over de beschikbare compute resource grootten die worden ondersteund in een App Service-omgeving, Raadpleeg de [prijzen van App Service] [ AppServicePricing] pagina en bekijk de beschikbare opties voor App Service-omgevingen in de prijscategorie Premium.

## <a name="virtual-network-support"></a>Virtual Network-ondersteuning
Een App Service-omgeving kunnen worden gemaakt in **beide** een virtueel netwerk van Azure Resource Manager, **of** een virtueel netwerk voor klassieke implementatie-model ([meer informatie over virtuele netwerken][MoreInfoOnVirtualNetworks]).  Omdat een App Service-omgeving altijd in een virtueel netwerk en nauwkeuriger binnen een subnet van een virtueel netwerk bestaat, kunt u gebruikmaken van de beveiligingsfuncties van virtuele netwerken om te beheren, zowel binnenkomend en uitgaand netwerkcommunicatie.  

Een App Service-omgeving kan worden beide Internetgericht met een openbaar IP-adres of interne geconfronteerd met alleen een adres Azure interne Load Balancer (ILB).

U kunt [netwerkbeveiligingsgroepen] [ NetworkSecurityGroups] te beperken van binnenkomende netwerkcommunicatie met het subnet waarin een App Service-omgeving zich bevindt.  Hiermee kunt u apps achter upstream apparaten en services, zoals web application firewalls en aanbieders van SaaS-netwerk uitvoeren.

Apps moeten ook vaak toegang tot bedrijfsbronnen zoals interne databases en -services.  Vaak wordt deze eindpunten om beschikbaar te maken alleen voor interne netwerkverkeer dat binnen een virtuele Azure-netwerk.  Zodra een App-serviceomgeving is toegevoegd aan hetzelfde virtuele netwerk als de interne services, apps die worden uitgevoerd in de omgeving ze toegankelijk zijn, inclusief eindpunten bereikbaar is via [Site-naar-Site] [ SiteToSite] en [Azure ExpressRoute] [ ExpressRoute] verbindingen.

Voor meer informatie over de werking van App Service-omgevingen met virtuele netwerken en on-premises netwerken raadpleegt u de volgende artikelen op [netwerkarchitectuur][NetworkArchitectureOverview], [binnenkomend verkeer beheren][ControllingInboundTraffic], en [veilig verbinding te maken met back-ends][SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Aan de slag
Om aan de slag met App Service-omgevingen, Zie [hoe te maken van een App Service-omgeving][HowToCreateAnAppServiceEnvironment]

Zie voor een overzicht van de App Service-omgeving netwerkarchitectuur de [netwerk architectuuroverzicht] [ NetworkArchitectureOverview] artikel.

Zie voor meer informatie over het gebruik van een App Service-omgeving met ExpressRoute, het volgende artikel op [Express Route- en App Service-omgevingen][NetworkConfigDetailsForExpressRoute].

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: ../azure-web-sites-web-hosting-plans-in-depth-overview.md
[HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  app-service-app-service-environment-geo-distributed-scale.md
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  app-service-web-configure-an-app-service-environment.md
[ControllingInboundTraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[SecurelyConnectingToBackends]:  app-service-app-service-environment-securely-connecting-to-backend-resources.md
[NetworkArchitectureOverview]:  app-service-app-service-environment-network-architecture-overview.md
[NetworkConfigDetailsForExpressRoute]:  app-service-app-service-environment-network-configuration-expressroute.md
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->


