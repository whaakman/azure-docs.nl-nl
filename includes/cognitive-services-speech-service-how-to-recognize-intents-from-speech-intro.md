---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: a4accb0846ec9cf5efc5ca5aa00c731edd41d4d4
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43143906"
---
<!-- N.B. no header, language-agnostic -->

De Microsoft Cognitive Services [spraak SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) biedt een manier voor het herkennen van **intents van gesproken inhoud** en wordt ondersteund door de Cognitive Services [Language Understanding-service (LUIS)](https://www.luis.ai/home).

1. Een spraak-factory maken met een LUIS-abonnementssleutel en [regio](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition) als parameters. De abonnementssleutel LUIS heet **eindpuntsleutel** in de documentatie van de service. U kunt de sleutel ontwerpen LUIS niet gebruiken. (Zie de opmerking later in deze sectie.)

1. Ontvang een intentie herkenning van de factory spraak. Een kenmerk kunt van het apparaat standaard microfoon, een audiostream of audio uit een bestand gebruiken.

1. Ophalen van het taalmodel wat die gebaseerd op uw **AppId**. Voeg de die u nodig hebt. 

1. De gebeurtenissen voor asynchrone bewerking bezighouden indien gewenst. Uw gebeurtenis-handlers voor de herkenning wordt aangeroepen wanneer er een tijdelijke en laatste resultaten (inclusief intents). Als u de gebeurtenissen niet bezighouden, krijgt uw toepassing alleen een resultaat van de definitieve transcriptie.

1. Intentieherkenning starten. Voor één spraakherkenning, zoals opdracht of een query spraakherkenning, gebruikt u de `RecognizeAsync()` methode. Deze methode retourneert de eerste herkende utterance. Voor langlopende spraakherkenning, gebruikt u de `StartContinuousRecognitionAsync()` methode. De gebeurtenissen voor asynchrone herkenningsresultaten bezighouden.

Zie de volgende codefragmenten voor intentieherkenning-scenario's die gebruikmaken van de spraak-SDK. Vervang de waarden in het voorbeeld door uw eigen abonnementssleutel LUIS (eindpuntsleutel), de [regio van uw abonnement](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition), en de **AppId** van de intentie model.

> [!NOTE]
> In tegenstelling tot andere services die wordt ondersteund door de SDK-spraak, is intentieherkenning vereist een specifiek abonnement-sleutel (LUIS eindpuntsleutel). Zie voor meer informatie over de technologie intentieherkenning de [LUIS website](https://www.luis.ai). Voor meer informatie over het verkrijgen van de **eindpuntsleutel**, Zie [maken van een LUIS-eindpuntsleutel](https://docs.microsoft.com/azure/cognitive-services/LUIS/luis-how-to-azure-subscription#create-luis-endpoint-key).
