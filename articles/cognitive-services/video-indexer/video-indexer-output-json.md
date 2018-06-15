---
title: Bekijk de uitvoer op Azure Video indexeerfunctie | Microsoft Docs
description: In dit onderwerp wordt de uitvoer van de Video indexeerfunctie gecontroleerd.
services: cognitive services
documentationcenter: ''
author: juliako
manager: cflower
ms.service: cognitive-services
ms.topic: article
ms.date: 05/30/2018
ms.author: juliako
ms.openlocfilehash: 7f61833d70cea3a6d176f5a08eb0db1c30e70e86
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "35345910"
---
# <a name="examine-the-video-indexer-output-produced-by-v1-api"></a>Bekijk de Video indexeerfunctie uitvoer geproduceerd door API v1

> [!Note]
> De Video indexeerfunctie V1 API's zijn gedeprecieerd en wordt verwijderd tijdens het 1e augustus 2018. U moet beginnen met de Video indexeerfunctie v2 API's om te voorkomen van onderbrekingen.
>
> Om te zetten met Video indexeerfunctie v2-API's, raadpleegt u de instructies [hier](https://api-portal.videoindexer.ai/). 

Als u aanroept de **ophalen storingen** API en de antwoordstatus in orde is, kunt u kunt een gedetailleerde JSON-uitvoer als de antwoordinhoud. De JSON-inhoud bevat details van de opgegeven video inzichten inclusief (tekst, optisch, mensen). Trefwoorden (onderwerpen), vlakken, blokkeert de volgende gegevens bevatten. Elk blok bevat tijdsbereik, de tekst van regels, OCR regels, patronen, vlakken en blokkeren van miniatuurweergaven.

U kunt de **ophalen storingen** API's voor de volledige uitsplitsing van een video als een JSON-inhoud ophalen.  
 
    GET https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/63c6d532ff HTTP/1.1
    Host: videobreakdown.azure-api.net
    Ocp-Apim-Subscription-Key: ••••••••••••••••••••••••••••••••
    
U kunt ook visueel samengevatte insights van de video bekijken door te drukken de **afspelen** knop op de video in de portal Video indexeerfunctie. Zie voor meer informatie [weergeven en bewerken video insights](video-indexer-view-edit.md).

![Inzichten](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

In dit artikel onderzoekt de JSON-inhoud die wordt geretourneerd door de **ophalen storingen** API. Dit is mogelijk handig om te controleren de [concepten](video-indexer-concepts.md) artikel.

> [!NOTE]
> Vervaldatum van de toegangstokens in Video indexeerfunctie is een uur.

## <a name="root-elements"></a>Hoofdelementen

Kenmerk | Beschrijving
---|---
id|De Id van deze video. Bijvoorbeeld: 63c6d532ff.
partitie|Een logische partitie die de gebruiker in uploaden om te zoeken naar deze later kunt opgeven.
naam|De naam van de video. Bijvoorbeeld: Azure bewaken.
description|De beschrijving van de video. Bijvoorbeeld 'John Kemnetz koppelt Scott Hanselman om te laten zien hoe u de kracht van Azure bewakingsgegevens ontgrendelen met Azure-Monitor'.
Gebruikersnaam|De maker van de video. Bijvoorbeeld: Channel9 video's.
createTime |De tijd is gemaakt. Bijvoorbeeld, 2017-03-31T16:36:41.4504249 + 00:00 uur.
privacyMode|De video kan een van de volgende modi hebben: **persoonlijke**, **openbare**. **Openbare** -de video voor iedereen in uw account en iedereen die een koppeling naar de video zichtbaar is. **Persoonlijke** -de video zichtbaar is voor iedereen in uw account.
isOwned|True als de huidige gebruiker eigenaar is van de video. Anders wordt onwaar.  
isBase|Waar als de uitsplitsing is gebaseerd op een bronvideo. ONWAAR als de uitsplitsing van een afspeellijst die is afgeleid van een andere uitsplitsing.
durationInSeconds|De duur van de video.
summarizedInsights|Bevat een [summarizedInsights](#summarizedInsights).
Storingen|Kan een of meer bevatten [storingen](#breakdowns)
Sociale|Bevat een **sociale** element dat aantal beschrijft positieve en weergaven van de video.

## <a name="summarizedinsights"></a>summarizedInsights

In deze sectie bevat een overzicht van de inzichten.

Kenmerk | Beschrijving
---|---
naam|De naam van de video. Bijvoorbeeld: Azure bewaken.
shortId|De ID van de video. Bijvoorbeeld: 63c6d532ff.
privacyMode|De verdeling kan een van de volgende modi hebben: **persoonlijke**, **openbare**. **Openbare** -de video voor iedereen in uw account en iedereen die een koppeling naar de video zichtbaar is. **Persoonlijke** -de video zichtbaar is voor iedereen in uw account.
duur|Bevat een duur die beschrijft de tijd die een beter inzicht is opgetreden. Er is een duur in seconden.
thumbnailUrl|De video miniatuur volledige URL. Bijvoorbeeld 'https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO.... " U ziet als de video persoonlijke, de URL een toegangstoken van één uur bevat. Na een uur, de URL niet langer geldig en moet u aan de uitsplitsing opnieuw met een nieuwe url in het ophalen of aanroepen GetAccessToken om een nieuw toegangstoken en de volledige url handmatig maken ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken [= accessToken]').
vlakken|Kan een of meer bevatten [vlakken](#faces)
Onderwerpen|Kan een of meer bevatten [onderwerpen](#topics)
Zie|Kan een of meer bevatten [patronen](#sentiments)
audioEffects| Kan een of meer bevatten [audioEffects](#audioEffects)
merken| Kan bevatten nul of meer [merken](#brands)
statistieken|Zie voor meer informatie [statistieken](#Statistics)
.
### <a name="statistics"></a>statistieken

|Naam|Beschrijving|
|---|---|
|CorrespondenceCount|Het aantal berichten in de video.|
|WordCount|Het aantal woorden per spreker.|
|SpeakerNumberOfFragments|De hoeveelheid fragmenten de spreker heeft in een video.|
|SpeakerLongestMonolog|De spreker langste monolog. Als de spreker silences binnen de monolog is opgenomen. Stilte aan het begin- en het einde van de monolog wordt verwijderd.| 
|SpeakerTalkToListenRatio|De berekening is gebaseerd op de tijd die op de spreker monolog (zonder de stilte in tussen) gedeeld door de totale tijd van de video. De tijd wordt afgerond op het derde decimaalteken.|

## <a name="breakdowns"></a>Storingen

Deze sectie bevat de details van de inzichten.

Kenmerk | Beschrijving
---|---
id|De uitsplitsing-ID. Bijvoorbeeld: 63c6d532ff.
state|De verwerkingsstatus van de uitsplitsing van de opgegeven id. Kan een van de volgende zijn: geüploade, verwerking, verwerkt, is mislukt.
processingProgress|De voortgang. Bijvoorbeeld: 10%.
externalId| Tijdens het uploaden, kunt u externalId instellen. Bijvoorbeeld "4f9c3500-eca7-4ab3-987e-a745017af698." U kunt later zoeken naar uw video's op deze externe-ID.
externalUrl|Tijdens het uploaden, kunt u externalUrl instellen. 
metagegevens|U kunt metagegevens instellen tijdens het uploaden. 
inzichten|Kan een of meer bevatten [insights](#insights)
thumbnailUrl|De video miniatuur volledige URL. Bijvoorbeeld 'https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO... '. U ziet als de video persoonlijke, de URL een toegangstoken van één uur bevat. Na een uur, de URL niet langer geldig en moet u aan de uitsplitsing opnieuw met een nieuwe url in het ophalen of aanroepen GetAccessToken om een nieuw toegangstoken en de volledige url handmatig maken ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken [= accessToken]').
publishedUrl|De gepubliceerde URL. Bijvoorbeeld 'https://BreakdownMedia.azureedge.net:443/d5e5232d-48e2-4fbc-9893-0ea6335da563/Azure%20Monitor%20%20Azure%20Friday.ism/manifest. "
viewToken|De bearer-token
sourceLanguage|De bron-taal. De volgende worden ondersteund: Chinees, Engels, Frans, Duits, Italiaans, Japans, Portugees, Russisch, Spaans.
taal|De taal van de tekst van de.

## <a name="insights"></a>inzichten

Kenmerk | Beschrijving 
---|---
transcriptBlocks|Kan een of meer bevatten [transcriptBlocks](#transcriptBlocks)
Onderwerpen|Kan een of meer bevatten [onderwerpen](#topics)
vlakken|Kan een of meer bevatten [vlakken](#faces)
deelnemers|Kan een of meer bevatten [deelnemers](#participants)
contentModeration|Kan een teken bevatten dat [contentModeration](#contentModeration)
audioEffectsCategories|Kan een of meer bevatten [audioEffectsCategories](#audioEffectsCategories)

## <a name="faces"></a>vlakken

### <a name="summarizedinsights"></a>summarizedInsights

**vlakken** die worden vermeld onder **summarizedInsights**, een samenvatting van elke gevonden in de video face weergeven.

Kenmerk | Beschrijving 
---|---
id|De ID van een persoon. Bijvoorbeeld: 11775.
shortId|De korte-ID. Omdat een afspeellijst kan worden afgeleid van verschillende storingen, is deze ID nodig om erachter te komen welke van de verdeling van de oorsprong van elke gezicht is.  
naam|Als de face wordt herkend, wordt de naam van de persoon toegevoegd. Bijvoorbeeld 'Scott Hanselman'. Als de face onbekend is, wordt "Onbekende #" toegevoegd. 
description|Als de face wordt herkend, kan de beschrijving is ingevuld op basis van de zoekopdracht Bing-API. Anders wordt de beschrijving is **null**.
titel|Als de face wordt herkend, kan de beschrijving is ingevuld op basis van de zoekopdracht Bing-API. Anders wordt de titel is **null**.
thumbnailFullUrl|De face miniatuur volledige URL. Bijvoorbeeld 'https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO.... " U ziet als de video persoonlijke, de URL een toegangstoken van één uur bevat. Na een uur, de URL niet langer geldig en moet u aan de uitsplitsing opnieuw met een nieuwe url in het ophalen of aanroepen GetAccessToken om een nieuw toegangstoken en de volledige url handmatig maken ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken [= accessToken]').
weergaven|Kan een of meer bevatten [weergaven](#appearances)
seenDuration|Voor hoe lang de knopafbeelding is gezien (in seconden).
seenDurationRatio|Aanwezigheid ten opzichte van de video duur (0-1).

### <a name="breakdown-insights"></a>uitsplitsing insights

**vlakken** die worden vermeld onder **storingen**, informatie over elke gevonden in de video face beschrijven.

Kenmerk | Beschrijving 
---|---
id|De ID van een persoon. Bijvoorbeeld: 11775.
bingId|
naam|Als de face wordt herkend, wordt de naam van de persoon toegevoegd. Bijvoorbeeld 'Scott Hanselman'. Als de face onbekend is, wordt "Onbekende #" toegevoegd. 
thumbnailId|Bijvoorbeeld, 616468f0-. 1636-4efa-94e7-262f2e575059.
description|Als de face wordt herkend, kan de beschrijving is ingevuld op basis van de zoekopdracht Bing-API. Anders wordt de beschrijving is **null**.
titel|Als de face wordt herkend, kan de beschrijving is ingevuld op basis van de zoekopdracht Bing-API. Anders wordt de titel is **null**.
imageUrl|Deze URL verwijst naar een afbeelding die is genomen van de bron video.  
vertrouwen|De score vertrouwen (hoger is beter).
knownPersonId|De id van een bekende persoon (bijvoorbeeld beroemdheden). Als u niet bekend is van een persoon, bevat de id nullen. Bijvoorbeeld, e3eaff5f-ee1b-4eac-80ce-ebac47aadf64.

## <a name="topics"></a>Onderwerpen

### <a name="summarizedinsights"></a>summarizedInsights

**onderwerpen over** die worden vermeld onder **summarizedInsights**, een samenvatting van elk onderwerp gevonden in de video weergeven.

Kenmerk | Beschrijving 
---|---
naam|De naam van het onderwerp (bijvoorbeeld 'Azure'). 
weergaven|Kan een of meer bevatten [uiterlijk](#appearances).
isTranscript|True is, als gevonden in de tekst van een. False, als in een OCR gevonden.

### <a name="breakdown-insights"></a>uitsplitsing insights

**onderwerpen over** die worden vermeld onder **storingen**, details over elk onderwerp gevonden in de video wordt beschreven.

|Kenmerk | Beschrijving |
|---|---|
|id|Unieke onderwerp-ID.|
|naam|De naam van het onderwerp.|
|stam|Dit kenmerk wordt momenteel niet gebruikt.|
|woorden|Dit kenmerk wordt momenteel niet gebruikt.|
|positie|Relevantie score (hoger is beter).|

## <a name="sentiments"></a>Zie

Kenmerk | Beschrijving
---|---
sentimentKey| Op dit moment wordt de volgende patronen worden ondersteund: positieve, neutrale, negatief. 
weergaven|Kan een of meer bevatten [weergaven](#appearances)|.
seenDurationRatio|Aanwezigheid ten opzichte van de video duur (0-1).

## <a name="audioeffects"></a>audioEffects

Kenmerk | Beschrijving 
---|---
audioEffectKey| Geldige waarden zijn: spraak, stilte, HandClaps.
weergaven|Kan een of meer bevatten [weergaven](#appearances)
seenDurationRatio|Aanwezigheid ten opzichte van de video duur (0-1).
seenDuration|Hoe lang de audio-effect was aanwezig zijn (in seconden).

## <a name="appearances"></a>weergaven

Kenmerk | Beschrijving 
---|---
startTime| Tijdwaarde.
endTime|Tijdwaarde.
startSeconds| Tijdwaarde.
endSeconds| Tijdwaarde.

## <a name="participants"></a>deelnemers

Kenmerk | Beschrijving 
---|---
id|De ID van de deelnemer.
naam|De naam van de deelnemer. Bijvoorbeeld: Sprekeridentificatie #1.
Afbeeldings-URL|De **afbeeldings-URL** kenmerk is gereserveerd voor toekomstig gebruik.

## <a name="contentmoderation"></a>contentModeration

Kenmerk | Beschrijving 
---|---
adultClassifierValue|Het vertrouwensniveau dat de video inhoud voor volwassenen heeft.
racyClassifierValue|Het vertrouwensniveau dat de video mooie inhoud heeft.
bannedWordsCount|Aantal taalgebruik woorden. 
bannedWordsRatio|Verhouding van taalgebruik woorden van het totale aantal woorden.
reviewRecommended|Booleaanse waarde die aangeeft of de video handmatig moet worden gecontroleerd.
isAdult|Boole-waarden die aangeeft of de video wordt beschouwd als een volwassene video na handmatige controle.

## <a name="audioeffectscategories"></a>audioEffectsCategories

Kenmerk | Beschrijving 
---|---
type|ID van de categorie.
sleutel|Een van de volgende:-spraak, stilte, HandClaps. 

## <a name="transcriptblocks"></a>transcriptBlocks

Kenmerk | Beschrijving
---|---
id|ID van het blok.
regels|Kan een of meer bevatten [regels](#lines)
sentimentIds|De **sentimentIds** kenmerk is gereserveerd voor toekomstig gebruik.
thumbnailIds|De **thumbnailIds** kenmerk is gereserveerd voor toekomstig gebruik.
Gevoel|De gevoel in het blok (0-1, negatief voor positief).
vlakken|Kan een of meer bevatten [vlakken](#faces).
optisch|Kan een of meer bevatten [optisch](#ocrs).
audioEffectInstances|Kan een of meer bevatten [audioEffectInstances](#audioEffectInstances).
schermen|Kan een of meer bevatten [schermen](#scenes).
Aantekeningen|Kan bevatten nul of meer [aantekeningen](#annotations).

## <a name="ocrs"></a>optisch

Hierin wordt beschreven op welk moment in de video die de text-inhoud is gevonden. 

Kenmerk | Beschrijving 
---|---
timeRange|Het tijdsbereik in de oorspronkelijke video.
adjustedTimeRange|AdjustedTimeRange is het tijdsbereik ten opzichte van de huidige afspeellijst. Omdat u een afspeellijst van andere regels van verschillende video's maken kunt, kunt u een één uur video nemen en slechts 1 regel af, bijvoorbeeld: 10:00-10:15. In dat geval hebt u een afspeellijst met 1 regel, waar het tijdsbereik 10:00-10:15 is, maar de adjustedTimeRange is 00:00-00:15.
regels|Kan een of meer bevatten [regels](#lines).

## <a name="lines"></a>regels

### <a name="transcriptblocks"></a>transcriptBlocks

**regels** die worden vermeld onder **transcriptBlocks**, regels van transcripties gevonden in de video wordt beschreven.

Kenmerk | Beschrijving 
---|---
id| De ID van de regel.
timeRange|Het tijdsbereik in de oorspronkelijke video.
adjustedTimeRange|AdjustedTimeRange is het tijdsbereik ten opzichte van de huidige afspeellijst. Omdat u een afspeellijst van andere regels van verschillende video's maken kunt, kunt u een één uur video nemen en slechts 1 regel af, bijvoorbeeld: 10:00-10:15. In dat geval hebt u een afspeellijst met 1 regel, waar het tijdsbereik 10:00-10:15 is, maar de adjustedTimeRange is 00:00-00:15.
participantID| De id van de spreker van deze regel.
tekst| De tekst van de.
isIncluded| In de base storingen altijd waar. In de afgeleide afspeellijsten, de regels die zijn opgenomen in de bronvideo zijn ingesteld op isIncluded = true. Alle andere regels zijn ingesteld op false.

### <a name="ocrs"></a>optisch

**regels** die worden vermeld onder **optisch**, regels van optisch gevonden in de video wordt beschreven.

Kenmerk | Beschrijving 
---|---
id|De OCR-ID.
Breedte|Dit kenmerk wordt momenteel niet gebruikt.
Hoogte|Dit kenmerk wordt momenteel niet gebruikt.
taal|De OCR-taal.
textData|De tekst.
vertrouwen|De score vertrouwen (hoger is beter).

## <a name="scenes"></a>schermen

Kenmerk | Beschrijving 
---|---
id|De scene-ID.
timeRange|Bevat een **timeRange**.
keyFrame|De tijd van de belangrijkste frame.
schermafbeeldingen|Kan een of meer bevatten [schermafbeeldingen](#shots).

## <a name="shots"></a>schermafbeeldingen

Kenmerk | Beschrijving 
---|---
id||De schermopname-ID.
timeRange|Bevat een **timeRange**.
keyFrame|De tijd van de belangrijkste frame.

## <a name="audioeffectinstances"></a>audioEffectInstances

Kenmerk | Beschrijving 
---|---
type|De index van de audio-gebeurtenis: voorspoed = 1, HandClaps = 2, muziek = 3, spraak = 4, stilte = 5
bereiken|Kan een of meer bevatten [bereiken](#ranges).

## <a name="ranges"></a>bereiken

**bereiken** die worden vermeld onder **audioEffectInstances**, audio-effecten in deze bereiken worden beschreven.

Kenmerk | Beschrijving 
---|---
timeRange|Het tijdsbereik in de oorspronkelijke video.
adjustedTimeRange|AdjustedTimeRange is het tijdsbereik ten opzichte van de huidige afspeellijst. Omdat u een afspeellijst van andere regels van verschillende video's maken kunt, kunt u een één uur video nemen en slechts één regel af, bijvoorbeeld: 10:00-10:15. In dat geval hebt u een afspeellijst met 1 regel, waar het tijdsbereik 10:00-10:15 is, maar de adjustedTimeRange is 00:00-00:15.

## <a name="annotations"></a>Aantekeningen

Retourneert de tags op basis van herkenbare objecten, levende wezens, achtergrond, acties en visuele patronen.

|Kenmerk|Beschrijving|
|---|---|
|id|De ID van de aantekening.|
|Naam|De naam van de aantekening (bijvoorbeeld, persoon, sport spel zwart Frames).|
|weergaven|Kan een of meer weergaven bevatten.|

## <a name="brands"></a>merken

Bedrijfs- en merknamen gedetecteerd in de spraak naar de tekst van de tekst en/of Video OCR. Dit omvat geen visual herkenning van merken of logo detectie.

Kenmerk | Beschrijving
---|---
id | De ID van een merk.
naam | De naam van het merk van Bing of een aangepaste merk.  
wikiId | Het achtervoegsel van de url merk-Wikipedia (Engelstalig). Bijvoorbeeld, "Target_Corporation" is het achtervoegsel van [ https://en.wikipedia.org/wiki/Target_Corporation ](https://en.wikipedia.org/wiki/Target_Corporation).
wikiUrl | Het merk de url van de Wikipedia (Engelstalig), als bestaat. Bijvoorbeeld [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
vertrouwen | De waarde van de betrouwbaarheid van de Video indexeerfunctie merk detectie (0-1).
description | Beschrijving van het merk geëxtraheerd uit Wikipedia (Engelstalig). 
weergaven | Kan een of meer weergaven bevatten.
seenDuration | Aanwezigheid ten opzichte van de video duur (0-1).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het maken van uw eigen uitsplitsing [weergeven en bewerken van Video indexeerfunctie insights](video-indexer-view-edit.md).

Zie voor meer informatie over het insluiten van widgets in uw toepassing [insluiten van Video indexeerfunctie widgets in uw toepassingen](video-indexer-embed-widgets.md). 

## <a name="see-also"></a>Zie ook

[Video indexeerfunctie API](https://videobreakdown.portal.azure-api.net/docs/services/582074fb0dc56116504aed75/operations/5857caeb0dc5610f9ce979e4)

[Video indexeerfunctie-overzicht](video-indexer-overview.md)

