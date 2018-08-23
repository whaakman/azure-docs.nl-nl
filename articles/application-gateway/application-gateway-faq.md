---
title: Veelgestelde vragen over Azure Application Gateway
description: Deze pagina vindt u antwoorden op veelgestelde vragen over Azure Application Gateway
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 8/10/2018
ms.author: victorh
ms.openlocfilehash: 858427bfd2a9b4c40ddf7054e09d98bcf5c1a992
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42057194"
---
# <a name="frequently-asked-questions-for-application-gateway"></a>Veelgestelde vragen over Application Gateway

## <a name="general"></a>Algemeen

**Q. Wat is Application Gateway?**

Azure Application Gateway is een Application Delivery Controller (ADC) als een service-aanbieding van laag 7 voor taakverdeling van mogelijkheden voor uw toepassingen. Het biedt zeer beschikbare en schaalbare service, die volledig wordt beheerd door Azure.

**Q. Welke functies biedt ondersteuning voor Application Gateway?**

Application Gateway ondersteunt SSL-offloading en end-to-end SSL, Web Application Firewall, cookies gebaseerde sessieaffiniteit, url-pad gebaseerde routering, hosten van meerdere sites en anderen. Voor een volledige lijst van ondersteunde functies, gaat u naar [Inleiding tot Application Gateway](application-gateway-introduction.md)

**Q. Wat is het verschil tussen de Application Gateway en Azure Load Balancer?**

Application Gateway is een layer 7 load balancer, wat betekent dat het werkt met alleen webverkeer (HTTP/HTTPS/WebSocket). Het ondersteunt mogelijkheden zoals SSL-beëindiging, sessieaffiniteit op basis van een cookie en round robin voor taakverdeling van verkeer. Load Balancer, laden saldo verkeer in laag 4 (TCP/UDP).

**Q. Welke protocollen ondersteunt Application Gateway?**

Application Gateway biedt ondersteuning voor HTTP, HTTPS, HTTP/2 en WebSocket.

**Q. Hoe biedt Application Gateway ondersteuning voor HTTP/2?**

Ondersteuning voor HTTP/2-protocol is beschikbaar voor clients verbinding maken met Application Gateway listeners alleen. De communicatie met de back-endserverpools is via HTTP/1.1. 

Ondersteuning voor HTTP/2 is standaard uitgeschakeld. De volgende Azure PowerShell-codefragment codevoorbeeld laat zien hoe u deze kunt inschakelen:

```
$gw = Get-AzureRmApplicationGateway -Name test -ResourceGroupName hm
$gw.EnableHttp2 = $true
Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

**Q. Welke bronnen worden momenteel ondersteund als onderdeel van de back-endpool?**

Back-endpools kunnen bestaan uit NIC's, virtuele-machineschaalsets, openbare IP-adressen, namen van interne IP-adressen, de volledig gekwalificeerde domeinnaam (FQDN) en multitenant back-ends, zoals Azure Webapps. Back-endpoolleden voor Application Gateway zijn niet gekoppeld aan een beschikbaarheidsset. Leden van de back-endpools kunnen zich in meerdere computerclusters, datacenters, of buiten Azure, zolang ze IP-connectiviteit hebben.

**Q. Welke regio's is de service beschikbaar is in?**

Application Gateway is beschikbaar in alle regio's van de globale Azure. Het is ook beschikbaar in [Azure China](https://www.azure.cn/) en [Azure Government](https://azure.microsoft.com/overview/clouds/government/)

**Q. Is dit een specifieke implementatie voor mijn abonnement of deze verdeeld over klanten?**

Application Gateway is een specifieke implementatie in uw virtuele netwerk.

**Q. HTTP-is > omleiding HTTPS ondersteund?**

Omleiding wordt ondersteund. Ga naar [overzicht van Application Gateway redirect](application-gateway-redirect-overview.md) voor meer informatie.

**Q. In welke volgorde worden listeners verwerkt?**

Listeners worden verwerkt in de volgorde waarin die ze worden weergegeven. Om die reden als een basislistener overeenkomt met een binnenkomende aanvraag verwerkt eerst.  Multi-site-listeners moeten worden geconfigureerd voordat u een basislistener om te zorgen dat verkeer wordt gerouteerd naar de juiste back-end.

**Q. Waar vind ik het IP- en DNS-Application-Gateway?**

Wanneer u een openbaar IP-adres gebruikt als een eindpunt, deze informatie kan worden gevonden op het openbare IP-adresresource of op de pagina overzicht voor Application Gateway in de portal. Voor interne IP-adressen, kan dit worden gevonden op de pagina overzicht.

**Q. Wordt het IP- of DNS gewijzigd gedurende de levensduur van de Application Gateway?**

Het VIP kunt wijzigen als de gateway wordt gestopt en door de klant gestart. De DNS-server die is gekoppeld aan Application Gateway verandert niet gedurende de levenscyclus van de gateway. Om deze reden is het aanbevolen om een CNAME-alias gebruiken en wijs het DNS-adres van de toepassingsgateway.

**Q. Application Gateway biedt ondersteuning voor statische IP-adres?**

Nee, Application Gateway biedt geen ondersteuning voor statische openbare IP-adressen, maar biedt ondersteuning voor statische interne IP-adressen.

**Q. Application Gateway ondersteunt de meerdere openbare IP-adressen op de gateway?**

Slechts één openbaar IP-adres wordt ondersteund in een toepassingsgateway.

**Q. Hoe groot moet ik mijn subnet voor Application Gateway?**

Application Gateway verbruikt één privé IP-adres per exemplaar, plus een andere privé-IP-adres als een privé front-end-IP-configuratie is geconfigureerd. Ook Azure reserveert de eerste vier en laatste IP-adres in elk subnet voor intern gebruik.
Bijvoorbeeld, als Application Gateway is ingesteld op drie exemplaren en geen privé front-end-IP, klikt u vervolgens een/29 subnet, grootte of hoger is vereist. In dit geval Application Gateway maakt gebruik van drie IP-adressen. Als u drie exemplaren en een IP-adres voor de privé front-end-IP-configuratie, klikt u vervolgens een/28 hebt subnet groot of groter is nodig omdat tijdens vier IP-adressen zijn vereist.

**Q. Application Gateway biedt ondersteuning voor x doorgestuurd voor headers?**

Ja, voegt de Application Gateway x doorgestuurd voor x-doorgestuurd-protocol en x-doorgestuurd-poort kopteksten in de aanvraag doorgestuurd naar de back-end. De indeling voor x-doorgestuurd-voor-header is een door komma's gescheiden lijst met IP: poort. De geldige waarden voor de x-doorgestuurd-protocol zijn http of https. X-doorgestuurd-poort geeft de poort waarop de aanvraag bij de Application Gateway is bereikt.

Application Gateway voegt ook X-oorspronkelijke-Host-header met de oorspronkelijke Host-header waarmee de aanvraag is ontvangen. Deze header is handig in scenario's zoals de integratie van Azure-Website, waar de binnenkomende host-header is gewijzigd voordat het verkeer wordt doorgestuurd naar de back-end.

**Q. Hoe lang duurt het implementeren van een Application Gateway? Mijn Application-Gateway nog steeds werkt wanneer wordt bijgewerkt?**

Nieuwe Application Gateway-implementaties kunnen maximaal 20 minuten duren om in te richten. Wijzigingen in exemplaren grootte zijn niet verstorende en de gateway blijft actief gedurende deze tijd.

## <a name="configuration"></a>Configuratie

**Q. Application Gateway altijd geïmplementeerd in een virtueel netwerk?**

Ja, Application Gateway altijd geïmplementeerd in een subnet van een virtueel netwerk. Dit subnet mag alleen Application Gateways.

**Q. Kan Application Gateway communiceren met instanties buiten het virtuele netwerk?**

Application Gateway kan communiceren met instanties buiten het virtuele netwerk dat deel uitmaakt, zolang er een IP-verbinding is. Als u van plan bent om te gebruiken van interne IP-adressen als back-endpoolleden, wordt hiervoor [VNET-Peering](../virtual-network/virtual-network-peering-overview.md) of [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).

**Q. Kan ik iets anders in het subnet voor Application Gateway implementeren?**

Nee, maar u kunt andere Toepassingsgateways in het subnet implementeren.

**Q. Worden Netwerkbeveiligingsgroepen op het subnet voor Application Gateway ondersteund?**

Netwerkbeveiligingsgroepen worden ondersteund in de Application Gateway-subnet met de volgende beperkingen:

* Uitzonderingen moeten in worden geplaatst voor binnenkomend verkeer op poort 65503 65534. Dit poortbereik is vereist voor communicatie met Azure-infrastructuur. Ze zijn beveiligd (vergrendeld) met Azure-certificaten. Zonder de juiste certificaten kunnen zich externe entiteiten, inclusief de klanten van deze gateways, niet kunnen initiëren wijzigingen op de eindpunten.

* Uitgaande verbinding met internet kan niet worden geblokkeerd.

* Verkeer van de tag AzureLoadBalancer moet worden toegestaan.

**Q. Worden de gebruiker gedefinieerde routes ondersteund op het subnet van de gateway?**

Gebruiker gedefinieerde routes (udr's) worden ondersteund op het subnet van de gateway, zolang ze de end-to-end verzoek/reactie-communicatie niet wijzigen.

Bijvoorbeeld, u kunt een UDR in het subnet van de gateway instellen om te verwijzen naar een firewallapparaat voor pakketinspecties uitvoeren, maar moet u ervoor zorgen dat het pakket de inspectie van de bestemming van het bericht kan bereiken. Dit niet doet, kan leiden tot onjuiste health test of verkeer omleiden gedrag. Dit omvat geleerde routes of 0.0.0.0/0 standaardroutes doorgegeven door ExpressRoute of VPN-Gateways in het virtuele netwerk.

**Q. Wat zijn de limieten in Application Gateway? Kan ik deze limieten verhogen?**

Ga naar [Application Gateway-limieten](../azure-subscription-service-limits.md#application-gateway-limits) om de limieten weer te geven.

**Q. Kan ik tegelijkertijd Application Gateway voor interne en externe verkeer gebruiken?**

Ja, Application Gateway ondersteunt het gebruik van een interne IP-adres en één extern IP-adres per Application Gateway.

**Q. Wordt de VNet-peering ondersteund?**

Ja, VNet-peering wordt ondersteund en is het nuttig zijn voor taakverdeling van verkeer in andere virtuele netwerken.

**Q. Kan ik vragen stellen met on-premises servers wanneer ze zijn verbonden met ExpressRoute of VPN-tunnels?**

Ja, zolang er verkeer is toegestaan.

**Q. Kan ik een back-endpool voor veel toepassingen op verschillende poorten hebben?**

Servicearchitectuur van micro-wordt ondersteund. U moet meerdere HTTP-instellingen die zijn geconfigureerd om te testen op verschillende poorten.

**Q. Aangepaste tests bieden ondersteuning jokertekens/reguliere expressie op antwoordgegevens?**

Aangepaste tests ondersteunen geen jokertekens of reguliere expressie op antwoordgegevens. 

**Q. Hoe worden de regels verwerkt?**

Regels worden verwerkt in de volgorde waarin die ze zijn geconfigureerd. Het verdient aanbeveling dat multi-site-regels zijn geconfigureerd voordat basic regels om te verminderen de kans dat verkeer wordt doorgestuurd naar de onjuiste back-end als de basisregel wordt gezocht naar verkeer op basis van de poort voordat de multi-site-regel wordt geëvalueerd.

**Q. Wat het Host-veld voor aangepaste tests geven?**

Veld host Hiermee geeft u de naam van de test om te verzenden. Van toepassing alleen als er meerdere sites is geconfigureerd in Application Gateway, anders gebruiken '127.0.0.1'. Deze waarde verschilt van de VM-hostnaam en is in de indeling \<protocol\>://\<host\>:\<poort\>\<pad\>.

**Q. Kan ik de lijst met toegestane adressen Application Gateway toegang tot een paar bron-IP's?**

In dit scenario kan worden gedaan met nsg's op Application Gateway-subnet. De volgende beperkingen moeten worden geplaatst op het subnet in de vermelde volgorde van prioriteit:

* Toestaan van binnenkomend verkeer van bron-IP/IP-bereik.

* Toestaan van binnenkomende aanvragen van alle bronnen op poorten 65503 65534 voor [back-end health communicatie](application-gateway-diagnostics.md). Dit poortbereik is vereist voor communicatie met Azure-infrastructuur. Ze zijn beveiligd (vergrendeld) met Azure-certificaten. Zonder de juiste certificaten kunnen zich externe entiteiten, inclusief de klanten van deze gateways, niet kunnen initiëren wijzigingen op de eindpunten.

* Toestaan van binnenkomende Azure Load Balancer-tests (tag AzureLoadBalancer) en binnenkomend verkeer in virtuele netwerken (VirtualNetwork-tag) op de [NSG](../virtual-network/security-overview.md).

* Alle andere binnenkomende verkeer met een weigeren alle regel blokkeren.

* Sta uitgaand verkeer naar internet voor alle bestemmingen.

**Q. Kan de dezelfde poort worden gebruikt voor zowel openbare als persoonlijke gerichte listeners?**

Nee, dit wordt niet ondersteund.

## <a name="performance"></a>Prestaties

**Q. Hoe ondersteunt Application Gateway hoge beschikbaarheid en schaalbaarheid?**

Application Gateway ondersteunt scenario's voor hoge beschikbaarheid als er twee of meer instanties zijn geïmplementeerd. Deze instanties verdeelt Azure over update- en foutdomeinen domeinen om ervoor te zorgen dat alle exemplaren niet op hetzelfde moment mislukken. Application Gateway ondersteunt schaalbaarheid door meerdere exemplaren van dezelfde gateway voor het delen van de belasting toe te voegen.

**Q. Hoe kan ik een herstel na noodgevallen in datacenters met Application Gateway?**

Klanten kunnen Traffic Manager gebruiken om het verkeer verdelen over meerdere Toepassingsgateways in verschillende datacenters.

**Q. Wordt automatisch schalen ondersteund?**

Nee, maar Application Gateway biedt een doorvoer metrische gegevens die kan worden gebruikt om u te waarschuwen wanneer een drempelwaarde is bereikt. Handmatig exemplaren toevoegen of wijzigen van grootte van de gateway niet opnieuw wordt opgestart en heeft geen invloed op bestaande verkeer.

**Q. Handmatig schalen wordt omhoog/omlaag oorzaak downtime?**

Er is geen downtime, exemplaren worden verdeeld over upgrade-domeinen en domeinen met fouten.

**Q. Biedt de verwerkingsstop van application gateway ondersteuning?**

Ja. U kunt configureren als u wilt wijzigen van de leden in een back endpool zonder onderbreking Verwerkingsstop voor verbindingen. Hierdoor kunnen bestaande verbindingen om door te gaan naar de vorige bestemming worden verzonden totdat de verbinding is gesloten of een configureerbare time-out is verlopen. Houd er rekening mee dat verwerkingsstop van verbindingen alleen wacht op huidige actieve verbindingen om te voltooien. Application Gateway is niet op de hoogte van de sessiestatus van toepassing.

**Q. Wat is application gateway-grootten?**

Application Gateway wordt momenteel aangeboden in drie grootten: **klein**, **middelgroot** en **groot**. Kleine exemplaargrootten zijn bedoeld voor het ontwikkelen en testen van scenario's.

U kunt maximaal 50 toepassingsgateways per abonnement maken. Elke toepassingsgateway kan maximaal 10 exemplaren hebben. Elke toepassingsgateway kan bestaan uit 20 HTTP-listeners. Zie [Servicelimieten voor Application Gateway](../azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits) voor een volledige lijst van toepassingsgateway-limieten.

In de volgende tabel staan gemiddelde doorvoerprestaties voor elk toepassingsgateway-exemplaar waarvoor SSL-offload is uitgeschakeld:

| Gemiddelde paginalaadtijd van back-end Antwoordgrootte | Klein | Middelgroot | Groot |
| --- | --- | --- | --- |
| 6KB |7,5 Mbps |13 Mbps |50 Mbps |
| 100KB |35 Mbps |100 Mbps |200 Mbps |

> [!NOTE]
> Deze waarden zijn geschatte waarden voor de doorvoer van een toepassingsgateway. De werkelijke doorvoer hangt af van verschillende details van de omgeving, zoals de gemiddelde paginagrootte, locatie van back-endexemplaren en de verwerkingstijd voor een pagina. Voor nauwkeurige prestatiecijfers moet u uw eigen tests uitvoeren. Deze waarden worden alleen geboden als richtlijn voor de capaciteitsplanning.

**Q. Kan ik instantiegrootte van middelgrote tot grote zonder onderbreking wijzigen?**

Ja, wordt Azure exemplaren verspreid over update- en foutdomeinen domeinen om ervoor te zorgen dat alle exemplaren niet op hetzelfde moment mislukken. Application Gateway biedt ondersteuning voor schalen door meerdere exemplaren van dezelfde gateway voor het delen van de belasting toe te voegen.

## <a name="ssl-configuration"></a>SSL-configuratie

**Q. Welke certificaten worden ondersteund in Application Gateway?**

Zelf-ondertekende certificaten, CA-certificaten en jokertekens certificaten worden ondersteund. VW certificaten worden niet ondersteund.

**Q. Wat zijn de huidige coderingssuites die door Application Gateway ondersteund?**

Hieronder vindt u de huidige coderingssuites die wordt ondersteund door application gateway. Ga naar: [configureren SSL beleid versies en coderingssuites in Application Gateway](application-gateway-configure-ssl-policy-powershell.md) voor meer informatie over het aanpassen van SSL-opties.

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

**Q. Biedt Application Gateway ook opnieuw versleutelen van verkeer naar de back-end ondersteuning?**

Ja, Application Gateway ondersteunt SSL-offload en end-to-end SSL, dit opnieuw versleutelt het verkeer naar de back-end.

**Q. Kan ik een SSL-beleid voor het beheren van SSL-protocolversies configureren?**

Ja, kunt u Application-Gateway voor het weigeren van TLS1.0 TLS1.1 en TLS1.2 configureren. SSL 2.0 en 3.0 zijn standaard al uitgeschakeld en zijn niet configureerbaar.

**Q. Kan ik coderingssuites en beleidsvolgorde configureren?**

Ja, [configuratie van coderingssuites](application-gateway-ssl-policy-overview.md) wordt ondersteund. Bij het definiëren van een aangepast beleid moet ten minste één van de volgende coderingssuites zijn ingeschakeld. Application gateway maakt gebruik van SHA256 op voor het beheer van back-end.

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

**Q. Het aantal SSL-certificaten worden ondersteund?**

Maximaal 20 SSL worden-certificaten ondersteund.

**Q. Het aantal verificatiecertificaten voor het opnieuw versleutelen van back-end worden ondersteund?**

Certificaten worden maximaal 10 verificatie ondersteund met een standaardwaarde van 5.

**Q. Application Gateway integreren met Azure Key Vault systeemeigen?**

Nee, is het niet geïntegreerd met Azure Key Vault.

## <a name="web-application-firewall-waf-configuration"></a>Web Application Firewall (WAF)-configuratie

**Q. Biedt de WAF-SKU alle functies die beschikbaar zijn met de standaard-SKU?**

Ja, WAF biedt ondersteuning voor alle functies in de standaard-SKU.

**Q. Wat is de versie CRS Application Gateway ondersteunt?**

Application Gateway ondersteunt CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) en CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).

**Q. Hoe controleer ik WAF?**

WAF via registratie in diagnoselogboek wordt bewaakt, meer informatie over het vastleggen van diagnostische gegevens kan worden gevonden op [logboekregistratie van diagnostische gegevens en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md)

**Q. Blokkeert detectiemodus verkeer?**

Detectiemodus registreert Nee, alleen verkeer, waardoor een WAF-regel geactiveerd.

**Q. Hoe ik de WAF-regels aanpassen?**

Ja, WAF-regels kunnen worden aangepast, voor meer informatie over hoe u ze aan te passen Bezoek [regels en regelgroepen van WAF aanpassen](application-gateway-customize-waf-rules-portal.md)

**Q. Welke regels zijn momenteel beschikbaar?**

Op dit moment ondersteunt CRS WAF [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) en [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), hier diebasislijnbeveiligingtegendemeestevandetop10beveiligingslekkengeïdentificeerddoorvandeOpenWebApplicationSecurityProject(OWASP)biedengevonden[ OWASP top 10 beveiligingsproblemen](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)

* Beveiliging tegen SQL-injecties

* Beveiliging tegen scripting op meerdere sites

* Beveiliging tegen veelvoorkomende aanvallen via internet, zoals opdrachtinjectie, het smokkelen van HTTP-aanvragen, het uitsplitsen van HTTP-antwoorden en aanvallen waarbij een extern bestand wordt ingesloten

* Beveiliging tegen schendingen van het HTTP-protocol

* Beveiliging tegen afwijkingen van het HTTP-protocol, zoals een gebruikersagent voor de host en Accept-headers die ontbreken

* Beveiliging tegen bots, crawlers en scanners

 * Detectie van veelvoorkomende onjuiste configuraties van toepassingen (dat wil zeggen, Apache, IIS, enz.)

**Q. Ondersteunt WAF ook DDoS voorkomen?**

Nee, WAF biedt geen DDoS-preventie.

## <a name="diagnostics-and-logging"></a>Diagnostische gegevens en logboekregistratie

**Q. Welke soorten logboeken zijn beschikbaar met Application Gateway?**

Er zijn drie logboeken beschikbaar voor Application Gateway. Bezoek voor meer informatie over deze logboeken en andere diagnostische mogelijkheden [back-end-status, diagnostische logboeken en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md).

- **ApplicationGatewayAccessLog** -het logboek voor toegang tot elke aanvraag verzonden naar de frontend van de Application Gateway bevat. De gegevens van de oproepende functie IP-adres, URL aangevraagd, wachttijd van het antwoord bevat, kunt u code bytes in en uit te retourneren. Toegangslogboek worden verzameld om de 300 seconden. Dit logboek bevat één record per exemplaar van Application Gateway.
- **ApplicationGatewayPerformanceLog** -het logboekbestand voor prestaties bevat informatie over de prestaties op basis van afzonderlijke instanties zoals Totaal aantal aanvragen plaatsvindt, doorvoer in bytes, totaal aantal aanvragen dat plaatsvindt, aantal mislukte aanvragen, gezonde en niet in orde back-end aantal instanties.
- **ApplicationGatewayFirewallLog** -logboek voor de firewall bevat aanvragen die zijn geregistreerd via detectie of preventie modus van een application gateway die is geconfigureerd met web application firewall.

**Q. Hoe weet ik of Mijn back-endpoolleden in orde zijn?**

U kunt de PowerShell-cmdlet `Get-AzureRmApplicationGatewayBackendHealth` of status via de portal controleren door naar de pagina [Application Gateway Diagnostics](application-gateway-diagnostics.md)

**Q. Wat is het bewaarbeleid voor de diagnostische logboeken?**

Stroom voor diagnostische logboeken naar het opslagaccount voor klanten en klanten kunnen het bewaarbeleid op basis van hun voorkeur ingesteld. Logboeken met diagnostische gegevens kunnen ook worden verzonden naar een Event Hub of Log Analytics. Ga naar [Application Gateway Diagnostics](application-gateway-diagnostics.md) voor meer informatie.

**Q. Hoe krijg ik auditlogboeken voor Application Gateway?**

Auditlogboeken zijn beschikbaar voor Application Gateway. Klik in de portal op **activiteitenlogboek** in de menu-blade van een toepassingsgateway voor toegang tot het auditlogboek. 

**Q. Kan ik waarschuwingen kunt instellen met Application Gateway?**

Ja, Application Gateway biedt ondersteuning voor waarschuwingen, meldingen zijn geconfigureerd als uitgeschakeld metrische gegevens. Application Gateway momenteel heeft een metrische waarde van "doorvoer', wat kan worden geconfigureerd op waarschuwing. Voor meer informatie over waarschuwingen, gaat u naar [meldingen van waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

**Q. Back-endstatus onbekende status, wat kan worden veroorzaakt door deze status retourneert?**

De meest voorkomende reden is toegang tot de back-end wordt geblokkeerd door een NSG of een aangepaste DNS. Ga naar [back-end-status, de logboekregistratie van diagnostische gegevens en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Application Gateway Bezoek [wat is Azure Application Gateway?](overview.md)
