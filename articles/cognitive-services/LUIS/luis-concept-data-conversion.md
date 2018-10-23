---
title: Gegevens conversie concepten in LUIS - Language Understanding
titleSuffix: Azure Cognitive Services
description: Informatie over hoe uitingen kunnen worden gewijzigd voordat voorspellingen in Language Understanding (LUIS)
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 9324f7b4f7bed844f16d17b8960878892be4b165
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638389"
---
# <a name="data-conversion-concepts-in-luis"></a>Gegevens conversie concepten in LUIS
LUIS maakt gebruik van Cognitive Services Speech service uitingen van gesproken uitingen converteren naar tekst uitingen voordat voorspelling. 

## <a name="speech-to-intent-conversion-concepts"></a>Spraak naar intentie conversie-concepten
Conversie van spraak naar tekst in LUIS kunt u een eindpunt gesproken uitingen verzenden en ontvangen van een reactie van LUIS voorspelling. Het proces is een integratie van de [spraak](https://docs.microsoft.com/azure/cognitive-services/Speech) service met LUIS. 

### <a name="key-requirements"></a>Belangrijke vereisten
U hoeft niet te maken een **Bing Speech-API** voor deze integratie. Een **Language Understanding** sleutel hebt gemaakt in Azure portal werkt voor deze integratie. Gebruik niet de sleutel van de starter LUIS, dat werkt niet voor deze integratie.

### <a name="new-endpoint"></a>Nieuw eindpunt 
Deze integratie maakt een nieuw eindpunt en [prijzen](luis-boundaries.md#key-limits) model. Het eindpunt via de [spraak SDK](https://github.com/Azure-Samples/cognitive-services-speech-sdk), kan ontvangen beide gesproken en tekst uitingen zodat u kunt gebruiken als één eindpunt. 

### <a name="quota-usage"></a>Gebruik van netwerkquota 's
Zie [sleutel limieten](luis-boundaries.md#key-limits) voor meer informatie. 

### <a name="data-retention"></a>Bewaartijd van gegevens
De gegevens die worden verzonden naar het eindpunt, via de SDK-spraak, ongeacht of het spraak of tekst, wordt alleen gebruikt voor het verbeteren van uw spraakmodel. Het is niet buiten uw model gebruikt voor het verbeteren van spraak of LUIS in een algemene capaciteit. Wanneer de LUIS-app wordt verwijderd, wordt de bewaarde gegevens ook verwijderd.

<!-- TBD: Machine translation conversion concepts -->

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Gebruik spraak naar tekst](luis-tutorial-speech-to-intent.md)

