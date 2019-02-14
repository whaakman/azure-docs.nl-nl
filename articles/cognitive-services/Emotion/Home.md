---
title: Wat is de Emotion-API?
titlesuffix: Azure Cognitive Services
description: Met het cloudalgortime voor emotieherkenning kunt u apps bouwen die persoonlijker zijn.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: overview
ms.date: 02/06/2017
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: ceeea8c143792e9a46b4b8a9892cad07770c5fbf
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56236444"
---
# <a name="what-is-the-emotion-api"></a>Wat is de Emotion-API?

> [!IMPORTANT]
> De Emotion-API wordt op 15 februari 2019 afgeschaft. De mogelijkheid voor de herkenning van emoties is nu algemeen beschikbaar als onderdeel van de [Face-API](https://docs.microsoft.com/azure/cognitive-services/face/). 

Welkom bij de Emotion-API van Microsoft, waarmee u persoonlijkere apps kunt bouwen met het cloudalgoritme voor emotieherkenning van Microsoft.

### <a name="emotion-recognition"></a>Emotieherkenning

De bètaversie van de Emotion-API gebruikt een afbeelding als invoer en retourneert de betrouwbaarheid voor een set emoties voor elk gezicht in de afbeelding, evenals de begrenzingsrechthoek voor het gezicht, met behulp van de Face-API. De gedetecteerde emoties zijn blijdschap, droefheid, verbazing, boosheid, angst, afkeuring, weerzin en neutraal. Deze emoties worden universeel, in verschillende culturen, met dezelfde basisgezichtsuitdrukkingen gecommuniceerd, die door de Emotion-API worden herkend.

**Resultaten interpreteren:**

Bij het interpreteren van de resultaten van de Emotion-API zou de emotie met de hoogste score als de gedetecteerde emotie moeten worden geïnterpreteerd, aangezien de scores worden genormaliseerd. Gebruikers kunnen een hogere drempelwaarde voor betrouwbaarheid instellen in de toepassing, afhankelijk van hun behoeften.

Zie de API-naslaginformatie voor meer informatie over het detecteren van emoties:
  * Basic: Als gebruikers al over de Face-API beschikken, kunnen ze de gezichtsrechthoek als invoer verzenden en de categorie Basic gebruiken. [API-naslaginformatie](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f23eb019845524ec61c4d7)
  * Standard: Als gebruikers geen gezichtsrechthoek verzenden, moeten ze de standaardmodus gebruiken.  [API-naslaginformatie](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)

Zie [How to Analyze Videos in Real Time](https://docs.microsoft.com/azure/cognitive-services/emotion/emotion-api-how-to-topics/howtoanalyzevideo_emotion) (Video's in realtime analyseren) voor een voorbeeld van het interpreteren van streaming video met de Emotion-API.
