---
title: 'Zelfstudie 1: Intenties zoeken in aangepaste LUIS-app'
titleSuffix: Azure Cognitive Services
description: Maak een aangepaste app waarmee de intentie van een gebruiker wordt voorspeld. Deze app is het eenvoudigste type LUIS-app omdat de app geen verschillende gegevenselementen uit de utterancetekst extraheert, zoals e-mailadressen of datums.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: cc631f139e7f11d82f2caac83770754060353d07
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277712"
---
# <a name="tutorial-1-build-custom-app-to-determine-user-intentions"></a>Zelfstudie 1: Aangepaste app compileren om gebruikersintents te bepalen

In deze zelfstudie maakt u een aangepaste HR-app (Human Resources) waarmee de intentie van een gebruiker wordt voorspeld op basis van de utterance (tekst). Wanneer u klaar bent, hebt u een LUIS-eindpunt in de cloud.

Het doel van de app is om de intentie van natuurlijke omgangstaal in een tekst te bepalen. Deze intenties worden onderverdeeld in **Intents**. Deze app heeft enkele intents. De eerste intent, **`GetJobInformation`**, stelt vast wanneer een gebruiker informatie wil over vacatures binnen een bedrijf. De tweede intent, **`None`**, wordt gebruikt voor utterances van de gebruiker die zich buiten het _domein_ (bereik) van deze app bevinden. Later wordt er een derde intent, **`ApplyForJob`**, toegevoegd voor alle utterances over solliciteren naar een functie. Deze derde intent wijkt af van `GetJobInformation` omdat de informatie over de functie al bekend moet zijn wanneer iemand naar die functie solliciteert. Maar afhankelijk van de woordkeuze kan het moeilijk zijn de juiste intent te bepalen omdat beide betrekking hebben op een functie.

Nadat LUIS de JSON-reactie heeft geretourneerd, is LUIS klaar met deze aanvraag. LUIS geeft geen antwoord op deze utterances van gebruikers, maar stelt alleen vast om wat voor soort informatie er wordt gevraagd in natuurlijke taal. 

**In deze zelfstudie leert u het volgende:**

> [!div class="checklist"]
> * Een nieuwe app maken 
> * Intents maken
> * Voorbeelden van utterances toevoegen
> * App trainen
> * App publiceren
> * Intent van eindpunt ophalen

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="create-a-new-app"></a>Een nieuwe app maken

1. Meld u aan bij de LUIS-portal met de URL [https://www.luis.ai](https://www.luis.ai). 

2. Selecteer **Create new app**.  

    [![](media/luis-quickstart-intents-only/app-list.png "Schermafbeelding van de pagina My Apps van Language Understanding (LUIS)")](media/luis-quickstart-intents-only/app-list.png#lightbox)

3. Typ de naam in het pop-updialoogvenster `HumanResources` en houd de standaardcultuur, **Engels**, aan. Laat de beschrijving leeg.

    ![Nieuwe LUIS-app](./media/luis-quickstart-intents-only/create-app.png)

    Vervolgens geeft de app de pagina **Intents** weer met de intent **None**.

## <a name="getjobinformation-intent"></a>GetJobInformation-intent

1. Selecteer **Create new intent**. Voer de naam `GetJobInformation` in voor de nieuwe intent. Deze intent wordt voorspeld telkens wanneer een gebruiker informatie wil over vacatures in het bedrijf.

    ![](media/luis-quickstart-intents-only/create-intent.png "Schermafbeelding van het dialoogvenster New intent van Language Understanding (LUIS) My Apps")

2. Door _voorbeelden van utterances_ op te geven traint u LUIS met het soort utterances dat voor deze intent kan worden voorspeld. Voeg verschillende voorbeelden van utterances toe aan deze intent waarnaar een gebruiker waarschijnlijk zal vragen, zoals:

    | Voorbeelden van utterances|
    |--|
    |Zijn er vandaag nieuwe vacatures geplaatst?|
    |Welke vacatures bestaan er voor hoofdtechnici?|
    |Zijn er banen beschikbaar voor het werken met databases?|
    |Op zoek naar een nieuwe werkomgeving met verantwoordelijkheden als boekhouder|
    |Waar is de vacaturelijst|
    |Nieuwe vacatures?|
    |Zijn er nieuwe vacatures in het kantoor in Seattle?|

    [![](media/luis-quickstart-intents-only/utterance-getstoreinfo.png "Schermopname van het invoeren van nieuwe utterances voor MyStore-intent")](media/luis-quickstart-intents-only/utterance-getstoreinfo.png#lightbox)

    [!INCLUDE[Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]    


## <a name="none-intent"></a>None- intent 
De clienttoepassing moet weten of een utterance buiten het onderwerpsdomein van de toepassing valt. Als LUIS de **None**-intent retourneert voor een utterance, kan uw clienttoepassing vragen of de gebruiker het gesprek wil beëindigen. De clienttoepassing kan ook meer aanwijzingen geven voor het vervolgen van het gesprek als de gebruiker dit niet wil beëindigen. 

Deze voorbeeldutterances, buiten het onderwerpsdomein, worden ondergebracht in de **None**-intent. U mag deze intent niet leeglaten. 

1. Selecteer **Intents** in het linkerpaneel.

2. Selecteer de intent **None**. Voeg drie utterances toe die een gebruiker misschien kan invoeren, maar die niet relevant zijn voor uw HR-app. Als de app betrekking heeft op uw vacatures, zijn dit enkele **None**-utterances:

    | Voorbeelden van utterances|
    |--|
    |Blaffende honden zijn irritant|
    |Bestel een pizza voor me|
    |Pinguïns in de oceaan|


## <a name="train"></a>Trainen 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publiceren

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent"></a>Intentie ophalen

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Ga naar het einde van de URL in de adresbalk en voer `I'm looking for a job with Natural Language Processing` in. De laatste parameter van de queryreeks is `q`, de utterance**query**. Deze utterance is niet hetzelfde als een van de voorbeeldutterances. Dit is een goede test die de `GetJobInformation`-intent als de best scorende intent moet retourneren. 

    ```JSON
    {
      "query": "I'm looking for a job with Natural Language Processing",
      "topScoringIntent": {
        "intent": "GetJobInformation",
        "score": 0.8965092
      },
      "intents": [
        {
          "intent": "GetJobInformation",
          "score": 0.8965092
        },
        {
          "intent": "None",
          "score": 0.147104025
        }
      ],
      "entities": []
    }
    ```

    De resultaten bevatten **alle intents** in de app; momenteel zijn dat er 2. De entiteitenmatrix is leeg omdat deze app momenteel geen entiteiten bevat. 

    Het JSON-resultaat identificeert de hoogst scorende intent als **`topScoringIntent`**-eigenschap. Alle scores liggen tussen 1 en 0, met de betere score dichterbij 1. 

## <a name="applyforjob-intent"></a>ApplyForJob-intent
Ga terug naar de website van LUIS en maak een nieuwe intent om te bepalen of de gebruikersutterance over het solliciteren naar een functie gaat.

1. Selecteer **Build** in het menu rechtsboven om terug te keren naar het compileren van apps.

2. Selecteer **Intents** in het linkermenu.

3. Selecteer **Create new intent** en voer de naam `ApplyForJob` in. 

    ![LUIS-dialoogvenster om nieuwe intent te maken](./media/luis-quickstart-intents-only/create-applyforjob-intent.png)

4. Voeg verschillende utterances toe aan deze intent waarnaar een gebruiker waarschijnlijk zal vragen, zoals:

    | Voorbeelden van utterances|
    |--|
    |Ik wil solliciteren op de nieuwe functie van boekhouder|
    |Sollicitatieformulier invullen voor vacature 123456|
    |Cv indienen voor vacature van engineer|
    |Hier is mijn cv voor vacature 654234|
    |Vacature 567890 en mijn papierwerk|

    [![](media/luis-quickstart-intents-only/utterance-applyforjob.png "Schermopname van het invoeren van nieuwe utterances voor ApplyForJob-intent")](media/luis-quickstart-intents-only/utterance-applyforjob.png#lightbox)

    De gelabelde intent is rood omlijnd omdat LUIS momenteel niet zeker weet of de intent juist is. Door de app te trainen weet LUIS wanneer de utterances voor de juiste intent zijn. 

## <a name="train-again"></a>Opnieuw trainen

[!INCLUDE[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-again"></a>Opnieuw publiceren

[!INCLUDE[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent-again"></a>Intent opnieuw ophalen

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Voer in het nieuwe browservenster `Can I submit my resume for job 235986` in aan het einde van de URL. 

    ```JSON
    {
      "query": "Can I submit my resume for job 235986",
      "topScoringIntent": {
        "intent": "ApplyForJob",
        "score": 0.9166808
      },
      "intents": [
        {
          "intent": "ApplyForJob",
          "score": 0.9166808
        },
        {
          "intent": "GetJobInformation",
          "score": 0.07162977
        },
        {
          "intent": "None",
          "score": 0.0262826588
        }
      ],
      "entities": []
    }
    ```

    De resultaten bevatten de nieuwe intent **ApplyForJob** samen met de bestaande intents. 

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u de app Human Resources (HR) gemaakt, hebt u 2 intents gemaakt, voorbeeldutterances aan elke intent toegevoegd, voorbeeldutterances aan de None-intent toegevoegd, de app getraind, gepubliceerd en getest op het eindpunt. Dit zijn de basisstappen voor het compileren van een LUIS-model. 

> [!div class="nextstepaction"]
> [Vooraf gemaakte intents en entiteiten aan deze app toevoegen](luis-tutorial-prebuilt-intents-entities.md)
