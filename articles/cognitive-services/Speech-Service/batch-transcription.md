---
title: Het gebruik van Batch Transcriptie - spraakservices
titlesuffix: Azure Cognitive Services
description: Batch transcriptie is ideaal als u wilt dat een grote hoeveelheid audio in opslag, zoals Azure Blobs transcriberen. U kunt met behulp van de toegewezen REST-API, wijst u audio-bestanden met een shared access signature (SAS) URI en asynchroon ontvangen transcripties.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: panosper
ms.openlocfilehash: b71400c3ae3c1cc6737d9194b4d94bf0b9c7efa9
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606748"
---
# <a name="why-use-batch-transcription"></a>Waarom Batch transcriptie gebruiken?

Batch transcriptie is ideaal als u wilt dat een grote hoeveelheid audio in opslag, zoals Azure Blobs transcriberen. U kunt met behulp van de toegewezen REST-API, wijst u audio-bestanden met een shared access signature (SAS) URI en asynchroon ontvangen transcripties.

## <a name="prerequisites"></a>Vereisten

### <a name="subscription-key"></a>Abonnementssleutel

Als met alle functies van de spraak-service die u een abonnementssleutel van maakt de [Azure-portal](https://portal.azure.com) door onze [introductiehandleiding](get-started.md). Als u van plan bent om op te halen transcripties van onze basislijn-modellen, is het maken van een sleutel alles die wat u nodig om te doen.

>[!NOTE]
> Een standard-abonnement (S0) voor Speech Services is vereist voor het gebruik van batch transcriptie. Gratis abonnementssleutels (F0) werkt niet. Zie voor meer informatie, [prijzen en beperkingen](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="custom-models"></a>Aangepaste modellen

Als u van plan bent om aan te passen akoestische of taal modellen, volgt u de stappen in [akoestische modellen aanpassen met](how-to-customize-acoustic-models.md) en [aanpassen van taalmodellen](how-to-customize-language-model.md). U moet de model-id's voor het gebruik van de gemaakte modellen in batch transcriptie. Deze ID is niet de eindpunt-ID die u op de detailweergave van het eindpunt vinden, is de model-ID die u ophalen kunt wanneer u de details van de modellen selecteren.

## <a name="the-batch-transcription-api"></a>De Batch-transcriptie API

De Batch-API voor transcriptie biedt asynchrone transcriptie van spraak naar tekst, samen met extra functies. Er is een REST-API die methoden voor het beschrijft:

1. Het maken van batch verwerken van aanvragen
1. Querystatus
1. Transcripties downloaden

> [!NOTE]
> De Batch-API voor transcriptie is ideaal voor callcenters, die meestal worden verzameld duizenden uur aan audio. Maakt het gemakkelijk grote hoeveelheden audio-opnamen transcriberen.

### <a name="supported-formats"></a>Ondersteunde indelingen

De Batch-API voor transcriptie ondersteunt de volgende indelingen:

| Indeling | Codec | Bitrate | Samplefrequentie |
|--------|-------|---------|-------------|
| WAV | PCM | 16-bits | 8 of 16 kHz, mono, stereo |
| MP3 | PCM | 16-bits | 8 of 16 kHz, mono, stereo |
| OGG | OPUS | 16-bits | 8 of 16 kHz, mono, stereo |

De Batch-transcriptie API wordt de linker- en -kanaal voor stereo audiostreams gesplitst tijdens de transcriptie. De twee JSON-bestanden met het resultaat zijn alle gemaakt van één kanaal. De tijdstempels per utterance bieden de ontwikkelaar te maken van een geordende definitieve transcriptie. In dit voorbeeld van een aanvraag bevat de eigenschappen voor het filteren van grof taalgebruik, interpunctie en word niveau tijdstempels.

### <a name="configuration"></a>Configuratie

Parameters voor de configuratie worden gegeven als JSON:

```json
{
  "recordingsUrl": "<URL to the Azure blob to transcribe>",
  "models": [{"Id":"<optional acoustic model ID>"},{"Id":"<optional language model ID>"}],
  "locale": "<locale to us, for example en-US>",
  "name": "<user defined name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "ProfanityFilterMode": "Masked",
    "PunctuationMode": "DictatedAndAutomatic",
    "AddWordLevelTimestamps" : "True",
    "AddSentiment" : "True"
  }
}
```

> [!NOTE]
> De Batch transcriptie-API maakt gebruik van een REST-service voor het aanvragen van transcripties, hun status en de bijbehorende resultaten. U kunt de API vanuit een willekeurige taal gebruiken. De volgende sectie wordt beschreven hoe de API wordt gebruikt.

### <a name="configuration-properties"></a>Configuratie-eigenschappen

Gebruik deze optionele eigenschappen transcriptie configureren:

| Parameter | Description |
|-----------|-------------|
| `ProfanityFilterMode` | Geeft aan hoe grof taalgebruik in herkenningsresultaten worden verwerkt. Geaccepteerde waarden zijn `none` die wordt uitgeschakeld grof taalgebruik filteren, `masked` die grof taalgebruik vervangen door sterretjes, `removed` waarbij alle scheldwoorden worden verwijderd uit het resultaat, of `tags` zodat 'grof taalgebruik' tags aan wordt toegevoegd. De standaardinstelling is `masked`. |
| `PunctuationMode` | Geeft aan hoe interpunctie in herkenningsresultaten worden verwerkt. Geaccepteerde waarden zijn `none` die wordt uitgeschakeld interpunctie, `dictated` dit expliciete interpunctie houdt `automatic` waarmee de decoder interpunctie, behandelt of `dictatedandautomatic` dit houdt bepaald leestekens of automatisch. |
 | `AddWordLevelTimestamps` | Hiermee geeft u als word niveau tijdstempels moet worden toegevoegd aan de uitvoer. Geaccepteerde waarden zijn `true` waarmee word niveau tijdstempels en `false` (de standaardwaarde) uitschakelen. |
 | `AddSentiment` | Hiermee geeft u op gevoel moet worden toegevoegd aan de utterance. Geaccepteerde waarden zijn `true` waarmee sentiment per utterance en `false` (de standaardwaarde) uitschakelen. |
 | `AddDiarization` | Hiermee geeft u op dat alalysis diarization moet worden uitgevoerd op de invoer die met twee stemmen mono-kanaal wordt verwacht. Geaccepteerde waarden zijn `true` waarmee diarization en `false` (de standaardwaarde) uitschakelen. Het is ook vereist `AddWordLevelTimestamps` moet worden ingesteld op true.|

### <a name="storage"></a>Opslag

Batch ondersteunt transcriptie [Azure Blob-opslag](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) voor het lezen van audio en transcripties van het schrijven naar de opslag.

## <a name="webhooks"></a>Webhooks

Polling voor de status van transcriptie mogelijk niet de meeste prestaties of de beste gebruikerservaring te bieden. Als u wilt laten pollen van status, kunt u zich registreren callbacks, die de client ontvangt wanneer langlopende transcriptie taken zijn voltooid.

Zie voor meer informatie, [Webhooks](webhooks.md).

## <a name="speaker-separation-diarization"></a>Sprekerherkenning scheiding (Diarization)

Diarization is het proces voor het scheiden van luidsprekers in een stukje audio. Onze pijplijn Batch ondersteunt Diarization en is geschikt voor twee sprekers op mono kanaal opnamen herkennen.

Als u wilt dat uw aanvraag audiotranscriptie voor diarization is verwerkt, moet u gewoon de relevante parameter in de HTTP-aanvraag toevoegen, zoals hieronder wordt weergegeven.

 ```json
{
  "recordingsUrl": "<URL to the Azure blob to transcribe>",
  "models": [{"Id":"<optional acoustic model ID>"},{"Id":"<optional language model ID>"}],
  "locale": "<locale to us, for example en-US>",
  "name": "<user defined name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "AddWordLevelTimestamps" : "True",
    "AddDiarization" : "True"
  }
}
```

Word niveau tijdstempels moet ook zijn ' ingeschakeld ' als de parameters in de bovenstaande aanvraag geven.

De bijbehorende audio bevat de luidsprekers geïdentificeerd door een getal (momenteel kunnen slechts twee stemmen, zodat de luidsprekers aangeduid als ' spreker 1 ' en 'Spreker 2') gevolgd door de uitvoer transcriptie.

Houd er ook rekening mee Diarization is niet beschikbaar in Stereo-opnamen. Bovendien alle JSON de uitvoer bevat de Sprekerherkenning-tag. Als diarization niet gebruikt wordt, wordt het weergegeven ' spreker: Null' in de JSON-uitvoer.

> [!NOTE]
> Diarization is beschikbaar in alle regio's en voor alle landinstellingen!

## <a name="sentiment"></a>Sentiment

Sentiment is een nieuwe functie in Batch transcriptie API en is een belangrijk onderdeel in het call center-domein. Klanten kunnen gebruikmaken van de `AddSentiment` parameters op hun aanvragen aan

1.  Verkrijg inzicht in de tevredenheid van klanten
2.  Krijg inzicht in de prestaties van de agents (team de gesprekken)
3.  Met deze functie het exacte punt in tijd wanneer een aanroep van een inschakelen vond in een negatieve richting
4.  Wat is een fout als negatief aanroepen naar positieve inschakelen met deze functie
5.  Wat klanten en wat ze niet bevalt over een product of een service identificeren

Gevoel wordt berekend per audio segment waarbij een audio-segment is gedefinieerd als het verstrijken van de tijd tussen het begin van de utterance (offset) en de detectie van stilte van einde van de bytestroom. De volledige tekst in dat segment wordt gebruikt om het gevoel te berekenen. We niet berekenen alle cumulatieve sentiment-waarden voor de volledige aanroep of de hele spraak van elk kanaal. Deze aggregaties worden van links naar de domeineigenaar verder toepassen.

Gevoel wordt toegepast op het lexicale formulier.

Een voorbeeld van de JSON-uitvoer ziet eruit zoals hieronder:

```json
{
  "AudioFileResults": [
    {
      "AudioFileName": "Channel.0.wav",
      "AudioFileUrl": null,
      "SegmentResults": [
        {
          "RecognitionStatus": "Success",
          "ChannelNumber": null,
          "Offset": 400000,
          "Duration": 13300000,
          "NBest": [
            {
              "Confidence": 0.976174,
              "Lexical": "what's the weather like",
              "ITN": "what's the weather like",
              "MaskedITN": "what's the weather like",
              "Display": "What's the weather like?",
              "Words": null,
              "Sentiment": {
                "Negative": 0.206194,
                "Neutral": 0.793785,
                "Positive": 0.0
              }
            }
          ]
        }
      ]
    }
  ]
}
```
De functie maakt gebruik van een model Sentiment, dat zich momenteel in de bètaversie.

## <a name="sample-code"></a>Voorbeeldcode

Volledige voorbeelden zijn beschikbaar in de [voorbeeldopslagplaats in GitHub](https://aka.ms/csspeech/samples) binnen de `samples/batch` submap.

U moet de voorbeeldcode met de gegevens van uw abonnement, de regio van de service, de SAS-URI die verwijst naar het audiobestand te transcriberen en model van id's in het geval u wilt gebruiken een aangepaste akoestische of taal model aanpassen.

[!code-csharp[Configuration variables for batch transcription](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchdefinition)]

De voorbeeldcode wordt de client setup en dien de aanvraag transcriptie. Deze vervolgens of er statusinformatie en details over de voortgang transcriptie afdrukken.

[!code-csharp[Code to check batch transcription status](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchstatus)]

Zie voor meer informatie over het aanroepen van de voorgaande onze [Swagger-document](https://westus.cris.ai/swagger/ui/index). Voor het volledige voorbeeld dat hier wordt weergegeven, gaat u naar [GitHub](https://aka.ms/csspeech/samples) in de `samples/batch` submap.

Noteer de asynchrone instellingen voor het plaatsen van audio en transcriptie status ontvangen. De client die u maakt, is een .NET-HTTP-client. Er is een `PostTranscriptions` methode voor het verzenden van de details van de audio-bestand en een `GetTranscriptions` methode voor het ontvangen van de resultaten. `PostTranscriptions` retourneert een ingang en `GetTranscriptions` gebruikt voor het maken van een greep om de status van de transcriptie.

De huidige voorbeeldcode opgeven niet een aangepast model. De service maakt gebruik van de basislijn-modellen voor te transcriberen van het bestand of de bestanden. U kunt doorgeven om op te geven de modellen, op dezelfde manier als de model-id voor de akoestische en het taalmodel.

> [!NOTE]
> Voor basislijn transcripties moet u niet de ID voor de basislijn-modellen declareren. Als u alleen een taal model-ID (en geen akoestisch model-ID) opgeeft, wordt een overeenkomende akoestisch model automatisch geselecteerd. Als u alleen een akoestisch model-ID opgeeft, wordt een overeenkomende taalmodel automatisch geselecteerd.

## <a name="download-the-sample"></a>Het voorbeeld downloaden

U vindt het voorbeeld in de `samples/batch` Active directory in de [voorbeeldopslagplaats in GitHub](https://aka.ms/csspeech/samples).

> [!NOTE]
> Batch transcriptie taken zijn gepland op beste vermogen, wordt er geen geschatte tijd voor wanneer een taak wordt gewijzigd in de actieve status. Eenmaal actief is, is de werkelijke transcriptie sneller verwerkt dan de audio realtime.

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
