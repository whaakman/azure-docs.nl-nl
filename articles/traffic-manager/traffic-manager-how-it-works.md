---
title: De werking van Azure Traffic Manager | Microsoft Docs
description: In dit artikel krijgt u inzicht in hoe Traffic Manager routeert verkeer voor hoge prestaties en beschikbaarheid van uw web-apps
services: traffic-manager
documentationcenter: ''
author: kumudd
manager: twooley
editor: ''
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2018
ms.author: kumud
ms.openlocfilehash: 26d61c1b263a8fa7ff4f0ff5b2888f1d900e772e
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55567839"
---
# <a name="how-traffic-manager-works"></a>How Traffic Manager Works

Met Azure Traffic Manager kunt u voor het beheren van de distributie van verkeer tussen de toepassingseindpunten van uw. Een eindpunt is een internetgerichte service die binnen of buiten Azure wordt gehost.

Traffic Manager biedt twee belangrijke voordelen:

- Distributie van verkeer op basis van een van verschillende [routeringsmethoden voor verkeer](traffic-manager-routing-methods.md)
- [Doorlopende bewaking van status van eindpunt](traffic-manager-monitoring.md) en automatische failover wanneer eindpunten mislukken

Wanneer een client probeert verbinding maken met een service, moet deze eerst de DNS-naam van de service omzetten naar een IP-adres. De client maakt vervolgens verbinding met IP-adres voor toegang tot de service.

**Het belangrijkste punt om te begrijpen is dat Traffic Manager op DNS-niveau werkt.**  Traffic Manager gebruikt DNS om clients omleiden naar specifieke service-eindpunten op basis van de regels van de verkeersrouteringsmethode. Clients verbinding maken met het eindpunt van de geselecteerde **rechtstreeks**. Traffic Manager is niet een proxy of een gateway. Traffic Manager het verkeer te geven tussen de client en de service niet te zien.

## <a name="traffic-manager-example"></a>Voorbeeld van Traffic Manager

Contoso Corp hebben een nieuwe partner-portal ontwikkeld. De URL voor deze portal is https://partners.contoso.com/login.aspx. De toepassing wordt gehost in drie regio's van Azure. Als u wilt de beschikbaarheid te verbeteren en wereldwijde prestaties te maximaliseren, ze Traffic Manager gebruiken om clientverkeer naar de dichtstbijzijnde beschikbare eindpunt te distribueren.

Voor het bereiken van deze configuratie, uitvoeren ze de volgende stappen:

1. Drie exemplaren van de service implementeren. De DNS-namen van deze implementaties zijn 'contoso-us.cloudapp .net', 'contoso-eu.cloudapp .net' en 'contoso-asia.cloudapp .net'.
2. Maak een Traffic Manager-profiel met de naam 'contoso.trafficmanager.net', en configureer deze voor het gebruik van de methode 'Prestaties' Routering van verkeer voor de drie eindpunten.
* De aangepaste domeinnaam, 'partners.contoso.com', om te verwijzen naar contoso.trafficmanager.net, met behulp van een DNS CNAME-record configureren.

![Traffic Manager-DNS-configuratie][1]

> [!NOTE]
> Wanneer u een aangepast domein met Azure Traffic Manager, moet u een CNAME uw aangepaste domeinnaam verwijzen naar de naam van uw Traffic Manager-domein. DNS-standaarden staan niet toe dat u een CNAME in de 'top' (of basiscertificaat) van een domein maken. U kunt geen dus een CNAME voor 'contoso.com' (ook wel een domein 'zonder voorvoegsel zijn' genoemd) maken. U kunt alleen een CNAME voor een domein 'contoso.com', zoals 'www.contoso.com' maken. U kunt deze beperking omzeilen, wordt aangeraden uw DNS-domein hosten op [Azure DNS](../dns/dns-overview.md) en het gebruik van [aliasrecords](../dns/tutorial-alias-tm.md) om te verwijzen naar uw traffic manager-profiel. U kunt ook kunt u een eenvoudige HTTP-omleiding op direct aanvragen voor 'contoso.com' naar een alternatieve naam, zoals 'www.contoso.com'.

### <a name="how-clients-connect-using-traffic-manager"></a>Hoe clients verbinding maken met behulp van Traffic Manager

Verder uit het vorige voorbeeld, wanneer een client een aanvraag de pagina https://partners.contoso.com/login.aspx, voert de volgende stappen uit om op te lossen van de DNS-naam en een verbinding tot stand brengen van de client:

![Verbinding tot stand brengen met Traffic Manager][2]

1. De client verzendt een DNS-query naar de geconfigureerde recursieve DNS-service om op te lossen van de naam 'partners.contoso.com'. Een recursieve DNS-service, ook wel een lokale DNS-service, is niet rechtstreeks host DNS-domeinen. De client off-loads in plaats daarvan het werk van het contact opnemen met de verschillende gezaghebbende DNS-services via het Internet die nodig zijn om op te lossen een DNS-naam.
2. U kunt oplossen door de DNS-naam, zoekt de recursieve DNS-service de naamservers voor het domein 'contoso.com'. Het neemt vervolgens contact op deze naamservers om aan te vragen van de DNS-record 'partners.contoso.com'. De DNS-servers voor contoso.com retourneren de CNAME-record die naar contoso.trafficmanager.net verwijst.
3. Vervolgens zoekt de recursieve DNS-service de naamservers voor het domein 'trafficmanager.net', die worden geleverd door de service Azure Traffic Manager. Vervolgens stuurt een aanvraag voor de DNS-record 'contoso.trafficmanager.net' naar deze DNS-servers.
4. De aanvraag wordt ontvangen door de naamservers van Traffic Manager. Ze kiezen een eindpunt op basis van:

    - De geconfigureerde status van elk eindpunt (uitgeschakelde eindpunten worden niet geretourneerd)
    - De huidige status van elk eindpunt, zoals wordt bepaald door de status van Traffic Manager controleert. Zie voor meer informatie, [Traffic Manager-eindpunt bewaking](traffic-manager-monitoring.md).
    - De routering van verkeer gekozen methode. Zie voor meer informatie, [methoden voor Traffic Manager Routing](traffic-manager-routing-methods.md).

5. Het eindpunt van de gekozen wordt als een andere DNS-CNAME-record geretourneerd. In dit geval laat het ons Stel contoso-us.cloudapp.net wordt geretourneerd.
6. Vervolgens zoekt de recursieve DNS-service de naamservers voor het domein 'cloudapp.net'. Het neemt contact op met de namen van servers om aan te vragen de contoso-us.cloudapp .net DNS-record. Een DNS-'A'-record met het IP-adres van de VS gevestigde service-eindpunten wordt geretourneerd.
7. De recursieve DNS-service de resultaten worden geconsolideerd en retourneert één DNS-antwoord naar de client.
8. De client ontvangt de resultaten van de DNS- en maakt verbinding met het opgegeven IP-adres. De client maakt verbinding met het service-eindpunt van de toepassing rechtstreeks, niet via Traffic Manager. Omdat dit een HTTPS-eindpunt, de client voert de vereiste SSL/TLS-handshake en maakt vervolgens een HTTP GET-aanvraag voor de ' / login.aspx' pagina.

De recursieve DNS-service slaat de DNS-antwoorden die deze ontvangt. Het resultaat van de DNS-resolver op het clientapparaat worden ook opgeslagen. In cache opslaan, kunt volgende DNS-query's kunnen sneller worden beantwoord door gegevens uit de cache gebruiken in plaats van query's op andere naamservers. De duur van de cache wordt bepaald door de eigenschap (TTL) 'time-to-live' van elke DNS-record. Kortere waarden resulteren in sneller cache verlopen en dus meer retouren naar de naamservers van Traffic Manager. Meer waarden betekenen dat het langer voor verkeer van een eindpunt is mislukt duren kan. Traffic Manager kunt u het configureren van de TTL-waarde die wordt gebruikt in Traffic Manager-DNS-antwoorden worden zo laag 0 seconden en zo hoog 2.147.483.647 seconden (compatibel zijn met het maximumbereik [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt)), zodat u kunt de waarde kiezen die het meest Geeft een balans tussen de behoeften van uw toepassing.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over Traffic Manager [eindpunt bewakings- en automatische failover](traffic-manager-monitoring.md).

Meer informatie over Traffic Manager [verkeersrouteringsmethoden](traffic-manager-routing-methods.md).

<!--Image references-->
[1]: ./media/traffic-manager-how-traffic-manager-works/dns-configuration.png
[2]: ./media/traffic-manager-how-traffic-manager-works/flow.png

