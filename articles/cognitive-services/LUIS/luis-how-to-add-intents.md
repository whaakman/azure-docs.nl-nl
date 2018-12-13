---
title: Intents toevoegen
titleSuffix: Language Understanding - Azure Cognitive Services
description: Intents toevoegen aan uw LUIS-app voor het identificeren van groepen van vragen of de opdrachten die het hetzelfde doel hebben.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.component: language-understanding
ms.topic: article
ms.date: 10/24/2018
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: 3e6064b4c202c36e4b63d6e06edfbf3149f6665f
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53139854"
---
# <a name="add-intents"></a>Intents toevoegen 

Voeg [intents](luis-concept-intent.md) naar uw LUIS-app en het identificeren van vragen of opdrachten die hetzelfde doel hebben. 

Intents worden beheerd vanuit de bovenste navigatiebalk staat **bouwen** sectie, klikt u vervolgens in het linkerpaneel **Intents**. 

## <a name="create-an-app"></a>Een app maken

1. Aanmelden bij de [LUIS](https://www.luis.ai) portal.

1. Selecteer **Create new app**. 

1. Naam van de nieuwe app `MyHumanResourcesApp`. Selecteer de **Engels** cultuur. De beschrijving is optioneel. 

1. Selecteer **Done**. 

## <a name="add-intent"></a>Intentie toevoegen

1. De app wordt geopend op de **Intents** lijst.

1. Selecteer **Create new intent** op de pagina **Intents**.

1. In de **maken van nieuwe bedoeling** dialoogvenster vak, voer de naam van de intentie `GetEmployeeInformation`, en klikt u op **gedaan**.

    ![Doel toevoegen](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

## <a name="add-an-example-utterance"></a>Een voorbeeld-utterance toevoegen

Voorbeeld uitingen zijn voorbeelden van de tekst van de gebruiker vragen of opdrachten. Als u wilt leren Language Understanding (LUIS), moet u voorbeeld utterances toevoegen aan een doel.

1. Op de **GetEmployeeInformation** intentie details pagina, voer een relevante utterance die u kunt van uw gebruikers, zoals verwachten `Does John Smith work in Seattle?` in het tekstvak onder de naam van de intentie en druk op Enter.
 
    ![Schermafbeelding van de intenties pagina, met utterance gemarkeerd](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

    LUIS alle uitingen geconverteerd naar kleine letters en voegt spaties rond tokens zoals afbreekstreepjes bevatten.

## <a name="intent-prediction-discrepancy-errors"></a>Voorspelling van intentie discrepantie fouten 

Een utterance in een doel kan een voorspelling van intentie verschil tussen het geselecteerde doel en de voorspelling score hebben. LUIS geeft aan dat dit verschil met een rood kader rond de **met het label bedoeling** op de rij van de voorbeeld-utterance. 

![Schermafbeelding van de intenties pagina, met utterance voorspelling discrepantie fouten](./media/luis-how-to-add-intents/prediction-discrepancy-intent.png) 

Selecteer in de bovenste navigatiebalk **Train**. Het verschil voorspellen is nu verdwenen.

## <a name="add-a-custom-entity"></a>Een aangepaste entiteit toevoegen

Zodra een utterance wordt toegevoegd aan een doel, kunt u tekst uit de utterance om een aangepaste entiteit te maken. Een aangepaste entiteit is een manier om de labeltekst voor uitpakken, samen met de juiste intentie. 

1. Selecteer het woord `Seattle`, in de utterance. Vierkante haken worden getekend rondom de tekst en een vervolgkeuzelijst wordt weergegeven. 

    ![Schermafbeelding van de intenties de detailpagina van het maken van aangepaste entiteit](./media/luis-how-to-add-intents/create-custom-entity.png) 

    In het volgende voorbeeld wordt één woord om te markeren als een entiteit. U kunt één werkt en zinnen markeren als entiteiten.

1. Voer in het bovenste tekstvak-van het menu, `Location`en selecteer vervolgens **nieuwe entiteit maken**. 

    ![Schermafbeelding van de intenties detailpagina, naam van de aangepaste entiteit maken](./media/luis-how-to-add-intents/create-custom-entity-name.png) 

1. In de **welk type entiteit wilt u maken?** pop-upvenster voor het maken van een entiteit, Controleer of de **entiteitnaam** is _locatie_, en de **entiteitstype**  is _eenvoudige_. Selecteer **Done**.

## <a name="entity-prediction-discrepancy-errors"></a>Entiteit voorspelling discrepantie fouten 

De entiteit wordt dit onderstreept in rood om aan te geven een [entiteit voorspelling discrepantie](luis-how-to-add-example-utterances.md#entity-status-predictions). Omdat dit het eerste exemplaar van een entiteit, er zijn niet voldoende voorbeelden voor LUIS hebben een hoge betrouwbaarheid die deze tekst is gecodeerd met de juiste entiteit. Dit verschil wordt verwijderd wanneer de app wordt getraind. 

![Schermafbeelding van de intenties de pagina met details, de naam van de aangepaste entiteit in het blauw is gemarkeerd](./media/luis-how-to-add-intents/create-custom-entity-name-blue-highlight.png) 

De tekst is in blauw, die wijzen op een entiteit gemarkeerd.  

## <a name="add-a-prebuilt-entity"></a>Een vooraf gedefinieerde entiteit toevoegen

Zie voor meer informatie, [vooraf gedefinieerde entiteit](luis-how-to-add-entities.md#add-prebuilt-entity).

## <a name="using-the-contextual-toolbar"></a>Met behulp van de contextuele werkbalk

Wanneer een of meer voorbeeld uitingen is geselecteerd in de lijst het selectievakje aan de linkerkant van een utterance, wordt de werkbalk boven de lijst met utterance kunt u de volgende acties uitvoeren:

* Opnieuw toewijzen van doel: utterance(s) verplaatsen naar een ander doel
* Utterance(s) verwijderen
* Entiteit filters: alleen weergeven met gefilterde entiteiten uitingen
* Alles weergeven / alleen fouten: uitingen met voorspelling fouten weergeven of weergeven van alle uitingen
* Entiteiten/Tokens weergave: entiteiten weergeven met namen van entiteiten of onbewerkte tekst van utterance weergeven
* Vergrootglas: zoek uitingen met specifieke tekst

## <a name="working-with-an-individual-utterance"></a>Werken met een afzonderlijke utterance

De volgende acties kunnen worden uitgevoerd op een afzonderlijke utterance in het menu van de drie puntjes aan de rechterkant van de utterance:

* Bewerken: de tekst van de utterance wijzigen
* Verwijderen: Verwijder de utterance uit het doel. Als u nog steeds de utterance wilt, een betere methode is om te verplaatsen naar de **geen** intentie. 
* Voeg een patroon: Een patroon kunt u een algemene utterance en markeren replaceable en ignorable tekst, waardoor de noodzaak van meer uitingen in het doel. 

De **met het label bedoeling** kolom kunt u het doel van de utterance wijzigen.

## <a name="train-your-app-after-changing-model-with-intents"></a>Uw app na het wijzigen van model met intents trainen

Na het toevoegen, bewerken of verwijderen van intents, [trainen](luis-how-to-train.md) en [publiceren](luis-how-to-publish-app.md) uw app zodat uw wijzigingen worden toegepast op eindpunt query's. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het toevoegen van [voorbeeld uitingen](luis-how-to-add-example-utterances.md) met entiteiten. 
