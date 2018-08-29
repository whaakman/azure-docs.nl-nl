---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: ee50a104a75d3cd5ff958bd49a1ff7010c5d5083
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43144570"
---
<!-- N.B. no header, language-agnostic -->

De Microsoft Cognitive Services [spraak SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) biedt de eenvoudigste manier om het gebruik van **spraakomzetting** in uw toepassing.
De SDK biedt de volledige functionaliteit van de service. Het basisproces voor het uitvoeren van de vertaling van gesproken tekst bevat de volgende stappen uit:

1. Maken van een factory spraak en geef een abonnementssleutel voor spraak-service of een verificatietoken en een [regio](~/articles/cognitive-services/speech-service/regions.md) als parameters.
   
1. Maken een vertaling herkenning van de factory spraak. U kunt de talen van het bron- en NAT configureren, evenals opgeven of u wilt dat SMS of spraak uitvoer. Er zijn verschillende versies van de vertaling kenmerken op basis van de bron van de audio die u gebruikt.

1. De gebeurtenissen voor asynchrone bewerking bezighouden indien gewenst. Uw gebeurtenis-handlers voor de herkenning wordt aangeroepen wanneer er tussentijdse en definitieve resultaten, evenals een synthese-gebeurtenis voor de optionele audio-uitvoer. Anders ontvangt uw toepassing alleen een resultaat van de definitieve transcriptie.

1. Opname starten. Voor één vertaling gebruiken de `RecognizeAsync()` methode, waarbij de eerste herkende utterance retourneert. Voor langlopende vertalingen, gebruikt u de `StartContinuousRecognitionAsync()` methode en tie van de gebeurtenissen voor asynchrone herkenningsresultaten.

Zie de volgende codefragmenten voor spraak vertaling scenario's die gebruikmaken van de spraak-SDK.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]
