---
title: Bekijk de uitvoer Azure Video Indexer | Microsoft Docs
description: In dit onderwerp onderzoekt de Video Indexer-uitvoer.
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
# <a name="examine-the-video-indexer-output-produced-by-v1-api"></a>Bekijk de Video Indexer-uitvoer geproduceerd door de v1-API

> [!Note]
> De Video Indexer V1 API's zijn afgeschaft en op 1 augustus 2018 wordt verwijderd. U moet beginnen met behulp van de Video Indexer v2-API's om storingen te voorkomen.
>
> Als u wilt ontwikkelen met Video Indexer v2-API's, Zie de instructies [hier](https://api-portal.videoindexer.ai/). 

Wanneer u aanroepen de **ophalen uitsplitsingen** API en de status van het antwoord is OK, kunt u een gedetailleerde JSON-uitvoer als de inhoud van de reactie. De JSON-inhoud bevat details van de opgegeven inzichten in video's met inbegrip van (transcript, optisch, mensen). Trefwoorden (onderwerpen), gezichten, blokkeert de volgende gegevens bevatten. Elk blok bevat tijdsbereik, transcript regels, OCR-regels, sentimenten, gezichten, of te blokkeren van miniaturen.

U kunt de **ophalen uitsplitsingen** API om op te halen van de volledige uitsplitsing van een video als een JSON-inhoud.  
 
    GET https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/63c6d532ff HTTP/1.1
    Host: videobreakdown.azure-api.net
    Ocp-Apim-Subscription-Key: ••••••••••••••••••••••••••••••••
    
U kunt ook visueel overzicht van de video-inzichten bekijken door te drukken de **afspelen** knop op de video in de Video Indexer-portal. Zie voor meer informatie, [weergeven en bewerken inzichten in video's](video-indexer-view-edit.md).

![Inzichten](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

In dit artikel onderzoekt de JSON-inhoud die wordt geretourneerd door de **ophalen uitsplitsingen** API. Kan het zijn handig om te controleren de [concepten](video-indexer-concepts.md) artikel.

> [!NOTE]
> Vervaldatum van de toegangstokens in Video Indexer is één uur.

## <a name="root-elements"></a>Basis-elementen

Kenmerk | Beschrijving
---|---
id|De Id van deze video. Bijvoorbeeld: 63c6d532ff.
partitie|Een logische partitie die de gebruiker in uploaden om te zoeken naar deze later kunt opgeven.
naam|De naam van de video. Bijvoorbeeld: Azure Monitor.
description|De beschrijving van de video. Bijvoorbeeld, "John Kemnetz lid wordt van Scott Hanselman om te laten zien hoe u voor het ontgrendelen van de kracht van Azure-bewakingsgegevens met Azure Monitor."
Gebruikersnaam|De maker van de video. Bijvoorbeeld, Channel 9-video's.
CreateTime |De tijd die zijn gemaakt. Bijvoorbeeld: 2017-03-31T16:36:41.4504249 + 00:00 uur.
privacyMode|Uw video kan hebben een van de volgende modi: **persoonlijke**, **openbare**. **Openbare** -de video is zichtbaar voor iedereen in uw account en iedereen met een koppeling naar de video. **Persoonlijke** -de video is zichtbaar voor iedereen in uw account.
isOwned|Waar, als de huidige gebruiker is eigenaar van de video. Anders wordt onwaar.  
isBase|Waar, als de uitsplitsing van de is gebaseerd op een video bron. ONWAAR als de uitsplitsing van een afspeellijst vindt die is afgeleid van een andere uitsplitsing.
durationInSeconds|De duur van de video.
summarizedInsights|Bevat een [summarizedInsights](#summarizedInsights).
uitsplitsingen|Kan bevatten een of meer [uitsplitsingen](#breakdowns)
Sociale|Bevat een **sociale** element dat wordt beschreven aantal likes en weergaven van de video.

## <a name="summarizedinsights"></a>summarizedInsights

In deze sectie bevat een overzicht van de inzichten.

Kenmerk | Beschrijving
---|---
naam|De naam van de video. Bijvoorbeeld: Azure Monitor.
shortId|De ID van de video. Bijvoorbeeld: 63c6d532ff.
privacyMode|Uitsplitsing van uw kan hebben een van de volgende modi: **persoonlijke**, **openbare**. **Openbare** -de video is zichtbaar voor iedereen in uw account en iedereen met een koppeling naar de video. **Persoonlijke** -de video is zichtbaar voor iedereen in uw account.
duur|Bevat een duur van de beschrijving van de tijd die een inzicht is opgetreden. Er is een duur in seconden.
thumbnailUrl|Miniatuur van de video volledige URL. Bijvoorbeeld, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...." U ziet als de video privé is, de URL van een toegangstoken van één uur bevat. Na een uur, de URL niet langer geldig en moet u naar de uitsplitsing van de opnieuw met een nieuwe url in het ophalen of aanroepen GetAccessToken om een nieuw toegangstoken en de volledige url handmatig maken ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [ accessToken]').
gezichten|Kan bevatten een of meer [gezichten](#faces)
onderwerpen|Kan bevatten een of meer [onderwerpen](#topics)
sentimenten|Kan bevatten een of meer [sentimenten](#sentiments)
audioEffects| Kan bevatten een of meer [audioEffects](#audioEffects)
merken| Kan nul of meer bevatten [merken](#brands)
statistieken|Zie voor meer informatie, [statistieken](#Statistics)
.
### <a name="statistics"></a>statistieken

|Naam|Beschrijving|
|---|---|
|CorrespondenceCount|Het aantal berichten in de video.|
|WordCount|Het aantal woorden per spreker.|
|SpeakerNumberOfFragments|De hoeveelheid van de spreker in een video heeft fragmenten.|
|SpeakerLongestMonolog|Van de spreker langste monolog. Als de spreker silences binnen de monolog is opgenomen. Stilte aan het begin en het einde van de monolog wordt verwijderd.| 
|SpeakerTalkToListenRatio|De berekening is gebaseerd op de tijd die op van de spreker monolog (zonder de stilte in tussen) gedeeld door de totale tijd van de video. De tijd wordt afgerond op de derde decimaalteken.|

## <a name="breakdowns"></a>uitsplitsingen

In deze sectie worden de details van de inzichten.

Kenmerk | Beschrijving
---|---
id|De id van de verdeling. Bijvoorbeeld: 63c6d532ff.
state|De verwerkingsstatus van de uitsplitsing van de opgegeven id. Kan een van de volgende zijn: geüploade, verwerking, verwerkt, is mislukt.
processingProgress|De voortgang. Bijvoorbeeld: 10%.
externalId| U kunt externalId instellen tijdens het uploaden. Bijvoorbeeld, "4f9c3500-eca7-4ab3-987e-a745017af698." U kunt later zoeken naar uw video's door deze externe ID.
externalUrl|U kunt externalUrl instellen tijdens het uploaden. 
metagegevens|U kunt metagegevens instellen tijdens het uploaden. 
inzichten|Kan bevatten een of meer [insights](#insights)
thumbnailUrl|Miniatuur van de video volledige URL. Bijvoorbeeld, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO... '. U ziet als de video privé is, de URL van een toegangstoken van één uur bevat. Na een uur, de URL niet langer geldig en moet u naar de uitsplitsing van de opnieuw met een nieuwe url in het ophalen of aanroepen GetAccessToken om een nieuw toegangstoken en de volledige url handmatig maken ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [ accessToken]').
publishedUrl|De gepubliceerde URL. Bijvoorbeeld, "https://BreakdownMedia.azureedge.net:443/d5e5232d-48e2-4fbc-9893-0ea6335da563/Azure%20Monitor%20%20Azure%20Friday.ism/manifest."
viewToken|Het bearer-token
sourceLanguage|De source-taal. Het volgende worden ondersteund: Chinees, Engels, Frans, Duits, Italiaans, Japans, Portugees, Russisch, Spaans.
Taal|De taal van het transcript.

## <a name="insights"></a>inzichten

Kenmerk | Beschrijving 
---|---
transcriptBlocks|Kan bevatten een of meer [transcriptBlocks](#transcriptBlocks)
onderwerpen|Kan bevatten een of meer [onderwerpen](#topics)
gezichten|Kan bevatten een of meer [gezichten](#faces)
deelnemers|Kan bevatten een of meer [deelnemers](#participants)
contentModeration|Mag een [contentModeration](#contentModeration)
audioEffectsCategories|Kan bevatten een of meer [audioEffectsCategories](#audioEffectsCategories)

## <a name="faces"></a>gezichten

### <a name="summarizedinsights"></a>summarizedInsights

**gezichten** die worden weergegeven onder **summarizedInsights**, een samenvatting van elk gezicht gevonden in de video weergeven.

Kenmerk | Beschrijving 
---|---
id|De ID van een persoon. Bijvoorbeeld: 11775.
shortId|De korte-ID. Omdat een afspeellijst worden afgeleid van diverse storingen, is deze ID nodig om erachter te komen welke van deze specificaties van de oorsprong van elk gezicht is.  
naam|Als het gezicht wordt herkend, wordt de naam van de persoon zijn toegevoegd. Bijvoorbeeld, "Scott Hanselman." Als het gezicht onbekend is, wordt 'Onbekende #' toegevoegd. 
description|Als het gezicht wordt herkend, wordt de beschrijving ingevuld op basis van de Bing-API voor search. Anders wordt de beschrijving is **null**.
titel|Als het gezicht wordt herkend, wordt de beschrijving ingevuld op basis van de Bing-API voor search. Anders wordt de titel is **null**.
thumbnailFullUrl|Miniatuur van het gezicht volledige URL. Bijvoorbeeld, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...." U ziet als de video privé is, de URL van een toegangstoken van één uur bevat. Na een uur, de URL niet langer geldig en moet u naar de uitsplitsing van de opnieuw met een nieuwe url in het ophalen of aanroepen GetAccessToken om een nieuw toegangstoken en de volledige url handmatig maken ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [ accessToken]').
vormgeving|Kan bevatten een of meer [uiterlijk](#appearances)
seenDuration|Voor hoe lang het gezicht is gedetecteerd (in seconden).
seenDurationRatio|Aanwezigheid ten opzichte van de duur van de video (0-1).

### <a name="breakdown-insights"></a>-breakdowninzichten verkrijgen

**gezichten** die worden weergegeven onder **uitsplitsingen**, Geef een beschrijving details over elk gezicht gevonden in de video.

Kenmerk | Beschrijving 
---|---
id|De ID van een persoon. Bijvoorbeeld: 11775.
bingId|
naam|Als het gezicht wordt herkend, wordt de naam van de persoon zijn toegevoegd. Bijvoorbeeld, "Scott Hanselman." Als het gezicht onbekend is, wordt 'Onbekende #' toegevoegd. 
thumbnailId|Bijvoorbeeld, 616468f0-. 1636-4efa-94e7-262f2e575059.
description|Als het gezicht wordt herkend, wordt de beschrijving ingevuld op basis van de Bing-API voor search. Anders wordt de beschrijving is **null**.
titel|Als het gezicht wordt herkend, wordt de beschrijving ingevuld op basis van de Bing-API voor search. Anders wordt de titel is **null**.
imageUrl|Deze URL verwijst naar een afbeelding die moet worden uitgevoerd van de bron-video.  
vertrouwen|De betrouwbaarheidsscore (hoger is het beter).
knownPersonId|De id van een bekende persoon (bijvoorbeeld beroemdheid). Als een persoon niet bekend is, wordt met de id nullen bevat. Bijvoorbeeld, e3eaff5f-ee1b-4eac-80ce-ebac47aadf64.

## <a name="topics"></a>onderwerpen

### <a name="summarizedinsights"></a>summarizedInsights

**onderwerpen over** die worden weergegeven onder **summarizedInsights**, een samenvatting van elk onderwerp gevonden in de video weergeven.

Kenmerk | Beschrijving 
---|---
naam|De onderwerpnaam (bijvoorbeeld 'Azure'). 
vormgeving|Kan bevatten een of meer [uiterlijk](#appearances).
isTranscript|Waar, als in een transcript gevonden. Ingesteld op False, als in een OCR gevonden.

### <a name="breakdown-insights"></a>-breakdowninzichten verkrijgen

**onderwerpen over** die worden weergegeven onder **uitsplitsingen**, Geef een beschrijving details over elk onderwerp gevonden in de video.

|Kenmerk | Beschrijving |
|---|---|
|id|De unieke onderwerp-ID.|
|naam|De naam van het onderwerp.|
|stem|Dit kenmerk wordt op dit moment niet gebruikt.|
|woorden|Dit kenmerk wordt op dit moment niet gebruikt.|
|positie|Relevantiescore (hoger is het beter).|

## <a name="sentiments"></a>sentimenten

Kenmerk | Beschrijving
---|---
sentimentKey| Op dit moment de volgende sentimenten worden ondersteund: positief, neutraal, negatief zijn. 
vormgeving|Kan bevatten een of meer [uiterlijk](#appearances)|.
seenDurationRatio|Aanwezigheid ten opzichte van de duur van de video (0-1).

## <a name="audioeffects"></a>audioEffects

Kenmerk | Beschrijving 
---|---
audioEffectKey| Geldige waarden zijn:-spraak, stilte, HandClaps.
vormgeving|Kan bevatten een of meer [uiterlijk](#appearances)
seenDurationRatio|Aanwezigheid ten opzichte van de duur van de video (0-1).
seenDuration|Voor hoe lang het audio-effect is aanwezig zijn (in seconden).

## <a name="appearances"></a>vormgeving

Kenmerk | Beschrijving 
---|---
startTime| Tijd-waarde.
endTime|Tijd-waarde.
startSeconds| Tijd-waarde.
endSeconds| Tijd-waarde.

## <a name="participants"></a>deelnemers

Kenmerk | Beschrijving 
---|---
id|De ID van de deelnemer kan.
naam|De naam van de deelnemer kan. Bijvoorbeeld, Sprekerherkenning #1.
Afbeeldings-URL|De **afbeeldings-URL** kenmerk is gereserveerd voor toekomstig gebruik.

## <a name="contentmoderation"></a>contentModeration

Kenmerk | Beschrijving 
---|---
adultClassifierValue|Het betrouwbaarheidsniveau dat de video inhoud voor volwassenen heeft.
racyClassifierValue|Het betrouwbaarheidsniveau dat de video ongepaste inhoud heeft.
bannedWordsCount|Het aantal woorden grof taalgebruik. 
bannedWordsRatio|Verhouding van grof taalgebruik woorden van het totale aantal woorden.
reviewRecommended|Booleaanse waarde waarmee wordt aangegeven als de video handmatig moet worden gecontroleerd.
isAdult|Booleaanse waarden waarmee wordt aangegeven als de video wordt beschouwd als een volwassene video na handmatig worden gecontroleerd.

## <a name="audioeffectscategories"></a>audioEffectsCategories

Kenmerk | Beschrijving 
---|---
type|ID van de categorie.
sleutel|Een van de volgende:-spraak, stilte, HandClaps. 

## <a name="transcriptblocks"></a>transcriptBlocks

Kenmerk | Beschrijving
---|---
id|ID van het blok.
regels|Kan bevatten een of meer [regels](#lines)
sentimentIds|De **sentimentIds** kenmerk is gereserveerd voor toekomstig gebruik.
thumbnailIds|De **thumbnailIds** kenmerk is gereserveerd voor toekomstig gebruik.
sentiment|Het gevoel in het blok (0-1, negatief zijn om te positief).
gezichten|Kan bevatten een of meer [gezichten](#faces).
optisch|Kan bevatten een of meer [optisch](#ocrs).
audioEffectInstances|Kan bevatten een of meer [audioEffectInstances](#audioEffectInstances).
schermen|Kan bevatten een of meer [schermen](#scenes).
aantekeningen|Kan nul of meer bevatten [aantekeningen](#annotations).

## <a name="ocrs"></a>optisch

Hierin wordt beschreven op welk moment in de video die de tekstinhoud is gevonden. 

Kenmerk | Beschrijving 
---|---
timeRange|Het tijdsbereik in de oorspronkelijke video.
adjustedTimeRange|AdjustedTimeRange is het tijdsbereik ten opzichte van de huidige afspeellijst. Omdat u een afspeellijst van verschillende regels van verschillende video's maken kunt, kunt u een één uur video ondernemen en slechts 1 regel van het, bijvoorbeeld, 10:00-10:15 gebruiken. In dat geval hebt u een afspeellijst met 1 regel, waar het tijdsbereik 10:00:15-10 is, maar de adjustedTimeRange is 00:00-00:15.
regels|Kan bevatten een of meer [regels](#lines).

## <a name="lines"></a>regels

### <a name="transcriptblocks"></a>transcriptBlocks

**regels** die worden weergegeven onder **transcriptBlocks**, regels Transcripten gevonden in de video wordt beschreven.

Kenmerk | Beschrijving 
---|---
id| De ID van de regel.
timeRange|Het tijdsbereik in de oorspronkelijke video.
adjustedTimeRange|AdjustedTimeRange is het tijdsbereik ten opzichte van de huidige afspeellijst. Omdat u een afspeellijst van verschillende regels van verschillende video's maken kunt, kunt u een één uur video ondernemen en slechts 1 regel van het, bijvoorbeeld, 10:00-10:15 gebruiken. In dat geval hebt u een afspeellijst met 1 regel, waar het tijdsbereik 10:00:15-10 is, maar de adjustedTimeRange is 00:00-00:15.
participantID| De id van de spreker analyseren van deze regel.
tekst| Het transcript.
isIncluded| In de basis uitsplitsingen altijd true. In afgeleide afspeellijsten, de regels die zijn opgenomen in de bron van de video en zijn ingesteld op isIncluded = true. Alle andere regels zijn ingesteld op false.

### <a name="ocrs"></a>optisch

**regels** die worden weergegeven onder **optisch**, regels optisch gevonden in de video wordt beschreven.

Kenmerk | Beschrijving 
---|---
id|De OCR-ID.
Breedte|Dit kenmerk wordt op dit moment niet gebruikt.
Hoogte|Dit kenmerk wordt op dit moment niet gebruikt.
Taal|De OCR-taal.
textData|De tekst.
vertrouwen|De betrouwbaarheidsscore (hoger is het beter).

## <a name="scenes"></a>schermen

Kenmerk | Beschrijving 
---|---
id|De id van de scène.
timeRange|Bevat een **timeRange**.
sleutelframes|De tijd van de belangrijkste frame.
foto 's|Kan bevatten een of meer [opnamen](#shots).

## <a name="shots"></a>foto 's

Kenmerk | Beschrijving 
---|---
id||Id van de schermopname.
timeRange|Bevat een **timeRange**.
sleutelframes|De tijd van de belangrijkste frame.

## <a name="audioeffectinstances"></a>audioEffectInstances

Kenmerk | Beschrijving 
---|---
type|De index van de audio gebeurtenis: voorspoed = 1, HandClaps = 2, muziek = 3, spraak = 4, stilte = 5
bereiken|Kan bevatten een of meer [bereiken](#ranges).

## <a name="ranges"></a>bereiken

**bereiken** die worden weergegeven onder **audioEffectInstances**, audio-effecten in deze bereiken worden beschreven.

Kenmerk | Beschrijving 
---|---
timeRange|Het tijdsbereik in de oorspronkelijke video.
adjustedTimeRange|AdjustedTimeRange is het tijdsbereik ten opzichte van de huidige afspeellijst. Omdat u een afspeellijst van verschillende regels van verschillende video's maken kunt, kunt u een één uur video ondernemen en slechts één regel code van het, bijvoorbeeld, 10:00-10:15. In dat geval hebt u een afspeellijst met 1 regel, waar het tijdsbereik 10:00:15-10 is, maar de adjustedTimeRange is 00:00-00:15.

## <a name="annotations"></a>aantekeningen

Retourneert tags op basis van herkenbare objecten, levende wezens, landschappen, acties en visuele patronen.

|Kenmerk|Beschrijving|
|---|---|
|id|De ID van de aantekening.|
|Naam|De naam van de aantekening (bijvoorbeeld persoon, sport game, zwart Frames).|
|Vormgeving|Kan een of meer weergaven bevatten.|

## <a name="brands"></a>merken

Bedrijfs- en merknamen gedetecteerd in de spraak-naar-tekst transcriptie en/of Video OCR. Dit omvat geen visual herkenning van merken of logo detectie.

Kenmerk | Beschrijving
---|---
id | De ID van een merk.
naam | De naam van het merk van Bing of een aangepaste merk.  
wikiId | Het achtervoegsel van het merk wikipedia-url. Bijvoorbeeld, "Target_Corporation" is het achtervoegsel van [ https://en.wikipedia.org/wiki/Target_Corporation ](https://en.wikipedia.org/wiki/Target_Corporation).
wikiUrl | Het merk de Wikipedia-url, indien aanwezig. Bijvoorbeeld [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
vertrouwen | De waarde van de betrouwbaarheid van de Video Indexer merk detector (0-1).
description | Beschrijving van het merk van Wikipedia hebt uitgepakt. 
vormgeving | Kan een of meer weergaven bevatten.
seenDuration | Aanwezigheid ten opzichte van de duur van de video (0-1).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het maken van uw eigen uitsplitsing [weergeven en bewerken van Video Indexer-inzichten](video-indexer-view-edit.md).

Zie voor meer informatie over het widgets insluiten in uw toepassing [widgets insluiten van Video Indexer in uw toepassingen](video-indexer-embed-widgets.md). 

## <a name="see-also"></a>Zie ook

[Video Indexer-API](https://videobreakdown.portal.azure-api.net/docs/services/582074fb0dc56116504aed75/operations/5857caeb0dc5610f9ce979e4)

[Video Indexer-overzicht](video-indexer-overview.md)

