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
ms.date: 10/22/2018
ms.author: juliako
ms.openlocfilehash: 224f60d5b06f87e7b619e6c56b161ec700c657b9
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49986601"
---
# <a name="streaming-endpoints"></a>Streaming-eindpunten

In Microsoft Azure Media Services (AMS), de [Streaming-eindpunten](https://docs.microsoft.com/rest/api/media/streamingendpoints) entiteit vertegenwoordigt een streamingservice waarmee u kunt inhoud rechtstreeks aan een client-afspeeltoepassing leveren, of aan een Content Delivery Network (CDN) voor meer distributie. De uitgaande stroom van de service van een Streaming-eindpunt mag bestaan uit een live stream of een video op aanvraag actief in Media Services-account. Wanneer u een Media Services-account, maakt een **standaard** Streaming-eindpunt is voor u gemaakt in een status ' gestopt '. U kunt het standaard Streaming-eindpunt niet verwijderen. Aanvullende Streaming-eindpunten kunnen worden gemaakt onder het account. Voor video's wilt streamen, moet u de Streaming-eindpunt starten. 

## <a name="streamingendpoint-types"></a>Streamingendpoint zo typen  

Er zijn twee **streamingendpoint zo** typen: **Standard** en **Premium**. Het type is gedefinieerd door het aantal schaaleenheden (`scaleUnits`) u toewijzen voor het streaming-eindpunt. 

De tabel staan de typen:  

|Type|Schaaleenheden|Beschrijving|
|--------|--------|--------|  
|**Standard Streaming-eindpunt** (aanbevolen)|0|De **Standard** type is de aanbevolen optie voor vrijwel alle streaming scenario's en doelgroepen van elke grootte. De **Standard** type automatisch wordt geschaald uitgaande bandbreedte. <br/>Voor klanten met een bijzonder veeleisende behoeften Media Services biedt **Premium** streaming-eindpunten, die kunnen worden gebruikt voor het opschalen van capaciteit voor de grootste internet doelgroepen. Als u verwacht grote doelgroepen en gelijktijdige viewers dat, contact met ons op amsstreaming@microsoft.com voor informatie over de noodzaak om te verplaatsen naar de **Premium** type. |
|**Premium-Streaming-eindpunt**|>0|**Premium**-streaming-eindpunten zijn geschikt voor geavanceerde workloads omdat er gebruik wordt gemaakt van toegewezen, schaalbare bandbreedtecapaciteit. U verplaatst naar een **Premium** type door aan te passen `scaleUnits`. `scaleUnits` bieden u speciale uitgangscapaciteit die kan worden aangeschaft per 200 Mbps. Wanneer u de **Premium** type, elke ingeschakelde unit biedt extra bandbreedtecapaciteit voor de toepassing. |

## <a name="working-with-cdn"></a>Werken met CDN

In de meeste gevallen hebt u CDN is ingeschakeld. Echter, als u maximale gelijktijdigheid lager is dan 500 viewers zijn planningscyclus vervolgens het verdient aanbeveling CDN uitschakelen omdat CDN aanbevolen met gelijktijdigheid schaalt.

### <a name="detailed-explanation-of-how-caching-works"></a>Gedetailleerde uitleg van hoe caching werkt

Er is geen specifieke bandbreedte-waarde wanneer is afhankelijk van het CDN toevoegen omdat de hoeveelheid bandbreedte die nodig is voor een CDN streaming-eindpunt is ingeschakeld. Veel is afhankelijk van het type inhoud, hoe populair is, bitsnelheden en protocollen. Het CDN is alleen caching wat wordt aangevraagd. Dit betekent dat dat populaire inhoud rechtstreeks vanuit het CDN-moet worden verzonden, zolang de video fragment in de cache is opgeslagen. Live-inhoud is waarschijnlijk in de cache worden opgeslagen omdat u meestal veel mensen bekijken precies hetzelfde. On-demand inhoud mag iets moeilijker omdat u bepaalde inhoud die is populaire en sommige die niet kan hebben. Hebt u miljoenen videomedia waar geen van beide populaire (alleen 1 of 2 viewers per week) zijn, maar u hebt duizenden mensen alle andere video's bekijkt, wordt het CDN veel minder effectief. Met deze cache missers in, verhoogt u de belasting op het streaming-eindpunt.
 
U moet ook rekening houden met hoe adaptieve streaming werkt. Elke afzonderlijke video fragment in de cache geplaatst omdat het eigen entiteit. Bijvoorbeeld, als de eerste keer dat een bepaalde video is bekeken, de persoon wordt overgeslagen rond het bekijken van slechts enkele seconden alleen hier en daar de video fragmenten die zijn gekoppeld aan wat de persoon bekeken ophalen in de cache opgeslagen in het CDN. Met adaptieve streaming hebt u doorgaans 5 tot en met 7 verschillende bitsnelheden van video. Als één persoon één bitsnelheid kijkt wordt en een andere persoon kijkt naar een andere bitrate, ze worden alle opgeslagen in het cachegeheugen afzonderlijk in het CDN. Zelfs als de twee personen volgt dezelfde bitsnelheid kunnen ze streaming via verschillende protocollen. Elk protocol (HLS, MPEG-DASH, Smooth Streaming) wordt afzonderlijk in cache. Dus elke bitrate en protocol afzonderlijk worden opgeslagen en alleen deze video fragmenten die zijn aangevraagd in de cache zijn opgeslagen.
 
## <a name="streamingendpoint-properties"></a>Streamingendpoint zo eigenschappen 

In deze sectie geeft informatie over een aantal van de eigenschappen van de streamingendpoint zo. Zie voor meer voorbeelden van het maken van een nieuwe streaming-eindpunt en een beschrijving van alle eigenschappen [Streaming-eindpunt](https://docs.microsoft.com/rest/api/media/streamingendpoints/create). 

|Eigenschap|Beschrijving|  
|--------------|----------|
|`accessControl`|Gebruikt voor het configureren van de volgende instellingen voor dit streaming-eindpunt: Akamai handtekening header verificatiesleutels en IP-adressen die zijn toegestaan verbinding maken met dit eindpunt.<br />Deze eigenschap kan worden ingesteld wanneer `cdnEnabled`' ' is ingesteld op false.|  
|`cdnEnabled`|Geeft aan of de Azure CDN-integratie voor dit streaming-eindpunt is ingeschakeld (standaard uitgeschakeld.)<br /><br /> Als u instelt `cdnEnabled` op true, de volgende configuraties uitgeschakeld: `customHostNames` en `accessControl`.<br /><br />Niet alle datacenters ondersteuning voor de Azure CDN-integratie. Als u wilt controleren of uw datacenter de Azure CDN heeft integratie beschikbaar het volgende doen:<br /><br /> -Probeert in te stellen de `cdnEnabled` op ' True '.<br /><br /> -Controleer de geretourneerde resultaten voor een `HTTP Error Code 412` (PreconditionFailed) met het bericht "Streaming-eindpunt CdnEnabled eigenschap kan niet worden ingesteld op true als het CDN-functionaliteit is niet beschikbaar in de huidige regio."<br /><br /> Als u deze fout optreedt, kan het datacenter deze biedt geen ondersteuning. Probeer een ander datacenter.|  
|`cdnProfile`|Wanneer `cdnEnabled` is ingesteld op true, u kunt ook doorgeven `cdnProfile` waarden. `cdnProfile` de naam van het CDN-profiel is waar het CDN-eindpunt dat wordt gemaakt. U kunt een bestaande cdnProfile of gebruik een nieuwe. Als de waarde NULL is en `cdnEnabled` waar is, is de standaardwaarde 'AzureMediaStreamingPlatformCdnProfile' wordt gebruikt. Als de opgegeven `cdnProfile` al bestaat, een eindpunt wordt gemaakt onder het. Als het profiel niet aanwezig is, wordt een nieuw profiel automatisch gemaakt.|
|`cdnProvider`|Wanneer CDN is ingeschakeld, kunt u ook doorgeven `cdnProvider` waarden. `cdnProvider` Hiermee bepaalt u welke provider worden gebruikt. Op dit moment drie waarden worden ondersteund: "StandardVerizon", "PremiumVerizon" en 'StandardAkamai'. Als er geen waarde is opgegeven en `cdnEnabled` is ingesteld op true, 'StandardVerizon' wordt gebruikt (dat wil zeggen de standaardwaarde.)|
|`crossSiteAccessPolicies`|Hiermee geeft cross-site-beleidsregels voor verschillende clients. Zie voor meer informatie, [interdomein-bestand beleidsspecificatie](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html) en [maken van een Service Available Across Domain Boundaries](http://msdn.microsoft.com/library/cc197955\(v=vs.95\).aspx).|  
|`customHostNames`|Gebruikt voor het configureren van een streaming-eindpunt voor het accepteren van verkeer doorgestuurd naar een aangepaste hostnaam. Hierdoor kunnen voor eenvoudiger beheer van configuratie van het verkeer via een algemene Traffic Manager (GTM) en voor merknaam domeinnamen moet worden gebruikt als de streaming-eindpuntnaam.<br /><br /> Het eigendom van de domeinnaam moet worden bevestigd door Azure Media Services. Azure Media Services controleert het eigendom van de naam van domein of doordat een `CName` record met de Azure Media Services-account-ID als een onderdeel dat moet worden toegevoegd aan het domein wordt gebruikt. Een voorbeeld: voor 'sports.contoso.com' moet worden gebruikt als een aangepaste hostnaam voor het streaming-eindpunt, een record voor `<accountId>.contoso.com` moet worden geconfigureerd om te verwijzen naar een van de hostnamen voor Media Services-verificatie. De naam van de verificatie van verifydns bestaat. \<mediaservices-dns-zone >. De volgende tabel bevat de verwachte DNS-zones moet worden gebruikt in de record controleren voor verschillende Azure-regio's.<br /><br /> Noord-Amerika, Europa, Singapore, Hongkong, Japan:<br /><br /> -mediaservices.windows.net<br /><br /> -verifydns.mediaservices.windows.net<br /><br /> China:<br /><br /> -mediaservices.chinacloudapi.cn<br /><br /> -verifydns.mediaservices.chinacloudapi.cn<br /><br /> Bijvoorbeeld, een `CName` record die wordt toegewezen '945a4c4e-28ea-45-cd-8ccb-a519f6b700ad.contoso.com' naar 'verifydns.mediaservices.windows.net' blijkt dat de 945a4c4e-28ea-45cd-8ccb-a519f6b700ad Azure Media Services-ID het eigendom van heeft de domein contoso.com, waardoor u van elke naam in contoso.com moet worden gebruikt als een aangepaste hostnaam voor een streaming-eindpunt onder dat account.<br /><br /> Als u de Media Service-ID-waarde zoekt, gaat u naar de [Azure-portal](https://portal.azure.com/) en selecteer uw Media Service-account. De MEDIA SERVICE-ID wordt weergegeven aan de rechterkant van de pagina DASHBOARD.<br /><br /> **Waarschuwing**: als er een poging tot het instellen van een aangepaste hostnaam zonder een juiste verificatie van de `CName` record, het DNS-antwoord mislukken en worden opgeslagen in het cachegeheugen voor enige tijd. Zodra een juiste record ingesteld is, kan het even duren voordat totdat het antwoord in de cache opnieuw wordt gevalideerd. Afhankelijk van de DNS-provider voor het aangepaste domein, kan het een paar minuten tot een uur tot duren valideren van de record.<br /><br /> Naast de `CName` die wordt toegewezen `<accountId>.<parent domain>` naar `verifydns.<mediaservices-dns-zone>`, moet u een andere `CName` die de aangepaste hostnaam wordt toegewezen (bijvoorbeeld `sports.contoso.com`) naar uw Media Services-StreamingEndpont-hostnaam (bijvoorbeeld `amstest.streaming.mediaservices.windows.net`).<br /><br /> **Houd er rekening mee**: Streaming-eindpunten die zich in hetzelfde Datacenter niet delen dezelfde aangepaste hostnaam.<br /> Deze eigenschap is ongeldig voor Standard en premium streaming-eindpunten en kunnen worden ingesteld als "cdnEnabled": false<br/><br/> Op dit moment biedt geen AMS ondersteuning voor SSL met aangepaste domeinen.  |  
|`maxCacheAge`|Onderdrukt de standaard max-age HTTP cache control-header ingesteld door het streaming-eindpunt op media fragmenten en op aanvraag manifesten. De waarde is ingesteld in een paar seconden.|
|`resourceState`|Waarden voor de eigenschap zijn onder andere:<br /><br /> -Is gestopt. De initiële status van een streaming-eindpunt nadat is gemaakt.<br /><br /> -Wordt gestart. Het streaming-eindpunt is overstappen naar de actieve status.<br /><br /> -Wordt uitgevoerd. Het streaming-eindpunt kan inhoud te streamen naar clients.<br /><br /> -Schaal. De schaaleenheden worden verhoogd of verlaagd.<br /><br /> -Moet worden gestopt. Het streaming-eindpunt is overstappen naar de status ' gestopt '.<br/><br/> -Wordt verwijderd. Het streaming-eindpunt wordt verwijderd.|
|`scaleUnits `|scaleUnits bieden u speciale uitgangscapaciteit die kan worden aangeschaft per 200 Mbps. Als u verplaatsen wilt naar een **Premium** typt, aanpassen `scaleUnits`. |

## <a name="next-steps"></a>Volgende stappen

Het voorbeeld [in deze opslagplaats](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) laat zien hoe u start het standaardstreaming-eindpunt met .NET.

