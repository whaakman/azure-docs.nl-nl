---
title: Nog 10 minuten tot uw eerste LUIS-app
titleSuffix: Azure Cognitive Services
description: Maak een LUIS-app die gebruikmaakt van het vooraf gemaakte domein `HomeAutomation` voor het in- en uitschakelen van lichten en apparaten. Dit vooraf gemaakte domein biedt u intenties, entiteiten voorbeelden van utterances. Wanneer u klaar bent, hebt u een LUIS-eindpunt in de cloud.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: d7c80cce3b046bb6c8e4412272c7d166bc038d9c
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282115"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Snelstart: Een vooraf gemaakte app voor huisautomatisering gebruiken

In deze snelstart maakt u een LUIS-app die gebruikmaakt van het vooraf gemaakte domein `HomeAutomation` voor het in- en uitschakelen van lichten en apparaten. Dit vooraf gemaakte domein biedt u intenties, entiteiten voorbeelden van utterances. Wanneer u klaar bent, hebt u een LUIS-eindpunt in de cloud.

## <a name="prerequisites"></a>Vereisten

Voor dit artikel hebt u een gratis LUIS-account nodig, dat u kunt maken in de LUIS-portal op [https://www.luis.ai](https://www.luis.ai). 

## <a name="create-a-new-app"></a>Een nieuwe app maken
U kunt uw toepassingen maken en beheren in **Mijn apps**. 

1. Meld u aan bij de LUIS-portal.

2. Selecteer **Create new app**.

    [![](media/luis-quickstart-new-app/app-list.png "Schermopname van de lijst met apps")](media/luis-quickstart-new-app/app-list.png)

3. Geef in het dialoogvenster uw toepassing de naam 'Home Automation'.

    [![](media/luis-quickstart-new-app/create-new-app-dialog.png "Schermopname van het pop-upvenster voor de naam van een nieuwe app")](media/luis-quickstart-new-app/create-new-app-dialog.png)

4. Kies de cultuur voor uw toepassing. Kies voor deze Home Automation-app de optie Engels. Selecteer vervolgens **Done**. De Home Automation-app wordt door LUIS gemaakt. 

    >[!NOTE]
    >De cultuur kan niet worden gewijzigd nadat de toepassing is gemaakt. 

## <a name="add-prebuilt-domain"></a>Vooraf gemaakt domein toevoegen

Selecteer **Prebuilt domains** in het navigatiedeelvenster aan de linkerkant. Zoek vervolgens naar 'Home'. Selecteer **Add domain**.

[![](media/luis-quickstart-new-app/home-automation.png "Schermafbeelding van het domein 'Home Automation' opgeroepen in het menu met vooraf gemaakte domeinen")](media/luis-quickstart-new-app/home-automation.png)

Wanneer het vooraf gemaakte domein is toegevoegd, wordt in het vak met vooraf gemaakte domeinen de knop **Remove domain** weergegeven.

[![](media/luis-quickstart-new-app/remove-domain.png "Schermafbeelding van het domein 'Home Automation' met de knop Remove")](media/luis-quickstart-new-app/remove-domain.png)

## <a name="intents-and-entities"></a>Intenties en entiteiten

Selecteer **Intents** in het navigatiedeelvenster aan de linkerkant om de intenties van het domein 'Home Automation' te controleren. Elke intentie bevat voorbeelden van utterances.

> [!NOTE]
> **None** is een intentie die door alle LUIS-apps wordt geboden. U kunt deze gebruiken voor het afhandelen van utterances die niet overeenkomen met de functionaliteit die uw app biedt. 

Selecteer de intentie **HomeAutomation.TurnOff**. U kunt zien dat de intentie een lijst met utterances bevat die zijn gelabeld met entiteiten.

[![](media/luis-quickstart-new-app/home-automation-turnon.png "Schermopname van de intentie HomeAutomation.TurnOff")](media/luis-quickstart-new-app/home-automation-turnon.png)

## <a name="train-the-luis-app"></a>LUIS-app trainen

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-your-app"></a>Uw app testen
Nadat u uw app hebt getraind, kunt u deze testen. Selecteer **Test** in de bovenste navigatiebalk. Typ een test-utterance, zoals 'Turn off the lights' in het interactieve testdeelvenster en druk op Enter. 

```
Turn off the lights
```

Controleer of de best scorende intentie overeenkomt met de intentie die u verwachtte voor elke test-utterance.

In dit voorbeeld wordt 'Turn off the lights' correct geïdentificeerd als de best scorende intentie van 'HomeAutomation.TurnOff'.

[![](media/luis-quickstart-new-app/test.png "Schermafbeelding van deelvenster Test met utterance gemarkeerd")](media/luis-quickstart-new-app/test.png)


Selecteer **Test** nogmaals om het deelvenster samen te vouwen. 

<a name="publish-your-app"></a>

## <a name="publish-the-app-to-get-the-endpoint-url"></a>App publiceren om eindpunt-URL op te vragen

[!INCLUDE[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Eindpunt opvragen met een andere utterance

1. [!INCLUDE[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Ga naar het einde van de URL in het adres, voer `turn off the living room light` in en druk op Enter. Het JSON-antwoord van uw HTTP-eindpunt wordt weergegeven in de browser.

    [![](media/luis-quickstart-new-app/turn-off-living-room.png "Schermafbeelding van browser met JSON-resultaat waarin de intentie TurnOff is gedetecteerd")](media/luis-quickstart-new-app/turn-off-living-room.png)
    
## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

U kunt het eindpunt aanroepen vanuit code:

> [!div class="nextstepaction"]
> [Een LUIS-eindpunt aanroepen met behulp van code](luis-get-started-cs-get-intent.md)
