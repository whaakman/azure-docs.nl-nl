---
title: Spraak herkend met behulp van de REST-API
titleSuffix: Azure Cognitive Services
description: Informatie over het gebruik van de Speech to Text-API in de Cognitive Services Speech-service.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: erhopf
ms.openlocfilehash: ceb8e929520becd2bf097fefc9c6fe8959b1bf85
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51709811"
---
# <a name="recognize-speech-by-using-the-rest-api"></a>Spraak herkend met behulp van de REST-API

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

De REST-API kan worden gebruikt voor het herkennen van korte uitingen met behulp van een HTTP POST-aanvraag.

De REST-API is de eenvoudigste manier voor het herkennen van spraak als u een taal die wordt ondersteund door de [SDK](speech-sdk.md). U moet een HTTP POST-aanvraag naar de service-eindpunt en de hele utterance in de hoofdtekst van de aanvraag doorgeven. U ontvangt een antwoord met de herkende tekst.

> [!NOTE]
> Uitingen zijn beperkt tot 15 seconden of minder bij het gebruik van de REST-API.
> Bekijk de [spraak SDK](how-to-recognize-speech-csharp.md) voor herkenning van langer uitingen.

Voor meer informatie over de **spraak naar tekst** REST-API, raadpleegt u de [REST-API's](rest-apis.md#speech-to-text-api) artikel. Als u wilt de API in actie zien, downloaden de [REST-API-voorbeelden](https://github.com/Azure-Samples/SpeechToText-REST) vanuit GitHub.

## <a name="next-steps"></a>Volgende stappen

- Zie de [REST API-overzicht](rest-apis.md).
