---
title: Bekijk de Azure-Video Indexer-uitvoer geproduceerd door de v2-API | Microsoft Docs
description: In dit onderwerp wordt de Video Indexer-uitvoer geproduceerd door de v2-API gecontroleerd.
services: cognitive services
documentationcenter: ''
author: juliako
manager: cfowler
ms.service: cognitive-services
ms.topic: article
ms.date: 05/30/2018
ms.author: juliako
ms.openlocfilehash: 8b32b241c4122893bb07993402a22d2223053f3d
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39115174"
---
# <a name="examine-the-video-indexer-output-produced-by-v2-api"></a>Bekijk de Video Indexer-uitvoer geproduceerd door de v2-API

> [!Note]
> De Video Indexer V1 API's zijn afgeschaft en op 1 augustus 2018 wordt verwijderd. U moet beginnen met behulp van de Video Indexer v2-API's om storingen te voorkomen.
>
> Als u wilt ontwikkelen met Video Indexer v2-API's, Zie de instructies [hier](https://api-portal.videoindexer.ai/). 

Wanneer u aanroepen de **Video-Index ophalen** API en de status van het antwoord is OK, kunt u een gedetailleerde JSON-uitvoer als de inhoud van de reactie. De JSON-inhoud bevat details van de opgegeven inzichten in video's. De inzichten dimensies zoals bevatten: transcripties, optisch, gezichten, onderwerpen, blokken, enzovoort. De afmetingen zijn instanties van tijd bereiken die worden weergegeven wanneer elke dimensie wordt weergegeven in de video.  

U kunt ook visueel overzicht van de video-inzichten bekijken door te drukken de **afspelen** knop op de video in de Video Indexer-portal. Zie voor meer informatie, [weergeven en bewerken inzichten in video's](video-indexer-view-edit.md).

![Inzichten](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

In dit artikel onderzoekt de JSON-inhoud die wordt geretourneerd door de **Video-Index ophalen** API. 

> [!NOTE]
> Vervaldatum van de toegangstokens in Video Indexer is één uur.


## <a name="root-elements"></a>Basis-elementen

|Naam|Beschrijving|
|---|---|
|accountId|Van de afspeellijst VI account-ID.|
|id|Van de afspeellijst-ID.|
|naam|De naam van de afspeellijst.|
|description|Beschrijving van de afspeellijst.|
|Gebruikersnaam|De naam van de gebruiker die de afspeellijst gemaakt.|
|gemaakt|Aanmaaktijd van de afspeellijst.|
|privacyMode|Van de afspeellijst privacymodus (privé/openbaar).|
|state|Van de afspeellijst (geüploade, verwerking, verwerkt, is mislukt, de in quarantaine geplaatste).|
|isOwned|Geeft aan of de afspeellijst is gemaakt door de huidige gebruiker.|
|isEditable|Geeft aan of de huidige gebruiker is gemachtigd op de afspeellijst bewerken.|
|isBase|Geeft aan of de afspeellijst een base afspeellijst (video is) of een afspeellijst van andere video's (afgeleid) gemaakt.|
|durationInSeconds|De totale duur van de afspeellijst.|
|summarizedInsights|Bevat een [summarizedInsights](#summarizedinsights).
|video's|Een lijst met [video's](#videos) de afspeellijst maken.<br/>Als deze afspeellijst met samengesteld uit tijdsbereiken van andere video's (afgeleid), de video's in deze lijst bevat alleen gegevens uit de opgenomen tijdsbereik.|

```json
{
  "accountId": "bca61527-1221-bca6-1527-a90000002000",
  "id": "abc3454321",
  "name": "My first video",
  "description": "I am trying VI",
  "userName": "Some name",
  "created": "2018/2/2 18:00:00.000",
  "privacyMode": "Private",
  "state": "Processed",
  "isOwned": true,
  "isEditable": false,
  "isBase": false,
  "durationInSeconds": 120, 
  "summarizedInsights" : { . . . }
  "videos": [{ . . . }]
}
```

## <a name="summarizedinsights"></a>summarizedInsights

In deze sectie bevat een overzicht van de inzichten.

|Kenmerk | Beschrijving|
|---|---|
|naam|De naam van de video. Bijvoorbeeld: Azure Monitor.|
|shortId|De ID van de video. Bijvoorbeeld: 63c6d532ff.|
|privacyMode|Uitsplitsing van uw kan hebben een van de volgende modi: **persoonlijke**, **openbare**. **Openbare** -de video is zichtbaar voor iedereen in uw account en iedereen met een koppeling naar de video. **Persoonlijke** -de video is zichtbaar voor iedereen in uw account.|
|duur|Bevat een duur van de beschrijving van de tijd die een inzicht is opgetreden. Er is een duur in seconden.|
|thumbnailUrl|Miniatuur van de video volledige URL. Bijvoorbeeld, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO... '. U ziet als de video privé is, de URL van een toegangstoken van één uur bevat. Na een uur, de URL niet langer geldig en moet u naar de uitsplitsing van de opnieuw met een nieuwe url in het ophalen of aanroepen GetAccessToken om een nieuw toegangstoken en de volledige url handmatig maken ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [ accessToken]').|
|gezichten|Kan nul of meer gezichten bevatten. Zie voor meer informatie, [gezichten](#faces).|
|trefwoorden|Kan nul of meer trefwoorden bevatten. Zie voor meer informatie, [trefwoorden](#keywords).|
|sentimenten|Kan nul of meer sentimenten bevatten. Zie voor meer informatie, [sentimenten](#sentiments).|
|audioEffects| Kan nul of meer audioEffects bevatten. Zie voor meer informatie, [audioEffects](#audioeffects).|
|labels| Kan nul of meer labels bevatten. Zie voor meer informatie, [labels](#labels).|
|merken| Kan nul of meer merken bevatten. Zie voor meer informatie, [merken](#brands).|
|statistieken | Zie voor meer informatie, [statistieken](#statistics).|

## <a name="videos"></a>video's

|Naam|Beschrijving|
|---|---|
|accountId|Van de video VI account-ID.|
|id|Van de video-ID.|
|naam|De naam van de video.
|state|Status van de video (geüploade, verwerking, verwerkt, is mislukt, de in quarantaine geplaatste).|
|processingProgress|De voortgang tijdens de verwerking (bijvoorbeeld 20%).|
|failureCode|De foutcode als proces (bijvoorbeeld ' UnsupportedFileType') is mislukt.|
|failureMessage|Het foutbericht als kan niet worden verwerkt.|
|externalId|De video van de externe ID (indien opgegeven door de gebruiker).|
|externalUrl|Van de video externe url (indien opgegeven door de gebruiker).|
|metagegevens|De metagegevens van video's externe (indien opgegeven door de gebruiker).|
|isAdult|Geeft aan of er handmatig de video is bekeken en geïdentificeerd als een volwassene video.|
|inzichten|De insights-object. Zie voor meer informatie, [insights](#insights).|
|thumbnailUrl|Miniatuur van de video volledige URL. Bijvoorbeeld, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO... '. U ziet als de video privé is, de URL van een toegangstoken van één uur bevat. Na een uur, de URL niet langer geldig en moet u naar de uitsplitsing van de opnieuw met een nieuwe url in het ophalen of aanroepen GetAccessToken om een nieuw toegangstoken en de volledige url handmatig maken ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [ accessToken]').|
|publishedUrl|Een url naar de video te streamen.|
|publishedUrlProxy|Een url naar het streamen van video van (voor Apple-apparaten).|
|viewToken|Een korte levensduur hebben weergave-token voor het streamen van de video.|
|sourceLanguage|De brontaal van de video.|
|Taal|Huidige taal van de video (NAT).|
|indexingPreset|De definitie die wordt gebruikt om te indexeren van de video.|
|streamingPreset|De vooraf gedefinieerde instellingen gebruikt voor het publiceren van de video.|
|linguisticModelId|Het CRI model gebruikt de video te transcriberen.|
|statistieken | Zie voor meer informatie, [statistieken](#statistics).|

```json
{
    "videos": [{
        "accountId": "2cbbed36-1972-4506-9bc7-55367912df2d",
        "id": "142a356aa6",
        "state": "Processed",
        "privacyMode": "Private",
        "processingProgress": "100%",
        "failureCode": "General",
        "failureMessage": "",
        "externalId": null,
        "externalUrl": null,
        "metadata": null,
        "insights": {. . . },
        "thumbnailId": "89d7192c-1dab-4377-9872-473eac723845",
        "publishedUrl": "https://videvmediaservices.streaming.mediaservices.windows.net:443/d88a652d-334b-4a66-a294-3826402100cd/Xamarine.ism/manifest",
        "publishedProxyUrl": null,
        "viewToken": "Bearer=<token>",
        "sourceLanguage": "En-US",
        "language": "En-US",
        "indexingPreset": "Default",
        "linguisticModelId": "00000000-0000-0000-0000-000000000000"
    }],
}
```
### <a name="insights"></a>inzichten

De inzichten zijn een set van dimensies (bijvoorbeeld regels tekst, gezichten, merken, enzovoort), waarbij elke dimensie een lijst met unieke elementen (bijvoorbeeld face1, face2, face3), en elk element heeft een eigen metagegevens en een lijst van de exemplaren (die zijn tijd van reeksen met aanvullende optionele metagegevens).

Een gezicht mogelijk een ID, een naam, een miniatuur, andere metagegevens en een lijst van de tijdelijke exemplaren (bijvoorbeeld: 00:00:05: 00:00:10, 00:01:00 - 00:02:30 en 00:41:21: 00:41:49.) Elke tijdelijke instantie kan aanvullende metagegevens hebben. Bijvoorbeeld, coördinaten van het gezichtsrechthoek (20,230,60,60).

|Versie|De codeversie|
|---|---|
|sourceLanguage|Van de video source-taal (ervan uitgaande dat één master taal). In de vorm van een [BCP-47](https://tools.ietf.org/html/bcp47) tekenreeks.|
|Taal|De insights-taal (vertaald uit de source-taal). In de vorm van een [BCP-47](https://tools.ietf.org/html/bcp47) tekenreeks.|
|transcript|De [transcript](#transcript) dimensie.|
|OCR|De [ocr](#ocr) dimensie.|
|trefwoorden|De [trefwoorden](#keywords) dimensie.|
|blokken|Kan bevatten een of meer [blokken](#blocks)|
|gezichten|De [gezichten](#faces) dimensie.|
|labels|De [labels](#labels) dimensie.|
|foto 's|De [opnamen](#shots) dimensie.|
|merken|De [merken](#brands) dimensie.|
|audioEffects|De [audioEffects](#audioEffects) dimensie.|
|sentimenten|De [sentimenten](#sentiments) dimensie.|
|visualContentModeration|De [visualContentModeration](#visualcontentmoderation) dimensie.|
|textualConentModeration|De [textualConentModeration](#textualconentmoderation) dimensie.|

Voorbeeld:

```json
{
  "version": "0.9.0.0",
  "sourceLanguage": "en-US",
  "language": "es-ES",
  "transcript": ...,
  "ocr": ...,
  "keywords": ...,
  "faces": ...,
  "labels": ...,
  "shots": ...,
  "brands": ...,
  "audioEffects": ...,
  "sentiments": ...,
  "visualContentModeration": ...,
  "textualConentModeration": ...
}
```

#### <a name="blocks"></a>blokken

Kenmerk | Beschrijving
---|---
id|ID van het blok.|
instanties|Een lijst met tijdsbereiken van dit blok.|

#### <a name="transcript"></a>transcript

|Naam|Beschrijving|
|---|---|
|id|De regel-ID.|
|tekst|Het transcript zelf.|
|Taal|De transcript-taal. Bedoeld ter ondersteuning van transcript waarbij elke regel een andere taal kan hebben.|
|instanties|Een lijst met bereiken van tijd waarop deze regel wordt weergegeven. Als de instantie transcript is, is er slechts 1 exemplaar.|

Voorbeeld:

```json
"transcript": [
{
    "id": 0,
    "text": "Hi I'm Doug from office.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    }
    ]
},
{
    "id": 1,
    "text": "I have a guest. It's Michelle.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:02.7200000",
        "end": "00:00:03.9600000"
    }
    ]
}
] 
```

#### <a name="ocr"></a>OCR

|Naam|Beschrijving|
|---|---|
|id|De OCR-regel-ID.|
|tekst|De tekst.|
|vertrouwen|De opname-vertrouwen.|
|Taal|De OCR-taal.|
|instanties|Een lijst met tijdsbereik waar deze OCR weergegeven (de dezelfde OCR kan meerdere keren voorkomen).|

```json
"ocr": [
    {
      "id": 0,
      "text": "LIVE FROM NEW YORK",
      "confidence": 0.91,
      "language": "en-US",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:52"
        }
      ]
    },
    {
      "id": 1,
      "text": "NOTICIAS EN VIVO",
      "confidence": 0.9,
      "language": "es-ES",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:28"
        },
        {
          "start": "00:00:32",
          "end": "00:00:38"
        }
      ]
    }
  ],
```

#### <a name="keywords"></a>trefwoorden

|Naam|Beschrijving|
|---|---|
|id|Het sleutelwoord-ID.|
|tekst|De tekst trefwoord.|
|vertrouwen|Van het sleutelwoord erkenning vertrouwen.|
|Taal|Het sleutelwoord-taal (wanneer het wordt omgezet).|
|instanties|Een lijst met tijdsbereik waar dit sleutelwoord wordt weergegeven (een trefwoord kan meerdere keren voorkomen).|

```json
"keywords": [
{
    "id": 0,
    "text": "office",
    "confidence": 1.6666666666666667,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    },
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    }
    ]
},
{
    "id": 1,
    "text": "icons",
    "confidence": 1.4,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    },
    {
        "start": "00:00:13.9900000",
        "end": "00:00:15.6100000"
    }
    ]
}
] 

```

#### <a name="faces"></a>gezichten

|Naam|Beschrijving|
|---|---|
|id|De face-ID.|
|naam|De naam van pictogramtitel. 'Onbekende #0', een geïdentificeerde beroemdheden of een persoon met de klant kan zijn.|
|vertrouwen|De face id vertrouwen.|
|description|Een beschrijving van de beroemdheden. |
|thumbnalId|De ID van de miniatuur van die gezicht.|
|knownPersonId|Als dit een bekende persoon, in de interne-ID is.|
|referenceId|Als het een Bing beroemdheden, het Bing-ID.|
|referenceType|Op dit moment alleen Bing.|
|titel|Als het een beroemdheden, de titel "(bijvoorbeeld van Microsoft CEO).|
|imageUrl|Als het een beroemdheden, de afbeeldings-url.|
|instanties|Dit zijn exemplaren van waar het gezicht wordt weergegeven in de opgegeven periode. Elk exemplaar heeft ook een thumbnailsId. |

```json
"faces": [{
    "id": 2002,
    "name": "Xam 007",
    "confidence": 0.93844,
    "description": null,
    "thumbnailId": "00000000-aee4-4be2-a4d5-d01817c07955",
    "knownPersonId": "8340004b-5cf5-4611-9cc4-3b13cca10634",
    "referenceId": null,
    "title": null,
    "imageUrl": null,
    "instances": [{
        "thumbnailsIds": ["00000000-9f68-4bb2-ab27-3b4d9f2d998e",
        "cef03f24-b0c7-4145-94d4-a84f81bb588c"],
        "adjustedStart": "00:00:07.2400000",
        "adjustedEnd": "00:00:45.6780000",
        "start": "00:00:07.2400000",
        "end": "00:00:45.6780000"
    },
    {
        "thumbnailsIds": ["00000000-51e5-4260-91a5-890fa05c68b0"],
        "adjustedStart": "00:10:23.9570000",
        "adjustedEnd": "00:10:39.2390000",
        "start": "00:10:23.9570000",
        "end": "00:10:39.2390000"
    }]
}]
```

#### <a name="labels"></a>labels

|Naam|Beschrijving|
|---|---|
|id|De label-ID.|
|naam|De labelnaam (bijvoorbeeld 'Computer', 'Tv-programma's ').|
|Taal|De taal met de label-naam (wanneer het wordt omgezet). BCP-47|
|instanties|Een lijst met tijdsbereik waar dit label wordt weergegeven (een label kan meerdere keren voorkomen). Elk exemplaar heeft een veld vertrouwen. |


```json
"labels": [
    {
      "id": 0,
      "name": "person",
      "language": "en-US",
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 00.0000000",
          "end": "00: 00: 25.6000000"
        },
        {
          "confidence": 1.0,
          "start": "00: 01: 33.8670000",
          "end": "00: 01: 39.2000000"
        }
      ]
    },
    {
      "name": "indoor",
      "language": "en-US",
      "id": 1,
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 06.4000000",
          "end": "00: 00: 07.4670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 09.6000000",
          "end": "00: 00: 10.6670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 11.7330000",
          "end": "00: 00: 20.2670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 21.3330000",
          "end": "00: 00: 25.6000000"
        }
      ]
    }
  ] 
```

#### <a name="shots"></a>foto 's

|Naam|Beschrijving|
|---|---|
|id|Id van de schermopname.|
|Hoofdframes|Een lijst met belangrijke frames in beeld (elk heeft een ID en een lijst met instanties tijdsbereik). Belangrijkste frames-exemplaren beschikken over een thumbnailId veld met de miniatuur van het sleutelframes-id.|
|instanties|Een lijst met tijdsbereiken van deze schermopname (opnamen hebben slechts 1 exemplaar).|

```json
"Shots": [
    {
      "id": 0,
      "keyFrames": [
        {
          "id": 0,
          "instances": [
            {
          "thumbnailId": "00000000-0000-0000-0000-000000000000",
              "start": "00: 00: 00.1670000",
              "end": "00: 00: 00.2000000"
            }
          ]
        }
      ],
      "instances": [
        {
       "thumbnailId": "00000000-0000-0000-0000-000000000000",   
          "start": "00: 00: 00.2000000",
          "end": "00: 00: 05.0330000"
        }
      ]
    },
    {
      "id": 1,
      "keyFrames": [
        {
          "id": 1,
          "instances": [
            {
          "thumbnailId": "00000000-0000-0000-0000-000000000000",        
              "start": "00: 00: 05.2670000",
              "end": "00: 00: 05.3000000"
            }
          ]
        }
      ],
      "instances": [
        {
      "thumbnailId": "00000000-0000-0000-0000-000000000000",
          "start": "00: 00: 05.2670000",
          "end": "00: 00: 10.3000000"
        }
      ]
    }
  ]
```

#### <a name="brands"></a>merken

Bedrijfs- en merknamen gedetecteerd in de spraak-naar-tekst transcriptie en/of Video OCR. Dit omvat geen visual herkenning van merken of logo detectie.

|Naam|Beschrijving|
|---|---|
|id|De merk-ID.|
|naam|De naam van merken.|
|referenceId | Het achtervoegsel van het merk wikipedia-url. Bijvoorbeeld, "Target_Corporation" is het achtervoegsel van [ https://en.wikipedia.org/wiki/Target_Corporation ](https://en.wikipedia.org/wiki/Target_Corporation).
|referenceUrl | Het merk de Wikipedia-url, indien aanwezig. Bijvoorbeeld [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|description|De beschrijving van merken.|
|tags|Een lijst met vooraf gedefinieerde tags die gekoppeld aan deze merk zijn.|
|vertrouwen|De waarde van de betrouwbaarheid van de Video Indexer merk detector (0-1).|
|instanties|Een lijst met tijdsbereiken van deze merk. Elk exemplaar heeft een brandType, waarmee wordt aangegeven of deze huisstijl in het transcript of OCR weergegeven.|

```json
"brands": [
{
    "id": 0,
    "name": "MicrosoftExcel",
    "referenceId": "Microsoft_Excel",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft_Excel",
    "referenceType": "Wiki",
    "description": "Microsoft Excel is a sprea..",
    "tags": [],
    "confidence": 0.975,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 00: 31.3000000",
        "end": "00: 00: 39.0600000"
    }
    ]
},
{
    "id": 1,
    "name": "Microsoft",
    "referenceId": "Microsoft",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft",
    "description": "Microsoft Corporation is...",
    "tags": [
    "competitors",
    "technology"
    ],
    "confidence": 1.0,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 01: 44",
        "end": "00: 01: 45.3670000"
    },
    {
        "brandType": "Ocr",
        "start": "00: 01: 54",
        "end": "00: 02: 45.3670000"
    }
    ]
}
]
```

#### <a name="statistics"></a>statistieken

|Naam|Beschrijving|
|---|---|
|CorrespondenceCount|Het aantal berichten in de video.|
|WordCount|Het aantal woorden per spreker.|
|SpeakerNumberOfFragments|De hoeveelheid van de spreker in een video heeft fragmenten.|
|SpeakerLongestMonolog|Van de spreker langste monolog. Als de spreker silences binnen de monolog is opgenomen. Stilte aan het begin en het einde van de monolog wordt verwijderd.| 
|SpeakerTalkToListenRatio|De berekening is gebaseerd op de tijd die op van de spreker monolog (zonder de stilte in tussen) gedeeld door de totale tijd van de video. De tijd wordt afgerond op de derde decimaalteken.|

#### <a name="audioeffects"></a>audioEffects

|Naam|Beschrijving|
|---|---|
|id|De id van de audio-effect.|
|type|Het type audio-effect (bijvoorbeeld klappen, spraak, stilte).|
|instanties|Een lijst met tijdsbereik waar deze audio-effect werden weergegeven.|

```json
"audioEffects": [
{
    "id": 0,
    "type": "Clapping",
    "instances": [
    {
        "start": "00:00:00",
        "end": "00:00:03"
    },
    {
        "start": "00:01:13",
        "end": "00:01:21"
    }
    ]
}
]
```

#### <a name="sentiments"></a>sentimenten

Sentimenten worden samengevoegd door hun sentimentType veld (positieve/neutraal/negatieve). Bijvoorbeeld, 0-0.1, 0.1 0.2.

|Naam|Beschrijving|
|---|---|
|id|De id van de gevoelsscore.|
|averageScore |Het gemiddelde van alle scores van alle exemplaren van dat type sentiment - positieve/neutraal/negatieve|
|instanties|Een lijst met tijdsbereik waar deze sentiment werd weergegeven.|
|sentimentType |Het type kan worden 'Positief', 'Neutrale' of 'Negatieve'.|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:13",
        "end": "00:00:21"
    }
    ]
}
]
```

#### <a name="visualcontentmoderation"></a>visualContentModeration

Het blok visualContentModeration bevat tijdsbereik die Video Indexer hebben mogelijk inhoud voor volwassenen gevonden. Als visualContentModeration leeg is, is er geen inhoud voor volwassenen die is geïdentificeerd.

Video's die zijn gevonden voor volwassenen of ongepaste inhoud mogelijk beschikbaar voor alleen persoonlijke weergave. Gebruikers hebben de optie voor het verzenden van een aanvraag voor een menselijke beoordeling van de inhoud in dit geval kan het kenmerk IsAdult het resultaat van de menselijke beoordeling bevat.

|Naam|Beschrijving|
|---|---|
|id|De id van de visual inhoudstoezicht.|
|adultScore|De erotiekscore (van content moderator).|
|racyScore|De erotiekscore (van inhoudstoezicht).|
|instanties|Een lijst met tijdsbereik waar deze visual inhoudstoezicht werd weergegeven.|

```json
"VisualContentModeration": [
{
    "id": 0,
    "adultScore": 0.00069,
    "racyScore": 0.91129,
    "instances": [
    {
        "start": "00:00:25.4840000",
        "end": "00:00:25.5260000"
    }
    ]
},
{
    "id": 1,
    "adultScore": 0.99231,
    "racyScore": 0.99912,
    "instances": [
    {
        "start": "00:00:35.5360000",
        "end": "00:00:35.5780000"
    }
    ]
}
] 
```

#### <a name="textualconentmoderation"></a>textualConentModeration 

|Naam|Beschrijving|
|---|---|
|id|De tekstuele inhoudstoezicht-ID.|
|bannedWordsCount |Het nummer van verboden woorden.|
|bannedWordsRatio |De verhouding van het totale aantal woorden.|


## <a name="next-steps"></a>Volgende stappen

[Video Indexer-API](https://videobreakdown.portal.azure-api.net/docs/services/582074fb0dc56116504aed75/operations/5857caeb0dc5610f9ce979e4)

Zie voor meer informatie over het widgets insluiten in uw toepassing [widgets insluiten van Video Indexer in uw toepassingen](video-indexer-embed-widgets.md). 

