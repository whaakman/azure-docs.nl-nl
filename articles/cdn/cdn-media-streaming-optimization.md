---
title: Mediastreaming optimalisatie met Azure CDN
description: Streaming van mediabestanden voor een naadloze levering optimaliseren
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
ms.date: 05/01/2018
ms.author: magattus
ms.openlocfilehash: 9802296170f07bb8599058e230798f647e900d4d
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093694"
---
# <a name="media-streaming-optimization-with-azure-cdn"></a>Mediastreaming optimalisatie met Azure CDN 
 
Gebruik van high-definition-video is op het internet, die zorgt voor problemen voor het efficiënt leveren van grote bestanden te vergroten. Klanten verwachten van smooth afspelen van video op aanvraag of live video assets op een groot aantal netwerken en clients over de hele wereld. Een snelle en efficiënte leveringsmechanisme voor bestanden van mediastreaming is van essentieel belang om te controleren of de ervaring van een soepele en leuker consumenten.  

Live streaming media is uiterst moeilijk zijn om te leveren vanwege de grote omvang en het aantal gelijktijdige gebruikers. Lange vertragingen veroorzaken gebruikers te verlaten. Omdat live streams in de cache vooraf kunnen niet worden opgeslagen en grote latentie aanvaardbaar is voor viewers niet, moeten video fragmenten tijdig worden geleverd. 

De aanvraag-patronen van streaming bieden ook enkele nieuwe uitdagingen. Wanneer een populaire live stream of een nieuwe reeks is vrijgegeven voor video op aanvraag, kunnen de stroom u duizenden naar miljoenen kijkers vragen op hetzelfde moment. Consolidatie van slimme aanvraag is in dit geval het essentieel de bronservers niet blijvend wordt overbelast wanneer de activa zijn niet opgeslagen in de cache nog.
 

## <a name="media-streaming-optimizations-for-azure-cdn-from-microsoft"></a>Mediastreaming optimalisaties voor Azure CDN van Microsoft

**Azure CDN Standard van Microsoft** eindpunten leveren streaming media-activa rechtstreeks met behulp van het type Algemeen web levering optimalisatie. 

Optimalisatie van mediastreaming **Azure CDN Standard van Microsoft** wordt met ingang van live en video on demand streaming media die gebruikmaakt van afzonderlijke media fragmenten voor de levering van. Dit proces wijkt af van een enkele grote asset overgedragen via progressief downloaden of met behulp van de byte-bereikaanvragen. Zie voor informatie over die stijl van de medialevering van, [optimalisatie van grote bestanden downloaden met Azure CDN](cdn-large-file-optimization.md).

De algemene levering of video on demand media leveren optimalisatie mediatypen voor het gebruik van Azure Content Delivery Network (CDN) met back-end optimalisaties sneller leveren van media-activa. Ze ook configuraties gebruiken voor media-assets op basis van best practices voor geleerd na verloop van tijd.

### <a name="partial-cache-sharing"></a>Gedeeltelijke cache delen
Gedeeltelijke cache delen, kunt de CDN voor het leveren van gedeeltelijk in de cache inhoud naar nieuwe aanvragen. Bijvoorbeeld, als de eerste aanvraag naar de CDN in een cache ontbreekt resulteert, wordt de aanvraag verzonden naar de oorsprong. Hoewel deze onvolledige inhoud in de CDN-cache wordt geladen, kunnen andere aanvragen voor het CDN beginnen met ophalen van deze gegevens. 


## <a name="media-streaming-optimizations-for-azure-cdn-from-verizon"></a>Mediastreaming optimalisaties voor Azure CDN van Verizon

**Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon** eindpunten leveren streaming media-activa rechtstreeks met behulp van het type Algemeen web levering optimalisatie. Enkele functies op het CDN wordt rechtstreeks helpen bij het leveren van media-activa standaard.

### <a name="partial-cache-sharing"></a>Gedeeltelijke cache delen

Gedeeltelijke cache delen, kunt de CDN voor het leveren van gedeeltelijk in de cache inhoud naar nieuwe aanvragen. Bijvoorbeeld, als de eerste aanvraag naar de CDN in een cache ontbreekt resulteert, wordt de aanvraag verzonden naar de oorsprong. Hoewel deze onvolledige inhoud in de CDN-cache wordt geladen, kunnen andere aanvragen voor het CDN beginnen met ophalen van deze gegevens. 

### <a name="cache-fill-wait-time"></a>Wachttijd voor cache opvulling

 De functie cache opvulling voor wait zorgt ervoor dat de edge-server voor het opslaan van alle volgende aanvragen voor dezelfde resource totdat het HTTP-antwoordheaders binnenkomen bij de oorspronkelijke server. Als HTTP-antwoordheaders bij de oorsprong plaatsvinden voordat de timer verloopt, worden alle aanvragen die in de wachtstand plaatsen zijn geleverd vanuit de groeiende cache. Op hetzelfde moment, worden de cache wordt gevuld door gegevens uit de oorsprong. De wachttijd voor opvulling van cache is standaard ingesteld op 3000 milliseconden. 

 
## <a name="media-streaming-optimizations-for-azure-cdn-from-akamai"></a>Optimalisaties voor Azure CDN van Akamai streaming van Media
 
**Azure CDN Standard van Akamai** biedt een functie die voorziet in streaming media-activa efficiënt aan gebruikers over de hele wereld op schaal. De functie vermindert latenties omdat de belasting van de bronservers vermindert. Deze functie is beschikbaar met de standard-Akamai prijscategorie. 

Optimalisatie van mediastreaming **Azure CDN Standard van Akamai** wordt met ingang van live en video on demand streaming media die gebruikmaakt van afzonderlijke media fragmenten voor de levering van. Dit proces wijkt af van een enkele grote asset overgedragen via progressief downloaden of met behulp van de byte-bereikaanvragen. Zie voor informatie over die stijl van de medialevering van, [optimalisatie van grote bestanden](cdn-large-file-optimization.md).

De algemene levering of video on demand media leveren optimalisatie mediatypen voor het gebruik van een CDN met back-end optimalisaties sneller leveren van media-activa. Ze ook configuraties gebruiken voor media-assets op basis van best practices voor geleerd na verloop van tijd.

### <a name="configure-an-akamai-cdn-endpoint-to-optimize-media-streaming"></a>Configureren van een Akamai CDN-eindpunt voor het optimaliseren van mediastreaming
 
U kunt uw content delivery network (CDN)-eindpunt voor het optimaliseren van levering voor grote bestanden via Azure portal kunt configureren. U kunt ook de REST API's of een van de client-SDK's gebruiken om dit te doen. De volgende stappen ziet u het proces via de Azure-portal voor een **Azure CDN Standard van Akamai** profiel:

1. Een nieuw eindpunt toevoegen aan een Akamai **CDN-profiel** weergeeft, schakelt **eindpunt**.
  
    ![Nieuw eindpunt](./media/cdn-media-streaming-optimization/cdn-new-akamai-endpoint.png)

2. In de **geoptimaliseerd voor** vervolgkeuzelijst, selecteer **Video on-demandstreaming van media** voor video-on-demand activa. Als u een combinatie van live en video on demand streaming doen, selecteert u **algemene mediastreaming**.

    ![Streaming geselecteerd](./media/cdn-media-streaming-optimization/02_Creating.png) 
 
Nadat u het eindpunt hebt gemaakt, wordt de optimalisatie voor alle bestanden die voldoen aan bepaalde criteria is van toepassing. De volgende sectie wordt beschreven in dit proces. 

### <a name="caching"></a>Caching

Als **Azure CDN Standard van Akamai** detecteert dat de asset is een streamingmanifest of een fragment, op verschillende tijdstippen in de cache verlopen van algemene webweergave wordt gebruikt. (Zie de volledige lijst in de volgende tabel.) Zoals altijd het cache-control of Expires-koppen die zijn verzonden vanaf de oorsprong worden herkend. Als de asset niet gelijk is aan een media-activa, slaat deze met behulp van de tijden van de vervaldatum voor algemene webweergave.

De korte tijd voor negatieve opslaan in cache is nuttig om de oorsprong offload wanneer veel gebruikers vragen een fragment dat nog niet bestaat. Een voorbeeld is een live stream waar de pakketten zijn niet beschikbaar bij de oorsprong die tweede. Het interval voor meer opslaan in cache helpt ook bij het aanvragen van de oorsprong offload omdat video-inhoud doorgaans wordt niet gewijzigd.
 

|   | Algemene webweergave | Algemene mediastreaming | Video on demand streaming van media  
--- | --- | --- | ---
Opslaan in cache: positief <br> HTTP 200, 203, 300, <br> 301, 302 en 410 | 7 dagen |365 dagen | 365 dagen   
Opslaan in cache: negatief zijn <br> HTTP 204, 305, 404, <br> en 405 | Geen | 1 seconde | 1 seconde
 
### <a name="deal-with-origin-failure"></a>Fout bij de oorsprong zijn getroffen  

Algemene medialevering en video on demand media leveren ook hebben oorsprong time-outs en een nieuwe poging logboek op basis van best practices voor typische aanvraag patronen. Bijvoorbeeld, omdat algemene medialevering voor live is en video on demand media leveren, maakt gebruik van een kortere time-out van de verbinding vanwege de aard tijdgebonden van live streamen.

Wanneer een verbinding een optreedt time-out, probeert een aantal keer voordat er een fout '504 - time-out voor de Gateway' naar de client verzendt opnieuw door het CDN. 

Wanneer een bestand overeenkomt met de lijst in het type en grootte-voorwaarden, wordt het gedrag in het CDN gebruikt voor het streamen van media. Anders wordt de algemene webweergave.
   
### <a name="conditions-for-media-streaming-optimization"></a>Voorwaarden voor de optimalisatie van mediastreaming 

De volgende tabel bevat de set criteria worden voldaan voordat voor optimalisatie van mediastreaming: 
 
Ondersteunde typen voor streaming | Bestandsextensies  
--- | ---  
Apple HLS | m3u8, m3u, m3ub, sleutel, ts, aac
Adobe HDS | f4m, f4x, drmmeta, bootstrap, f4f,<br>Segmenten-totaal aantal URL-structuur <br> (die overeenkomt met de reguliere expressie: ^(/.*)Seq(\d+)-Frag(\d+)
STREEPJE | MPD, streepje, divx, ismv, m4s, m4v, mp4, mp4v, <br> sidx, webm, mp4a, m4a, isma
Smooth streaming | / manifest//QualityLevels /-fragmenten /
  
 
