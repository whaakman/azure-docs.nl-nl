---
title: Aangepaste Service spraak meters en quota's in Azure | Microsoft Docs
description: Informatie over meters en quota's van aangepaste spraak-Service op Azure.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/08/2017
ms.author: panosper
ms.openlocfilehash: d2225dec818c600febfad2f9ebc42594f6ac09ac
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344437"
---
# <a name="custom-speech-service-meters-and-quotas"></a>Aangepaste Service spraak meters en quota 's

Met aangepaste spraak Cloudservice, kunt u spraak modellen voor spraak-naar-tekst schrijffouten aanpassen.

Om te beginnen met de Service voor aangepaste spraak, gaat u naar de [aangepaste spraak-serviceportal](https://cris.ai).

Prijzen voor de huidige meters, gaat u naar de [cognitieve Services prijzen voor aangepaste spraak Service](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/) pagina.

## <a name="tiers-explained"></a>Lagen uitgelegd
Voor het testen en alleen prototype voorstellen we gebruiken de laag gratis F0. Voor productiesystemen stellen we gebruiken de laag S2. U kunt met behulp van de laag S2 uw implementatie aan het aantal schaaleenheden (SUs) die uw scenario vereist schalen.

> [!NOTE]
> U *kan niet* migreren tussen de F0-laag en de S2 laag.
>

## <a name="meters-explained"></a>Meters uitgelegd

### <a name="scale-out"></a>Opwaarts schalen
Scale-Out is een nieuwe functie die we met het nieuwe prijsmodel uitgebracht. U kunt het aantal gelijktijdige aanvragen op dat kan worden verwerkt door het model beheren met behulp van een Scale-Out.

U kunt gelijktijdige aanvragen instellen met behulp van de meting SU in de **Model implementatie maken** weergeven. Zie voor meer informatie [Maak een aangepaste spraak naar tekst eindpunt](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-endpoint.md). Afhankelijk van de hoeveelheid verkeer die u voorzien in het model gebruiken, kunt u een geschikt aantal SUs. 

> [!NOTE]
> Elke schaaleenheid garandeert 5 gelijktijdige aanvragen. U kunt kopen 1 of meer SUs, naar gelang van toepassing. Omdat het aantal SUs in stappen van 1 stijgt, moet het aantal gelijktijdige aanvragen zijn gegarandeerd worden verhoogd met intervallen van 5.
>

### <a name="log-management"></a>Log-beheer
U kunt ervoor kiezen om uitschakelen audio traceringen voor een geïmplementeerde model een extra kosten. Aangepaste spraak-Service wordt niet geregistreerd de audio aanvragen of de transcripties van dit model.

## <a name="next-steps"></a>Volgende stappen
Ga voor meer informatie over het gebruik van de aangepaste spraak-Service naar de [aangepaste spraak-serviceportal](https://cris.ai).

* [Aan de slag](cognitive-services-custom-speech-get-started.md)
* [Veelgestelde vragen](cognitive-services-custom-speech-faq.md)
* [Woordenlijst](cognitive-services-custom-speech-glossary.md)
 