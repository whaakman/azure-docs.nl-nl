---
title: Inleiding tot Azure App Service-omgevingen
description: Beknopt overzicht van Azure App Service-omgevingen
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 3c7eaefa-1850-4643-8540-428e8982b7cb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 322cf2ebbe83d00fcebcec618e07141d26f4f255
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-app-service-environments"></a>Inleiding tot de App Service-omgevingen #
 
## <a name="overview"></a>Overzicht ##

Azure App Service-omgeving is een Azure App Service-functie een volledig geïsoleerde en toegewezen omgeving biedt voor het uitvoeren van App Service-apps veilig op grote schaal. Deze mogelijkheid kan host uw web-apps, [mobiele apps][mobileapps], API-apps en [functies][Functions].

App Service-omgevingen (ASEs) zijn geschikt is voor de toepassing werklast die nodig:

- Zeer grote schaal.
- Isolatie en beveiligde netwerktoegang.
- Hoog geheugengebruik.

Klanten kunnen meerdere ASEs binnen één Azure-regio of op meerdere Azure-regio's maken. Deze flexibiliteit maakt ASEs ideaal voor stateless toepassingslagen ter ondersteuning van werklasten met hoge RPS horizontaal schalen.

ASEs zijn geïsoleerd, zodat slechts één klant toepassingen uitvoeren en altijd in een virtueel netwerk worden geïmplementeerd. Klanten hebben fijnmazig controle over de toepassing voor binnenkomend en uitgaand netwerkverkeer. Toepassingen kunnen snelle beveiligde verbindingen via VPN-verbindingen maken met lokale bedrijfsbronnen.

* ASEs inschakelen hoge schaalbaarheid app hosten met beveiligde netwerktoegang. Zie voor meer informatie de [AzureCon diepgaand](https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/) op ASEs.
* Meerdere ASEs kunnen worden gebruikt om horizontaal schalen. Zie voor meer informatie [het instellen van een app geografisch verspreide footprint](app-service-app-service-environment-geo-distributed-scale.md).
* ASEs kan worden gebruikt voor het configureren van de beveiligingsarchitectuur, zoals wordt weergegeven in de diepgaand AzureCon. Zie hoe de beveiligingsarchitectuur weergegeven in de diepgaand AzureCon is geconfigureerd, de [artikel over het implementeren van een gelaagd beveiligingsarchitectuur](app-service-app-service-environment-layered-security.md) met App Service-omgevingen.
* Apps die worden uitgevoerd op ASEs kunnen de geregeld door de upstream-apparaten, zoals web application firewalls (WAFs) toegang hebben. Zie voor meer informatie [configureren van een WAF voor App Service-omgevingen](app-service-app-service-environment-web-application-firewall.md).

## <a name="dedicated-environment"></a>Specifieke omgeving ##

Een as-omgeving is toegewezen aan één abonnement en 100 exemplaren kan hosten. Het bereik kan 100 exemplaren in een enkele App Service-abonnement 100 single instance App Service-abonnementen en alles in tussen omspannen.

Een as-omgeving bestaat uit een front-ends en werknemers. Front-ends zijn verantwoordelijk voor het HTTP/HTTPS-beëindiging en automatische taakverdeling van app-aanvragen in een as-omgeving. Front-ends worden automatisch toegevoegd als de App Service-abonnementen in de as-omgeving zijn uitgebreid.

Werknemers zijn functies die klant-apps host. Werknemers zijn beschikbaar in drie vaste grootten:

* Een core/3.5 GB RAM-geheugen
* Twee core/7 GB RAM-geheugen
* Vier core/14 GB RAM-geheugen

Klanten hoeven niet voor het beheren van front-ends en werknemers. Alle infrastructuur wordt automatisch toegevoegd als klanten scale-out hun App Service-abonnementen. Als de App Service-abonnementen zijn gemaakt of geschaald in een as-omgeving, wordt de vereiste infrastructuur toegevoegd of verwijderd afhankelijk van wat geschikt.

Er is een plat maandelijks percentage voor een as-omgeving die betaalt voor de infrastructuur en verandert niet met de grootte van de as-omgeving. Er is bovendien een kosten per core van App Service-plan. Alle apps die worden gehost in een as-omgeving zijn in de SKU-prijzen geïsoleerd. Zie voor informatie over prijzen voor een as-omgeving, de [App Service-prijzen] [ Pricing] pagina en bekijk de beschikbare opties voor ASEs.

## <a name="virtual-network-support"></a>Virtual network-ondersteuning ##

Een as-omgeving kan alleen in een virtueel netwerk van Azure Resource Manager worden gemaakt. Zie voor meer informatie over virtuele netwerken in Azure, de [Azure virtuele netwerken Veelgestelde vragen over](https://azure.microsoft.com/documentation/articles/virtual-networks-faq/). Een as-omgeving bestaat altijd in een virtueel netwerk en nauwkeuriger, binnen een subnet van een virtueel netwerk. De beveiligingsfuncties van virtuele netwerken kunt u binnenkomend en uitgaand netwerkcommunicatie voor uw apps te beheren.

Een as-omgeving kan worden internetgerichte met een openbaar IP-adres of interne bedrijfstoepassingen met alleen een adres voor Azure interne load balancer (ILB).

[Netwerkbeveiligingsgroepen] [ NSGs] beperken van binnenkomende netwerkcommunicatie met het subnet waarin een as-omgeving zich bevindt. Nsg's kunt u apps achter upstream apparaten en services, zoals WAFs en aanbieders van SaaS-netwerk uitvoeren.

Apps moeten ook vaak toegang tot bedrijfsbronnen zoals interne databases en -services. Als u het as-omgeving in een virtueel netwerk met een VPN-verbinding met de on-premises netwerk implementeert, kunnen de lokale bronnen toegang tot de apps in de as-omgeving. Deze mogelijkheid geldt ongeacht of de VPN-verbinding is een [site-naar-site](https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/) of [Azure ExpressRoute](http://azure.microsoft.com/services/expressroute/) VPN.

Zie voor meer informatie over de werking van ASEs met virtuele netwerken en on-premises netwerken [overwegingen met betrekking tot het netwerk van het App Service-omgeving][ASENetwork].

## <a name="app-service-environment-v1"></a>App Service-omgeving v1 ##

App Service-omgeving zijn er twee versies: ASEv1 en ASEv2. De voorgaande informatie is gebaseerd op ASEv2. Deze sectie leest u de verschillen tussen ASEv1 en ASEv2. 

In ASEv1 moet u alle bronnen handmatig te beheren. Dat betekent onder meer de front-ends, werknemers en IP-adressen gebruikt voor SSL op basis van IP. Voordat u uw App Service-abonnement uitbreiden kunt, moet u eerst scale-out de worker-groep waar u deze hosten.

ASEv1 maakt gebruik van een andere prijscategorie model uit ASEv2. In ASEv1 betaalt u voor elke core toegewezen. Dat betekent onder meer kernen gebruikt voor de front-ends of workers die werkbelastingen worden niet als host. Grootte van de maximale schaal van een as-omgeving is in ASEv1, 55 totale hosts. Die bevat werknemers en -front-ends. Een voordeel ASEv1 is dat deze kan worden geïmplementeerd in een klassiek virtueel netwerk en een virtueel netwerk van Resource Manager. Zie voor meer informatie over ASEv1, [App Service-omgeving v1 inleiding][ASEv1Intro].

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[webapps]: ../app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
