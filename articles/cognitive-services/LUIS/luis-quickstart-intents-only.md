---
title: Een eenvoudige app met twee intents maken - Azure | Microsoft Docs
description: In deze snelstart leert u hoe u een eenvoudige LUIS-app maakt met behulp van twee 'intents' en geen entiteiten om 'utterances' van gebruikers te identificeren.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 06/27/2018
ms.author: v-geberr
ms.openlocfilehash: 0668ba050a6918995deb42d8feea5afbbab3b010
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865808"
---
# <a name="tutorial-1-build-app-with-custom-domain"></a>Zelfstudie: 1. Web-app met aangepast domein
In deze zelfstudie maakt u een app die laat zien hoe u **intents** gebruikt om de _bedoeling_ van de gebruiker te bepalen op basis van de utterance (tekst) die ze naar de app verzenden. Wanneer u klaar bent, hebt u een LUIS-eindpunt in de cloud.

Deze app is het eenvoudigste type LUIS-app omdat dit type geen gegevens uit de utterances ophaalt. Dit type bepaalt alleen wat de gebruiker bedoelt met de utterance.

<!-- green checkmark -->
> [!div class="checklist"]
> * Een nieuwe app voor een Human Resources (HR)-domein maken 
> * GetJobInformation-intent toevoegen
> * Voorbeeldutterances toevoegen aan de GetJobInformation-intent 
> * App trainen en publiceren
> * Eindpunt van app opvragen om JSON-antwoord van LUIS te zien
> * ApplyForJob-intent toevoegen
> * Voorbeeldutterances toevoegen aan de ApplyForJob-intent 
> * Eindpunt opnieuw trainen, publiceren en opvragen 

Voor dit artikel hebt u een gratis [LUIS](luis-reference-regions.md#luis-website)-account nodig om uw LUIS-toepassing te creëren.

## <a name="purpose-of-the-app"></a>Doel van de app
Deze app heeft enkele intents. De eerste intent, **`GetJobInformation`**, stelt vast wanneer een gebruiker informatie wil over vacatures binnen een bedrijf. De tweede intent, **`None`**, stelt elk ander type utterance vast. Verderop in de snelstart wordt een derde intent, `ApplyForJob`, toegevoegd. 

## <a name="create-a-new-app"></a>Een nieuwe app maken
1. Meld u aan op de website van [LUIS](luis-reference-regions.md#luis-website). Doe dit bij de [regio](luis-reference-regions.md#publishing-regions) waarin u de LUIS-eindpunten wilt publiceren.

2. Selecteer op de website van [LUIS](luis-reference-regions.md#luis-website) de optie **Create new app**.  

    [![](media/luis-quickstart-intents-only/app-list.png "Schermopname van pagina My Apps")](media/luis-quickstart-intents-only/app-list.png#lightbox)

3. Typ in het pop-upvenster de naam `HumanResources`. Deze app handelt vragen af over de afdeling Human Resources van uw bedrijf. Dit type afdeling verwerkt kwesties met betrekking tot werkgelegenheid zoals posities in het bedrijf die moeten worden ingevuld.

    ![Nieuwe LUIS-app](./media/luis-quickstart-intents-only/create-app.png)

4. Als dat proces is voltooid, ziet u de pagina **Intents** met de intent **None**. 

    [![](media/luis-quickstart-intents-only/intents-list.png "Schermopname van de lijst met intents")](media/luis-quickstart-intents-only/intents-list.png#lightbox)

## <a name="create-getjobinformation-intention"></a>GetJobInformation-intent maken
1. Selecteer **Create new intent**. Voer de naam `GetJobInformation` in voor de nieuwe intent. Deze intent wordt voorspeld telkens wanneer een gebruiker informatie wil over vacatures in uw bedrijf.

    ![](media/luis-quickstart-intents-only/create-intent.png "Schermopname van het dialoogvenster New intent")

    Door een intent te maken, maakt u een categorie van gegevens die u wilt identificeren. Door de categorie een naam te geven kunnen andere toepassingen die de queryresultaten van LUIS gebruiken, die categorienaam gebruiken om een geschikt antwoord te vinden. LUIS geeft geen antwoord op deze vragen, maar stelt alleen vast om wat voor soort informatie er wordt gevraagd in natuurlijke taal. 

2. Voeg zeven utterances toe aan deze intent waarnaar een gebruiker waarschijnlijk zal vragen, zoals:

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

3. De LUIS-app heeft momenteel geen utterances voor de intent **None**. Er zijn utterances nodig die de app niet beantwoordt. U mag deze intent niet leeglaten. Selecteer **Intents** in het linkerpaneel. 

4. Selecteer de intent **None**. Voeg drie utterances toe die een gebruiker misschien kan invoeren, maar die niet relevant zijn voor uw app. Als de app betrekking heeft op uw vacatures, zijn dit enkele goede **None**-utterances:

    | Voorbeelden van utterances|
    |--|
    |Blaffende honden zijn irritant|
    |Bestel een pizza voor me|
    |Pinguïns in de oceaan|

    Als LUIS de intent **None** retourneert voor een toepassing die LUIS aanroept, zoals een chatbot, kan de bot hierop reageren door te vragen of de gebruiker het gesprek wil beëindigen. De chatbot kan ook meer aanwijzingen geven voor het vervolgen van het gesprek als de gebruiker dit niet wil beëindigen. 

## <a name="train-and-publish-the-app"></a>De app trainen en publiceren
1. Selecteer rechtsboven op de website van LUIS de knop **Train**. 

    ![De knop Train](./media/luis-quickstart-intents-only/train-button.png)

    Het trainen is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het trainen is gelukt.

    ![Getrainde statusbalk](./media/luis-quickstart-intents-only/trained.png)

2. Selecteer rechtsboven in de website van LUIS de knop **Publish** om de pagina Publish te openen. De productiesite is standaard geselecteerd. Selecteer de knop **Publish** door de productiesite te kiezen. Het publiceren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het publiceren is gelukt.

    U hoeft niet geen LUIS-eindpuntsleutel in Azure Portal te maken voordat u publiceert of voordat u de eindpunt-URL test. Elke LUIS-app bestaat uit een gratis startersleutel voor ontwerpen. Dit biedt u onbeperkte ontwerpmogelijkheden en [enkele eindpunttreffers](luis-boundaries.md#key-limits). 

## <a name="query-endpoint-for-getjobinformation-intent"></a>Query-eindpunt voor GetJobInformation-intent
1. Selecteer onder aan de pagina **Publish** de koppeling **endpoint**. Er wordt nu een nieuw browservenster geopend, met de eindpunt-URL in de adresbalk. 

2. Ga naar het einde van de URL in het adres en voer `I'm looking for a job with Natual Language Processing` in. De laatste parameter van de queryreeks is `q`, de utterance**query**. Deze utterance is niet hetzelfde als een van de voorbeeld-utterances in stap 4. Daarom is dit een goede test die de intent `GetJobInformation` als belangrijkste scorings-intent moet geven. 

    ```
    {
      "query": "I'm looking for a job with Natual Language Processing",
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

## <a name="create-applyforjob-intention"></a>ApplyForJob-intent maken
Ga terug naar het browsertabblad voor de website van LUIS en maak een nieuwe intent om te solliciteren op een vacature.

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

    Opnieuw [trainen en publiceren](#train-and-publish-the-app). 

## <a name="query-endpoint-for-applyforjob-intent"></a>Query-eindpunt voor ApplyForJob-intent
Selecteer onder aan de pagina **Publish** de koppeling **endpoint**. Voer in het nieuwe browservenster `Can I submit my resume for job 235986` in aan het einde van de URL. 

    ```
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

## <a name="what-has-this-luis-app-accomplished"></a>Wat is er met deze LUIS-app bereikt?
Deze app heeft met slechts enkele intents een query in natuurlijke taal geïdentificeerd die hetzelfde doel heeft maar anders is geformuleerd. 

Het JSON-resultaat identificeert de hoogst scorende intent. Alle scores liggen tussen 1 en 0, met de betere score dichterbij 1. De scores van de `GetJobInformation`- en `None`-intents zijn veel dichter bij nul. 

## <a name="where-is-this-luis-data-used"></a>Waar worden deze gegevens van LUIS gebruikt? 
LUIS hoeft niets meer te doen met deze aanvraag. De aanroepende toepassing, zoals een chatbot, kan het topScoringIntent-resultaat gebruiken en informatie vinden (die niet is opgeslagen in LUIS) om de vraag te beantwoorden of de conversatie beëindigen. Dit zijn programma-opties voor de chatbot of aanroepende toepassing. LUIS hoeft die taak niet uit te voeren. LUIS bepaalt alleen wat de bedoeling van de gebruiker is. 

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u de LUIS-app niet meer nodig hebt, kunt u deze verwijderen. Daarvoor selecteert u **My apps** in het menu linksboven. Selecteer het weglatingsteken (***...***) rechts van de app-naam in de lijst met apps en selecteer vervolgens **Verwijderen**. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Vooraf gemaakte intents en entiteiten aan deze app toevoegen](luis-tutorial-prebuilt-intents-entities.md)
