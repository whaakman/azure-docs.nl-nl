---
title: Conversatie Learner Controlestroom - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over de Controlestroom Conversatiecursist.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 8007e9e9fe2f404b4d702471610ff76047f7ed86
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53790925"
---
## <a name="control-flow"></a>Controlestroom

Dit document beschrijft de Controlestroom van de conversatie Learner (CL) zoals weergegeven het onderstaande diagram.

![](media/controlflow.PNG)

1. Gebruiker voert een term of woordgroep in de bot, bijvoorbeeld: 'Wat is het weer in Seattle?'
1. De invoer van de gebruiker naar een model voor machine learning die entiteiten ophaalt CL is geslaagd
    - Dit model bouwen door Conversatiecursist is en die wordt gehost door www.luis.ai
1. Alle entiteiten opgehaald en de gebruiker de invoer van tekst, worden doorgegeven aan de entiteit-detectie terugbelmethode in de code van de bot.
    - Deze code kan entiteitswaarden set/wissen/bewerken
1. Vervolgens wordt de uitvoer van de entiteiten extraheren en de invoer van de gebruiker en alle acties gedefinieerd in de bot-scores CL neurale netwerk
    - In dit voorbeeld is de hoogste kans-actie voor de weersverwachting:

    ![](media/controlflow_forecast.PNG)

1. Een API-aanroep om op te halen van de weersvoorspelling voor de geselecteerde actie, in dit geval vereist. 
1. Deze API is geregistreerd via de CL. AddCallback methode wordt aangeroepen.  Het resultaat van deze API wordt vervolgens geretourneerd naar de gebruiker als een bericht--bijvoorbeeld 'Sunny met een hoge 67.'
1. Vervolgens wordt de aanroep uitgevoerd met het neurale netwerk om te bepalen van de volgende actie op basis van de vorige stap.
1. Het neurale netwerk voorspelt vervolgens de volgende set met mogelijke acties en de geselecteerde actie wordt weergegeven voor de gebruiker, in dit geval 'Iets anders?'

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Hoe om te leren met Conversatiecursist ](./how-to-teach-cl.md)
