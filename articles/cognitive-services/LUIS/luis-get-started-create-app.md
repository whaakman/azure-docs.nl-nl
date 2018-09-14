---
title: Uw eerste LUIS-app (Language Understanding) maken in 10 minuten - Cognitive Services-LUIS | Microsoft Docs
description: In deze snelstart maakt u een LUIS-app die gebruikmaakt van het vooraf gemaakte domein `HomeAutomation` voor het in- en uitschakelen van lichten en apparaten. Dit vooraf gemaakte domein biedt u intenties, entiteiten voorbeelden van utterances. Wanneer u klaar bent, hebt u een LUIS-eindpunt in de cloud.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/22/2018
ms.author: diberry
ms.openlocfilehash: 457f23936dec0cf85e9aebbf3e54bba37c2f3ca3
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/23/2018
ms.locfileid: "43769940"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Snelstart: Een vooraf gemaakte app voor huisautomatisering gebruiken

In deze snelstart maakt u een LUIS-app die gebruikmaakt van het vooraf gemaakte domein `HomeAutomation` voor het in- en uitschakelen van lichten en apparaten. Dit vooraf gemaakte domein biedt u intenties, entiteiten voorbeelden van utterances. Wanneer u klaar bent, hebt u een LUIS-eindpunt in de cloud.

## <a name="prerequisites"></a>Vereisten

Voor dit artikel hebt u een gratis LUIS-account nodig, dat u kunt maken in de LUIS-portal op [http://www.luis.ai](http://www.luis.ai). 

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

Selecteer **Intents** in het navigatiedeelvenster aan de linkerkant om de intenties van het domein 'Home Automation' te controleren. 

[![](media/luis-quickstart-new-app/home-automation-intents.png "Schermopname van lijst met intenties met de namen van intenties gemarkeerd in de tabel")](media/luis-quickstart-new-app/home-automation-intents.png)

Elke intentie bevat voorbeelden van utterances.

> [!NOTE]
> **None** is een intentie die door alle LUIS-apps wordt geboden. U kunt deze gebruiken voor het afhandelen van utterances die niet overeenkomen met de functionaliteit die uw app biedt. 

Selecteer de intentie **HomeAutomation.TurnOff**. U kunt zien dat de intentie een lijst met utterances bevat die zijn gelabeld met entiteiten.

[![](media/luis-quickstart-new-app/home-automation-turnon.png "Schermopname van de intentie HomeAutomation.TurnOff")](media/luis-quickstart-new-app/home-automation-turnon.png)

## <a name="train-your-app"></a>Uw app trainen

Selecteer **Train** in de bovenste navigatiebalk.

[![](media/luis-quickstart-new-app/trained.png "Schermopname van de intentie HomeAutomation.TurnOff met melding dat trainen is gelukt (groen)")](media/luis-quickstart-new-app/trained.png)

## <a name="test-your-app"></a>Uw app testen
Nadat u uw app hebt getraind, kunt u deze testen. Selecteer **Test** in de bovenste navigatiebalk. Typ een test-utterance, zoals 'Turn off the lights' in het interactieve testdeelvenster en druk op Enter. 

```
Turn off the lights
```

Controleer of de best scorende intentie overeenkomt met de intentie die u verwachtte voor elke test-utterance.

In dit voorbeeld wordt 'Turn off the lights' correct geïdentificeerd als de best scorende intentie van 'HomeAutomation.TurnOff'.

[![](media/luis-quickstart-new-app/test.png "Schermafbeelding van deelvenster Test met utterance gemarkeerd")](media/luis-quickstart-new-app/test.png)


Selecteer **Test** nogmaals om het deelvenster samen te vouwen. 

## <a name="publish-your-app"></a>Uw app publiceren
Selecteer **Publish** in de bovenste navigatiebalk. 

[![](media/luis-quickstart-new-app/publish.png "Schermopname van app met de knop Publish gemarkeerd")](media/luis-quickstart-new-app/publish.png)

Selecteer de slot Production en vervolgens de knop **Publish**.

In de groene meldingsbalk boven aan wordt aangegeven dat de app is gepubliceerd.

[![](media/luis-quickstart-new-app/published.png "Schermopname van app met de melding dat de app is gepubliceerd")](media/luis-quickstart-new-app/published.png)

Nadat het publiceren is voltooid, kunt u de eindpunt-URL gebruiken die wordt weergegeven in de **app Publish**.

[![](media/luis-quickstart-new-app/endpoint.png "Schermopname van de pagina Publish met eindpunt-URL gemarkeerd")](media/luis-quickstart-new-app/endpoint.png)

## <a name="use-your-app"></a>Uw app gebruiken
U kunt uw gepubliceerde eindpunt testen in een browser met behulp van de gegenereerde URL. Open deze URL in uw browser en stel de URL-parameter '&q' in op uw testquery. Voeg bijvoorbeeld `turn off the living room light` aan het einde van uw URL toe en druk op Enter. Het JSON-antwoord van uw HTTP-eindpunt wordt weergegeven in de browser.


[![](media/luis-quickstart-new-app/turn-off-living-room.png "Schermafbeelding van browser met JSON-resultaat waarin de intentie TurnOff is gedetecteerd")](media/luis-quickstart-new-app/turn-off-living-room.png)

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u de LUIS-app niet meer nodig hebt, kunt u deze verwijderen. Selecteer hiervoor de drie puntjes (***...***) rechts van de app-naam in de lijst met apps en selecteer vervolgens **Verwijderen**. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.

## <a name="next-steps"></a>Volgende stappen

U kunt het eindpunt aanroepen vanuit code:

> [!div class="nextstepaction"]
> [Een LUIS-eindpunt aanroepen met behulp van code](luis-get-started-cs-get-intent.md)
