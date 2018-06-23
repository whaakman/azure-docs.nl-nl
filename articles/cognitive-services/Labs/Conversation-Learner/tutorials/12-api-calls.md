---
title: Gebruik gebruik API-aanroepen met een toepassing conversatie cursist - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik API-aanroepen met een conversatie cursist toepassing gebruiken.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: ec752cbadfac7a47e08ed7b0ffe8bb475969fac5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345292"
---
# <a name="how-to-add-api-calls-to-a-conversation-learner-application"></a>API-aanroepen naar een conversatie cursist-toepassing toevoegen

Deze zelfstudie laat zien hoe API-aanroepen voor uw toepassing toevoegen. API-aanroepen zijn functies die u definieert en schrijft in uw bot en welke conversatie cursist kunt aanroepen.

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist dat de bot 'tutorialAPICalls.ts' wordt uitgevoerd.

    npm run tutorial-api-calls

## <a name="details"></a>Details

- API-aanroepen kunnen lezen en manipuleren van entiteiten.
- API-aanroepen hebben toegang tot het beheerobject voor geheugen.
- API-aanroepen kunnen ook rekening houden met argumenten--hierdoor opnieuw met de dezelfde API-aanroep naar een ander doel.

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst met Apps van de webgebruikersinterface, op de zelfstudie-12-APICalls. 

### <a name="entities"></a>Entiteiten

We hebben één entiteit gedefinieerd in de toepassing met de naam van het nummer.

![](../media/tutorial12_entities.PNG)

### <a name="api-calls"></a>API-aanroepen
De code voor de API-aanroepen is gedefinieerd in dit bestand: C:\<installedpath\>\src\demos\tutorialAPICalls.ts.

![](../media/tutorial12_apicalls.PNG)

- De eerste API-retouraanroep is RandomGreeting. Retourneert een willekeurig begroeting gedefinieerd in de variabele begroeting.
- De API Multiply-callback: deze twee getallen opgegeven door de gebruiker wordt vermenigvuldigen. Vervolgens wordt het resultaat van de vermenigvuldiging van de twee getallen. Dit betekent dat API retouraanroepen duren de invoer voordat kunnen. Houd er rekening mee dat manager geheugen is het eerste argument. 
- De callback ClearEntities API: Hiermee wist u de numerieke entiteit zodat de gebruiker het volgende nummer. Dit wordt geïllustreerd hoe API-aanroepen entiteiten kunnen bewerken.

### <a name="actions"></a>Acties

Wij hebben vier acties gemaakt. 

![](../media/tutorial12_actions.PNG)

- Naast het 'welk nummer wilt u om te vermenigvuldigen met 12?' Dit is een communicative actie, er zijn drie verschillende API aanroepen die aangeven van de typische patronen voor API-aanroep.

- RandomGreeting: is een niet-wait-actie. Om in te stellen dit, in het dialoogvenster actie maken, wordt de actie API_LOCAL geselecteerd en klik vervolgens RandomGreeting geselecteerd. 

![](../media/tutorial12_setupapicall.PNG)

De knop Vernieuwen naast de API wordt gebruikt als we de bot stoppen en wijzigingen aanbrengen in de API's. Klik op Vernieuwen zou kunnen worden opgepikt de meest recente wijzigingen.

Hier ziet u hoe we gemaakt de actie vermenigvuldigen: na het selecteren van API_Local en API we een entiteit ($number) hebt ingevoerd voor de eerste invoerwaarde (num1string) en een waarde (12) voor de tweede invoerwaarde (num2string). Dit biedt een niveau van indirectietabel tussen de bot en de API-aanroepen zodat de callback van dezelfde kan worden toegewezen aan een aantal acties in het systeem en verschillen op hoe de acties worden toegewezen.

![](../media/tutorial12_actionmultiply.PNG)

### <a name="train-dialog"></a>Dialoogvenster Train

We gaan een dialoogvenster onderwijs doorlopen.

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster.
1. Voer 'Hallo'.
2. Klik op de actie Score.
3. Klik op RandomGreeting. Hiermee wordt de willekeurige begroeting API-aanroep uitgevoerd.
3. Schakel 'welke getal en u wilt vermenigvuldigen met 12?'
4. Voer '8'. Klik vervolgens op Score acties.
4. Selecteer ' vermenigvuldigen $number 12'. Let op het resultaat van de vermenigvuldiging.
5. Selecteer 'Wissen entiteiten'.
    - Houd er rekening mee dat de numerieke entiteitswaarde is gewist.
3. Schakel 'welke getal en u wilt vermenigvuldigen met 12?'
4. Klik op gereed testen.

![](../media/tutorial12_dialog.PNG)

U hebt nu registreren API callbacks, gezien hun algemene patronen en het definiëren van argumenten en waarden en entiteiten in ze koppelen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Kaarten, deel 1](./13-cards-1.md)
