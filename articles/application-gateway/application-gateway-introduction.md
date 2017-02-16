---
title: Inleiding tot Application Gateway | Microsoft Docs
description: Deze pagina geeft een overzicht van de Application Gateway-service voor de taakverdeling in laag 7, inclusief de grootte van de gateway, HTTP-taakverdeling, op cookies gebaseerde sessie-affiniteit en SSL-offload.
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: b37a2473-4f0e-496b-95e7-c0594e96f83e
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 3c1fda0661d577e3042f1ad7103017eed74704ad
ms.openlocfilehash: bb979cde449a7e6d8038f6ea392c3529ced29853


---
# <a name="application-gateway-overview"></a>Overzicht van Application Gateway

## <a name="what-is-application-gateway"></a>Wat is Application Gateway

Microsoft Azure Application Gateway biedt een ADC (Application Delivery Controller) als een service en biedt daarmee veel mogelijkheden van laag 7 voor taakverdeling voor uw toepassing. Hiermee kunnen klanten de productiviteit van webfarms optimaliseren door het offloaden van CPU intensieve SSL-beëindiging met de Application Gateway. Het bevat ook andere routeringsmogelijkheden voor laag 7, met inbegrip van round robin-distributie van inkomend verkeer, cookies op basis van sessieaffiniteit, URL-pad op basis van routering en de mogelijkheid voor het hosten van meerdere websites achter één Application Gateway. Application Gateway biedt ook een WAF (Web Application Firewall) die uw toepassing beschermt tegen de meeste van de veelvoorkomende web beveiligingslekken uit de OWASP top 10. Application Gateway kan worden geconfigureerd als op internet gerichte gateway, interne enige gateway of een combinatie van beide. Application Gateway wordt volledig door Azure beheerd en is zeer schaalbaar en maximaal beschikbaar. Het biedt een uitgebreide verzameling diagnostische gegevens en logboekregistratiemogelijkheden voor betere beheerbaarheid. Application Gateway werkt met virtuele machines, cloudservices en intern of extern gerichte webtoepassingen.

Application Gateway is een speciaal virtueel apparaat voor uw toepassing en bestaat uit meerdere workerinstanties voor schaalbaarheid en hoge beschikbaarheid. Wanneer u een Application Gateway maakt, wordt er een eindpunt (openbare VIP of interne ILB IP) gekoppeld en voor inkomend netwerkverkeer gebruikt. Deze VIP of ILB IP wordt geleverd door Azure Load Balancer en werkt op transportniveau (TCP/UDP) en laat al het inkomende netwerkverkeer verdelen naar de Application Gateway-workerinstanties. De Application Gateway routeert het HTTP/HTTPS-verkeer vervolgens op basis van de configuratie, of het nu een virtuele machine, een cloudservice, intern of een extern IP-adres is. Raadpleeg voor de SLA en prijzen verwijzen de pagina's [SLA](https://azure.microsoft.com/support/legal/sla/) en [Prijzen](https://azure.microsoft.com/pricing/details/application-gateway/).

## <a name="features"></a>Functies

Application Gateway ondersteunt momenteel levering van laag 7-toepassingen met de volgende functies:

* **[Web Application Firewall (Preview)](application-gateway-webapplicationfirewall-overview.md)**: de firewall-webtoepassing (WAF) in Azure Application Gateway beschermt webtoepassingen tegen algemene webgebaseerde aanvallen, zoals SQL-injectie, XSS-aanvallen (cross-site scripting) en sessiekapingen.
* **HTTP-taakverdeling**: Application Gateway biedt round robin-taakverdeling. Taakverdeling wordt uitgevoerd op laag 7 en wordt alleen gebruikt voor HTTP(S)-verkeer.
* **Sessieaffiniteit op basis van cookies**: deze functie is handig als u een gebruikerssessie op dezelfde back-end wilt behouden. Met behulp van de gatewaybeheerde cookies kan de Application Gateway het daarop volgende verkeer van een gebruikerssessie naar dezelfde back-end leiden voor verwerking. Deze functie is belangrijk wanneer de sessiestatus lokaal wordt opgeslagen op de back-endserver voor een gebruikerssessie.
* **[Secure Sockets Layer-offload (SSL)](application-gateway-ssl-arm.md)**: deze functie neemt de intensieve taak van het versleutelen van HTTPS-verkeer over van uw webservers. Door de SSL-verbinding bij de Application Gateway te beëindigen en de aanvraag niet versleuteld naar de server door te sturen, hoeft de webserver niet meer te ontsleutelen.  De Application Gateway versleutelt het antwoord opnieuw voordat het naar de client wordt verstuurd. Deze functie is handig wanneer de back-end zich in hetzelfde beveiligde virtuele netwerk bevindt als de Application Gateway in Azure.
* **[End-to-end-SSL](application-gateway-backend-ssl.md)**: Application Gateway ondersteunt de end-to-endversleuteling van verkeer. Application Gateway doet dit door de SSL-verbinding bij de toepassingsgateway te beëindigen. De gateway past vervolgens de routeringsregels op het verkeer toe, versleutelt het pakket opnieuw en stuurt het pakket naar de juiste back-end op basis van de gedefinieerde routeringsregels. Reacties van de webserver ondergaan hetzelfde proces terug naar de eindgebruiker.
* **[URL-gebaseerde routering van inhoud](application-gateway-url-route-overview.md)**: deze functie biedt de mogelijkheid om verschillende back-endservers voor verschillend verkeer te gebruiken. Netwerkverkeer voor een map op de webserver of een CDN kan worden doorgestuurd naar een andere back-end, waardoor overbodige belasting van back-ends die geen specifieke inhoud verwerken, wordt voorkomen.
* **[Routering van meerdere sites](application-gateway-multi-site-overview.md)**: met Application Gateway kunt u maximaal 20 websites op een enkele toepassingsgateway consolideren.
* **[Websocket-ondersteuning](application-gateway-websocket.md)**: een andere fantastische functie van de Application Gateway is de systeemeigen ondersteuning voor Websocket.
* **[Statuscontrole](application-gateway-probe-overview.md)**: Application Gateway biedt standaard statuscontrole van back-endresources en aangepaste tests om te controleren op specifiekere scenario's.
* **[Geavanceerde diagnostische gegevens](application-gateway-diagnostics.md)**: Application Gateway biedt volledige diagnostische gegevens en toegangslogboeken. Er zijn firewalllogboeken beschikbaar voor Application Gateway-resources waarvoor WAF is ingeschakeld.

## <a name="benefits"></a>Voordelen

Application Gateway is nuttig voor:

* Toepassingen waarvoor aanvragen van dezelfde gebruiker/clientsessie zijn vereist om dezelfde virtuele back-endmachine te bereiken. Voorbeelden van deze toepassingen zijn winkelwagen-apps en webmailservers.
* Toepassingen die webserverfarms willen ontdoen van bewerkingen met SSL-beëindiging.
* Toepassingen, zoals een netwerk voor contentlevering, waarvoor meerdere HTTP-aanvragen op dezelfde langlopende TCP-verbinding moeten worden doorgestuurd of verdeeld naar andere back-endservers.
* Toepassingen die ondersteuning bieden voor websocket-verkeer
* Webtoepassingen beveiligen tegen veelvoorkomende webgebaseerde aanvallen, zoals SQL-injectie, SSX-aanvallen (cross-site scripting) en sessiekapingen.

Met Application Gateway-taakverdeling als een door Azure beheerde service kan een load balancer van laag 7 worden ingericht achter de Azure-softwaretaakverdeler. Traffic Manager kan worden gebruikt voor het voltooien van het scenario, zoals te zien is op de volgende afbeelding waar Traffic Manager omleiding en beschikbaarheid biedt van verkeer naar meerdere Application Gateway-resources in verschillende gebieden, terwijl Application Gateway regio-overkoepelende taakverdeling voor laag 7 biedt. Een voorbeeld van dit scenario vindt u in: [Load balancing-services gebruiken in de Azure-cloud](../traffic-manager/traffic-manager-load-balancing-azure.md)

![scenario voor Traffic Manager en Application Gateway](./media/application-gateway-introduction/tm-lb-ag-scenario.png)

[!INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="gateway-sizes-and-instances"></a>Gateway-grootten en -exemplaren

Application Gateway wordt momenteel aangeboden in drie grootten: **klein**, **middelgroot** en **groot**. Kleine exemplaargrootten zijn bedoeld voor het ontwikkelen en testen van scenario's.

Er zijn momenteel twee SKU's voor Application Gateway: **WAF** en **Standard**.

U kunt maximaal 50 toepassingsgateways per abonnement maken. Elke toepassingsgateway kan maximaal 10 exemplaren hebben. Elke toepassingsgateway kan bestaan uit 20 HTTP-listeners. Zie [Servicelimieten voor Application Gateway](../azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits) voor een volledige lijst van toepassingsgateway-limieten.

In de volgende tabel staan gemiddelde doorvoerprestaties voor elk toepassingsgateway-exemplaar:

| Antwoord van de back-endpagina | Klein | Middelgroot | Groot |
| --- | --- | --- | --- |
| 6K |7,5 Mbps |13 Mbps |50 Mbps |
| 100K |35 Mbps |100 Mbps |200 Mbps |

> [!NOTE]
> Deze waarden zijn geschatte waarden voor de doorvoer van een toepassingsgateway. De werkelijke doorvoer hangt af van verschillende details van de omgeving, zoals de gemiddelde paginagrootte, locatie van back-endexemplaren en de verwerkingstijd voor een pagina. Voor nauwkeurige prestatiecijfers moet u uw eigen tests uitvoeren. Deze waarden worden alleen geboden als richtlijn voor de capaciteitsplanning.

## <a name="health-monitoring"></a>Statuscontrole

Azure Application Gateway bewaakt automatisch de status van de back-endexemplaren via basistests of aangepaste statustests. Door statustests te gebruiken, zorgt u ervoor dat alleen veilige hosts op verkeer reageren. Zie voor meer informatie [Overzicht van Application Gateway-statuscontrole](application-gateway-probe-overview.md).

## <a name="configuring-and-managing"></a>Configuratie en beheer

Application Gateway kan voor het eindpunt een openbaar IP-adres, een privé IP-adres of beide hebben wanneer het is geconfigureerd. Application Gateway wordt in een virtueel netwerk geconfigureerd in een eigen subnet. Het subnet dat voor Application Gateway is gemaakt of wordt gebruikt, mag geen andere typen resources bevatten. De enige andere toegestane resources in het subnet zijn andere toepassingsgateways. Voor het beveiligen van uw back-endresources kunnen de back-endservers zich in een ander subnet in hetzelfde virtuele netwerk bevinden als de toepassingsgateway. Dit aanvullende subnet is niet vereist voor de back-endtoepassingen. Zolang de toepassingsgateway het IP-adres kan bereiken, kan Application Gateway ADC-mogelijkheden bieden voor de back-endservers.

U kunt een toepassingsgateway maken en beheren met REST-API's, PowerShell-cmdlets, Azure CLI of [Azure Portal](https://portal.azure.com/).

## <a name="next-steps"></a>Volgende stappen

Nadat u informatie hebt opgedaan over Application Gateway, kunt u [een toepassingsgateway maken](application-gateway-create-gateway-portal.md) of [een SSL-offload voor een toepassingsgateway maken](application-gateway-ssl-arm.md) voor taakverdeling van HTTPS-verbindingen.

Voor meer informatie over het maken van een toepassingsgateway met behulp van URL-gebaseerd routeren van content gaat u naar [Een toepassingsgateway maken met behulp van URL-gebaseerde routering](application-gateway-create-url-route-arm-ps.md).



<!--HONumber=Jan17_HO3-->


