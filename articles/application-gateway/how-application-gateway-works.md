---
title: De werking van een application gateway
description: Dit artikel bevat informatie over de werking van een application gateway
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: a16421182f533f5aa2ad4bcc2e58e910cc7e8ca6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64702413"
---
# <a name="how-an-application-gateway-works"></a>De werking van een application gateway

In dit artikel wordt uitgelegd hoe een application gateway inkomende aanvragen worden geaccepteerd en stuurt deze door naar de back-end.

![Hoe een aanvraag in een toepassingsgateway worden geaccepteerd](./media/how-application-gateway-works/how-application-gateway-works.png)

## <a name="how-an-application-gateway-accepts-a-request"></a>Hoe een aanvraag in een toepassingsgateway worden geaccepteerd

1. Voordat een client een aanvraag naar een application gateway verzendt, wordt de domeinnaam van de toepassingsgateway omgezet met behulp van een Domain Name System (DNS)-server. Azure bepaalt de DNS-vermelding omdat alle application gateways zich in het domein azure.com.

2. De Azure DNS-server retourneert het IP-adres aan de client, die de front-end-IP-adres van de toepassingsgateway.

3. De toepassingsgateway accepteert verkeer dat binnenkomt op een of meer listeners. Een listener is een logische eenheid die wordt gecontroleerd voor verbindingsaanvragen. Het geconfigureerd met een front-end-IP-adres, het protocol en het poortnummer voor verbindingen van clients naar de application gateway.

4. Als een web application firewall (WAF) gebruikt wordt, controleert de application gateway de aanvraagheaders en de hoofdtekst, indien aanwezig, met de WAF-regels. Deze actie wordt gecontroleerd of de aanvraag geldige aanvraag of een beveiligingsrisico. Als de aanvraag geldig is, wordt dat doorgestuurd naar de back-end. Als de aanvraag is niet geldig, wordt het geblokkeerd als een beveiligingsrisico.

Azure Application Gateway kan worden gebruikt als een toepassing met interne load balancer of als een internetgerichte toepassing load balancer. Een internetgerichte application gateway maakt gebruik van openbare IP-adressen. De DNS-naam van een internetgerichte application gateway is openbaar omgezette naar het openbare IP-adres. Als gevolg hiervan kunnen internetgerichte Toepassingsgateways clientaanvragen doorsturen naar het internet.

Interne Toepassingsgateways maken gebruik van alleen privé IP-adressen. De DNS-naam van een interne applicatie-gateway is een openbaar omgezette het particuliere IP-adres. Interne load balancers kunnen daarom alleen aanvragen van clients die toegang heeft met een virtueel netwerk voor application gateway routeren.

Zowel internetgerichte en interne applicatie-gateway routeren van aanvragen naar de back-endservers met behulp van privé-IP-adressen. Back-endservers hoeft geen openbare IP-adressen voor het ontvangen van aanvragen van een interne of een internetgerichte application-gateway.

## <a name="how-an-application-gateway-routes-a-request"></a>Hoe een application gateway routeert een aanvraag

Als een aanvraag ongeldig is of een WAF niet wordt gebruikt, evalueert de toepassingsgateway de routeringsregel aanvraag die is gekoppeld aan de listener. Hiermee bepaalt welke back-endpool voor het routeren van de aanvraag voor het.

Regels worden verwerkt in de volgorde die worden weergegeven in de portal. De toepassingsgateway op basis van de regel voor het doorsturen van aanvraag, bepaalt of voor het routeren van alle aanvragen op de listener voor een specifieke back-endpool, route aanvragen naar verschillende back-endpools op basis van de URL-pad of de aanvragen omleiden naar een andere poort of externe site.

Wanneer de toepassingsgateway selecteert de back-endpool, stuurt de aanvraag naar een van de endservers in orde back-in de groep (y.y.y.y). De status van de server wordt bepaald door een statustest. Als de back-endpool meerdere servers bevat, gebruikt de toepassingsgateway een round robin-algoritme voor het routeren van de aanvragen tussen servers in orde. Deze belasting verdeelt de aanvragen op de servers.

Nadat de toepassingsgateway wordt vastgesteld dat de back-endserver, wordt een nieuwe TCP-sessie geopend met de back-endserver op basis van HTTP-instellingen. HTTP-instellingen opgeven voor het protocol, poort en andere route-gerelateerde instellingen die nodig zijn voor een nieuwe sessie starten met de back-endserver.

De poort en protocol dat wordt gebruikt in de HTTP-instellingen te bepalen of het verkeer tussen de application gateway- en back-end servers (dus uitvoeren van end-to-end-SSL), worden versleuteld of niet versleuteld is.

Wanneer een application gateway de oorspronkelijke aanvraag naar de back-endserver verzendt, houdt deze geen aangepaste configuratie die in de HTTP-instellingen met betrekking tot het overschrijven van de hostnaam, het pad en de-protocol. Deze actie wordt bijgehouden cookies gebaseerde sessieaffiniteit, verbinding verwerkingsstop van verbindingen, hostnaam selectie van de back-end, enzovoort.

Een interne application gateway maakt gebruik van alleen privé IP-adressen. De DNS-naam van een interne applicatie-gateway is omgezet in het particuliere IP-adres. Als gevolg hiervan kunnen interne load balancers alleen aanvragen van clients met toegang tot het virtuele netwerk voor application gateway routeren.

 >[!NOTE]
 >Beide Toepassingsgateways internetgerichte en interne voor het routeren van aanvragen naar de back-endservers met behulp van privé-IP-adressen. Dit gebeurt wanneer uw back-end-pool resource een privé IP-adres, VM NIC-configuratie of een intern omgezet adres bevat. Als de back-endpool:
> - **Is een openbaar eindpunt**, de application gateway maakt gebruik van de front-end openbaar IP-adres om de server te bereiken. Als er geen een openbare IP-adres voor front-end, is een toegewezen voor de uitgaande externe connectiviteit.
> - **Een intern omzetbare FQDN-naam of een privé IP-adres bevat**, de application gateway stuurt de aanvraag door naar de back-endserver met behulp van de instantie privé IP-adressen.
> - **Een extern eindpunt of een extern omzetbare FQDN-naam bevat**, de application gateway stuurt de aanvraag door naar de back-endserver via het openbare IP-adres voor front-end. De DNS-omzetting is gebaseerd op een privé-DNS-zone of een aangepaste DNS-server, als geconfigureerd of de standaard Azure verschafte DNS wordt gebruikt. Als er geen een openbare IP-adres voor front-end, is een toegewezen voor de uitgaande externe connectiviteit.

### <a name="modifications-to-the-request"></a>Wijzigingen in de aanvraag

Een application gateway voegt vier extra kopteksten die moeten alle aanvragen voordat de aanvragen voor de back-end wordt doorgestuurd. Deze headers zijn x doorgestuurd voor, x-doorgestuurd-protocol, x-doorgestuurd-poort en x-oorspronkelijke-host. De indeling voor x-doorgestuurd-voor-header is een door komma's gescheiden lijst met IP: poort.

De geldige waarden voor de x-doorgestuurd-protocol zijn HTTP of HTTPS. X-doorgestuurd-poort geeft de poort waarop de aanvraag de application gateway is bereikt. X-oorspronkelijke-host-header bevat de oorspronkelijke host-header waarmee de aanvraag is ontvangen. Deze header is handig in de integratie van Azure-website, waar de binnenkomende host-header is gewijzigd voordat het verkeer wordt doorgestuurd naar de back-end. Als sessieaffiniteit als een optie is ingeschakeld, wordt het een affiniteitscookie-gateway beheerde.

U kunt application-gateway voor het wijzigen van headers met behulp van [Herschrijf de HTTP-headers](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) of te wijzigen van de URI-pad met behulp van een pad-override-instelling. Alle inkomende aanvragen zijn echter geproxied naar de back-end, tenzij geconfigureerd om dit te doen.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over application gateway-onderdelen](application-gateway-components.md)
