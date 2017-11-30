---
title: Veelgestelde vragen over Azure Application Gateway | Microsoft Docs
description: Deze pagina vindt u antwoorden op veelgestelde vragen over Azure Application Gateway
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: d54ee7ec-4d6b-4db7-8a17-6513fda7e392
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: davidmu
ms.openlocfilehash: f92af44df9863bbf48abb4afcf9b1505c843fadc
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2017
---
# <a name="frequently-asked-questions-for-application-gateway"></a>Veelgestelde vragen voor Application Gateway

## <a name="general"></a>Algemeen

**Q. Wat is Application Gateway?**

Azure Application Gateway is een toepassing levering Controller (ADC) als een service biedt u verschillende laag 7 taakverdeling mogelijkheden voor uw toepassingen. Maximaal beschikbare en schaalbare service, die volledig worden beheerd door Azure biedt.

**Q. Welke functies biedt ondersteuning voor Application Gateway?**

Toepassingsgateway ondersteunt SSL-offloading en complete SSL, Web Application Firewall, sessie cookies gebaseerde affiniteit, url pad gebaseerde routering, hosting van multi-site en anderen. Voor een volledige lijst van ondersteunde functies, gaat u naar [Inleiding tot Application Gateway](application-gateway-introduction.md)

**Q. Wat is het verschil tussen een Application Gateway en Azure Load Balancer?**

Application Gateway is een load balancer van laag 7, wat betekent dat dit proces werkt met alleen webverkeer (HTTP/HTTPS/WebSocket). Mogelijkheden, zoals SSL-beëindiging sessie cookies gebaseerde affiniteit en round-robin worden ondersteund voor load balancing-verkeer. De Load Balancer, laadt het saldo's verkeer op laag 4 (TCP/UDP).

**Q. Welke protocollen biedt ondersteuning voor Application Gateway?**

Application Gateway biedt ondersteuning voor HTTP, HTTPS en WebSocket.

**Q. Welke bronnen worden nu ondersteund als onderdeel van de back-endpool?**

Back-endpools kunnen bestaan uit NIC's, virtuele-machineschaalsets, openbare IP-adressen, de namen van interne IP-adressen, de volledig gekwalificeerde domeinnaam (FQDN) en back-ends van meerdere tenants zoals Azure Webapps. Application Gateway back-end-groepsleden zijn niet gekoppeld aan een beschikbaarheidsset. Leden van de back-endpools mag tussen verschillende clusters, datacenters, of buiten Azure zolang ze beschikken over IP-verbinding.

**Q. Welke regio's is de service beschikbaar is in?**

Application Gateway is beschikbaar in alle regio's van globale Azure. Het is ook beschikbaar in [Azure China](https://www.azure.cn/) en [Azure Government](https://azure.microsoft.com/en-us/overview/clouds/government/)

**Q. Is dit een specifieke implementatie voor mijn abonnement of is het voor klanten is gedeeld?**

Application Gateway is een specifieke implementatie in uw virtuele netwerk.

**Q. HTTP-is > omleiding van HTTPS ondersteund?**

Omleiding wordt ondersteund. Ga naar [Application Gateway omleiding overzicht](application-gateway-redirect-overview.md) voor meer informatie.

**Q. In welke volgorde listeners verwerkt?**

Listeners worden verwerkt in de volgorde waarin die ze worden weergegeven. Daarom als een eenvoudige listener overeenkomt met een inkomende aanvraag verwerkt eerst.  Multi-site-listeners moeten worden geconfigureerd voordat een basic listener om ervoor te zorgen verkeer wordt doorgestuurd naar de juiste back-end.

**Q. Waar vind ik de IP- en DNS van de Gateway van de toepassing?**

Wanneer u een openbaar IP-adres gebruikt als een eindpunt, deze informatie vindt u op het openbare IP-adres resource of op de pagina overzicht voor de toepassingsgateway in de portal. Voor interne IP-adressen, kan dit worden gevonden op de pagina overzicht.

**Q. De IP- of DNS-verandert gedurende de levensduur van de toepassingsgateway?**

Het VIP kunt wijzigen als de gateway wordt gestopt en door de klant gestart. De DNS-server die is gekoppeld aan de toepassingsgateway wordt niet gewijzigd gedurende de levenscyclus van de gateway. Daarom is het aanbevolen gebruik van een CNAME-alias en wijs het DNS-adres van de toepassingsgateway.

**Q. Application Gateway biedt ondersteuning voor statische IP-adres?**

Nee, Application Gateway biedt geen ondersteuning voor statische openbare IP-adressen, maar biedt ondersteuning voor statische interne IP-adressen.

**Q. Ondersteunt Application Gateway meerdere openbare IP-adressen op de gateway?**

Slechts één openbaar IP-adres wordt ondersteund op een toepassingsgateway.

**Q. Application Gateway biedt ondersteuning voor x doorgestuurd voor headers?**

Ja, voegt de Application Gateway headers x doorgestuurd voor x-doorgestuurd-protocol en x-doorgestuurd-poort in de aanvraag doorgestuurd naar de back-end. De indeling voor x doorgestuurd voor header is een door komma's gescheiden lijst met IP: poort. De geldige waarden voor de x-doorgestuurd-protocol zijn http of https. X-doorgestuurd poort geeft de poort waarop de aanvraag op de toepassingsgateway bereikt.

**Q. Hoe lang duurt het implementeren van een Application Gateway? Mijn Application Gateway nog steeds werkt als wordt bijgewerkt?**

Nieuwe Application Gateway-implementaties kunnen maximaal 20 minuten duren om in te richten. Wijzigingen in exemplaren grootte zijn niet verstoren, en de gateway blijft actief gedurende deze tijd.

## <a name="configuration"></a>Configuratie

**Q. Toepassingsgateway altijd geïmplementeerd in een virtueel netwerk?**

Ja, Application Gateway altijd in een virtueel netwerksubnet geïmplementeerd. Dit subnet kan alleen Toepassingsgateways bevatten.

**Q. Kan Application Gateway Neem contact op met exemplaren buiten het virtuele netwerk?**

Application Gateway kan verbinden met exemplaren buiten het virtuele netwerk dat het, zolang er een IP-verbinding is. Als u van plan bent om te gebruiken van interne IP-adressen als back-end-groepsleden, is het vereist [VNET-Peering](../virtual-network/virtual-network-peering-overview.md) of [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).

**Q. Kan ik iets anders in het subnet voor Application Gateway implementeren?**

Nee, maar u kunt andere Toepassingsgateways in het subnet implementeren.

**Q. Ondersteunt het subnet voor Application Gateway Netwerkbeveiligingsgroepen?**

Netwerkbeveiligingsgroepen worden ondersteund op het subnet voor Application Gateway met de volgende beperkingen:

* Uitzonderingen moeten in worden geplaatst voor binnenkomend verkeer op poorten 65503-65534 voor back-end health correct te laten werken.

* Uitgaande verbinding met internet kan niet worden geblokkeerd.

* Verkeer van de AzureLoadBalancer-tag moet worden toegestaan.

**Q. Wat zijn de limieten voor Application Gateway? Kan ik deze limieten verhogen?**

Ga naar [Application Gateway limieten](../azure-subscription-service-limits.md#application-gateway-limits) om de limieten weer te geven.

**Q. Kan ik tegelijkertijd Application Gateway voor externe en interne verkeer gebruiken?**

Ja, Application Gateway ondersteunt het gebruik van een intern IP-adres en één extern IP-adres per Application Gateway.

**Q. Wordt VNet-peering ondersteund?**

Ja, VNet-peering wordt ondersteund en is handig voor taakverdeling verkeer in andere virtuele netwerken.

**Q. Kan ik met on-premises servers praten wanneer ze zijn verbonden via ExpressRoute- of VPN-tunnels?**

Ja, zolang er verkeer is toegestaan.

**Q. Kan ik een back-endpool voor veel toepassingen op verschillende poorten hebben?**

Architectuur micro service wordt ondersteund. U moet meerdere HTTP-instellingen geconfigureerd voor de test op verschillende poorten.

**Q. Ondersteunen aangepaste tests jokertekens/regex op antwoordgegevens?**

Aangepaste tests ondersteunen geen jokertekens of reguliere expressie op antwoordgegevens. 

**Q. Hoe kan ik regels verwerkt?**

Regels worden verwerkt in de volgorde waarin die ze zijn geconfigureerd. Het wordt aanbevolen dat de regels voor meerdere locaties worden geconfigureerd voordat basisregels om te verminderen de kans dat verkeer wordt doorgestuurd naar de back-end ongeschikte omdat de eenvoudige regel zou overeenkomt met het verkeer op basis van de poort voordat de multi-site-regel wordt geëvalueerd.

**Q. Hoe kan ik regels verwerkt?**

Regels worden verwerkt in de volgorde waarin die ze zijn gemaakt. Het wordt aanbevolen dat de regels voor meerdere locaties zijn geconfigureerd voor het basisregels. Multi-site-listeners eerst configureert, deze configuratie vermindert de kans dat verkeer wordt doorgestuurd naar de ongeschikte back-end. Deze routering probleem kan optreden als de basis regel overeenkomen met verkeer dat op basis van de poort voordat de multi-site-regel wordt geëvalueerd.

**Q. Wat het veld Host voor de aangepaste tests geven?**

Veld host geeft de naam de test te verzenden. Van toepassing alleen als er meerdere locaties is geconfigureerd op Application Gateway, of gebruik anders '127.0.0.1'. Deze waarde verschilt van de VM-hostnaam en indeling \<protocol\>://\<host\>:\<poort\>\<pad\>.

**Q. Kan ik geaccepteerde Application Gateway toegang tot enkele bron-IP-adressen?**

Dit scenario kan worden gedaan met nsg's op Application Gateway-subnet. De volgende beperkingen moeten worden geplaatst op het subnet in de vermelde volgorde van prioriteit:

* Toestaan dat binnenkomend verkeer van de bron-IP-of het IP-bereik.

* Toestaan van binnenkomende aanvragen van alle bronnen poorten 65503 65534 voor [back-end health communicatie](application-gateway-diagnostics.md).

* Toestaan van binnenkomende Azure Load Balancer-tests (AzureLoadBalancer-tag) en binnenkomende virtueel netwerkverkeer (VirtualNetwork-tag) op de [NSG](../virtual-network/virtual-networks-nsg.md).

* Alle andere binnenkomende verkeer met een weigeren alle regel blokkeren.

* Uitgaand verkeer naar het internet voor alle bestemmingen toestaan.

## <a name="performance"></a>Prestaties

**Q. Hoe ondersteunt Application Gateway hoge beschikbaarheid en schaalbaarheid?**

Toepassingsgateway ondersteunt scenario's voor hoge beschikbaarheid als er twee of meer instanties zijn geïmplementeerd. Azure distribueert deze instanties in update en fouttolerantie domeinen om ervoor te zorgen dat alle exemplaren niet op hetzelfde moment mislukken. Application Gateway biedt ondersteuning voor schaalbaarheid door meerdere exemplaren van dezelfde gateway voor het delen van de belasting toe te voegen.

**Q. Hoe ik herstel na noodgevallen bereikt via datacenters met Application Gateway?**

Klanten kunnen Traffic Manager gebruiken voor de distributie van verkeer over meerdere Toepassingsgateways in verschillende datacenters.

**Q. Wordt automatische schaling ondersteund?**

Nee, maar Application Gateway heeft een doorvoer metrische gegevens die kan worden gebruikt om u te waarschuwen wanneer een drempelwaarde is bereikt. Handmatig exemplaren toevoegen of wijzigen van de grootte van de gateway niet opnieuw wordt opgestart en heeft geen gevolgen voor bestaande-verkeer.

**Q. Ondersteunt handmatige schaal omhoog/omlaag oorzaak uitvaltijd?**

Er is geen downtime, exemplaren zijn verdeeld over upgradedomeinen en domeinen met fouten.

**Q. Kan ik exemplaargrootte wijzigen van gemiddeld tot grote zonder onderbreking?**

Ja, verdeelt Azure exemplaren over de update en fouttolerantie domeinen om ervoor te zorgen dat alle exemplaren niet op hetzelfde moment mislukken. Toepassingsgateway ondersteunt schalen door meerdere exemplaren van dezelfde gateway voor het delen van de belasting toe te voegen.

## <a name="ssl-configuration"></a>SSL-configuratie

**Q. Welke certificaten op Application Gateway worden ondersteund?**

Zelf-ondertekende certificaten, CA-certificaten, en certificaten voor het jokerteken worden ondersteund. EV certificaten worden niet ondersteund.

**Q. Wat zijn de huidige coderingssuites die wordt ondersteund door Application Gateway?**

Hieronder vindt u de huidige coderingssuites die wordt ondersteund door de toepassingsgateway. Ga naar: [configureren SSL beleid versies en coderingssuites in toepassingsgateway](application-gateway-configure-ssl-policy-powershell.md) voor informatie over het aanpassen van SSL-opties.

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

**Q. Ondersteunt Application Gateway ook opnieuw versleutelen van het verkeer naar de back-end?**

Ja, ondersteunt Application Gateway SSL-offload- en end-to-end SSL, dit opnieuw versleutelt het verkeer naar de back-end.

**Q. Kan ik een SSL-beleid om te bepalen met SSL-Protocol versies configureren?**

Ja, kunt u een toepassingsgateway voor het weigeren van TLS1.0 TLS1.1 en TLS1.2 configureren. SSL 2.0 en 3.0 zijn standaard al uitgeschakeld en zijn niet configureerbaar.

**Q. Kan ik coderingssuites en beleid volgorde configureren?**

Ja, [configuratie van coderingssuites](application-gateway-ssl-policy-overview.md) wordt ondersteund. Bij het definiëren van een aangepast beleid moet ten minste één van de volgende coderingssuites zijn ingeschakeld. SHA256 voor toepassingsgateway gebruikt voor het beheer van back-end.

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

**Q. Hoeveel SSL-certificaten worden ondersteund?**

Maximaal 20 SSL zijn certificaten worden ondersteund.

**Q. Hoeveel verificatiecertificaten voor opnieuw versleutelen van back-end worden ondersteund?**

Certificaten worden maximaal 10 verificatie met een standaardwaarde van 5 ondersteund.

**Q. Kan Application Gateway integreren met Azure Key Vault systeemeigen?**

Nee, dit niet is geïntegreerd met Azure Sleutelkluis.

## <a name="web-application-firewall-waf-configuration"></a>Web Application Firewall (WAF)-configuratie

**Q. Biedt de SKU WAF alle functies die beschikbaar zijn met de standaard SKU?**

Ja, ondersteunt WAF alle functies in de standaard SKU.

**Q. Wat is de versie CRS Application Gateway ondersteunt?**

Toepassingsgateway ondersteunt CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) en CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).

**Q. Hoe bewaak ik WAF?**

WAF via Diagnostische logboekregistratie wordt bewaakt, meer informatie over het vastleggen van diagnostische gegevens kan worden gevonden op [logboekregistratie van diagnostische gegevens en metrische gegevens voor de toepassingsgateway](application-gateway-diagnostics.md)

**Q. Blokkeert modus detectie verkeer?**

Nee, registreert detectie-modus alleen-verkeer, wat een regel WAF geactiveerd.

**Q. Hoe ik WAF regels aanpassen?**

Ja, WAF regels kunnen worden aangepast, voor meer informatie over het aanpassen van Bezoek [regelgroepen WAF aanpassen en regels](application-gateway-customize-waf-rules-portal.md)

**Q. Welke regels zijn momenteel beschikbaar?**

Op dit moment ondersteunt CRS WAF [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) en [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), hier diebasislijnbeschermdtegenhetmerendeelvandetop10beveiligingslekkengeïdentificeerddoorvandeOpenWebApplicationSecurityProject(OWASP)biedengevonden[ OWASP top 10 beveiligingsproblemen](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)

* Beveiliging tegen SQL-injecties

* Beveiliging tegen scripting op meerdere sites

* Beveiliging tegen veelvoorkomende aanvallen via internet, zoals opdrachtinjectie, het smokkelen van HTTP-aanvragen, het uitsplitsen van HTTP-antwoorden en aanvallen waarbij een extern bestand wordt ingesloten

* Beveiliging tegen schendingen van het HTTP-protocol

* Beveiliging tegen afwijkingen van het HTTP-protocol, zoals een gebruikersagent voor de host en Accept-headers die ontbreken

* Beveiliging tegen bots, crawlers en scanners

 * Detectie van veelvoorkomende onvolkomenheden in toepassing (dat wil zeggen, Apache, IIS, enz.)

**Q. Ondersteunt WAF ook DDoS voorkomen?**

Nee, WAF biedt geen DDoS te voorkomen.

## <a name="diagnostics-and-logging"></a>Diagnostische gegevens en logboekregistratie

**Q. Welke typen logboeken beschikbaar zijn met Application Gateway?**

Er zijn drie logboeken beschikbaar zijn voor de toepassingsgateway. Voor meer informatie over deze logboeken en andere diagnostische mogelijkheden, gaat u naar [back-end status, diagnostische logboeken en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md).

- **ApplicationGatewayAccessLog** -toegangslogboek voor de bevat elke aanvraag is verzonden naar de frontend Application Gateway. De gegevens van de oproepende functie IP-adres aangevraagd, URL antwoord latentie bevatten, retourcode, bytes in en uit. Toegangslogboek verzameld om de 300 seconden. Dit logboek bevat één record per exemplaar van de toepassingsgateway.
- **ApplicationGatewayPerformanceLog** -het prestatielogboek bevat informatie over de prestaties op per exemplaar basis totale aanvraag geleverd, inclusief doorvoer in bytes, het totaal aantal aanvragen geleverd, aantal mislukte aanvragen, in orde en slecht back-end aantal exemplaren.
- **ApplicationGatewayFirewallLog** -logboek voor de firewall aanvragen die zijn geregistreerd via de modus voor detectie of ter voorkoming van een toepassingsgateway die is geconfigureerd met web application firewall bevat.

**Q. Hoe weet ik of Mijn back-end-groepsleden in orde zijn?**

U kunt de PowerShell-cmdlet `Get-AzureRmApplicationGatewayBackendHealth` of status via de portal controleren door bezoeken [Application Gateway Diagnostics](application-gateway-diagnostics.md)

**Q. Wat is het bewaarbeleid op de logboeken met diagnostische gegevens?**

Diagnostische logboeken stroom op het opslagaccount van klanten en klanten het bewaarbeleid op basis van hun voorkeur kunnen instellen. Logboeken met diagnostische gegevens kunnen ook worden verzonden naar een Event Hub of logboekanalyse. Ga naar [Application Gateway Diagnostics](application-gateway-diagnostics.md) voor meer informatie.

**Q. Hoe krijg controlelogboeken voor Application Gateway?**

Controlelogboeken zijn beschikbaar voor Application Gateway. Klik in de portal op **activiteitenlogboek** op de blade menu van een toepassingsgateway voor toegang tot het controlelogboek. 

**Q. Kan ik waarschuwingen met Application Gateway instellen?**

Ja, Application Gateway biedt ondersteuning voor waarschuwingen, waarschuwingen uitschakelen metrische gegevens zijn geconfigureerd.  Toepassingsgateway heeft momenteel metric 'doorvoer', die kan worden geconfigureerd op waarschuwing. Voor meer informatie over waarschuwingen, gaat u naar [meldingen van waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

**Q. Back-end health status onbekend, wat kan worden veroorzaakt door deze status retourneert?**

De meest voorkomende reden is toegang tot de back-end wordt geblokkeerd door een NSG of aangepaste DNS. Ga naar [back-end health logboekregistratie van diagnostische gegevens en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Application Gateway Bezoek [Inleiding tot Application Gateway](application-gateway-introduction.md).
