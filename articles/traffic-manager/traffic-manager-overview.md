---
title: Wat is er Traffic Manager | Microsoft Docs
description: Dit artikel helpt u begrijpen wat Traffic Manager is en of deze de juiste traffic routing keuze voor uw toepassing
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 75d5ff9a-f4b9-4b05-af32-700e7bdfea5a
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/15/2017
ms.author: kumud
ms.openlocfilehash: 50d7f14d0d4234ee98d8a46e903b5f916cb02fab
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="overview-of-traffic-manager"></a>Overzicht van Traffic Manager

Microsoft Azure Traffic Manager kunt u bepalen van de distributie van gebruikersverkeer voor service-eindpunten in verschillende datacenters. Service-eindpunten die worden ondersteund door Traffic Manager bevatten Azure virtuele machines, Web-Apps en cloudservices. U kunt Traffic Manager ook gebruiken met externe eindpunten die niet van Azure zijn.

Traffic Manager maakt gebruik van de Domain Name System (DNS) om te leiden aanvragen van clients naar de meest geschikte eindpunt op basis van een methode voor verkeersroutering en de status van de eindpunten. Traffic Manager biedt een reeks [verkeersroutering methoden](traffic-manager-routing-methods.md) en [eindpunt controle-opties](traffic-manager-monitoring.md) aanpassen aan de behoeften van verschillende groepen van toepassingen en automatische failover-modellen. Traffic Manager is mislukt, met inbegrip van het uitvallen van een volledige Azure-regio tegen.

## <a name="traffic-manager-benefits"></a>Voordelen van het Traffic Manager

Traffic Manager kan u helpen:

* **Verbeter de beschikbaarheid van bedrijfskritieke toepassingen**

    Traffic Manager biedt hoge beschikbaarheid voor uw toepassingen aan de bewaking van uw eindpunten en automatische failover te bieden wanneer een eindpunt uitvalt.

* **Reactietijd voor toepassingen met hoge prestaties te verbeteren**

    Kunt u met Azure cloudservices of websites in datacenters over de hele wereld worden uitgevoerd. Traffic Manager verbetert de reactietijd van toepassing door het verkeer naar het eindpunt met de laagste netwerklatentie voor de client.

* **Uitvoeren van serviceonderhoud zonder uitvaltijd**

    U kunt geplande onderhoudsbewerkingen uitvoeren op uw toepassingen zonder uitvaltijd. Traffic Manager stuurt het verkeer naar alternatieve eindpunten terwijl het onderhoud uitgevoerd wordt.

* **Combineren van on-premises en cloudtoepassingen**

    Traffic Manager biedt ondersteuning voor externe, niet-Azure-eindpunten inschakelen om te worden gebruikt met hybride cloud en on-premises implementaties, met inbegrip van de 'burst-naar-cloud,' 'migreren-naar-cloud,' en 'failover cloud'-scenario's.

* **Verkeer voor grote, complexe implementaties distribueren**

    Met behulp van [Traffic Manager-profielen genest](traffic-manager-nested-profiles.md), verkeersroutering methoden voor het maken van geavanceerde en flexibele regels ter ondersteuning van de behoeften van grotere, complexe implementaties kunnen worden gecombineerd.

## <a name="how-traffic-manager-works"></a>Hoe Traffic Manager werkt

Azure Traffic Manager kunt u het beheer van de distributie van verkeer tussen de eindpunten van uw toepassing. Een eindpunt is een internetgerichte service die wordt gehost binnen of buiten Azure.

Traffic Manager biedt twee belangrijke voordelen:

1. Distributie van verkeer volgens een van de [verkeersroutering methoden](traffic-manager-routing-methods.md)
2. [Doorlopende bewaking van de status van endpoint](traffic-manager-monitoring.md) en automatische failover wanneer eindpunten mislukken

Wanneer een client probeert verbinding maken met een service, moet deze eerst de DNS-naam van de service leiden naar een IP-adres. De client maakt vervolgens verbinding met dat IP-adres voor toegang tot de service.

**Het meest belangrijk punt om te begrijpen is dat het Traffic Manager op het niveau van DNS werkt.**  Traffic Manager gebruikt DNS om clients omleiden naar een specifieke service-eindpunten op basis van de regels van de methode verkeer routering. Clients verbinding maken met het geselecteerde eindpunt **rechtstreeks**. Traffic Manager is niet een proxy of gateway. Traffic Manager is niet zichtbaar voor het verkeer tussen de client en de service wordt doorgegeven.

### <a name="traffic-manager-example"></a>Traffic Manager-voorbeeld

Contoso Corp hebben een nieuwe Partnerportal ontwikkeld. De URL voor deze portal is https://partners.contoso.com/login.aspx. De toepassing wordt gehost in drie gebieden van Azure. Verbeter de beschikbaarheid en prestaties globale, ze Traffic Manager gebruiken om clientverkeer naar de dichtstbijzijnde beschikbare eindpunt te distribueren.

Voor deze configuratie, uitvoeren ze de volgende stappen:

1. Drie exemplaren van de service implementeren. De DNS-namen van deze implementaties zijn 'contoso-us.cloudapp .net', 'contoso-eu.cloudapp .net' en 'contoso-asia.cloudapp .net'.
2. Maak een Traffic Manager-profiel met de naam 'contoso.trafficmanager.net', en configureer deze voor het gebruik van de methode 'Prestaties' Routering van netwerkverkeer tussen de drie eindpunten.
* Configureer hun domeinnaam vanity, 'partners.contoso.com' om te verwijzen naar contoso.trafficmanager.net, met een DNS CNAME-record.

![Traffic Manager-DNS-configuratie][1]

> [!NOTE]
> Wanneer u een vanity-domein met Azure Traffic Manager, moet u een CNAME uw aangepaste domeinnaam verwijzen naar de domeinnaam van uw Traffic Manager. DNS-standaarden staan niet toe dat u een CNAME 'top' (of hoofdmap) van een domein maken. U kunt geen dus een CNAME voor contoso.com (ook wel een domein, open' genoemd) maken. U kunt alleen een CNAME voor een domein onder 'contoso.com', zoals 'www.contoso.com' maken. U kunt deze beperking omzeilen, wordt u aangeraden een eenvoudige HTTP-omleiding op directe aanvragen voor 'contoso.com' naar een alternatieve naam zoals 'www.contoso.com'.

### <a name="how-clients-connect-using-traffic-manager"></a>Hoe clients verbinding maken met behulp van Traffic Manager

U doorgaat van het vorige voorbeeld, wanneer een client de https://partners.contoso.com/login.aspx pagina aanvraagt, voert de client de volgende stappen voor het oplossen van de DNS-naam en een verbinding tot stand brengen:

![Verbinding tot stand brengen met Traffic Manager][2]

1. De client verzendt een DNS-query naar de geconfigureerde recursieve DNS-service omzetten van de naam partners.contoso.com. DNS-domeinen is niet rechtstreeks host van een recursieve DNS-service, ook wel een lokale DNS-service wordt genoemd. De client off-loads in plaats daarvan het werk van de verbinding met de verschillende gezaghebbende DNS-services via het Internet die nodig zijn voor het oplossen van een DNS-naam.
2. U kunt oplossen door de DNS-naam, zoekt de recursieve DNS-service de naamservers voor het domein 'contoso.com'. Deze vraagt vervolgens deze naamservers om aan te vragen van de 'partners.contoso.com' DNS-record. De contoso.com DNS-servers retourneren de CNAME-record die naar contoso.trafficmanager.net verwijst.
3. De recursieve DNS-service zoekt vervolgens naar de naamservers voor het domein 'trafficmanager.net', die worden geleverd door de service Azure Traffic Manager. Deze stuurt een aanvraag voor de 'contoso.trafficmanager.net' DNS-record voor deze DNS-servers.
4. De aanvraag wordt ontvangen door de naamservers Traffic Manager. Ze kiezen een eindpunt op basis van:

    - De geconfigureerde status van elk eindpunt (uitgeschakelde eindpunten worden niet geretourneerd)
    - De huidige status van elk eindpunt, zoals wordt bepaald door de status van Traffic Manager controleert. Zie voor meer informatie [Traffic Manager-eindpunt bewaking](traffic-manager-monitoring.md).
    - De gekozen verkeersroutering methode. Zie voor meer informatie [Traffic Manager routering methoden](traffic-manager-routing-methods.md).

5. Het gekozen eindpunt wordt als een andere DNS CNAME-record geretourneerd. In dit geval laat het ons Stel contoso us.cloudapp.net wordt geretourneerd.
6. De recursieve DNS-service zoekt vervolgens naar de naamservers voor het domein 'cloudapp.net'. Het contact opneemt met die naamservers om aan te vragen van de contoso-us.cloudapp .net DNS-record. Een DNS-'A'-record met het IP-adres van de VS gebaseerde service-eindpunt wordt geretourneerd.
7. De recursieve DNS-service de resultaten worden geconsolideerd en retourneert één DNS-antwoord naar de client.
8. De client ontvangt de DNS-resultaten en verbinding maakt met het opgegeven IP-adres. De client verbinding maakt met het service-eindpunt voor toepassing rechtstreeks, niet via het Traffic Manager. Omdat het een HTTPS-eindpunt, de client voert de vereiste SSL/TLS-handshake en maakt vervolgens een HTTP GET-aanvraag voor de ' / login.aspx' pagina.

De recursieve DNS-service in de cache opgeslagen voor de DNS-antwoorden die het ontvangt. Het resultaat van de DNS-resolver op het clientapparaat worden ook opgeslagen. Opslaan in cache kunt volgende DNS-query's sneller worden beantwoord door gegevens uit de cache kan gebruiken in plaats van query's op de naamservers van andere. De duur van de cache wordt bepaald door de eigenschap (TTL) 'time-to-live' van elke DNS-record. Kortere waarden resulteren in snellere verstrijken van de cache en dus meer retouren naar de naamservers Traffic Manager. Langer waarden betekenen dat het langer voor directe verkeer weg van een mislukte-eindpunt duren kan. Traffic Manager kunt u voor het configureren van de TTL-waarde gebruikt in Traffic Manager-DNS-antwoorden worden zo laag 0 seconden en zo hoog 2.147.483.647 seconden (compatibel zijn met het maximumbereik [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt)), zodat u de waarde kiezen die het meest een compromis tussen de behoeften van uw toepassing.

## <a name="pricing"></a>Prijzen

Zie voor informatie over prijzen, [Traffic Manager prijzen](https://azure.microsoft.com/pricing/details/traffic-manager/).

## <a name="faq"></a>Veelgestelde vragen

Zie voor veelgestelde vragen over Traffic Manager [Traffic Manager Veelgestelde vragen](traffic-manager-FAQs.md)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het Traffic Manager [eindpunt bewakings- en automatische failover](traffic-manager-monitoring.md).

Meer informatie over het Traffic Manager [verkeersrouteringsmethoden](traffic-manager-routing-methods.md).

Informatie over enkele van de andere belangrijke [netwerkmogelijkheden](../networking/networking-overview.md) van Azure.

<!--Image references-->
[1]: ./media/traffic-manager-how-traffic-manager-works/dns-configuration.png
[2]: ./media/traffic-manager-how-traffic-manager-works/flow.png

