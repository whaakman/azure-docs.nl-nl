---
title: Het gebruik van API-aanroepen met een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van API-aanroepen met een Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 815d1e9f6d1e4b9937647d55b67665e1b27f501e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240763"
---
# <a name="how-to-add-api-calls-to-a-conversation-learner-model"></a>API-aanroepen naar een model Conversatiecursist toevoegen

In deze zelfstudie laat zien hoe API-aanroepen aan uw model toevoegen. API-aanroepen zijn functies die u definieert en schrijven in uw bot, en welke Conversatiecursist kunt aanroepen.

## <a name="video"></a>Video

[![Zelfstudie-12-Preview](https://aka.ms/cl-tutorial-12-preview)](https://aka.ms/blis-tutorial-12)

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist dat de bot 'tutorialAPICalls.ts' wordt uitgevoerd.

    npm run tutorial-api-calls

## <a name="details"></a>Details

- API-aanroepen kunnen lezen en bewerken van entiteiten.
- API-aanroepen hebben toegang tot het geheugen manager-object.
- API-aanroepen kunnen ook rekening houden met argumenten--Hierdoor kan opnieuw met behulp van de dezelfde API-aanroep voor verschillende doeleinden gebruikt.

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst Model van de web-UI op zelfstudie-12-APICalls. 

### <a name="entities"></a>Entiteiten

We hebben één entiteit in het model met de naam nummer gedefinieerd.

![](../media/tutorial12_entities.PNG)

### <a name="api-calls"></a>API-aanroepen
De code voor de API-aanroepen is gedefinieerd in het bestand: C:\<installedpath\>\src\demos\tutorialAPICalls.ts.

![](../media/tutorial12_apicalls.PNG)

- De eerste API-Callback is RandomGreeting. Retourneert een willekeurig begroeting gedefinieerd in de begroeting-variabele.
- De callback vermenigvuldigen API: deze wordt Vermenigvuldigt twee getallen die zijn opgegeven door de gebruiker. Deze retourneert vervolgens het resultaat van het vermenigvuldigen van de twee getallen. U ziet dat de callbacks API duren de invoer voordat kunnen. Houd er rekening mee dat geheugenbeheer is het eerste argument. 
- De callback ClearEntities API: de numerieke entiteit zodat de gebruiker het volgende nummer wordt gewist. Dit wordt geïllustreerd hoe API-aanroepen entiteiten kunnen bewerken.

### <a name="actions"></a>Acties

We hebt vier acties gemaakt. 

![](../media/tutorial12_actions.PNG)

- Naast het 'welk nummer u wilt vermenigvuldigen met 12?' Dit is een communicative actie, zijn er drie verschillende API aanroepen om de standaard API-aanroep patronen te illustreren.

- RandomGreeting: is een niet-wait-actie. Om in te stellen deze boven in het dialoogvenster actie maken, er geselecteerd van de actie API_LOCAL en RandomGreeting geselecteerd. 

![](../media/tutorial12_setupapicall.PNG)

De vernieuwknop naast de API wordt gebruikt als we het stoppen van de bot, en wijzigingen aanbrengen in de API's. De meest recente wijzigingen zou te klikken op Vernieuwen kiezen.

Hier volgt hoe we gemaakt de actie vermenigvuldigen: na het selecteren van API_Local en -API, we een entiteit ($number) voor de eerste invoerwaarde (num1string) en een waarde (12) voor de tweede invoerwaarde (num2string) hebt ingevoerd. Dit biedt een niveau van de richting tussen de bot en de API-aanroepen, zodat de callback die dezelfde kan worden toegewezen aan een aantal acties in het systeem en deze afwijken op hoe de acties die zijn toegewezen.

![](../media/tutorial12_actionmultiply.PNG)

### <a name="train-dialog"></a>Dialoogvenster van de trein

We nemen een dialoogvenster voor onderwijs.

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein.
1. Voer 'Hallo'.
2. Klik op Score actie.
3. Klik op RandomGreeting. Hiermee wordt de willekeurige begroeting API-aanroep uitgevoerd.
3. Klik om te selecteren 'welke getal en u wilt vermenigvuldigen met 12?'
4. Voer '8'. Klik vervolgens op Score acties.
4. Selecteer ' vermenigvuldigen $number 12'. Houd er rekening mee het resultaat van de vermenigvuldiging.
5. Selecteer 'Wissen entiteiten'.
    - De `number` waarde van de entiteit is gewist.
3. Klik om te selecteren 'welke getal en u wilt vermenigvuldigen met 12?'
4. Klik op gereed testen.

![](../media/tutorial12_dialog.PNG)

U hebt nu gezien hoe API callbacks, registreren hun algemene patronen en het definiëren van de argumenten en waarden en entiteiten in deze koppelen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Kaarten deel 1](./13-cards-1.md)
