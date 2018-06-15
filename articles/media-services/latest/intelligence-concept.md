---
title: Azure media intelligence | Microsoft Docs
description: Wanneer u Azure Media Services gebruikt, kunt u uw audio en video contnet met AudioAnalyzerPreset en VideoAnalyzerPreset analyseren.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/24/2018
ms.author: juliako
ms.openlocfilehash: 804a418f6ee88974d6e74a2c18bc5d01b6adf838
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788751"
---
# <a name="media-intelligence"></a>Mediagegevens

Azure Media Services REST v3-API kunt u audio en video-inhoud analyseren. Voor het analyseren van uw inhoud die u maakt een **transformeren** en het verzenden van een **taak** die gebruikmaakt van een van deze standaardinstellingen: **AudioAnalyzerPreset** of **VideoAnalyzerPreset** . 

## <a name="audioanalyzerpreset"></a>AudioAnalyzerPreset

**AudioAnalyzerPreset** kunt u meerdere audio insights extraheren uit een audio- of -bestand. De uitvoer bevat een JSON-bestand (met de inzichten) en VTT-bestand voor de tekst van de audio. Deze definitie accepteert een eigenschap die specificeert de taal van het bestand voor invoer in de vorm van een [BCP47](https://tools.ietf.org/html/bcp47) tekenreeks. De audio inzichten zijn onder andere:

* Audio schrijffouten – de tekst van de gesproken woorden met tijdstempels. Meerdere talen worden ondersteund
* Spreker indexeren – een toewijzing van de luidsprekers en de bijbehorende gesproken woorden
* Spraak gevoel analyse – de uitvoer van gevoel analyse wordt uitgevoerd op de audio schrijffouten
* Trefwoorden: trefwoorden die worden opgehaald uit de audio schrijffouten.

## <a name="videoanalyzerpreset"></a>VideoAnalyzerPreset

**VideoAnalyzerPreset** kunt u meerdere audio en video insights extraheren uit een videobestand. De uitvoer bevat een JSON-bestand (met de inzichten), een VTT-bestand voor de tekst van de video en een verzameling van miniatuurweergaven. Deze definitie accepteert ook een [BCP47](https://tools.ietf.org/html/bcp47) type string (de taal van de video) als een eigenschap. De video inzichten omvatten de audio inzichten bovengenoemde en de volgende extra onderdelen:

* Face volgen – de tijd gedurende welke vlakken aanwezig in de video zijn. Elk vlak heeft een face-id en een bijbehorende verzameling van miniatuurweergaven
* Visual tekst-de tekst die wordt gedetecteerd via OCR. De tekst is geautoriseerd en ook gebruikt voor het uitpakken van trefwoorden (naast de tekst van de audio)
* Keyframes: een verzameling van keyframes die worden opgehaald uit de video
* Visual inhoud toezicht – het gedeelte van de video's die zijn gemarkeerd als volwassene of mooie aard
* Aantekening – een resultaat van de video's op basis van een vooraf gedefinieerde objectmodel aantekeningen maken

##  <a name="insightsjson-elements"></a>Insights.JSON elementen

De uitvoer bevat een JSON-bestand (insights.json) met de inzichten die zijn gevonden in de video of audio. De json kan de volgende elementen bevatten:

### <a name="transcript"></a>de tekst

|Naam|Beschrijving|
|---|---|
|id|De regel-ID.|
|Tekst|De tekst zelf.|
|taal|De taal van de tekst. Bedoeld ter ondersteuning van de tekst waarbij elke regel een andere taal kan hebben.|
|instanties|Een lijst met tijdsbereik waarop deze regel komt. Als de instantie een van de tekst is, wordt er slechts 1 exemplaar.|

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

|Naam|Beschrijving|
|---|---|
|id|De OCR regel-id.|
|Tekst|De tekst.|
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

### <a name="keywords"></a>Trefwoorden

|Naam|Beschrijving|
|---|---|
|id|De id sleutelwoord.|
|Tekst|De tekst sleutelwoord.|
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

### <a name="faces"></a>vlakken

|Naam|Beschrijving|
|---|---|
|id|De face-id.|
|naam|De naam van pictogramtitel. 'Onbekende #0', een geïdentificeerde beroemdheden of een persoon met de klant kan zijn.|
|vertrouwen|Het vertrouwen van face-identificatie.|
|description|In geval van een beroemdheden, de beschrijving (' Satya Nadella is geboren op... '). |
|thumbnalId|De id van de miniatuur van die gezicht.|
|knownPersonId|In geval van een bekende persoon, de interne id.|
|ReferenceId|In geval van een beroemdheden Bing bijbehorende Bing-id.|
|referenceType|Op dit moment alleen Bing.|
|titel|In geval van een beroemdheden titel (bijvoorbeeld ' Microsoft CEO').|
|imageUrl|In geval van een beroemdheden de afbeeldings-url.|
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

### <a name="labels"></a>Labels

|Naam|Beschrijving|
|---|---|
|id|de label-id.|
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

### <a name="shots"></a>schermafbeeldingen

|Naam|Beschrijving|
|---|---|
|id|De schermopname-id.|
|keyFrames|Een lijst met keyframes in beeld (elk met een Id en een lijst met instanties tijdsbereik).|
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
              "start": "00: 00: 00.1670000",
              "end": "00: 00: 00.2000000"
            }
          ]
        }
      ],
      "instances": [
        {
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
              "start": "00: 00: 05.2670000",
              "end": "00: 00: 05.3000000"
            }
          ]
        }
      ],
      "instances": [
        {
          "start": "00: 00: 05.2670000",
          "end": "00: 00: 10.3000000"
        }
      ]
    }
  ]
```

### <a name="audioeffects"></a>audioEffects

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


### <a name="sentiments"></a>Zie

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

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: Analyseren video's met Azure Media Services](analyze-videos-tutorial-with-api.md)
