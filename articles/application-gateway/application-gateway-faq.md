---
title: Veelgestelde vragen over Azure Application Gateway
description: Deze pagina vindt u antwoorden op veelgestelde vragen over Azure Application Gateway
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/6/2018
ms.author: victorh
ms.openlocfilehash: 9cb14e5076379e5095ca88dc749a954e9e5d5aa4
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53994841"
---
# <a name="frequently-asked-questions-for-application-gateway"></a>Veelgestelde vragen over Application Gateway

## <a name="general"></a>Algemeen

### <a name="what-is-application-gateway"></a>Wat is Application Gateway?

Azure Application Gateway is een Application Delivery Controller (ADC) als een service biedt u diverse layer 7 load balancing mogelijkheden voor uw toepassingen. Het biedt zeer beschikbare en schaalbare service, die volledig wordt beheerd door Azure.

### <a name="what-features-does-application-gateway-support"></a>Welke functies biedt ondersteuning voor Application Gateway?

Application Gateway biedt ondersteuning voor automatisch schalen, SSL-offloading en end-to-end SSL, Web Application Firewall, cookies gebaseerde sessieaffiniteit, url-pad gebaseerde routering, hosten van meerdere sites en anderen. Zie voor een volledige lijst van ondersteunde functies, [Inleiding tot Application Gateway](application-gateway-introduction.md).

### <a name="what-is-the-difference-between-application-gateway-and-azure-load-balancer"></a>Wat is het verschil tussen de Application Gateway en Azure Load Balancer?

Application Gateway is een layer 7 load balancer, wat betekent dat het werkt met alleen webverkeer (HTTP/HTTPS/WebSocket). Het ondersteunt mogelijkheden zoals SSL-beëindiging, sessieaffiniteit op basis van een cookie en round robin voor taakverdeling van verkeer. Load Balancer laden saldo verkeer op laag 4 (TCP/UDP).

### <a name="what-protocols-does-application-gateway-support"></a>Welke protocollen ondersteunt Application Gateway?

Application Gateway biedt ondersteuning voor HTTP, HTTPS, HTTP/2 en WebSocket.

### <a name="how-does-application-gateway-support-http2"></a>Hoe biedt Application Gateway ondersteuning voor HTTP/2?

Ondersteuning voor HTTP/2-protocol is beschikbaar voor clients verbinding maken met application gateway alleen listeners. De communicatie met de back-endserverpools is via HTTP/1.1. 

Ondersteuning voor HTTP/2 is standaard uitgeschakeld. De volgende Azure PowerShell-codefragment codevoorbeeld laat zien hoe u deze kunt inschakelen:

```
$gw = Get-AzureRmApplicationGateway -Name test -ResourceGroupName hm
$gw.EnableHttp2 = $true
Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

### <a name="what-resources-are-supported-today-as-part-of-backend-pool"></a>Welke bronnen worden momenteel ondersteund als onderdeel van de back-endpool?

Back-endpools kunnen bestaan uit NIC's, virtuele-machineschaalsets, openbare IP-adressen, namen van interne IP-adressen, de volledig gekwalificeerde domeinnaam (FQDN) en multitenant back-ends, zoals Azure Appservice. Back-endpoolleden voor Application Gateway zijn niet gekoppeld aan een beschikbaarheidsset. Leden van de back-endpools kunnen zich in meerdere computerclusters, datacenters, of buiten Azure, zolang ze IP-connectiviteit hebben.

### <a name="what-regions-is-the-service-available-in"></a>Welke regio's is de service beschikbaar is in?

Application Gateway is beschikbaar in alle regio's van de globale Azure. Het is ook beschikbaar in [Azure China](https://www.azure.cn/) en [Azure Government](https://azure.microsoft.com/overview/clouds/government/)

### <a name="is-this-a-dedicated-deployment-for-my-subscription-or-is-it-shared-across-customers"></a>Is dit een specifieke implementatie voor mijn abonnement of deze verdeeld over klanten?

Application Gateway is een specifieke implementatie in uw virtuele netwerk.

### <a name="is-http-https-redirection-supported"></a>HTTP-is > omleiding HTTPS ondersteund?

Omleiding wordt ondersteund. Zie [overzicht van Application Gateway redirect](application-gateway-redirect-overview.md) voor meer informatie.

### <a name="in-what-order-are-listeners-processed"></a>In welke volgorde worden listeners verwerkt?

Listeners worden verwerkt in de volgorde waarin die ze worden weergegeven. Om die reden als een basislistener overeenkomt met een binnenkomende aanvraag verwerkt eerst.  Multi-site-listeners moeten worden geconfigureerd voordat u een basislistener om te zorgen dat verkeer wordt gerouteerd naar de juiste back-end.

### <a name="where-do-i-find-application-gateways-ip-and-dns"></a>Waar vind ik het IP- en DNS-Application-Gateway?

Wanneer u een openbaar IP-adres gebruikt als een eindpunt, deze informatie kan worden gevonden op het openbare IP-adresresource of op de pagina overzicht voor application gateway in de portal. Voor interne IP-adressen, kan dit worden gevonden op de pagina overzicht.

### <a name="does-the-ip-or-dns-name-change-over-the-lifetime-of-the-application-gateway"></a>Wordt het IP- of DNS-naam gewijzigd gedurende de levensduur van de Application Gateway?

Het VIP kunt wijzigen als de toepassingsgateway is gestopt en gestart. De DNS-naam die is gekoppeld aan de application gateway verandert niet gedurende de levenscyclus van de gateway. Om deze reden is het aanbevolen om een CNAME-alias gebruiken en wijs het DNS-adres van de toepassingsgateway.

### <a name="does-application-gateway-support-static-ip"></a>Application Gateway biedt ondersteuning voor statische IP-adres?

Ja, de v2-SKU van Application Gateway ondersteunt statische openbare IP-adressen. De v1-SKU biedt ondersteuning voor statische interne IP-adressen.

### <a name="does-application-gateway-support-multiple-public-ips-on-the-gateway"></a>Application Gateway ondersteunt de meerdere openbare IP-adressen op de gateway?

Slechts één openbaar IP-adres wordt ondersteund in een toepassingsgateway.

### <a name="how-large-should-i-make-my-subnet-for-application-gateway"></a>Hoe groot ik moet mijn subnet voor Application Gateway?

Application Gateway verbruikt één privé IP-adres per exemplaar, plus een andere privé-IP-adres als een privé front-end-IP-configuratie is geconfigureerd. Ook Azure reserveert de eerste vier en laatste IP-adres in elk subnet voor intern gebruik.
Bijvoorbeeld, als een application gateway is ingesteld op drie exemplaren en geen privé front-end-IP, klikt u vervolgens een/29 subnet, grootte of hoger is vereist. In dit geval de application gateway maakt gebruik van drie IP-adressen. Als u drie exemplaren en een IP-adres voor de privé front-end-IP-configuratie, klikt u vervolgens een/28 hebt subnet groot of groter is nodig omdat tijdens vier IP-adressen zijn vereist.

### <a name="q-can-i-deploy-more-than-one-application-gateway-resource-to-a-single-subnet"></a>V. Kan ik meer dan één Application Gateway-resource met één subnet implementeren?

Ja, naast de meerdere exemplaren van een bepaalde implementatie van de Application Gateway, kunt u een andere unieke Application Gateway-resource aan een bestaand subnet met een andere resource van Application Gateway inrichten.

Met een combinatie van Standard_v2 en Standard Application Gateway in hetzelfde subnet wordt niet ondersteund. Als automatisch schalen is ingeschakeld, kan een subnet bovendien slechts één application gateway hebben.

### <a name="does-application-gateway-support-x-forwarded-for-headers"></a>Application Gateway biedt ondersteuning voor x doorgestuurd voor headers?

Ja, voegt de Application Gateway x doorgestuurd voor x-doorgestuurd-protocol en x-doorgestuurd-poort kopteksten in de aanvraag doorgestuurd naar de back-end. De indeling voor x-doorgestuurd-voor-header is een door komma's gescheiden lijst met IP: poort. De geldige waarden voor de x-doorgestuurd-protocol zijn http of https. X-doorgestuurd-poort geeft de poort waarop de aanvraag bij de application gateway is bereikt.

Application Gateway voegt ook X-oorspronkelijke-Host-header met de oorspronkelijke Host-header waarmee de aanvraag is ontvangen. Deze header is handig in scenario's zoals de integratie van Azure-Website, waar de binnenkomende host-header is gewijzigd voordat het verkeer wordt doorgestuurd naar de back-end.

### <a name="how-long-does-it-take-to-deploy-an-application-gateway-does-my-application-gateway-still-work-when-being-updated"></a>Hoe lang duurt het implementeren van een Application Gateway? Mijn Application-Gateway nog steeds werkt wanneer wordt bijgewerkt?

Nieuwe Application Gateway v1-SKU-implementaties kunnen maximaal 20 minuten duren om in te richten. Wijzigingen in exemplaren grootte zijn niet verstorende en de gateway blijft actief gedurende deze tijd.

V2-SKU-implementaties kunnen ongeveer vijf of zes minuten duren om in te richten.

Application Gateway biedt ondersteuning voor x doorgestuurd voor headers?

## <a name="configuration"></a>Configuratie

### <a name="is-application-gateway-always-deployed-in-a-virtual-network"></a>Application Gateway altijd geïmplementeerd in een virtueel netwerk?

Ja, Application Gateway altijd geïmplementeerd in een subnet van een virtueel netwerk. Dit subnet mag alleen Application Gateways.

### <a name="can-application-gateway-communicate-with-instances-outside-its-virtual-network"></a>Kan Application Gateway communiceren met exemplaren buiten het virtuele netwerk?

Application Gateway kan communiceren met instanties buiten het virtuele netwerk dat deel uitmaakt, zolang er een IP-verbinding is. Als u van plan bent om te gebruiken van interne IP-adressen als back-endpoolleden, wordt hiervoor [VNET-Peering](../virtual-network/virtual-network-peering-overview.md) of [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).

### <a name="can-i-deploy-anything-else-in-the-application-gateway-subnet"></a>Kan ik iets anders in het subnet van de gateway implementeren?

Nee, maar u kunt andere Toepassingsgateways in het subnet implementeren.

### <a name="are-network-security-groups-supported-on-the-application-gateway-subnet"></a>Worden Netwerkbeveiligingsgroepen op het subnet van de gateway ondersteund?

Netwerkbeveiligingsgroepen (nsg's) worden ondersteund op het subnet van de gateway met de volgende beperkingen:

* Uitzonderingen moeten in worden geplaatst voor binnenkomend verkeer op poort 65503 65534 voor Application Gateway v1-SKU en poorten 65200-65535 voor de v2-SKU. Dit poortbereik is vereist voor communicatie met Azure-infrastructuur. Ze zijn beveiligd (vergrendeld) met Azure-certificaten. Zonder de juiste certificaten kunnen zich externe entiteiten, inclusief de klanten van deze gateways, niet kunnen initiëren wijzigingen op de eindpunten.

* Uitgaande verbinding met internet kan niet worden geblokkeerd.

* Verkeer van de tag AzureLoadBalancer moet worden toegestaan.

### <a name="are-user-defined-routes-supported-on-the-application-gateway-subnet"></a>Worden de gebruiker gedefinieerde routes ondersteund op het subnet van de gateway?

Gebruiker gedefinieerde routes (udr's) worden ondersteund op het subnet van de gateway, zolang ze de end-to-end verzoek/reactie-communicatie niet wijzigen.

Bijvoorbeeld, u kunt een UDR in het subnet van de gateway instellen om te verwijzen naar een firewallapparaat voor pakketinspecties uitvoeren, maar moet u ervoor zorgen dat het pakket de inspectie van de bestemming van het bericht kan bereiken. Dit niet doet, kan leiden tot onjuiste health test of verkeer omleiden gedrag. Dit omvat geleerde routes of 0.0.0.0/0 standaardroutes doorgegeven door ExpressRoute of VPN-Gateways in het virtuele netwerk.

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

Regels worden verwerkt in de volgorde waarin die ze zijn geconfigureerd. Het verdient aanbeveling dat multi-site-regels zijn geconfigureerd voordat basic regels om te verminderen de kans dat verkeer wordt doorgestuurd naar de onjuiste back-end als de basisregel wordt gezocht naar verkeer op basis van de poort voordat de multi-site-regel wordt geëvalueerd.

### <a name="what-does-the-host-field-for-custom-probes-signify"></a>Wat het Host-veld voor aangepaste tests geven?

Veld host Hiermee geeft u de naam van de test om te verzenden. Van toepassing alleen als er meerdere sites is geconfigureerd in Application Gateway, anders gebruiken '127.0.0.1'. Deze waarde verschilt van de VM-hostnaam en is in de indeling \<protocol\>://\<host\>:\<poort\>\<pad\>.

### <a name="can-i-whitelist-application-gateway-access-to-a-few-source-ips"></a>Kan ik de lijst met toegestane adressen Application Gateway toegang tot een paar bron-IP's?

In dit scenario kan worden gedaan met nsg's op het subnet van de gateway. De volgende beperkingen moeten worden geplaatst op het subnet in de vermelde volgorde van prioriteit:

* Toestaan van binnenkomend verkeer van bron-IP/IP-bereik.

* Toestaan van binnenkomende aanvragen van alle bronnen op poorten 65503 65534 voor [back-end health communicatie](application-gateway-diagnostics.md). Dit poortbereik is vereist voor communicatie met Azure-infrastructuur. Ze zijn beveiligd (vergrendeld) met Azure-certificaten. Zonder de juiste certificaten kunnen zich externe entiteiten, inclusief de klanten van deze gateways, niet kunnen initiëren wijzigingen op de eindpunten.

* Toestaan van binnenkomende Azure Load Balancer-tests (tag AzureLoadBalancer) en binnenkomend verkeer in virtuele netwerken (VirtualNetwork-tag) op de [NSG](../virtual-network/security-overview.md).

* Alle andere binnenkomende verkeer met een weigeren alle regel blokkeren.

* Sta uitgaand verkeer naar internet voor alle bestemmingen.

### <a name="can-the-same-port-be-used-for-both-public-and-private-facing-listeners"></a>Kan de dezelfde poort worden gebruikt voor zowel openbare als persoonlijke gerichte listeners?

Nee, dit wordt niet ondersteund.

## <a name="performance"></a>Prestaties

### <a name="how-does-application-gateway-support-high-availability-and-scalability"></a>Hoe ondersteunt Application Gateway hoge beschikbaarheid en schaalbaarheid?

De v1-SKU van Application Gateway biedt ondersteuning voor scenario's voor hoge beschikbaarheid als er twee of meer instanties zijn geïmplementeerd. Deze instanties verdeelt Azure over update- en foutdomeinen domeinen om ervoor te zorgen dat alle exemplaren niet op hetzelfde moment mislukken. De v1-SKU biedt ondersteuning voor schaalbaarheid door meerdere exemplaren van dezelfde gateway voor het delen van de belasting toe te voegen.

De v2-SKU zorgt automatisch voor dat nieuwe exemplaren worden verdeeld over foutdomeinen en updatedomeinen. Als zoneredundantie is gekozen, worden ook de nieuwste exemplaren verdeeld in meerdere beschikbaarheidszones zonegebonden fout flexibiliteit bieden.

### <a name="how-do-i-achieve-dr-scenario-across-data-centers-with-application-gateway"></a>Hoe kan ik een herstel na noodgevallen in datacenters met Application Gateway?

Klanten kunnen Traffic Manager gebruiken om het verkeer verdelen over meerdere Toepassingsgateways in verschillende datacenters.

### <a name="is-autoscaling-supported"></a>Is de ondersteuning voor automatisch schalen?

Ja, de v2-SKU van Application Gateway biedt ondersteuning voor automatisch schalen. Zie voor meer informatie, [automatisch schalen en Zone-redundante Application Gateway (openbare Preview)](application-gateway-autoscaling-zone-redundant.md).

### <a name="does-manual-scale-updown-cause-downtime"></a>Handmatig schalen wordt omhoog/omlaag oorzaak downtime?

Er is geen downtime, exemplaren worden verdeeld over upgrade-domeinen en domeinen met fouten.

### <a name="does-application-gateway-support-connection-draining"></a>Application Gateway ondersteunt verwerkingsstop?

Ja. U kunt configureren als u wilt wijzigen van de leden in een back endpool zonder onderbreking Verwerkingsstop voor verbindingen. Hierdoor kunnen bestaande verbindingen om door te gaan naar de vorige bestemming worden verzonden totdat de verbinding is gesloten of een configureerbare time-out is verlopen. Wacht alleen Verwerkingsstop voor verbindingen voor de huidige actieve verbindingen om te voltooien. Application Gateway is niet op de hoogte van de sessiestatus van toepassing.

### <a name="what-are-application-gateway-sizes"></a>Wat is application gateway-grootten?

Application Gateway wordt momenteel aangeboden in drie grootten: **Kleine**, **gemiddeld**, en **grote**. Kleine exemplaargrootten zijn bedoeld voor het ontwikkelen en testen van scenario's.

Zie [Servicelimieten voor Application Gateway](../azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits) voor een volledige lijst van toepassingsgateway-limieten.

In de volgende tabel staan gemiddelde doorvoerprestaties voor elk toepassingsgateway-exemplaar waarvoor SSL-offload is uitgeschakeld:

| Gemiddelde paginalaadtijd van back-end Antwoordgrootte | Klein | Middelgroot | Groot |
| --- | --- | --- | --- |
| 6 KB |7,5 Mbps |13 Mbps |50 Mbps |
| 100 KB |35 Mbps |100 Mbps |200 Mbps |

> [!NOTE]
> Deze waarden zijn geschatte waarden voor de doorvoer van een toepassingsgateway. De werkelijke doorvoer hangt af van verschillende details van de omgeving, zoals de gemiddelde paginagrootte, locatie van back-endexemplaren en de verwerkingstijd voor een pagina. Voor nauwkeurige prestatiecijfers moet u uw eigen tests uitvoeren. Deze waarden worden alleen geboden als richtlijn voor de capaciteitsplanning.

### <a name="can-i-change-instance-size-from-medium-to-large-without-disruption"></a>Kan ik instantiegrootte van middelgrote tot grote zonder onderbreking wijzigen?

Ja, wordt Azure exemplaren verspreid over update- en foutdomeinen domeinen om ervoor te zorgen dat alle exemplaren niet op hetzelfde moment mislukken. Application Gateway biedt ondersteuning voor schalen door meerdere exemplaren van dezelfde gateway voor het delen van de belasting toe te voegen.

## <a name="ssl-configuration"></a>SSL-configuratie

### <a name="what-certificates-are-supported-on-application-gateway"></a>Welke certificaten worden ondersteund in Application Gateway?

Zelfondertekende certificaten, CA-certificaten en jokertekens certificaten worden ondersteund. VW certificaten worden niet ondersteund.

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

Maximaal 20 SSL worden-certificaten ondersteund.

### <a name="how-many-authentication-certificates-for-backend-re-encryption-are-supported"></a>Het aantal verificatiecertificaten voor het opnieuw versleutelen van back-end worden ondersteund?

Certificaten worden maximaal 10 verificatie ondersteund met een standaardwaarde van 5.

### <a name="does-application-gateway-integrate-with-azure-key-vault-natively"></a>Application Gateway integreren met Azure Key Vault systeemeigen?

Nee, is het niet geïntegreerd met Azure Key Vault.

## <a name="web-application-firewall-waf-configuration"></a>Web Application Firewall (WAF)-configuratie

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

U kunt de PowerShell-cmdlet `Get-AzureRmApplicationGatewayBackendHealth` of status via de portal controleren door naar de pagina [Application Gateway Diagnostics](application-gateway-diagnostics.md)

### <a name="what-is-the-retention-policy-on-the-diagnostics-logs"></a>Wat is het bewaarbeleid voor de diagnostische logboeken?

Stroom voor diagnostische logboeken naar het opslagaccount voor klanten en klanten kunnen het bewaarbeleid op basis van hun voorkeur ingesteld. Logboeken met diagnostische gegevens kunnen ook worden verzonden naar een Event Hub of Log Analytics. Zie [Application Gateway Diagnostics](application-gateway-diagnostics.md) voor meer informatie.

### <a name="how-do-i-get-audit-logs-for-application-gateway"></a>Hoe krijg ik auditlogboeken voor Application Gateway?

Auditlogboeken zijn beschikbaar voor Application Gateway. Klik in de portal op **activiteitenlogboek** in de menu-blade van een toepassingsgateway voor toegang tot het auditlogboek. 

### <a name="can-i-set-alerts-with-application-gateway"></a>Kan ik waarschuwingen kunt instellen met Application Gateway?

Ja, Application Gateway ondersteunt waarschuwingen. Waarschuwingen zijn over metrische gegevens geconfigureerd. Zie [metrische gegevens van Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#metrics) voor meer informatie over Application Gateway metrische gegevens. Zie voor meer informatie over waarschuwingen, [meldingen van waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

### <a name="how-do-i-analyze-traffic-statistics-for-application-gateway"></a>Hoe ik verkeer statistische gegevens analyseren voor Application Gateway?

U kunt bekijken en analyseren van toegang tot logboeken via een aantal methoden, zoals Azure Log Analytics, Excel, Power BI enzovoort.

We hebben ook een Resource Manager-sjabloon die wordt geïnstalleerd en wordt uitgevoerd de populaire gepubliceerd [GoAccess](https://goaccess.io/) analyzer aanmelden voor Application Gateway toegang tot logboeken. GoAccess biedt waardevolle HTTP-verkeer-statistieken, zoals het unieke bezoekers, bestanden aangevraagd, Hosts, besturingssystemen, Browsers, HTTP-statuscodes en nog veel meer. Zie voor meer informatie de [Leesmij-bestand in de map met het Resource Manager-sjablonen in GitHub](https://aka.ms/appgwgoaccessreadme).

### <a name="backend-health-returns-unknown-status-what-could-be-causing-this-status"></a>Back-endstatus onbekende status, wat kan worden veroorzaakt door deze status retourneert?

De meest voorkomende reden is toegang tot de back-end wordt geblokkeerd door een NSG of een aangepaste DNS. Zie [back-end-status, de logboekregistratie van diagnostische gegevens en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Application Gateway Zie [wat is Azure Application Gateway?](overview.md)
