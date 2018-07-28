---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: ca342a812f6a8de19c732b5df4fab14a825f6c51
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39330819"
---
<!-- N.B. no header, no intents here, language-agnostic -->

De [spraak SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) biedt de eenvoudigste manier om het gebruik van **spraak naar tekst** in uw toepassing met volledige functionaliteit.

1. Maken van een factory spraak, bieden een abonnementssleutel voor spraak-service of een verificatietoken en een [regio](~/articles/cognitive-services/speech-service/regions.md).
   U kunt ook opties, zoals de taal of een aangepast eindpunt voor uw eigen modellen voor spraakherkenning Spraakherkenning configureren.

1. Ontvang een spraakherkenning van de factory spraak.
   Een kenmerk kunt van het apparaat standaard microfoon, een audiostream of audio uit een bestand gebruiken.

1. De gebeurtenissen voor asynchrone bewerking bezighouden indien gewenst.
   De gebeurtenis-handlers de herkenning wordt aangeroepen wanneer er een tijdelijke en laatste resultaten.
   Anders ontvangt uw toepassing een definitieve transcriptie-resultaat.

1. Opname starten.
   Gebruik voor één spraakherkenning, zoals opdracht of een query spraakherkenning, `RecognizeAsync()`, die de eerste utterance wordt erkend als resultaat geeft.
   Voor langlopende spraakherkenning, zoals transcriptie, gebruikt u `StartContinuousRecognitionAsync()` en de gebeurtenissen voor asynchrone herkenningsresultaten bezighouden.

Hieronder laten we zien verschillende codefragmenten voor spraakherkenning scenario's met de spraak-SDK.

[!include[Get a Subscription Key](cognitive-services-speech-service-get-subscription-key.md)]
