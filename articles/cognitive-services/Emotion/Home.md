---
title: Overzicht van emotion-API | Microsoft Docs
description: Gebruik de algoritme van Microsoft geavanceerde, cloud-gebaseerde emotion erkenning meer persoonlijke apps met de Emotion-API in cognitieve Services maken.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 02/06/2017
ms.author: anroth
ms.openlocfilehash: 8383370cba3f78060e809f444f4ad3dab7380f4e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344801"
---
# <a name="what-is-emotion-api"></a>Wat is Emotion-API?

> [!IMPORTANT]
> Emotion-API is op 30 oktober 2017 afgeschaft. De functionaliteit is nu onderdeel van [Face-API](https://docs.microsoft.com/en-us/azure/cognitive-services/face/).

Welkom bij de Emotion-API van Microsoft, zodat u meer persoonlijke apps met Microsoft cloud-gebaseerde emotion erkenning algoritme maken.

### <a name="emotion-recognition"></a>Emotion opname

De bètaversie Emotion-API wordt een afbeelding als invoer en retourneert het vertrouwen van een set emoties voor elk vlak in de installatiekopie, evenals een selectiekader voor het vlak van de Face-API. De emoties gedetecteerd zijn gelukkig, sadness, onverwacht, volgt uitzien, bang, contempt, beantwoord of neutrale. Deze emoties cross-culturally en universeel worden doorgegeven via de dezelfde basic bedacht, waar zijn geïdentificeerd door Emotion-API. 

**Interpreteren resultaten:** 

In het interpreteren van de resultaten van de Emotion-API, de emotion gedetecteerd moet worden geïnterpreteerd als de emotion met de hoogste score zoals scores zijn genormaliseerd aan som naar een. Gebruikers kunnen ervoor kiezen om in te stellen van een drempelwaarde voor hogere betrouwbaarheid in de toepassing, afhankelijk van hun behoeften. 

Zie voor meer informatie over emotiedetectie de API-verwijzing: 
  * Basic: Als een gebruiker al voor de Face-API aangeroepen is, kunnen indienen van de rechthoek face als invoer en met de basisstaffel. [API-naslaginformatie](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f23eb019845524ec61c4d7)
  * Standaard: Als een rechthoek face worden niet door een gebruiker worden verzonden, moeten ze standaardmodus gebruiken.  [API-naslaginformatie](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)

Zie voor een voorbeeld over het interpreteren van streaming video met Emotion-API [video's in realtime analyseren hoe](https://docs.microsoft.com/azure/cognitive-services/emotion/emotion-api-how-to-topics/howtoanalyzevideo_emotion).
