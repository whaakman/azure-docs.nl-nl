---
title: Azure voordeur Service - Veelgestelde vragen over de voordeur | Microsoft Docs
description: Deze pagina vindt u antwoorden op veelgestelde vragen over Azure voordeur Service
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2019
ms.author: sharadag
ms.openlocfilehash: 256435dfd016ebbd86dbbe49f4abbb346fb1cd19
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407746"
---
# <a name="frequently-asked-questions-for-azure-front-door-service"></a>Veelgestelde vragen over Azure voordeur Service

In dit artikel vindt u antwoorden op veelgestelde vragen over Azure voordeur Service-functies en functionaliteit. Als u het antwoord op uw vraag niet ziet, u kunt contact met ons opnemen via de volgende kanalen (in steeds sneller groeiende volgorde):

1. Het gedeelte met opmerkingen van dit artikel.
2. [Azure voordeur Service UserVoice](https://feedback.azure.com/forums/217313-networking?category_id=345025).
3. **Microsoft-ondersteuning:** Maken van een nieuwe ondersteuningsaanvraag in de Azure-portal op de **Help** tabblad de **Help en ondersteuning** knop en selecteer vervolgens **nieuwe ondersteuningsaanvraag**.

## <a name="general"></a>Algemeen

### <a name="what-is-azure-front-door-service"></a>Wat is Azure Front Door Service?

Azure voordeur-Service is een Application Delivery Network (ADN) als een service biedt u verschillende layer 7 load balancing mogelijkheden voor uw toepassingen. Het biedt dynamic site acceleration (DSA) samen met globale taakverdeling met bijna real-time failover. Het is een zeer beschikbare en schaalbare service, die volledig wordt beheerd door Azure.

### <a name="what-features-does-azure-front-door-service-support"></a>Welke functies biedt ondersteuning voor Azure voordeur Service?

Azure voordeur Service biedt ondersteuning voor dynamic site acceleration (DSA), SSL-offloading en end-to-end SSL, Web Application Firewall, cookies gebaseerde sessieaffiniteit, url-pad gebaseerde routering, gratis certificaten en meerdere domeinbeheer en anderen. Zie voor een volledige lijst van ondersteunde functies, [overzicht van Azure voordeur Service](front-door-overview.md).

### <a name="what-is-the-difference-between-azure-front-door-service-and-azure-application-gateway"></a>Wat is het verschil tussen Azure voordeur Service en Azure Application Gateway?

Terwijl de voordeur zowel Application Gateway zijn layer 7 (HTTP/HTTPS) load balancers, wordt het belangrijkste verschil is dat de voordeur een wereldwijde service is terwijl Application Gateway een regionale service is. Terwijl de voordeur kan verdelen tussen verschillende clusters-eenheden/stempel schaaleenheden tussen regio's, kan Application Gateway verdelen tussen uw virtuele machines/containers enzovoort die binnen de schaaleenheid.

### <a name="when-should-we-deploy-an-application-gateway-behind-front-door"></a>Wanneer moet er een Application Gateway achter voordeur implementeren?

De belangrijkste scenario's waarom een Application Gateway achter voordeur moet gebruiken, zijn:

- Voordeur kunt uitvoeren op pad gebaseerde taakverdeling alleen op globaal niveau, maar als een wil om verkeer te verdelen nog verder in het virtuele netwerk (VNET) en vervolgens deze Application Gateway moeten gebruiken.
- Sinds de voordeur niet werkt op het niveau van een virtuele machine/container, dus niet Verwerkingsstop voor verbindingen. Application Gateway, kunt u Verwerkingsstop voor verbindingen. 
- Met een Application Gateway achter AFD, kan een maar liefst 100% SSL-offload en alleen HTTP-aanvragen in het virtuele netwerk (VNET) route.
- Ondersteuning voor sessie-affiniteit voordeur en Application Gateway. Terwijl de voordeur kunt instellen dat volgende verkeer van een gebruikerssessie op hetzelfde cluster of back-end in een bepaalde regio, kan Application Gateway het verkeer naar dezelfde server binnen het cluster direct affiniteit.  

### <a name="can-we-deploy-azure-load-balancer-behind-front-door"></a>Kunnen we Azure Load Balancer achter voordeur implementeren?

Azure voordeur Service moet een openbare VIP-adres of een openbaar beschikbare DNS-naam voor het routeren van verkeer. Implementeren van een Azure Load Balancer achter voordeur is een algemene use-case.

### <a name="what-protocols-does-azure-front-door-service-support"></a>Welke protocollen ondersteunt Azure voordeur Service?

Azure voordeur Service biedt ondersteuning voor HTTP, HTTPS en HTTP/2.

### <a name="how-does-azure-front-door-service-support-http2"></a>Hoe biedt Azure voordeur Service ondersteuning voor HTTP/2?

Ondersteuning voor HTTP/2-protocol is beschikbaar voor clients alleen verbinding maken met Azure voordeur Service. De communicatie met de back-ends via de back-endpool is via HTTP/1.1. Ondersteuning voor HTTP/2 is standaard ingeschakeld.

### <a name="what-resources-are-supported-today-as-part-of-backend-pool"></a>Welke bronnen worden momenteel ondersteund als onderdeel van de back-endpool?

Back-endpools kunnen bestaan uit Storage, Web-App, Kubernetes-exemplaren of andere aangepaste hostnaam die openbare verbinding heeft. Azure voordeur-Service is vereist dat de back-ends via een openbaar IP-adres of een openbaar omgezette DNS-hostnaam zijn gedefinieerd. Leden van de back-endpools kunnen zich in verschillende regio's, zones of zelfs buiten Azure, zolang ze openbare verbinding hebben.

### <a name="what-regions-is-the-service-available-in"></a>Welke regio's is de service beschikbaar is in?

Azure voordeur-Service is een wereldwijde service en is niet gekoppeld aan een specifieke Azure-regio. De enige locatie waar die u opgeven moet tijdens het maken van een voordeur is de locatie voor resourcegroep, die is in feite opgeven waar de metagegevens voor de resourcegroep moet worden opgeslagen. Voordeur resource zelf wordt gemaakt als een globale bron en de configuratie van de wereldwijd wordt geïmplementeerd op alle de POP's (Point of Presence). 

### <a name="what-are-the-pop-locations-for-azure-front-door-service"></a>Wat zijn de POP-locaties voor Azure voordeur Service?

Azure voordeur-Service heeft de dezelfde lijst (Point of Presence) POP-locaties als Azure CDN van Microsoft. Voor de volledige lijst van onze POP's, kunt verwijzen [Azure CDN POP-locaties van Microsoft](https://docs.microsoft.com/azure/cdn/cdn-pop-locations).

### <a name="is-azure-front-door-service-a-dedicated-deployment-for-my-application-or-is-it-shared-across-customers"></a>Azure voordeur Service is een specifieke implementatie voor mijn toepassing of is deze gedeeld met klanten?

Azure voordeur-Service is een wereldwijd gedistribueerde multitenant-service. De infrastructuur voor voordeur is daarom, gedeeld met alle klanten. Door het maken van een voordeur definieert u echter de specifieke configuratie vereist voor uw toepassing en 

### <a name="is-http-https-redirection-supported"></a>HTTP-is > omleiding HTTPS ondersteund?

Voordeur op dit moment biedt geen ondersteuning voor URL-omleiding.

### <a name="in-what-order-are-routing-rules-processed"></a>In welke volgorde worden regels voor doorsturen verwerkt?

Routes voor de voordeur zijn niet ingedeeld en een specifieke route wordt geselecteerd op basis van het meest geschikt is. Meer informatie over [hoe voordeur komt overeen met de aanvragen voor een regel voor doorsturen](front-door-route-matching.md).

### <a name="how-do-i-lock-down-the-access-to-my-backend-to-only-azure-front-door-service"></a>Hoe ik de toegang tot de vergrendelen naar Mijn back-end met alleen voordeur Service van Azure?

U kunt IP-ACLing voor uw back-ends voor het accepteren van alleen verkeer van Azure voordeur-Service configureren. U kunt uw toepassing om te accepteren van binnenkomende verbindingen alleen vanaf de back-end-IP-adresruimte van Azure voordeur Service beperken. We werken voor het integreren met [Azure IP-adresbereiken en servicetags](https://www.microsoft.com/download/details.aspx?id=56519) , maar voor nu kunt u de IP-adresbereiken als hieronder verwijzen:
 
- **IPv4** - `147.243.0.0/16`
- **IPv6** - `2a01:111:2050::/44`

> [!WARNING]
> Onze back-end-IP-adresruimte kan later worden gewijzigd, maar we om te zorgen dat voordat dit gebeurt, we zouden hebt geïntegreerd met [IP-adresbereiken voor Azure en servicetags](https://www.microsoft.com/download/details.aspx?id=56519). Het is raadzaam dat u zich abonneert op [IP-adresbereiken voor Azure en servicetags](https://www.microsoft.com/download/details.aspx?id=56519) voor alle wijzigingen en updates. 

### <a name="can-the-anycast-ip-change-over-the-lifetime-of-my-front-door"></a>Kan het anycast-IP-adres wijzigen gedurende de levensduur van mijn voordeur?

De frontend anycast-IP voor de voordeur doorgaans niet te wijzigen en voor de levensduur van de voordeur statische kan blijven. Er zijn echter **geen garanties** voor dezelfde. Geval is, kunt geen directe afhankelijkheden ondernemen op de IP-adres.  

### <a name="does-azure-front-door-service-support-static-or-dedicated-ips"></a>Azure voordeur Service biedt ondersteuning voor statische of toegewezen IP-adressen?

Nee, Azure voordeur Service op dit moment biedt geen ondersteuning voor statische of toegewezen frontend anycast IP-adressen. 

### <a name="does-azure-front-door-service-support-x-forwarded-for-headers"></a>Biedt ondersteuning voor Azure voordeur Service x doorgestuurd voor headers?

Ja, Azure voordeur Service biedt ondersteuning voor de koptekst X doorgestuurd voor X-doorgestuurd-Host en X-doorgestuurd-protocol. Voor X doorgestuurd voor als de header is al aanwezig en voordeur voegt het socket-IP-adres van client toe. Anders wordt de koptekst met het socket-IP-adres van client als de waarde toegevoegd. Voor X-doorgestuurd-Host en de X-doorgestuurd-protocol, wordt de waarde overschreven.

Meer informatie over de [voordeur ondersteunde HTTP-headers](front-door-http-headers-protocol.md).  

### <a name="how-long-does-it-take-to-deploy-an-azure-front-door-service-does-my-front-door-still-work-when-being-updated"></a>Hoe lang duurt het om een Service van de voordeur Azure te implementeren? Mijn voordeur nog steeds werkt wanneer wordt bijgewerkt?

Het maken van een nieuwe voordeur of updates voor een bestaande voordeur duurt het ongeveer 3 tot 5 minuten voor algemene implementatie. Dit betekent dat ongeveer 3 tot 5 minuten, de configuratie van de voordeur wereldwijd wordt geïmplementeerd in al onze POP's.

Opmerking: aangepaste SSL-certificaatupdates ongeveer 30 minuten duren wereldwijd worden gedistribueerd.

## <a name="configuration"></a>Configuratie

### <a name="can-azure-front-door-load-balance-or-route-traffic-within-a-virtual-network"></a>Kan de voordeur Azure-taakverdeling of route verkeer binnen een virtueel netwerk?

Azure voordeur (AFD) vereist een openbaar IP-adres of een openbaar omgezette DNS-naam om verkeer te routeren. Het antwoord is dus geen AFD kunt niet rechtstreeks binnen een virtueel netwerk routeren, maar met behulp van een Application Gateway of een Azure Load Balancer tussen verhelpt dit scenario.

### <a name="what-are-the-various-timeouts-and-limits-for-azure-front-door-service"></a>Wat zijn de verschillende time-outs en limieten voor Azure voordeur Service?

Meer informatie over alle de gedocumenteerde [time-outs en limieten voor Azure voordeur Service](https://docs.microsoft.com/azure/azure-subscription-service-limits#azure-front-door-service-limits).

## <a name="performance"></a>Prestaties

### <a name="how-does-azure-front-door-service-support-high-availability-and-scalability"></a>Hoe biedt Azure voordeur Service ondersteuning voor hoge beschikbaarheid en schaalbaarheid?

Azure voordeur-Service is een wereldwijd gedistribueerde multitenant-platform met grote volumes van de capaciteit die geschikt zijn voor de schaalbaarheid van uw toepassingsvereisten. Geleverd vanaf de rand van het wereldwijde netwerk van Microsoft, biedt voordeur voor globale taakverdeling mogelijkheid waarmee u uw hele toepassing of zelfs afzonderlijke microservices failover tussen regio's of andere clouds.

## <a name="ssl-configuration"></a>SSL-configuratie

### <a name="what-tls-versions-are-supported-by-azure-front-door-service"></a>De TLS-versies worden ondersteund door Azure voordeur Service?

Voordeur biedt ondersteuning voor TLS-versies 1.0, 1.1 en 1.2. TLS 1.3 is nog niet ondersteund.

### <a name="what-certificates-are-supported-on-azure-front-door-service"></a>Welke certificaten worden ondersteund op Azure voordeur Service?

Om in te schakelen het HTTPS-protocol voor de veilige levering van inhoud op een aangepast domein voordeur, kunt u een certificaat dat wordt beheerd door Azure voordeur Service gebruiken of uw eigen certificaat gebruiken.
De voordeur optie bepalingen een standaard SSL-certificaat via Digicert beheerd en opgeslagen op de voorgrond van de deur van Key Vault. Als u kiest om uw eigen certificaat te gebruiken, wordt u kunt Onboarding van een certificaat van een ondersteunde CA en kan een standaard SSL, uitgebreide validatie-certificaat of zelfs een certificaat met jokertekens. Zelfondertekende certificaten worden niet ondersteund. Informatie over [HTTPS inschakelen voor een aangepast domein](https://aka.ms/FrontDoorCustomDomainHTTPS).

### <a name="what-are-the-current-cipher-suites-supported-by-azure-front-door-service"></a>Wat zijn de huidige coderingssuites die worden ondersteund door Azure voordeur Service?

Hier volgen de huidige coderingssuites die worden ondersteund door Azure voordeur Service:

- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA

### <a name="does-azure-front-door-service-also-support-re-encryption-of-traffic-to-the-backend"></a>Biedt Azure voordeur Service ook opnieuw versleutelen van verkeer naar de back-end ondersteuning?

Ja, ondersteunt Azure voordeur Service SSL-offload- en end-to-end SSL, dit opnieuw versleutelt het verkeer naar de back-end. In feite omdat de verbindingen met de back-end gebeuren via het openbare IP-adres is, is het aanbevolen dat u configureert de voordeur om HTTPS te gebruiken als het protocol doorsturen.

### <a name="can-i-configure-ssl-policy-to-control-ssl-protocol-versions"></a>Kan ik een SSL-beleid voor het beheren van SSL-protocolversies configureren?

Nee, momenteel voordeur biedt geen ondersteuning voor het weigeren van specifieke TLS-versies en kunt u de minimale TLS-versies instellen. 

### <a name="can-i-configure-front-door-to-only-support-specific-cipher-suites"></a>Kan ik de voordeur ter ondersteuning van alleen specifieke coderingssuites configureren?

Nee, voordeur configureren voor specifieke coderingssuites wordt niet ondersteund. 

## <a name="diagnostics-and-logging"></a>Diagnostische gegevens en logboekregistratie

### <a name="what-types-of-metrics-and-logs-are-available-with-azure-front-door-service"></a>Welke soorten logboeken en metrische gegevens zijn beschikbaar met Azure voordeur Service?

Zie voor meer informatie over Logboeken en andere diagnostische mogelijkheden [bewaking van metrische gegevens en logboeken voor voordeur](front-door-diagnostics.md).

### <a name="what-is-the-retention-policy-on-the-diagnostics-logs"></a>Wat is het bewaarbeleid voor de diagnostische logboeken?

Stroom voor diagnostische logboeken naar het opslagaccount voor klanten en klanten kunnen het bewaarbeleid op basis van hun voorkeur ingesteld. Logboeken met diagnostische gegevens kunnen ook worden verzonden naar een Event Hub of Azure Monitor Logboeken. Zie voor meer informatie, [Azure voordeur Service Diagnostics](front-door-diagnostics.md).

### <a name="how-do-i-get-audit-logs-for-azure-front-door-service"></a>Hoe krijg ik auditlogboeken voor Azure voordeur Service?

Auditlogboeken zijn beschikbaar voor Azure voordeur Service. Klik in de portal op **activiteitenlogboek** in de menu-blade van uw voordeur voor toegang tot het auditlogboek. 

### <a name="can-i-set-alerts-with-azure-front-door-service"></a>Kan ik waarschuwingen kunt instellen met Azure voordeur Service?

Ja, ondersteunt Azure voordeur Service de waarschuwingen. Waarschuwingen zijn over metrische gegevens geconfigureerd. 

## <a name="next-steps"></a>Volgende stappen

- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).
- Lees [hoe Front Door werkt](front-door-routing-architecture.md).