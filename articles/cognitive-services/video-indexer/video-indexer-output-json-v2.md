---
title: Bekijk de Video-indexeerfunctie Azure-uitvoer geproduceerd door v2 API | Microsoft Docs
description: In dit onderwerp wordt de indexeerfunctie Video-uitvoer geproduceerd door v2 API gecontroleerd.
services: cognitive services
documentationcenter: ''
author: juliako
manager: cfowler
ms.service: cognitive-services
ms.topic: article
ms.date: 05/30/2018
ms.author: juliako
ms.openlocfilehash: 87301e4ce3c5b7db054b3dd86e8ee1ac5d90d3ca
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309057"
---
# <a name="examine-the-video-indexer-output-produced-by-v2-api"></a>Bekijk de Video indexeerfunctie uitvoer geproduceerd door v2 API

> [!Note]
> De Video indexeerfunctie V1 API's zijn gedeprecieerd en wordt verwijderd tijdens het 1e augustus 2018. U moet beginnen met de Video indexeerfunctie v2 API's om te voorkomen van onderbrekingen.
>
> Om te zetten met Video indexeerfunctie v2-API's, raadpleegt u de instructies [hier](https://api-portal.videoindexer.ai/). 

Als u aanroept de **Video-Index ophalen** API en de antwoordstatus in orde is, kunt u kunt een gedetailleerde JSON-uitvoer als de antwoordinhoud. De JSON-inhoud bevat details van de opgegeven video inzichten. De inzichten dimensies zoals bevatten: transcripties, optisch, vlakken, onderwerpen, blokken, enzovoort. De dimensies zijn exemplaren van tijd bereiken die worden weergegeven wanneer elke dimensie in de video komt.  

U kunt ook visueel samengevatte insights van de video bekijken door te drukken de **afspelen** knop op de video in de portal Video indexeerfunctie. Zie voor meer informatie [weergeven en bewerken video insights](video-indexer-view-edit.md).

![Inzichten](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

In dit artikel onderzoekt de JSON-inhoud die wordt geretourneerd door de **Video-Index ophalen** API. 

> [!NOTE]
> Vervaldatum van de toegangstokens in Video indexeerfunctie is een uur.


## <a name="root-elements"></a>Hoofdelementen

|Naam|Beschrijving|
|---|---|
|accountId|De afspeellijst VI account-ID.|
|id|De afspeellijst-ID.|
|naam|Naam van de afspeellijst.|
|description|Beschrijving van de afspeellijst.|
|Gebruikersnaam|De naam van de gebruiker die de afspeellijst wordt gemaakt.|
|gemaakt|Tijd voor het maken van de afspeellijst.|
|privacyMode|De afspeellijst privacymodus (persoonlijke/openbaar).|
|state|De afspeellijst (geüploade, verwerking, verwerkt, is mislukt, in quarantaine geplaatste).|
|isOwned|Hiermee wordt aangegeven of de afspeellijst is gemaakt door de huidige gebruiker.|
|isEditable|Hiermee wordt aangegeven of de huidige gebruiker is gemachtigd voor het bewerken van de afspeellijst.|
|isBase|Andere video's (afgeleid) of de afspeellijst een base afspeellijst (video) of een afspeellijst wordt bestaat.|
|durationInSeconds|De totale duur van de afspeellijst.|
|summarizedInsights|Bevat een [summarizedInsights](#summarizedinsights).
|video's|Een lijst met [video's](#videos) construeren van de afspeellijst.<br/>Als deze afspeellijst van gemaakt op basis van tijdsbereiken van andere video's (afgeleid), de video's in deze lijst bevat alleen gegevens uit de opgenomen tijdsbereik.|

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
|naam|De naam van de video. Bijvoorbeeld: Azure bewaken.|
|shortId|De ID van de video. Bijvoorbeeld: 63c6d532ff.|
|privacyMode|De verdeling kan een van de volgende modi hebben: **persoonlijke**, **openbare**. **Openbare** -de video voor iedereen in uw account en iedereen die een koppeling naar de video zichtbaar is. **Persoonlijke** -de video zichtbaar is voor iedereen in uw account.|
|duur|Bevat een duur die beschrijft de tijd die een beter inzicht is opgetreden. Er is een duur in seconden.|
|thumbnailUrl|De video miniatuur volledige URL. Bijvoorbeeld 'https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO... '. U ziet als de video persoonlijke, de URL een toegangstoken van één uur bevat. Na een uur, de URL niet langer geldig en moet u aan de uitsplitsing opnieuw met een nieuwe url in het ophalen of aanroepen GetAccessToken om een nieuw toegangstoken en de volledige url handmatig maken ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken [= accessToken]').|
|vlakken|Kan een of meer vlakken bevatten. Zie voor meer informatie [vlakken](#faces).|
|Onderwerpen|Kan een of meer onderwerpen bevatten. Zie voor meer informatie [onderwerpen](#topics).|
|Zie|Kan een of meer patronen bevatten. Zie voor meer informatie [patronen](#sentiments).|
|audioEffects| Kan een of meer audioEffects bevatten. Zie voor meer informatie [audioEffects](#audioeffects).|
|merken| Kan nul of meer merken bevatten. Zie voor meer informatie [merken](#brands).|
|Statistieken | Zie voor meer informatie [statistieken](#statistics).|

### <a name="statistics"></a>Statistieken

|Naam|Beschrijving|
|---|---|
|CorrespondenceCount|Het aantal berichten in de video.|
|WordCount|Het aantal woorden per spreker.|
|SpeakerNumberOfFragments|De hoeveelheid fragmenten de spreker heeft in een video.|
|SpeakerLongestMonolog|De spreker langste monolog. Als de spreker silences binnen de monolog is opgenomen. Stilte aan het begin- en het einde van de monolog wordt verwijderd.| 
|SpeakerTalkToListenRatio|De berekening is gebaseerd op de tijd die op de spreker monolog (zonder de stilte in tussen) gedeeld door de totale tijd van de video. De tijd wordt afgerond op het derde decimaalteken.|

## <a name="videos"></a>video's

|Naam|Beschrijving|
|---|---|
|accountId|De video VI account-ID.|
|id|De video-ID.|
|naam|De naam van de video.
|state|De status van de video (geüploade, verwerking, verwerkt, is mislukt, in quarantaine geplaatste).|
|processingProgress|De voortgang van de verwerking tijdens de verwerking (bijvoorbeeld 20%).|
|failureCode|De foutcode als proces (bijvoorbeeld ' UnsupportedFileType') is mislukt.|
|failureMessage|Het bericht is mislukt of kan niet worden verwerkt.|
|externalId|De video externe id (indien opgegeven door de gebruiker).|
|externalUrl|De video externe url (indien opgegeven door de gebruiker).|
|metagegevens|De video externe metagegevens (indien opgegeven door de gebruiker).|
|isAdult|Of de video is handmatig is gecontroleerd en geïdentificeerd als een volwassene video.|
|inzichten|Het object insights.|
|thumbnailUrl|De video miniatuur volledige URL. Bijvoorbeeld 'https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO... '. U ziet als de video persoonlijke, de URL een toegangstoken van één uur bevat. Na een uur, de URL niet langer geldig en moet u aan de uitsplitsing opnieuw met een nieuwe url in het ophalen of aanroepen GetAccessToken om een nieuw toegangstoken en de volledige url handmatig maken ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken [= accessToken]').|
|publishedUrl|Een url naar de video te streamen.|
|publishedUrlProxy|Een url om te streamen video van (voor Apple-apparaten).|
|viewToken|Een korte levensduur weergave token voor de video te streamen.|
|sourceLanguage|Taal van de bron van de video.|
|taal|Huidige taal van de video (NAT).|
|indexingPreset|De standaardinstelling gebruikt om te indexeren van de video.|
|streamingPreset|De standaardinstelling gebruikt voor het publiceren van de video.|
|linguisticModelId|Het model cri's is de video transcriberen gebruikt.|

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

De inzichten zijn een aantal dimensies (bijvoorbeeld de tekst van regels, vlakken, merken, enzovoort), waarbij elke dimensie een lijst met unieke elementen (bijvoorbeeld face1, face2, face3) en voor elk element heeft een eigen metagegevens en een lijst van de exemplaren (die zijn tijdsbereik met aanvullende optionele metagegevens).

Een gezicht wellicht een ID, een naam, een miniatuur, andere metagegevens en een lijst van de tijdelijke exemplaren (bijvoorbeeld: 00:00:05: 00:00:10, 00:01:00 - 00:02:30 en 00:41:21: 00:41:49.) Elk exemplaar van de tijdelijke kan aanvullende metagegevens hebben. Bijvoorbeeld: coördineert de face rechthoek (20,230,60,60).

|Versie|De versie van de|
|---|---|
|sourceLanguage|De video source-taal (uitgaande van een master language). In de vorm van een [BCP 47](https://tools.ietf.org/html/bcp47) tekenreeks.|
|taal|De insights-taal (vertaald van de taal van de bron). In de vorm van een [BCP 47](https://tools.ietf.org/html/bcp47) tekenreeks.|
|de tekst|De [verslag](#transcript) dimensie.|
|OCR|De [ocr](#ocr) dimensie.|
|Trefwoorden|De [trefwoorden](#keywords) dimensie.|
|vlakken|De [vlakken](#faces) dimensie.|
|Labels|De [labels](#labels) dimensie.|
|schermafbeeldingen|De [schermafbeeldingen](#shots) dimensie.|
|merken|De [merken](#brands) dimensie.|
|audioEffects|De [audioEffects](#audioEffects) dimensie.|
|Zie|De [patronen](#sentiments) dimensie.|
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

#### <a name="transcript"></a>de tekst

|Naam|Beschrijving|
|---|---|
|id|De regel-ID.|
|tekst|De tekst zelf.|
|taal|De taal van de tekst. Bedoeld ter ondersteuning van de tekst waarbij elke regel een andere taal kan hebben.|
|instanties|Een lijst met tijdsbereik waarop deze regel komt. Als de instantie de tekst is, wordt er slechts 1 exemplaar.|

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
|id|De OCR regel-ID.|
|tekst|De tekst.|
|vertrouwen|Het vertrouwen herkenning.|
|taal|De OCR-taal.|
|instanties|Een lijst met tijdsbereik waar deze OCR komt (de dezelfde OCR kan meerdere keren voorkomen).|

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

#### <a name="keywords"></a>Trefwoorden

|Naam|Beschrijving|
|---|---|
|id|De id sleutelwoord.|
|tekst|De tekst sleutelwoord.|
|vertrouwen|Het sleutelwoord erkenning vertrouwen.|
|taal|Het sleutelwoord-taal (wanneer vertaald).|
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

#### <a name="faces"></a>vlakken

|Naam|Beschrijving|
|---|---|
|id|De face-ID.|
|naam|De naam van pictogramtitel. 'Onbekende #0', een geïdentificeerde beroemdheden of een persoon met de klant kan zijn.|
|vertrouwen|Het vertrouwen van face-identificatie.|
|description|Als het een beroemdheden, de beschrijving kan zijn: ' Satya Nadella is geboren op... '. |
|thumbnalId|De id van de miniatuur van die gezicht.|
|knownPersonId|Als het een bekende persoon, de interne-ID.|
|ReferenceId|Als het een Bing beroemdheden, de Bing-ID.|
|referenceType|Op dit moment alleen Bing.|
|titel|Indien een beroemdheden, de titel (bijvoorbeeld ' Microsoft CEO').|
|imageUrl|Indien een beroemdheden, de afbeeldings-url.|
|instanties|Dit zijn exemplaren van waar de face in het bereik van de opgegeven tijd komt. Elk exemplaar heeft ook een thumbnailsId. |

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

#### <a name="labels"></a>Labels

|Naam|Beschrijving|
|---|---|
|id|De label-ID.|
|naam|De labelnaam (bijvoorbeeld 'Computer', 'TV').|
|taal|De taal met de label-naam (wanneer vertaald). BCP 47|
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

#### <a name="shots"></a>schermafbeeldingen

|Naam|Beschrijving|
|---|---|
|id|De schermopname-ID.|
|keyFrames|Een lijst met keyframes in beeld (elk met een ID en een lijst met instanties tijdsbereik). Keyframes exemplaren hebben een thumbnailId veld met de keyFrame miniatuur-id.|
|instanties|Een lijst met tijdsbereiken van deze geschoten (schermafbeeldingen hebben slechts 1 exemplaar).|

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

Bedrijfs- en merknamen gedetecteerd in de spraak naar de tekst van de tekst en/of Video OCR. Dit omvat geen visual herkenning van merken of logo detectie.

|Naam|Beschrijving|
|---|---|
|id|De merk-ID.|
|naam|De naam merken.|
|ReferenceId | Het achtervoegsel van de url merk-Wikipedia (Engelstalig). Bijvoorbeeld, "Target_Corporation" is het achtervoegsel van [ https://en.wikipedia.org/wiki/Target_Corporation ](https://en.wikipedia.org/wiki/Target_Corporation).
|referenceUrl | Het merk de url van de Wikipedia (Engelstalig), als bestaat. Bijvoorbeeld [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|description|De beschrijving merken.|
|tags|Een lijst met vooraf gedefinieerde codes die gekoppeld aan deze huisstijl zijn.|
|vertrouwen|De waarde van de betrouwbaarheid van de Video indexeerfunctie merk detectie (0-1).|
|instanties|Een lijst met tijdsbereiken van dit merk. Elk exemplaar heeft een brandType waarmee wordt aangegeven of deze huisstijl in de tekst van de of OCR weergegeven.|

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

#### <a name="audioeffects"></a>audioEffects

|Naam|Beschrijving|
|---|---|
|id|De audio-effect-ID.|
|type|Het type audio-effect (bijvoorbeeld klappen, spraak, stilte).|
|instanties|Een lijst met tijdsbereik waar deze audio-effect komt.|

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

#### <a name="sentiments"></a>Zie

Zie worden samengevoegd door hun sentimentType veld (neutrale-positieve/negatieve). Bijvoorbeeld, 0-0,1 0,1 0,2.

|Naam|Beschrijving|
|---|---|
|id|De gevoel-ID.|
|averageScore |Het gemiddelde van alle scores van alle exemplaren van dat type gevoel - neutrale-positieve/negatieve|
|instanties|Een lijst met tijdsbereik waar deze gevoel komt.|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
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

Het blok visualContentModeration bevat tijdsbereik dat Video indexeerfunctie om potentieel inhoud voor volwassenen gevonden. Als visualContentModeration leeg is, is er geen inhoud voor volwassenen die is geïdentificeerd.

Video's die zijn gevonden voor inhoud voor volwassenen of mooie mogelijk beschikbaar voor alleen persoonlijke weergave. Gebruikers hebben de optie voor het verzenden van een aanvraag voor een menselijke beoordeling van de inhoud in dat geval kan het kenmerk IsAdult het resultaat van menselijke Lees bevat.

|Naam|Beschrijving|
|---|---|
|id|De visuele inhoud toezicht-ID.|
|adultScore|De volwassenen score (van inhoud beheerder).|
|racyScore|De mooie score (van inhoud toezicht).|
|instanties|Een lijst met tijdsbereik waar deze visual inhoud toezicht werd weergegeven.|

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
|id|De controle van tekstuele inhoud-ID.|
|bannedWordsCount |Het aantal verboden woorden.|
|bannedWordsRatio |De verhouding van het totale aantal woorden.|


## <a name="next-steps"></a>Volgende stappen

[Video indexeerfunctie API](https://videobreakdown.portal.azure-api.net/docs/services/582074fb0dc56116504aed75/operations/5857caeb0dc5610f9ce979e4)

Zie voor meer informatie over het insluiten van widgets in uw toepassing [insluiten van Video indexeerfunctie widgets in uw toepassingen](video-indexer-embed-widgets.md). 

