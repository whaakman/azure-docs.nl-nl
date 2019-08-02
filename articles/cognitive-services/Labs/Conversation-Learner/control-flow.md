---
title: Conversation Learner controle stroom-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over de Conversation Learner controle stroom.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 3ec839c1a930ffbe73989149360f1b02866a3c50
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705315"
---
## <a name="control-flow"></a>Controlestroom

In dit document wordt de controle stroom van de Conversation Learner (CL) beschreven, zoals weer gegeven in het onderstaande diagram.

![](media/controlflow.PNG)

1. De gebruiker voert in de bot een term of woord groep in, bijvoorbeeld ' wat is het weer in Seattle? '
1. LC geeft de invoer van de gebruiker door aan een machine learning model waarmee entiteiten worden geëxtraheerd
   - Dit model is gebaseerd op Conversation Learner en wordt gehost door www.luis.ai
1. Alle geëxtraheerde entiteiten en de invoer tekst van de gebruiker worden door gegeven aan de methode voor het terugbellen van entiteits detectie in de bot-code.
    - Met deze code kunnen entiteits waarden worden ingesteld/gewist of gemanipuleerd
1. LC Neural-netwerk neemt vervolgens de uitvoer van de entiteits extractie en de gebruikers invoer en verlaagt alle acties die zijn gedefinieerd in de bot
   - In dit voor beeld is de hoogste waarschijnlijke actie om de weers voorspelling te bieden:

     ![](media/controlflow_forecast.PNG)

1. Voor de geselecteerde actie is in dit geval een API-aanroep vereist om de weers voorspelling op te halen. 
1. Deze API, die is geregistreerd met behulp van de CL. Methode AddCallback wordt aangeroepen.  Het resultaat van deze API wordt vervolgens door gegeven aan de gebruiker als een bericht, bijvoorbeeld ' zonnig met een hoge van 67 '.
1. De aanroep wordt vervolgens gemaakt naar het Neural-netwerk om de volgende actie te bepalen op basis van de vorige stap.
1. Het Neural-netwerk wordt vervolgens voor speld op de volgende set mogelijke acties, waarna de geselecteerde actie wordt weer gegeven aan de gebruiker, in dit geval ' iets anders '? '

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Leren met Conversation Learner](./how-to-teach-cl.md)
