---
title: Application gateway-onderdelen
description: In dit artikel bevat informatie over de verschillende onderdelen in een application gateway
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: f5dfa34760bcef23bf54d65b35e3ad8f48cc2ee5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60831828"
---
# <a name="application-gateway-components"></a>Application gateway-onderdelen

 Een application gateway fungeert als de één contactpunt voor clients. Het verdeeld binnenkomende verkeer van de toepassing over meerdere back-endpools, waaronder Azure Virtual machines, virtuele-machineschaalsets, Azure App Service en on-premises/externe servers. Een application gateway gebruikt om verkeer te distribueren, verschillende onderdelen die worden beschreven in dit artikel.

![De onderdelen die worden gebruikt in een application gateway](./media/application-gateway-components/application-gateway-components.png)

## <a name="frontend-ip-addresses"></a>Frontend-IP-adressen

Een front-end-IP-adres is het IP-adres dat is gekoppeld aan een toepassingsgateway. U kunt een toepassingsgateway met een openbaar IP-adres, een privé IP-adres of beide. Een application gateway biedt ondersteuning voor een openbare of een privé IP-adres. Uw virtuele netwerk en openbare IP-adres moeten zich op dezelfde locatie bevinden als uw application gateway. Nadat deze gemaakt, is een front-end-IP-adres gekoppeld aan een listener.

### <a name="static-versus-dynamic-public-ip-address"></a>Van statische versus dynamische openbare IP-adres

De v2-SKU van Azure Application Gateway ondersteunt zowel statische interne en statische openbare IP-adressen, hoewel de v1-SKU alleen statische interne IP-adressen ondersteunt. Het virtuele IP-adres (VIP)-adres kunt wijzigen als een toepassingsgateway is gestopt en gestart.

De DNS-naam die is gekoppeld aan een application gateway verandert niet gedurende de levenscyclus van de gateway. Als gevolg hiervan moet u een CNAME-alias gebruiken en wijs het DNS-adres van de toepassingsgateway.

## <a name="listeners"></a>Listeners

Een listener is een logische eenheid die controleert op binnenkomende verbindingsaanvragen. Een listener accepteert een aanvraag als het protocol, de poort, de host en het IP-adres dat is gekoppeld aan de aanvraag overeenkomt met de dezelfde elementen die zijn gekoppeld aan de listenerconfiguratie van de.

Voordat u een application gateway gebruikt, moet u ten minste één listener toevoegen. Kunnen er meerdere listeners die is gekoppeld aan een application gateway, en ze kunnen worden gebruikt voor hetzelfde protocol.

Nadat een listener binnenkomende aanvragen van clients detecteert, stuurt de application gateway deze aanvragen door naar de leden in de back-endpool. De application gateway maakt gebruik van de aanvraag-routeringsregels gedefinieerd voor de listener die de inkomende aanvraag ontvangen.

Listeners ondersteunen de volgende poorten en protocollen.

### <a name="ports"></a>Poorten

Een poort is waar een listener luistert naar aanvragen van clients. U kunt poorten tussen 1 en 65502 voor de v1-SKU en 1-65199 voor de v2-SKU.

### <a name="protocols"></a>Protocollen

Application Gateway ondersteunt vier protocollen: HTTP, HTTPS, HTTP/2 en WebSocket:

- Geef tussen HTTP en HTTPS protocollen in de listenerconfiguratie van de.
- Ondersteuning voor [WebSockets- en HTTP/2-protocollen](https://docs.microsoft.com/azure/application-gateway/overview#websocket-and-http2-traffic) systeemeigen, wordt geleverd en [WebSocket-ondersteuning](https://docs.microsoft.com/azure/application-gateway/application-gateway-websocket) is standaard ingeschakeld. Er is geen door de gebruiker configureerbare instelling om selectief WebSocket-ondersteuning in of uit te schakelen. Gebruikmaken van WebSockets met HTTP en HTTPS-listeners.
- Ondersteuning voor HTTP/2-protocol is beschikbaar voor clients verbinding maken met application gateway alleen listeners. De communicatie met de back-endserverpools is via HTTP/1.1. Ondersteuning voor HTTP/2 is standaard uitgeschakeld. U kunt deze inschakelen.

Gebruik een HTTPS-listener voor SSL-beëindiging. Een HTTPS-listener verplaatst het werk voor versleuteling en ontsleuteling naar uw application gateway, zodat uw webservers worden niet als enige door overhead belast. Uw apps zijn vervolgens kunt richten op zakelijke logica.

### <a name="custom-error-pages"></a>Aangepaste foutpagina's

Application Gateway kunt u aangepaste foutpagina's in plaats van standaard foutpagina's maken. U kunt uw eigen huisstijl en lay-out hanteren door een aangepaste foutpagina te gebruiken. Application Gateway wordt een aangepaste foutpagina weergegeven wanneer een aanvraag tijdelijk niet bereikbaar voor de back-end.

Zie voor meer informatie, [aangepaste foutpagina's voor uw application gateway](https://docs.microsoft.com/azure/application-gateway/custom-error).

### <a name="types-of-listeners"></a>Typen listeners

Er zijn twee soorten listeners:

- **Basic**. Dit type listener luistert naar een enkel domein-site waarop het heeft een één DNS-toewijzing naar de IP-adres van de toepassingsgateway. Deze listenerconfiguratie is vereist wanneer u een enkele site achter een application gateway hosten.

- **Meerdere sites**. Deze listenerconfiguratie is vereist wanneer u meer dan één webtoepassing op de dezelfde toepassingsgateway-exemplaar configureren. Hiermee kunt u een efficiëntere topologie voor uw implementaties configureren door maximaal 100 websites toe te voegen aan één toepassingsgateway. Elke website kan worden omgeleid naar een eigen back-endpool. Bijvoorbeeld wijst drie subdomeinen, abc.contoso.com xyz.contoso.com en pqr.contoso.com, u het IP-adres van de toepassingsgateway. U zou maken van drie multi-site-listeners en elke listener configureren voor de respectieve poort en protocolinstelling.

    Zie voor meer informatie, [meerdere sites hosten](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview).

Nadat u een listener maakt, koppelt u deze met een regel voor het doorsturen van aanvragen. Deze regel bepaalt hoe de aanvraag is ontvangen op de listener moet worden doorgestuurd naar de back-end.

Application Gateway listeners in de aangegeven volgorde worden verwerkt. Als basislistener overeenkomt met een inkomende aanvraag, wordt deze eerst verwerkt. Configureren om verkeer te routeren naar de juiste back-end, een listener voor meerdere locaties voordat u een basislistener.

## <a name="request-routing-rules"></a>Aanvragen van regels voor doorsturen

Een regel voor het doorsturen van aanvraag is een belangrijk onderdeel van een application gateway, omdat het bepaalt hoe het routeren van verkeer op de listener. De regel verbindt de listener, de back-endserverpool en de back-end-HTTP-instellingen.

Wanneer een listener van een aanvraag heeft geaccepteerd, wordt de regel voor het doorsturen van aanvraag stuurt de aanvraag naar de back-end of elders wordt omgeleid. Als de aanvraag wordt doorgestuurd naar de back-end, definieert de regel voor het doorsturen van aanvraag welke back-endserverpool voor het doorsturen van deze. Ook, bepaalt de regel voor het doorsturen van aanvraag ook of de kopteksten in de aanvraag worden herschreven zijn. Een listener kan worden gekoppeld aan één regel.

Er zijn twee typen regels voor het doorsturen van aanvraag:

- **Basic**. Alle aanvragen op de bijbehorende listener (bijvoorbeeld blog.contoso.com/*) worden doorgestuurd naar de gekoppelde back-endpool met behulp van de bijbehorende HTTP-instelling.

- **Op pad gebaseerde**. Deze regel voor doorsturen kunt u de aanvragen versturen op de bijbehorende listener met een specifieke back endadresgroep, op basis van de URL in de aanvraag. Als het pad van de URL in een aanvraag overeenkomt met het padpatroon in een pad gebaseerde regel, stuurt de regel die aanvraag. Het padpatroon toepassing alleen op het URL-pad, niet op de queryparameters. Als de URL-pad voor een listener-aanvraag komt niet overeen met een van de regels op basis van het pad, de aanvraag wordt gerouteerd naar de standaard back-end-adresgroep en de HTTP-instellingen.

Zie voor meer informatie, [URL-gebaseerde routering](https://docs.microsoft.com/azure/application-gateway/url-route-overview).

### <a name="redirection-support"></a>Ondersteuning voor omleiding

De regel voor het doorsturen van aanvraag kunt u omleiden van verkeer op de application gateway. Dit is een mechanisme voor algemene omleiding, zodat u naar omleiden kunt en vanuit een willekeurige poort die u met behulp van regels definiëren.

U kunt het doel van omleiding naar een ander listener (dit kan helpen om automatische HTTP naar HTTPS-omleiding inschakelen) of een externe site. U kunt er ook voor kiezen om de tijdelijke of permanente omleiding of de URI-pad en de query-tekenreeks toevoegen aan de omgeleide URL.

Zie voor meer informatie, [omleiden van verkeer op uw application gateway](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

### <a name="rewrite-http-headers"></a>HTTP-headers opnieuw genereren

Met behulp van de regels voor het doorsturen van aanvragen, kunt u toevoegen, verwijderen of bijwerken van de aanvraag HTTP (S) en antwoordheaders als de aanvraag en respons pakketten verplaatsen tussen de client en de back-end-pools met de application gateway.

De headers kunnen worden ingesteld op statische waarden of op andere headers en servervariabelen. Dit helpt met belangrijke use-cases, zoals het extraheren van client-IP-adressen, verwijderen van gevoelige informatie over de back-end, meer beveiliging toe te voegen enzovoort.

Zie voor meer informatie, [Herschrijf de HTTP-headers in uw application gateway](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).

## <a name="http-settings"></a>HTTP-instellingen

Een application gateway routeert verkeer naar de back-endservers (opgegeven in de aanvraag-routeringsregel met de HTTP-instellingen) met behulp van het poortnummer, protocol en andere instellingen in dit onderdeel worden beschreven.

De poort en protocol dat wordt gebruikt in de HTTP-instellingen te bepalen of het verkeer tussen de application gateway- en back-end servers (inclusief end-to-end-SSL) is versleuteld of niet-versleuteld.

Dit onderdeel wordt ook gebruikt voor:

- Bepalen of een gebruikerssessie op dezelfde server worden gehouden met behulp van de [cookies gebaseerde sessieaffiniteit](https://docs.microsoft.com/azure/application-gateway/overview#session-affinity).

- Back-endpoolleden probleemloos verwijderen met behulp van [verwerkingsstop](https://docs.microsoft.com/azure/application-gateway/overview#connection-draining).

- Een aangepaste test om te controleren van de back endstatus, de time-outinterval van aanvraag instellen, onderdrukken van hostnaam en het pad in de aanvraag en één klik gemakkelijk instellingen opgeven voor de back-end van de App Service bieden koppelen.

## <a name="backend-pools"></a>Back-endpools

Een back-endpool stuurt aanvraag door naar de back-endservers, die de aanvraag dienen. Back-endpools kunnen bevatten:

- NIC’s
- Virtuele-machineschaalsets
- Openbare IP-adressen
- Interne IP-adressen
- FQDN-NAAM
- Back-ends voor meerdere tenants (zoals App Service)

Back-endpoolleden voor Application Gateway zijn niet gekoppeld aan een beschikbaarheidsset. Een application gateway kan communiceren met instanties buiten het virtuele netwerk dat deel uitmaakt. Als gevolg hiervan kunnen de leden van de back endadresgroepen zijn in meerdere computerclusters, in datacenters of buiten Azure, zolang er een IP-verbinding is.

Als u interne IP-adressen als back-endpoolleden gebruiken, moet u [peering op virtueel netwerk](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) of een [VPN-gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Peering op virtueel netwerk wordt ondersteund en nuttig zijn voor taakverdeling verkeer in andere virtuele netwerken.

Een application gateway kan ook met communiceren met on-premises servers wanneer ze zijn verbonden met Azure ExpressRoute of VPN-tunnels als verkeer is toegestaan.

U kunt verschillende back-endpools voor verschillende soorten aanvragen kunt maken. Maak bijvoorbeeld een back-endpool voor algemene aanvragen, en vervolgens op een andere back-endpool voor aanvragen voor de microservices voor uw toepassing.

## <a name="health-probes"></a>Statuscontroles

Standaard wordt een application gateway controleert de status van alle resources in de back-endpool en worden automatisch verwijderd die niet in orde. Deze wordt bewaakt exemplaren en voegt deze toe terug naar de goede back-endpool wanneer ze beschikbaar komen en op statuscontroles reageren.

Naast het gebruik van standaard health test controle, kunt u ook de statustest aan de behoeften van de vereisten van uw toepassing aanpassen. Aangepaste tests kunnen gedetailleerdere controle over de statuscontrole. Wanneer u aangepaste tests, kunt u de testinterval, de URL en pad om te testen en het aantal mislukte reacties op accepteren voordat het exemplaar van de groep back-end is gemarkeerd als niet in orde. Het is raadzaam dat u configureert aangepaste tests om te controleren van de status van elke back-endpool.

Zie voor meer informatie, [controleren van de status van uw toepassingsgateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview).

## <a name="next-steps"></a>Volgende stappen

Een toepassingsgateway maken:

* [In de Azure-portal](quick-create-portal.md)
* [Met behulp van Azure PowerShell](quick-create-powershell.md)
* [Met behulp van de Azure CLI](quick-create-cli.md)
