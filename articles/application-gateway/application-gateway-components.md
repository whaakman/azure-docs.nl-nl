---
title: Azure Application Gateway-onderdelen
description: In dit artikel bevat informatie over de verschillende onderdelen in Application Gateway
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: 44c8b331ebb258c39a003c91e0711e6dfb87cb12
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58076290"
---
# <a name="application-gateway-components"></a>Application gateway-onderdelen

 Een application gateway fungeert als de één contactpunt voor clients. Het verdeeld binnenkomende verkeer van de toepassing over meerdere back endpools, zoals virtuele Azure-machines, virtuele-machineschaalsets, App Services of on-premises/externe servers. U doet dit door deze verschillende onderdelen die worden beschreven in dit artikel gebruikt.

![Application-gateway-onderdelen](./media/application-gateway-components/application-gateway-components.png)

## <a name="frontend-ip-address"></a>IP-adres voor front-end

Een front-end-IP-adres is het IP-adres dat is gekoppeld aan de toepassingsgateway. U kunt de toepassingsgateway om ofwel een openbaar IP-adres of een privé IP-adres of beide. Slechts één openbaar IP-adres of een privé IP-adres wordt ondersteund. Uw virtuele netwerk en openbare IP-adres moeten zich op dezelfde locatie bevinden als uw application gateway.

Een Frontend-IP-adres is gekoppeld aan een *listener* nadat deze is gemaakt. 

### <a name="static-vs-dynamic-public-ip-address"></a>Statische versus dynamische openbare IP-adres

De v1-SKU biedt ondersteuning voor statische interne IP-adressen, maar biedt geen ondersteuning voor statische openbare IP-adressen. Het VIP kunt wijzigen als de toepassingsgateway is gestopt en gestart. De DNS-naam die is gekoppeld aan de application gateway verandert niet gedurende de levenscyclus van de gateway. Om deze reden is het aanbevolen om een CNAME-alias gebruiken en wijs het DNS-adres van de toepassingsgateway.

De v2-SKU van Application Gateway biedt ondersteuning voor statische openbare IP-adressen, evenals de statische interne IP-adressen. 

## <a name="listeners"></a>Listeners

Voordat u met behulp van uw application gateway begint, moet u een of meer listeners toevoegen. Een listener is een logische eenheid die wordt gecontroleerd op binnenkomende verbindingsaanvragen en de aanvragen worden geaccepteerd als het protocol, de poort, de host en de IP-adres die zijn gekoppeld aan de aanvraag overeenkomt met het protocol, de poort, de host en de IP-adres dat is gekoppeld aan de listenerconfiguratie van de. Er kunnen zich meerdere listeners die is gekoppeld aan een application gateway en ze kunnen worden gebruikt voor hetzelfde protocol. Nadat de listener binnenkomende aanvragen van clients detecteert, stuurt de Application Gateway deze aanvragen door naar de leden in de back-endpool, met behulp van de aanvraag routeringsregels toe die u definieert voor de listener die de inkomende aanvraag ontvangen.

Listeners ondersteunen de volgende poorten en protocollen:

### <a name="ports"></a>Poorten

Dit is de poort waarop de listener voor de clientaanvraag luistert. U kunt poorten tussen 1 en 65502 voor V1-SKU's en 1-65199 voor V2 SKU configureren.

### <a name="protocols"></a>Protocollen

Application gateway ondersteunt de volgende vier protocollen: HTTP, HTTPS, HTTP/2 en WebSocket

U opgeven de keuze tussen de protocollen HTTP en HTTPS in de listenerconfiguratie expliciet. De [ondersteuning voor de WebSockets en HTTP/2-protocollen](https://docs.microsoft.com/azure/application-gateway/overview#websocket-and-http2-traffic) systeemeigen wordt geleverd. [Ondersteuning voor Websocket](https://docs.microsoft.com/azure/application-gateway/application-gateway-websocket) is standaard ingeschakeld. Er is geen door de gebruiker configureerbare instelling om selectief WebSocket-ondersteuning in of uit te schakelen. U kunt WebSockets gebruiken met HTTP en HTTPS-listeners. Ondersteuning voor HTTP/2-protocol is beschikbaar voor clients verbinding maken met application gateway alleen listeners. De communicatie met de back-endserverpools is via HTTP/1.1. Ondersteuning voor HTTP/2 is standaard uitgeschakeld. U kunt deze inschakelen.

U kunt een HTTPS-listener voor SSL-beëindiging. Een HTTPS-listener verplaatst de versleuteling en ontsleuteling werken aan uw toepassingsgateway zodat uw webservers worden niet als enige belast door die overhead. Uw toepassingen zijn vervolgens kunt richten op hun zakelijke logica.

### <a name="custom-error-pages"></a>Aangepaste foutpagina's

Application gateway kunt u aangepaste foutpagina's in plaats van standaard foutpagina's maken. U kunt uw eigen huisstijl en lay-out hanteren door een aangepaste foutpagina te gebruiken. Application-gateway kan een aangepaste foutpagina weergegeven wanneer een aanvraag tijdelijk niet bereikbaar voor de back-end. Zie voor meer informatie, [aangepaste foutpagina's voor uw Application Gateway](https://docs.microsoft.com/azure/application-gateway/custom-error).

### <a name="types-of-listeners"></a>Typen listeners

Er zijn twee soorten listeners:

- **Basic**: Dit type listener luistert naar een enkel domein-site waarop het heeft een DNS-toewijzing van één IP-adres van de toepassingsgateway. Deze listenerconfiguratie is vereist wanneer u een enkele site achter een application gateway host.
- **Meerdere sites**: Deze listenerconfiguratie is vereist wanneer u meer dan één webtoepassing op de dezelfde toepassingsgateway-exemplaar configureren. Hiermee kunt u een efficiëntere topologie voor uw implementaties configureren door maximaal 100 websites toe te voegen aan één toepassingsgateway. Elke website kan worden omgeleid naar een eigen back-endpool. Bijvoorbeeld:  Voor drie subdomeinen: abc.contoso.com, xyz.contoso.com en pqr.contoso.com die verwijst naar het IP-adres van de toepassingsgateway. Maak drie listeners van het type *multi-site* en elke listener configureren voor de desbetreffende poort en protocol instellen. Zie voor meer informatie, [hosten van meerdere sites](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview).

Nadat u een listener maakt, koppelt u deze aan een aanvraag routeringsregel waarmee wordt bepaald hoe de ontvangen op de listener-aanvragen worden doorgestuurd naar de back-end.

Listeners worden verwerkt in de volgorde waarin die ze worden weergegeven. Om die reden als een basislistener overeenkomt met een binnenkomende aanvraag verwerkt eerst. Multi-site-listeners moeten worden geconfigureerd voordat u een basislistener om te zorgen dat verkeer wordt gerouteerd naar de juiste back-end.

## <a name="request-routing-rule"></a>Regel voor het doorsturen van aanvragen

Dit is het belangrijkste onderdeel van de toepassingsgateway en bepaalt hoe het verkeer op de listener die zijn gekoppeld aan deze regel wordt gerouteerd. De regel verbindt de listener, de back-endserverpool en de back-end-HTTP-instellingen. Wanneer een aanvraag is geaccepteerd door een listener, bepaalt de regel voor het doorsturen van aanvraag of de aanvraag worden doorgestuurd naar de back-end of elders wordt omgeleid. Als de aanvraag is vastgesteld dat moet worden doorgestuurd naar de back-end, geeft de regel voor het doorsturen van aanvraag wordt gedefinieerd welke back-endserverpool moet deze worden doorgestuurd. Verder, bepaalt de regel voor het doorsturen van aanvraag ook of de kopteksten in de aanvraag worden herschreven zijn. Een listener kan worden gekoppeld aan slechts één regel.

Er zijn twee typen van regels voor het doorsturen van aanvraag:

- **Basic:** Alle aanvragen op de bijbehorende listener (bijvoorbeeld: blog.contoso.com/*) worden doorgestuurd naar de gekoppelde back-endadresgroep met behulp van de bijbehorende HTTP-instelling.
- **Op basis van pad:** Dit regeltype kunt u de aanvragen versturen op de bijbehorende listener aan een specifieke back endadresgroep op basis van de URL in de aanvraag. Als het pad van de URL in een aanvraag overeenkomt met het padpatroon in een pad gebaseerde regel, wordt de aanvraag doorgestuurd met behulp van die regel. De pad-patroon wordt alleen toegepast op het pad van de URL, niet naar de queryparameters. Als het pad van de URL van een aanvraag voor een listener komt niet overeen met een van de regels op basis van het pad, wordt de aanvraag wordt doorgestuurd naar de *standaard* back-endpool en de *standaard* HTTP-instellingen. Zie voor meer informatie, [URL gebaseerde routering](https://docs.microsoft.com/azure/application-gateway/url-route-overview).

### <a name="redirection-support"></a>Ondersteuning voor omleiding

De regel voor het doorsturen van aanvraag kunt u omleiden van verkeer op de application gateway. Dit is een algemeen omleidingsmechanisme, zodat u op basis van regels kunt omleiden van en naar elke poort die u gebruikt. U kunt het doel van omleiding naar een ander listener (dit kan helpen om automatische HTTP naar HTTPS-omleiding inschakelen) of een externe site kiezen, kiest u de omleiding naar een tijdelijke of permanente of kiest u de URI-pad en de query-tekenreeks toe te voegen aan de URL van de omgeleide. Zie voor meer informatie, [omleiden van verkeer op uw Application Gateway](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

#### <a name="rewrite-http-headers"></a>HTTP-headers opnieuw genereren

Met behulp van de aanvraag routeringsregels kunt u toevoegen, verwijderen of bijwerken van HTTP (S) aanvragen en reactieheaders terwijl de aanvraag en antwoordpakketten verplaatsen tussen de client en back-end-pools, via de application gateway. De headers kunnen niet alleen worden ingesteld op statische waarden, maar ook voor andere headers en servervariabelen belangrijk. Hierdoor kunt u uitvoeren van verschillende belangrijke use-cases, zoals het ophalen van IP-adres van de clients, verwijderen van gevoelige informatie over de back-end, toevoegen van extra beveiligingsmaatregelen, enzovoort. Zie voor meer informatie, [Herschrijf de HTTP-headers in uw application gateway](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).

## <a name="http-settings"></a>HTTP-instellingen

De application gateway-routes, verkeer naar de back-end servers (opgegeven in de aanvraag routeringsregel waaraan de HTTP-instellingen zijn gekoppeld) met behulp van het poortnummer, protocol, en andere instellingen die zijn opgegeven in dit onderdeel. De poort en protocol dat wordt gebruikt in de HTTP-instellingen te bepalen of het verkeer tussen de gateway en back-end-toepassingsservers wordt versleuteld, dus het uitvoeren van end-to-end SSL of niet-versleuteld. Dit onderdeel wordt ook gebruikt voor het: bepalen of een gebruikerssessie op dezelfde server worden gehouden met behulp van de [cookies gebaseerde sessieaffiniteit](https://docs.microsoft.com/azure/application-gateway/overview#session-affinity), voeren correct verwijderen van back-endpoolleden met behulp van [verbinding een verwerkingsstop](https://docs.microsoft.com/azure/application-gateway/overview#connection-draining), aangepaste test om te controleren van de back endstatus, de time-outinterval van aanvraag instellen, overschrijven de hostnaam en het pad in de aanvraag te koppelen en één klik gemakkelijk om op te geven van de back-end-instelling voor back-end van App service bieden. 

## <a name="backend-pool"></a>Back-endpool

De back-endpool wordt gebruikt voor het routeren van aanvragen naar de back-endservers, die de aanvraag dienen. Back-endpools kunnen bestaan uit NIC's, de virtuele machine schalen sets, openbare IP-adressen, interne IP-adressen, FQDN-naam en multitenant back-ends, zoals Azure App Service. Back-endpoolleden voor Application gateway zijn niet gekoppeld aan een beschikbaarheidsset. Application Gateway kan communiceren met exemplaren buiten het virtuele netwerk dat deel uitmaakt, dus de leden van de back-endpools mag bestaan uit tussen verschillende clusters, datacenters, of buiten Azure, zolang er een IP-verbinding is. Als u van plan bent om te gebruiken van interne IP-adressen als back-endpoolleden, wordt hiervoor [VNET-Peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) of [VPN-Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). VNet-peering wordt ondersteund en kan nuttig zijn voor taakverdeling van verkeer in andere virtuele netwerken. De toepassingsgateway kan ook communiceren met met on-premises servers wanneer ze zijn verbonden met ExpressRoute of VPN-tunnels, zolang er verkeer is toegestaan.

U kunt verschillende back-endpools voor verschillende soorten aanvragen kunt maken. Maak bijvoorbeeld een back-endpool voor algemene aanvragen en andere back-endpool voor aanvragen voor de microservices voor uw toepassing.

## <a name="health-probes"></a>Statuscontroles

Application gateway standaard controleert de status van alle resources in de back-endpool en worden een resource als slecht beschouwd uit de groep automatisch verwijderd. Het blijft voor het bewaken van de exemplaren en terug naar de goede back-endpool toegevoegd zodra ze beschikbaar komen en op statuscontroles reageren. Naast het gebruik van standaard health test controle, kunt u ook de statustest aan de behoeften van de vereisten van uw toepassing aanpassen. Aangepaste tests kunnen u een meer nauwkeurige controle over de statuscontrole. Wanneer u aangepaste tests, kunt u de testinterval, de URL en pad om te testen en het aantal mislukte reacties op accepteren voordat het back-end-pool-exemplaar is gemarkeerd als niet in orde. Het raadzaam dat u aangepaste tests configureert om de status van elke back-end-toepassingen worden gecontroleerd. Zie voor meer informatie, [controleren van de status van uw toepassingsgateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview).

## <a name="next-steps"></a>Volgende stappen

Nadat u meer over Application Gateway-onderdelen, kunt u het volgende doen:
* [Een toepassingsgateway maken in Azure portal](quick-create-portal.md)
* [Een toepassingsgateway maken met behulp van Azure PowerShell](quick-create-powershell.md)
* [Een toepassingsgateway maken met behulp van de Azure CLI](quick-create-cli.md)
