---
title: Zelfstudie over het gebruiken van patronen voor het verbeteren van LUIS voorspellingen - Azure | Microsoft Docs
titleSuffix: Cognitive Services
description: In deze zelfstudie, gebruikt u patroon voor intents voor het verbeteren van LUIS intentie en entiteit voorspellingen.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 07/30/2018
ms.author: diberry
ms.openlocfilehash: 355c1edd4fa7433e68a9c0e903f4f782203326fe
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365875"
---
# <a name="tutorial-improve-app-with-patterns"></a>Zelfstudie: Een app met patronen verbeteren

In deze zelfstudie gebruiken u patronen voor het verhogen van de intentie en entiteit voorspelling.  

> [!div class="checklist"]
* Identificeren dat een patroon waarmee uw app
* Over het maken van een patroon
* Patroon voorspelling verbeteringen controleren

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Voordat u begint

Als u geen de Human Resources-app vanuit de [batch test](luis-tutorial-batch-testing.md) zelfstudie [importeren](luis-how-to-start-new-app.md#import-new-app) de JSON naar een nieuwe app in de [LUIS](luis-reference-regions.md#luis-website) website. De app voor het importeren is gevonden in de [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-batchtest-HumanResources.json) GitHub-opslagplaats.

Als u de oorspronkelijke Human Resources-app wilt gebruiken, kloont u de versie op de pagina [Settings](luis-how-to-manage-versions.md#clone-a-version) en wijzigt u de naam in `patterns`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. 

## <a name="patterns-teach-luis-common-utterances-with-fewer-examples"></a>Patronen leren LUIS algemene uitingen met minder voorbeelden

Vanwege de aard van het Human resources-domein, moet u er een paar algemene manieren om vragen over werknemer relaties in organisaties zijn. Bijvoorbeeld:

|Utterances|
|--|
|Wie Jill Jones rapporteren aan?|
|Die rapporteert aan Jill Jones?|

Deze uitingen zijn te dicht bij de contextuele Uniekheid van elk zonder op te geven veel utterance voorbeelden bepalen. Door toe te voegen een patroon voor een doel, LUIS algemene utterance-patronen leert herkennen voor een doel zonder op te geven veel utterance voorbeelden. 

Voorbeeld van de sjabloon uitingen hiervoor intentie zijn:

|Voorbeeld van de sjabloon uitingen|
|--|
|Wie komt {werknemer} rapporteren aan?|
|Die rapporteert aan {werknemer}?|

Het patroon wordt geboden via een sjabloon utterance-voorbeeld, waarin u de syntaxis voor het identificeren van entiteiten en ignorable tekst bevat. Een patroon is een combinatie van reguliere expressie die overeenkomen met en machine learning.  Het voorbeeld van een sjabloon utterance, samen met de intentie uitingen geven LUIS een beter beeld van welke uitingen past het doel.

Opdat een patroon worden vergeleken met een utterance, moeten de entiteiten in de utterance eerst de entiteiten in de sjabloon utterance overeenkomen. Echter, de sjabloon niet helpen bij het voorspellen entiteiten, alleen intents. 

**Terwijl patronen u minder voorbeeld uitingen kunnen, als de entiteiten niet worden gedetecteerd, wordt het patroon komt niet overeen.**

Houd er rekening mee dat werknemers zijn gemaakt de [lijst entiteit zelfstudie](luis-quickstart-intent-and-list-entity.md).

## <a name="create-new-intents-and-their-utterances"></a>Nieuwe intents en hun uitingen maken

Twee nieuwe intents toevoegen: `OrgChart-Manager` en `OrgChart-Reports`. LUIS retourneert één keer een voorspelling op de clientapp, de naam van de intentie kan worden gebruikt als de naam van een functie in de client-app en dat de werknemer-entiteit kan worden gebruikt als een parameter voor deze functie.

```Javascript
OrgChart-Manager(employee){
    ///
}
```

1. Zorg ervoor dat uw Human Resources-app zich in de sectie **Build** van LUIS bevindt. U kunt naar deze sectie gaan door **Build** te selecteren in de menubalk rechtsboven. 

2. Selecteer **Create new intent** op de pagina **Intents**. 

3. Voer in het pop-updialoogvenster `OrgChart-Manager` in en selecteer vervolgens **Done**.

    ![Pop-upvenster met een nieuw bericht maken](media/luis-tutorial-pattern/hr-create-new-intent-popup.png)

4. Voeg voorbeelden van utterances toe aan de intentie.

    |Voorbeelden van utterances|
    |--|
    |Wie is de onderliggend niveau van John W. Smith?|
    |Wie John W. Smith rapporteren aan?|
    |Wie is John W. Smith manager?|
    |Wie Jill Jones rechtstreeks rapporteren aan?|
    |Wie is de supervisor Jill Jones?|

    [![Schermafbeelding van LUIS toe te voegen nieuwe uitingen naar intentie](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png "Screenshot van LUIS toe te voegen nieuwe uitingen naar intentie")](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png#lightbox)

    U hoeft niet als de entiteit keyPhrase in de uitingen van het doel in plaats van de werknemer-entiteit wordt aangeduid. Beide zijn correct in het deelvenster en op het eindpunt voorspeld. 

5. Selecteer **Intents** in de linkernavigatiebalk.

6. Selecteer **Create new intent**. 

7. Voer in het pop-updialoogvenster `OrgChart-Reports` in en selecteer vervolgens **Done**.

8. Voeg voorbeelden van utterances toe aan de intentie.

    |Voorbeelden van utterances|
    |--|
    |Wie zijn onderliggende niveaus van John W. Smith?|
    |Die rapporteert aan John W. Smith?|
    |Wie het beheren van John W. Smith?|
    |Wie zijn Jill Jones directe ondergeschikten?|
    |Wie Jill Jones controleren?|

## <a name="caution-about-example-utterance-quantity"></a>Waarschuwing over voorbeeld utterance hoeveelheid

De hoeveelheid uitingen voorbeeld in deze intents is niet genoeg om te trainen LUIS goed. In een echte app, moet elk doel hebben een minimum van 15 uitingen met tal van word keuze en utterance lengte. Deze enkele uitingen zijn geselecteerd specifiek voor het markeren van patronen. 

## <a name="train-the-luis-app"></a>LUIS-app trainen

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>App publiceren om eindpunt-URL op te vragen

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Eindpunt opvragen met een andere utterance

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Ga naar het einde van de URL in het adres en voer `Who is the boss of Jill Jones?` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. 

    ```JSON
    {
        "query": "who is the boss of jill jones?",
        "topScoringIntent": {
            "intent": "OrgChart-Manager",
            "score": 0.353984952
        },
        "intents": [
            {
                "intent": "OrgChart-Manager",
                "score": 0.353984952
            },
            {
                "intent": "OrgChart-Reports",
                "score": 0.214128986
            },
            {
                "intent": "EmployeeFeedback",
                "score": 0.08434003
            },
            {
                "intent": "MoveEmployee",
                "score": 0.019131
            },
            {
                "intent": "GetJobInformation",
                "score": 0.004819009
            },
            {
                "intent": "Utilities.Confirm",
                "score": 0.0043958663
            },
            {
                "intent": "Utilities.StartOver",
                "score": 0.00312064588
            },
            {
                "intent": "Utilities.Cancel",
                "score": 0.002265454
            },
            {
                "intent": "Utilities.Help",
                "score": 0.00133465114
            },
            {
                "intent": "None",
                "score": 0.0011388344
            },
            {
                "intent": "Utilities.Stop",
                "score": 0.00111166481
            },
            {
                "intent": "FindForm",
                "score": 0.0008900076
            },
            {
                "intent": "ApplyForJob",
                "score": 0.0007836131
            }
        ],
        "entities": [
            {
                "entity": "jill jones",
                "type": "Employee",
                "startIndex": 19,
                "endIndex": 28,
                "resolution": {
                    "values": [
                        "Employee-45612"
                    ]
                }
            },
            {
                "entity": "boss of jill jones",
                "type": "builtin.keyPhrase",
                "startIndex": 11,
                "endIndex": 28
            }
        ]
    }
    ```

Is deze query gelukt? Voor deze cyclus training is het gelukt. De scores van de twee bovenste intenties zijn sluiten. Omdat LUIS training is niet precies hetzelfde telkens, er is een deel van de variant, deze twee scores op basis van de volgende cyclus van training kunnen omkeren. Het resultaat is dat het verkeerde doel kan worden geretourneerd. 

Patronen gebruiken om de juiste intentie score aanzienlijk hoger percentage en verder de volgende hoogste score. 

Laat dit tweede browservenster geopend. Deze gebruikt u het later opnieuw in de zelfstudie. 

## <a name="add-the-template-utterances"></a>De sjabloon utterances toevoegen

1. Selecteer **bouwen** in het bovenste menu.

2. In het linker navigatiedeelvenster, onder **verbeterde app-prestaties**, selecteer **patronen** in de linkernavigatiebalk.

3. Selecteer de **organigram-Manager** doel, voert u de volgende sjabloon uitingen, één voor één, selecteren enter na elke utterance sjabloon:

    |Uitingen van sjabloon|
    |:--|
    |Wie is {werknemer} het onderliggend niveau van [?]|
    |Wie komt {werknemer} rapporteren aan [?]|
    |Wie is de manager {werknemer} [de] [?]|
    |Wie komt {werknemer} rechtstreeks rapporteren aan [?]|
    |Wie is de supervisor {werknemer} [de] [?]|
    |Wie is de baas van {werknemer} [?]|

    De `{Employee}` syntaxis van de locatie van de entiteit in de sjabloon utterance als het goed als welke entiteit is gemarkeerd. 

    Entiteiten met behulp van syntaxis die de rolnaam van de bevat, en worden behandeld in een [afzonderlijke zelfstudie voor rollen](luis-tutorial-pattern-roles.md). 

    De syntaxis van de optionele `[]`, markeert u woorden of leestekens die optioneel zijn. LUIS komt overeen met de utterance, de optionele tekst tussen haakjes worden genegeerd.

    Als u de sjabloon utterance, LUIS kunt u in de entiteit bij het invoeren van de accolade links `{`.

    [![Schermafbeelding van het invoeren van de sjabloon-uitingen voor doel](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png)](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png#lightbox)

4. Selecteer de **organigram-rapporten** doel, voert u de volgende sjabloon uitingen, één voor één, selecteren enter na elke utterance sjabloon:

    |Uitingen van sjabloon|
    |:--|
    |Wie zijn {werknemer} [de] onderliggende niveaus [?]|
    |Die rapporteert aan {werknemer} [?]|
    |Wie komt {werknemer} [?] beheren|
    |Wie {werknemer} directe ondergeschikten [?] zijn|
    |Wie komt {werknemer} [?] controleren|
    |Wie komt {werknemer} baas [?]|

## <a name="query-endpoint-when-patterns-are-used"></a>Query uitvoeren op eindpunt wanneer patronen worden gebruikt

1. Trainen en publiceer de app opnieuw.

2. Browsertabbladen Ga terug naar het tabblad eindpunt-URL.

3. Ga naar het einde van de URL in het adres en voer `Who is the boss of Jill Jones?` als de utterance. De laatste parameter van de queryreeks is `q`, de utterance **query**. 

    ```JSON
    {
        "query": "who is the boss of jill jones?",
        "topScoringIntent": {
            "intent": "OrgChart-Manager",
            "score": 0.9999989
        },
        "intents": [
            {
                "intent": "OrgChart-Manager",
                "score": 0.9999989
            },
            {
                "intent": "OrgChart-Reports",
                "score": 7.616303E-05
            },
            {
                "intent": "EmployeeFeedback",
                "score": 7.84204349E-06
            },
            {
                "intent": "GetJobInformation",
                "score": 1.20674213E-06
            },
            {
                "intent": "MoveEmployee",
                "score": 7.91245157E-07
            },
            {
                "intent": "None",
                "score": 3.875E-09
            },
            {
                "intent": "Utilities.StartOver",
                "score": 1.49E-09
            },
            {
                "intent": "Utilities.Confirm",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Help",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Stop",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Cancel",
                "score": 1.225E-09
            },
            {
                "intent": "FindForm",
                "score": 1.123077E-09
            },
            {
                "intent": "ApplyForJob",
                "score": 5.625E-10
            }
        ],
        "entities": [
            {
                "entity": "jill jones",
                "type": "Employee",
                "startIndex": 19,
                "endIndex": 28,
                "resolution": {
                    "values": [
                        "Employee-45612"
                    ]
                },
                "role": ""
            },
            {
                "entity": "boss of jill jones",
                "type": "builtin.keyPhrase",
                "startIndex": 11,
                "endIndex": 28
            }
        ]
    }
    ```

De intentie voorspelling is nu aanzienlijk hoger.

## <a name="working-with-optional-text-and-prebuilt-entities"></a>Werken met optionele tekst en vooraf gemaakte entiteiten

Het vorige patroon sjabloon uitingen in deze zelfstudie heeft een aantal voorbeelden van optionele tekst, zoals het possessive gebruik van de letter s. `'s`, en het gebruik van het vraagteken `?`. Stel dat de eindpunt-uitingen weergeven dat managers en Human Resources vertegenwoordigers zoekt van historische gegevens, evenals werknemer verplaatst binnen het bedrijf dat gebeurt op een later tijdstip gepland.

Voorbeeld-uitingen zijn:

|Doel|Voorbeeld-uitingen met optionele tekst en vooraf gemaakte entiteiten|
|:--|:--|
|Organigram-Manager|`Who was Jill Jones manager on March 3?`|
|Organigram-Manager|`Who is Jill Jones manager now?`|
|Organigram-Manager|`Who will be Jill Jones manager in a month?`|
|Organigram-Manager|`Who will be Jill Jones manager on March 3?`|

Elk van deze voorbeelden gebruikt een tegenwoordige term `was`, `is`, `will be`, evenals een datum, `March 3`, `now`, en `in a month`, die LUIS moet correct te voorspellen. U ziet dat de laatste twee voorbeelden gebruikt u bijna dezelfde tekst, behalve voor `in` en `on`.

Voorbeeld van de sjabloon uitingen:
|Doel|Voorbeeld-uitingen met optionele tekst en vooraf gemaakte entiteiten|
|:--|:--|
|Organigram-Manager|`who was {Employee}['s] manager [[on]{datetimeV2}?`]|
|Organigram-Manager|`who is {Employee}['s] manager [[on]{datetimeV2}?]`|
|Organigram-Manager|`who will be {Employee}['s] manager [[in]{datetimeV2}?]`|
|Organigram-Manager|`who will be {Employee}['s] manager [[on]{datetimeV2}?]`|

Het gebruik van de optionele syntaxis van tussen vierkante haken, `[]`, kunt u deze optionele tekst eenvoudig toevoegen aan de sjabloon utterance en tot een tweede niveau kan worden genest `[[]]`, en entiteiten of tekst bevatten.

**Vraag: Waarom kunnen niet de laatste twee voorbeeld-uitingen combineren in een enkele sjabloon utterance?** De sjabloon patroon biedt geen ondersteuning of syntaxis. Om te kunnen vangen zowel de `in` versie en de `on` versie, die beide projecten moeten worden van een afzonderlijke sjabloon utterance.

**Vraag: Waarom zijn de `w` letters, de eerste letter in elke sjabloon utterance, kleine letters? Mag niet ze eventueel hoofdletters of kleine letters zijn?** De utterance verzonden naar het eindpunt van de query door de clienttoepassing wordt geconverteerd naar kleine letters. De sjabloon utterance kan hoofdletters of kleine letters en de utterance eindpunt kan ook zijn. De vergelijking wordt altijd uitgevoerd na de conversie naar kleine letters.

**Vraag: Waarom is niet vooraf gemaakte getal deel uitmaakt van de sjabloon utterance als 3 maart wordt voorspeld beide als getal `3` en datum `March 3`?** De sjabloon utterance contextueel wordt met behulp van een datum, ofwel letterlijk in `March 3` of abstracte als `in a month`. Een datum een waarde kan bevatten, maar een getal mag niet per se worden gezien als een datum. Gebruik altijd de entiteit die het best vertegenwoordigt het type dat u laten retourneren in de JSON-voorspellingsresultaten wilt.  

**Vraag: Hoe zit het met slecht geformuleerd uitingen zoals `Who will {Employee}['s] manager be on March 3?`.** Grammaticaal verschillende werkwoordtijden, zoals deze waar de `will` en `be` worden gescheiden moet een nieuwe sjabloon utterance. De bestaande sjabloon utterance komt niet overeen met het. Het doel van de utterance nog niet is gewijzigd, wordt de plaatsing van het woord in de utterance is gewijzigd. Deze wijziging heeft gevolgen voor de voorspelling in LUIS.

**Houd er rekening mee: entiteiten zijn gevonden eerst, en vervolgens het patroon overeenkomt.**

## <a name="edit-the-existing-pattern-template-utterance"></a>De bestaande patroon sjabloon utterance bewerken

1. Selecteer op de website LUIS **bouwen** in het bovenste menu Selecteer **patronen** in het menu links. 

2. Vinden van de bestaande sjabloon utterance, `Who is {Employee}['s] manager[?]`, en selecteer het weglatingsteken (***...*** ) aan de rechterkant. 

3. Selecteer **bewerken** in het pop-upmenu. 

4. De sjabloon utterance te wijzigen: `who is {Employee}['s] manager [[on]{datetimeV2}?]]`

## <a name="add-new-pattern-template-utterances"></a>Nieuwe patroon sjabloon utterances toevoegen

1. Terwijl u nog steeds in de **patronen** sectie van **bouwen**, verschillende nieuwe sjabloon uitingen patroon toevoegen. Selecteer **organigram-Manager** van de intentie vervolgkeuzelijst en voer elk van de volgende sjabloon uitingen:

    |Doel|Voorbeeld-uitingen met optionele tekst en vooraf gemaakte entiteiten|
    |--|--|
    |Organigram-Manager|`who was {Employee}['s] manager [[on]{datetimeV2}?]`|
    |Organigram-Manager|`who is {Employee}['s] manager [[on]{datetimeV2}?]`|
    |Organigram-Manager|`who will be {Employee}['s] manager [[in]{datetimeV2}?]`|
    |Organigram-Manager|`who will be {Employee}['s] manager [[on]{datetimeV2}?]`|

2. De app trainen.

3. Selecteer **Test** aan de bovenkant van het Configuratiescherm om de testen deelvenster te openen. 

4. Voer verschillende test-uitingen als u wilt controleren of het patroon overeenkomt en de intentie score aanzienlijk hoog is. 

    Nadat u de eerste utterance invoeren, selecteert u **inspecteren** onder het resultaat, zodat u de voorspellingsresultaten kunt zien.

    |Utterance|
    |--|
    |Wie zijn Jill Jones manager|
    |wie zijn Marie jones van manager|
    |Wie zijn Jill Jones van manager?|
    |wie zijn Marie jones manager op 3 maart|
    |Wie zijn Jill Jones manager volgende maand|
    |Wie zijn Jill Jones manager in een maand?|

Al deze uitingen de entiteiten gevonden binnen, dus ze overeenkomen met hetzelfde patroon en een hoge voorspelling score hebben.

## <a name="clean-up-resources"></a>Resources opschonen

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het gebruik van functies met een patroon](luis-tutorial-pattern-roles.md)