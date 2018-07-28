---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: 4b823a8dc15d33e0537fae348b34c3a3fee84558
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39330817"
---
<!-- N.B. no header, language-agnostic -->

De [spraak SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) biedt de eenvoudigste manier om het gebruik van de vertaling van gesproken tekst in uw toepassing.
De SDK biedt de volledige functionaliteit van de service.
Het basisproces voor het uitvoeren van de vertaling van gesproken tekst bevat de volgende stappen uit:

1. Maken van een factory spraak, bieden een abonnementssleutel voor spraak-service en een [regio](~/articles/cognitive-services/speech-service/regions.md).
   U ook configureren voor de bron- en talen voor tekstvertaling, evenals op te geven of u wilt dat SMS of spraak uitvoer.

1. Een NAT-herkenning van de fabriek voor spraak ophalen
   Er zijn verschillende versies van de vertaling kenmerken op basis van de audio bron die u gebruikt.

1. De gebeurtenissen voor asynchrone bewerking bezighouden indien gewenst.
   De gebeurtenis-handlers de herkenning wordt aangeroepen wanneer er een tijdelijke en laatste resultaten.
   Anders ontvangt uw toepassing een definitieve transcriptie-resultaat.

1. Herkenning en vertaling starten.

Hieronder laten we zien verschillende codefragmenten voor spraak vertaling scenario's met de spraak-SDK.

[!include[Get a Subscription Key](cognitive-services-speech-service-get-subscription-key.md)]
