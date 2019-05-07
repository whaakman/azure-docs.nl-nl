---
title: 'Quickstart: Een nieuwe app maken in de portal LUIS'
titleSuffix: Language Understanding - Azure Cognitive Services
description: In deze quickstart maakt u een nieuwe app in de portal LUIS. Maak de basisonderdelen van een app, intenties en entiteiten. De app testen door te geven een voorbeeld van gebruiker utterance in het deelvenster interactieve test om op te halen van de voorspelde intentie. Het bouwen van een app is gratis; Er is een Azure-abonnement geen vereist.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 2e4ff7dc97e3ee72336bd4c081caf1aa1a62bc56
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65146558"
---
# <a name="quickstart-create-a-new-app-in-the-luis-portal"></a>Quickstart: Een nieuwe app maken in de portal LUIS

In deze snelstartgids bouwt u een nieuwe app in de [LUIS portal](https://www.luis.ai). U maakt eerst de basisonderdelen van een app **intents**, en **entiteiten**. Vervolgens test u de app door op te geven een voorbeeld van gebruiker utterance in het deelvenster interactieve test om op te halen van de voorspelde intentie.

Het bouwen van een app is gratis en Azure-abonnement niet vereist. Wanneer u klaar om uw app te implementeren bent, ziet de [Snelstartgids voor het implementeren van een app](get-started-portal-deploy-app.md). Hier ziet u hoe u een Azure-Resource voor Cognitive Service maakt en deze toewijzen aan de app.

## <a name="create-an-app"></a>Een app maken

1. Open de [LUIS portal](https://www.luis.ai) in een browser en meld u aan. Als het de eerste keer aanmelden, moet u een gratis LUIS-portal-gebruikersaccount maken.

1. Selecteer **nieuwe app maken** via de werkbalk context.

   [![Nieuwe app maken in de portal van LUIS](./media/get-started-portal-build-app/create-app-in-portal.png)](./media/get-started-portal-build-app/create-app-in-portal.png#lightbox)

1. In het pop-upvenster, configureert u de app met de volgende instellingen en selecteer vervolgens **gedaan**.

   |Naam van instelling| Value | Doel|
   |--|--|--|
   |Name|`myEnglishApp`|De unieke naam van LUIS-app<br>vereist|
   |Culture|**Engels**|Taal van uitingen van gebruikers, **en-us**<br>vereist|
   |Description|`App made with LUIS Portal`|Beschrijving van app<br>optioneel|
   | | | |

   ![Voer de nieuwe appinstellingen](./media/get-started-portal-build-app/create-new-app-settings.png)

## <a name="create-intents"></a>Intents maken

Nadat de LUIS-app is gemaakt, moet u intents maken. Intents zijn een manier voor het categoriseren van tekst van gebruikers. Een human resources-app mogelijk bijvoorbeeld twee functies. Om mensen te helpen:

 1. Zoeken en toepassen voor taken
 1. Formulieren om toe te passen voor taken zoeken

De app de twee verschillende _bedoelingen_ uitgelijnd met de volgende intenties:

|Intentie|Van de voorbeeldtekst van gebruiker<br>ook wel een _utterance_|
|--|--|
|ApplyForJob|`I want to apply for the new software engineering position in Cairo.`|
|FindForm|`Where is the job transfer form hrf-123456?`|

Voor het maken van intents, voert u de volgende stappen uit:

1. Nadat de app is gemaakt, bent u op de **Intents** pagina van de **bouwen** sectie. Selecteer **Create new intent**.

   [![Selecteer de nieuwe intentie knop maken](./media/get-started-portal-build-app/create-new-intent-button.png)](./media/get-started-portal-build-app/create-new-intent-button.png#lightbox)

1. Voer de naam van de intentie `FindForm`, en selecteer vervolgens **gedaan**.

   ![Voer de naam van de intentie van FindForm](./media/get-started-portal-build-app/create-new-intent-dialog.png)

## <a name="add-an-example-utterance"></a>Een voorbeeld-utterance toevoegen

U toevoegen voorbeeld utterances nadat u intents hebt gemaakt. Voorbeeld-uitingen zijn tekst die een gebruiker moet in een chatbot of andere clienttoepassing invoeren. Ze de bedoeling van de tekst van de gebruiker worden toegewezen aan een LUIS-doel.

Voor deze voorbeeldtoepassing `FindForm` intentie, voorbeeld-uitingen bevat het formuliernummer. De clienttoepassing moet het nummer van de vorm van de gebruiker om aanvraag te voldoen, dus is het belangrijk om deze in de utterance.

[![Voorbeeld-uitingen voor het doel FindForm invoeren](./media/get-started-portal-build-app/add-example-utterance.png)](./media/get-started-portal-build-app/add-example-utterance.png#lightbox)

Voeg het volgende voorbeeld van 15-uitingen naar de `FindForm` intentie.

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

Standaard wordt deze voorbeeld-uitingen variëren in de volgende manieren:

* lengte van utterance
* Leestekens
* Word keuze
* term tegenwoordige (is, is, is)
* de woordvolgorde



## <a name="create-a-regular-expression-entity"></a>Een entiteit reguliere expressie maken

Als u wilt terugkeren het formuliernummer in het antwoord van de voorspellings-runtime, moet het formulier worden gemarkeerd als een entiteit. Omdat de tekst van het formulier is zeer gestructureerd, kunt u met behulp van een reguliere expressie entiteit kunt markeren. De entiteit is gemaakt met de volgende stappen uit:

1. Selecteer **entiteiten** in het menu aan de linkerkant.

1. Selecteer **nieuwe entiteit maken** op de **entiteiten** pagina.

1. Voer de naam `Human Resources Form Number`, selecteer de **Regex** entiteit, te typen en de reguliere expressie `hrf-[0-9]{6}`. Deze invoer komt overeen met de letterlijke tekens, `hrf-`, en kunt u precies 6 cijfers.

   ![Voer informatie in entiteit voor de entiteit reguliere expressie](./media/get-started-portal-build-app/create-regular-expression-entity.png)

1. Selecteer **Done**.

## <a name="add-example-utterances-to-the-none-intent"></a>Voorbeelduitingen toevoegen aan de intentie None

De **geen** kunt u lezen wat de terugval bedoeling is en mag niet leeg worden gelaten. Dit doel kan één utterance voor elke 10 voorbeeld-uitingen die u hebt toegevoegd voor de andere intentie van de app moet bevatten.

De **geen** van intentie voorbeeld uitingen moet buiten het domein van uw client-toepassing.

1. Selecteer **Intents** vanuit het menu aan de linkerkant en selecteer vervolgens **geen** in de lijst intents.

1. Het volgende voorbeeld utterances toevoegen aan het doel:

   |Geen voorbeeld van de intentie uitingen|
   |--|
   |Blaffende honden zijn irritant|
   |Bestel een pizza voor me|
   |Pinguïns in de oceaan|

   Voor deze app human resources vallen deze voorbeeld-uitingen buiten het domein. Als uw domein human resources bevatten dieren, eten of het in de Indische Oceaan, dan moet u verschillende voorbeeld-uitingen voor de **geen** intentie.

## <a name="train-the-app"></a>De app trainen

Selecteer in het menu in de rechterbovenhoek, **Train** om toe te passen de intentie en entiteit model wordt gewijzigd in de huidige versie van de app.

## <a name="look-at-the-regular-expression-entity-in-the-example-utterances"></a>Kijken naar de entiteit reguliere expressie in de voorbeeld-uitingen

1. Controleer of de entiteit is gevonden in de **FindForm** intentie hiervoor **Intents** in het menu links. Selecteer vervolgens **FindForm** intentie.

   De entiteit is gemarkeerd als waar deze wordt weergegeven in de voorbeeld-uitingen. Als u zien van de oorspronkelijke tekst in plaats van de naam van de entiteit wilt, in-/ uitschakelen **entiteiten weergeven** via de werkbalk.

   [![Alle uitingen van voorbeeld gemarkeerd met entiteiten](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png)](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png#lightbox)

## <a name="test-your-new-app-with-the-interactive-test-pane"></a>Uw nieuwe app testen met de interactieve testvenster

Gebruik de interactieve **Test** deelvenster in de portal LUIS om te valideren dat de entiteit is geëxtraheerd uit nieuwe uitingen die de app nog niet is gezien.

1. Selecteer **Test** in het menu rechtsboven.

1. Een nieuwe utterance toevoegen en druk op Enter:

   ```Is there a form named hrf-234098```

   ![Testen van nieuwe utterance in testvenster](./media/get-started-portal-build-app/test-new-utterance.png)

   De bovenste voorspeld bedoeling correct is **FindForm** met meer dan 90% vertrouwen (0.977). De **Human Resources formuliernummer** entiteit wordt opgehaald met een waarde van hrf 234098.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met deze Quick Start en zijn niet verdergaat met de volgende snelstartgids, selecteert u **mijn apps** van het bovenste navigatiemenu. Selecteer links selectievakje van de app uit de lijst en selecteer **verwijderen** via het context-werkbalk boven de lijst.

[![App verwijderen uit de lijst met mijn apps](./media/get-started-portal-build-app/delete-app.png)](./media/get-started-portal-build-app/delete-app.png#lightbox)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [2. Een app implementeren](get-started-portal-deploy-app.md)
