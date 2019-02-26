---
title: How Azure Application Gateway Works
description: In dit artikel bevat informatie over hoe u aan de Application Gateway werkt
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: 06206ececcb1a51da402c4232f19801793c1cd4a
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56807334"
---
# <a name="how-application-gateway-works"></a>De werking van Application Gateway

Application Gateway is een cloud Application Delivery Controller met laag 7 (HTTP) taakverdeling die kunt u voor het beheren van het webverkeer te genereren voor al uw servers. Bovendien biedt het ook mogelijkheden voor Web Application Firewall (WAF) die gecentraliseerde beveiliging van uw web-services tegen algemene webaanvallen en beveiligingsproblemen bieden.

Wanneer een client een HTTP-aanvraag aan de toepassingsgateway, fungeert als een omgekeerde proxy en stuurt het verkeer naar de back-endservers op basis van uw configuratie. Bovendien de Application Gateway ook controleert de status van de back-endservers en zorgt ervoor dat verkeer wordt gerouteerd alleen op in orde back-ends.

![how-application-gateway-works](.\media\how-application-gateway-works\how-application-gateway-works.png)

## <a name="how-request-is-accepted"></a>Hoe de aanvraag is geaccepteerd

Voordat een client een aanvraag naar uw Application Gateway verzendt, wordt deze omgezet domeinnaam van de toepassingsgateway met behulp van een Domain Name System (DNS)-server. De DNS-vermelding wordt beheerd door Azure, omdat de Gateways van uw toepassing in het domein azure.com. De Azure DNS-server retourneert het IP-adres aan de client, dit is de *front-end-IP-adres* van de toepassingsgateway. De toepassingsgateway accepteert verkeer dat binnenkomt op een of meer *listeners*. Een listener is een proces waarmee wordt gecontroleerd voor verbindingsaanvragen. Deze is geconfigureerd met een fronted IP-adres, het protocol en het poortnummer voor verbindingen van clients naar de Application Gateway. Als WAF is ingeschakeld, Application Gateway controleert of de aanvraagheaders en de hoofdtekst (indien aanwezig) op basis van de *WAF-regels* om te bepalen of de aanvraag een geldige aanvraag - in welk geval de verzameling wordt doorgestuurd naar de back-end- of een beveiligingsrisico welk geval de aanvraag wordt geblokkeerd.  

## <a name="how-request-is-routed"></a>Hoe de aanvraag wordt gerouteerd

Als de aanvraag moet geldige is gevonden (of als WAF is niet ingeschakeld), wordt de *routeringsregel aanvragen* die zijn gekoppeld aan de *listener* wordt geëvalueerd om vast te stellen de *back-endpool* aan de aanvraag is om te worden gerouteerd. Regels worden verwerkt in de volgorde die wordt weergegeven in de portal. Op basis van de *aanvragen routeringsregel* configureren van de toepassingsgateway besluit of voor het routeren van alle aanvragen op de listener voor een specifieke back-endpool of ze naar verschillende back endpools, afhankelijk van op het URL-pad of doorsturen *aanvragen omleiden* naar een andere poort of een externe site

Zodra een *back-end* *pool* is gekozen, Application Gateway de aanvraag verzendt naar een van de back-endservers geconfigureerd in de groep die is in orde (y.y.y.y). De status van de server wordt bepaald door een *statustest*. Als de back-endpool meer dan één server bevat, Application Gateway maakt gebruik van de round robin-algoritme om te routeren van de aanvragen tussen de servers in orde, dus wordt de taakverdeling van de aanvragen op de servers.

Nadat u een back-endserver is bepaald, Application Gateway wordt een nieuwe TCP-sessie geopend met de back-endserver op basis van de configuratie in de *HTTP-instellingen*. De *HTTP-instellingen* is een onderdeel dat Hiermee geeft u het protocol, poort en andere routering met betrekking tot instelling die is vereist voor het tot stand brengen van een nieuwe sessie met de back-endserver. De poort en protocol dat wordt gebruikt in de HTTP-instellingen te bepalen of het verkeer tussen de Application Gateway en de back-endservers wordt versleuteld, dus het uitvoeren van end-to-end SSL of niet-versleuteld. Tijdens het verzenden van de oorspronkelijke aanvraag naar de back-endserver, Application Gateway zich houdt aan alle aangepaste configuratie die in de HTTP-instellingen met betrekking tot het overschrijven van de hostnaam, het pad, het protocol. beheren van cookies gebaseerde sessieaffiniteit, verwerkingsstop, verzamelen de hostnaam van de back-end, enzovoort.

De toepassingsgateway voegt ook deze drie standaard HTTP-*-headers: x doorgestuurd voor, x-doorgestuurd-protocol en x-doorgestuurd-poort in de aanvraag doorgestuurd naar de back-end.

Zodra de back-endserver de aanvraag verwerkt en een HTTP-antwoord met de pagina-inhoud naar de Application Gateway stuurt, verzendt de toepassingsgateway het antwoord op de client wanneer de pagina wordt weergegeven in de browser.

## <a name="application-load-balancing-type"></a>Load Balancing toepassingstype

Als een toepassing met interne load balancer of een internetgerichte toepassing load balancer kunt u de toepassingsgateway. Een internetgerichte Application Gateway biedt openbare IP-adressen. De DNS-naam van een internetgerichte Application Gateway is openbaar omgezette naar het openbare IP-adres. Internetgerichte Application Gateways kunnen daarom aanvragen van clients worden gerouteerd via Internet.

Een interne Application-Gateway heeft alleen privé IP-adres. De DNS-naam van een interne Application-Gateway is intern worden opgelost als de privé IP-adres. Interne load balancers kunnen daarom alleen aanvragen van clients die toegang heeft met het VNET versturen voor Application Gateway.

Houd er rekening mee dat zowel internetgerichte en interne Toepassingsgateways routeren van aanvragen naar uw back-endservers met behulp van privé-IP-adressen. Als uw back-end-pool resource bevat een privé IP-adres, VM NIC-configuratie of een intern omgezet adres, en als uw back endpool is een openbaar eindpunt, Application Gateway maakt gebruik van de front-end openbaar IP-adres om de server te bereiken. Als u een openbaar IP-adres voor front-end nog niet hebt ingericht, wordt een toegewezen voor de uitgaande externe connectiviteit.

## <a name="next-steps"></a>Volgende stappen

Nadat u meer over de werking van Application Gateway, gaat u naar [Application Gateway-onderdelen](application-gateway-components.md) om te begrijpen van de onderdelen in meer informatie.
