---
title: Zelfstudie over het gebruiken van patronen voor het verbeteren van LUIS voorspellingen - Azure | Microsoft Docs
titleSuffix: Cognitive Services
description: In deze zelfstudie, gebruikt u patroon voor intents voor het verbeteren van LUIS intentie en entiteit voorspellingen.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 07/20/2018
ms.author: v-geberr;
ms.openlocfilehash: 5d486272f7f713c5d4e6f7b598073c5c09875d43
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39172463"
---
# <a name="tutorial-improve-app-with-patterns"></a>Zelfstudie: Een app met patronen verbeteren

In deze zelfstudie gebruiken u patronen voor het verhogen van de intentie en entiteit voorspelling.  

> [!div class="checklist"]
* Identificeren dat een patroon waarmee uw app
* Over het maken van een patroon 
* Patroon voorspelling verbeteringen controleren

Voor dit artikel hebt u een gratis [LUIS](luis-reference-regions.md)-account nodig om uw LUIS-toepassing te creëren.

## <a name="before-you-begin"></a>Voordat u begint
Als u geen de Human Resources-app vanuit de [batch test](luis-tutorial-batch-testing.md) zelfstudie [importeren](luis-how-to-start-new-app.md#import-new-app) de JSON naar een nieuwe app in de [LUIS](luis-reference-regions.md#luis-website) website. De app voor het importeren is gevonden in de [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-batchtest-HumanResources.json) GitHub-opslagplaats.

Als u de oorspronkelijke Human Resources-app wilt gebruiken, kloont u de versie op de pagina [Settings](luis-how-to-manage-versions.md#clone-a-version) en wijzigt u de naam in `patterns`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. 

## <a name="patterns-teach-luis-common-utterances-with-fewer-examples"></a>Patronen leren LUIS algemene uitingen met minder voorbeelden
Vanwege de aard van het Human resources-domein, moet u er een paar algemene manieren om vragen over werknemer relaties in organisaties zijn. Bijvoorbeeld:

```
Who does Jill Jones report to?
Who reports to Jill Jones? 
```

Deze uitingen zijn te dicht bij de contextuele Uniekheid van elk zonder op te geven veel utterance voorbeelden bepalen. Door toe te voegen een patroon voor een doel, LUIS algemene utterance-patronen leert herkennen voor een doel zonder op te geven veel utterance voorbeelden. 

Voorbeeld van de sjabloon uitingen hiervoor intentie zijn:

```
Who does {Employee} report to?
Who reports to {Employee}? 
```

Het patroon wordt geboden via een sjabloon utterance-voorbeeld, waarin u de syntaxis voor het identificeren van entiteiten en ignorable tekst bevat. Een patroon is een combinatie van reguliere expressie die overeenkomen met en machine learning.  Het voorbeeld van een sjabloon utterance, samen met de intentie uitingen geven LUIS een beter beeld van welke uitingen past het doel.

Opdat een patroon worden vergeleken met een utterance, moeten de entiteiten in de utterance eerst de entiteiten in de sjabloon utterance overeenkomen. Echter, de sjabloon niet helpen bij het voorspellen entiteiten, alleen intents. 

**Terwijl patronen u minder voorbeeld uitingen kunnen, als de entiteiten niet worden gedetecteerd, wordt het patroon komt niet overeen.**

Houd er rekening mee dat werknemers zijn gemaakt de [lijst entiteit zelfstudie](luis-quickstart-intent-and-list-entity.md).

## <a name="create-new-intents-and-their-utterances"></a>Nieuwe intents en hun uitingen maken
Twee nieuwe intents toevoegen: `OrgChart-Manager` en `OrgChart-Reports`. LUIS retourneert één keer een voorspelling op de clientapp, de naam van de intentie kan worden gebruikt als de naam van een functie in de client-app en dat de werknemer-entiteit kan worden gebruikt als een parameter voor deze functie.

```
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

    [![](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png "Schermafbeelding van LUIS toe te voegen nieuwe uitingen naar intentie")](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png#lightbox)

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
De nieuwe intentie en uitingen vereisen training. 

1. Selecteer rechtsboven op de website van LUIS de knop **Train**.

    ![Afbeelding van de knop Train](./media/luis-tutorial-pattern/hr-train-button.png)

2. Het trainen is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het trainen is gelukt.

    ![Afbeelding van melding dat het trainen is voltooid](./media/luis-tutorial-pattern/hr-trained-inline.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>App publiceren om eindpunt-URL op te vragen
Om LUIS een voorspelling te laten geven in een chatbot of een andere toepassing, moet u de app publiceren. 

1. Selecteer rechtsboven op de website van LUIS de knop **Publish**. 

2. Selecteer de slot Production en vervolgens de knop **Publish**.

    [ ![Schermafbeelding van publiceren pagina publiceren naar productie sleuf knop gemarkeerd](./media/luis-tutorial-pattern/hr-publish-to-production.png)](./media/luis-tutorial-pattern/hr-publish-to-production.png#lightbox)

3. Het publiceren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het publiceren is gelukt.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Eindpunt opvragen met een andere utterance
1. Selecteer onder aan de pagina **Publish** de koppeling **endpoint**. Er wordt nu een nieuw browservenster geopend, met de eindpunt-URL in de adresbalk. 

    [ ![Schermafbeelding van publiceren-pagina met eindpunt-URL gemarkeerd](./media/luis-tutorial-pattern/hr-publish-select-endpoint.png)](./media/luis-tutorial-pattern/hr-publish-select-endpoint.png#lightbox)


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
    
    Entiteiten met functies gebruikt met de naam van de rol, en worden behandeld in een [afzonderlijke zelfstudie voor rollen](luis-tutorial-pattern-roles.md). 

    De syntaxis van de optionele `[]`, markeert u woorden of leestekens die optioneel zijn. LUIS komt overeen met de utterance, de optionele tekst tussen haakjes worden genegeerd.

    Als u de sjabloon utterance, LUIS kunt u in de entiteit bij het invoeren van de accolade links `{`.

    [ ![Schermafbeelding van het invoeren van de sjabloon-uitingen voor doel](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png)](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png#lightbox)



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

2. Selecteer onder aan de pagina **Publish** de koppeling **endpoint**. Er wordt nu een nieuw browservenster geopend, met de eindpunt-URL in de adresbalk. 

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

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u de LUIS-app niet meer nodig hebt, kunt u deze verwijderen. Om dit te doen, selecteer het weglatingsteken (***...*** ) aan de rechterkant van de naam van de app in de lijst met Apps, selecteer **verwijderen**. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het gebruik van functies met een patroon](luis-tutorial-pattern-roles.md)