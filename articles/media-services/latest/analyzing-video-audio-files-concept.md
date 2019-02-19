---
title: Analyseren van de video en audio-bestanden met Azure Media Services | Microsoft Docs
description: Wanneer u Azure Media Services gebruikt, kunt u uw audio en video contnet met behulp van AudioAnalyzerPreset en VideoAnalyzerPreset kunt analyseren.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/13/2019
ms.author: juliako
ms.openlocfilehash: 247d72396d1737d568a89656c544bbe699f11e30
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56342385"
---
# <a name="analyzing-video-and-audio-files"></a>Video-en audiobestanden analyseren

Azure Media Services v3 kunt u voor het extraheren van inzichten uit uw video en audio bestanden met Video Indexer via AMS v3 analyzer voorinstellingen (beschreven in dit artikel). Als u meer gedetailleerde inzichten wilt, gebruikt u Video Indexer rechtstreeks. Als u het verschil wilt weten tussen het gebruik van instellingen van Video Indexer en Media Services, raadpleegt u het [vergelijkingsdocument](../video-indexer/compare-video-indexer-with-media-services-presets.md).

Voor het analyseren van uw inhoud met behulp van Media Services v3 voorinstellingen, maakt u een **transformeren** en het verzenden van een **taak** die gebruikmaakt van een van deze standaardinstellingen: [VideoAnalyzerPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#videoanalyzerpreset) of **AudioAnalyzerPreset**. Het volgende artikel ziet u hoe u **VideoAnalyzerPreset**: [Zelfstudie: Analyseren van video's met Azure Media Services](analyze-videos-tutorial-with-api.md).

> [!NOTE]
> Wanneer u voorinstellingen voor een Video of Audio Analyzer gebruikt, moet u de Azure-portal gebruiken om uw account in te stellen op 10 S3 Door media gereserveerde eenheden. Zie [Mediaverwerking schalen](../previous/media-services-scale-media-processing-overview.md) voor meer informatie.

## <a name="built-in-presets"></a>Ingebouwde voorinstellingen

Media Services ondersteunt momenteel de volgende ingebouwde analyzer voorinstellingen:  

|**Vooraf ingestelde naam**|**Scenario**|**Details**|
|---|---|---|
|**AudioAnalyzerPreset**|Audio analyseren|De definitie van een vooraf gedefinieerde set op basis van AI analysis-bewerkingen, met inbegrip van spraaktranscriptie toepassing. De vooraf ingestelde ondersteunt momenteel de verwerking van inhoud met een één audiotrack die gesproken tekst in één taal bevat. U kunt de taal voor de nettolading van de audio in de invoer met behulp van de BCP-47-indeling van 'taal tag-regio' opgeven. Ondersteunde talen zijn Engels ('en-US' en "en-GB"), Spaans ('es-ES' en 'es-MX'), Frans ('fr-FR'), Italiaans ('it-IT'), Japans ('ja-JP'), Portugees ('pt-BR'), Chinees ('zh-CN'), Duits ('nl-nl'), Arabisch ('ar-bijvoorbeeld'), Russisch ('ru-RU'), Hindi ("Hallo-IN' ), en Koreaans ('ko-KR').<br/><br/> Als de taal is niet opgegeven of is ingesteld op null-waarde, automatische taaldetectie wordt de eerste taal gedetecteerd kiezen en verwerken met de geselecteerde taal voor de duur van het bestand. De functie voor automatische taalherkenning ondersteunt momenteel het Engels, Chinees, Frans, Duits, Italiaans, Japans, Spaans, Russisch en Portugees. Het ondersteunt momenteel geen dynamisch schakelen tussen talen nadat de eerste taal die is gedetecteerd. De functie voor automatische taalherkenning werkt het beste met audio-opnamen met duidelijke spraak. Als automatische taaldetectie niet vinden van de taal, terugvallen de transcriptie naar het Engels.|
|**VideoAnalyzerPreset**|Analyse van audio en video|Inzichten (uitgebreide metagegevens) wordt geëxtraheerd uit de audio en video en voert een bestand met JSON-indeling. U kunt opgeven of u alleen audio inzicht wilt bij het verwerken van een videobestand. Zie voor meer informatie, [analyseren video](analyze-videos-tutorial-with-api.md).|

### <a name="audioanalyzerpreset"></a>AudioAnalyzerPreset

De vooraf gedefinieerde instellingen kunt u meerdere audio inzichten uit te extraheren uit een audio- of -bestand. De uitvoer bevat een JSON-bestand (met de inzichten) en een VTT-bestand voor de audiotranscript. Deze definitie accepteert een eigenschap die Hiermee geeft u de taal van het invoerbestand in de vorm van een [BCP47](https://tools.ietf.org/html/bcp47) tekenreeks. De audio inzichten zijn onder andere:

* Audiotranscriptie – een transcript van de gesproken woorden met tijdstempels. Meerdere talen worden ondersteund
* Sprekerherkenning indexeren – een toewijzing van de sprekers en de bijbehorende gesproken woorden
* Sentimentanalyse voor spraak: de uitvoer van sentimentanalyse uitgevoerd op de audiotranscriptie
* Trefwoorden: trefwoorden die worden opgehaald uit de audiotranscriptie.

### <a name="videoanalyzerpreset"></a>VideoAnalyzerPreset

De vooraf gedefinieerde instellingen kunt u meerdere audio en video-inzichten ophalen uit een videobestand. De uitvoer bevat een JSON-bestand (met de inzichten), een VTT-bestand voor de videotranscriptie en een verzameling van miniatuurweergaven. Deze definitie accepteert ook een [BCP47](https://tools.ietf.org/html/bcp47) tekenreeks (die vertegenwoordigt de taal van de video) als een eigenschap. De inzichten in video's zijn onder andere de audio inzichten die hierboven worden vermeld en in de volgende extra items:

* Face volgen – de tijd gedurende welke gezichten aanwezig in de video zijn. Elk gezicht heeft een gezichts-id en bijbehorende verzameling van miniatuurweergaven
* Visual tekst-de tekst die via optische tekenherkenning wordt gedetecteerd. De tekst is het tijd stempel en ook worden gebruikt om op te halen van trefwoorden (naast de audiotranscript)
* Hoofdframes: een verzameling van hoofdframes die worden opgehaald uit de video
* Visual inhoudstoezicht – het gedeelte van de video's die zijn gemarkeerd als volwassene of ongepaste in de natuur
* Aantekening – een resultaat van de video's op basis van een vooraf gedefinieerde objectmodel aantekeningen te maken

##  <a name="insightsjson-elements"></a>insights.json elements

De uitvoer bevat een JSON-bestand (insights.json) met de inzichten die zijn gevonden in de video of audio. De json, kan de volgende elementen bevatten:

### <a name="transcript"></a>transcript

|Name|Description|
|---|---|
|id|De regel-ID.|
|tekst|Het transcript zelf.|
|language|De transcript-taal. Bedoeld ter ondersteuning van transcript waarbij elke regel een andere taal kan hebben.|
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

### <a name="ocr"></a>OCR

|Name|Description|
|---|---|
|id|De OCR-regel-ID.|
|tekst|De tekst.|
|vertrouwen|De opname-vertrouwen.|
|language|De OCR-taal.|
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

### <a name="faces"></a>faces

|Name|Description|
|---|---|
|id|De face-ID.|
|naam|De naam van pictogramtitel. 'Onbekende #0', een geïdentificeerde beroemdheden of een persoon met de klant kan zijn.|
|vertrouwen|De face id vertrouwen.|
|description|Een beschrijving van de beroemdheden. |
|thumbnailId|De ID van de miniatuur van die gezicht.|
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

### <a name="shots"></a>foto 's

|Name|Description|
|---|---|
|id|Id van de schermopname.|
|keyFrames|Een lijst met belangrijke frames in beeld (elk heeft een ID en een lijst met instanties tijdsbereik). Belangrijkste frames-exemplaren beschikken over een thumbnailId veld met de miniatuur van het sleutelframes-id.|
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

### <a name="statistics"></a>statistieken

|Name|Description|
|---|---|
|CorrespondenceCount|Het aantal berichten in de video.|
|WordCount|Het aantal woorden per spreker.|
|SpeakerNumberOfFragments|De hoeveelheid van de spreker in een video heeft fragmenten.|
|SpeakerLongestMonolog|Van de spreker langste monolog. Als de spreker silences binnen de monolog is opgenomen. Stilte aan het begin en het einde van de monolog wordt verwijderd.| 
|SpeakerTalkToListenRatio|De berekening is gebaseerd op de tijd die op van de spreker monolog (zonder de stilte in tussen) gedeeld door de totale tijd van de video. De tijd wordt afgerond op de derde decimaalteken.|


### <a name="sentiments"></a>sentimenten

Sentimenten worden samengevoegd door hun sentimentType veld (positieve/neutraal/negatieve). Bijvoorbeeld, 0-0.1, 0.1 0.2.

|Name|Description|
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

### <a name="labels"></a>labels

|Name|Description|
|---|---|
|id|De label-ID.|
|naam|De labelnaam (bijvoorbeeld 'Computer', 'Tv-programma's ').|
|language|De taal met de label-naam (wanneer het wordt omgezet). BCP-47|
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

### <a name="keywords"></a>trefwoorden

|Name|Description|
|---|---|
|id|Het sleutelwoord-ID.|
|tekst|De tekst trefwoord.|
|vertrouwen|Van het sleutelwoord erkenning vertrouwen.|
|language|Het sleutelwoord-taal (wanneer het wordt omgezet).|
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

#### <a name="visualcontentmoderation"></a>visualContentModeration

Het blok visualContentModeration bevat tijdsbereik die Video Indexer hebben mogelijk inhoud voor volwassenen gevonden. Als visualContentModeration leeg is, is er geen inhoud voor volwassenen die is geïdentificeerd.

Video's die zijn gevonden voor volwassenen of ongepaste inhoud mogelijk beschikbaar voor alleen persoonlijke weergave. Gebruikers hebben de optie voor het verzenden van een aanvraag voor een menselijke beoordeling van de inhoud in dit geval kan het kenmerk IsAdult het resultaat van de menselijke beoordeling bevat.

|Name|Description|
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
## <a name="next-steps"></a>Volgende stappen

[Zelfstudie: Analyseren van video's met Azure Media Services](analyze-videos-tutorial-with-api.md)
