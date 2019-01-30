---
title: Aangepaste Spraakservice meters en quota's voor Azure | Microsoft Docs
description: Informatie over meters en quota's van Custom Speech Service op Azure.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 07/08/2017
ms.author: panosper
ms.openlocfilehash: eb5a9e4a3a27a2a8c044749b8b4df0f198583bde
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55223660"
---
# <a name="custom-speech-service-meters-and-quotas"></a>Aangepaste Spraakservice meters en quota

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Met cloud-gebaseerde Custom Speech Service, kunt u spraakmodellen voor transcriptie van spraak naar tekst aanpassen.

Om te beginnen met behulp van de Custom Speech Service gaat u naar de [Custom Speech Service portal](https://cris.ai).

Prijzen voor de huidige meters, Ga naar de [prijzen voor Cognitive Services voor Custom Speech Service](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/) pagina.

## <a name="tiers-explained"></a>Lagen uitgelegd
Voor het testen en prototype alleen stellen we de gratis laag van F0 gebruiken. Voor productiesystemen stellen we de S2-laag gebruiken. U kunt uw implementatie om het aantal schaaleenheden (su's) die uw scenario moet schalen met behulp van de S2-laag.

> [!NOTE]
> U *kan geen* migreren tussen de F0-laag en de S2-laag.
>

## <a name="meters-explained"></a>Meters uitgelegd

### <a name="scale-out"></a>Opwaarts schalen
Uitschalen is een nieuwe functie die we hebben uitgebracht met het nieuwe prijsmodel. Met behulp van uitschalen, kunt u bepalen het aantal gelijktijdige aanvragen waarmee uw model kan worden verwerkt.

U kunt gelijktijdige aanvragen instellen met behulp van de meting SU in de **Modelimplementatie maken** weergeven. Zie voor meer informatie, [maken van een aangepaste spraak-naar-tekst-eindpunt](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-endpoint.md). Afhankelijk van de hoeveelheid netwerkverkeer die u voorzien in het model verbruikt, kunt u een geschikt aantal su's. 

> [!NOTE]
> Elke schaaleenheid garandeert 5 gelijktijdige aanvragen. 1 of meer SUs, waar nodig, kunt u kopen. Omdat het aantal su's in stappen van 1 stijgt, wordt het aantal gelijktijdige aanvragen worden gegarandeerd te verhogen in stappen van 5.
>

### <a name="log-management"></a>Logboekbeheer
U kunt kiezen uit de audio traceringen voor een geïmplementeerde model een extra kosten te schakelen. Met Custom Speech Service worden geen logboekgebeurtenissen de audio aanvragen of de Transcripten van dit model.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over het gebruik van de Custom Speech Service gaat u naar de [Custom Speech Service portal](https://cris.ai).

* [Aan de slag](cognitive-services-custom-speech-get-started.md)
* [Veelgestelde vragen](cognitive-services-custom-speech-faq.md)
* [Woordenlijst](cognitive-services-custom-speech-glossary.md)
 
