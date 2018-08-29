---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: 1103e5a217ca4972cc1c983a7ad0d07b0797e9e9
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43122642"
---
<!-- N.B. no header, no intents here, language-agnostic -->

De Cognitive Services [spraak SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) biedt de eenvoudigste manier om het gebruik van **spraak naar tekst** in uw toepassing met volledige functionaliteit.

1. Maken van een factory spraak en bieden een abonnementssleutel voor spraak-service (of een verificatietoken) en een [regio](~/articles/cognitive-services/speech-service/regions.md) als parameters. U kunt ook een aangepast eindpunt om op te geven van een niet-standaard service-eindpunt opgeven.

1. Ontvang een spraakherkenning van de factory spraak. U kunt de taal en indeling van de uitvoer configureren. Een kenmerk kunt van het apparaat standaard microfoon, een audiostream of audio uit een bestand gebruiken.

1. De gebeurtenissen voor asynchrone bewerking bezighouden indien gewenst. De gebeurtenis-handlers de herkenning wordt aangeroepen wanneer er een tijdelijke en laatste resultaten. Anders ontvangt uw toepassing alleen een resultaat van de definitieve transcriptie.

1. Opname starten. Voor één spraakherkenning, zoals opdracht of een query spraakherkenning, gebruikt u de `RecognizeAsync()` methode. Deze methode retourneert de eerste herkende utterance. Voor langlopende spraakherkenning, zoals transcriptie, gebruikt u de `StartContinuousRecognitionAsync()` methode. De gebeurtenissen voor asynchrone herkenningsresultaten bezighouden.

Zie de volgende codefragmenten voor spraak opname-scenario's die gebruikmaken van de spraak-SDK.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]
