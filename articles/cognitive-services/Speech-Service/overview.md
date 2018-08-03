---
title: Wat is de spraakservice (preview)?
description: 'De spraak-service, onderdeel van de Microsoft Cognitive Services, koppelt zijn verschillende Azure speech services die eerder beschikbaar afzonderlijk waren netwerk: Bing Speech (die bestaat uit spraakherkenning en tekst naar spraak), aangepaste spraak en Spraakomzetting.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 0cd6d984ac9329112aa388e8d8ee808d4c3e6227
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39445631"
---
# <a name="what-is-the-speech-service-preview"></a>Wat is de spraakservice (preview)?

De spraak-service wordt mogelijk gemaakt door de technologieën die worden gebruikt in andere Microsoft-producten, met inbegrip van Cortana en Microsoft Office.  Deze dezelfde service is beschikbaar voor elke klant met een als een Cognitive Service. 

> [!NOTE]
> De spraak-service is momenteel in openbare preview. Ga terug hier regelmatig op updates naar documentatie en voorbeelden van aanvullende code.

Onze Speech-service biedt ontwikkelaars een eenvoudige manier om aan te geven van hun toepassingen krachtige spraak ingeschakelde functies met één abonnement. Uw apps kunnen nu voorzien van gesproken opdrachten, transcriptie, dicteren, spraaksynthese en vertaling.

## <a name="speech-service-features"></a>Spraak-service-functies

|Functie|Beschrijving|
|-|-|
|[Spraak-naar-tekst](speech-to-text.md)| Transcribes audiostreams naar tekst die uw toepassing als invoer accepteren kunt. Ook worden geïntegreerd met de [Language Understanding service](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) worden afgeleid van de bedoeling van gebruikers van uitingen.|
|[Tekst naar spraak](text-to-speech.md)| Converteert tekst zonder opmaak naar natuurlijk klinkende spraak, die worden geleverd aan uw toepassing in een geluidsbestand. Meerdere stemmen, variëren in geslacht of accent, zijn beschikbaar voor vele ondersteunde talen. |
|[Spraakomzetting](speech-translation.md)| Kan worden gebruikt voor de omzetting van streaming audio in near-real-time of opgenomen spraak te verwerken. |
|[Spraak apparaten SDK](speech-devices-sdk.md)| Met de introductie van de geïntegreerde Speech-service bieden Microsoft en haar partners een geïntegreerde hardware/software-platform dat is geoptimaliseerd voor het ontwikkelen spraak ingeschakelde apparaten |

## <a name="using-the-speech-service"></a>Met behulp van de Speech-service

De spraak-service is beschikbaar op twee manieren. [De SDK](speech-sdk.md) de details van de netwerkprotocollen volledig weggewerkt. De [REST-API](rest-apis.md) werkt met elke programmeertaal, maar biedt niet alle functies die worden aangeboden door de SDK.

|<br>Methode|Speech<br>naar tekst|Tekst die moet worden<br>Speech|Speech<br>Omzetting|<br>Beschrijving|
|-|-|-|-|-|
|[SDK 's](speech-sdk.md)|Ja|Nee|Ja|-Bibliotheken voor specifieke programmeertalen die eenvoudiger.|
|[REST](rest-apis.md)|Ja|Ja|Nee|Een eenvoudige, door de HTTP-gebaseerde API waarmee u eenvoudig spraak toevoegen aan uw toepassing.|

## <a name="next-steps"></a>Volgende stappen

Een gratis proefabonnement-sleutel voor de service voor spraak ophalen.

> [!div class="nextstepaction"]
> [De spraak-service gratis uitproberen](get-started.md)
