---
title: Dynamische Site-versnelling via Azure CDN
description: Dynamische site-versnelling diepgaand
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: v-semcev
ms.openlocfilehash: be2719e0e02c8bc69800ef4a3e7da3c3164cb9dd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="dynamic-site-acceleration-via-azure-cdn"></a>Dynamische Site-versnelling via Azure CDN

Met de explosie van sociale media, elektronische transacties en het web hyper persoonlijke snel toenemende percentage van de inhoud aangeboden aan eindgebruikers gegenereerd in realtime. Gebruikers verwachten snelle, betrouwbare en gepersonaliseerde webervaringen, onafhankelijk van hun browser, locatie, apparaat of netwerk. De zeer vernieuwingen waaruit deze dus ook bezighouden ervaringen trage pagina downloads en de risico voor de kwaliteit van de consumer-ervaring. 

Standaard CDN-mogelijkheid omvat de mogelijkheid om cachebestanden dichter voor eindgebruikers te versnellen levering van statische bestanden. Echter met dynamische webtoepassingen die inhoud op rand locaties opslaan in cache is niet mogelijk omdat de server de inhoud in reactie op gebruikersgedrag genereert. Versnellen van de levering van deze inhoud is complexer dan traditionele edge cache en een end-to-end-oplossing die geoptimaliseerd voor elk element in het hele gegevenspad van begin tot levering vereist. Met Azure CDN dynamische Site Acceleration (DSA), is de prestaties van webpagina's met dynamische inhoud aantoonbaar verbeterd.

Azure CDN van Akamai en Verizon biedt DSA optimalisatie via de **geoptimaliseerd voor** menu tijdens het maken van het eindpunt.

## <a name="configuring-cdn-endpoint-to-accelerate-delivery-of-dynamic-files"></a>CDN-eindpunt om te versnellen levering van dynamische bestanden configureren

Uw CDN-eindpunt voor het optimaliseren van de levering van dynamische bestanden via Azure portal door te selecteren kunt u de **dynamische site-versnelling** optie onder de **geoptimaliseerd voor** eigenschap selecteren tijdens de maken van het eindpunt. U kunt ook onze REST-API's of een van de client-SDK's gebruiken hetzelfde programmatisch doen. 

### <a name="probe-path"></a>Pad van de test
Test-pad is een functie die specifiek zijn voor de dynamische Site-versnelling en een geldig is vereist voor het maken van. DSA maakt gebruik van een klein *test pad* bestand op de oorsprong optimaliseren van routering netwerkconfiguraties voor de CDN geplaatst. U kunt downloaden en onze voorbeeldbestand uploaden naar uw site of een bestaande asset gebruiken op uw oorsprong is ongeveer 10 KB voor de test-pad in plaats daarvan als de asset bestaat.

> [!Note]
> DSA extra kosten in rekening worden gebracht. Zie voor meer informatie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cdn/) voor meer informatie.

De volgende schermafbeeldingen illustreren het proces via de Azure-portal.
 
![Een nieuw CDN-eindpunt toevoegen](./media/cdn-dynamic-site-acceleration/01_Endpoint_Profile.png) 

*Afbeelding 1: Het toevoegen van een nieuw CDN-eindpunt van het CDN-profiel*
 
![Het maken van een nieuw CDN-eindpunt met DSA](./media/cdn-dynamic-site-acceleration/02_Optimized_DSA.png)  

*Afbeelding 2: Het maken van een CDN-eindpunt met dynamische site-versnelling optimalisatie geselecteerd*

Zodra het CDN-eindpunt is gemaakt, wordt het toegepast de DSA-optimalisaties voor alle bestanden die aan bepaalde criteria voldoen. De volgende sectie wordt de DSA-optimalisatie in detail beschreven.

## <a name="dsa-optimization-using-azure-cdn"></a>DSA optimalisatie met behulp van Azure CDN

Dynamische Site-versnelling op Azure CDN wordt versneld levering van dynamische elementen met behulp van de volgende technieken:

-   Route optimalisatie
-   Optimalisaties voor TCP
-   Object Prefetch (alleen Akamai)
-   Compressie mobiele afbeelding (alleen Akamai)

### <a name="route-optimization"></a>Route optimalisatie

Optimalisatie van de route is belangrijk, omdat het Internet een dynamische plaats is, waar verkeer en tijdelijk storingen voortdurend de netwerktopologie verandert zijn. Het Border Gateway Protocol (BGP) is het routeringsprotocol van Internet, maar er is mogelijk sneller routes via een tussenliggende punt aanwezigheid (PoP)-servers. 

Route optimalisatie kiest dat het meest optimale pad naar de oorsprong zodat een site voortdurend toegankelijk en dynamische inhoud wordt aan eindgebruikers via de snelste en betrouwbaarste route mogelijk wordt geleverd. 

Het netwerk Akamai gebruikt technieken voor het verzamelen van real-time gegevens en verschillende paden via andere knooppunten in de server Akamai, evenals de standaardroute BGP via het open Internet om te bepalen van de snelste manier tussen de oorsprong en de rand CDN vergelijken. Deze technieken te voorkomen dat Internet congestie punten en lang routes. 

Op deze manier Verizon netwerk gebruikt een combinatie van Anycast DNS, hoge capaciteit ondersteuning POP's en statuscontroles, om te bepalen van de beste gateways voor de beste routegegevens van de client naar de oorsprong.
 
Als gevolg hiervan volledig dynamische en transactionele leveren van inhoud sneller en betrouwbaarder aan eindgebruikers, zelfs wanneer het uncacheable. 

### <a name="tcp-optimizations"></a>Optimalisaties voor TCP

Transmission Control Protocol (TCP) is de standaard van de Internet-protocolsuite gebruikt voor het leveren van gegevens tussen toepassingen op een IP-netwerk.  Er zijn standaard verschillende heen en weer aanvragen een TCP-verbinding, evenals de beperkingen instellen moeten om te voorkomen dat netwerk congestions, waardoor het inefficiëntie op grote schaal. Azure CDN van Akamai omgaat met dit probleem door te optimaliseren in drie gebieden: 

 - Trage start elimineren
 - Gebruik van permanente verbindingen
 - afstemming van TCP-pakket parameters (alleen Akamai)

#### <a name="eliminating-slow-start"></a>Trage start elimineren

*Trage start* deel uitmaakt van het TCP-protocol waarmee wordt voorkomen opstoppingen in het netwerk dat door het beperken van de hoeveelheid gegevens die via het netwerk worden verzonden. Deze begint uitschakelen met kleine congestie venstergrootte tussen zender en ontvanger totdat het maximum is bereikt of pakketverlies wordt gedetecteerd.

Azure CDN van Akamai en Verizon elimineert trage start in drie stappen:

1.  Zowel Akamai van Verizon netwerk gebruiken status en de bewaking van de bandbreedte voor het meten van de bandbreedte van de verbindingen tussen randservers pop-server.
2. De metrische gegevens worden gedeeld tussen edge PoP-servers, zodat elke server op de hoogte van de netwerk-voorwaarden en de serverstatus van de andere POP's omheen is.  
3. De CDN edge-servers zijn nu kunnen aanbrengen veronderstellingen over sommige transmission-parameters, zoals de optimale venstergrootte moet tijdens de communicatie met andere servers van de rand CDN in de buurt. Deze stap betekent dat de grootte van het eerste opstoppingen in het venster kan worden verhoogd als de status van de verbinding tussen de servers van de rand CDN hoger pakket gegevensoverdrachten staat is.  

#### <a name="leveraging-persistent-connections"></a>Gebruik van permanente verbindingen

Met behulp van een CDN minder unieke computers verbinding maken met uw oorsprong-server rechtstreeks wordt vergeleken met gebruikers direct verbinding maken met uw oorsprong. Azure CDN van Akamai en Verizon groep ook gebruikersaanvragen samen te stellen minder verbindingen met de oorsprong.

Zoals eerder gezegd, netwerkverzoeken TCP-verbindingen verschillende heen en weer in een handshake tot stand brengen van een nieuwe verbinding. Permanente verbindingen, ook wel bekend als ' HTTP Keep-Alive, ' opnieuw gebruiken van bestaande TCP-verbindingen naar meerdere HTTP-aanvragen op te slaan tijden geretourneerd versnellen levering. 

Het netwerk Verizon verzendt ook periodieke keepalive-pakketten via de TCP-verbinding om te voorkomen dat een open verbinding wordt afgesloten.

#### <a name="tuning-tcp-packet-parameters"></a>Afstemming van TCP-pakket parameters

Azure CDN van Akamai ook de nummers de parameters die betrekking op server-naar-server verbindingen en reduceert de hoeveelheid lange afstand retouren vereist voor het ophalen van inhoud is ingesloten in de site met behulp van de volgende technieken:

1.  Vergroot het venster initiële congestie zodat meer pakketten kunnen worden verzonden zonder te wachten op een bevestiging.
2.  De time-out voor de eerste retransmit verlagen zodat een verlies wordt gedetecteerd en herverzending sneller vindt plaats.
3.  De time-out voor de minimale en maximale retransmit om te verminderen, de wachttijd voordat wordt aangenomen dat pakketten verloren zijn gegaan in overdracht verlagen.

### <a name="object-prefetch-akamai-only"></a>Object Prefetch (alleen Akamai)

De meeste websites bestaan uit een HTML-pagina die verwijst naar diverse andere resources, zoals afbeeldingen en scripts. Normaal gesproken wanneer een client een webpagina aanvraagt, de browser downloadt eerst parseert de HTML-object en maakt vervolgens extra aanvragen tot gekoppelde bedrijfsmiddelen die nodig zijn voor de pagina volledig is geladen. 

*Prefetch* is een techniek voor het ophalen van afbeeldingen en scripts ingesloten in de HTML-pagina, terwijl u de HTML-code naar de browser wordt verzonden en voordat de browser zelfs deze aanvragen object maakt. 

Met de **prefetch** optie ingeschakeld op het moment waarop de CDN fungeert de base HTML-pagina naar de clientbrowser, de CDN parseert het HTML-bestand en zorg aanvullende aanvragen voor alle gekoppelde resources en op te slaan in de cache. Wanneer de client de aanvragen voor de gekoppelde activa, wordt de CDN edge-server al heeft van de gevraagde objecten en kan fungeren ze onmiddellijk zonder een retouren naar de oorsprong. Deze optimalisatie voordelen biedt voor caching geschikte en niet-caching geschikte inhoud.

### <a name="adaptive-image-compression-akamai-only"></a>Adaptieve compressie (alleen Akamai)

Op sommige apparaten, met name mobiele die ervaren tragere netwerksnelheden van tijd tot tijd. In deze scenario's is het meer nuttig zijn voor de gebruiker kleinere afbeeldingen sneller ontvangen in hun webpagina in plaats van een lange wachttijd voor installatiekopieën van de volledige resolutie.

Deze functie automatisch bewaakt netwerk kwaliteit en JPEG-compressie standaardmethoden veiligheidsmaatregelen wanneer netwerksnelheden trager leveringstijd verbeteren.

Adaptieve compressie | Bestandsextensies  
--- | ---  
JPEG-compressie | JPG, JPEG, .jpe, .jig, .jgig, .jgi

## <a name="caching"></a>Caching

Met DSA, is opslaan in cache standaard uitgeschakeld op de CDN zelfs wanneer de oorsprong cache-control/verloopt kopteksten in het antwoord bevat. Deze standaard is uitgeschakeld omdat DSA doorgaans voor dynamische activa dat mag niet worden opgeslagen gebruikt wordt omdat ze uniek voor elke client zijn en cache standaard ingeschakeld op dit gedrag kunt verbreken.

Als u een website met een combinatie van statische en dynamische activa hebt, is het beste een hybride-benadering voor de beste prestaties. 

Als u van ADN met Premium van Verizon gebruikmaakt, kunt u opnieuw in de cache voor specifieke gevallen de regels-engine.  

Een alternatief is het gebruik van CDN-eindpunten. Een met DSA dynamische activa en een ander eindpunt met een statische optimalisatie-type, zoals de levering van algemene webtoepassingen voor caching geschikte bedrijfsmiddelen levering leveren. Om dit alternatief bereiken, wijzigt u de URL van uw webpagina directe koppeling naar de asset op het CDN-eindpunt dat u wilt gebruiken. 

Bijvoorbeeld: `mydynamic.azureedge.net/index.html` is een dynamische pagina en is geladen vanuit het DSA-eindpunt.  De html-pagina verwijst naar meerdere statische elementen zoals JavaScript-bibliotheken of installatiekopieën die worden geladen vanuit het statische CDN-eindpunt, zoals `mystatic.azureedge.net/banner.jpg` en `mystatic.azureedge.net/scripts.js`. 

U vindt een voorbeeld [hier](https://docs.microsoft.com/azure/cdn/cdn-cloud-service-with-cdn#controller) op het gebruik van domeincontrollers in een ASP.NET-webtoepassing voor het leveren van inhoud via een specifieke CDN-URL.




