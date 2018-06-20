---
title: Inzicht in gegevens conversie concepten in LUIS - Azure | Microsoft Docs
description: Meer informatie over hoe utterances kan worden gewijzigd voordat voorspellingen in Language Understanding (LUIS)
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: 56de113b41be419a5e39d705a22466139c1c29ce
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266093"
---
# <a name="data-conversion-concepts-in-luis"></a>Gegevens conversie concepten in LUIS
LUIS biedt een manier gesproken utterances utterances converteren naar tekst utterances voordat voorspelling. 

## <a name="speech-to-intent-conversion-concepts"></a>Spraak naar opzet conversie-concepten
Conversie van spraak naar tekst in LUIS kunt u een eindpunt gesproken utterances verzenden en ontvangen van een voorspelling LUIS antwoord. Het proces is een integratie van de [spraak](https://docs.microsoft.com/azure/cognitive-services/Speech) service met LUIS. 

### <a name="key-requirements"></a>Belangrijke vereisten
U hoeft niet te maken een **Bing Speech-API** sleutel op voor deze integratie. De sleutel LUIS werkt voor deze integratie.

### <a name="new-endpoint"></a>Nieuw eindpunt 
Deze integratie maakt een nieuw eindpunt en [prijzen](luis-boundaries.md#key-limits) model. Het eindpunt is kunnen beide gesproken ontvangen en tekst utterances zodat u als één eindpunt te gebruiken. 

### <a name="quota-usage"></a>Quotagebruik
Zie [sleutel limieten](luis-boundaries.md#key-limits) voor meer informatie. 

### <a name="data-retention"></a>Bewaartijd van gegevens
De gegevens naar het eindpunt verzonden, ongeacht of het spraak of tekst, wordt alleen gebruikt voor het verbeteren van uw model spraak. Het is niet buiten uw model gebruikt voor het verbeteren van spraak of LUIS in een algemeen capaciteit. Wanneer de app LUIS wordt verwijderd, wordt de bewaarde gegevens ook verwijderd.

<!-- TBD: Machine translation conversion concepts -->

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Juiste spellingcontrole uit te voeren in deze zelfstudie](luis-tutorial-bing-spellcheck.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions