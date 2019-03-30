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
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 61b877c322fcd58472990c328beea2e309502bce
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652345"
---
# <a name="define-account-filters-and-asset-filters"></a>Accountfilters en asset filters definiëren  

Wanneer uw inhoud levert aan klanten (Live Streaming-gebeurtenissen of Video on Demand) is de client mogelijk meer flexibiliteit dan wat wordt beschreven in het manifestbestand van de standaard-asset. Azure Media Services kunt u accountfilters en filters actief voor uw inhoud definiëren. 

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
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`<br/>Gebruik voor HLS-v3: `format=m3u8-aapl-v3`.|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

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
|presentationTimeRange|Het tijdsbereik van de presentatie. Deze eigenschap wordt gebruikt voor het filteren van manifest start/eindpunten, de lengte van de presentatie-venster en de beginpositie van live. <br/>Zie voor meer informatie, [PresentationTimeRange](#presentationtimerange).|
|sporen te wissen|De voorwaarden van de nummers selectie. Zie voor meer informatie, [sporen te wissen](#tracks)|

### <a name="presentationtimerange"></a>presentationTimeRange

Gebruik deze eigenschap met **Asset Filters**. Het is niet raadzaam om in te stellen de eigenschap met de **Accountfilters**.

|Name|Description|
|---|---|
|**endTimestamp**|Van toepassing op Video on Demand (VoD).<br/>Voor de presentatie Live Streaming, wordt deze op de achtergrond genegeerd en toegepast wanneer de presentatie-ends en de stroom wordt weergegeven als VoD.<br/>Dit is een long-waarde die een absolute eindpunt van de presentatie, afgerond op het dichtstbijzijnde volgende GOP begin vertegenwoordigt. De eenheid is de tijdschaal, dus een endTimestamp van 1800000000 voor de 3 minuten is.<br/>Gebruik startTimestamp en endTimestamp waaruit de fragmenten die weergegeven in de afspeellijst (manifest worden).<br/>Bijvoorbeeld, startTimestamp = 40000000 en endTimestamp 100000000 = een afspeellijst met fragmenten tussen 4 seconden en 10 seconden van de presentatie VoD met behulp van de standaard-tijdschaal wordt gegenereerd. Als een fragment de grens gevestigd, wordt het hele fragment worden opgenomen in het manifest.|
|**forceEndTimestamp**|Is van toepassing alleen Live streamen.<br/>Geeft aan of de eigenschap endTimestamp aanwezig zijn moet. Indien waar, endTimestamp moet worden opgegeven of een onjuiste aanvraagcode wordt geretourneerd.<br/>Toegestane waarden: false, true.|
|**liveBackoffDuration**|Is van toepassing alleen Live streamen.<br/> Deze waarde bepaalt de meest recente live positie aan die een client kan worden gezocht tot.<br/>Deze eigenschap gebruikt, kunt u live afspelen positie vertraging en maken van een buffer-serverzijde voor spelers.<br/>De eenheid voor deze eigenschap is tijdschaal (Zie hieronder).<br/>Het maximum live uitstel duur is 300 seconden (3000000000).<br/>Bijvoorbeeld: een waarde van 2000000000 betekent dat de meest recente inhoud 20 seconden vertraagd ten opzichte van de rand van het werkelijke live is.|
|**presentationWindowDuration**|Is van toepassing alleen Live streamen.<br/>Gebruik presentationWindowDuration om toe te passen een sliding window van fragmenten om op te nemen in een afspeellijst vindt.<br/>De eenheid voor deze eigenschap is tijdschaal (Zie hieronder).<br/>Bijvoorbeeld, stel presentationWindowDuration = 1200000000 om toe te passen een sliding window van twee minuten. Media binnen twee minuten van de rand van het live worden opgenomen in de afspeellijst. Als een fragment de grens gevestigd, wordt het hele fragment worden opgenomen in de afspeellijst. De duur van het venster minimale presentatie is 60 seconden.|
|**startTimestamp**|Van toepassing op Video on Demand (VoD) of Live Streaming.<br/>Dit is een long-waarde die een absolute beginpunt van de stroom vertegenwoordigt. De waarde wordt afgerond naar de dichtstbijzijnde volgende GOP starten. De eenheid is de tijdschaal, dus een startTimestamp van 150000000 15 seconden is.<br/>Gebruik startTimestamp en endTimestampp waaruit de fragmenten die weergegeven in de afspeellijst (manifest worden).<br/>Bijvoorbeeld, startTimestamp = 40000000 en endTimestamp 100000000 = een afspeellijst met fragmenten tussen 4 seconden en 10 seconden van de presentatie VoD met behulp van de standaard-tijdschaal wordt gegenereerd. Als een fragment de grens gevestigd, wordt het hele fragment worden opgenomen in het manifest.|
|**timescale**|Het is van toepassing op alle tijdstempels en de duur in het tijdsbereik van een presentatie, opgegeven als het aantal stappen in één seconde.<br/>Standaard is 10000000 - tien miljoen stappen in één seconde, waarbij elke incrementele eenheden van 100 nanoseconden lang kan worden.<br/>Bijvoorbeeld, als u een startTimestamp ingesteld op 30 seconden wilt, zou u een waarde van 300000000 bij het gebruik van de standaard-tijdschaal.|

### <a name="tracks"></a>sporen te wissen

U geeft een lijst met filter bijhouden Eigenschapvoorwaarden (FilterTrackPropertyConditions) op basis van waarop de nummers van uw stream (Live streamen of Video on Demand) moeten worden opgenomen in de dynamisch gemaakte manifest. De filters worden gecombineerd met behulp van een logische **en** en **of** bewerking.

Eigenschap van de filtervoorwaarden bijhouden beschrijven tracktypen, waarden (in de volgende tabel beschreven) en bewerkingen (gelijk, NotEqual). 

|Name|Description|
|---|---|
|**Bitrate**|Gebruik de bitrate van het spoor voor filteren.<br/><br/>De aanbevolen waarde is een reeks bitsnelheden in bits per seconde. Bijvoorbeeld: '0-2427000'.<br/><br/>Opmerking: tijdens een specifieke bitrate-waarde, zoals 250000 (bits per seconde), kunt u deze methode wordt niet aanbevolen, omdat de exacte bitsnelheden van een Asset naar een andere variëren kan.|
|**FourCC**|Gebruik de waarde van de code van het spoor voor het filteren.<br/><br/>De waarde is het eerste element van codecs indeling, zoals opgegeven in [RFC 6381](https://tools.ietf.org/html/rfc6381). Op dit moment worden de volgende codecs ondersteund: <br/>Video: "Avc1", "hev1", "hvc1"<br/>Voor Audio: "Mp4a", "EG-3"<br/><br/>Om te bepalen van de code-waarden voor de nummers in een Asset, ophalen en onderzoeken van het manifestbestand.|
|**Taal**|Gebruik de taal van de track voor filteren.<br/><br/>De waarde is de code van een taal die u opnemen wilt, als opgegeven in RFC 5646. Bijvoorbeeld, "en".|
|**Naam**|Gebruik de naam van de track voor het filteren.|
|**Type**|Gebruik het type van het spoor voor het filteren.<br/><br/>De volgende waarden zijn toegestaan: "video", 'audio' of 'tekst'.|

## <a name="associate-filters-with-streaming-locator"></a>Filters met Streaming-Locator gemaakt koppelen

U kunt een lijst met activa of account filters, die voor uw Streaming-Locator gemaakt gelden. De [dynamische packager](dynamic-packaging-overview.md) deze lijst met filters samen met die de client Hiermee geeft u in de URL van toepassing is. Deze combinatie genereert een [dyanamic manifest](filters-dynamic-manifest-overview.md), die is gebaseerd op filters in de URL en filters die u op Streaming-Locator gemaakt opgeeft. U wordt aangeraden dat u deze functie gebruiken als u filters wilt toepassen, maar niet wilt weergeven van de filterfunctie namen in de URL.

## <a name="definition-example"></a>Voorbeeld van de definitie

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

