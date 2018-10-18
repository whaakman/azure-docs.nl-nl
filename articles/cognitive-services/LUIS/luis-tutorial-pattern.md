---
title: 'Zelfstudie 3: Patronen voor het verbeteren van LUIS-voorspellingen'
titleSuffix: Azure Cognitive Services
description: Gebruik patronen voor het verhogen van intent- en entiteitsvoorspelling terwijl er minder voorbeeld-utterances nodig zijn. Het patroon wordt geleverd via een voorbeeld van een sjabloon-utterance, met de syntaxis voor het identificeren van entiteiten en te negeren tekst.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: b09ebbb358b909c98df4eb05154c29b4b3cb7ee9
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48888251"
---
# <a name="tutorial-3-add-common-utterance-formats"></a>Zelfstudie 3: Algemene vormen van utterances toevoegen

Gebruik in deze zelfstudie patronen voor het verhogen van intent- en entiteitsvoorspelling terwijl er minder voorbeeld-utterances nodig zijn. Het patroon wordt geleverd via een voorbeeld van een sjabloon-utterance, met de syntaxis voor het identificeren van entiteiten en te negeren tekst. Een patroon is een combinatie van het vergelijken van expressies en machine learning.  Het voorbeeld van de sjabloon-utterance geeft samen met de intent-utterances LUIS een beter beeld van welke utterances het beste bij de intent passen. 

**In deze zelfstudie leert u het volgende:**

> [!div class="checklist"]
> * Bestaande zelfstudie-app gebruiken 
> * Intent maken
> * Trainen
> * Publiceren
> * Intenties en entiteiten ophalen van eindpunt
> * Een patroon maken
> * Verbeteringen van patroonvoorspellingen verifiëren
> * Tekst markeren als te negeren en nesten binnen het patroon
> * Testdeelvenster gebruiken om te controleren of het patroon is geslaagd

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Bestaande app gebruiken

Ga door met de in de laatste zelfstudie gemaakt app, **Human Resources**. 

Als u niet over de app Human Resources uit de vorige zelfstudie beschikt, voert u de volgende stappen uit:

1.  Download het [JSON-bestand van de app](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-batchtest-HumanResources.json) en sla het op.

2. Importeer de JSON in een nieuwe app.

3. Ga naar het gedeelte **Beheren**, open het tabblad **Versies**, kloon de versie en noem deze `patterns`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. Omdat de versienaam wordt gebruikt als onderdeel van de URL-route, kan de naam geen tekens bevatten die niet zijn toegestaan in een URL.

## <a name="create-new-intents-and-their-utterances"></a>Nieuwe intenties en de bijbehorende utterances maken

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Selecteer **Create new intent** op de pagina **Intents**. 

3. Voer in het pop-updialoogvenster `OrgChart-Manager` in en selecteer vervolgens **Done**.

    ![Pop-upvenster om een nieuw bericht te maken](media/luis-tutorial-pattern/hr-create-new-intent-popup.png)

4. Voeg voorbeelden van utterances toe aan de intentie.

    |Voorbeelden van utterances|
    |--|
    |Van wie is John W. Smith de ondergeschikte?|
    |Aan wie moet John W. Smith verantwoording afleggen?|
    |Wie is de manager van John W. Smith?|
    |Aan wie moet Jill Jones rechtstreeks verantwoording afleggen?|
    |Wie is de supervisor van Jill Jones?|

    [![Schermafbeelding van LUIS waarin nieuwe utterances worden toegevoegd aan intent](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png "Schermafbeelding van LUIS waarin nieuwe utterances worden toegevoegd aan intent")](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png#lightbox)

    Maakt u zich geen zorgen als in de utterances van de intent de keyPhrase-entiteit is gemarkeerd in plaats van de werknemer-entiteit. Beide zijn in het deelvenster Test en bij het eindpunt juist voorspeld. 

5. Selecteer **Intents** in de linkernavigatiebalk.

6. Selecteer **Create new intent**. 

7. Voer in het pop-updialoogvenster `OrgChart-Reports` in en selecteer vervolgens **Done**.

8. Voeg voorbeelden van utterances toe aan de intentie.

    |Voorbeelden van utterances|
    |--|
    |Wie zijn de ondergeschikten van John W. Smith?|
    |Wie legt verantwoording af bij John W. Smith?|
    |Van wie is John W. Smith de leidinggevende?|
    |Wie moeten er rechtstreeks verantwoording afleggen aan Jill Jones?|
    |Van wie is Jill Jones de supervisor?|

## <a name="caution-about-example-utterance-quantity"></a>Waarschuwing over hoeveelheid voorbeeld-utterances

[!include[Too few examples](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]

## <a name="train"></a>Trainen

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publiceren

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Intenties en entiteiten ophalen van eindpunt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

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

Is deze query gelukt? Voor deze trainingscyclus is het gelukt. De scores van de twee belangrijkste intenties liggen dicht bij elkaar. Omdat LUIS-training niet altijd precies hetzelfde is en er daarom kleine variaties kunnen ontstaan, kunnen deze twee scores bij de volgende trainingscyclus zijn omgekeerd. Het resultaat is dat de verkeerde intent kan worden geretourneerd. 

Gebruik patronen om de score van de juiste intent een aanzienlijk hoger percentage te geven en ervoor te zorgen dat de score verder weg ligt van de eerstvolgende hogere score. 

Laat dit tweede browservenster geopend. U hebt het verderop in deze zelfstudie namelijk weer nodig. 

## <a name="template-utterances"></a>Sjabloon-utterances
Vanwege de aard van het Human Resources-domein zijn er een paar algemene manieren om te vragen naar werknemersrelaties in organisaties. Bijvoorbeeld:

|Utterances|
|--|
|Aan wie moet Jill Jones verantwoording afleggen?|
|Wie legt er verantwoording af aan Jill Jones?|

Deze utterances liggen te dicht bij elkaar om de contextuele uniekheid van elk ervan te bepalen zonder veel utterance-voorbeelden te moeten verstrekken. Door een patroon voor een intent toe te voegen, leert LUIS algemene utterance-patronen voor een intent te herkennen zonder veel utterance-voorbeelden te moeten verstrekken. 

Dit zijn voorbeelden van sjabloon-utterances voor deze intent:

|Voorbeelden van sjabloon-utterances|betekenis van de syntaxis|
|--|--|
|Aan wie legt {Werknemer} verantwoording af[?]|interchangeable {Werknemer}, ignore [?]}|
|Wie legt er verantwoording af aan {Werknemer}[?]|interchangeable {Werknemer}, ignore [?]}|

De syntaxis `{Employee}` markeert de locatie van de entiteit in de sjabloon-utterance en geeft aan om welke entiteit het gaat. De optionele syntaxis `[?]` markeert woorden of leestekens die optioneel zijn. LUIS kijkt naar de utterance, waarbij de optionele tekst tussen de haakjes wordt genegeerd.

Hoewel de syntaxis er uitziet als reguliere expressies, gaat het hier niet om reguliere expressies. Alleen de accolades, `{}`, en de vierkant haakjes, `[]`, worden als syntaxis ondersteund. Ze kunnen tot maximaal twee niveaus worden genest.

Om ervoor te zorgen dat een patroon overeenkomt met een utterance, moeten eerst de entiteiten binnen de utterance worden vergeleken met de entiteiten in de sjabloon-utterance. De sjabloon helpt echter niet bij het voorspellen van entiteiten, alleen van intenties. 

**Hoewel u voor patronen minder voorbeeld-utterances hoeft op te geven, komt het patroon niet overeen als de entiteiten niet worden gedetecteerd.**

In deze zelfstudie voegt u twee nieuwe intenties toe: `OrgChart-Manager` en `OrgChart-Reports`. 

|Intent|Utterance|
|--|--|
|OrgChart-Manager|Aan wie moet Jill Jones verantwoording afleggen?|
|OrgChart-Reports|Wie legt er verantwoording af aan Jill Jones?|

Nadat LUIS een voorspelling naar de client-app heeft geretourneerd, kan de naam van de intent als functienaam in de client-app worden gebruikt en de werknemer-entiteit kan worden gebruikt als parameter voor deze functie.

```Javascript
OrgChartManager(employee){
    ///
}
```

Houd er rekening mee dat werknemers werden gemaakt in de [zelfstudie voor de lijstentiteit](luis-quickstart-intent-and-list-entity.md).

1. Selecteer **Build** in het bovenste menu.

2. Selecteer in het linkernavigatievenster, onder **Improve app performance**, de optie **Patterns**.

3. Selecteer de intent **OrgChart-Manager** en voer de volgende sjabloon-utterances in:

    |Sjabloon-utterances|
    |:--|
    |Van is {Werknemer} de ondergeschikte[?]|
    |Aan wie legt {Werknemer} verantwoording af[?]|
    |Wie is de manager van {Werknemer}[?]|
    |Aan wie legt {Werknemer} rechtstreeks verantwoording af[?]|
    |Wie is de supervisor van {Werknemer}[?]|
    |Wie is de baas van {Werknemer}[?]|

    Entiteiten met rollen maken gebruik van een syntaxis waarin de rolnaam is opgenomen. Deze komen aan de orde in een [afzonderlijke zelfstudie voor rollen](luis-tutorial-pattern-roles.md). 

    Als u de sjabloon-utterance typt, helpt LUIS u bij het invullen van de entiteit wanneer u de linkeraccolade `{` invoert.

    [![Schermafbeelding van het invoeren van sjabloon-utterances voor intent](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png)](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png#lightbox)

4. Selecteer de intent **OrgChart-Reports** en voer de volgende sjabloon-utterances in:

    |Sjabloon-utterances|
    |:--|
    |Wie zijn de ondergeschikten van {Werknemer}[?]|
    |Wie legt er verantwoording af aan {Werknemer}[?]|
    |Van wie is {Werknemer} de manager[?]|
    |Wie leggen er rechtstreeks verantwoording af aan {Werknemer}[?]|
    |Van wie is {Werknemer} de supervisor[?]|
    |Van wie is {Werknemer} de baas[?]|

## <a name="query-endpoint-when-patterns-are-used"></a>Query uitvoeren op eindpunt wanneer patronen worden gebruikt

1. De app opnieuw trainen en publiceren.

2. Browsertabbladen terugzetten naar het tabblad van de eindpunt-URL.

3. Ga naar het einde van de URL in het adres en voer `Who is the boss of Jill Jones?` in als utterance. De laatste parameter van de queryreeks is `q`, de utterance **query**. 

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

De intentvoorspelling is nu aanzienlijk hoger.

## <a name="working-with-optional-text-and-prebuilt-entities"></a>Werken met optionele tekst en voorafgemaakte entiteiten

De vorige patroonsjabloon-utterances in deze zelfstudie hadden enkele voorbeelden van optionele tekst `'s`, zoals het gebruik van het vraagteken, `?`. Stel dat de eindpunt-utterances laten zien dat managers en Human Resources-medewerkers op zoek zijn naar historische gegevens en naar geplande overplaatsingen van werknemers binnen het bedrijf die nog moeten plaatsvinden.

Voorbeelden van utterances zijn:

|Intent|Voorbeeld-utterances met optionele tekst en voorafgemaakte entiteiten|
|:--|:--|
|OrgChart-Manager|`Who was Jill Jones manager on March 3?`|
|OrgChart-Manager|`Who is Jill Jones manager now?`|
|OrgChart-Manager|`Who will be Jill Jones manager in a month?`|
|OrgChart-Manager|`Who will be Jill Jones manager on March 3?`|

In elk van deze voorbeelden wordt gebruikgemaakt van een werkwoordsvorm, `was`, `is`, `will be`, evenals een datum, `March 3`, `now`, en `in a month`, die LUIS correct moet voorspellen. U ziet dat in de laatste twee voorbeelden bijna dezelfde tekst wordt gebruikt, behalve voor `in` en `on`.

Voorbeeld van sjabloon-utterances:
|Intent|Voorbeeld-utterances met optionele tekst en voorafgemaakte entiteiten|
|:--|:--|
|OrgChart-Manager|`who was {Employee}['s] manager [[on]{datetimeV2}?`]|
|OrgChart-Manager|`who is {Employee}['s] manager [[on]{datetimeV2}?]`|
|OrgChart-Manager|`who will be {Employee}['s] manager [[in]{datetimeV2}?]`|
|OrgChart-Manager|`who will be {Employee}['s] manager [[on]{datetimeV2}?]`|

Dankzij het gebruik van de optionele syntaxis met vierkante haken, `[]`, kan deze optionele tekst eenvoudig worden toegevoegd aan de sjabloon-utterance en worden genest tot het tweede niveau met `[[]]`, en kunnen entiteiten of tekst worden opgenomen.

**Vraag: Waarom konden de laatste twee voorbeeld-utterances niet worden samengevoegd tot één sjabloon-utterance?** De patroonsjabloon biedt geen ondersteuning voor de OR-syntaxis. Om zowel de `in`- als de `on`-versie te kunnen afvangen, moet elk daarvan een afzonderlijke sjabloon-utterance zijn.

**Vraag: Waarom zijn alle letters `w`, als eerste letter in elke sjabloon-utterance, kleine letters? Moeten die niet naar keuze hoofdletters of kleine letters kunnen zijn?** De utterance die door de clienttoepassing naar het query-eindpunt wordt verzonden, wordt omgezet in kleine letters. De sjabloon-utterance kan uit hoofdletters of kleine letters bestaan en dat geldt ook voor de eindpunt-utterance. De vergelijking wordt altijd uitgevoerd na de conversie naar kleine letters.

**Vraag: Waarom maakt het vooraf gemaakte getal geen deel uit van de sjabloon-utterance als 3 maart wordt voorspeld als het getal `3` en als de datum `March 3`?** De sjabloon-utterance maakt contextueel gebruik van een datum, letterlijk zoals in `March 3`, of figuurlijk zoals `in a month`. Een datum kan een waarde bevatten, maar een getal hoeft niet per se als een datum te worden beschouwd. Gebruik altijd de entiteit die het beste het type aangeeft dat u wilt laten retourneren in de JSON-voorspellingsresultaten.  

**Vraag: Hoe zit het met slecht geformuleerde utterances zoals `Who will {Employee}['s] manager be on March 3?`.** Grammaticaal verschillende werkwoordsvormen, zoals deze waarbij `will` en `be` van elkaar zijn gescheiden, moeten als een nieuwe sjabloon-utterance worden ingesteld. De bestaande sjabloon-utterance zal een dergelijke werkwoordsvorm niet herkennen. Het doel van de utterance is weliswaar niet veranderd, maar dat geldt wel voor de woordplaatsing in de utterance. Deze wijziging heeft gevolgen voor de voorspelling in LUIS.

**Houd er rekening mee dat als eerste entiteiten worden gevonden, waarna het patroon wordt vergeleken.**

## <a name="edit-the-existing-pattern-template-utterance"></a>De bestaande patroonsjabloon-utterance bewerken

1. Selecteer op de website LUIS de optie **Build** in het bovenste menu en selecteer **Patterns** in het menu links. 

2. Zoek de bestaande sjabloon-utterance, `Who is {Employee}['s] manager[?]`, en selecteer het weglatingsteken (***...*** ) aan de rechterkant. 

3. Selecteer **Edit** in het pop-upmenu. 

4. Wijzig de sjabloon-utterance in: `who is {Employee}['s] manager [[on]{datetimeV2}?]]`

## <a name="add-new-pattern-template-utterances"></a>Nieuwe patroonsjabloon-utterances toevoegen

1. Terwijl u zich nog steeds in de sectie **Patterns** van **Build** bevindt, voegt u verschillende nieuwe patroonsjabloon-utterances toe. Selecteer **OrgChart-Manager** in de vervolgkeuzelijst Intent en voer elk van de volgende sjabloon-utterances in:

    |Intent|Voorbeeld-utterances met optionele tekst en voorafgemaakte entiteiten|
    |--|--|
    |OrgChart-Manager|`who was {Employee}['s] manager [[on]{datetimeV2}?]`|
    |OrgChart-Manager|`who is {Employee}['s] manager [[on]{datetimeV2}?]`|
    |OrgChart-Manager|`who will be {Employee}['s] manager [[in]{datetimeV2}?]`|
    |OrgChart-Manager|`who will be {Employee}['s] manager [[on]{datetimeV2}?]`|

2. Train de app.

3. Selecteer **Test** aan de bovenkant van het deelvenster om de testvenster te openen. 

4. Voer verschillende test-utterances in om te controleren of het patroon overeenkomt en de intentiescore hoog genoeg is. 

    Nadat u de eerste utterance hebt ingevoerd, selecteert u **Inspect** onder het resultaat zodat u alle voorspellingsresultaten kunt zien.

    |Utterance|
    |--|
    |Wie wordt Jill Jones' manager|
    |wie wordt Jill Jones' manager|
    |Wie wordt Jill Jones' manager?|
    |wie wordt op 3 maart Jill Jones' manager|
    |Wie wordt volgende maand Jill Jones' manager|
    |Wie wordt Jill Jones' manager over een maand?|

Voor al deze utterances zijn de daarin opgenomen entiteiten gevonden en daarom komen ze overeen met hetzelfde patroon en hebben ze een hoge voorspellingsscore.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie worden twee intenties toegevoegd voor utterances die zonder veel voorbeeld-utterances moeilijk met grote nauwkeurigheid te voorspellen waren. Door hiervoor patronen toe te voegen kon LUIS de intentie beter voorspellen met een aanzienlijk hogere score. Door entiteiten en te negeren tekst te markeren kon LUIS het patroon toepassen op meer verschillende utterances.

> [!div class="nextstepaction"]
> [Informatie over het gebruik van rollen met een patroon](luis-tutorial-pattern-roles.md)