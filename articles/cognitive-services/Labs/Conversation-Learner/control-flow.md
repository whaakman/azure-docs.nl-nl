---
title: Conversatie cursist Controlestroom - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over de conversatie cursist Controlestroom.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 592ca82db7e0ab3ff89d25b61f38f8b226f3bc86
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345258"
---
## <a name="control-flow"></a>Controlestroom

Dit document beschrijft de Controlestroom van de conversatie cursist (CL), zoals weergegeven het onderstaande diagram.

![](media/controlflow.PNG)

1. Gebruiker voert een term of woordgroep in de bot, bijvoorbeeld 'Wat is het weer in Haarlem?'
1. CL wordt doorgegeven voor de invoer van de gebruiker naar een machine learning-model waarmee entiteiten worden geëxtraheerd
    - Dit model bouwen door conversatie cursist is, en wordt gehost door www.luis.ai
2. Alle entiteiten uitgepakt en de gebruikersinvoer tekst, worden doorgegeven aan de retouraanroepmethode entiteit detectie in de bot-code.
    - Deze code kan waarden entiteit instellen, wissen/bewerken
1. Vervolgens wordt de uitvoer van de extractie van de entiteit de invoer van gebruiker en alle acties gedefinieerd in de bot scores CL neurale netwerk
    - In dit voorbeeld is de hoogste kans actie voor de prognose weer:

![](media/controlflow_forecast.PNG)

5. De geselecteerde actie vereist in dit geval een API-aanroep voor het ophalen van de prognose weer. 
6. Deze API is geregistreerd via de CL. De methode AddAPICallback, vervolgens wordt aangeroepen.  Het resultaat van deze API wordt vervolgens geretourneerd naar de gebruiker als een bericht--bijvoorbeeld 'Sunny met een hoge 67'.
7. Vervolgens wordt de aanroep uitgevoerd met het neurale netwerk om te bepalen van de volgende actie op basis van de vorige stap.
8. Het neurale netwerk voorspelt vervolgens de volgende reeks mogelijke acties en de geselecteerde actie is gepresenteerd aan de gebruiker in dit geval 'Iets anders?'

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Hoe u met de conversatie cursist leren ](./how-to-teach-cl.md)
