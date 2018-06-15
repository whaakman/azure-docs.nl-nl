---
title: Mediastreaming optimalisatie met Azure CDN
description: Optimaliseren voor de levering van smooth streaming media-bestanden
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
ms.date: 05/01/2018
ms.author: v-deasim
ms.openlocfilehash: 8a2b69aaa601e1d00152f57841a4d67f98680181
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33766169"
---
# <a name="media-streaming-optimization-with-azure-cdn"></a>Mediastreaming optimalisatie met Azure CDN 
 
Gebruik van hoogwaardige video neemt op het internet, wat voor efficiënte levering van grote bestanden moeilijkheden. Klanten verwachten smooth afspelen van video op aanvraag of live video activa op tal van netwerken en clients over de hele wereld. Een snelle en efficiënte bezorgingsmechanisme voor media streaming-bestanden is essentieel voor een consumer smooth en uitmuntende ervaring.  

Live streaming media is uiterst moeilijk te leveren vanwege de grote grootte en het aantal gelijktijdige gebruikers. Lange vertragingen veroorzaken voor gebruikers te verlaten. Omdat live gegevensstromen tevoren kunnen niet worden opgeslagen en grote latenties niet aanvaardbaar is voor gebruikers, moeten video fragmenten tijdig worden geleverd. 

De aanvraag patronen van streaming bieden ook enkele nieuwe uitdagingen. Wanneer een populaire live stream of een nieuwe reeks voor video op aanvraag wordt uitgebracht, kunnen duizenden naar miljoenen viewers verzoeken om de stroom op hetzelfde moment. Smart aanvraag consolidatie is in dit geval de oorsprong-servers niet overbelast wanneer de activa zijn niet opgeslagen in de cache nog essentieel.
 

## <a name="media-streaming-optimizations-for-azure-cdn-from-microsoft"></a>Mediastreaming optimalisaties voor Azure CDN van Microsoft

**Azure CDN Standard van Microsoft** eindpunten streaming media activa leveren via het type Algemeen web levering optimalisatie. 

Optimalisatie voor streaming media **Azure CDN Standard van Microsoft** is van kracht voor live of video-on-demand media die gebruikmaakt van afzonderlijke media fragmenten voor de levering van streaming. Dit proces wijkt af van een enkel grote actief worden overgedragen via progressief downloaden of met behulp van de byte-bereikaanvragen. Zie voor informatie over deze stijl media leveringsmethode, [optimalisatie van grote bestanden downloaden met Azure CDN](cdn-large-file-optimization.md).

De algemene levering of video-on-demand media levering optimalisatie mediatypen voor het gebruik van Azure Content Delivery Network (CDN) met back-end-optimalisaties sneller media-elementen. Ze ook configuraties gebruiken voor media activa op basis van aanbevolen procedures geleerd gedurende een bepaalde periode.

### <a name="partial-cache-sharing"></a>Gedeeltelijke cache delen
Gedeeltelijke cache delen, kunt de CDN deels in cache opgeslagen inhoud naar nieuwe aanvragen bedienen. Bijvoorbeeld, als het eerste verzoek bij de CDN in een cache ontbreekt resulteert, wordt de aanvraag verzonden naar de oorsprong. Hoewel deze onvolledige inhoud in de cache CDN is geladen, kunnen andere aanvragen naar de CDN beginnen met deze gegevens ophalen. 


## <a name="media-streaming-optimizations-for-azure-cdn-from-verizon"></a>Optimalisaties voor Azure CDN van Verizon streaming media

**Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon** eindpunten streaming media activa leveren via het type Algemeen web levering optimalisatie. Enkele onderdelen op de CDN dat rechtstreeks helpt bij het leveren van media-elementen standaard.

### <a name="partial-cache-sharing"></a>Gedeeltelijke cache delen

Gedeeltelijke cache delen, kunt de CDN deels in cache opgeslagen inhoud naar nieuwe aanvragen bedienen. Bijvoorbeeld, als het eerste verzoek bij de CDN in een cache ontbreekt resulteert, wordt de aanvraag verzonden naar de oorsprong. Hoewel deze onvolledige inhoud in de cache CDN is geladen, kunnen andere aanvragen naar de CDN beginnen met deze gegevens ophalen. 

### <a name="cache-fill-wait-time"></a>Cache opvulling wachttijd

 De cachefunctie opvulling wacht tijd zorgt ervoor dat de edge-server voor het opslaan van alle volgende aanvragen voor dezelfde resource pas HTTP-antwoordheaders op de bronserver binnenkomen. Als HTTP-antwoordheaders vanuit de oorsprong plaatsvinden voordat de timer verloopt, worden alle aanvragen die in de wachtstand plaatsen zijn buiten de groeiende cache geleverd. Op hetzelfde moment wordt de cache gevuld door de gegevens van de oorsprong. De wachttijd van cache opvulling is standaard ingesteld op 3000 milliseconden. 

 
## <a name="media-streaming-optimizations-for-azure-cdn-from-akamai"></a>Optimalisaties voor Azure CDN van Akamai streaming media
 
**Azure CDN Standard van Akamai** biedt een functie die zorgt voor streaming media activa efficiënt aan gebruikers overal ter wereld op grote schaal. De functie vermindert latenties omdat dit de belasting van de oorsprong-servers verlaagt. Deze functie is beschikbaar met de standaard Akamai prijscategorie. 

Optimalisatie voor streaming media **Azure CDN Standard van Akamai** is van kracht voor live of video-on-demand media die gebruikmaakt van afzonderlijke media fragmenten voor de levering van streaming. Dit proces wijkt af van een enkel grote actief worden overgedragen via progressief downloaden of met behulp van de byte-bereikaanvragen. Zie voor informatie over deze stijl media leveringsmethode, [groot bestand optimalisatie](cdn-large-file-optimization.md).

De algemene levering of video-on-demand media levering optimalisatie mediatypen voor het gebruik van een CDN met back-end-optimalisaties sneller media-elementen. Ze ook configuraties gebruiken voor media activa op basis van aanbevolen procedures geleerd gedurende een bepaalde periode.

### <a name="configure-an-akamai-cdn-endpoint-to-optimize-media-streaming"></a>Een Akamai CDN-eindpunt voor het optimaliseren van mediastreaming configureren
 
U kunt uw eindpunt content delivery network (CDN) voor het optimaliseren van leveringsmethode voor grote bestanden via de Azure-portal configureren. U kunt ook de REST API's of een van de client-SDK's gebruiken om dit te doen. De volgende stappen ziet u het proces via de Azure-portal voor een **Azure CDN Standard van Akamai** profiel:

1. Een nieuw eindpunt toevoegen aan een Akamai **CDN-profiel** pagina **eindpunt**.
  
    ![Nieuw eindpunt](./media/cdn-media-streaming-optimization/cdn-new-akamai-endpoint.png)

2. In de **geoptimaliseerd voor** vervolgkeuzelijst, selecteer **Video op aanvraag mediastreaming** voor video-on-demand activa. Als u een combinatie van live en video-on-demand streaming doen, selecteert u **algemene mediastreaming**.

    ![Streaming geselecteerd](./media/cdn-media-streaming-optimization/02_Creating.png) 
 
Nadat u het eindpunt hebt gemaakt, wordt het toegepast de optimalisatie voor alle bestanden die aan bepaalde criteria voldoen. De volgende sectie wordt beschreven in dit proces. 

### <a name="caching"></a>Caching

Als **Azure CDN Standard van Akamai** detecteert dat de asset is een streaming-manifest of fragment, op verschillende tijdstippen in de cache verloopt uit algemene webtoepassingen levering wordt gebruikt. (Zie de volledige lijst in de volgende tabel.) Zoals altijd worden cache-control of Expires-koppen verzonden vanuit de oorsprong gehonoreerd. Als de asset niet een activum media, slaat het met behulp van de vervaldatum tijden voor de levering van algemene webtoepassingen.

De korte tijd van de negatieve cache is nuttig voor de oorsprong offload wanneer veel gebruikers vragen een fragment dat nog niet bestaat. Een voorbeeld is een live stream waar de pakketten zijn niet beschikbaar vanuit de oorsprong die seconde. Het meer cachebewerkingen interval helpt ook bij het aanvragen van de oorsprong offload omdat video-inhoud doorgaans niet is gewijzigd.
 

|   | Algemene webweergave | Algemene mediastreaming | Video-on-demand mediastreaming  
--- | --- | --- | ---
Opslaan in cache: positief <br> HTTP 200, 203, 300, <br> 301, 302 en 410 | 7 dagen |365 dagen | 365 dagen   
Opslaan in cache: negatieve <br> HTTP 204, 305, 404, <br> en 405 | Geen | 1 seconde | 1 seconde
 
### <a name="deal-with-origin-failure"></a>Omgaan met fouten in de oorsprong  

Algemene media leveren en video-on-demand media leveren ook hebben oorsprong-outs en een nieuwe poging logboek op basis van best practices voor typische aanvraag patronen. Bijvoorbeeld, omdat algemene media leveren voor live en video-on-demand media leveren, een kortere time-out van de verbinding vanwege de aard van tijdgebonden van wordt live streamen.

Wanneer een verbinding een optreedt time-out, probeert de CDN opnieuw een aantal keren voordat het een fout '504 - Gateway timeout gegenereerd' naar de client verzendt. 

Wanneer een bestand overeenkomt met de lijst met bestanden en de tekengrootte voorwaarden, de CDN maakt gebruik van het gedrag voor het streamen van media. Anders gebruikt het algemene webtoepassingen levering.
   
### <a name="conditions-for-media-streaming-optimization"></a>Voorwaarden voor de optimalisatie van mediastreaming 

De volgende tabel bevat de set criteria te zijn voor de optimalisatie van mediastreaming voldaan: 
 
Ondersteunde typen streaming | Bestandsextensies  
--- | ---  
Apple HLS | m3u8, m3u, m3ub, key, ts, aac
Adobe HDS | f4m, f4x, drmmeta, bootstrap, f4f,<br>URL totaal aantal segmenten-structuur <br> (die overeenkomen met de reguliere expressie: ^(/.*)Seq(\d+)-Frag(\d+)
STREEPJE | MPD, streepjes, divx, ismv, m4s, m4v, mp4, mp4v, <br> sidx, webm, mp4a, m4a, isma
Smooth streaming | / manifest//QualityLevels/fragmenten /
  
 
