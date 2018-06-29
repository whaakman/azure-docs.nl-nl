---
title: Inzicht in gegevens conversie concepten in LUIS - Azure | Microsoft Docs
description: Meer informatie over hoe utterances kan worden gewijzigd voordat voorspellingen in Language Understanding (LUIS)
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/27/2018
ms.author: v-geberr;
ms.openlocfilehash: 16b0df4b81220885e2c3747470272cee9536e10c
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063558"
---
# <a name="data-conversion-concepts-in-luis"></a>Gegevens conversie concepten in LUIS
LUIS biedt een manier gesproken utterances utterances converteren naar tekst utterances voordat voorspelling. 

## <a name="speech-to-intent-conversion-concepts"></a>Spraak naar opzet conversie-concepten
Conversie van spraak naar tekst in LUIS kunt u een eindpunt gesproken utterances verzenden en ontvangen van een voorspelling LUIS antwoord. Het proces is een integratie van de [spraak](https://docs.microsoft.com/azure/cognitive-services/Speech) service met LUIS. 

### <a name="key-requirements"></a>Belangrijke vereisten
U hoeft niet te maken een **Bing Speech-API** sleutel op voor deze integratie. Een **Language Understanding** sleutel gemaakt in de Azure portal is geschikt voor deze integratie. Gebruik niet de sleutel van de starter LUIS, werken niet voor deze integratie.

### <a name="new-endpoint"></a>Nieuw eindpunt 
Deze integratie maakt een nieuw eindpunt en [prijzen](luis-boundaries.md#key-limits) model. Het eindpunt, via de [spraak SDK](https://github.com/Azure-Samples/cognitive-services-speech-sdk), kan zowel gesproken ontvangen en tekst utterances zodat u als één eindpunt te gebruiken. 

### <a name="quota-usage"></a>Quotagebruik
Zie [sleutel limieten](luis-boundaries.md#key-limits) voor meer informatie. 

### <a name="data-retention"></a>Bewaartijd van gegevens
De gegevens verzonden naar het eindpunt, via de SDK-spraak, ongeacht of het spraak of tekst, wordt alleen gebruikt voor het verbeteren van uw model spraak. Het is niet buiten uw model gebruikt voor het verbeteren van spraak of LUIS in een algemeen capaciteit. Wanneer de app LUIS wordt verwijderd, wordt de bewaarde gegevens ook verwijderd.

<!-- TBD: Machine translation conversion concepts -->

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Spraak naar tekst gebruiken](luis-tutorial-speech-to-intent.md)

