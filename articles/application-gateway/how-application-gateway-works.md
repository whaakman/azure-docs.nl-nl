---
title: How Azure Application Gateway Works
description: In dit artikel bevat informatie over hoe u aan de Application Gateway werkt
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: ef07def377b74fb74d57372f471efcf48fcf7aa2
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57881092"
---
# <a name="how-application-gateway-works"></a>De werking van Application Gateway

In dit artikel wordt uitgelegd hoe de toepassingsgateway accepteert de binnenkomende aanvragen en stuurt deze door naar de back-end.

![how-application-gateway-works](./media/how-application-gateway-works/how-application-gateway-works.png)

## <a name="how-a-request-is-accepted"></a>Hoe een aanvraag is geaccepteerd

Voordat een client een aanvraag naar uw application gateway verzendt, wordt deze omgezet domeinnaam van de toepassingsgateway met behulp van een Domain Name System (DNS)-server. De DNS-vermelding wordt beheerd door Azure, omdat de Toepassingsgateways van uw in het domein azure.com. De Azure DNS-server retourneert het IP-adres aan de client, dit is de *front-end-IP-adres* van de toepassingsgateway. De toepassingsgateway accepteert verkeer dat binnenkomt op een of meer *listeners*. Een listener is een logische eenheid die wordt gecontroleerd voor verbindingsaanvragen. Deze is geconfigureerd met een fronted IP-adres, het protocol en het poortnummer voor verbindingen van clients naar de application gateway. Als de Web Application Firewall (WAF) is ingeschakeld, Application Gateway controleert of de aanvraagheaders en de hoofdtekst (indien aanwezig) op basis van de *WAF-regels* om te bepalen of de aanvraag een geldige aanvraag - in dat geval wordt deze doorgestuurd naar de back-end- of een beveiligingsrisico in dat geval de aanvraag wordt geblokkeerd.  

Application gateway kan worden gebruikt als een toepassing met interne load balancer of een internetgerichte toepassing load balancer. Een internetgerichte application gateway biedt openbare IP-adressen. De DNS-naam van een internetgerichte application gateway is openbaar omgezette naar het openbare IP-adres. Daarom kunnen internetgerichte Toepassingsgateways aanvragen van clients versturen via Internet. Interne Toepassingsgateways hebben alleen persoonlijke IP-adres. De DNS-naam van een interne applicatie-gateway is een openbaar omgezette het particuliere IP-adres. Interne load balancers kunnen daarom alleen aanvragen van clients die toegang heeft met het VNET voor application gateway routeren. Zowel internetgerichte en interne Toepassingsgateways routeren van aanvragen naar uw back-endservers met behulp van privé-IP-adressen. Uw back-endservers hoeft daarom geen openbare IP-adressen voor het ontvangen van aanvragen van een interne of een internetgerichte Application-Gateway.

## <a name="how-a-request-is-routed"></a>Hoe een aanvraag wordt gerouteerd

Als de aanvraag moet geldige is gevonden (of als WAF is niet ingeschakeld), wordt de *routeringsregel aanvragen* die zijn gekoppeld aan de *listener* wordt geëvalueerd om vast te stellen de *back-endpool* aan de aanvraag is om te worden gerouteerd. Regels worden verwerkt in de volgorde die wordt weergegeven in de portal. Op basis van de *aanvragen routeringsregel* configureren van de toepassingsgateway besluit of voor het routeren van alle aanvragen op de listener voor een specifieke back-endpool of ze naar verschillende back endpools, afhankelijk van op het URL-pad of doorsturen *aanvragen omleiden* naar een andere poort of een externe site

Zodra een *back-end* *pool* is gekozen, application-gateway de aanvraag verzendt naar een van de back-endservers geconfigureerd in de groep die is in orde (y.y.y.y). De status van de server wordt bepaald door een *statustest*. Als de back-endpool meer dan één server bevat, application gateway maakt gebruik van de round robin-algoritme om te routeren van de aanvragen tussen de servers in orde, dus wordt de taakverdeling van de aanvragen op de servers.

Nadat u een back-endserver is bepaald, application-gateway wordt een nieuwe TCP-sessie geopend met de back-endserver op basis van de configuratie in de *HTTP-instellingen*. De *HTTP-instellingen* is een onderdeel dat Hiermee geeft u het protocol, poort en andere routering met betrekking tot instelling die is vereist voor het tot stand brengen van een nieuwe sessie met de back-endserver. De poort en protocol dat wordt gebruikt in de HTTP-instellingen te bepalen of het verkeer tussen de gateway en back-end-toepassingsservers wordt versleuteld, dus het uitvoeren van end-to-end SSL of niet-versleuteld. Tijdens het verzenden van de oorspronkelijke aanvraag naar de back-endserver, application-gateway zich houdt aan alle aangepaste configuratie die in de HTTP-instellingen met betrekking tot het overschrijven van de hostnaam, het pad, het protocol. beheren van cookies gebaseerde sessieaffiniteit, verwerkingsstop, verzamelen de hostnaam van de back-end, enzovoort.

Een interne Application-Gateway heeft alleen privé IP-adres. De DNS-naam van een interne Application-Gateway is intern worden opgelost als de privé IP-adres. Interne load balancers kunnen daarom alleen aanvragen van clients die toegang heeft met het VNET versturen voor Application Gateway.

Houd er rekening mee dat zowel internetgerichte en interne Toepassingsgateways routeren aanvragen naar uw back-endservers met behulp van privé IP-adressen, als uw back-end-pool resource bevat een privé IP-adres, VM NIC-configuratie of een intern omgezet adres, en als uw back endpool is een openbaar eindpunt, Application Gateway maakt gebruik van de front-end openbaar IP-adres om de server te bereiken. Als u een openbaar IP-adres voor front-end nog niet hebt ingericht, wordt een toegewezen voor de uitgaande externe connectiviteit.

### <a name="modifications-to-the-request"></a>Wijzigingen in de aanvraag

Toepassingsgateway voegt 4 extra kopteksten die moeten alle aanvragen voordat de aanvragen voor de back-end wordt doorgestuurd. Deze headers zijn X doorgestuurd voor X-doorgestuurd-protocol, X-doorgestuurd-poort en X-oorspronkelijke-host. De indeling voor x-doorgestuurd-voor-header is een door komma's gescheiden lijst met IP: poort. De geldige waarden voor de x-doorgestuurd-protocol zijn HTTP of HTTPS. X-doorgestuurd-poort geeft de poort waarop de aanvraag bij de application gateway is bereikt. X-oorspronkelijke-host-header bevat de oorspronkelijke host-header waarmee de aanvraag is ontvangen. Deze header is handig in scenario's zoals de integratie van Azure-Website, waar de binnenkomende host-header is gewijzigd voordat het verkeer wordt doorgestuurd naar de back-end. (Optioneel) als de sessie-affiniteit is ingeschakeld, is worden een gateway beheerde affiniteitscookie ingevoegd. 

U kunt ook configureren met application-gateway als u wilt wijzigen met behulp van headers [Herschrijf de HTTP-headers](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) of wijzigen van URI-pad met pad onderdrukking instellen. Maar tenzij ze zijn geconfigureerd om dit te doen, alle inkomende aanvragen worden geproxied als de back-end.


## <a name="next-steps"></a>Volgende stappen

Nadat u meer over de werking van application gateway, Zie [Application gateway-onderdelen](application-gateway-components.md) om te begrijpen van de onderdelen in meer informatie.
