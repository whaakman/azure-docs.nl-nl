---
title: Conversatie cursist standaardconfiguratie - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over de standaardconfiguratie van de conversatie cursist.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 56e2140b83bf1c5722a459c14f31b2b4b0ba6b15
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345263"
---
# <a name="default-values-and-boundaries"></a>Standaardwaarden en grenzen

Dit document beschrijft de standaardconfiguratie van de conversatie cursist en grenzen aspecten van de service.

## <a name="default-configuration"></a>Standaardconfiguratie

Parameter | Standaardwaarde
--- | --- 
Standaard sessietime-out | 30 minuten

## <a name="boundaries"></a>Grenzen

Parameter | Limiet
--- | --- 
API ontwerpen, max HTTP-aanroepen per maand | 5M
API ontwerpen, max HTTP-aanroepen per seconde | 25
Sessie-API, Max. HTTP-aanroepen per maand | 500.000
Sessie-API, Max. HTTP-aanroepen per seconde | 10
Maximumaantal aangepaste (niet-programmatische) entiteiten per toepassing | Zie [LUIS grenzen doc](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-boundaries); in de praktijk werkelijke nummer mogelijk iets kleiner
Maximumaantal vooraf samengestelde entiteiten per toepassing | Zie [LUIS grenzen doc](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-boundaries)
Maximumaantal entiteiten (in totaal) per toepassing | 100
Maximumaantal acties per toepassing | 32
Maximumaantal train dialoogvensters per toepassing | 1000
Hiermee schakelt u maximumaantal gebruikers per train dialoogvenster | 100
Maximumaantal logboek dialoogvensters per toepassing | Geen vooraf ingestelde limiet, maar log dialoogvensters blijven alleen behouden voor een bepaalde periode voordat het wordt verwijderd.  Ook wordt de gebruikersinterface van de conversatie cursist 100 logboek dialoogvensters weergegeven op een tijdstip. 
Maximumaantal toepassingen per gebruiker | Geen vooraf ingestelde limiet
Maximumaantal opeenvolgende niet-wait-acties | 5 (*)

(*) Alle niet-wait-acties worden gemaskeerd na 5 opeenvolgende niet-wait-acties, en conversatie cursist kiest onder acties beschikbaar wacht.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aan de slag met de conversatie cursist](./quickstart.md)
