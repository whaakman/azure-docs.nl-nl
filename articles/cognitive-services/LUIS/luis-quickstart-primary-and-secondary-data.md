---
title: 'Zelfstudie 7: Eenvoudige entiteit met woordgroepenlijst in LUIS'
titleSuffix: Azure Cognitive Services
description: Via machine learning verkregen gegevens ophalen uit een utterance
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 941c29506aa8f17dcb6262495b28dd26e78194d5
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47036050"
---
# <a name="tutorial-7-extract-names-with-simple-entity-and-phrase-list"></a>Zelfstudie 7: Namen met een eenvoudige entiteit en woordenlijst extraheren

In deze zelfstudie haalt u via machine learning verkregen gegevens over een functienaam op uit een utterance met behulp van de entiteit **Simple**. Als u de nauwkeurigheid van het extraheren wilt vergroten, voegt u een woordenlijst toe met termen die specifiek gekoppeld zijn aan de Simple-entiteit.

Tijdens deze zelfstudie wordt een nieuwe Simple-entiteit toegevoegd waarmee de functienaam kan worden geëxtraheerd. Het doel van de entiteit Simple in deze LUIS-app is om LUIS te leren wat een functienaam is en waar dit kan worden gevonden in een utterance. Het deel van de utterance dat de functienaam vormt, kan per utterance verschillen afhankelijk van woordkeuze en lengte van de utterance. LUIS heeft voorbeelden nodig van functienamen uit alle intents waarin gebruik wordt gemaakt van functienamen.  

Een Simple-entiteit is een goede keuze voor dit type gegevens wanneer:

* Gegevens één concept vormen.
* Gegevens niet goed zijn opgemaakt als een reguliere expressie.
* Gegevens niet van algemene aard zijn zoals een vooraf gedefinieerde entiteit van een telefoonnummer of gegevens.
* Gegevens niet precies overeenkomen met een lijst van bekende woorden, zoals een lijstentiteit.
* Gegevens geen andere gegevensitems bevatten zoals een samengestelde entiteit of hiërarchische entiteit.

**In deze zelfstudie leert u het volgende:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Bestaande zelfstudie-app gebruiken
> * Een entiteit Simple maken om taken te extraheren uit een app
> * Woordgroepenlijst toevoegen om signalering van taakwoorden te verbeteren
> * Trainen 
> * Publiceren 
> * Intenties en entiteiten ophalen van eindpunt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Bestaande app gebruiken

Ga door met de in de laatste zelfstudie gemaakt app, **HumanResources**. 

Als u niet over de app HumanResources uit de vorige zelfstudie beschikt, voert u de volgende stappen uit:

1.  Download het [JSON-bestand van de app](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-composite-HumanResources.json) en sla het op.

2. Importeer de JSON in een nieuwe app.

3. Ga naar het gedeelte **Beheren**, open het tabblad **Versies**, kloon de versie en noem deze `simple`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. Omdat de versienaam wordt gebruikt als onderdeel van de URL-route, kan de naam geen tekens bevatten die niet zijn toegestaan in een URL.

## <a name="simple-entity"></a>Simple-entiteit
De Simple-entiteit detecteert een concept met enkelvoudige gegevens dat is opgenomen in woorden of woordgroepen.

Laten we de volgende utterances uit een chatbot als voorbeeld nemen:

|Utterance|Extraheerbare functienaam|
|:--|:--|
|Ik wil solliciteren op de nieuwe functie als boekhouder.|boekhouder|
|Verzend mijn CV voor de functie van technicus.|technicus|
|Sollicitatieformulier invullen voor vacature 123456|123456|

Het is moeilijk om te bepalen wat de functienaam is omdat dit een zelfstandig naamwoord, werkwoord of een woordgroep van meerdere woorden kan zijn. Bijvoorbeeld:

|Taken|
|--|
|technicus|
|software-engineer|
|senior software-engineer|
|manager technisch team |
|luchtverkeersleider|
|automonteur|
|ambulancebestuurder|
|operateur|
|onderhoudsmonteur|
|monteur voor draaiende machineonderdelen|

In deze LUIS-app komen functienamen in verschillende intenties voor. Door deze woorden in alle utterances binnen intents van een label te voorzien, leert LUIS meer over wat elke functienaam is en waar deze kan worden gevonden in utterances.

Zodra de entiteiten in de voorbeeldutterances zijn gemarkeerd, is het belangrijk dat u een woordgroepenlijst toevoegt om het signaal van de Simple-entiteit te verbeteren. Een woordgroepenlijst wordt **niet** gebruikt als een exacte overeenkomst en hoeft niet elke mogelijke waarde te zijn die u verwacht. 

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Selecteer op de pagina **Intent** de intentie **ApplyForJob**. 

3. In de utterance `I want to apply for the new accounting job` selecteert u `accounting`, voert u `Job` in het bovenste veld van het snelmenu in en selecteert u **Create new entity** in het snelmenu. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-entity.png "Schermopname van LUIS met de intentie ApplyForJob en de stappen voor het creëren van een entiteit gemarkeerd")](media/luis-quickstart-primary-and-secondary-data/hr-create-entity.png#lightbox)

4. Controleer de naam en het type van de entiteit in het pop-upvenster en selecteer **Done**.

    ![Een modaal pop-updialoogvenster voor een entiteit Simple creëren met de naam van de functie en het type Simple](media/luis-quickstart-primary-and-secondary-data/hr-create-simple-entity-popup.png)

5. In de utterance `Submit resume for engineering position` labelt u het woord `engineering` als een entiteit Job. Selecteer het woord `engineering` en selecteer **Job** in het snelmenu. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-label-simple-entity.png "Schermopname van LUIS waarin het labelen van de entiteit Job is gemarkeerd")](media/luis-quickstart-primary-and-secondary-data/hr-label-simple-entity.png#lightbox)

    Alle utterances zijn gelabeld maar vijf utterances zijn niet voldoende om LUIS te leren welke woorden en woordgroepen gerelateerd zijn aan de functie. De functies die gebruikmaken van de cijferwaarde hebben niet meer voorbeelden nodig omdat deze van een reguliere expressie gebruikmaken. Van functies die uit woorden of woordgroepen bestaan, moeten ten minste 15 meer voorbeelden bestaan. 

6. Voeg meer utterances toe en markeer de woorden en woordgroepen als entiteit **Job**. De functietypen zijn algemeen voor aanstellingen van een aanstellingsservice. Als u functies voor een specifieke bedrijfstak had willen hebben, moet dat duidelijk worden uit de woorden voor de functie. 

    |Utterance|De entiteit Job|
    |:--|:--|
    |Ik solliciteer voor de functie van programmamanager op de afdeling Onderzoek en Ontwikkeling|Programmamanager|
    |Hier is mijn sollicitatiebrief naar de functie van kok.|kok|
    |Mijn cv voor de functie van kampleider heb ik bijgesloten.|kampleider|
    |Dit is mijn cv voor de functie van administratief medewerker.|administratief medewerker|
    |Ik wil graag solliciteren op de managementfunctie op de verkoopafdeling.|management, verkoopafdeling|
    |Dit is mijn cv voor de nieuwe functie als boekhouder.|boekhouder|
    |Mijn sollicitatiebrief voor de functie van barmedewerker is bijgesloten.|barmedewerker|
    |Ik wil met deze brief solliciteren naar de functie van dakdekker en glaszetter.|dakdekker, glaszetter|
    |Mijn cv voor de functie van buschauffeur vindt u hier.|buschauffeur|
    |Ik ben een gediplomeerd verpleegkundige. Hier is mijn cv.|gediplomeerd verpleegkundige|
    |Ik wil u hierbij mijn sollicatieformulieren aanbieden voor de positie als docent die ik in de krant heb gelezen.|docent|
    |Dit is mijn cv voor de functie van vakkenvuller op de afdeling Fruit en groenten.|vakkenvuller|
    |Solliciteren als tegelzetter.|tegelzetter|
    |Bijgevoegd cv voor landschapsarchitect.|landschapsarchitect|
    |Mijn cv voor de functie als biologiedocent is bijgesloten.|biologiedocent|
    |Ik wil graag solliciteren naar een functie binnen het vakgebied fotografie.|fotografie|git 

## <a name="label-entity-in-example-utterances"></a>Entiteit in voorbeeldutterances labelen

Door de entiteit te labelen, of te _markeren_, weet LUIS waar de entiteit in de voorbeeldutterances is te vinden.

1. Selecteer **Intents** in het linkermenu.

2. Selecteer **GetJobInformation** in de lijst met intenties. 

3. Label de functies in de voorbeelden van utterances:

    |Utterance|De entiteit Job|
    |:--|:--|
    |Zijn er banen beschikbaar in het werken met databases?|databases|
    |Op zoek naar een nieuwe werkomgeving met verantwoordelijkheden als boekhouder|boekhouder|
    |Welke vacatures bestaan er voor hoofdtechnici?|hoofdtechnici|

    Er zijn nog meer voorbeelden van utterances maar die bevatten geen woorden met betrekking tot functies.

## <a name="train"></a>Trainen

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publiceren

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Intenties en entiteiten ophalen van eindpunt 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Ga naar het einde van de URL in het adres en voer `Here is my c.v. for the programmer job` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. Deze utterance is niet hetzelfde als een van de gelabelde utterances en dit is dus een goede test die de utterances van `ApplyForJob` als resultaat moet geven.

    ```JSON
    {
      "query": "Here is my c.v. for the programmer job",
      "topScoringIntent": {
        "intent": "ApplyForJob",
        "score": 0.9826467
      },
      "intents": [
        {
          "intent": "ApplyForJob",
          "score": 0.9826467
        },
        {
          "intent": "GetJobInformation",
          "score": 0.0218927357
        },
        {
          "intent": "MoveEmployee",
          "score": 0.007849265
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.00349470088
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.00348804821
        },
        {
          "intent": "None",
          "score": 0.00319909188
        },
        {
          "intent": "FindForm",
          "score": 0.00222647213
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00211193133
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00172086991
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.00138010911
        }
      ],
      "entities": [
        {
          "entity": "programmer",
          "type": "Job",
          "startIndex": 24,
          "endIndex": 33,
          "score": 0.5230502
        }
      ]
    }
    ```
    
    LUIS heeft de juiste intent, **ApplyForJob**, gevonden en de juiste entiteit, **Job**, uitgepakt met een waarde van `programmer`.


## <a name="names-are-tricky"></a>Namen zijn verraderlijk
De LUIS-app heeft weliswaar de juiste intentie gevonden met een hoge betrouwbaarheidsfactor en heeft de functienaam geëxtraheerd, maar namen zijn verraderlijk. Probeer het met utterance `This is the lead welder paperwork`.  

In de volgende JSON antwoordt LUIS met de juiste intentie, `ApplyForJob`, maar is de functienaam `lead welder` niet geëxtraheerd. 

```JSON
{
  "query": "This is the lead welder paperwork.",
  "topScoringIntent": {
    "intent": "ApplyForJob",
    "score": 0.468558252
  },
  "intents": [
    {
      "intent": "ApplyForJob",
      "score": 0.468558252
    },
    {
      "intent": "GetJobInformation",
      "score": 0.0102701457
    },
    {
      "intent": "MoveEmployee",
      "score": 0.009442534
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00639619166
    },
    {
      "intent": "None",
      "score": 0.005859333
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.005087704
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.00315379258
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00259344373
    },
    {
      "intent": "FindForm",
      "score": 0.00193389168
    },
    {
      "intent": "Utilities.Confirm",
      "score": 0.000420796918
    }
  ],
  "entities": []
}
```

Omdat alles een naam kan zijn, is LUIS beter in staat om entiteiten te herkennen als er een woordgroepenlijst bestaat die het signaleringsvermogen versterkt.

## <a name="to-boost-signal-add-phrase-list"></a>Versterk het signaal door een woordgroepenlijst toe te voegen

Open [jobs-phrase-list.csv](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/job-phrase-list.csv) in de GitHub-opslagplaats met LUIS-voorbeelden. De lijst bevat meer dan duizend woorden en woordgroepen met betrekking tot functies. Zoek in de lijst naar woorden met betrekking tot functies die relevant voor uw zijn. Als de woorden of woordgroepen waar u naar op zoek bent niet in de lijst voorkomen, kunt u deze zelf toevoegen.

1. In de sectie **Build** van de LUIS-app, selecteert u **Phrase lists** onder het menu **Improve app performance**.

2. Selecteer **Create new phrase list**. 

3. Noem de nieuwe woordgroepenlijst `Job` en kopieer de lijst van jobs-phrase-list.csv naar het tekstvak voor het invoeren van **waarden**. Selecteer Enter. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png "Schermopname van het pop-updialoogvenster voor het maken van een nieuwe woordgroepenlijst")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png#lightbox)

    Als u meer woorden aan de woordgroepenlijst wilt toevoegen, bekijkt u de **Gerelateerde waarden** en voegt u de woorden toe die relevant voor u zijn. 

4. Selecteer **Save** om de woordgroepenlijst te activeren.

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png "Schermopname van het pop-updialoogvenster met woorden in het vak voor waarden")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png#lightbox)

5. U moet de app opnieuw [trainen](#train-the-luis-app) en [publiceren](#publish-the-app-to-get-the-endpoint-URL) om de woordgroepenlijst te kunnen gebruiken.

6. Een eindpunt opnieuw opvragen met de dezelfde utterance: `This is the lead welder paperwork.`

    Het JSON-antwoord bevat de geëxtraheerde entiteit:

    ```JSON
    {
        "query": "This is the lead welder paperwork.",
        "topScoringIntent": {
            "intent": "ApplyForJob",
            "score": 0.920025647
        },
        "intents": [
            {
            "intent": "ApplyForJob",
            "score": 0.920025647
            },
            {
            "intent": "GetJobInformation",
            "score": 0.003800706
            },
            {
            "intent": "Utilities.StartOver",
            "score": 0.00299335527
            },
            {
            "intent": "MoveEmployee",
            "score": 0.0027167045
            },
            {
            "intent": "None",
            "score": 0.00259556063
            },
            {
            "intent": "FindForm",
            "score": 0.00224019377
            },
            {
            "intent": "Utilities.Stop",
            "score": 0.00200693542
            },
            {
            "intent": "Utilities.Cancel",
            "score": 0.00195913855
            },
            {
            "intent": "Utilities.Help",
            "score": 0.00162656687
            },
            {
            "intent": "Utilities.Confirm",
            "score": 0.0002851904
            }
        ],
        "entities": [
            {
            "entity": "lead welder",
            "type": "Job",
            "startIndex": 12,
            "endIndex": 22,
            "score": 0.8295959
            }
        ]
    }
    ```

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie gebruikt de app Human Resources een via machine learning verkregen Simple-entiteit om functienamen te vinden in utterances. Omdat functienamen tal van verschillende beschrijvingen kunnen hebben, had de app een woordgroepenlijst nodig om de woorden voor functienamen sneller te kunnen genereren. 

> [!div class="nextstepaction"]
> [Een vooraf gemaakte entiteit KeyPhrase toevoegen](luis-quickstart-intent-and-key-phrase.md)