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
ms.openlocfilehash: 33869ed71cbcdef454c9fcee8b4e6279ad5dfe05
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796812"
---
# <a name="how-to-add-api-calls-to-a-conversation-learner-model"></a>API-aanroepen naar een model Conversatiecursist toevoegen

In deze zelfstudie laat zien hoe API-aanroepen aan uw model toevoegen. API-aanroepen zijn functies die u definieert en schrijven in uw Bot, en welke Conversatiecursist kunt aanroepen.

## <a name="video"></a>Video

[![Zelfstudie Preview-API-aanroepen](https://aka.ms/cl_Tutorial_v3_APICalls_Preview)](https://aka.ms/cl_Tutorial_v3_APICalls)

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist dat de bot 'tutorialAPICalls.ts' wordt uitgevoerd.

    npm run tutorial-api-calls

## <a name="details"></a>Details

- API-aanroepen kunnen lezen en bewerken van entiteiten.
- API-aanroepen hebben toegang tot het geheugen manager-object.
- API-aanroepen kunnen duren voordat de argumenten.

### <a name="open-the-demo"></a>Open de demo

In de web-UI, klik op "Zelfstudies importeren" en selecteer het model met de naam '-zelfstudie-14-APICalls'.

### <a name="entities"></a>Entiteiten

We hebben een entiteit hebt gedefinieerd in het Model met de naam `number`.

![](../media/tutorial12_entities.PNG)

### <a name="api-calls"></a>API-aanroepen
De code voor de API-aanroepen is gedefinieerd in het bestand: `C:\<installedpath>\src\demos\tutorialAPICalls.ts`.

![](../media/tutorial12_apicalls.PNG)

- De `RandomGreeting` callback retourneert een willekeurig begroeting gedefinieerd in de `greeting` matrix.
- De `Multiply` terugbellen wordt Vermenigvuldigt twee getallen die zijn doorgegeven door de actie die deze en retourneert een resultaat die kan worden weergegeven in de gebruikersinterface.
    - U ziet dat geheugenbeheer is het eerste argument. 
    - U ziet dat de callbacks API duren meerdere invoergegevens, in dit geval voordat kunnen `num1string` en `num2string`.
- De `ClearEntities` callback wist de numerieke entiteit zodat de gebruiker kan een ander nummer invoeren. 
    - Ziet u hoe de API-aanroepen entiteiten kunnen bewerken.

### <a name="actions"></a>Acties
We hebt vier acties gemaakt. Drie van deze 'Niet-Wait' API acties, met de vierde is een 'Tekst'-actie die de gebruiker wordt gevraagd een vraag die vergelijkbaar is met wat we hebben gezien in de andere zelfstudies. Om te zien hoe elk wordt gemaakt op de volgende handelingen uit:
1. In het linkerdeelvenster klikt u op 'Acties' en klik op een van de vier acties die worden vermeld in het raster.
2. U ziet de waarden van elk veld in het formulier dat verschijnt.
3. U ziet dat de `Refresh` knop naast de API-veld.
    - Als we het stoppen van de Bot en wijzigen naar de API's terwijl de UI-pagina is, kunt u op de `Refresh` om de meest recente wijzigingen.

![](../media/tutorial12_actions.PNG)

#### <a name="clearentities-multiply-and-randomgreeting"></a>ClearEntities, vermenigvuldigen en RandomGreeting
Alle drie van de volgende acties zijn API-Type. Ze allemaal zijn afhankelijk van de API-callback-functies uit te voeren werk mogelijk retourneert een waarde die moet worden weergegeven voor de gebruiker.

#### <a name="what-number-do-you-want-to-multiply-by-12"></a>"Welk nummer wilt u vermenigvuldigen met 12"
Dit is de actie 'Tekst' en deze gewoon een kwestie van de gebruiker wordt gevraagd. Terwijl deze actie geen daadwerkelijk interactie met een van de callbacks API hebben, wordt de gebruiker om te reageren met een getal dat in het geheugen van een entiteit die vervolgens kan worden gebruikt door de actie gaat 'Vermenigvuldigen', die maakt gebruik van een van de API-callbacks gevraagd.


### <a name="train-dialog"></a>Dialoogvenster van de trein

We nemen een dialoogvenster"Training'.

1. Klik in het linkerdeelvenster op `Train Dialogs`, dan zal de `New Train Dialog` knop.
2. Type "Hallo".
3. Klik op de knop `Score Actions`.
4. Selecteer `RandomGreeting`. 
    - Hiermee wordt de willekeurige begroeting API-aanroep uitgevoerd.
    - Dit wordt niet wachten op een reactie van gebruiker.
5. Selecteer `What number to do you want to multiply by 12?`
6. Typ een getal, een getal en alleen een getal.
    - U ziet dat het nummer van uw is automatisch gelabeld als de `number` entiteit.
7. Klik op de knop `Score Actions`.
8. Selecteer de `Multiply` actie.
    - U ziet het resultaat van de vermenigvuldiging van 12.
    - U ziet dat het geheugen bevat nog steeds de waarde die u hebt ingevoerd voor `number`
9. Selecteer de `ClearEntities` actie.
    - U ziet dat de entiteit waarde voor de `number` is verwijderd uit het geheugen.
10. Klik op de knop `Save`.

U hebt nu gezien hoe API callbacks, registreren hun algemene patronen en het definiëren van de argumenten en waarden en entiteiten in deze koppelen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Kaarten deel 1](./15-cards.md)
