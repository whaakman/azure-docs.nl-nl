---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 80bf9247bbb07fa61b7153e321b1991b82a9d616
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56246829"
---
<!-- N.B. no header, language-agnostic -->

De Microsoft Cognitive Services [spraak SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) biedt een manier voor het herkennen van **intents van gesproken inhoud** en wordt ondersteund door de Cognitive Services [Language Understanding-service (LUIS)](https://www.luis.ai/home).

1. Een spraak-configuratie maken met een LUIS-abonnementssleutel en [regio](~/articles/cognitive-services/speech-service/regions.md#intent-recognition) als parameters. De abonnementssleutel LUIS heet **eindpuntsleutel** in de documentatie van de service. U kunt de sleutel ontwerpen LUIS niet gebruiken. (Zie de opmerking later in deze sectie.)

1. Maak een intentie herkenning van de spraak-configuratie. Geef een audio-configuratie als u wilt dat kent van een andere bron dan de standaard-microfoon (bijvoorbeeld audiostream of audio-bestand).

1. Ophalen van het taalmodel wat die gebaseerd op uw **AppId**. Voeg de die u nodig hebt.

1. De gebeurtenissen voor asynchrone bewerking bezighouden indien gewenst. Uw gebeurtenis-handlers voor de herkenning wordt aangeroepen wanneer er een tijdelijke en laatste resultaten (inclusief intents). Als u de gebeurtenissen niet bezighouden, krijgt uw toepassing alleen een resultaat van de definitieve transcriptie.

1. Intentieherkenning starten. Voor één spraakherkenning, zoals opdracht of een query spraakherkenning, gebruikt u de `RecognizeOnceAsync()` methode. Deze methode retourneert de eerste herkende utterance. Voor langlopende spraakherkenning, gebruikt u de `StartContinuousRecognitionAsync()` methode. De gebeurtenissen voor asynchrone herkenningsresultaten bezighouden.

Zie de volgende codefragmenten voor intentieherkenning-scenario's die gebruikmaken van de spraak-SDK. Vervang de waarden in het voorbeeld door uw eigen abonnementssleutel LUIS (eindpuntsleutel), de [regio van uw abonnement](~/articles/cognitive-services/speech-service/regions.md#intent-recognition), en de **AppId** van de intentie model.

> [!NOTE]
> In tegenstelling tot andere services die wordt ondersteund door de SDK-spraak, is intentieherkenning vereist een specifiek abonnement-sleutel (LUIS eindpuntsleutel). Zie voor meer informatie over de technologie intentieherkenning de [LUIS website](https://www.luis.ai). Voor meer informatie over het verkrijgen van de **eindpuntsleutel**, Zie [maken van een LUIS-eindpuntsleutel](https://docs.microsoft.com/azure/cognitive-services/LUIS/luis-how-to-azure-subscription).
