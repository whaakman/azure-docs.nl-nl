---
title: Overzicht van de netwerkarchitectuur van App Service-omgevingen
description: Overzicht van de architectuur van netwerk-topologie ofApp Service-omgevingen.
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 13d03a37-1fe2-4e3e-9d57-46dfb330ba52
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.openlocfilehash: 3362a55524da42914681db06b8d2c0da8df773d8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="network-architecture-overview-of-app-service-environments"></a>Overzicht van de netwerkarchitectuur van App Service-omgevingen
## <a name="introduction"></a>Inleiding
App Service-omgevingen worden altijd gemaakt binnen een subnet van een [virtueel netwerk] [ virtualnetwork] -apps die worden uitgevoerd in een App Service-omgeving kunnen communiceren met de persoonlijke eindpunten zich binnen de virtuele netwerktopologie.  Aangezien klanten delen van hun virtuele netwerkinfrastructuur vergrendelen kunnen, is het belangrijk om te begrijpen welke typen netwerk communicatie stromen die bij een App Service-omgeving optreden.

## <a name="general-network-flow"></a>Algemene netwerk stroom
Wanneer een as-omgeving (App Service omgeving) een openbare virtuele IP-adres (VIP) voor apps gebruikt, worden alle binnenkomend verkeer binnenkomt op die openbare VIP.  Dit omvat HTTP en HTTPS-verkeer voor apps, evenals het andere verkeer voor FTP-functionaliteit voor foutopsporing op afstand en Azure beheerbewerkingen.  Zie het artikel op voor een volledige lijst van de specifieke poorten (vereiste en optionele) die beschikbaar op het openbare VIP zijn [binnenkomend verkeer te regelen] [ controllinginboundtraffic] naar een App Service-omgeving. 

App Service-omgevingen ondersteunen ook actief apps die afhankelijk zijn van alleen de interne adres van een virtueel netwerk, ook wel een ILB-adres (interne load balancer).  Op een ILB ingeschakelde as-omgeving, HTTP en HTTPS-verkeer voor apps en externe foutopsporing oproepen, op het adres ILB binnenkomen.  Voor de meest voorkomende configuraties ILB-as-omgeving aankomt FTP-/ FTPS-verkeer ook op het adres ILB.  Azure beheerbewerkingen nog steeds worden overgebracht naar poorten 454/455 op het openbare VIP van een ILB ingeschakeld echter as-omgeving.

Het onderstaande diagram ziet u een overzicht van de verschillende binnenkomende en uitgaande tenantnetwerkstromen voor een App Service-omgeving waar de apps die zijn gebonden aan een openbare virtuele IP-adres:

![Algemene Tenantnetwerkstromen][GeneralNetworkFlows]

Een App Service-omgeving kunnen communiceren met tal van persoonlijke klant eindpunten.  Bijvoorbeeld: apps die worden uitgevoerd in App Service-omgeving kunnen verbinden met database server (s) die worden uitgevoerd op IaaS virtuele machines in de virtuele netwerktopologie.

> [!IMPORTANT]
> Het netwerkdiagram bekijkt, de 'andere Compute Resources' worden geïmplementeerd in een ander Subnet uit de App Service-omgeving. Implementatie van resources in hetzelfde Subnet met de as-omgeving, wordt de verbinding tussen de as-omgeving en deze bronnen (met uitzondering van specifieke intra-as-omgeving routering) geblokkeerd. Implementeer in plaats daarvan op een ander Subnet (in hetzelfde VNET). De App Service-omgeving zijn vervolgens verbinding kunnen maken. Er is geen aanvullende configuratie nodig.
> 
> 

App Service-omgevingen ook worden gecommuniceerd met Sql-database en Azure Storage bronnen nodig voor het beheren van en het gebruik van een App Service-omgeving.  Sommige van de Sql- en Storage-resources die een App-serviceomgeving met communiceert bevinden zich in dezelfde regio bevinden als de App Service-omgeving, terwijl anderen bevinden zich in de externe Azure-regio's.  Als gevolg hiervan is uitgaande verbinding met Internet altijd vereist voor een App Service-omgeving te laten functioneren. 

Aangezien een App Service-omgeving is geïmplementeerd in een subnet, kunnen netwerkbeveiligingsgroepen waarmee binnenkomend verkeer op het subnet worden gebruikt.  Zie voor meer informatie over het beheren van inkomend verkeer naar een App Service-omgeving de volgende [artikel][controllinginboundtraffic].

Zie voor meer informatie over het toestaan van uitgaande internetverbinding vanaf een App Service-omgeving het volgende artikel over het werken met [Express Route][ExpressRoute].  Dezelfde aanpak beschreven in het artikel is van toepassing als u werkt met Site-naar-Site-connectiviteit en gebruik van geforceerde tunneling.

## <a name="outbound-network-addresses"></a>Uitgaande netwerkadressen
Wanneer een App Service-omgeving uitgaande oproepen maakt, is een IP-adres altijd gekoppeld aan de uitgaande aanroepen.  Het specifieke IP-adres dat wordt gebruikt, is afhankelijk van of het eindpunt wordt aangeroepen zich binnen de virtuele netwerktopologie of buiten de virtuele netwerktopologie bevindt.

Als het eindpunt wordt aangeroepen **buiten** van de virtuele netwerktopologie vervolgens het uitgaande adres (ook wel de uitgaande NAT-adres) dat wordt gebruikt is het openbare VIP van de App Service-omgeving.  Dit adres vindt u in de gebruikersinterface van de portal voor de App Service-omgeving in eigenschappenblade.

![Uitgaande IP-adres][OutboundIPAddress]

Dit adres kan ook worden bepaald voor ASEs waarvoor alleen openbare VIP door het maken van een app in App Service-omgeving en vervolgens uit te voeren een *nslookup* op adres van de app. Het resulterende IP-adres is zowel het openbare VIP, evenals uitgaande NAT-adres van de App Service-omgeving.

Als het eindpunt wordt aangeroepen **binnen** van de virtuele netwerktopologie het uitgaande adres van de aanroepende app worden de interne IP-adres van de afzonderlijke rekenbron uitvoeren van de app.  Er is echter geen permanente toewijzing van virtueel netwerk interne IP-adressen voor apps.  Apps kunnen verplaatsen tussen verschillende rekenresources en de groep met beschikbare compute bronnen in een App Service-omgeving vanwege schalen bewerkingen kunnen wijzigen.

Echter, omdat er nog steeds een App Service-omgeving zich in een subnet, wordt gegarandeerd dat het interne IP-adres van een berekeningsresource uitvoeren van een app altijd wordt liggen binnen het CIDR-bereik van het subnet.  Als gevolg hiervan als fijnmazig ACL's of netwerkbeveiligingsgroepen worden gebruikt voor beveiligde toegang tot de andere eindpunten in het virtuele netwerk, moet het bereik van subnet met de App-serviceomgeving toegang te krijgen.

Het volgende diagram toont deze begrippen in meer detail:

![Uitgaande netwerkadressen][OutboundNetworkAddresses]

In het bovenstaande diagram:

* Omdat het openbare VIP van de App-serviceomgeving 192.23.1.2, dat het uitgaande IP-adres gebruikt bij het aanroepen naar 'Internet' eindpunten is.
* De CIDR-bereik van het insluitende subnet voor de App-serviceomgeving is 10.0.1.0/26.  Andere eindpunten in dezelfde virtuele-netwerkinfrastructuur ziet aanroepen vanuit apps als zijnde afkomstig van ergens binnen dit adresbereik.

## <a name="calls-between-app-service-environments"></a>Aanroepen tussen App Service-omgevingen
Een complexere kan gebeuren als u meerdere App Service-omgevingen in hetzelfde virtuele netwerk implementeert, en uitgaande aanroepen vanuit een App Service-omgeving naar een andere App Service-omgeving te maken.  Deze typen cross-App Service-omgeving aanroepen, ook worden behandeld als 'Internet' aanroepen.

Het volgende diagram toont een voorbeeld van een gelaagde architectuur met apps op één App Service-omgeving (bijvoorbeeld) 'Voordeur' web-apps) voor het aanroepen van apps op een tweede App Service-omgeving (bijvoorbeeld interne back-end-API-apps niet moeten toegankelijk zijn vanaf Internet). 

![Aanroepen tussen App Service-omgevingen][CallsBetweenAppServiceEnvironments] 

In het bovenstaande voorbeeld heeft de App-serviceomgeving 'As-omgeving een' een uitgaande IP-adres van 192.23.1.2.  Als een app uitgevoerd op deze wordt App Service-omgeving maakt een uitgaande aanroep van een app uitgevoerd op een tweede App Service-omgeving ('as-omgeving twee') zich in hetzelfde virtuele netwerk, de uitgaande aanroep worden beschouwd als een aanroep van 'Internet'.  Als gevolg hiervan het netwerkverkeer dat binnenkomt op de tweede App Service-omgeving wordt weergegeven als zijnde afkomstig van 192.23.1.2 (d.w.z. niet het subnet adresbereik van de eerste App Service-omgeving).

Hoewel aanroepen tussen verschillende App Service-omgevingen worden behandeld als 'Internet'-aanroepen wanneer beide App Service-omgevingen bevinden zich in dezelfde Azure-regio wordt het netwerkverkeer blijft op de regionale Azure-netwerk en niet fysiek worden overgebracht via het openbare Internet.  Als gevolg hiervan kunt u een netwerkbeveiligingsgroep op het subnet van de tweede App Service-omgeving toe te staan dat inkomende aanroepen van de eerste App Service-omgeving (192.23.1.2 waarvan uitgaande IP-adres is), zodat een veilige communicatie tussen de App Service-omgevingen.

## <a name="additional-links-and-information"></a>Aanvullende koppelingen en informatie
Meer informatie over binnenkomende poorten gebruikt door App Service-omgevingen en binnenkomend verkeer te beheren met behulp van netwerkbeveiligingsgroepen is beschikbaar [hier][controllinginboundtraffic].

Informatie over het gebruik van de gebruiker gedefinieerde routes toe te kennen uitgaande internettoegang tot de App Service-omgevingen zijn beschikbaar in deze [artikel][ExpressRoute]. 

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[ExpressRoute]:  app-service-app-service-environment-network-configuration-expressroute.md

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

