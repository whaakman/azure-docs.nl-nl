---
title: Conversatie Learner standaardconfiguratie - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over de standaardconfiguratie voor Conversatiecursist.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 8050008bbae4a23f09b5fa94874a6315e798b448
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55211284"
---
# <a name="default-values-and-boundaries"></a>Standaardwaarden en -grenzen

Dit document beschrijft de standaardconfiguratie van Conversatiecursist en belangrijke servicegrenzen.

## <a name="default-configuration"></a>Standaardconfiguratie

Parameter | Standaardwaarde
--- | --- 
Standaard sessietime-out | 30 minuten

## <a name="boundaries"></a>Grenzen

Parameter | Limiet
--- | --- 
API ontwerpen, max HTTP-aanroepen per maand | 5 MIN.
API ontwerpen, max HTTP-aanroepen per seconde | 25
Sessie-API, max HTTP-aanroepen per maand | 500.000
Sessie-API, max HTTP-aanroepen per seconde | 10
Maximumaantal aangepaste (niet-programmatische) entiteiten per model | Zie [LUIS grenzen doc](https://docs.microsoft.com/azure/cognitive-services/luis/luis-boundaries); in de praktijk werkelijke nummer is mogelijk iets kleinere
Maximumaantal vooraf gemaakte entiteiten per model | Zie [LUIS grenzen doc-bestand](https://docs.microsoft.com/azure/cognitive-services/luis/luis-boundaries)
Maximumaantal per model-entiteiten (in totaal) | 100
Maximumaantal acties per model | 32
Maximumaantal van de trein dialogen per model | 1000
Hiermee schakelt u maximumaantal gebruikers per dialoogvenster van de trein | 100
Maximumaantal log dialoogvensters per model | Er is geen vooraf ingestelde limiet, maar log dialoogvensters worden alleen bewaard voor een bepaalde periode voordat het wordt verwijderd.  Ook wordt de gebruikersinterface van de cursist conversatie 100 log dialoogvensters weergeven op een tijdstip. 
Maximumaantal modellen per gebruiker | Geen vooraf ingestelde limiet
Maximumaantal opeenvolgende acties voor niet-wait | 5 (*)

(*) Alle acties voor niet-wait worden gemaskeerd na 5 opeenvolgende niet-wait-acties en Conversatiecursist, kiest u onder acties beschikbaar wachten.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aan de slag met Conversatiecursist](./quickstart.md)
