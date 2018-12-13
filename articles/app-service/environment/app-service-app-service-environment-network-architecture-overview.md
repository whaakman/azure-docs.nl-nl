---
title: Overzicht van netwerkarchitectuur van App Service-omgevingen - Azure
description: Overzicht van de architectuur van netwerk-topologie ofApp Service-omgevingen.
services: app-service
documentationcenter: ''
author: stefsch
manager: erikre
editor: ''
ms.assetid: 13d03a37-1fe2-4e3e-9d57-46dfb330ba52
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: 0d7d4af46e54ad89e0d084cb15af13e56115e996
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53274160"
---
# <a name="network-architecture-overview-of-app-service-environments"></a>Overzicht van de netwerkarchitectuur van App Service-omgevingen
## <a name="introduction"></a>Inleiding
App Service-omgevingen worden altijd gemaakt binnen een subnet van een [virtueel netwerk] [ virtualnetwork] -apps die worden uitgevoerd in een App Service Environment kunnen communiceren met persoonlijke eindpunten die zich in dezelfde virtuele netwerktopologie.  Omdat klanten delen van hun virtuele netwerkinfrastructuur vergrendelen kunnen, is het belangrijk om te begrijpen welke typen netwerk communicatie stromen die worden uitgevoerd in een App Service Environment.

## <a name="general-network-flow"></a>Algemene netwerkstroom
Wanneer een App Service Environment (ASE) maakt gebruik van een openbare virtuele IP-adres (VIP) voor apps, wordt al het binnenkomende verkeer binnenkomt op dat openbaar VIP-adres.  Dit omvat HTTP en HTTPS-verkeer voor apps, evenals andere verkeer voor FTP-functionaliteit voor externe foutopsporing en Azure management-bewerkingen.  Zie het artikel op voor een volledige lijst van de specifieke poorten (vereiste en optionele) die beschikbaar op het openbare VIP zijn [inkomend verkeer beheren] [ controllinginboundtraffic] aan een App Service Environment. 

App Service-omgevingen bieden ook ondersteuning voor actieve apps die afhankelijk zijn van alleen de interne adres van een virtueel netwerk, ook wel een ILB-adres (interne load balancer) genoemd.  Op een ILB ingeschakeld as-omgeving, HTTP en HTTPS-verkeer voor apps, evenals externe foutopsporing oproepen, op het ILB-adres binnenkomen.  FTP-/ FTPS-verkeer wordt voor de meest voorkomende configuraties van de ILB-as-omgeving, ook op het ILB-adres binnenkomen.  Echter ingeschakeld Azure management-bewerkingen nog steeds worden overgebracht naar poorten 454/455 op het openbare VIP-adres van een ILB as-omgeving.

Het volgende diagram toont een overzicht van de verschillende inkomende en uitgaande stromen voor een App Service-omgeving waar de apps die zijn gebonden aan een openbare virtuele IP-adres:

![Algemene netwerk stromen][GeneralNetworkFlows]

Een App Service Environment kan communiceren met een verscheidenheid aan persoonlijke klant-eindpunten.  Apps die worden uitgevoerd in de App Service Environment kunnen bijvoorbeeld verbinden met database (s) die worden uitgevoerd op IaaS virtuele machines in dezelfde topologie van het virtuele netwerk.

> [!IMPORTANT]
> De netwerkdiagram bekijkt, de 'andere Compute-Resources' worden geïmplementeerd in een ander Subnet van de App Service Environment. Implementeren van resources in hetzelfde Subnet met de as-omgeving, wordt de verbinding van de as-omgeving met deze resources (met uitzondering van specifieke intra-as-omgeving routering) geblokkeerd. Implementeren naar een ander Subnet, in plaats daarvan (in hetzelfde VNET). De App Service-omgeving wordt vervolgens mogelijk zijn om verbinding te maken. Er is geen aanvullende configuratie nodig.
> 
> 

App Service-omgevingen ook worden gecommuniceerd met de Sql-database en Azure Storage resources die nodig zijn voor het beheren en uitvoeren van een App Service Environment.  Enkele van de Sql- en opslagresources die een App Service Environment met communiceert bevinden zich in dezelfde regio als de App Service-omgeving, terwijl anderen bevinden zich in de externe Azure-regio's.  Als gevolg hiervan is uitgaande verbinding met Internet altijd vereist voor een App Service Environment te laten functioneren. 

Omdat een App Service Environment wordt geïmplementeerd in een subnet, kunnen de netwerkbeveiligingsgroepen worden gebruikt voor het beheren van inkomend verkeer op het subnet.  Zie voor meer informatie over het beheren van inkomend verkeer naar een App Service-omgeving, de volgende [artikel][controllinginboundtraffic].

Zie voor meer informatie over het toestaan van uitgaande internetverbinding vanuit een App Service-omgeving, het volgende artikel over het werken met [Express Route][ExpressRoute].  De dezelfde benadering beschreven in het artikel is van toepassing als u werkt met Site-naar-Site-connectiviteit en met behulp van geforceerde tunneling.

## <a name="outbound-network-addresses"></a>Uitgaande netwerkadressen
Wanneer een App Service Environment uitgaande oproepen, maakt is een IP-adres altijd gekoppeld aan de uitgaande aanroepen.  De specifieke IP-adres dat wordt gebruikt, is afhankelijk van of het eindpunt dat wordt aangeroepen zich in de topologie van het virtuele netwerk of buiten de topologie van het virtuele netwerk bevindt.

Als het eindpunt dat wordt aangeroepen **buiten** van de virtuele netwerktopologie, is het uitgaande adres (ook wel de uitgaande NAT-adres) dat wordt gebruikt vervolgens het openbare VIP-adres van de App Service-omgeving.  Dit adres kan worden gevonden in de portal-gebruikersinterface voor de App Service-omgeving in de blade met Apparaateigenschappen.

![Uitgaande IP-adres][OutboundIPAddress]

Dit adres kan ook worden bepaald voor as-omgevingen die alleen beschikken over een openbare VIP-adres met het maken van een app in de App Service-omgeving en vervolgens het uitvoeren van een *nslookup* op adres van de app. Het resulterende IP-adres is zowel de openbare VIP, evenals de uitgaande NAT-adres van de App Service-omgeving.

Als het eindpunt dat wordt aangeroepen **binnen** van de virtuele netwerktopologie, is het uitgaande adres van de aanroepende app het interne IP-adres van de afzonderlijke compute-resource met de app.  Er is echter geen permanente toewijzing van virtueel netwerk interne IP-adressen aan apps.  Apps kunnen verplaatsen tussen verschillende compute-resources en de groep met beschikbare compute-resources in een App Service-omgeving vanwege het schalen herverdelen kunnen wijzigen.

Echter, omdat een App Service Environment altijd in een subnet bevinden is, wordt gegarandeerd dat het interne IP-adres van een compute-resource met een app altijd wordt liggen binnen het CIDR-bereik van het subnet.  Als gevolg hiervan als uiterst gedetailleerde ACL's of netwerkbeveiligingsgroepen worden gebruikt voor beveiligde toegang tot andere eindpunten in het virtuele netwerk, moet het subnetbereik met de App Service-omgeving toegang worden verleend.

Het volgende diagram toont deze concepten in meer detail:

![Uitgaande netwerkadressen][OutboundNetworkAddresses]

In het bovenstaande diagram:

* Omdat het openbare VIP-adres van de App Service Environment 192.23.1.2 is, is dat het uitgaande IP-adres dat is gebruikt bij het aanroepen naar 'Internet' eindpunten.
* De CIDR-bereik van het subnet dat voor de App Service Environment is 10.0.1.0/26.  Andere eindpunten in dezelfde virtuele-netwerkinfrastructuur ziet aanroepen vanuit apps als die afkomstig zijn van ergens binnen dit adresbereik.

## <a name="calls-between-app-service-environments"></a>Aanroepen tussen App Service-omgevingen
Een complexere scenario kan gebeuren als u meerdere App Service-omgevingen in hetzelfde virtuele netwerk te implementeren en uitgaande vanuit een App Service-omgeving naar een andere App Service-omgeving aanroepen.  Deze typen cross-aanroepen wordt ook beschouwd als 'Internet' aanroepen van App Service Environment.

Het volgende diagram toont een voorbeeld van een gelaagde architectuur met apps op één App Service Environment (bijvoorbeeld) 'Voordeur' web-apps) aanroepen van apps op een tweede App Service Environment (bijvoorbeeld interne back-end API-apps niet moeten zijn vanop het Internet). 

![Aanroepen tussen App Service-omgevingen][CallsBetweenAppServiceEnvironments] 

In het bovenstaande voorbeeld heeft de App Service Environment 'As-omgeving een' een uitgaande IP-adres van 192.23.1.2.  Als een app die wordt uitgevoerd op deze wordt App Service-omgeving maakt een uitgaande aanroep van een app die wordt uitgevoerd op een tweede App Service-omgeving ("as-omgeving twee") zich in hetzelfde virtuele netwerk, de uitgaande gesprekken worden behandeld als een aanroep van de 'Internet'.  Als gevolg hiervan het netwerkverkeer dat binnenkomt op de tweede App Service-omgeving wordt weergegeven als zijnde afkomstig van 192.23.1.2 (dat wil zeggen niet de subnet-adresbereik van de eerste App Service-omgeving).

Hoewel aanroepen tussen verschillende App Service-omgevingen worden behandeld als 'Internet'-aanroepen, wanneer beide App Service-omgevingen bevinden zich in dezelfde Azure-regio het netwerkverkeer wordt blijven op de regionale Azure-netwerk en niet fysiek stroom via de openbare Internet.  Als gevolg hiervan kunt u een netwerkbeveiligingsgroep op het subnet van de tweede App Service-omgeving toe te staan dat inkomende aanroepen van de eerste App Service Environment (waarvan uitgaande IP-adres is 192.23.1.2), zodat beveiligde communicatie tussen de App. Service-omgevingen.

## <a name="additional-links-and-information"></a>Aanvullende koppelingen en informatie
Meer informatie over binnenkomende poorten die door App Service-omgevingen worden gebruikt en vindt u inkomend verkeer beheren met behulp van netwerkbeveiligingsgroepen [hier][controllinginboundtraffic].

Informatie over het gebruik van de gebruiker gedefinieerde routes te verlenen uitgaande internettoegang tot App Service-omgevingen is beschikbaar in deze [artikel][ExpressRoute]. 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[ExpressRoute]:  app-service-app-service-environment-network-configuration-expressroute.md

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

