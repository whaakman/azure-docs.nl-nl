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
ms.date: 2/20/2019
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 3b403eb80bae01efe730b69b7e6a5ddaea81355a
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447647"
---
# <a name="why-use-batch-transcription"></a>Waarom Batch transcriptie gebruiken?

Batch transcriptie is ideaal als u wilt dat een grote hoeveelheid audio in opslag, zoals Azure Blobs transcriberen. U kunt met behulp van de toegewezen REST-API, wijst u audio-bestanden met een shared access signature (SAS) URI en asynchroon ontvangen transcripties.

## <a name="prerequisites"></a>Vereisten

### <a name="subscription-key"></a>Abonnementssleutel

Als met alle functies van de spraak-service die u een abonnementssleutel van maakt de [Azure-portal](https://portal.azure.com) door onze [introductiehandleiding](get-started.md). Als u van plan bent om op te halen transcripties van onze basislijn-modellen, is het maken van een sleutel alles die wat u nodig om te doen.

>[!NOTE]
> Een standard-abonnement (S0) voor Speech Services is vereist voor het gebruik van batch transcriptie. Gratis abonnementssleutels (F0) werkt niet. Zie voor meer informatie, [prijzen en beperkingen](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/speech-services/).

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
  "models": ["<optional acoustic model ID>, <optional language model ID>"],
  "locale": "<local to us, for example en-US>",
  "name": "<user define name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "ProfanityFilterMode": "Masked",
    "PunctuationMode": "DictatedAndAutomatic",
    "AddWordLevelTimestamps" : "True"
  }
}
```

> [!NOTE]
> De Batch transcriptie-API maakt gebruik van een REST-service voor het aanvragen van transcripties, hun status en de bijbehorende resultaten. U kunt de API vanuit een willekeurige taal gebruiken. De volgende sectie wordt beschreven hoe de API wordt gebruikt.

### <a name="configuration-properties"></a>Configuratie-eigenschappen

| Parameter | Description | Vereiste / optioneel |
|-----------|-------------|---------------------|
| `ProfanityFilterMode` | Geeft aan hoe grof taalgebruik in herkenningsresultaten worden verwerkt. Geaccepteerde waarden zijn `none` die wordt uitgeschakeld grof taalgebruik filteren, `masked` die grof taalgebruik vervangen door sterretjes, `removed` waarbij alle scheldwoorden worden verwijderd uit het resultaat, of `tags` zodat 'grof taalgebruik' tags aan wordt toegevoegd. De standaardinstelling is `masked`. | Optioneel |
| `PunctuationMode` | Geeft aan hoe interpunctie in herkenningsresultaten worden verwerkt. Geaccepteerde waarden zijn `none` die wordt uitgeschakeld interpunctie, `dictated` dit expliciete interpunctie houdt `automatic` waarmee de decoder interpunctie, behandelt of `dictatedandautomatic` dit houdt bepaald leestekens of automatisch. | Optioneel |
 | `AddWordLevelTimestamps` | Hiermee geeft u als word niveau tijdstempels moet worden toegevoegd aan de uitvoer. Geaccepteerde waarden zijn `true` waarmee word niveau tijdstempels en `false` (de standaardwaarde) uitschakelen. | Optioneel |

## <a name="sample-code"></a>Voorbeeldcode

Het volledige voorbeeld is beschikbaar in de [voorbeeldopslagplaats in GitHub](https://aka.ms/csspeech/samples) binnen de `samples/batch` submap.

U moet de voorbeeldcode met de gegevens van uw abonnement, de regio van de service, de SAS-URI die verwijst naar het audiobestand te transcriberen en model van id's in het geval u wilt gebruiken een aangepaste akoestische of taal model aanpassen. 

[!code-csharp[Configuration variables for batch transcription](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchdefinition)]

De voorbeeldcode wordt de client setup en dien de aanvraag transcriptie. Deze vervolgens of er statusinformatie en details over de voortgang transcriptie afdrukken.

[!code-csharp[Code to check batch transcription status](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchstatus)]

Zie voor meer informatie over het aanroepen van de voorgaande onze [Swagger-document](https://westus.cris.ai/swagger/ui/index). Voor het volledige voorbeeld dat hier wordt weergegeven, gaat u naar [GitHub](https://aka.ms/csspeech/samples) in de `samples/batch` submap.

Noteer de asynchrone instellingen voor het plaatsen van audio en transcriptie status ontvangen. De client die u maakt, is een .NET-HTTP-client. Er is een `PostTranscriptions` methode voor het verzenden van de details van de audio-bestand en een `GetTranscriptions` methode voor het ontvangen van de resultaten. `PostTranscriptions` retourneert een ingang en `GetTranscriptions` gebruikt voor het maken van een greep om de status van de transcriptie.

De huidige voorbeeldcode opgeven niet een aangepast model. De service maakt gebruik van de basislijn-modellen voor te transcriberen van het bestand of de bestanden. U kunt doorgeven om op te geven de modellen, op dezelfde manier als de model-id voor de akoestische en het taalmodel.

> [!NOTE]
> Voor basislijn transcripties moet u niet de ID voor de basislijn-modellen declareren. Als u alleen een taal model-ID (en geen akoestisch model-ID) opgeeft, wordt een overeenkomende akoestisch model automatisch geselecteerd. Als u alleen een akoestisch model-ID opgeeft, wordt een overeenkomende taalmodel automatisch geselecteerd.

### <a name="supported-storage"></a>Ondersteunde opslag

Op dit moment wordt alleen Azure Blob-opslag ondersteund.

## <a name="download-the-sample"></a>Het voorbeeld downloaden

U vindt het voorbeeld in de `samples/batch` Active directory in de [voorbeeldopslagplaats in GitHub](https://aka.ms/csspeech/samples).

> [!NOTE]
> Batch transcriptie taken zijn gepland op beste vermogen, wordt er geen geschatte tijd voor wanneer een taak wordt gewijzigd in de actieve status. Eenmaal actief is, is de werkelijke transcriptie sneller verwerkt dan de audio realtime.

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
