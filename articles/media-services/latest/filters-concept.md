---
title: Filters definiëren in Azure Media Services
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
ms.date: 02/12/2019
ms.author: juliako
ms.openlocfilehash: 09de372ffdb48c00fde9a43c07f8f8b574462d1f
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56405714"
---
# <a name="define-account-filters-and-asset-filters"></a>Accountfilters en asset filters definiëren  

Wanneer uw inhoud levert aan klanten (streaming Live gebeurtenissen of Video on Demand) is de client mogelijk meer flexibiliteit dan wat wordt beschreven in het manifestbestand van de standaard-asset. Azure Media Services kunt u accountfilters en filters actief voor uw inhoud definiëren. 

Filters zijn serverzijde regels waarmee uw klanten voor handelingen zoals: 

- Alleen een gedeelte van een video (in plaats van de hele video afspelen) worden afgespeeld. Bijvoorbeeld:
  - Beperken van het manifest om weer te geven van een subclips van live-gebeurtenis ("subclips filteren'), of
  - Verwijder het begin van een video ("een video bijsnijden').
- Lever alleen de opgegeven voorinstelling en/of opgegeven taal nummers die worden ondersteund door het apparaat dat wordt gebruikt voor het afspelen van de inhoud ("weergavefiltering"). 
- Presentatie venster (DVR) aanpassen om te voorzien van een beperkt de lengte van de DVR-venster in de speler ("aanpassen presentatievenster").

Media Services biedt [dynamische manifesten](filters-dynamic-manifest-overview.md) op basis van vooraf gedefinieerde filters. Als u filters definiëren, kunnen uw clients deze gebruiken in de streaming-URL. Filters kunnen worden toegepast op adaptieve bitrate streamingprotocollen: Apple HTTP Live Streaming (HLS), MPEG-DASH en Smooth Streaming.

De volgende tabel ziet u enkele voorbeelden van URL's met filters:

|Protocol|Voorbeeld|
|---|---|
|HLS V4-PROCESSORS|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|HLS V3|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3,filter=myAccountFilter)`|
|MPEG DASH|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=myAssetFilter)`|

## <a name="define-filters"></a>Filters definiëren

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

## <a name="example"></a>Voorbeeld

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

## <a name="next-steps"></a>Volgende stappen

De volgende artikelen laten zien hoe om filters te maken via een programma.  

- [Filters maken met REST-API 's](filters-dynamic-manifest-rest-howto.md)
- [Filters maken met .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Filters maken met CLI](filters-dynamic-manifest-cli-howto.md)

