---
title: 'Quickstart: Nieuwe app maken met LUIS-Portal'
titleSuffix: Language Understanding - Azure Cognitive Services
description: In deze quickstart maakt u een nieuwe app in de portal LUIS. De basisonderdelen van een app, intenties en entiteiten maken en vervolgens testen door te geven een voorbeeld van gebruiker utterance in het deelvenster interactieve test om op te halen van de voorspelde intentie. Het bouwen van een app is gratis; Er is een Azure-abonnement geen vereist.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 03/11/2019
ms.author: diberry
ms.openlocfilehash: e97be28261d28c2a72e507adcdac0248691745c7
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57783167"
---
# <a name="quickstart-create-a-new-app-in-the-luis-portal"></a>Quickstart: Een nieuwe app maken in de portal LUIS

In deze snelstartgids maakt u een nieuwe app in de [LUIS portal](https://www.luis.ai). Maken van de basisonderdelen van een app **intents** en **entiteiten**, klikt u vervolgens testen door te geven een voorbeeld van gebruiker utterance in het deelvenster interactieve test om op te halen van de voorspelde intentie.

Het bouwen van een app is gratis; Er is een Azure-abonnement geen vereist. Wanneer u klaar bent voor uw app, implementeert u een Azure-Resource voor Cognitive Service maken en deze toewijzen aan de app. Dit proces wordt de [volgende snelstartgids](get-started-portal-deploy-app.md).

## <a name="create-app"></a>App maken 

1. Open de [LUIS portal](https://www.luis.ai) in een browser en meld u aan. Als dit de eerste keer aanmeldt, moet u een gratis LUIS-portal-gebruikersaccount maken.

1. Selecteer **nieuwe app maken** via de werkbalk context.

    [![Nieuwe app maken in de portal van LUIS](./media/get-started-portal-build-app/create-app-in-portal.png)](./media/get-started-portal-build-app/create-app-in-portal.png#lightbox)

1. In het pop-upvenster, configureert u de app met de volgende instellingen en selecteer vervolgens **gedaan**.

    |Naam van instelling| Value | Doel|
    |--|--|--|
    |Name|`myEnglishApp`|De unieke naam van LUIS-app<br>vereist|
    |Culture|**Engels**|Taal van uitingen van gebruikers, **en-us**<br>vereist|
    |Description|`App made with LUIS Portal`|Beschrijving van app<br>optioneel|

    ![Voer de nieuwe appinstellingen](./media/get-started-portal-build-app/create-new-app-settings.png)


## <a name="create-intent"></a>Intent maken 

Nadat u deze app is gemaakt, wordt de volgende stap is het maken van intents. Intents zijn een manier voor het categoriseren van tekst van gebruikers. Als u hebt een human resources-app met twee functies: eerst zorgen ervoor dat personen zoeken en toepassen voor taken en het tweede zoeken formulieren om toe te passen voor taken, deze twee verschillende _bedoelingen_ uitgelijnd met de volgende intenties:

|Intentie|Van de voorbeeldtekst van gebruiker<br>ook wel een _utterance_|
|--|--|
|ApplyForJob|`I want to apply for the new software engineering position in Cairo.`|
|FindForm|`Where is the job transfer form hrf-123456?`|

1. Nadat de app is gemaakt, bent u op de **Intents** pagina van de **bouwen** sectie. Selecteer **Create new intent**. 

    [![Selecteer de nieuwe intentie knop maken](./media/get-started-portal-build-app/create-new-intent-button.png)](./media/get-started-portal-build-app/create-new-intent-button.png#lightbox)

1. Voer de naam van de intentie `FindForm` en selecteer **gedaan**.

    ![Voer de naam van de intentie van FindForm](./media/get-started-portal-build-app/create-new-intent-dialog.png)

## <a name="add-example-utterance"></a>Voorbeeld utterance toevoegen 

Na het maken van het doel, wordt de volgende stap is het voorbeeld utterances toevoegen. Dit is de tekst, ingevoerd door de gebruiker in een chatbot of andere clienttoepassing, die de bedoeling van de tekst van de gebruiker wordt toegewezen aan een LUIS-doel. 

Voor deze voorbeeldtoepassing `FindForm` intentie, voorbeeld-uitingen bevat het aantal formulier, die belangrijke informatie in de utterance dat de clienttoepassing nodig om fullfil verzoek van de gebruiker heeft. 

Toevoegen van de volgende vijftien voorbeeld uitingen naar de `FindForm` intentie. 

|#|Voorbeelden van utterances|
|--|--|
|1|Op zoek naar hrf 123456|
|2|Waar is de hrf formulier human resources-234591?|
|3|hrf-345623, waar is die|
|4|Is het mogelijk om Stuur mij hrf 345794|
|5|Moet ik hrf-234695 om toe te passen voor een interne taak?|
|6|Moet mijn manager ik ben wordt toegepast voor een taak met hrf 234091 weten|
|7|Waar kan ik hrf 234918 verzenden? Krijg ik een antwoord per e-mail die is ontvangen?|
|8|hrf-234555|
|9|Wanneer is hrf 234987 bijgewerkt?|
|10|Gebruik ik formulier hrf-876345 om toe te passen voor technische functies|
|11|Er is een nieuwe versie van hrf-765234 ingediend voor Mijn open vereist?|
|12|Gebruik ik hrf 234234 voor internationale taken?|
|13|hrf 234598 spelling fout|
|14|hrf 234567 wordt worden bewerkt voor nieuwe vereisten|
|15|hrf 123456, hrf 123123, hrf 234567|

Deze voorbeeld-uitingen variëren opzet, in de volgende manieren:

* lengte van utterance
* Leestekens
* Word keuze
* term tegenwoordige (is, is, is)
* de woordvolgorde

[![Voorbeeld-uitingen voor het doel FindForm invoeren](./media/get-started-portal-build-app/add-example-utterance.png)](./media/get-started-portal-build-app/add-example-utterance.png#lightbox)

## <a name="create-regular-expression-entity"></a>Reguliere expressie entiteit maken 

Om het formulier dat wordt geretourneerd als onderdeel van het antwoord van de voorspellings-runtime, heeft de vorm moet worden gemarkeerd als een entiteit. Omdat de tekst van het formulier is zeer gestructureerd, kan worden gemarkeerd met een reguliere expressie-entiteit. De entiteit maken met de volgende stappen uit. 

1. Selecteer **entiteiten** in het menu van de navigatie aan de linkerkant. 

1. Selecteer **nieuwe entiteit maken** op de pagina entiteiten.

1. Voer de naam `Human Resources Form Number`, selecteer de **Regex** entiteit, te typen en de reguliere expressie `hrf-[0-9]{6}`. Dit komt overeen met de letterlijke tekens, `hrf-`, en kunt u precies 6 cijfers. 

    ![Voer informatie in entiteit voor de entiteit reguliere expressie](./media/get-started-portal-build-app/create-regular-expression-entity.png)

1. Selecteer **Done**. 

## <a name="add-example-utterances-to-none-intent"></a>Voorbeeld utterances toevoegen op None intentie

De **geen** kunt u lezen wat de terugval bedoeling is en mag niet leeg worden gelaten. Dit doel hebt 1 utterance voor elke 10 in de rest van de intentie van de app. 

De **geen** van intentie voorbeeld uitingen moet buiten het domein van uw client-toepassing. 

1. Selecteer **Intents** in het menu links, selecteer **geen** in de lijst Intents.

1. Het volgende voorbeeld utterances toevoegen aan het doel:

    |Geen voorbeeld van de intentie uitingen|
    |--|
    |Blaffende honden zijn irritant|
    |Bestel een pizza voor me|
    |Pinguïns in de oceaan|

    Voor deze app human resources vallen deze voorbeeld-uitingen buiten het domein. Als uw human resources-domein, zoals dieren, eten of het in de Indische Oceaan, klikt u vervolgens deze voorbeeld-uitingen mag niet worden gebruikt voor de **geen** intentie. 

## <a name="train-the-app"></a>De app trainen

Selecteer in de navigatie in de rechterbovenhoek, **Train** om toe te passen de intentie en entiteit model wordt gewijzigd in de huidige versie van de app. 

## <a name="look-at-the-regular-expression-entity-in-the-example-utterances"></a>Kijken naar de entiteit reguliere expressie in de voorbeeld-uitingen

1. Controleer of de entiteit is gevonden in de **FindForm** intentie door te selecteren **Intents** in het menu links, selecteer **FindForm** intentie. 

    De entiteit is gemarkeerd als waar deze wordt weergegeven in de voorbeeld-uitingen. Als u wilt zien van de oorspronkelijke tekst, in plaats van de naam van de entiteit in-/ uitschakelen **entiteiten weergeven** via de werkbalk.

    [![Alle uitingen van voorbeeld gemarkeerd met entiteiten](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png)](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png#lightbox)

## <a name="test-your-new-app-with-the-interactive-test-pane"></a>Uw nieuwe app testen met de interactieve testvenster

Gebruik de interactieve **Test** deelvenster in de portal LUIS voor het valideren van de entiteit is geëxtraheerd uit nieuwe uitingen die de app nog niet is gezien.

1. Selecteer **Test** in het menu rechtsboven.

1. Een nieuwe utterance toevoegen en druk op Enter:

    ```Is there a form named hrf-234098```

    ![Testen van nieuwe utterance in testvenster](./media/get-started-portal-build-app/test-new-utterance.png)

    De bovenste voorspeld bedoeling correct is **FindForm** met meer dan 90% vertrouwen (0.977) en de **Human Resources formuliernummer** entiteit wordt opgehaald met een waarde van hrf 234098. 

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u klaar bent met deze Quick Start en u geen verdergaat met de volgende snelstartgids, selecteert u **mijn apps** van het bovenste navigatiemenu. Klik links selectievakje van de app in de lijst en selecteer **verwijderen** via het context-werkbalk boven de lijst. 

[![App verwijderen uit de lijst met mijn apps](./media/get-started-portal-build-app/delete-app.png)](./media/get-started-portal-build-app/delete-app.png#lightbox)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [2. Een app implementeren](get-started-portal-deploy-app.md)