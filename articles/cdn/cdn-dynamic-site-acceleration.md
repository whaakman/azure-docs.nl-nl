---
title: Dynamische site-versnelling via Azure CDN
description: Azure CDN biedt ondersteuning voor dynamische siteregistratie acceleration (DSA) optimalisatie voor bestanden met dynamische inhoud.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2018
ms.author: rli
ms.openlocfilehash: d105c88105512df4a9f8d999f64ad001b5d54917
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2018
---
# <a name="dynamic-site-acceleration-via-azure-cdn"></a>Dynamische site-versnelling via Azure CDN

Met de explosie van sociale media, elektronische transacties en het web hyper persoonlijke snel toenemende percentage van de inhoud aangeboden aan eindgebruikers gegenereerd in realtime. Gebruikers verwachten dat een snelle, betrouwbare en persoonlijke webervaring, onafhankelijk van hun browser, locatie, apparaat of netwerk. De zeer vernieuwingen waaruit deze dus ook bezighouden ervaringen trage pagina downloads en de risico voor de kwaliteit van de consumer-ervaring. 

Standaard content delivery network (CDN) mogelijkheid omvat de mogelijkheid om cachebestanden dichter voor eindgebruikers te versnellen levering van statische bestanden. Echter met dynamische webtoepassingen die inhoud op rand locaties opslaan in cache is niet mogelijk omdat de server de inhoud in reactie op gebruikersgedrag genereert. Versnellen van de levering van deze inhoud is complexer dan traditionele edge cache en een end-to-end-oplossing die geoptimaliseerd voor elk element in het hele gegevenspad van begin tot levering vereist. Met optimalisatie van Azure CDN dynamische site-versnelling (DSA), is de prestaties van webpagina's met dynamische inhoud aantoonbaar verbeterd.

**Azure CDN van Akamai** en **Azure CDN van Verizon** bieden een DSA optimalisatie via de **geoptimaliseerd voor** menu tijdens het maken van het eindpunt.

> [!Important]
> Voor **Azure CDN van Akamai** profielen, zijn toegestaan voor de optimalisatie van een CDN-eindpunt niet wijzigen nadat deze is gemaakt.
>   
> Voor **Azure CDN van Verizon** profielen, u niet wijzigen de optimalisatie van een CDN-eindpunt nadat deze is gemaakt.

## <a name="configuring-cdn-endpoint-to-accelerate-delivery-of-dynamic-files"></a>CDN-eindpunt om te versnellen levering van dynamische bestanden configureren

Voor het configureren van een CDN-eindpunt voor het optimaliseren van de levering van dynamische bestanden, kunt u de Azure-portal, de REST API's of een van de client-SDK's ofwel gebruik voor hetzelfde programmatisch. 

**Een CDN-eindpunt voor DSA optimalisatie configureren met behulp van de Azure-portal:**

1. In de **CDN-profiel** pagina **eindpunt**.

   ![Een nieuw CDN-eindpunt toevoegen](./media/cdn-dynamic-site-acceleration/cdn-endpoint-profile.png) 

   Het deelvenster **Een eindpunt toevoegen** wordt weergegeven.

2. Onder **geoptimaliseerd voor**, selecteer **dynamische site-versnelling**.

    ![Maak een nieuw CDN-eindpunt met DSA](./media/cdn-dynamic-site-acceleration/cdn-endpoint-dsa.png)

3. Voor **test pad**, voer een geldig pad naar een bestand.

    Test-pad is een functie die specifiek zijn voor DSA en een geldig pad is vereist voor het maken van. DSA maakt gebruik van een klein *test pad* bestand op de oorspronkelijke server te optimaliseren van routering netwerkconfiguraties voor de CDN geplaatst. Voor het bestand van het pad test, kunt u downloaden en het voorbeeldbestand uploaden naar uw site of een bestaande asset gebruiken op uw oorsprong die ongeveer 10 KB groot is.

4. Voer de overige vereiste endpoint-opties (Zie voor meer informatie [een nieuw CDN-eindpunt maken](cdn-create-new-endpoint.md#create-a-new-cdn-endpoint)), selecteer daarna **toevoegen**.

   Nadat het CDN-eindpunt is gemaakt, wordt het toegepast de DSA-optimalisaties voor alle bestanden die aan bepaalde criteria voldoen. 


**Een bestaand eindpunt configureren voor DSA (Azure CDN van Akamai profielen alleen):**

1. In de **CDN-profiel** pagina, selecteert u het eindpunt dat u wilt wijzigen.

2. Selecteer in het linkerdeelvenster **optimalisatie**. 

   De **optimalisatie** pagina wordt weergegeven.

3. Onder **geoptimaliseerd voor**, selecteer **dynamische site-versnelling**, selecteer daarna **opslaan**.

> [!Note]
> DSA extra kosten in rekening worden gebracht. Zie voor meer informatie [Content Delivery Network prijzen](https://azure.microsoft.com/pricing/details/cdn/).

## <a name="dsa-optimization-using-azure-cdn"></a>DSA optimalisatie met behulp van Azure CDN

Dynamische Site-versnelling op Azure CDN sneller levering van dynamische elementen met behulp van de volgende technieken:

-   [Route optimalisatie](#route-optimization)
-   [Optimalisaties voor TCP](#tcp-optimizations)
-   [Object prefetch (Azure CDN van Akamai alleen)](#object-prefetch-azure-cdn-from-akamai-only)
-   [Adaptieve compressie (Azure CDN van Akamai alleen)](#adaptive-image-compression-azure-cdn-from-akamai-only)

### <a name="route-optimization"></a>Route optimalisatie

Optimalisatie van de route is belangrijk, omdat het Internet een dynamische plaats is, waar verkeer en tijdelijk storingen voortdurend de netwerktopologie verandert zijn. Het Border Gateway Protocol (BGP) is het routeringsprotocol van Internet, maar er is mogelijk sneller routes via een tussenliggende punt aanwezigheid (PoP)-servers. 

Route optimalisatie kiest dat het meest optimale pad naar de oorsprong zodat een site voortdurend toegankelijk en dynamische inhoud wordt aan eindgebruikers via de snelste en betrouwbaarste route mogelijk wordt geleverd. 

Het netwerk Akamai gebruikt technieken voor het verzamelen van real-time gegevens en verschillende paden via andere knooppunten in de server Akamai, evenals de standaardroute BGP via het open Internet om te bepalen van de snelste manier tussen de oorsprong en de rand CDN vergelijken. Deze technieken te voorkomen dat Internet congestie punten en lang routes. 

Op deze manier Verizon netwerk gebruikt een combinatie van Anycast DNS, hoge capaciteit ondersteuning POP's en statuscontroles, om te bepalen van de beste gateways voor de beste routegegevens van de client naar de oorsprong.
 
Als gevolg hiervan volledig dynamische en transactionele leveren van inhoud sneller en betrouwbaarder aan eindgebruikers, zelfs wanneer het uncacheable. 

### <a name="tcp-optimizations"></a>Optimalisaties voor TCP

Transmission Control Protocol (TCP) is de standaard van de Internet-protocolsuite gebruikt voor het leveren van gegevens tussen toepassingen op een IP-netwerk.  Standaard zijn diverse en weer schakelen aanvragen vereist een TCP-verbinding, evenals de beperkingen instellen om te voorkomen dat netwerk congestions, waardoor het inefficiëntie op grote schaal. **Azure CDN van Akamai** verwerkt van dit probleem door te optimaliseren in drie gebieden: 

 - [Trage start TCP elimineren](#eliminating-tcp-slow-start)
 - [Gebruik van permanente verbindingen](#leveraging-persistent-connections)
 - [Afstemming van TCP-pakket parameters](#tuning-tcp-packet-parameters)

#### <a name="eliminating-tcp-slow-start"></a>Trage start TCP elimineren

TCP *start trage* is een algoritme van de TCP-protocol waarmee wordt voorkomen opstoppingen in het netwerk dat door het beperken van de hoeveelheid gegevens die via het netwerk worden verzonden. Deze begint uitschakelen met kleine congestie venstergrootte tussen zender en ontvanger totdat het maximum is bereikt of pakketverlies wordt gedetecteerd.

 Beide **Azure CDN van Akamai** en **Azure CDN van Verizon** profielen elimineren TCP trage beginnen met de volgende drie stappen:

1. Status en de bewaking van de bandbreedte wordt gebruikt voor het meten van de bandbreedte van de verbindingen tussen randservers pop-server.
    
2. Metrische gegevens worden gedeeld tussen edge PoP-servers, zodat elke server op de hoogte van de netwerk-voorwaarden en de serverstatus van de andere POP's omheen is.  
    
3. De randservers CDN zorg veronderstellingen over sommige transmission-parameters, zoals de optimale venstergrootte moet tijdens de communicatie met andere servers van de rand CDN in de buurt. Deze stap betekent dat de grootte van het eerste opstoppingen in het venster kan worden verhoogd als de status van de verbinding tussen de servers van de rand CDN hoger pakket gegevensoverdrachten staat is.  

#### <a name="leveraging-persistent-connections"></a>Gebruik van permanente verbindingen

Met behulp van een CDN minder unieke computers verbinding maken met uw oorsprong-server rechtstreeks wordt vergeleken met gebruikers direct verbinding maken met uw oorsprong. Azure CDN groep ook gebruikersaanvragen samen te stellen minder verbindingen met de oorsprong.

Zoals eerder vermeld, worden diverse handshake aanvragen vereist een TCP-verbinding tot stand brengen. Permanente verbindingen worden geïmplementeerd door de `Keep-Alive` HTTP-header, bestaande TCP-verbindingen naar meerdere HTTP-aanvragen op te slaan tijden geretourneerd versnellen levering opnieuw gebruiken. 

**Azure CDN van Verizon** verzendt ook periodieke keepalive-pakketten via de TCP-verbinding om te voorkomen dat een open verbinding wordt afgesloten.

#### <a name="tuning-tcp-packet-parameters"></a>Afstemming van TCP-pakket parameters

**Azure CDN van Akamai** de parameters die server naar server verbindingen van toepassing verbetert de prestatie- en reduceert de hoeveelheid lange afstand retouren vereist voor het ophalen van inhoud die is ingesloten in de site met behulp van de volgende technieken:

- Vergroot het venster initiële congestie zodat meer pakketten kunnen worden verzonden zonder te wachten op een bevestiging.
- De time-out voor de eerste retransmit verlagen zodat een verlies wordt gedetecteerd en herverzending sneller vindt plaats.
- De time-out voor de minimale en maximale retransmit om te verminderen, de wachttijd voordat wordt aangenomen dat pakketten verloren zijn gegaan in overdracht verlagen.

### <a name="object-prefetch-azure-cdn-from-akamai-only"></a>Object prefetch (Azure CDN van Akamai alleen)

De meeste websites bestaan uit een HTML-pagina die verwijst naar diverse andere resources, zoals afbeeldingen en scripts. Normaal gesproken wanneer een client een webpagina aanvraagt, de browser downloadt eerst parseert de HTML-object en maakt vervolgens extra aanvragen tot gekoppelde bedrijfsmiddelen die nodig zijn voor de pagina volledig is geladen. 

*Prefetch* is een techniek voor het ophalen van afbeeldingen en scripts ingesloten in de HTML-pagina, terwijl u de HTML-code naar de browser wordt verzonden en voordat de browser zelfs deze aanvragen object maakt. 

Met de optie prefetch is ingeschakeld op het moment waarop de CDN de base HTML-pagina naar de clientbrowser fungeert, de CDN het HTML-bestand wordt geparseerd en aanvullende aanvragen voor alle gekoppelde resources en opslaan in de cache. Wanneer de client de aanvragen voor de gekoppelde activa, wordt de CDN edge-server al heeft van de gevraagde objecten en kan fungeren ze onmiddellijk zonder een retouren naar de oorsprong. Deze optimalisatie voordelen biedt voor caching geschikte en niet-caching geschikte inhoud.

### <a name="adaptive-image-compression-azure-cdn-from-akamai-only"></a>Adaptieve compressie (Azure CDN van Akamai alleen)

Op sommige apparaten, met name mobiele die ervaren tragere netwerksnelheden van tijd tot tijd. In deze scenario's is het meer nuttig zijn voor de gebruiker kleinere afbeeldingen sneller ontvangen in hun webpagina in plaats van een lange wachttijd voor installatiekopieën van de volledige resolutie.

Deze functie automatisch bewaakt netwerk kwaliteit en JPEG-compressie standaardmethoden veiligheidsmaatregelen wanneer netwerksnelheden trager leveringstijd verbeteren.

Adaptieve compressie | Bestandsextensies  
--- | ---  
JPEG-compressie | JPG, JPEG, .jpe, .jig, .jgig, .jgi

## <a name="caching"></a>Caching

Met DSA, opslaan in cache is standaard uitgeschakeld op de CDN, zelfs wanneer de oorsprong bevat `Cache-Control` of `Expires` kopteksten in het antwoord. DSA wordt doorgaans gebruikt voor dynamische activa dat mag niet worden opgeslagen omdat ze uniek voor elke client zijn. Dit gedrag kunt verbreken, opslaan in cache.

Als u een website met een combinatie van statische en dynamische activa hebt, is het beste een hybride-benadering voor de beste prestaties. 

Voor **Azure CDN van Verizon standaard** en **Azure CDN van Akamai standaard** profielen, kunt u inschakelen in cache opslaan voor specifieke DSA-eindpunten met behulp van [regels opslaan in cache](cdn-caching-rules.md).

Toegang krijgen tot cachebewerkingen regels:

1. Van de **CDN-profiel** pagina onder instellingen, selecteert **regels opslaan in cache**.  
    
    ![Knop regels CDN opslaan in cache](./media/cdn-dynamic-site-acceleration/cdn-caching-rules-btn.png)

    De **regels opslaan in cache** pagina wordt geopend.

2. Een globale of aangepaste cacheregel om in te schakelen voor uw eindpunt DSA caching maken. 

Voor **Azure CDN van Verizon Premium** profielen, inschakelen in cache opslaan voor specifieke DSA-eindpunten met behulp van de [regelengine](cdn-rules-engine.md). Alleen de eindpunten van uw profiel die zijn geoptimaliseerd voor DSA van invloed op alle regels die zijn gemaakt. 

Toegang krijgen tot de regelengine:
    
1. Van de **CDN-profiel** pagina **beheren**.  
    
    ![Knop CDN-profiel beheren](./media/cdn-dynamic-site-acceleration/cdn-manage-btn.png)

    Hiermee opent u de CDN-beheerportal.

2. Selecteer in de beheerportal CDN **ADN**, selecteer daarna **regelengine**. 

    ![Regels voor DSA-engine](./media/cdn-dynamic-site-acceleration/cdn-dsa-rules-engine.png)



U kunt ook twee CDN-eindpunten gebruiken: één eindpunt geoptimaliseerd met DSA leveren dynamische activa en een ander eindpunt geoptimaliseerd met een statische optimalisatie-type, zoals algemene web-levering aan levering caching geschikte activa. Wijzig de URL van uw webpagina directe koppeling naar de asset op het CDN-eindpunt dat u wilt gebruiken. 

Bijvoorbeeld: `mydynamic.azureedge.net/index.html` is een dynamische pagina en is geladen vanuit het DSA-eindpunt.  De html-pagina verwijst naar meerdere statische elementen zoals JavaScript-bibliotheken of installatiekopieën die worden geladen vanuit het statische CDN-eindpunt, zoals `mystatic.azureedge.net/banner.jpg` en `mystatic.azureedge.net/scripts.js`. 

Zie voor een voorbeeld over het gebruik van domeincontrollers in een ASP.NET-webtoepassing inhoud via een specifieke CDN URL [inhoud verzorgen vanaf een domeincontroller acties via Azure CDN](https://docs.microsoft.com/azure/cdn/cdn-cloud-service-with-cdn#controller).




