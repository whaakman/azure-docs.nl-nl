---
title: Intents toevoegen
titleSuffix: Language Understanding - Azure Cognitive Services
description: Intents toevoegen aan uw LUIS-app voor het identificeren van groepen van vragen of de opdrachten die het hetzelfde doel hebben.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/01/2019
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: 0c42ab44ba317888b982ba7c72f78be4ca73d93c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65148172"
---
# <a name="add-intents-to-determine-user-intention-of-utterances"></a>Intents om te bepalen van de gebruiker de bedoeling uitingen toevoegen

Voeg [intents](luis-concept-intent.md) naar uw LUIS-app en het identificeren van vragen of opdrachten die hetzelfde doel hebben. 

Intents worden beheerd vanuit de bovenste navigatiebalk staat **bouwen** sectie, klikt u vervolgens in het linkerpaneel **Intents**. 

## <a name="add-intent"></a>Intentie toevoegen

1. Selecteer **Create new intent** op de pagina **Intents**.

1. In de **maken van nieuwe bedoeling** dialoogvenster vak, voer de naam van de intentie `GetEmployeeInformation`, en klikt u op **gedaan**.

    ![Doel toevoegen](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

## <a name="add-an-example-utterance"></a>Een voorbeeld-utterance toevoegen

Voorbeeld uitingen zijn voorbeelden van de tekst van de gebruiker vragen of opdrachten. Als u wilt leren Language Understanding (LUIS), moet u voorbeeld utterances toevoegen aan een doel.

1. Op de **GetEmployeeInformation** intentie details pagina, voer een relevante utterance die u kunt van uw gebruikers, zoals verwachten `Does John Smith work in Seattle?` in het tekstvak onder de naam van de intentie en druk op Enter.
 
    ![Schermafbeelding van de intenties pagina, met utterance gemarkeerd](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

    LUIS alle uitingen geconverteerd naar kleine letters en voegt spaties rond tokens zoals afbreekstreepjes bevatten.

<a name="#intent-prediction-discrepancy-errors"></a>

## <a name="intent-prediction-errors"></a>Voorspelling van intentie-fouten 

Een voorbeeld utterance in een doel kan een voorspelling van intentie-fout tussen de bedoeling die de voorbeeld-utterance is momenteel in en de voorspelling intentie bepaald tijdens de training hebben. 

Utterance voorspelling fouten gevonden en corrigeren, gebruikt u de **Filter** van optie **evaluatie** opties van onjuist en Unclear in combinatie met de **weergave** optie van **Gedetailleerde weergave**. 

![Als u wilt zoeken utterance voorspelling fouten en los ze, gebruik de filteroptie.](./media/luis-how-to-add-intents/find-intent-prediction-errors.png)

Wanneer de filters en de weergave worden toegepast en er voorbeeld uitingen met fouten zijn, wordt de lijst van de utterance voorbeeld bevat de uitingen en de problemen.

![! [Wanneer de filters en de weergave worden toegepast, en er voorbeeld uitingen met fouten, de lijst met voorbeelden utterance toont de uitingen en de problemen.] (. / media/luis-how-to-add-intents/find-errors-in-utterances.png)](./media/luis-how-to-add-intents/find-errors-in-utterances.png#lightbox)

Elke rij bevat de huidige training voorspelling score voor de voorbeeld-utterance, de dichtstbijzijnde rivaal score, die het verschil in deze twee scores is. 

### <a name="fixing-intents"></a>Intents oplossen

Voor meer informatie over het oplossen van fouten van de intentie voorspelling, gebruikt de [Dashboard samenvatting van](luis-how-to-use-dashboard.md). Het dashboard samenvatting bevat een analyse voor de actieve versie laatste training en biedt de hoogste suggesties om op te lossen van uw model.  

## <a name="add-a-custom-entity"></a>Een aangepaste entiteit toevoegen

Zodra een utterance wordt toegevoegd aan een doel, kunt u tekst uit de utterance om een aangepaste entiteit te maken. Een aangepaste entiteit is een manier om de labeltekst voor uitpakken, samen met de juiste intentie. 

Zie [entiteit toevoegen aan utterance](luis-how-to-add-example-utterances.md) voor meer informatie.

## <a name="entity-prediction-discrepancy-errors"></a>Entiteit voorspelling discrepantie fouten 

De entiteit wordt dit onderstreept in rood om aan te geven een [entiteit voorspelling discrepantie](luis-how-to-add-example-utterances.md#entity-status-predictions). Omdat dit het eerste exemplaar van een entiteit, er zijn niet voldoende voorbeelden voor LUIS hebben een hoge betrouwbaarheid die deze tekst is gecodeerd met de juiste entiteit. Dit verschil wordt verwijderd wanneer de app wordt getraind. 

![Schermafbeelding van de intenties de pagina met details, de naam van de aangepaste entiteit in het blauw is gemarkeerd](./media/luis-how-to-add-intents/create-custom-entity-name-blue-highlight.png) 

De tekst is in blauw, die wijzen op een entiteit gemarkeerd.  

## <a name="add-a-prebuilt-entity"></a>Een vooraf gedefinieerde entiteit toevoegen

Zie voor meer informatie, [vooraf gedefinieerde entiteit](luis-how-to-add-entities.md#add-a-prebuilt-entity-to-your-app).

## <a name="using-the-contextual-toolbar"></a>Met behulp van de contextuele werkbalk

Wanneer u een of meer voorbeeld uitingen hebt geselecteerd in de lijst het selectievakje aan de linkerkant van een utterance wordt de werkbalk boven de lijst met utterance kunt u de volgende acties uitvoeren:

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
