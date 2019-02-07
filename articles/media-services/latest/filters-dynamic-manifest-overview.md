---
title: Filters en Azure Media Services dynamische manifesten | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u filters maken, zodat de client voor het specifieke secties van de stroom van een stroom gebruiken kunt. Media Services wordt gemaakt dynamische manifesten als u wilt archiveren deze selectief streaming.
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
ms.date: 01/24/2019
ms.author: juliako
ms.openlocfilehash: 9c463740acf6ef464880a43e0e68de683b97f64f
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55813415"
---
# <a name="filters-and-dynamic-manifests"></a>Filters en dynamische manifesten

Wanneer uw inhoud levert aan klanten (streaming Live gebeurtenissen of Video on Demand) is de client mogelijk meer flexibiliteit dan wat wordt beschreven in het manifestbestand van de standaard-asset. Azure Media Services kunt u accountfilters en filters actief voor uw inhoud definiëren. 

Filters zijn serverzijde regels waarmee uw klanten voor handelingen zoals: 

- Alleen een gedeelte van een video (in plaats van de hele video afspelen) worden afgespeeld. Bijvoorbeeld:

    - Beperken van het manifest om weer te geven van een subclip van een live-gebeurtenis ("subclips filteren'), of
    - Verwijder het begin van een video ("een video bijsnijden').

- Lever alleen de opgegeven voorinstelling en/of opgegeven taal nummers die worden ondersteund door het apparaat dat wordt gebruikt voor het afspelen van de inhoud ("weergavefiltering"). 
- Presentatie venster (DVR) aanpassen om te voorzien van een beperkt de lengte van de DVR-venster in de speler ("aanpassen presentatievenster").

Dit onderwerp wordt beschreven [concepten](#concepts) en bevat definities van filters. Vervolgens krijgt informatie over algemene scenario's. Aan het einde van het artikel vindt u koppelingen die laten zien hoe u filters maken via een programma.  

## <a name="concepts"></a>Concepten

### <a name="dynamic-manifests"></a>Dynamische manifesten

Media Services biedt **dynamische manifesten** op basis van vooraf gedefinieerde filters. Zodra u filters definiëren, kunnen uw clients deze gebruiken om een specifieke weergave of subquery korte clips van uw video te streamen. Ze zou filter (s) in de streaming-URL opgeven. Filters kunnen worden toegepast op adaptieve bitrate streamingprotocollen: Apple HTTP Live Streaming (HLS), MPEG-DASH en Smooth Streaming. 

De volgende tabel ziet u enkele voorbeelden van URL's met filters:

|Protocol|Voorbeeld|
|---|---|
|HLS V4-PROCESSORS|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|HLS V3|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3,filter=myAccountFilter)`|
|MPEG DASH|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=myAssetFilter)`|

> [!NOTE]
> De asset en de standaard-manifest voor de activa wijzigen dynamische manifesten niet. De client kunt kiezen om aan te vragen van een stream met of zonder filters. 
> 
> 

### <a name="manifest-files"></a>Manifestbestanden

Wanneer u een asset voor adaptive bitratestreaming, codeert een **manifest** ()-afspeellijstbestand wordt gemaakt (het bestand is op basis van tekst of XML-indeling). De **manifest** bestand bevat metagegevens zoals streaming: bijhouden van type (audio, video of tekst), naam, begintijd en eindtijd, bitrate (kenmerken), bijhouden talen, presentatievenster (sliding window van vaste duur), video-codec (bijhouden Code). Hiermee geeft u ook de speler om op te halen van het volgende fragment door te geven informatie over het volgende kan worden afgespeeld video fragmenten beschikbaar en hun locatie. Fragmenten (of segmenten) zijn de werkelijke 'segmenten' van een video-inhoud.

Hier volgt een voorbeeld van een manifestbestand HLS: 

```
#EXT-X-MEDIA:TYPE=AUDIO,GROUP-ID="audio",NAME="aac_eng_2_128041_2_1",LANGUAGE="eng",DEFAULT=YES,AUTOSELECT=YES,URI="QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)"
#EXT-X-STREAM-INF:BANDWIDTH=536209,RESOLUTION=320x180,CODECS="avc1.64000d,mp4a.40.2",AUDIO="audio"
QualityLevels(380658)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=536209,RESOLUTION=320x180,CODECS="avc1.64000d",URI="QualityLevels(380658)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=884474,RESOLUTION=480x270,CODECS="avc1.640015,mp4a.40.2",AUDIO="audio"
QualityLevels(721426)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=884474,RESOLUTION=480x270,CODECS="avc1.640015",URI="QualityLevels(721426)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=1327838,RESOLUTION=640x360,CODECS="avc1.64001e,mp4a.40.2",AUDIO="audio"
QualityLevels(1155246)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=1327838,RESOLUTION=640x360,CODECS="avc1.64001e",URI="QualityLevels(1155246)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=2414544,RESOLUTION=960x540,CODECS="avc1.64001f,mp4a.40.2",AUDIO="audio"
QualityLevels(2218559)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=2414544,RESOLUTION=960x540,CODECS="avc1.64001f",URI="QualityLevels(2218559)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=3805301,RESOLUTION=1280x720,CODECS="avc1.640020,mp4a.40.2",AUDIO="audio"
QualityLevels(3579378)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=3805301,RESOLUTION=1280x720,CODECS="avc1.640020",URI="QualityLevels(3579378)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=139017,CODECS="mp4a.40.2",AUDIO="audio"
QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)
```

### <a name="get-and-examine-manifest-files"></a>Ophalen en onderzoeken-manifestbestanden

U geeft een lijst van de eigenschap van de filtervoorwaarden bijhouden op basis van waarop de nummers van uw stream (Live of Video on Demand) moeten worden opgenomen in de dynamisch gemaakte manifest. U kunt ophalen en controleert u de eigenschappen van de nummers, die u moet eerst het manifest Smooth Streaming laden.

De [uploaden, coderen en streamen van bestanden met .NET](stream-files-tutorial-with-api.md) zelfstudie leert u hoe u kunt de streaming-URL's met .NET bouwen (zien, de [het bouwen van URL's](stream-files-tutorial-with-api.md#get-streaming-urls) sectie). Als u de app uitvoert, een van de URL verwijst naar het manifest Smooth Streaming: `https://amsaccount-usw22.streaming.media.azure.net/00000000-0000-0000-0000-0000000000000/ignite.ism/manifest`.<br/>Kopieer en plak de URL in de adresbalk van een browser. Het bestand wordt gedownload. U kunt deze openen in een teksteditor van uw keuze.

Zie voor het voorbeeld REST [uploaden, coderen en streamen van bestanden met REST](stream-files-tutorial-with-rest.md#list-paths-and-build-streaming-urls).

### <a name="monitor-the-bitrate-of-a-video-stream"></a>De bitrate van een videostream bewaken

U kunt de [Azure Media Player-demo pagina](http://aka.ms/amp) voor het bewaken van de bitrate van een videostream. De demo-pagina wordt weergegeven voor informatie over de diagnostische gegevens in de **Diagnostics** tabblad:

![Azure Media Player diagnostics][amp_diagnostics]

## <a name="defining-filters"></a>Filters definiëren

Er zijn twee typen asset filters: 

* [Account Filters](https://docs.microsoft.com/rest/api/media/accountfilters) (wereldwijde) - kunnen worden toegepast op alle activa in het Azure Media Services-account, hebben een levensduur van het account.
* [Asset-Filters](https://docs.microsoft.com/rest/api/media/assetfilters) (lokaal) - kan alleen worden toegepast op een asset waarmee het filter gekoppeld nadat deze is gemaakt is, hebben een levensduur van de asset. 

[Filter account](https://docs.microsoft.com/rest/api/media/accountfilters) en [Asset Filter](https://docs.microsoft.com/rest/api/media/assetfilters) typen hebben dezelfde eigenschappen voor het definiëren van/beschrijven van het filter. Met uitzondering van bij het maken van de **Asset Filter**, moet u de naam van de activa die u wilt koppelen van het filter opgeven.

Afhankelijk van uw scenario, kunt u bepalen welk type een filter is beter geschikt (Asset filteren of Account filteren). Accountfilters zijn geschikt voor apparaatprofielen (weergavefiltering) waar Asset Filters kan worden gebruikt om een specifieke asset trim.

U de volgende eigenschappen voor het beschrijven van de filters gebruiken. 

|Name|Description|
|---|---|
|firstQuality|De eerste kwaliteit bitrate van het filter.|
|presentationTimeRange|Het tijdsbereik van de presentatie. Deze eigenschap wordt gebruikt voor het filteren van manifest start/eindpunten, de lengte van de presentatie-venster en de beginpositie van live. <br/>Zie voor meer informatie, [PresentationTimeRange](#PresentationTimeRange).|
|sporen te wissen|De voorwaarden van de nummers selectie. Zie voor meer informatie, [sporen te wissen](#tracks)|

### <a name="presentationtimerange"></a>PresentationTimeRange

Gebruik deze eigenschap met **Asset Filters**. Het is niet raadzaam om in te stellen de eigenschap met de **Accountfilters**.

|Name|Description|
|---|---|
|**endTimestamp**|De grens van de absolute end-tijd. Van toepassing op Video on Demand (VoD). Voor de Live presentatie, wordt deze op de achtergrond genegeerd en toegepast wanneer de presentatie-ends en de stroom wordt weergegeven als VoD.<br/><br/>De waarde vertegenwoordigt een absolute eindpunt van de stroom. Dit wordt afgerond naar de dichtstbijzijnde volgende GOP starten.<br/><br/>Gebruik StartTimestamp en EndTimestamp waaruit de afspeellijst (manifest). Bijvoorbeeld, StartTimestamp = 40000000 en EndTimestamp = 100000000 een afspeellijst met media tussen StartTimestamp en EndTimestamp wordt gegenereerd. Als een fragment de grens gevestigd, wordt het hele fragment worden opgenomen in het manifest.<br/><br/>Zie ook de **forceEndTimestamp** definitie die volgt.|
|**forceEndTimestamp**|Van toepassing op actieve filters.<br/><br/>**forceEndTimestamp** is de Booleaanse waarde die aangeeft of **endTimestamp** is ingesteld op een geldige waarde. <br/><br/>Als de waarde **waar**, wordt de **endTimestamp** waarde moet worden opgegeven. Als deze niet is opgegeven, wordt een onjuiste aanvraag geretourneerd.<br/><br/>Als bijvoorbeeld, u wilt definiëren van een filter op dat bij 5 minuten in de video-invoer begint en duurt tot het einde van de stroom, u stelt **forceEndTimestamp** op onwaar en laat de instelling **endTimestamp**.|
|**liveBackoffDuration**|Geldt alleen voor Live. De eigenschap wordt gebruikt voor het afspelen van live positie definiëren. Deze regel gebruikt, kunt u live afspelen positie vertraging en maken van een buffer-serverzijde voor spelers. LiveBackoffDuration is ten opzichte van de live positie. De duur van de maximale live uitstel is 300 seconden.|
|**presentationWindowDuration**|Van toepassing op Live. Gebruik **presentationWindowDuration** een sliding window toepassen op de afspeellijst. Bijvoorbeeld, stel presentationWindowDuration = 1200000000 om toe te passen een sliding window van twee minuten. Media binnen twee minuten van de rand van het live worden opgenomen in de afspeellijst. Als een fragment de grens gevestigd, wordt het hele fragment worden opgenomen in de afspeellijst. De duur van het venster minimale presentatie is 60 seconden.|
|**startTimestamp**|Geldt voor VoD- of Live streams. De waarde vertegenwoordigt een absolute beginpunt van de stroom. De waarde wordt afgerond naar de dichtstbijzijnde volgende GOP starten.<br/><br/>Gebruik **startTimestamp** en **endTimestamp** waaruit de afspeellijst (manifest). Bijvoorbeeld, startTimestamp = 40000000 en endTimestamp = 100000000 een afspeellijst met media tussen StartTimestamp en EndTimestamp wordt gegenereerd. Als een fragment de grens gevestigd, wordt het hele fragment worden opgenomen in het manifest.|
|**timescale**|Geldt voor VoD- of Live streams. De tijdschaal die worden gebruikt door de tijdstempels en de hierboven opgegeven duur. De standaard-tijdschaal is 10000000. Een alternatieve tijdschaal kan worden gebruikt. De standaardwaarde is 10000000 HNS (100 nanoseconden).|

### <a name="tracks"></a>sporen te wissen

U geeft een lijst met filter bijhouden Eigenschapvoorwaarden (FilterTrackPropertyConditions) op basis van waarop de nummers van uw stream (Live of Video on Demand) moeten worden opgenomen in de dynamisch gemaakte manifest. De filters worden gecombineerd met behulp van een logische **en** en **of** bewerking.

Eigenschap van de filtervoorwaarden bijhouden beschrijven tracktypen, waarden (in de volgende tabel beschreven) en bewerkingen (gelijk, NotEqual). 

|Name|Description|
|---|---|
|**Bitrate**|Gebruik de bitrate van het spoor voor filteren.<br/><br/>De aanbevolen waarde is een reeks bitsnelheden in bits per seconde. Bijvoorbeeld: '0-2427000'.<br/><br/>Opmerking: tijdens een specifieke bitrate-waarde, zoals 250000 (bits per seconde), kunt u deze methode wordt niet aanbevolen, omdat de exacte bitsnelheden van een Asset naar een andere variëren kan.|
|**FourCC**|Gebruik de waarde van de code van het spoor voor het filteren.<br/><br/>De waarde is het eerste element van codecs indeling, zoals opgegeven in [RFC 6381](https://tools.ietf.org/html/rfc6381). Op dit moment worden de volgende codecs ondersteund: <br/>Video: "Avc1", "hev1", "hvc1"<br/>Voor Audio: "Mp4a", "EG-3"<br/><br/>Om te bepalen van de waarden van de code voor de nummers in een actief [ophalen en bekijk het manifestbestand](#get-and-examine-manifest-files).|
|**Taal**|Gebruik de taal van de track voor filteren.<br/><br/>De waarde is de code van een taal die u opnemen wilt, als opgegeven in RFC 5646. Bijvoorbeeld, "en".|
|**Naam**|Gebruik de naam van de track voor het filteren.|
|**Type**|Gebruik het type van het spoor voor het filteren.<br/><br/>De volgende waarden zijn toegestaan: "video", 'audio' of 'tekst'.|

### <a name="example"></a>Voorbeeld

```json
{
  "properties": {
    "presentationTimeRange": {
      "startTimestamp": 0,
      "endTimestamp": 170000000,
      "presentationWindowDuration": 9223372036854776000,
      "liveBackoffDuration": 0,
      "timescale": 10000000,
      "forceEndTimestamp": false
    },
    "firstQuality": {
      "bitrate": 128000
    },
    "tracks": [
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Audio"
          },
          {
            "property": "Language",
            "operation": "NotEqual",
            "value": "en"
          },
          {
            "property": "FourCC",
            "operation": "NotEqual",
            "value": "EC-3"
          }
        ]
      },
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Video"
          },
          {
            "property": "Bitrate",
            "operation": "Equal",
            "value": "3000000-5000000"
          }
        ]
      }
    ]
  }
}
```

## <a name="rendition-filtering"></a>Weergavefiltering

U kunt uw asset voor meerdere codering profielen (H.264 basislijn H.264 hoog, AACL, AACH, Dolby Digital Plus) en meerdere kwaliteit bitsnelheden coderen. Niet alle clientapparaten ondersteunen echter de profielen en bitrates van uw asset. Oudere Android-apparaten ondersteunen bijvoorbeeld alleen H.264 basislijn + AACL. Hogere bitsnelheden verzenden naar een apparaat, wat kan niet aan de voordelen, verspilt netwerkbandbreedte en apparaat-berekeningen. Dit apparaat moet worden gedecodeerd alle opgegeven informatie, alleen voor omlaag schalen om weer te geven.

Met dynamische Manifest u apparaatprofielen maken, zoals mobiele apparaten, console, HD/SD, enz., en omvatten de sporen te wissen en de kenmerken die u wilt worden van een onderdeel van elk profiel.

![Weergave filteren voorbeeld][renditions2]

In het volgende voorbeeld is een coderingsprogramma gebruikt een tussentijds asset coderen in zeven ISO MP4s video voorinstelling (van 180p 1080p). De gecodeerde asset kan dynamisch worden verpakt in een van de volgende protocollen voor streaming: HLS, MPEG DASH en Smooth.  Aan de bovenkant van het diagram, het HLS-manifest voor de asset met geen filters wordt weergegeven (bevat alle zeven voorinstelling).  In de linksonder, wordt het HLS-manifest waarop een filter met de naam "ott" is toegepast weergegeven. Het filter "ott" Hiermee geeft u als u wilt verwijderen van alle bitsnelheden hieronder 1 Mbps, wat leidde tot de twee quality-niveaus van onder, worden verwijderd uit in het antwoord. In de rechts onderaan wordt het HLS-manifest waarop een filter met de naam 'mobiel' is toegepast weergegeven. Het filter 'mobiel' Hiermee geeft u het verwijderen van vertoningen waar de oplossing is groter dan 720p, wat leidde tot de twee 1080p voorinstelling wordt overblijft.

![Weergavefiltering][renditions1]

## <a name="removing-language-tracks"></a>Taal verwijderen
Uw activa advies inwinnen bij meerdere audio talen zoals Engels, Spaans, Frans, enzovoort. Normaal gesproken standaardselectie audiotrack voor de Windows Media Player SDK-managers en beschikbaar audio worden bijgehouden per Gebruikersselectie. Het is lastig voor het ontwikkelen van dergelijke Player SDK's, hiervoor verschillende implementaties voor apparaat-specifieke player-frameworks. Ook Player-API's op sommige platformen moet kennis zijn beperkt en omvatten geen audio selectie functie waar gebruikers niet selecteren of het audiospoor standaardinstelling wijzigen. Met asset filters, kunt u het gedrag beheren met het maken van de filters die zijn alleen de gewenste audio talen.

![Taal filteren][language_filter]

## <a name="trimming-start-of-an-asset"></a>Begin van een asset bijsnijden
Operators Voer enkele tests uit voordat u de werkelijke gebeurtenis in de meeste live streaming-gebeurtenissen. Ze zijn bijvoorbeeld onder andere een slate als volgt vóór het begin van de gebeurtenis: 'Programma wordt overgenomen door'. Als het archiveren van het programma is zijn de test- en slates gegevens ook gearchiveerd en opgenomen in de presentatie. Deze informatie moet niet worden weergegeven op de clients. U kunt met dynamische Manifest maken van een filter voor start en de ongewenste gegevens verwijderen uit het manifest.

![Start bijsnijden][trim_filter]

## <a name="creating-subclips-views-from-a-live-archive"></a>Subclips (weergaven) te maken van een live-archief
Veel live gebeurtenissen zijn langere tijd worden uitgevoerd en livearchief meerdere gebeurtenissen kan opnemen. Nadat de live-gebeurtenis is geëindigd, willen tv-zenders splitst u het livearchief in logische programma starten en stoppen van reeksen. Vervolgens deze virtuele-programma's publiceren afzonderlijk zonder na verwerking van het livearchief en niet het maken van afzonderlijke activa (dit is het voordeel van het bestaande in de cache-fragmenten niet ophalen in het CDN's). Voorbeelden van dergelijke virtuele programma's zijn het kwartaal van een football of basketbalwedstrijd, innings in baseball of afzonderlijke gebeurtenissen van een sport-programma.

U kunt met dynamische Manifest filters met de begin-/ eindtijd maken en virtuele weergaven maken via de bovenkant van uw live-archief. 

![Subclip filter][subclip_filter]

Gefilterde Asset:

![Skiën][skiing]

## <a name="adjusting-presentation-window-dvr"></a>Presentatievenster (DVR) aanpassen
Op dit moment biedt Azure Media Services circulaire archief waar de duur tussen 5 minuten: 25 uur kan worden geconfigureerd. Manifest filteren kan worden gebruikt om u te maken van een rolling DVR-venster op de bovenkant van het archief, zonder te verwijderen van media. Er zijn veel scenario's waarin tv-zenders wilt met een beperkte DVR-venster als u wilt gaan met de rand van het live en op hetzelfde moment een grotere archiveren venster bieden. Een tv-station mogelijk wilt u de gegevens die zich buiten de DVR-venster om te markeren clips gebruiken of ze wilt voor verschillende DVR-vensters voor verschillende apparaten. De meeste van de mobiele apparaten verwerken niet zo groot DVR windows (u kunt een 2 minuten DVR-venster voor mobiele apparaten en één uur voor bureaublad-clients hebben).

![DVR-venster][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>LiveBackoff (live positie) aanpassen
Manifest filteren kan worden gebruikt om enkele seconden verwijderen uit de live rand van een live-programma. Filteren, kunt de presentatie op het punt van de publicatie preview bekijken en maken aankondiging voordat de viewers de stream (back-off van 30 seconden) ontvangt invoegen punten tv-zenders. Tv-zenders vervolgens push deze aankondigingen op de client-frameworks in tijd te ontvangen en verwerken van de gegevens voordat u de advertentie-mogelijkheid.

Naast de ondersteuning van de aankondiging, kan de instelling LiveBackoff worden gebruikt met het aanpassen van de positie viewers, zodat wanneer clients drift en druk op de rand van het live ze nog steeds fragmenten kunnen ophalen van de server in plaats van een HTTP 404 of 412-fout.

![livebackoff_filter][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>Het combineren van meerdere regels in één filter
U kunt meerdere regels voor filteren in één filter combineren. Zo kunt u een regel' bereik' slates verwijderen uit een live-archief en ook filteren om beschikbare bitsnelheden. Bij het toepassen van meerdere regels voor filteren, is het eindresultaat het snijpunt van alle regels.

![meerdere regels][multiple-rules]

## <a name="combining-multiple-filters-filter-composition"></a>Meerdere filters (filter samenstelling) combineren

U kunt ook meerdere filters in een URL voor eenmalige combineren. 

De volgende scenario wordt uitgelegd waarom u kunt filters combineren:

1. U moet voor het filteren van uw video kenmerken voor mobiele apparaten, zoals Android- of iPAD (om te kunnen beperken video kenmerken). Als u wilt verwijderen van de ongewenste kenmerken, maakt u een filter Account geschikt is voor de apparaatprofielen. Accountfilters kunnen worden gebruikt voor al uw activa onder de dezelfde media services-account zonder verdere koppeling. 
2. U wilt ook de begin- en -tijd van een asset trim. Om dit te doen, zou u een filter Asset maken en de begin-/ eindtijd. 
3. U wilt beide van deze filters combineren (zonder combinatie, moet u om toe te voegen kwaliteit filteren om het filter bijsnijden, waardoor filter gebruik lastiger zijn).

Als u wilt combineren filters, moet u de filternamen instellen op het manifest/afspeellijst-URL met door puntkomma's gescheiden. Stel, u hebt een filter met de naam *MyMobileDevice* die kenmerken filters en hebt u een ander met de naam *MyStartTime* om in te stellen een specifieke begintijd. U kunt ze combineren als volgt:

U kunt maximaal drie filters combineren. 

Zie voor meer informatie, [dit](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blog.

## <a name="considerations-and-limitations"></a>Overwegingen en beperkingen

- De waarden voor **forceEndTimestamp**, **presentationWindowDuration**, en **liveBackoffDuration** mag niet worden ingesteld voor een VoD-filter. Ze worden alleen gebruikt voor scenario's voor live-filter. 
- Dynamische manifest werkt in GOP grenzen (sleutel Frames) daarom bijsnijden heeft GOP nauwkeurigheid. 
- U kunt dezelfde filternaam voor Account- en Asset filters gebruiken. Asset-filters hebben een hogere prioriteit en accountfilters wordt overschreven.
- Als u een filter bijwerkt, kan het tot 2 minuten voor streaming-eindpunt om te vernieuwen van de regels duren. Als de inhoud is uitgevoerd met bepaalde filters (en caches in proxy's en CDN-cache), kan deze filters bijwerken resulteren in fouten player. Het verdient aanbeveling om de cache leeg na het bijwerken van het filter. Als deze optie niet mogelijk is is, kunt u overwegen een ander filter.
- Klanten moeten handmatig het manifest downloaden en parseren van de exacte startTimestamp en schaal.
    
    - Eigenschappen van de nummers in een actief [ophalen en bekijk het manifestbestand](#get-and-examine-manifest-files).
    - De formule voor het instellen van het filter asset timestamp-eigenschappen: <br/>startTimestamp = &lt;begintijd in het manifest&gt; +  &lt;verwacht filter starttijd in seconden&gt;* tijdschaal


## <a name="next-steps"></a>Volgende stappen

De volgende artikelen laten zien hoe om filters te maken via een programma.  

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
