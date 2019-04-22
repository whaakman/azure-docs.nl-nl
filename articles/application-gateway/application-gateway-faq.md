---
title: Veelgestelde vragen over Azure Application Gateway
description: Vind antwoorden op veelgestelde vragen over Azure Application Gateway.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 3/20/2019
ms.author: victorh
ms.openlocfilehash: 3c8a2fe9f4486fe4d33754b58f4e7ebec1b3252d
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682947"
---
# <a name="frequently-asked-questions-about-application-gateway"></a>Veelgestelde vragen over Application Gateway

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="general"></a>Algemeen

### <a name="what-is-application-gateway"></a>Wat is Application Gateway?

Azure Application Gateway biedt application delivery controller (ADC) als een service. Het biedt verschillende layer 7 load balancing mogelijkheden voor uw toepassingen. Deze service is maximaal beschikbare, schaalbare en volledig beheerd door Azure.

### <a name="what-features-does-application-gateway-support"></a>Welke functies biedt ondersteuning voor Application Gateway?

Application Gateway biedt ondersteuning voor automatisch schalen, SSL-offloading, en end-to-end SSL, een web application firewall (WAF), cookies gebaseerde sessieaffiniteit, URL-pad gebaseerde routering, meerdere locaties die als host fungeert, en andere functies. Zie voor een volledige lijst van ondersteunde functies, [Inleiding tot Application Gateway](application-gateway-introduction.md).

### <a name="how-do-application-gateway-and-azure-load-balancer-differ"></a>Hoe verschillen Application Gateway en Azure Load Balancer?

Application Gateway is een layer 7 load balancer, wat betekent dat het werkt alleen met webverkeer (HTTP, HTTPS, WebSocket en HTTP/2). Deze mogelijkheden, zoals SSL-beëindiging, sessieaffiniteit op basis van een cookie en round robin ondersteunt voor de taakverdeling verkeer. Load Balancer taakverdelingen van verkeer op laag 4 (TCP of UDP).

### <a name="what-protocols-does-application-gateway-support"></a>Welke protocollen ondersteunt Application Gateway?

Application Gateway biedt ondersteuning voor HTTP, HTTPS, HTTP/2 en WebSocket.

### <a name="how-does-application-gateway-support-http2"></a>Hoe biedt Application Gateway ondersteuning voor HTTP/2?

Zie [ondersteuning voor HTTP/2](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http2-support).

### <a name="what-resources-are-supported-as-part-of-a-backend-pool"></a>Welke bronnen worden ondersteund als onderdeel van een back-endpool?

Zie [ondersteund back-endresources](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#backend-pool).

### <a name="in-what-regions-is-application-gateway-available"></a>In welke regio's is de Application Gateway beschikbaar?

Application Gateway is beschikbaar in alle regio's van de globale Azure. Het is ook beschikbaar in [Azure China 21Vianet](https://www.azure.cn/) en [Azure Government](https://azure.microsoft.com/overview/clouds/government/).

### <a name="is-this-deployment-dedicated-for-my-subscription-or-is-it-shared-across-customers"></a>Deze implementatie toegewezen voor mijn abonnement is, of is deze gedeeld met klanten?

Application Gateway is een specifieke implementatie in uw virtuele netwerk.

### <a name="does-application-gateway-support-http-to-https-redirection"></a>Application Gateway biedt ondersteuning voor omleiding van HTTP naar HTTPS?

Omleiding wordt ondersteund. Zie [overzicht van Application Gateway redirect](application-gateway-redirect-overview.md).

### <a name="in-what-order-are-listeners-processed"></a>In welke volgorde worden listeners verwerkt?

Zie de [volgorde van listener voor de verwerking van](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-listeners).

### <a name="where-do-i-find-the-application-gateway-ip-and-dns"></a>Waar vind ik de Application Gateway-IP- en DNS?

Als u een openbaar IP-adres als een eindpunt gebruikt, vindt u de IP-adres en DNS-informatie over de openbare IP-adresresource. Of vinden in de portal, op de overzichtspagina voor de toepassingsgateway. Als u interne IP-adressen, moet u de gegevens op de overzichtspagina vinden.

### <a name="what-are-the-settings-for-keep-alive-timeout-and-tcp-idle-timeout"></a>Wat zijn de instellingen voor Keep-Alive-out en time-out voor inactiviteit van TCP?

 In de Application Gateway v1-SKU is de time-out voor de Keep-Alive 120 seconden. De Keep-Alive-out voor de v2-SKU is 75 seconden. De time-out voor inactiviteit TCP is standaard 4 minuten op de front-end virtuele IP-adres (VIP) van Application Gateway.

### <a name="does-the-ip-or-dns-name-change-over-the-lifetime-of-the-application-gateway"></a>Wordt het IP- of DNS-naam gewijzigd gedurende de levensduur van de application gateway?

Het VIP kunt wijzigen als u stopt en de toepassingsgateway start. Maar de DNS-naam die is gekoppeld aan de toepassingsgateway niet gewijzigd tijdens de levensduur van de gateway. Omdat de DNS-naam niet wijzigen, moet u een CNAME-alias gebruiken en wijs het DNS-adres van de toepassingsgateway.

### <a name="does-application-gateway-support-static-ip"></a>Application Gateway biedt ondersteuning voor statische IP-adres?

Ja, de v2-SKU van Application Gateway biedt ondersteuning voor statische openbare IP-adressen. De v1-SKU biedt ondersteuning voor statische interne IP-adressen.

### <a name="does-application-gateway-support-multiple-public-ips-on-the-gateway"></a>Application Gateway ondersteunt de meerdere openbare IP-adressen op de gateway?

Een application gateway biedt ondersteuning voor slechts één openbaar IP-adres.

### <a name="how-large-should-i-make-my-subnet-for-application-gateway"></a>Hoe groot ik moet mijn subnet voor Application Gateway?

Zie [overwegingen bij de grootte van de Application Gateway-subnet](https://docs.microsoft.com/azure/application-gateway/configuration-overview#size-of-the-subnet).

### <a name="can-i-deploy-more-than-one-application-gateway-resource-to-a-single-subnet"></a>Kan ik meer dan één Application Gateway-resource met één subnet implementeren?

Ja. U kunt een andere unieke Application Gateway-resource aan een bestaand subnet met een andere resource van Application Gateway inrichten naast meerdere exemplaren van een bepaalde implementatie van de toepassingsgateway.

Één subnet kan niet ondersteunen zowel Standard_v2 als standaard Application-Gateway samen.

### <a name="does-application-gateway-support-x-forwarded-for-headers"></a>Application Gateway biedt ondersteuning voor x doorgestuurd voor headers?

Ja. Zie [wijzigingen in een aanvraag](https://docs.microsoft.com/azure/application-gateway/how-application-gateway-works#modifications-to-the-request).

### <a name="how-long-does-it-take-to-deploy-an-application-gateway-will-my-application-gateway-work-while-its-being-updated"></a>Hoe lang duurt het implementeren van een application gateway? Werkt mijn toepassingsgateway terwijl deze wordt bijgewerkt?

Nieuwe Application Gateway v1-SKU-implementaties kunnen maximaal 20 minuten duren om in te richten. Wijzigingen voor de instantiegrootte of aantal verstorende niet en de gateway blijft actief gedurende deze tijd.

Implementaties die gebruikmaken van de v2-SKU kunnen tot 6 minuten duren om in te richten.

### <a name="can-i-use-exchange-server-as-a-backend-with-application-gateway"></a>Kan ik Exchange-Server gebruiken als een back-end met Application Gateway?

Nee. Application Gateway biedt geen ondersteuning voor protocollen zoals SMTP- en IMAP POP3-e-mailbericht. 

## <a name="performance"></a>Prestaties

### <a name="how-does-application-gateway-support-high-availability-and-scalability"></a>Hoe ondersteunt Application Gateway hoge beschikbaarheid en schaalbaarheid?

De v1-SKU van Application Gateway biedt ondersteuning voor scenario's voor hoge beschikbaarheid tijdens de implementatie van twee of meer exemplaren. Deze instanties verdeelt Azure over update- en foutdomeinen domeinen om ervoor te zorgen dat exemplaren niet alle fouten op hetzelfde moment. De v1-SKU biedt ondersteuning voor schaalbaarheid door meerdere exemplaren van dezelfde gateway voor het delen van de belasting toe te voegen.

De v2-SKU zorgt automatisch voor dat nieuwe exemplaren worden verdeeld over foutdomeinen en updatedomeinen. Als u zoneredundantie kiest, worden ook de nieuwste exemplaren verdeeld in meerdere beschikbaarheidszones zonegebonden fout tolerantie bieden.

### <a name="how-do-i-achieve-a-dr-scenario-across-datacenters-by-using-application-gateway"></a>Hoe kan ik een herstel na noodgevallen in datacenters met behulp van Application Gateway?

Traffic Manager gebruiken voor het verkeer verdelen over meerdere Toepassingsgateways in verschillende datacenters.

### <a name="does-application-gateway-support-autoscaling"></a>Application Gateway biedt ondersteuning voor automatisch schalen?

Ja, de v2-SKU van Application Gateway biedt ondersteuning voor automatisch schalen. Zie voor meer informatie, [automatisch schalen en zone-redundante Application Gateway (openbare preview)](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant).

### <a name="does-manual-scale-up-or-scale-down-cause-downtime"></a>Handmatig schalen wordt omhoog of omlaag oorzaak downtime schalen?

Nee. Exemplaren worden verdeeld over upgrade-domeinen en domeinen met fouten.

### <a name="does-application-gateway-support-connection-draining"></a>Application Gateway ondersteunt verwerkingsstop?

Ja. U kunt de verwerkingsstop van verbindingen als u wilt wijzigen van de leden in een back endpool zonder onderbreking instellen. Deze instelling kunt u doorgaan met het verzenden van bestaande verbindingen op hun vorige bestemming totdat de verbinding wordt gesloten of een configureerbare time-out is verlopen. Er wacht een verwerkingsstop voor verbindingen voor alleen actieve die onderweg zijn verbindingen om te voltooien. Application Gateway is niet op de hoogte van de status van de toepassing-sessie.

### <a name="can-i-change-instance-size-from-medium-to-large-without-disruption"></a>Kan ik instantiegrootte van middelgrote tot grote zonder onderbreking wijzigen?

Ja. Azure verdeeld exemplaren over update- en foutdomeinen domeinen om ervoor te zorgen dat exemplaren niet alle op hetzelfde moment mislukken. Application Gateway biedt ondersteuning voor schalen door meerdere exemplaren van dezelfde gateway voor het delen van de belasting toe te voegen.

## <a name="configuration"></a>Configuratie

### <a name="is-application-gateway-always-deployed-in-a-virtual-network"></a>Application Gateway altijd geïmplementeerd in een virtueel netwerk?

Ja. Application Gateway wordt altijd geïmplementeerd in een subnet van een virtueel netwerk. Dit subnet kan alleen Toepassingsgateways bevatten. Zie voor meer informatie, [virtuele netwerk en subnet vereisten](https://docs.microsoft.com/azure/application-gateway/configuration-overview#azure-virtual-network-and-dedicated-subnet).

### <a name="can-application-gateway-communicate-with-instances-outside-of-its-virtual-network-or-outside-of-its-subscription"></a>Kan Application Gateway communiceren met instanties buiten het virtuele netwerk of buiten het abonnement?

Als u IP-connectiviteit hebt, kan Application Gateway communiceren met instanties buiten het virtuele netwerk dat deel uitmaakt van. Application Gateway kan ook communiceren met instanties buiten het abonnement dat deel uitmaakt. Als u van plan bent om te gebruiken van interne IP-adressen als back-endpoolleden, gebruikt u [peering op virtueel netwerk](../virtual-network/virtual-network-peering-overview.md) of [Azure VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).

### <a name="can-i-deploy-anything-else-in-the-application-gateway-subnet"></a>Kan ik iets anders in het subnet van de gateway implementeren?

Nee. Maar u kunt andere Toepassingsgateways in het subnet implementeren.

### <a name="are-network-security-groups-supported-on-the-application-gateway-subnet"></a>Worden netwerkbeveiligingsgroepen op het subnet van de gateway ondersteund?

Zie [Netwerkbeveiligingsgroepen in het subnet voor Application Gateway](https://docs.microsoft.com/azure/application-gateway/configuration-overview#network-security-groups-on-the-application-gateway-subnet).

### <a name="does-the-application-gateway-subnet-support-user-defined-routes"></a>Het subnet van de gateway biedt ondersteuning voor gebruiker gedefinieerde routes?

Zie [gebruiker gedefinieerde routes die worden ondersteund in de Application Gateway-subnet](https://docs.microsoft.com/azure/application-gateway/configuration-overview#user-defined-routes-supported-on-the-application-gateway-subnet).

### <a name="what-are-the-limits-on-application-gateway-can-i-increase-these-limits"></a>Wat zijn de limieten in Application Gateway? Kan ik deze limieten verhogen?

Zie [Application Gateway beperkt](../azure-subscription-service-limits.md#application-gateway-limits).

### <a name="can-i-simultaneously-use-application-gateway-for-both-external-and-internal-traffic"></a>Kan ik Application Gateway tegelijkertijd gebruiken voor interne en externe verkeer?

Ja. Application Gateway biedt ondersteuning voor een interne IP-adres en één extern IP-adres per toepassingsgateway.

### <a name="does-application-gateway-support-virtual-network-peering"></a>Application Gateway ondersteunt peering op virtueel netwerk?

Ja. Peering op virtueel netwerk kunt verdelen verkeer in andere virtuele netwerken.

### <a name="can-i-talk-to-on-premises-servers-when-theyre-connected-by-expressroute-or-vpn-tunnels"></a>Kan ik vragen stellen met on-premises servers wanneer ze zijn verbonden met ExpressRoute of VPN-tunnels?

Ja, zolang er verkeer is toegestaan.

### <a name="can-one-backend-pool-serve-many-applications-on-different-ports"></a>Kan een back-endpool fungeren voor veel toepassingen op verschillende poorten?

Microservice-architectuur wordt ondersteund. Als u wilt testen op verschillende poorten, moet u meerdere HTTP-instellingen configureren.

### <a name="do-custom-probes-support-wildcards-or-regex-on-response-data"></a>Aangepaste tests bieden ondersteuning jokertekens of reguliere expressie op antwoordgegevens?

Nee. 

### <a name="how-are-routing-rules-processed-in-application-gateway"></a>Hoe worden de regels voor doorsturen in Application Gateway verwerkt?

Zie [volgorde van de regels worden verwerkt](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-rules).

### <a name="for-custom-probes-what-does-the-host-field-signify"></a>Voor aangepaste tests, wat het veld Host overheen?

Het Host-veld geeft de naam voor het verzenden van de test om te wanneer u hebt geconfigureerd met meerdere sites op Application Gateway. Gebruik anders '127.0.0.1'. Deze waarde verschilt van de hostnaam van de virtuele machine. De indeling \<protocol\>://\<host\>:\<poort\>\<pad\>.

### <a name="can-i-whitelist-application-gateway-access-to-only-a-few-source-ips"></a>Kan ik de lijst met toegestane adressen Application Gateway toegang tot slechts enkele bron-IP's?

Ja. Zie [toegang beperken tot specifieke bron-IP-adressen](https://docs.microsoft.com/azure/application-gateway/configuration-overview#whitelist-application-gateway-access-to-a-few-source-ips).

### <a name="can-i-use-the-same-port-for-both-public-facing-and-private-facing-listeners"></a>Kan ik dezelfde poort gebruiken voor zowel openbare als persoonlijke gerichte listeners?

Nee.

## <a name="configuration---ssl"></a>Configuratie - SSL

### <a name="what-certificates-does-application-gateway-support"></a>Welke certificaten biedt ondersteuning voor Application Gateway?

Application Gateway biedt ondersteuning voor zelfondertekende certificaten, certificaten van certificeringsinstanties (CA), uitgebreide validatie (VW)-certificaten en jokertekens-certificaten.

### <a name="what-cipher-suites-does-application-gateway-support"></a>Welke versleutelingssuites Application Gateway biedt ondersteuning voor?

Application Gateway ondersteunt de volgende coderingssuites. 

- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_CBC_SHA
- TLS_DHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_3DES_EDE_CBC_SHA
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA

Zie voor meer informatie over het aanpassen van SSL-opties [SSL configureren voor versies en coderingssuites in Application Gateway](application-gateway-configure-ssl-policy-powershell.md).

### <a name="does-application-gateway-support-reencryption-of-traffic-to-the-backend"></a>Application Gateway biedt ondersteuning voor versleuteling van het verkeer naar de back-end?

Ja. Application Gateway ondersteunt SSL-offload en end-to-end SSL, waarmee u verkeer naar de back-end.

### <a name="can-i-configure-ssl-policy-to-control-ssl-protocol-versions"></a>Kan ik een SSL-beleid voor het besturingselement SSL-protocolversies configureren?

Ja. U kunt Application Gateway voor het weigeren van TLS1.0 TLS1.1 en TLS1.2 configureren. Standaard worden SSL 2.0 en 3.0 zijn al uitgeschakeld en niet kunnen worden geconfigureerd.

### <a name="can-i-configure-cipher-suites-and-policy-order"></a>Kan ik coderingssuites en beleidsvolgorde configureren?

Ja. In Application Gateway, kunt u [coderingssuites configureren](application-gateway-ssl-policy-overview.md). Voor het definiëren van een aangepast beleid, moet u ten minste één van de volgende versleutelingssuites inschakelen. 

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

Application Gateway maakt gebruik van SHA256 op voor het beheer van back-end.

### <a name="how-many-ssl-certificates-does-application-gateway-support"></a>Het aantal SSL-certificaten biedt ondersteuning voor Application Gateway?

Application Gateway biedt ondersteuning voor maximaal 100 SSL-certificaten.

### <a name="how-many-authentication-certificates-for-backend-reencryption-does-application-gateway-support"></a>Het aantal verificatiecertificaten voor back-end versleuteling biedt ondersteuning voor Application Gateway?

Application Gateway ondersteunt maximaal 10 verificatiecertificaten. De standaardwaarde is 5.

### <a name="does-application-gateway-natively-integrate-with-azure-key-vault"></a>Werkt de Application Gateway systeemeigen integratie met Azure Key Vault?

Nee.

### <a name="how-do-i-configure-https-listeners-for-com-and-net-sites"></a>Hoe configureer ik HTTP-luisteraars voor .com en .net sites? 

Voor meerdere op basis van een domein (op een host gebaseerde) routering, kunt u maken met meerdere sites listeners, zet listeners die HTTPS als het protocol gebruiken en de listeners koppelen aan de regels voor doorsturen. Zie voor meer informatie, [meerdere sites hosten met behulp van Application Gateway](https://docs.microsoft.com/azure/application-gateway/multiple-site-overview). 

## <a name="configuration---web-application-firewall-waf"></a>Configuratie - de web application firewall (WAF)

### <a name="does-the-waf-sku-offer-all-the-features-available-in-the-standard-sku"></a>Biedt de WAF-SKU alle functies die beschikbaar zijn in de standaard-SKU?

Ja. WAF ondersteunt alle functies in de standaard-SKU.

### <a name="which-crs-versions-does-application-gateway-support"></a>Welke versies CRS biedt ondersteuning voor Application Gateway?

Application Gateway ondersteunt CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) en CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).

### <a name="how-do-i-monitor-waf"></a>Hoe controleer ik WAF?

Monitor WAF via registratie in diagnoselogboek. Zie voor meer informatie, [diagnostische logboeken en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md).

### <a name="does-detection-mode-block-traffic"></a>Blokkeert detectiemodus verkeer?

Nee. Detectiemodus legt alleen verkeer dat een WAF-regel wordt geactiveerd.

### <a name="can-i-customize-waf-rules"></a>Kan ik de WAF-regels aanpassen?

Ja. Zie voor meer informatie, [regels en regelgroepen van WAF aanpassen](application-gateway-customize-waf-rules-portal.md).

### <a name="what-rules-are-currently-available-for-waf"></a>Welke regels zijn momenteel beschikbaar voor WAF?

Op dit moment ondersteunt CRS WAF [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) en [3.0](application-gateway-crs-rulegroups-rules.md#owasp30). Deze regels bieden basislijn beveiliging tegen de meeste van de top 10 zwakke plekken die Open Web Application Security Project (OWASP) identificeert: 

* Beveiliging tegen SQL-injecties
* Beveiliging tegen cross-site scripting
* Bescherming tegen algemene aanvallen via Internet, zoals opdracht injectie, HTTP-aanvraag smokkelen, HTTP-antwoorden en extern bestand opgenomen aanval
* Beveiliging tegen schendingen van het HTTP-protocol
* Beveiliging tegen afwijkingen van het HTTP-protocol, zoals een gebruikersagent voor de host en Accept-headers die ontbreken
* Beveiliging tegen bots, crawlers en scanners
* Detectie van veelvoorkomende onjuiste configuraties van toepassingen (dat wil zeggen, Apache, IIS, enzovoort)

Zie voor meer informatie, [OWASP top 10 beveiligingslekken](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013).

### <a name="does-waf-support-ddos-protection"></a>Ondersteunt WAF DDoS protection?

Ja. DDoS-beveiliging op het virtuele netwerk waar de application gateway is geïmplementeerd, kunt u inschakelen. Deze instelling zorgt ervoor dat de service Azure DDoS Protection ook de application gateway virtueel IP-adres (VIP beveiligt).

## <a name="diagnostics-and-logging"></a>Diagnostische gegevens en logboekregistratie

### <a name="what-types-of-logs-does-application-gateway-provide"></a>Welke typen logboeken biedt Application Gateway?

Application Gateway biedt drie logboeken: 

* **ApplicationGatewayAccessLog**: De toegang tot het logboek bevat elke aanvraag verzonden naar de frontend van de application gateway. De gegevens bevat en afmelden van de oproepende functie IP, URL aangevraagd, wachttijd van het antwoord, retourcode en bytes. Het toegangslogboek worden verzameld om de 300 seconden. Het bevat één record per toepassingsgateway.
* **ApplicationGatewayPerformanceLog**: Het logboekbestand voor prestaties bevat informatie over de prestaties voor elke application gateway. Informatie omvat de doorvoer in bytes, totaal aantal aanvragen dat plaatsvindt, aantal mislukte aanvragen en in orde is en niet in orde back-end-exemplaren.
* **ApplicationGatewayFirewallLog**: Voor application gateways die u met WAF configureert, bevat de firewall-logboek aanvragen die zijn geregistreerd via de detectiemodus of preventiemodus.

Zie voor meer informatie, [back-end-status, diagnostische logboeken en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md).

### <a name="how-do-i-know-if-my-backend-pool-members-are-healthy"></a>Hoe weet ik of Mijn back-endpoolleden in orde zijn?

Status controleren met behulp van de PowerShell-cmdlet `Get-AzApplicationGatewayBackendHealth` of de portal. Zie voor meer informatie, [Application Gateway diagnostics](application-gateway-diagnostics.md).

### <a name="whats-the-retention-policy-for-the-diagnostic-logs"></a>Wat is het bewaarbeleid voor de diagnostische logboeken?

De stroom van de logboeken met diagnostische gegevens naar het storage-account van de klant. Klanten kunnen het bewaarbeleid op basis van hun voorkeur instellen. Logboeken met diagnostische gegevens kunnen ook worden verzonden naar een event hub of Azure Monitor-Logboeken. Zie voor meer informatie, [Application Gateway diagnostics](application-gateway-diagnostics.md).

### <a name="how-do-i-get-audit-logs-for-application-gateway"></a>Hoe krijg ik auditlogboeken voor Application Gateway?

Selecteer in de portal op het menu-blade van een toepassingsgateway **activiteitenlogboek** voor toegang tot het auditlogboek. 

### <a name="can-i-set-alerts-with-application-gateway"></a>Kan ik waarschuwingen kunt instellen met Application Gateway?

Ja. Waarschuwingen worden in Application Gateway geconfigureerd op de metrische gegevens. Zie voor meer informatie, [metrische gegevens over Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#metrics) en [meldingen van waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

### <a name="how-do-i-analyze-traffic-statistics-for-application-gateway"></a>Hoe ik verkeer statistische gegevens analyseren voor Application Gateway?

U kunt bekijken en analyseren van Logboeken op verschillende manieren openen. Gebruik Azure Monitor-Logboeken, Excel, Power BI, enzovoort.

U kunt ook een Resource Manager-sjabloon die wordt geïnstalleerd en wordt uitgevoerd de populaire [GoAccess](https://goaccess.io/) melden analyzer voor Application Gateway-Logboeken. GoAccess biedt waardevolle HTTP-verkeer-statistieken, zoals het unieke bezoekers, aangevraagde bestanden, hosts, besturingssystemen, browsers en HTTP-statuscodes. Zie voor meer informatie in GitHub, de [Leesmij-bestand in de sjabloonmap van de Resource Manager-](https://aka.ms/appgwgoaccessreadme).

### <a name="what-could-cause-backend-health-to-return-an-unknown-status"></a>Wat kan leiden tot back-endstatus om terug te keren een onbekende status?

Meestal het geval is, ziet u een onbekende status wanneer toegang tot de back-end wordt geblokkeerd door een netwerkbeveiligingsgroep (NSG), aangepaste DNS- of (UDR) gebruiker gedefinieerde routering op het subnet van de gateway. Zie voor meer informatie, [back-end-status, de logboekregistratie van diagnostische gegevens en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Application Gateway, [wat is Azure Application Gateway?](overview.md).
