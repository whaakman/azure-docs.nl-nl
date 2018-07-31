---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: d7d73d9c5e85ac550b24c73797d536dc4d0fc6ed
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39346270"
---
<!-- N.B. no header, no intents here, language-agnostic -->

De [spraak SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) biedt de eenvoudigste manier om het gebruik van **spraak naar tekst** in uw toepassing met volledige functionaliteit.

1. Maken van een factory spraak, bieden een abonnementssleutel voor spraak-service of een verificatietoken en een [regio](~/articles/cognitive-services/speech-service/regions.md). U kunt ook een aangepast eindpunt om op te geven van een niet-standaard service-eindpunt opgeven.

1. Ontvang een spraakherkenning van de factory spraak. U kunt de invoer taal, evenals de uitvoerindeling configureren.  Een kenmerk kunt van het apparaat standaard microfoon, een audiostream of audio uit een bestand gebruiken.

1. De gebeurtenissen voor asynchrone bewerking bezighouden indien gewenst. De gebeurtenis-handlers de herkenning wordt aangeroepen wanneer er een tijdelijke en laatste resultaten.  Anders ontvangt uw toepassing een definitieve transcriptie-resultaat.

1. Opname starten. Gebruik voor één spraakherkenning, zoals opdracht of een query spraakherkenning, `RecognizeAsync()`, die de eerste utterance wordt erkend als resultaat geeft. Voor langlopende spraakherkenning, zoals transcriptie, gebruikt u `StartContinuousRecognitionAsync()` en de gebeurtenissen voor asynchrone herkenningsresultaten bezighouden.

Zie de codefragmenten hieronder voor spraakherkenning scenario's met de spraak-SDK.

[!include[Get a Subscription Key](cognitive-services-speech-service-get-subscription-key.md)]
