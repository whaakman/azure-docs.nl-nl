---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: 74b9da1b3b81f9f35f231ef5caef8eafedc9aefc
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39343036"
---
<!-- N.B. no header, language-agnostic -->

De [spraak SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) biedt de eenvoudigste manier om het gebruik van **spraakomzetting** in uw toepassing.
De SDK biedt de volledige functionaliteit van de service. Het basisproces voor het uitvoeren van de vertaling van gesproken tekst bevat de volgende stappen uit:

1. Maken van een factory spraak, bieden een abonnementssleutel voor spraak-service of een verificatietoken en een [regio](~/articles/cognitive-services/speech-service/regions.md).
   
1. Maken een vertaling herkenning van de factory spraak. U kunt de bron- en talen voor tekstvertaling, evenals op te geven of u wilt dat SMS of spraak uitvoer configureren. Er zijn verschillende versies van de vertaling kenmerken op basis van de audio bron die u gebruikt.

1. De gebeurtenissen voor asynchrone bewerking bezighouden indien gewenst. Uw gebeurtenis-handlers voor de herkenning wordt aangeroepen wanneer er tussentijdse en definitieve resultaten, evenals een synthese-gebeurtenis voor de optionele audio-uitvoer. Anders ontvangt uw toepassing een definitieve transcriptie-resultaat.

1. Opname starten. Voor één vertaling gebruik RecognizeAsync(), dat wordt geretourneerd van de eerste utterance wordt herkend. Voor langlopende vertalingen, gebruikt u StartContinuousRecognitionAsync() en de gebeurtenissen voor asynchrone herkenningsresultaten bezighouden.

Zie de codefragmenten hieronder voor spraak vertaling scenario's met de spraak-SDK.

[!include[Get a Subscription Key](cognitive-services-speech-service-get-subscription-key.md)]
