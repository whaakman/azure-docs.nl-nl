---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
origin.date: 09/24/2018
ms.date: 04/01/2019
ms.author: v-biyu
ms.openlocfilehash: ab4795325f733d15403b53f027daa4fb8c824cc6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60531039"
---
<!-- N.B. no header, language-agnostic -->

De Microsoft Cognitive Services [spraak SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) biedt de eenvoudigste manier om het gebruik van **spraakomzetting** in uw toepassing.
De SDK biedt de volledige functionaliteit van de service. Het basisproces voor het uitvoeren van de vertaling van gesproken tekst bevat de volgende stappen uit:

1. Maak een configuratie van de vertaling spraak en bieden een abonnementssleutel voor spraak-service (of een verificatietoken) en een [regio](~/articles/cognitive-services/speech-service/regions.md) als parameters. Indien nodig, kunt u de configuratie wijzigen. Bijvoorbeeld, kunt u de talen van het bron- en NAT configureren, evenals opgeven of u wilt dat SMS of spraak uitvoer.

1. Maak een NAT-herkenning van de configuratie van de vertaling spraak. Geef een audio-configuratie als u wilt dat kent van een andere bron dan de standaard-microfoon (bijvoorbeeld audiostream of audio-bestand).

1. De gebeurtenissen voor asynchrone bewerking bezighouden indien gewenst. Uw gebeurtenis-handlers voor de herkenning wordt aangeroepen wanneer er tussentijdse en definitieve resultaten, evenals een synthese-gebeurtenis voor de optionele audio-uitvoer. Anders ontvangt uw toepassing alleen een resultaat van de definitieve transcriptie.

1. Opname starten. Voor één vertaling gebruiken de `RecognizeOnceAsync()` methode, waarbij de eerste herkende utterance retourneert. Voor langlopende vertalingen, gebruikt u de `StartContinuousRecognitionAsync()` methode en tie van de gebeurtenissen voor asynchrone herkenningsresultaten.

Zie de volgende codefragmenten voor spraak vertaling scenario's die gebruikmaken van de spraak-SDK.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]
