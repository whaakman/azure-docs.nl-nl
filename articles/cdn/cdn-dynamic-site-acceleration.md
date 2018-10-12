---
title: Dynamische siteversnelling via Azure CDN
description: Azure CDN biedt ondersteuning voor dynamic site acceleration (DSA) optimalisatie voor bestanden met dynamische inhoud.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2018
ms.author: magattus
ms.openlocfilehash: 4fa681e800197ea241ba1c6cf2180ba04b6e565b
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49092577"
---
# <a name="dynamic-site-acceleration-via-azure-cdn"></a>Dynamische siteversnelling via Azure CDN

Met de explosie van sociale media, e-commerce en de hyper-gepersonaliseerde web, wordt een steeds sneller percentage van de inhoud die is geleverd aan eindgebruikers in realtime gegenereerd. Gebruikers verwachten een snel, betrouwbaar en gepersonaliseerde hinder van ondervindt, onafhankelijk van hun browser, locatie, apparaat of netwerk. Echter de zeer innovaties die deze dus ook aantrekkelijke ervaringen langzame pagina downloads en de kwaliteit van de ervaring van consumenten risico. 

Standard content delivery network (CDN) mogelijkheid biedt de mogelijkheid te cachebestanden dichter bij te stellen aan eindgebruikers om de levering van statische bestanden te versnellen. Echter met dynamische webtoepassingen die inhoud in edge-locaties opslaan in cache is niet mogelijk omdat de server de inhoud in reactie op gebruikersgedrag genereert. Versnellen van de levering van deze inhoud is complexer dan traditionele edge opslaan in cache en vereist een end-to-end-oplossing die geoptimaliseerd op elk element in het hele gegevenspad van begin tot levering. Met Azure CDN dynamic site acceleration (DSA) optimalisatie, is de prestaties van webpagina's met dynamische inhoud merkbaar verbeterd.

**Azure CDN van Akamai** en **Azure CDN van Verizon** bieden allebei DSA optimalisatie via de **geoptimaliseerd voor** menu tijdens het maken van het eindpunt.

> [!Important]
> Voor **Azure CDN van Akamai** profielen, u mag de optimalisatie van een CDN-eindpunt wijzigen nadat deze is gemaakt.
>   
> Voor profielen van **Azure CDN van Verizon** geldt dat u de optimalisatie van een CDN-eindpunt niet kunt wijzigen nadat deze is gemaakt.

## <a name="cdn-endpoint-configuration-to-accelerate-delivery-of-dynamic-files"></a>CDN-eindpuntconfiguratie Versnel de levering van de dynamische-bestanden

Voor het configureren van een CDN-eindpunt voor het optimaliseren van de levering van de dynamische-bestanden, kunt u ofwel de Azure-portal, de REST API's of een van de client-SDK's aan hetzelfde doet via een programma gebruiken. 

**Een CDN-eindpunt voor DSA optimalisatie configureren met behulp van de Azure-portal:**

1. In de **CDN-profiel** weergeeft, schakelt **eindpunt**.

   ![Een nieuw CDN-eindpunt toevoegen](./media/cdn-dynamic-site-acceleration/cdn-endpoint-profile.png) 

   Het deelvenster **Een eindpunt toevoegen** wordt weergegeven.

2. Onder **geoptimaliseerd voor**, selecteer **dynamische siteversnelling**.

    ![Maak een nieuw CDN-eindpunt met DSA](./media/cdn-dynamic-site-acceleration/cdn-endpoint-dsa.png)

3. Voor **controlepad**, voer een geldig pad naar een bestand.

    Controlepad is een functie die specifiek zijn voor DSA en een geldig pad is vereist voor het maken. DSA maakt gebruik van een kleine *controlepad* bestand geplaatst op de oorspronkelijke server te optimaliseren van routering netwerkconfiguraties voor het CDN. Voor het bestand van het pad test, kunt u downloaden en de voorbeeld-bestand uploaden naar uw site of een bestaande asset gebruiken in uw bron die ongeveer 10 KB groot is.

4. Voer de overige vereiste endpoint-opties (Zie voor meer informatie, [maken van een nieuw CDN-eindpunt](cdn-create-new-endpoint.md#create-a-new-cdn-endpoint)) en selecteer vervolgens **toevoegen**.

   Nadat het CDN-eindpunt is gemaakt, is deze van toepassing de DSA-optimalisaties voor alle bestanden die voldoen aan bepaalde criteria. 


**Een bestaand eindpunt configureren voor DSA (Azure CDN van Akamai profielen alleen):**

1. In de **CDN-profiel** pagina, selecteert u het eindpunt dat u wilt wijzigen.

2. Selecteer in het linkerdeelvenster **optimalisatie**. 

   De **optimalisatie** pagina wordt weergegeven.

3. Onder **geoptimaliseerd voor**, selecteer **dynamische siteversnelling**en selecteer vervolgens **opslaan**.

> [!Note]
> DSA leidt tot extra kosten. Zie voor meer informatie, [prijzen van Content Delivery Network](https://azure.microsoft.com/pricing/details/cdn/).

## <a name="dsa-optimization-using-azure-cdn"></a>DSA-optimalisatie met Azure CDN

Dynamische Siteversnelling op Azure CDN versnelt de levering van dynamische elementen in met behulp van de volgende technieken:

-   [Optimalisatie van routes](#route-optimization)
-   [Optimalisatie van TCP](#tcp-optimizations)
-   [Vooraf ophalen van objecten (Azure CDN van Akamai alleen)](#object-prefetch-azure-cdn-from-akamai-only)
-   [Adaptieve afbeeldingscompressie (Azure CDN van Akamai alleen)](#adaptive-image-compression-azure-cdn-from-akamai-only)

### <a name="route-optimization"></a>Optimalisatie van routes

Optimalisatie van routes is belangrijk omdat het Internet een dynamische aanwezig is is, waarbij verkeer en tijdelijk storingen voortdurend de netwerktopologie veranderen. Het Border Gateway Protocol (BGP) is het routeringsprotocol van het Internet, maar kunnen er sneller routes via een tussenliggende Point of Presence (PoP)-servers. 

Optimalisatie van routes kiest dat het optimale pad naar de oorsprong zodat een site continu toegankelijk en dynamische inhoud wordt aan eindgebruikers via de snelste en meest betrouwbare route mogelijk wordt geleverd. 

De Akamai-netwerk maakt gebruik van technieken voor het verzamelen van real-time gegevens en verschillende paden in verschillende knooppunten in de Akamai-server, evenals de standaard BGP-route van het open Internet om te bepalen van de snelste route tussen de oorsprong en de rand van CDN met elkaar vergelijken. Deze technieken te voorkomen dat Internet congestie punten en lange routes. 

Op dezelfde manier de Verizon netwerk gebruikt een combinatie van Anycast DNS, hoge capaciteit ondersteunen POP's en statuscontroles, om te bepalen van de beste gateways het beste routeren van gegevens van de client naar de oorsprong.
 
Als gevolg hiervan volledig dynamische en transactionele leveren van inhoud sneller en betrouwbaarder aan eindgebruikers, zelfs wanneer het uncacheable. 

### <a name="tcp-optimizations"></a>Optimalisatie van TCP

Transmission Control Protocol (TCP) is de standaard van het Internet protocol-pakket gebruikt voor het leveren van gegevens tussen toepassingen op een IP-netwerk.  Standaard zijn verschillende en weer schakelen-aanvragen vereist voor het instellen van een TCP-verbinding, evenals de grenzen om te voorkomen dat netwerk congestions, wat leiden tot inefficiënties op schaal. **Azure CDN van Akamai** verwerkt dit probleem door te optimaliseren in drie gebieden: 

 - [Trage start TCP elimineren](#eliminating-tcp-slow-start)
 - [Gebruik te maken van permanente verbindingen](#leveraging-persistent-connections)
 - [Afstemming van TCP-pakket parameters](#tuning-tcp-packet-parameters)

#### <a name="eliminating-tcp-slow-start"></a>Trage start TCP elimineren

TCP *start langzaam* is een algoritme van de TCP-protocol waarmee wordt voorkomen opstoppingen in het netwerk dat door het beperken van de hoeveelheid gegevens die via het netwerk worden verzonden. Het begint al uitschakelen met kleine congestion window-groottes tussen de afzender en de ontvanger totdat het maximum is bereikt of pakketverlies wordt gedetecteerd.

 Beide **Azure CDN van Akamai** en **Azure CDN van Verizon** profielen elimineren TCP trage beginnen met de volgende drie stappen:

1. Status en de bewaking van de bandbreedte wordt gebruikt voor het meten van de bandbreedte van verbindingen tussen edge PoP-servers.
    
2. Metrische gegevens worden gedeeld tussen PoP randservers, zodat elke server op de hoogte van de netwerkomstandigheden en de status van de andere POP's om ze is.  
    
3. Veronderstellingen over sommige overdracht-parameters, zoals de optimale venstergrootte moet bij het communiceren met andere CDN edge-servers in de nabijheid van het CDN edge-servers. Deze stap betekent dat de grootte van het eerste opstoppingen in het venster kan worden verhoogd als de status van de verbinding tussen het CDN edge-servers geschikt voor hogere pakket voor de overdracht van gegevens is.  

#### <a name="leveraging-persistent-connections"></a>Gebruik te maken van permanente verbindingen

Gebruik van een CDN minder unieke computers verbinding maken met uw oorspronkelijke server rechtstreeks wordt vergeleken met gebruikers die verbinding maken rechtstreeks naar uw oorsprong. Azure CDN voor aanvragen van gebruikers tot minder verbinding maken met de oorsprong ook groepen.

Zoals eerder vermeld, zijn verschillende handshake-aanvragen vereist een TCP-verbinding tot stand brengen. Permanente verbindingen die worden geïmplementeerd via de `Keep-Alive` HTTP-header, hergebruik bestaande TCP-verbindingen naar meerdere HTTP-aanvragen op te slaan traject keer sneller leveren. 

**Azure CDN van Verizon** verzendt ook periodieke keepalive pakketten via de TCP-verbinding om te voorkomen dat een open verbinding wordt afgesloten.

#### <a name="tuning-tcp-packet-parameters"></a>Afstemming van TCP-pakket parameters

**Azure CDN van Akamai** verbetert de prestatie de parameters voor server-naar-server-verbindingen en wordt de hoeveelheid lange afstand retouren vereist voor het ophalen van inhoud die is ingesloten in de site met behulp van de volgende technieken:

- Het eerste opstoppingen in het venster verhogen zodat er meer pakketten kunnen worden verzonden zonder te wachten op een bevestiging.
- De time-out voor de eerste retransmit verlagen zodat een verlies wordt gedetecteerd en doorgifte sneller plaatsvindt.
- De time-out voor de minimale en maximale retransmit om te beperken van de wachttijd voordat wordt aangenomen dat pakketten verloren zijn gegaan in overdracht verlagen.

### <a name="object-prefetch-azure-cdn-from-akamai-only"></a>Vooraf ophalen van objecten (Azure CDN van Akamai alleen)

De meeste websites bestaan uit een HTML-pagina, die verwijst naar verschillende andere resources, zoals afbeeldingen en scripts. Normaal gesproken wanneer een client vraagt om een webpagina, de browser downloadt eerst en parseert het HTML-object en vervolgens aanvullende aanvragen maakt voor gekoppelde assets die nodig zijn voor de pagina volledig is geladen. 

*Prefetch* is een techniek om op te halen van afbeeldingen en scripts ingesloten in de HTML-pagina terwijl de HTML-code naar de browser wordt verzonden, en voordat de browser wordt ook deze aanvragen object. 

Met de prefetch-optie is ingeschakeld op het moment wanneer het CDN dient voor de basis HTML-pagina naar de browser van de client, het CDN de HTML-bestand parseert en aanvullende aanvragen voor alle gekoppelde resources maken en opslaan in de cache. Wanneer de client de aanvragen voor de gekoppelde activa, maakt de CDN edge-server al heeft de aangevraagde objecten en kan fungeren ze onmiddellijk zonder een retour naar de oorsprong. Deze optimalisatie voordelen gecachet kan worden en niet gecachet kan worden inhoud.

### <a name="adaptive-image-compression-azure-cdn-from-akamai-only"></a>Adaptieve afbeeldingscompressie (Azure CDN van Akamai alleen)

Sommige apparaten, met name mobiele dashboards, ondervinden trager netwerk van tijd tot tijd. In deze scenario's is het meer nuttig zijn voor de gebruiker voor het ontvangen van de kleinere afbeeldingen sneller in hun webpagina in plaats van een lange wachttijd voor afbeeldingen in volledige resolutie.

Deze functie automatisch kwaliteit netwerk gecontroleerd en JPEG-compressie standaardmethoden in dienst als netwerksnelheden langzamer leveringstijd verbeteren.

Adaptieve afbeeldingscompressie | Bestandsextensies  
--- | ---  
JPEG-compressie | JPG, JPEG, .jpe, .jig, .jgig, .jgi

## <a name="caching"></a>Caching

Met DSA, opslaan in cache is standaard uitgeschakeld op het CDN, zelfs wanneer de oorsprong bevat `Cache-Control` of `Expires` headers in het antwoord. DSA wordt doorgaans gebruikt voor dynamische activa die mag niet worden opgeslagen omdat ze uniek voor elke client zijn. Dit gedrag kan veroorzaken in opslaan in cache.

Als u een website met een combinatie van statische en dynamische elementen hebt, is het beste een hybride benadering om op te halen van de beste prestaties. 

Voor **Azure CDN Standard van Verizon** en **Azure CDN Standard van Akamai** profielen, kunt u op opslaan in cache voor specifieke DSA-eindpunten met behulp van [regels voor caching](cdn-caching-rules.md).

Voor toegang tot de regels voor opslaan in cache:

1. Uit de **CDN-profiel** pagina, onder instellingen, selecteer **regels voor Caching**.  
    
    ![Knop Regels voor CDN-caching](./media/cdn-dynamic-site-acceleration/cdn-caching-rules-btn.png)

    De **regels voor Caching** pagina wordt geopend.

2. Maak een globale of aangepaste opslaan in cache regel in te schakelen in cache opslaan voor uw DSA-eindpunt. 

Voor **Azure CDN Premium van Verizon** profielen alleen inschakelen in cache opslaan voor specifieke DSA-eindpunten met behulp van de [regels-engine](cdn-rules-engine.md). Alleen deze eindpunten van uw profiel die zijn geoptimaliseerd voor DSA van invloed op alle regels die zijn gemaakt. 

Voor toegang tot de regelengine:
    
1. Uit de **CDN-profiel** weergeeft, schakelt **beheren**.  
    
    ![Knop voor CDN-profiel beheren](./media/cdn-dynamic-site-acceleration/cdn-manage-btn.png)

    De CDN-beheerportal wordt geopend.

2. Selecteer in de CDN-beheerportal, **ADN**en selecteer vervolgens **regels-Engine**. 

    ![Regels-engine voor DSA](./media/cdn-dynamic-site-acceleration/cdn-dsa-rules-engine.png)



U kunt ook twee CDN-eindpunten gebruiken: één eindpunt geoptimaliseerd met DSA leveren dynamische activa en een ander eindpunt geoptimaliseerd met een statische optimalisatietype, zoals algemene webweergave levering gecachet kan worden activa. Wijzig de URL van uw webpagina directe koppeling naar de activa op het CDN-eindpunt dat u van plan bent te gebruiken. 

Bijvoorbeeld: `mydynamic.azureedge.net/index.html` is een dynamische pagina en wordt geladen vanuit de DSA-eindpunt.  De html-pagina verwijst naar meerdere statische assets zoals JavaScript-bibliotheken of installatiekopieën die worden geladen vanuit de statische CDN-eindpunt, zoals `mystatic.azureedge.net/banner.jpg` en `mystatic.azureedge.net/scripts.js`. 



