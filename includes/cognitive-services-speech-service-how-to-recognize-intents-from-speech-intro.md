---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: 79dc1fe9fffd8c44c97cf68a12bb90655b672577
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39577977"
---
<!-- N.B. no header, language-agnostic -->

De [spraak SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) biedt een manier voor het herkennen van **intents van gesproken inhoud**, aangedreven door de Speech-service en de [Language Understanding-service (LUIS)](https://www.luis.ai/home).

1. Maken van een factory spraak, bieden de abonnementssleutel van een Language Understanding-service en [regio](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition). De abonnementssleutel van Language Understanding-service wordt aangeroepen **eindpuntsleutel** in de documentatie van de service. U kunt de sleutel ontwerpen Language Understanding-service niet gebruiken. Zie ook de **Opmerking** hieronder.

1. Ontvang een intentie herkenning van de factory spraak. Een kenmerk kunt van het apparaat standaard microfoon, een audiostream of audio uit een bestand gebruiken.

1. Het taalmodel van de kennis op basis van uw App-id ophalen en toevoegen van de intenties die u nodig hebt. 

1. De gebeurtenissen voor asynchrone bewerking bezighouden indien gewenst. Uw gebeurtenis-handlers voor de herkenning wordt aangeroepen wanneer het tussentijdse en definitieve resultaten (inclusief intents) heeft. Anders ontvangt uw toepassing een definitieve transcriptie-resultaat.

1. Intentieherkenning starten. Gebruik voor één spraakherkenning, zoals opdracht of een query spraakherkenning, `RecognizeAsync()`, die de eerste utterance wordt erkend als resultaat geeft. Gebruik voor herkenning van langlopende `StartContinuousRecognitionAsync()` en de gebeurtenissen voor asynchrone herkenningsresultaten bezighouden.

Zie de codefragmenten hieronder voor intentieherkenning scenario's met de spraak-SDK. Vervangen door uw eigen abonnementssleutel Language Understanding (eindpuntsleutel), de [regio van uw abonnement](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition), en de AppId van de intentie model op de juiste plaatsen in de voorbeelden.

> [!NOTE]
> In tegenstelling tot andere services die wordt ondersteund door de SDK-spraak, is intentieherkenning vereist een specifiek abonnementssleutel (Language Understanding-service-eindpunt). [Hier](https://www.luis.ai) vindt u meer informatie over de intentieherkenning-technologie. Over het verkrijgen van de **eindpuntsleutel** wordt beschreven [hier](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/luis-how-to-azure-subscription#create-luis-endpoint-key).
