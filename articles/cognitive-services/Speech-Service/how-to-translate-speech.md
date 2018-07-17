---
title: Spraak met behulp van spraakservices vertalen | Microsoft Docs
description: Meer informatie over het gebruik van de vertaling van gesproken tekst in de Speech-service.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: v-jerkin
ms.openlocfilehash: 6acfcf0ae8ab4c63e6cc943f93da6b947f3d118c
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39071093"
---
# <a name="translate-speech-using-speech-service"></a>Spraak met behulp van de spraakservice vertalen

De [spraak SDK](speech-sdk.md) is de eenvoudigste manier om het gebruik van de vertaling van gesproken tekst in uw toepassing. De SDK biedt de volledige functionaliteit van de service. Het basisproces voor het uitvoeren van de vertaling van gesproken tekst bevat de volgende stappen uit:

1. Maken van een factory spraak en bieden een abonnementssleutel voor spraak-service en [regio](regions.md) of een verificatietoken. U de bron- en talen voor tekstvertaling ook op dit moment hebt configureren en op te geven of u wilt dat SMS of spraak uitvoer.

2. Ontvang een kenmerk van de factory. Voor de vertaling, selecteer een NAT-herkenning. (De andere kenmerken zijn voor *spraak naar tekst*.) Er zijn verschillende versies van het NAT-herkenning op basis van de audio bron die u gebruikt.

4. Gebeurtenissen voor asynchrone bewerking bezighouden indien gewenst. De herkenning roept de gebeurtenis-handlers wanneer het tussentijdse en definitieve resultaat heeft. Anders ontvangt uw toepassing een definitieve vertaling-resultaat.

5. Herkenning en vertaling starten.

# <a name="sdk-samples"></a>SDK-voorbeelden

Zie voor de meest recente set voorbeelden, de [Cognitive Services Speech SDK voorbeeld GitHub-opslagplaats](https://aka.ms/csspeech/samples).

# <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Water in C#](quickstart-csharp-dotnet-windows.md)
