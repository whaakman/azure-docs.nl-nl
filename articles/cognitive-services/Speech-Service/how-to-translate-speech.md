---
title: Met behulp van spraakservices spraak vertalen | Microsoft Docs
description: Informatie over het spraak vertaling gebruiken in de service spraak.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 7f39f284998489574049d82c44b3d3a0a3797adb
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "35350009"
---
# <a name="translate-speech-using-speech-service"></a>Met behulp van spraak service spraak vertalen

De [spraak SDK](speech-sdk.md) is de eenvoudigste manier om de vertaling van spraak gebruiken in uw toepassing. De SDK biedt de volledige functionaliteit van de service. Het basisproces voor het uitvoeren van spraak vertaling omvat de volgende stappen uit:

1. Maken van een fabriek spraak en geef een sleutel Speech service-abonnement of een verificatietoken. U de talen van de bron en doel vertaling ook op dit moment hebt configureren en opgeven of u wilt dat tekst of gesproken uitvoer.

2. Een kenmerk ophalen van de fabriek. Selecteer een vertaling herkenner voor omzetting. (De andere kenmerken zijn voor *spraak naar tekst*.) Er zijn verschillende versies van de vertaling herkenner op basis van de audio-bron die u gebruikt.

4. Gebeurtenissen voor asynchrone bewerking bezighouden indien gewenst. De herkenner roept de gebeurtenis-handlers wanneer het tussentijdse en definitieve resultaten heeft. Uw toepassing krijgt anders een definitieve vertaling resultaat.

5. Opname en vertaling starten.

# <a name="sdk-samples"></a>SDK-voorbeelden

Zie voor de meest recente set steekproeven van het [cognitieve Services spraak SDK voorbeeld GitHub-opslagplaats](https://aka.ms/csspeech/samples).

# <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Herkent spraak in C#](quickstart-csharp-windows.md)
