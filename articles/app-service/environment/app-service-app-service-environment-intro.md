---
title: Inleiding tot App Service Environment v1
description: Meer informatie over de App Service Environment v1-functie die beveiligde, lid is van een VNet, toegewezen schaaleenheden biedt voor het uitvoeren van al uw apps.
services: app-service
documentationcenter: ''
author: stefsch
manager: erikre
editor: ''
ms.assetid: 78e6d4f5-da46-4eb5-a632-b5fdc17d2394
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: a2efb3177a97aa362a269ab3a00239a0d391f026
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45574688"
---
# <a name="introduction-to-app-service-environment-v1"></a>Inleiding tot App Service Environment v1

> [!NOTE]
> In dit artikel gaat over de App Service Environment v1.  Er is een nieuwere versie van de App Service Environment die gebruiksvriendelijker en wordt uitgevoerd op een krachtigere infrastructuur. Voor meer informatie over de nieuwe versie begin met het [Inleiding tot App Service Environment](intro.md).
> 

## <a name="overview"></a>Overzicht
Een App Service Environment is een [Premium] [ PremiumTier] service-plan-optie van [Azure App Service](../app-service-web-overview.md) die zorgt voor een volledig geïsoleerde en toegewezen omgeving voor het veilig uitvoeren Azure App Service-apps op grote schaal, met inbegrip van Web-Apps, Mobile Apps en API Apps.  

App Service-omgevingen zijn ideaal voor toepassingsworkloads die waarvoor nodig:

* Zeer grote schaal
* Isolatie en beveiligde netwerktoegang

Klanten kunnen meerdere App Service-omgevingen binnen één Azure-regio, evenals over meerdere Azure-regio's maken.  Hierdoor wordt de App Service-omgevingen ideaal voor het horizontaal schalen zonder status toepassingslagen ter ondersteuning van hoge RPS-werkbelastingen.

App Service-omgevingen zijn geïsoleerd actieve slechts één klant toepassingen, en worden altijd geïmplementeerd in een virtueel netwerk.  Klanten hebben meer controle over zowel toepassing binnenkomend en uitgaand netwerkverkeer en toepassingen zeer snelle, veilige verbindingen kunnen maken via virtuele netwerken tot on-premises bedrijfsresources.

Voor een overzicht van hoe App Service-omgevingen grote schaal inschakelen en beveiligde netwerktoegang, Zie de [AzureCon Deep Dive] [ AzureConDeepDive] op App Service-omgevingen.

Voor een uitgebreide voor horizontaal schalen met behulp van meerdere App Service-omgevingen raadpleegt u het artikel instellen van een [geo-distributed app footprint][GeodistributedAppFootprint].

Als u wilt zien hoe de beveiligingsarchitectuur in AzureCon Deep Dive is geconfigureerd, ziet u het artikel over het implementeren van een [gelaagde beveiligingsarchitectuur](app-service-app-service-environment-layered-security.md) met App Service-omgevingen.

Apps die worden uitgevoerd op App Service-omgevingen hebben hun toegang vergrendeld met upstream-apparaten, zoals web application Firewall (WAF).  In het artikel over [een WAF configureren voor App Service-omgevingen](app-service-app-service-environment-web-application-firewall.md) bevat informatie over dit scenario. 

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="dedicated-compute-resources"></a>Toegewezen Compute-Resources
Alle van de compute-resources in een App Service Environment zijn exclusief toegewezen aan één abonnement en een App Service Environment kan worden geconfigureerd met maximaal vijftig (50) compute-resources voor exclusief gebruik door één toepassing.

Een App Service-omgeving bestaat uit een front-compute-resourcegroep, evenals één tot drie worker compute-resourcegroepen. 

De front-end-pool bevat rekenresources die verantwoordelijk is voor SSL-beëindiging als ook automatische taakverdeling van app-aanvragen in een App Service Environment. 

Elke groep met werkrollen bevat rekenresources die zijn toegewezen aan [App Service-abonnementen][AppServicePlan], die een of meer Azure App Service-apps op zijn beurt bevatten.  Omdat er maximaal drie verschillende werknemersgroepen aanwezig in een App Service Environment zijn kunnen, hebt u de flexibiliteit om verschillende compute-resources voor elke workergroep kiezen.  

Bijvoorbeeld: Hiermee kunt u een groep met werkrollen maken met minder krachtige compute-resources voor App Service-abonnementen bedoeld voor ontwikkelings- of apps.  Een tweede (of zelfs derde) werknemersgroep kan krachtigere rekenresources die zijn bedoeld voor App Service-abonnementen met productie-apps gebruiken.

Zie voor meer informatie over de hoeveelheid rekenresources die beschikbaar zijn voor de front-end- en worker-groepen [het configureren van een App Service Environment][HowToConfigureanAppServiceEnvironment].  

Voor meer informatie over de beschikbare compute resource grootten die worden ondersteund in een App Service Environment, moet u de [prijzen voor App Service] [ AppServicePricing] pagina en bekijk de beschikbare opties voor App Service-omgevingen in de De prijscategorie Premium.

## <a name="virtual-network-support"></a>Virtual Network-ondersteuning
Een App Service Environment kan worden gemaakt in **beide** een virtueel netwerk van Azure Resource Manager, **of** een klassieke implementatie model virtueel netwerk ([meer informatie over virtuele netwerken] [MoreInfoOnVirtualNetworks]).  Aangezien een App Service-omgeving altijd in een virtueel netwerk bestaat en nauwkeuriger binnen een subnet van een virtueel netwerk, kunt u gebruikmaken van de beveiligingsfuncties van virtuele netwerken voor het beheren van zowel binnenkomende en uitgaande netwerkcommunicatie.  

Een App Service-omgeving kan een van beide internetgerichte met een openbaar IP-adres of interne geconfronteerd met alleen een Azure interne Load Balancer (ILB)-adres zijn.

U kunt [netwerkbeveiligingsgroepen] [ NetworkSecurityGroups] om te beperken van binnenkomende netwerkcommunicatie met het subnet waarin een App Service-omgeving zich bevindt.  Hiermee kunt u apps achter upstream-apparaten en services, zoals firewalls voor webtoepassingen en SaaS-netwerkproviders uitvoeren.

Apps hebben ook vaak toegang nodig tot bedrijfsresources zoals interne databases en webservices.  Een algemene aanpak is om alleen voor interne netwerkverkeer stromen binnen een virtueel Azure-netwerk beschikbaar maken voor deze eindpunten.  Zodra een App Service Environment is toegevoegd aan hetzelfde virtuele netwerk bevinden als de interne services, apps die worden uitgevoerd in de omgeving toegang krijgen tot deze, met inbegrip van eindpunten bereikbaar is via [Site-naar-Site] [ SiteToSite] en [Azure ExpressRoute] [ ExpressRoute] verbindingen.

Voor meer informatie over de werking van App Service-omgevingen met virtuele netwerken en on-premises netwerken raadpleegt u de volgende artikelen op [netwerkarchitectuur][NetworkArchitectureOverview], [inkomende beheren Verkeer][ControllingInboundTraffic], en [veilig maken van verbinding met back-ends][SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Aan de slag
Als u wilt aan de slag met App Service-omgevingen, Zie [hoe om te maken van een App Service-omgeving][HowToCreateAnAppServiceEnvironment]

Zie voor een overzicht van de netwerkarchitectuur van App Service-omgeving, de [overzicht van netwerkarchitectuur] [ NetworkArchitectureOverview] artikel.

Zie voor meer informatie over het gebruik van een App Service Environment met ExpressRoute, het volgende artikel op [Express Route- en App Service-omgevingen][NetworkConfigDetailsForExpressRoute].

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
[SiteToSite]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-multi-site
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  app-service-web-configure-an-app-service-environment.md
[ControllingInboundTraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[SecurelyConnectingToBackends]:  app-service-app-service-environment-securely-connecting-to-backend-resources.md
[NetworkArchitectureOverview]:  app-service-app-service-environment-network-architecture-overview.md
[NetworkConfigDetailsForExpressRoute]:  app-service-app-service-environment-network-configuration-expressroute.md
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->


