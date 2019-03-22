---
title: Veelgestelde vragen over Azure Application Gateway
description: Deze pagina vindt u antwoorden op veelgestelde vragen over Azure Application Gateway
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 3/20/2019
ms.author: victorh
ms.openlocfilehash: f549f9c612797c1c956d6921fe4898a5f8bee9e6
ms.sourcegitcommit: 5e4ca656baf3c7d370ab3c0fbad0278aa2c9f1e6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319411"
---
# <a name="frequently-asked-questions-for-application-gateway"></a>Veelgestelde vragen over Application Gateway

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="general"></a>Algemeen

### <a name="what-is-application-gateway"></a>Wat is Application Gateway?

Azure Application Gateway is een Application Delivery Controller (ADC) als een service biedt u diverse layer 7 load balancing mogelijkheden voor uw toepassingen. Het biedt zeer beschikbare en schaalbare service, die volledig wordt beheerd door Azure.

### <a name="what-features-does-application-gateway-support"></a>Welke functies biedt ondersteuning voor Application Gateway?

Application Gateway biedt ondersteuning voor automatisch schalen, SSL-offloading en end-to-end SSL, Web Application Firewall, cookies gebaseerde sessieaffiniteit, url-pad gebaseerde routering, meerdere locaties die als host fungeert en anderen. Zie voor een volledige lijst van ondersteunde functies, [Inleiding tot Application Gateway](application-gateway-introduction.md).

### <a name="what-is-the-difference-between-application-gateway-and-azure-load-balancer"></a>Wat is het verschil tussen de Application Gateway en Azure Load Balancer?

Application Gateway is een layer 7 load balancer, wat betekent dat het werkt met alleen webverkeer (HTTP/HTTPS/WebSocket/HTTP/2). Het ondersteunt mogelijkheden zoals SSL-beëindiging, sessieaffiniteit op basis van een cookie en round robin voor taakverdeling van verkeer. Load Balancer laden saldo verkeer op laag 4 (TCP/UDP).

### <a name="what-protocols-does-application-gateway-support"></a>Welke protocollen ondersteunt Application Gateway?

Application Gateway biedt ondersteuning voor HTTP, HTTPS, HTTP/2 en WebSocket.

### <a name="how-does-application-gateway-support-http2"></a>Hoe biedt Application Gateway ondersteuning voor HTTP/2?

Zie [ondersteuning voor HTTP/2](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http2-support) voor meer informatie over hoe Application gateway ondersteunt het HTTP/2-protocol.

### <a name="what-resources-are-supported-today-as-part-of-backend-pool"></a>Welke bronnen worden momenteel ondersteund als onderdeel van de back-endpool?

Zie [ondersteund back-endresources](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#backend-pool) voor meer informatie over welke bronnen worden ondersteund door Application gateway.

### <a name="what-regions-is-the-service-available-in"></a>Welke regio's is de service beschikbaar is in?

Application Gateway is beschikbaar in alle regio's van de globale Azure. Het is ook beschikbaar in [Azure China 21Vianet](https://www.azure.cn/) en [Azure Government](https://azure.microsoft.com/overview/clouds/government/)

### <a name="is-this-a-dedicated-deployment-for-my-subscription-or-is-it-shared-across-customers"></a>Is dit een specifieke implementatie voor mijn abonnement of deze verdeeld over klanten?

Application Gateway is een specifieke implementatie in uw virtuele netwerk.

### <a name="is-http-https-redirection-supported"></a>HTTP-is > omleiding HTTPS ondersteund?

Omleiding wordt ondersteund. Zie [overzicht van Application Gateway redirect](application-gateway-redirect-overview.md) voor meer informatie.

### <a name="in-what-order-are-listeners-processed"></a>In welke volgorde worden listeners verwerkt?

Zie [volgorde van de verwerking van listeners](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-listeners).

### <a name="where-do-i-find-application-gateways-ip-and-dns"></a>Waar vind ik het IP- en DNS-Application-Gateway?

Wanneer u een openbaar IP-adres gebruikt als een eindpunt, deze informatie kan worden gevonden op het openbare IP-adresresource of op de pagina overzicht voor application gateway in de portal. Voor interne IP-adressen, kan dit worden gevonden op de pagina overzicht.

### <a name="what-is-keep-alive-timeout-and-tcp-idle-timeout-setting-on-application-gateway"></a>Wat is Keep-Alive-out en TCP-time-out voor inactiviteit instellen in Application Gateway?

Keep-Alive-out in v1-SKU is 120 sec. Keep-Alive-out op v2 SKU 75 sec. TCP-inactiviteit is standaard 4 minuten op het front-end VIP van de toepassingsgateway.

### <a name="does-the-ip-or-dns-name-change-over-the-lifetime-of-the-application-gateway"></a>Wordt het IP- of DNS-naam gewijzigd gedurende de levensduur van de Application Gateway?

Het VIP kunt wijzigen als de toepassingsgateway is gestopt en gestart. De DNS-naam die is gekoppeld aan de application gateway verandert niet gedurende de levenscyclus van de gateway. Om deze reden is het aanbevolen om een CNAME-alias gebruiken en wijs het DNS-adres van de toepassingsgateway.

### <a name="does-application-gateway-support-static-ip"></a>Application Gateway biedt ondersteuning voor statische IP-adres?

Ja, de v2-SKU van Application Gateway ondersteunt statische openbare IP-adressen. De v1-SKU biedt ondersteuning voor statische interne IP-adressen.

### <a name="does-application-gateway-support-multiple-public-ips-on-the-gateway"></a>Application Gateway ondersteunt de meerdere openbare IP-adressen op de gateway?

Slechts één openbaar IP-adres wordt ondersteund in een toepassingsgateway.

### <a name="how-large-should-i-make-my-subnet-for-application-gateway"></a>Hoe groot ik moet mijn subnet voor Application Gateway?

Zie [overwegingen bij de grootte van de Application Gateway-subnet](https://docs.microsoft.com/azure/application-gateway/configuration-overview#size-of-the-subnet) om te begrijpen van de grootte van het gatewaysubnet vereist voor de implementatie.

### <a name="q-can-i-deploy-more-than-one-application-gateway-resource-to-a-single-subnet"></a>V. Kan ik meer dan één Application Gateway-resource met één subnet implementeren?

Ja, naast de meerdere exemplaren van een bepaalde implementatie van de Application Gateway, kunt u een andere unieke Application Gateway-resource aan een bestaand subnet met een andere resource van Application Gateway inrichten.

Met een combinatie van Standard_v2 en Standard Application Gateway in hetzelfde subnet wordt niet ondersteund.

### <a name="does-application-gateway-support-x-forwarded-for-headers"></a>Application Gateway biedt ondersteuning voor x doorgestuurd voor headers?

Ja. Zie [wijzigingen aan te vragen](https://docs.microsoft.com/azure/application-gateway/how-application-gateway-works#modifications-to-the-request) voor meer informatie over de x-doorgestuurd-voor-headers die door Application Gateway ondersteund.

### <a name="how-long-does-it-take-to-deploy-an-application-gateway-does-my-application-gateway-still-work-when-being-updated"></a>Hoe lang duurt het implementeren van een Application Gateway? Mijn Application-Gateway nog steeds werkt wanneer wordt bijgewerkt?

Nieuwe Application Gateway v1-SKU-implementaties kunnen maximaal 20 minuten duren om in te richten. Wijzigingen in exemplaren grootte zijn niet verstorende en de gateway blijft actief gedurende deze tijd.

V2-SKU-implementaties kunnen ongeveer vijf of zes minuten duren om in te richten.

### <a name="can-exchange-server-be-used-as-backend-with-application-gateway"></a>Kan de Exchange-server worden gebruikt als back-end met Application Gateway?

Application Gateway ondersteunt niet het geval is, geen e-mailprotocollen zoals SMTP, IMAP en POP3. 

## <a name="performance"></a>Prestaties

### <a name="how-does-application-gateway-support-high-availability-and-scalability"></a>Hoe ondersteunt Application Gateway hoge beschikbaarheid en schaalbaarheid?

De v1-SKU van Application Gateway biedt ondersteuning voor scenario's voor hoge beschikbaarheid als er twee of meer instanties zijn geïmplementeerd. Deze instanties verdeelt Azure over update- en foutdomeinen domeinen om ervoor te zorgen dat alle exemplaren niet op hetzelfde moment mislukken. De v1-SKU biedt ondersteuning voor schaalbaarheid door meerdere exemplaren van dezelfde gateway voor het delen van de belasting toe te voegen.

De v2-SKU zorgt automatisch voor dat nieuwe exemplaren worden verdeeld over foutdomeinen en updatedomeinen. Als zoneredundantie is gekozen, worden ook de nieuwste exemplaren verdeeld in meerdere beschikbaarheidszones zonegebonden fout flexibiliteit bieden.

### <a name="how-do-i-achieve-dr-scenario-across-data-centers-with-application-gateway"></a>Hoe kan ik een herstel na noodgevallen in datacenters met Application Gateway?

Klanten kunnen Traffic Manager gebruiken om het verkeer verdelen over meerdere Toepassingsgateways in verschillende datacenters.

### <a name="is-autoscaling-supported"></a>Is de ondersteuning voor automatisch schalen?

Ja, de v2-SKU van Application Gateway biedt ondersteuning voor automatisch schalen. Zie voor meer informatie, [automatisch schalen en Zone-redundante Application Gateway (openbare Preview)](application-gateway-autoscaling-zone-redundant.md).

### <a name="does-manual-scale-updown-cause-downtime"></a>Handmatig schalen wordt omhoog/omlaag oorzaak downtime?

Er is geen downtime. Exemplaren worden verdeeld over upgrade-domeinen en domeinen met fouten.

### <a name="does-application-gateway-support-connection-draining"></a>Application Gateway ondersteunt verwerkingsstop?

Ja. U kunt configureren als u wilt wijzigen van de leden in een back endpool zonder onderbreking Verwerkingsstop voor verbindingen. Hiermee kunt bestaande verbindingen om door te gaan naar de vorige bestemming worden verzonden totdat de verbinding is gesloten of een configureerbare time-out is verlopen. Wacht alleen Verwerkingsstop voor verbindingen voor de huidige actieve verbindingen om te voltooien. Application Gateway is niet op de hoogte van de sessiestatus van toepassing.

### <a name="can-i-change-instance-size-from-medium-to-large-without-disruption"></a>Kan ik instantiegrootte van middelgrote tot grote zonder onderbreking wijzigen?

Ja, wordt Azure exemplaren verspreid over update- en foutdomeinen domeinen om ervoor te zorgen dat alle exemplaren niet op hetzelfde moment mislukken. Application Gateway biedt ondersteuning voor schalen door meerdere exemplaren van dezelfde gateway voor het delen van de belasting toe te voegen.

## <a name="configuration"></a>Configuratie

### <a name="is-application-gateway-always-deployed-in-a-virtual-network"></a>Application Gateway altijd geïmplementeerd in een virtueel netwerk?

Ja, Application Gateway altijd geïmplementeerd in een subnet van een virtueel netwerk. Dit subnet mag alleen Application Gateways. Zie [virtuele netwerk en subnet vereisten](https://docs.microsoft.com/azure/application-gateway/configuration-overview#azure-virtual-network-and-dedicated-subnet) om te begrijpen van de subnet-overwegingen voor Application Gateway.

### <a name="can-application-gateway-communicate-with-instances-outside-of-the-virtual-network-it-is-in-or-outside-of-the-subscription-it-is-in"></a>Kan Application Gateway communiceren met instanties buiten het virtuele netwerk in of buiten het abonnement dat deel uitmaakt van?

Application Gateway kan communiceren met instanties buiten het virtuele netwerk dat deze zich in of buiten het abonnement dat deel uitmaakt, zolang er een IP-verbinding is. Als u van plan bent om te gebruiken van interne IP-adressen als back-endpoolleden, wordt hiervoor [VNET-Peering](../virtual-network/virtual-network-peering-overview.md) of [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).

### <a name="can-i-deploy-anything-else-in-the-application-gateway-subnet"></a>Kan ik iets anders in het subnet van de gateway implementeren?

Nee, maar u kunt andere Toepassingsgateways in het subnet implementeren.

### <a name="are-network-security-groups-supported-on-the-application-gateway-subnet"></a>Worden Netwerkbeveiligingsgroepen op het subnet van de gateway ondersteund?

Zie [Netwerkbeveiligingsgroepen beperkingen met betrekking tot de Application Gateway-subnet](https://docs.microsoft.com/azure/application-gateway/configuration-overview#network-security-groups-supported-on-the-application-gateway-subnet) meer informatie over de Netwerkbeveiligingsgroepen op het subnet van de gateway ondersteund.

### <a name="are-user-defined-routes-supported-on-the-application-gateway-subnet"></a>Worden de gebruiker gedefinieerde routes ondersteund op het subnet van de gateway?

Zie [beperkingen van de gebruiker gedefinieerde routes](https://docs.microsoft.com/azure/application-gateway/configuration-overview#user-defined-routes-supported-on-the-application-gateway-subnet) voor meer informatie over de gebruiker gedefinieerde routes die wordt ondersteund op het subnet van de gateway.

### <a name="what-are-the-limits-on-application-gateway-can-i-increase-these-limits"></a>Wat zijn de limieten in Application Gateway? Kan ik deze limieten verhogen?

Zie [Application Gateway-limieten](../azure-subscription-service-limits.md#application-gateway-limits) om de limieten weer te geven.

### <a name="can-i-use-application-gateway-for-both-external-and-internal-traffic-simultaneously"></a>Kan ik tegelijkertijd Application Gateway voor interne en externe verkeer gebruiken?

Ja, ondersteunt Application Gateway met een interne IP-adres en één extern IP-adres per toepassingsgateway.

### <a name="is-vnet-peering-supported"></a>Wordt de VNet-peering ondersteund?

Ja, VNet-peering wordt ondersteund en is het nuttig zijn voor taakverdeling van verkeer in andere virtuele netwerken.

### <a name="can-i-talk-to-on-premises-servers-when-they-are-connected-by-expressroute-or-vpn-tunnels"></a>Kan ik vragen stellen met on-premises servers wanneer ze zijn verbonden met ExpressRoute of VPN-tunnels?

Ja, zolang er verkeer is toegestaan.

### <a name="can-i-have-one-backend-pool-serving-many-applications-on-different-ports"></a>Kan ik een back-endpool voor veel toepassingen op verschillende poorten hebben?

Servicearchitectuur van micro-wordt ondersteund. U moet meerdere HTTP-instellingen die zijn geconfigureerd om te testen op verschillende poorten.

### <a name="do-custom-probes-support-wildcardsregex-on-response-data"></a>Aangepaste tests bieden ondersteuning jokertekens/reguliere expressie op antwoordgegevens?

Aangepaste tests ondersteunen geen jokertekens of reguliere expressie op antwoordgegevens.

### <a name="how-are-rules-processed"></a>Hoe worden de regels verwerkt?

Zie [volgorde van de regels worden verwerkt](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-rules) om te begrijpen hoe routering regels zijn processen in Application Gateway.

### <a name="what-does-the-host-field-for-custom-probes-signify"></a>Wat het Host-veld voor aangepaste tests geven?

Veld host Hiermee geeft u de naam van de test om te verzenden. Van toepassing alleen als er meerdere sites is geconfigureerd in Application Gateway, anders gebruiken '127.0.0.1'. Deze waarde verschilt van de VM-hostnaam en is in de indeling \<protocol\>://\<host\>:\<poort\>\<pad\>.

### <a name="can-i-whitelist-application-gateway-access-to-a-few-source-ips"></a>Kan ik de lijst met toegestane adressen Application Gateway toegang tot een paar bron-IP's?

Ja. Zie [toegang beperken tot specifieke bron-IP-adressen](https://docs.microsoft.com/azure/application-gateway/configuration-overview#whitelist-application-gateway-access-to-a-few-source-ips) wilt weten hoe u om ervoor te zorgen dat alleen goedgekeurde bron-IP's toegang tot de toepassingsgateway.

### <a name="can-the-same-port-be-used-for-both-public-and-private-facing-listeners"></a>Kan de dezelfde poort worden gebruikt voor zowel openbare als persoonlijke gerichte listeners?

Nee, dit wordt niet ondersteund.

## <a name="configuration---ssl"></a>Configuratie - SSL

### <a name="what-certificates-are-supported-on-application-gateway"></a>Welke certificaten worden ondersteund in Application Gateway?

Zelfondertekende certificaten, CA-certificaten, VW certificaten en jokertekens certificaten worden ondersteund.

### <a name="what-are-the-current-cipher-suites-supported-by-application-gateway"></a>Wat zijn de huidige coderingssuites die door Application Gateway ondersteund?

Hieronder vindt u de huidige coderingssuites die door Application Gateway ondersteund. Zie [SSL configureren voor versies en coderingssuites in Application Gateway](application-gateway-configure-ssl-policy-powershell.md) voor meer informatie over het aanpassen van SSL-opties.

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

### <a name="does-application-gateway-also-support-re-encryption-of-traffic-to-the-backend"></a>Biedt Application Gateway ook opnieuw versleutelen van verkeer naar de back-end ondersteuning?

Ja, Application Gateway ondersteunt SSL-offload en end-to-end SSL, dit opnieuw versleutelt het verkeer naar de back-end.

### <a name="can-i-configure-ssl-policy-to-control-ssl-protocol-versions"></a>Kan ik een SSL-beleid voor het beheren van SSL-protocolversies configureren?

Ja, kunt u Application-Gateway voor het weigeren van TLS1.0 TLS1.1 en TLS1.2 configureren. SSL 2.0 en 3.0 zijn standaard al uitgeschakeld en zijn niet configureerbaar.

### <a name="can-i-configure-cipher-suites-and-policy-order"></a>Kan ik coderingssuites en beleidsvolgorde configureren?

Ja, [configuratie van coderingssuites](application-gateway-ssl-policy-overview.md) wordt ondersteund. Bij het definiëren van een aangepast beleid moet ten minste één van de volgende coderingssuites zijn ingeschakeld. Application gateway maakt gebruik van SHA256 op voor het beheer van back-end.

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

### <a name="how-many-ssl-certificates-are-supported"></a>Het aantal SSL-certificaten worden ondersteund?

Maximaal 100 SSL worden-certificaten ondersteund.

### <a name="how-many-authentication-certificates-for-backend-re-encryption-are-supported"></a>Het aantal verificatiecertificaten voor het opnieuw versleutelen van back-end worden ondersteund?

Certificaten worden maximaal 10 verificatie ondersteund met een standaardwaarde van 5.

### <a name="does-application-gateway-integrate-with-azure-key-vault-natively"></a>Application Gateway integreren met Azure Key Vault systeemeigen?

Nee, is het niet geïntegreerd met Azure Key Vault.

### <a name="how-to-configure-https-listeners-for-com-and-net-sites"></a>Het configureren van HTTP-luisteraars voor .com en .net sites? 

Voor meerdere op basis van een domein (op een host gebaseerde) routering, kunt u multi-site-listeners maken, kiest u HTTPS als het protocol in de listenerconfiguratie en de listeners koppelen aan de regels voor doorsturen. Zie voor meer informatie, [meerdere sites met Application Gateway hosten](https://docs.microsoft.com/azure/application-gateway/multiple-site-overview). 

## <a name="configuration---web-application-firewall-waf"></a>Configuratie - Web Application Firewall (WAF)

### <a name="does-the-waf-sku-offer-all-the-features-available-with-the-standard-sku"></a>Biedt de WAF-SKU alle functies die beschikbaar zijn met de standaard-SKU?

Ja, WAF biedt ondersteuning voor alle functies in de standaard-SKU.

### <a name="what-is-the-crs-version-application-gateway-supports"></a>Wat is de versie CRS Application Gateway ondersteunt?

Application Gateway ondersteunt CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) en CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).

### <a name="how-do-i-monitor-waf"></a>Hoe controleer ik WAF?

WAF via registratie in diagnoselogboek wordt bewaakt, meer informatie over het vastleggen van diagnostische gegevens kan worden gevonden op [logboekregistratie van diagnostische gegevens en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md)

### <a name="does-detection-mode-block-traffic"></a>Blokkeert detectiemodus verkeer?

Detectiemodus registreert Nee, alleen verkeer, waardoor een WAF-regel geactiveerd.

### <a name="can-i-customize-waf-rules"></a>Kan ik de WAF-regels aanpassen?

Ja, de WAF-regels kunnen worden aangepast. Zie voor meer informatie, [regels en regelgroepen van WAF aanpassen](application-gateway-customize-waf-rules-portal.md)

### <a name="what-rules-are-currently-available"></a>Welke regels zijn momenteel beschikbaar

Op dit moment ondersteunt CRS WAF [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) en [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), hier diebasislijnbeveiligingtegendemeestevandetop10beveiligingslekkengeïdentificeerddoorvandeOpenWebApplicationSecurityProject(OWASP)biedengevonden[ OWASP top 10 beveiligingsproblemen](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)

* Beveiliging tegen SQL-injecties

* Beveiliging tegen scripting op meerdere sites

* Beveiliging tegen veelvoorkomende aanvallen via internet, zoals opdrachtinjectie, het smokkelen van HTTP-aanvragen, het uitsplitsen van HTTP-antwoorden en aanvallen waarbij een extern bestand wordt ingesloten

* Beveiliging tegen schendingen van het HTTP-protocol

* Beveiliging tegen afwijkingen van het HTTP-protocol, zoals een gebruikersagent voor de host en Accept-headers die ontbreken

* Beveiliging tegen bots, crawlers en scanners

* Detectie van veelvoorkomende onjuiste configuraties van toepassingen (dat wil zeggen, Apache, IIS, enz.)

### <a name="does-waf-also-support-ddos-prevention"></a>Ondersteunt WAF ook DDoS voorkomen?

Ja. DDos-beveiliging op de VNet waar de application gateway wordt geïmplementeerd, kunt u inschakelen. Dit zorgt ervoor dat de toepassingsgateway die VIP wordt eveneens beveiligd met behulp van de service Azure DDos Protection.

## <a name="diagnostics-and-logging"></a>Diagnostische gegevens en logboekregistratie

### <a name="what-types-of-logs-are-available-with-application-gateway"></a>Welke soorten logboeken zijn beschikbaar met Application Gateway?

Er zijn drie logboeken beschikbaar voor Application Gateway. Zie voor meer informatie over deze logboeken en andere diagnostische mogelijkheden [back-end-status, diagnostische logboeken en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md).

* **ApplicationGatewayAccessLog** -het logboek voor toegang tot elke aanvraag verzonden naar de frontend van de application gateway bevat. De gegevens van de oproepende functie IP-adres, URL aangevraagd, wachttijd van het antwoord bevat, kunt u code bytes in en uit te retourneren. Toegangslogboek worden verzameld om de 300 seconden. Dit logboek bevat één record per exemplaar van een toepassingsgateway.
* **ApplicationGatewayPerformanceLog** -het logboekbestand voor prestaties bevat informatie over de prestaties op basis van afzonderlijke instanties zoals Totaal aantal aanvragen plaatsvindt, doorvoer in bytes, totaal aantal aanvragen dat plaatsvindt, aantal mislukte aanvragen, gezonde en niet in orde back-end aantal instanties.
* **ApplicationGatewayFirewallLog** -logboek voor de firewall bevat aanvragen die zijn geregistreerd via detectie of preventie modus van een application gateway die is geconfigureerd met web application firewall.

### <a name="how-do-i-know-if-my-backend-pool-members-are-healthy"></a>Hoe weet ik of Mijn back-endpoolleden in orde zijn?

U kunt de PowerShell-cmdlet `Get-AzApplicationGatewayBackendHealth` of status via de portal controleren door naar de pagina [Application Gateway Diagnostics](application-gateway-diagnostics.md)

### <a name="what-is-the-retention-policy-on-the-diagnostics-logs"></a>Wat is het bewaarbeleid voor de diagnostische logboeken?

Stroom voor diagnostische logboeken naar het opslagaccount voor klanten en klanten kunnen het bewaarbeleid op basis van hun voorkeur ingesteld. Logboeken met diagnostische gegevens kunnen ook worden verzonden naar een Event Hub of Azure Monitor Logboeken. Zie [Application Gateway Diagnostics](application-gateway-diagnostics.md) voor meer informatie.

### <a name="how-do-i-get-audit-logs-for-application-gateway"></a>Hoe krijg ik auditlogboeken voor Application Gateway?

Auditlogboeken zijn beschikbaar voor Application Gateway. Klik in de portal op **activiteitenlogboek** op het menu-blade van een toepassingsgateway voor toegang tot het auditlogboek. 

### <a name="can-i-set-alerts-with-application-gateway"></a>Kan ik waarschuwingen kunt instellen met Application Gateway?

Ja, Application Gateway ondersteunt waarschuwingen. Waarschuwingen zijn over metrische gegevens geconfigureerd. Zie [metrische gegevens van Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#metrics) voor meer informatie over Application Gateway metrische gegevens. Zie voor meer informatie over waarschuwingen, [meldingen van waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

### <a name="how-do-i-analyze-traffic-statistics-for-application-gateway"></a>Hoe ik verkeer statistische gegevens analyseren voor Application Gateway?

U kunt bekijken en analyseren van toegang tot logboeken via verschillende mechanismen zoals Azure Monitor-Logboeken, Excel, Power BI enzovoort.

We hebben ook een Resource Manager-sjabloon die wordt geïnstalleerd en wordt uitgevoerd de populaire gepubliceerd [GoAccess](https://goaccess.io/) analyzer aanmelden voor Application Gateway toegang tot logboeken. GoAccess biedt waardevolle HTTP-verkeer-statistieken, zoals het unieke bezoekers, bestanden aangevraagd, Hosts, besturingssystemen, Browsers, HTTP-statuscodes en nog veel meer. Zie voor meer informatie de [Leesmij-bestand in de map met het Resource Manager-sjablonen in GitHub](https://aka.ms/appgwgoaccessreadme).

### <a name="backend-health-returns-unknown-status-what-could-be-causing-this-status"></a>Back-endstatus onbekende status, wat kan worden veroorzaakt door deze status retourneert?

De meest voorkomende reden is toegang tot de back-end wordt geblokkeerd door een NSG, aangepaste DNS, of u hebt een UDR voor het subnet van de gateway. Zie [back-end-status, de logboekregistratie van diagnostische gegevens en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Application Gateway Zie [wat is Azure Application Gateway?](overview.md)
