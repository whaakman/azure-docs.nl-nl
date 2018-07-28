---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: 1b55576581ebddc90c0af6b99a04dbe66d2e3b87
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39330813"
---
<!-- N.B. no header, language-agnostic -->

De [spraak SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) biedt een manier voor het herkennen van intenties van spraak, mogelijk gemaakt door de spraak-service en de [Language Understanding-service (LUIS)](https://luis.ai).

1. Maken van een factory spraak, bieden de abonnementssleutel van een Language Understanding-service en [regio](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition).


1. Ontvang een intentie herkenning van de factory spraak.
   Een kenmerk kunt van het apparaat standaard microfoon, een audiostream of audio uit een bestand gebruiken.

1. De gebeurtenissen voor asynchrone bewerking bezighouden indien gewenst.
   De gebeurtenis-handlers de herkenning wordt aangeroepen wanneer er een tijdelijke en laatste resultaten.
   Anders ontvangt uw toepassing een definitieve transcriptie-resultaat.

1. Opname starten.
   Gebruik voor één spraakherkenning, zoals opdracht of een query spraakherkenning, `RecognizeAsync()`, die de eerste utterance wordt erkend als resultaat geeft.
   Voor langlopende spraakherkenning, zoals transcriptie, gebruikt u `StartContinuousRecognitionAsync()` en de gebeurtenissen voor asynchrone herkenningsresultaten bezighouden.

Hieronder laten we zien verschillende codefragmenten voor intentieherkenning scenario's met de spraak-SDK.

> [!NOTE]
> Voer eerst een abonnementssleutel verkrijgen.
> In tegenstelling tot andere services die wordt ondersteund door de SDK-spraak, vereist intentieherkenning dat de sleutel van een bepaald abonnement.
> [Hier](https://www.luis.ai) vindt u meer informatie over de intentieherkenning-technologie, evenals informatie over het verkrijgen van een abonnementssleutel.
> Vervang de sleutel van uw eigen Language Understanding-abonnement, de [regio van uw abonnement](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition), en de AppId van de intentie model op de juiste plaatsen in de voorbeelden.
