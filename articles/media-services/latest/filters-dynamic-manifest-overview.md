---
title: Filters en Azure Media Services dynamische manifesten | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u filters maken, zodat de client voor het specifieke secties van de stroom van een stroom gebruiken kunt. Media Services wordt gemaakt dynamische manifesten voor het bereiken van deze selectief streaming.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 07/11/2019
ms.author: juliako
ms.openlocfilehash: ee0200f7c007b437a27b8e9d0f36becc13b8f611
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67835822"
---
# <a name="pre-filtering-manifests-by-using-dynamic-packager"></a>Vooraf manifesten filteren door dynamische Packager gebruiken

Wanneer u bent adaptive bitrate streaming van inhoud naar de apparaten geleverd, moet u vaak meerdere versies van een manifest publiceren naar de specifieke apparaatmogelijkheden doel of de beschikbare netwerkbandbreedte. De [dynamische Packager](dynamic-packaging-overview.md) kunt u opgeven de filters die u om specifieke codecs filteren kunnen, oplossingen, bitsnelheden en audio bijhouden combinaties op tijdens het verwijderen van de noodzaak om meerdere kopieën te maken. U hoeft voor het publiceren van een nieuwe URL met een specifieke set filters die zijn geconfigureerd voor uw doelapparaten (iOS, Android, SmartTV of browsers) en de netwerkmogelijkheden (hoge bandbreedte, mobiel of met een lage bandbreedte scenario's). In dit geval clients het streamen van uw inhoud via de queryreeks kunnen bewerken (door op te geven beschikbaar [Asset filters of accountfilters](filters-concept.md)) en filters naar specifieke secties van de stroom van een stroom gebruiken.

Sommige delivery-scenario's vereisen dat u controleert dat de klant is geen toegang tot specifieke sporen te wissen. U kunt bijvoorbeeld niet voor het publiceren van een manifest met HD-nummers naar een specifieke abonnee-laag. Of u wilt verwijderen van de nummers van specifieke adaptieve bitrate (ABR) om te verminderen van kosten voor levering aan een specifiek apparaat die niet voordeel van de nummers hebben veel. In dit geval kan koppelt u een lijst met vooraf gemaakte filters met uw [Streaming-Locator gemaakt](streaming-locators-concept.md) gemaakt. In dit geval clients niet bewerken, hoe de inhoud worden gestreamd, deze wordt gedefinieerd door de **Streaming-Locator gemaakt**.

U kunt combineren filteren door op te geven [filters op Streaming-Locator gemaakt](filters-concept.md#associating-filters-with-streaming-locator) + extra apparaat specifieke filters waarmee de client wordt aangegeven in de URL. Dit kan nuttig zijn om te beperken van aanvullende worden bijgehouden, zoals metagegevens of gebeurtenis-streams, audio talen of beschrijvende audionummers zijn. 

Deze mogelijkheid om op te geven van verschillende filters voor uw stroom, biedt een krachtige **dynamische Manifest** manipulatie oplossing om meerdere use-casescenario's voor de doelapparaten. Dit onderwerp wordt uitgelegd concepten met betrekking tot **dynamische manifesten** en bevat voorbeelden van scenario's waarin kunt u deze functie wilt gebruiken.

> [!NOTE]
> De asset en de standaard-manifest voor de activa wijzigen dynamische manifesten niet. 
> 

##  <a name="overview-of-manifests"></a>Overzicht van de manifesten

Azure Media Services biedt ondersteuning voor HLS, MPEG DASH en Smooth Streaming-protocollen. Als onderdeel van [dynamische verpakking](dynamic-packaging-overview.md), de streaming clientmanifesten (Master HLS Playlist, DASH Media presentatiebeschrijving [MPD] en Smooth Streaming) dynamisch worden gegenereerd op basis van de selectie van de indeling in de URL. Zie de protocollen voor levering in [algemene werkstroom voor on-demand](dynamic-packaging-overview.md#delivery-protocols). 

### <a name="get-and-examine-manifest-files"></a>Ophalen en onderzoeken-manifestbestanden

Kunt u opgeven een lijst van filter bijhouden Eigenschapvoorwaarden op basis van welke nummers van uw stream (live of video op aanvraag [VOD]) moeten worden opgenomen in een dynamisch gemaakte manifest. U kunt ophalen en controleert u de eigenschappen van de nummers, die u moet eerst het manifest Smooth Streaming laden.

De [uploaden, coderen en streamen van bestanden met .NET](stream-files-tutorial-with-api.md#get-streaming-urls) zelfstudie leert u over het bouwen van de streaming-URL's met .NET. Als u de app uitvoert, een van de URL verwijst naar het manifest Smooth Streaming: `https://amsaccount-usw22.streaming.media.azure.net/00000000-0000-0000-0000-0000000000000/ignite.ism/manifest`.<br/> Kopieer en plak de URL in de adresbalk van een browser. Het bestand wordt gedownload. U kunt deze openen in een teksteditor van uw keuze.

Zie voor een voorbeeld van een REST, [uploaden, coderen en streamen van bestanden met REST](stream-files-tutorial-with-rest.md#list-paths-and-build-streaming-urls).

### <a name="monitor-the-bitrate-of-a-video-stream"></a>De bitrate van een videostream bewaken

U kunt de [Azure Media Player-demo pagina](http://aka.ms/azuremediaplayer) voor het bewaken van de bitrate van een videostream. De demo-pagina Diagnostische gegevens wordt weergegeven op de **Diagnostics** tabblad:

![Azure Media Player diagnostics][amp_diagnostics]
 
### <a name="examples-urls-with-filters-in-query-string"></a>Voorbeelden: URL's met filters in de queryreeks

U kunt filters toepassen op ABR protocollen voor streaming: HLS, MPEG-DASH en Smooth Streaming. De volgende tabel ziet u enkele voorbeelden van URL's met filters:

|Protocol|Voorbeeld|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

## <a name="rendition-filtering"></a>Weergavefiltering

U kunt uw asset voor meerdere codering profielen (H.264 basislijn H.264 hoog, AACL, AACH, Dolby Digital Plus) en meerdere kwaliteit bitsnelheden coderen. Niet alle clientapparaten ondersteunen echter de profielen en bitrates van uw asset. Oudere Android-apparaten ondersteunen bijvoorbeeld alleen H.264 basislijn + AACL. Hogere bitsnelheden verzenden naar een apparaat dat niet kan profiteren verspilt netwerkbandbreedte en apparaat berekening. Een apparaat moet worden gedecodeerd alle opgegeven informatie, alleen voor omlaag schalen om weer te geven.

Met dynamische Manifest u apparaatprofielen maken (zoals mobiele apparaten, console, of HD/SD), maar ook de sporen te wissen en de kenmerken die u wilt worden van een onderdeel van elk profiel. Die wordt aangeroepen weergavefiltering. Het volgende diagram toont een voorbeeld hiervan.

![Voorbeeld van weergavefiltering][renditions2]

In het volgende voorbeeld is een coderingsprogramma gebruikt een tussentijds asset coderen in zeven ISO MP4s video voorinstelling (van 180p 1080p). De gecodeerde asset mag [dynamisch verpakte](dynamic-packaging-overview.md) in een van de volgende protocollen voor streaming: HLS, MPEG DASH en Smooth. 

De bovenkant van het volgende diagram toont dat de HLS manifest voor de asset met geen filters. (Deze bevat alle zeven voorinstelling.)  In de linkerbenedenhoek van het scherm toont het diagram een HLS-manifest waarop een filter met de naam "ott" is toegepast. Het filter "ott" Hiermee geeft u het verwijderen van alle bitsnelheden hieronder 1 Mbps, zodat de kwaliteit van onder twee niveaus zijn verwijderd uit in het antwoord. In de rechterbenedenhoek toont het diagram de HLS-manifest waarop een filter met de naam 'mobiel' is toegepast. Het filter 'mobiel' Hiermee geeft u het verwijderen van vertoningen waar de oplossing groter dan 720p, is zodat de twee 1080 p voorinstelling zijn overblijft.

![Weergavefiltering][renditions1]

## <a name="removing-language-tracks"></a>Taal verwijderen
Uw activa advies inwinnen bij meerdere audio talen zoals Engels, Spaans, Frans, enzovoort. Normaal gesproken standaardselectie audiotrack voor de Windows Media Player SDK-managers en beschikbaar audio worden bijgehouden per Gebruikersselectie.

Het ontwikkelen van dergelijke Player SDK's is lastig, omdat er verschillende implementaties is vereist voor apparaat-specifieke player-frameworks. Bovendien Player-API's op sommige platformen moet kennis zijn beperkt en omvatten niet de functie audio selectie waar gebruikers niet selecteren of het audiospoor standaardinstelling wijzigen. Met asset filters, kunt u het gedrag beheren met het maken van de filters die zijn alleen de gewenste audio talen.

![Het filteren van taal][language_filter]

## <a name="trimming-the-start-of-an-asset"></a>Het begin van een asset bijsnijden

Operators Voer enkele tests uit voordat u de werkelijke gebeurtenis in de meeste live streaming-gebeurtenissen. Ze zijn bijvoorbeeld onder andere een slate als volgt vóór het begin van de gebeurtenis: "Programma wordt verbroken." 

Als het archiveren van het programma is zijn de test- en slates gegevens ook gearchiveerd en opgenomen in de presentatie. Deze informatie moet niet worden weergegeven op de clients. U kunt met dynamische Manifest maken van een filter voor start en de ongewenste gegevens verwijderen uit het manifest.

![Start bijsnijden][trim_filter]

## <a name="creating-subclips-views-from-a-live-archive"></a>Subclips (weergaven) te maken van een live-archief

Veel live gebeurtenissen zijn langere tijd worden uitgevoerd en livearchief meerdere gebeurtenissen kan opnemen. Nadat de live-gebeurtenis is geëindigd, willen tv-zenders splitst u het livearchief in logische programma starten en stoppen van reeksen. 

U kunt deze virtuele programma's afzonderlijk publiceren zonder na verwerking van het livearchief en niet het maken van afzonderlijke activa (dit is het voordeel van het bestaande in de cache-fragmenten niet ophalen in het CDN's). Voorbeelden van dergelijke virtuele programma's zijn het kwartaal van een football of basketbalwedstrijd, innings in baseball of afzonderlijke gebeurtenissen van een sport-programma.

Met dynamische Manifest, kunt u filters maken met behulp van de begin-/ eindtijd en virtuele weergaven maken via de bovenkant van uw live-archief. 

![Subclip filter][subclip_filter]

Hier volgt de gefilterde asset:

![Skiën][skiing]

## <a name="adjusting-the-presentation-window-dvr"></a>Aanpassen van het presentatievenster (DVR)

Op dit moment biedt Azure Media Services circulaire archief waar de duur tussen 5 minuten en 25 uur kan worden geconfigureerd. Manifest filteren kan worden gebruikt om u te maken van een rolling DVR-venster op de bovenkant van het archief, zonder te verwijderen van media. Er zijn veel scenario's waarin tv-zenders wilt met een beperkte DVR-venster als u wilt gaan met de rand van het live en op hetzelfde moment een grotere archiveren venster bieden. Een tv-station mogelijk wilt u de gegevens die zich buiten de DVR-venster om te markeren clips gebruiken of ze wilt voor verschillende DVR-vensters voor verschillende apparaten. De meeste van de mobiele apparaten verwerken niet zo groot DVR windows (u kunt een 2 minuten DVR-venster voor mobiele apparaten en 1 uur voor bureaublad-clients hebben).

![DVR-venster][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>LiveBackoff (live positie) aanpassen

Manifest filteren kan worden gebruikt om enkele seconden verwijderen uit de live rand van een live-programma. Filteren, kunt de presentatie op het punt van de publicatie preview bekijken en maken aankondiging voordat de viewers de stream (ondersteund uitschakelen door 30 seconden) ontvangt invoegen punten tv-zenders. Tv-zenders kunnen vervolgens deze aankondigingen push naar de client-frameworks in tijd te ontvangen en verwerken van de gegevens voordat u de advertentie-mogelijkheid.

Naast de ondersteuning van de aankondiging, kan de instelling voor live uitstel de viewers positie aanpassen zodat wanneer clients drift en druk op de rand van het live, ze nog steeds fragmenten van de server krijgen kunnen worden gebruikt. Op die manier kunnen clients krijgen geen een HTTP 404 of 412-fout.

![Filter voor live uitstel][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>Het combineren van meerdere regels in één filter

U kunt meerdere regels voor filteren in één filter combineren. Zo kunt u een regel' bereik' slates verwijderen uit een live-archief en ook filteren om beschikbare bitsnelheden. Wanneer u meerdere filterregels toepast, is het eindresultaat het snijpunt van alle regels.

![Meerdere filterregels][multiple-rules]

## <a name="combining-multiple-filters-filter-composition"></a>Meerdere filters (filter samenstelling) combineren

U kunt ook meerdere filters in een URL voor eenmalige combineren. De volgende scenario wordt uitgelegd waarom u kunt filters combineren:

1. U moet voor het filteren van uw video kenmerken voor mobiele apparaten, zoals Android- of iPad (om te kunnen beperken video kenmerken). Als u wilt verwijderen van de ongewenste kenmerken, maakt u een filter account geschikt is voor de apparaatprofielen. U kunt accountfilters gebruiken voor al uw activa onder de dezelfde Media Services-account zonder verdere koppeling.
1. U wilt ook de begin- en -tijd van een asset trim. Om dit te bereiken, u een filter asset maken en de begin-/ eindtijd. 
1. Wilt u beide van deze filters combineren. Zonder combinatie moet u om toe te voegen kwaliteit filteren om het filter bijsnijden, waardoor de filter gebruik moeilijker.


Als u wilt combineren filters, moet u de filternamen instellen op het manifest/afspeellijst URL in de indeling met door puntkomma's gescheiden. Stel, u hebt een filter met de naam *MyMobileDevice* die kenmerken filters en u hebt een ander met de naam *MyStartTime* om in te stellen een specifieke begintijd. U kunt maximaal drie filters combineren. 

Zie voor meer informatie, [dit blogbericht](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).

## <a name="considerations-and-limitations"></a>Overwegingen en beperkingen

- De waarden voor **forceEndTimestamp**, **presentationWindowDuration**, en **liveBackoffDuration** mag niet worden ingesteld voor een VOD-filter. Ze worden gebruikt voor scenario's voor live-filter. 
-  Een dynamische manifest werkt in GOP grenzen (sleutel frames), dus bijsnijden heeft GOP nauwkeurigheid.
-  U kunt de naam van het hetzelfde filter voor account- en asset filters gebruiken. Asset-filters hebben een hogere prioriteit en accountfilters wordt overschreven.
- Als u een filter bijwerkt, kan het tot 2 minuten voor het streaming-eindpunt om te vernieuwen van de regels duren. Als u filters gebruikt om de inhoud (en u de inhoud van proxy's in cache opgeslagen en CDN-cache), kan deze filters bijwerken resulteren in fouten player. Het is raadzaam dat u de cache wissen na het bijwerken van het filter. Als deze optie niet mogelijk is, kunt u overwegen een ander filter.
- Klanten moeten handmatig download het manifest en de tijdstempel van de exacte startdatum parseren en tijdschaal.
    
    - Eigenschappen van de nummers in een actief [ophalen en bekijk het manifestbestand](#get-and-examine-manifest-files).
    - De formule om in te stellen van de eigenschappen van filter tijdstempel van asset is: <br/>startTimestamp = &lt;begintijd in het manifest&gt; +  &lt;verwacht filter starttijd in seconden&gt; * tijdschaal

## <a name="next-steps"></a>Volgende stappen

De volgende artikelen laten zien hoe u filters maken via een programma:  

- [Filters maken met REST-API 's](filters-dynamic-manifest-rest-howto.md)
- [Filters maken met .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Filters maken met CLI](filters-dynamic-manifest-cli-howto.md)

[renditions1]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/filters-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/filters-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/filters-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/filters-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/filters-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/filters-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/filters-dynamic-manifest-overview/media-services-skiing.png
[amp_diagnostics]: ./media/filters-dynamic-manifest-overview/amp_diagnostics.png
