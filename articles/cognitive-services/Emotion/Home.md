---
title: Overzicht van de emotion-API | Microsoft Docs
description: De Microsoft baanbrekende, op de cloud gebaseerde emoties opname-algoritme gebruiken voor apps bouwen die persoonlijker, met de Emotion-API in Cognitive Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 02/06/2017
ms.author: anroth
ms.openlocfilehash: 210990b0f436fd75cb36e71ea28928c457a5232e
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45573551"
---
# <a name="what-is-emotion-api"></a>Wat is Emotion-API?

> [!IMPORTANT]
> Emotion-API is op 30 oktober 2017 afgeschaft. De functionaliteit is nu onderdeel van [Face-API](https://docs.microsoft.com/azure/cognitive-services/face/).

Welkom bij de Emotion-API van Microsoft, waarmee u apps bouwen die persoonlijker met van Microsoft cloud-gebaseerde emoties erkenning algoritme.

### <a name="emotion-recognition"></a>Herkenning van emoties

De bètaversie van de Emotion-API wordt een afbeelding als invoer en retourneert de betrouwbaarheid voor een set emoties voor elk gezicht in de afbeelding, evenals een begrenzingsvak voor het gezicht van de Face-API. De gedetecteerde emoties zijn blijdschap, verdriet, verrassing, boosheid, angst, minachting, walging of neutraal. Deze emoties worden gecommuniceerd tussen verschillende culturen en worden universeel via de dezelfde basic gezichtsuitdrukkingen, indien worden aangeduid met de Emotion-API. 

**Resultaten interpreteren:** 

In het interpreteren van de resultaten van de Emotion-API, de emoties gedetecteerd moet worden geïnterpreteerd als de emoties met de hoogste score, terwijl scores zijn genormaliseerd op som naar een. Gebruikers kunnen ervoor kiezen om in te stellen van een drempelwaarde voor hogere betrouwbaarheid in de toepassing, afhankelijk van hun behoeften. 

Zie voor meer informatie over detectie van emoties in de API-verwijzing: 
  * Basic: Als een gebruiker al voor de Face-API aangeroepen is, ze kunnen indienen van het gezichtsrechthoek als invoer en de basic-laag. [API-naslaginformatie](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f23eb019845524ec61c4d7)
  * Standaard: Als een gebruiker niet een gezichtsrechthoek verzenden is, moeten ze standaardmodus gebruiken.  [API-naslaginformatie](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)

Zie voor een voorbeeld over het interpreteren van streaming video met de Emotion-API, [hoe u video's analyseren in Real Time](https://docs.microsoft.com/azure/cognitive-services/emotion/emotion-api-how-to-topics/howtoanalyzevideo_emotion).
