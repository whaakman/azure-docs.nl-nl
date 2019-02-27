---
title: Streaming-eindpunten in Azure mediaservices | Microsoft Docs
description: Dit artikel bevat een uitleg over wat Streaming-eindpunten zijn en hoe ze worden gebruikt door Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/25/2019
ms.author: juliako
ms.openlocfilehash: d5ca9e602416e6e575be8b79942cd6dba2a2fd69
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56889151"
---
# <a name="streaming-endpoints"></a>Streaming-eindpunten

In Microsoft Azure Media Services (AMS), de [Streaming-eindpunten](https://docs.microsoft.com/rest/api/media/streamingendpoints) entiteit vertegenwoordigt een streamingservice waarmee u kunt inhoud rechtstreeks aan een client-afspeeltoepassing leveren, of aan een Content Delivery Network (CDN) voor meer distributie. De uitgaande stroom van een **Streaming-eindpunt** service mag bestaan uit een live stream of een video op aanvraag actief in Media Services-account. Wanneer u een Media Services-account, maakt een **standaard** Streaming-eindpunt is voor u gemaakt in een status ' gestopt '. U kunt niet verwijderen de **standaard** Streaming-eindpunt. Aanvullende Streaming-eindpunten kunnen worden gemaakt onder het account. 

> [!NOTE]
> Voor het starten van video's streamen, moet u start de **Streaming-eindpunt** vanwaaruit u wilt de video te streamen. 

## <a name="naming-convention"></a>Naamgevingsregels

Voor het standaardeindpunt: `{AccountName}-{DatacenterAbbreviation}.streaming.media.azure.net`

Voor elke extra eindpunten: `{EndpointName}-{AccountName}-{DatacenterAbbreviation}.streaming.media.azure.net`

## <a name="types"></a>Typen  

Er zijn twee **Streaming-eindpunt** typen: **Standard** en **Premium**. Het type is gedefinieerd door het aantal schaaleenheden (`scaleUnits`) u toewijzen voor het streaming-eindpunt. 

De tabel staan de typen:  

|Type|Schaaleenheden|Description|
|--------|--------|--------|  
|**Standard Streaming-eindpunt** (aanbevolen)|0|De **Standard** type is de aanbevolen optie voor vrijwel alle streaming scenario's en doelgroepen van elke grootte. De **Standard** type automatisch wordt geschaald uitgaande bandbreedte. <br/>Voor klanten met een bijzonder veeleisende behoeften Media Services biedt **Premium** streaming-eindpunten, die kunnen worden gebruikt voor het opschalen van capaciteit voor de grootste internet doelgroepen. Als u verwacht grote doelgroepen en gelijktijdige viewers dat, contact met ons op amsstreaming@microsoft.com voor informatie over de noodzaak om te verplaatsen naar de **Premium** type. |
|**Premium Streaming Endpoint**|>0|**Premium**-streaming-eindpunten zijn geschikt voor geavanceerde workloads omdat er gebruik wordt gemaakt van toegewezen, schaalbare bandbreedtecapaciteit. U verplaatst naar een **Premium** type door aan te passen `scaleUnits`. `scaleUnits` bieden u speciale uitgangscapaciteit die kan worden aangeschaft per 200 Mbps. Wanneer u de **Premium** type, elke ingeschakelde unit biedt extra bandbreedtecapaciteit voor de toepassing. |

## <a name="working-with-cdn"></a>Werken met CDN

In de meeste gevallen hebt u CDN is ingeschakeld. Echter, als u maximale gelijktijdigheid lager is dan 500 viewers zijn planningscyclus vervolgens het verdient aanbeveling CDN uitschakelen omdat CDN aanbevolen met gelijktijdigheid schaalt.

> [!NOTE]
> Het Streaming-eindpunt `hostname` en de streaming-URL blijft hetzelfde, ongeacht of u CDN inschakelt of niet.

### <a name="detailed-explanation-of-how-caching-works"></a>Gedetailleerde uitleg van hoe caching werkt

Er is geen specifieke bandbreedte-waarde wanneer is afhankelijk van het CDN toevoegen omdat de hoeveelheid bandbreedte die nodig is voor een CDN streaming-eindpunt is ingeschakeld. Veel is afhankelijk van het type inhoud, hoe populair is, bitsnelheden en protocollen. Het CDN is alleen caching wat wordt aangevraagd. Dit betekent dat dat populaire inhoud rechtstreeks vanuit het CDN-moet worden verzonden, zolang de video fragment in de cache is opgeslagen. Live-inhoud is waarschijnlijk in de cache worden opgeslagen omdat u meestal veel mensen bekijken precies hetzelfde. On-demand inhoud mag iets moeilijker omdat u bepaalde inhoud die is populaire en sommige die niet kan hebben. Hebt u miljoenen videomedia waar geen van beide populaire (alleen 1 of 2 viewers per week) zijn, maar u hebt duizenden mensen alle andere video's bekijkt, wordt het CDN veel minder effectief. Met deze cache missers in, verhoogt u de belasting op het streaming-eindpunt.
 
U moet ook rekening houden met hoe adaptieve streaming werkt. Elke afzonderlijke video fragment in de cache geplaatst omdat het eigen entiteit. Bijvoorbeeld, als de eerste keer dat een bepaalde video is bekeken, de persoon wordt overgeslagen rond het bekijken van slechts enkele seconden alleen hier en daar de video fragmenten die zijn gekoppeld aan wat de persoon bekeken ophalen in de cache opgeslagen in het CDN. Met adaptieve streaming hebt u doorgaans 5 tot en met 7 verschillende bitsnelheden van video. Als één persoon één bitsnelheid kijkt wordt en een andere persoon kijkt naar een andere bitrate, ze worden alle opgeslagen in het cachegeheugen afzonderlijk in het CDN. Zelfs als de twee personen volgt dezelfde bitsnelheid kunnen ze streaming via verschillende protocollen. Elk protocol (HLS, MPEG-DASH, Smooth Streaming) wordt afzonderlijk in cache. Dus elke bitrate en protocol afzonderlijk worden opgeslagen en alleen deze video fragmenten die zijn aangevraagd in de cache zijn opgeslagen.
 
## <a name="properties"></a>Properties 

In deze sectie geeft informatie over een aantal van de eigenschappen van het Streaming-eindpunt. Zie voor meer voorbeelden van het maken van een nieuwe streaming-eindpunt en een beschrijving van alle eigenschappen [Streaming-eindpunt](https://docs.microsoft.com/rest/api/media/streamingendpoints/create). 

- `accessControl` -Gebruikt voor het configureren van de volgende instellingen voor dit streaming-eindpunt: Verificatiesleutels van Akamai handtekening-header en IP-adressen die zijn toegestaan verbinding maken met dit eindpunt.<br />Deze eigenschap kan worden ingesteld wanneer `cdnEnabled` is ingesteld op false.
- `cdnEnabled` -Geeft aan of de Azure CDN-integratie voor dit streaming-eindpunt is ingeschakeld (standaard uitgeschakeld). Als u instelt `cdnEnabled` op true, de volgende configuraties uitgeschakeld: `customHostNames` en `accessControl`.
  
    Niet alle datacenters ondersteuning voor de Azure CDN-integratie. Als u wilt controleren of uw datacenter de Azure CDN-integratie beschikbaar heeft, het volgende doen:
 
   - Probeert in te stellen de `cdnEnabled` op ' True '.
   - Controleer de geretourneerde resultaten voor een `HTTP Error Code 412` (PreconditionFailed) met het bericht "Streaming-eindpunt CdnEnabled eigenschap kan niet worden ingesteld op true als het CDN-functionaliteit is niet beschikbaar in de huidige regio." 

    Als u deze fout optreedt, kan het datacenter deze biedt geen ondersteuning. Probeer een ander datacenter.
- `cdnProfile` -Wanneer `cdnEnabled` is ingesteld op true, u kunt ook doorgeven `cdnProfile` waarden. `cdnProfile` de naam van het CDN-profiel is waar het CDN-eindpunt dat wordt gemaakt. U kunt een bestaande cdnProfile of gebruik een nieuwe. Als de waarde NULL is en `cdnEnabled` waar is, is de standaardwaarde 'AzureMediaStreamingPlatformCdnProfile' wordt gebruikt. Als de opgegeven `cdnProfile` al bestaat, een eindpunt wordt gemaakt onder het. Als het profiel niet bestaat, wordt een nieuw profiel automatisch gemaakt.
- `cdnProvider` -Wanneer CDN is geactiveerd, kunt u ook doorgeven `cdnProvider` waarden. `cdnProvider` Hiermee bepaalt u welke provider worden gebruikt. Op dit moment worden drie waarden ondersteund: "StandardVerizon", "PremiumVerizon" and "StandardAkamai". Als er geen waarde is opgegeven en `cdnEnabled` is ingesteld op true, 'StandardVerizon' wordt gebruikt (dat wil zeggen de standaardwaarde).
- `crossSiteAccessPolicies` -Hiermee geeft cross-site-beleidsregels voor verschillende clients. Zie voor meer informatie, [interdomein-bestand beleidsspecificatie](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html) en [maken van een Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955\(v=vs.95\).aspx).
- `customHostNames` -Gebruikt voor het configureren van een Streaming-eindpunt voor het accepteren van verkeer doorgestuurd naar een aangepaste hostnaam.  Deze eigenschap is ongeldig voor Standard en Premium-Streaming-eindpunten en kunnen worden ingesteld wanneer `cdnEnabled`: false.
    
    Het eigendom van de domeinnaam moet worden bevestigd door Media Services. Media Services controleert het eigendom van de naam van domein of doordat een `CName` record met de ID van de Media Services-account als een onderdeel dat moet worden toegevoegd aan het domein wordt gebruikt. Een voorbeeld: voor 'sports.contoso.com' moet worden gebruikt als een aangepaste hostnaam voor het streaming-eindpunt, een record voor `<accountId>.contoso.com` moet worden geconfigureerd om te verwijzen naar een van de hostnamen voor Media Services-verificatie. De naam van de verificatie van verifydns bestaat. \<mediaservices-dns-zone >. 

    Hieronder vindt u de verwachte DNS-zones moet worden gebruikt in de record controleren voor verschillende Azure-regio's.
  
    - Noord-Amerika, Europa, Singapore, Hongkong SAR, Japan:
      
      - `media.azure.net`
      - `verifydns.media.azure.net`
      
    - China:
        
      - `mediaservices.chinacloudapi.cn`
      - `verifydns.mediaservices.chinacloudapi.cn`
        
    Bijvoorbeeld, een `CName` record die wordt toegewezen '945a4c4e-28ea-45-cd-8ccb-a519f6b700ad.contoso.com' naar 'verifydns.media.azure.net' blijkt dat de Media Services-ID 945a4c4e-28ea-45cd-8ccb-a519f6b700ad het eigendom van het domein contoso.com dus heeft inschakelen van een willekeurige naam onder contoso.com moet worden gebruikt als een aangepaste hostnaam voor een streaming-eindpunt onder dat account. Als u de Media Service-ID-waarde zoekt, gaat u naar de [Azure-portal](https://portal.azure.com/) en selecteer uw Media Service-account. De **Account-ID** wordt weergegeven in de rechterbovenhoek van de pagina.
        
    Als er een poging tot het instellen van een aangepaste hostnaam zonder een juiste verificatie van de `CName` record, het DNS-antwoord mislukken en worden opgeslagen in het cachegeheugen voor enige tijd. Zodra een juiste record ingesteld is, kan het even duren voordat totdat het antwoord in de cache opnieuw wordt gevalideerd. Afhankelijk van de DNS-provider voor het aangepaste domein, kan het een paar minuten tot een uur tot duren valideren van de record.
        
     Naast de `CName` die wordt toegewezen `<accountId>.<parent domain>` naar `verifydns.<mediaservices-dns-zone>`, moet u een andere `CName` die de aangepaste hostnaam wordt toegewezen (bijvoorbeeld `sports.contoso.com`) naar uw Media Services Streaming-eindpunt van hostnaam (bijvoorbeeld `amstest-usea.streaming.media.azure.net`).
 
    > [!NOTE]
    > Streaming-eindpunten die zich in hetzelfde Datacenter delen niet dezelfde aangepaste hostnaam.

    Op dit moment biedt geen Media Services ondersteuning voor SSL met aangepaste domeinen. 
    
- `maxCacheAge` -Onderdrukkingen de standaard max-age HTTP-cache beheren-header ingesteld door het streaming-eindpunt op media fragmenten en op aanvraag manifesten. De waarde is ingesteld in een paar seconden.
- `resourceState` -

    - Gestopt - de initiële status van een Streaming-eindpunt na het maken
    - Begin - is overstappen naar de actieve status
    - Actief - kan inhoud te streamen naar clients
    - Schalen - de schaal eenheden worden verhoogd of verlaagd
    - Stoppen - is overstappen naar de status gestopt
    - Verwijderen - wordt verwijderd
    
- `scaleUnits ` -U voorzien van speciale uitgangscapaciteit die kan worden aangeschaft per 200 Mbps. Als u verplaatsen wilt naar een **Premium** typt, aanpassen `scaleUnits`.

## <a name="next-steps"></a>Volgende stappen

Het voorbeeld [in deze opslagplaats](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) laat zien hoe u start het standaardstreaming-eindpunt met .NET.

