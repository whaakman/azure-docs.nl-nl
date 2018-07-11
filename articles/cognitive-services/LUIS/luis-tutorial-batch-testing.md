---
title: Testen van batch gebruiken voor het verbeteren van LUIS voorspellingen | Microsoft Docs
titleSuffix: Azure
description: Een belastingtest uit batch, bekijk de resultaten en verbeteren van LUIS voorspellingen te wijzigen.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/06/2018
ms.author: v-geberr
ms.openlocfilehash: 962f33a178048c459e8c6c2948eb17f0e78904ae
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37930987"
---
# <a name="improve-app-with-batch-test"></a>De app verbeteren met batch-test

Deze zelfstudie wordt gedemonstreerd hoe u met batch testen utterance voorspelling problemen vinden.  

In deze zelfstudie leert u het volgende:

<!-- green checkmark -->
> [!div class="checklist"]
* Maak een batchbestand testen 
* Een batch-test uitvoeren
* Bekijk de resultaten
* Corrigeer de fouten voor intents
* Testen van de batch

Voor dit artikel hebt u een gratis [LUIS](luis-reference-regions.md#luis-website)-account nodig om uw LUIS-toepassing te creëren.

## <a name="before-you-begin"></a>Voordat u begint
Als u geen de Human Resources-app vanuit de [bekijken eindpunt uitingen](luis-tutorial-review-endpoint-utterances.md) zelfstudie [importeren](luis-how-to-start-new-app.md#import-new-app) de JSON naar een nieuwe app in de [LUIS](luis-reference-regions.md#luis-website) website. De app die kan worden geïmporteerd bevindt zich in de GitHub-opslagplaats met [voorbeelden van LUIS](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-review-HumanResources.json).

Als u de oorspronkelijke Human Resources-app wilt gebruiken, kloont u de versie op de pagina [Settings](luis-how-to-manage-versions.md#clone-a-version) en wijzigt u de naam in `batchtest`. Klonen is een uitstekende manier om te experimenten met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. 

## <a name="purpose-of-batch-testing"></a>Doel van het testen van batch
Batch testen, kunt u de status van een model met een bekende set met test-uitingen valideren en entiteiten met het label. In de JSON-indeling batch-bestand, de utterances toevoegen en instellen van de entiteit-labels die u nodig hebt binnen de utterance voorspeld. 

De aanbevolen test-strategie voor LUIS maakt gebruik van drie verschillende sets van gegevens: voorbeeld uitingen opgegeven voor het model, batch test uitingen en eindpunt uitingen. Zorg ervoor dat u niet de uitingen van de voorbeeld-uitingen (toegevoegd aan een doel) of een eindpunt uitingen voor deze zelfstudie. 

Om te controleren of uw batch-test-uitingen op basis van de voorbeeld-uitingen en de eindpunt-uitingen [exporteren](luis-how-to-start-new-app.md#export-app) de app en [downloaden](luis-how-to-start-new-app.md#export-endpoint-logs) het querylogboek. De app voorbeeld utterance en logboek-uitingen query naar de batch-test-uitingen vergelijken. 

Vereisten voor het testen van batch:

* uitingen 1000 per test. 
* Geen dubbele waarden. 
* Entiteitstypen toegestaan: eenvoudig en samengesteld.

## <a name="create-a-batch-file-with-utterances"></a>Maak een batchbestand met uitingen
1. Maak `HumanResources-jobs-batch.json` in een teksteditor zoals [VSCode](https://code.visualstudio.com/). Of download [het bestand](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorial-batch-testing/HumanResources-jobs-batch.json) vanuit de LUIS-Samples-Github-opslagplaats.

2. In het JSON-indeling batchbestand utterances met toevoegen de **bedoeling** gewenste voorspelde in de test. 

    ```JSON
    [
        {
        "text": "Are there any janitorial jobs currently open?",
        "intent": "GetJobInformation",
        "entities": []
        },
        {
        "text": "I would like a fullstack typescript programming with azure job",
        "intent": "GetJobInformation",
        "entities": []
        },
        {
        "text": "Is there a database position open in Los Colinas?",
        "intent": "GetJobInformation",
        "entities": []
        },
        {
        "text": "Can I apply for any database jobs with this resume?",
        "intent": "GetJobInformation",
        "entities": []
        }
    ]
    ```

## <a name="run-the-batch"></a>Voer de batch

1. Selecteer **Test** in de bovenste navigatiebalk. 

    [ ![Schermafbeelding van LUIS-app met Test gemarkeerd in de bovenste, rechts navigatiebalk](./media/luis-tutorial-batch-testing/hr-first-image.png)](./media/luis-tutorial-batch-testing/hr-first-image.png#lightbox)

2. Selecteer **Batch testen deelvenster** in het deelvenster aan de rechterkant. 

    [ ![Schermafbeelding van LUIS-app met behulp van Batch test deelvenster gemarkeerd](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png)](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png#lightbox)

3. Selecteer **importeren gegevensset**.

    [ ![Schermafbeelding van LUIS-app met importeren gegevensset gemarkeerd](./media/luis-tutorial-batch-testing/hr-import-dataset-button.png)](./media/luis-tutorial-batch-testing/hr-import-dataset-button.png#lightbox)

4. Kies de locatie van het bestand system van de `HumanResources-jobs-batch.json` bestand.

5. Naam van de gegevensset `intents only` en selecteer **gedaan**.

    ![Bestand selecteren](./media/luis-tutorial-batch-testing/hr-import-new-dataset-ddl.png)

6. Selecteer de knop **Run**. Wacht totdat de test is voltooid.

    [ ![Schermafbeelding van LUIS-app met uitvoeren die zijn gemarkeerd](./media/luis-tutorial-batch-testing/hr-run-button.png)](./media/luis-tutorial-batch-testing/hr-run-button.png#lightbox)

7. Selecteer **resultaten**.

8. Bekijk de resultaten in de grafiek en legenda.

    [ ![Schermafbeelding van LUIS-app met de resultaten van batch](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png)](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png#lightbox)

## <a name="review-batch-results"></a>Bekijk de resultaten van batch
De batch worden vier kwadranten van de resultaten weergegeven. Aan de rechterkant van de grafiek is een filter. Het filter is standaard ingesteld op het eerste doel in de lijst. Het filter bevat alle intents en alleen eenvoudige, hiërarchische (bovenliggende alleen-), en samengestelde entiteiten. Wanneer u een sectie van de grafiek of een punt in de grafiek selecteert, wordt de bijbehorende utterance(s) onder de grafiek weergeven. 

Bij het aanwijzen van de grafiek, kan muiswiel vergroten of verkleinen van de weergave in de grafiek. Dit is handig wanneer er veel punten op de grafiek geclusterde nauw samen. 

De grafiek is in vier kwadranten, met twee van de secties in het rood weergegeven. **Dit zijn de secties zich richten op**. 

### <a name="applyforjob-test-results"></a>De resultaten van ApplyForJob
De **ApplyForJob** weergegeven in het filter de resultaten weergeven of 1 van de vier voorspellingen geslaagd is. Selecteer de naam **fout-positief** boven de bovenste juiste quadrant om te zien van de uitingen onder de grafiek. 

![LUIS batch test uitingen](./media/luis-tutorial-batch-testing/hr-applyforjobs-false-positive-results.png)

De drie uitingen had een belangrijkste doel van **ApplyForJob**. De bedoeling dat wordt vermeld in de batch-bestand heeft een lagere score. Waarom is dit gebeurd? De twee intenties zijn zeer nauw verwant woord keuze en word-indeling. Bovendien zijn er bijna drie keer zoveel voorbeelden voor **ApplyForJob** dan **GetJobInformation**. Deze eenvormigheid van uitingen voorbeeld weegt **ApplyForJob** van intentie voordeel. 

U ziet dat beide intents de dezelfde telling van fouten hebben: 

![LUIS batch test filter fouten](./media/luis-tutorial-batch-testing/hr-intent-error-count.png)

De utterance overeenkomt het belangrijkste punt in de **fout-positief** sectie `Can I apply for any database jobs with this resume?`. Het woord `resume` is alleen gebruikt **ApplyForJob**. 

De andere twee punten in de grafiek heeft veel lager scores voor de verkeerde bedoelingen, wat betekent dat ze zich dichter bij de juiste intentie. 

## <a name="fix-the-app-based-on-batch-results"></a>Herstellen van de app op basis van batch-resultaten
Het doel van deze sectie is dat de drie uitingen die onjuist zijn voorspeld voor **ApplyForJob** naar correct worden voorspeld voor **GetJobInformation**, nadat de app is verholpen. 

Een schijnbaar snelle oplossing zou zijn om toe te voegen deze uitingen van batch-bestand met de juiste intent. Dat is niet wat u wilt echter doen. Wilt u LUIS om goed te voorspellen deze uitingen zonder dat ze toe te voegen als voorbeelden. 

U vraagt zich misschien ook af over het verwijderen van uitingen van **ApplyForJob** totdat de hoeveelheid utterance hetzelfde als is **GetJobInformation**. Die de resultaten van het probleem kan mogelijk maar zou LUIS belemmeren van het voorspellen van dit doel nauwkeurig zodra. 

De eerste oplossing is het toevoegen van meer uitingen naar **GetJobInformation**. De tweede oplossing is het verminderen van het gewicht van het woord `resume` naar de **ApplyForJob** intentie. 

### <a name="add-more-utterances-to-getjobinformation"></a>Toevoegen van meer uitingen naar **GetJobInformation**
1. Sluit het deelvenster van de test batch door het selecteren van de **testen** knop in het bovenste navigatievenster. 

    [ ![Schermafbeelding van LUIS met knop testen gemarkeerd](./media/luis-tutorial-batch-testing/hr-close-test-panel.png)](./media/luis-tutorial-batch-testing/hr-close-test-panel.png#lightbox)

2. Selecteer **GetJobInformation** in de lijst intents. 

    [ ![Schermafbeelding van LUIS met knop testen gemarkeerd](./media/luis-tutorial-batch-testing/hr-select-intent-to-fix-1.png)](./media/luis-tutorial-batch-testing/hr-select-intent-to-fix-1.png#lightbox)

3. Toevoegen van meer uitingen die zijn verschillend voor de lengte, word keuze en word-indeling, zorg ervoor dat u de voorwaarden `resume` en `c.v.`:

    ```JSON
    Is there a new job in the warehouse for a stocker?
    Where are the roofing jobs today?
    I heard there was a medical coding job that requires a resume.
    I would like a job helping college kids write their c.v.s. 
    Here is my resume, looking for a new post at the community college using computers.
    What positions are available in child and home care?
    Is there an intern desk at the newspaper?
    My C.v. shows I'm good at analyzing procurement, budgets, and lost money. Is there anything for this type of work?
    Where are the earth drilling jobs right now?
    I've worked 8 years as an EMS driver. Any new jobs?
    New food handling jobs?
    How many new yard work jobs are available?
    Is there a new HR post for labor relations and negotiations?
    I have a masters in library and archive management. Any new positions?
    Are there any babysitting jobs for 13 year olds in the city today?
    ```

4. De app door het selecteren van de trein **Train** in het bovenste navigatievenster rechts.

## <a name="verify-the-fix-worked"></a>Controleren of de oplossing heeft gewerkt
Om te controleren dat de uitingen in de batch-test correct worden voorspeld, moet u de batch-test opnieuw uitvoeren.

1. Selecteer **Test** in de bovenste navigatiebalk. Als de resultaten van de batch nog steeds geopend zijn, selecteert u **terug naar lijst met**.  

2. Selecteer het weglatingsteken (***...*** ) aan de rechterkant van de batchnaam en selecteer **gegevensset uitvoeren**. Wacht totdat de batch-test is voltooid. U ziet dat de **resultaten** knop is nu groen. Dit betekent dat de hele batch is uitgevoerd.

3. Selecteer **resultaten**. De intenties moeten alle groen pictogrammen aan de linkerkant van de intentie namen hebben. 

    [ ![Schermafbeelding van LUIS met batch resultaten knop gemarkeerd](./media/luis-tutorial-batch-testing/hr-batch-test-intents-no-errors.png)](./media/luis-tutorial-batch-testing/hr-batch-test-intents-no-errors.png#lightbox)


## <a name="what-has-this-tutorial-accomplished"></a>Wat is in deze zelfstudie bereikt?
Deze app nauwkeurigheid is nu verhoogd door fouten in de batch opsporen en corrigeren van het model door meer voorbeeld uitingen toe te voegen aan de juiste intentie en training. 

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u de LUIS-app niet meer nodig hebt, kunt u deze verwijderen. Selecteer **mijn apps** in het bovenste menu links. Selecteer het weglatingsteken **...**  aan de rechterkant van de naam van de app in de lijst met Apps, selecteer **verwijderen**. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over patronen](luis-tutorial-pattern.md)

