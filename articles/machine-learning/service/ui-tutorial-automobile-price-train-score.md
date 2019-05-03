---
title: 'Zelfstudie: De prijs op met de visuele interface voorspellen'
titleSuffix: Azure Machine Learning service
description: Informatie over het trainen, score, en een machine learning-model met behulp van een visuele slepen en neerzetten-interface implementeren. In deze zelfstudie is deel één van een serie van twee op lineaire regressie gebruiken prijzen van auto's voorspellen.
author: peterlu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 04/06/2019
ms.openlocfilehash: 1dc905b2ac89667dcb5800b0f539cb3e9f3acdcd
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026870"
---
# <a name="tutorial-predict-automobile-price-with-the-visual-interface"></a>Zelfstudie: De prijs op met de visuele interface voorspellen

In deze zelfstudie maakt uitvoeren u een uitgebreide blik op het ontwikkelen van een voorspellende oplossing in de visuele interface voor Azure Machine Learning-service. Aan het einde van deze zelfstudie hebt u een oplossing die kunt voorspellen de prijs van een auto op basis van de technische specificaties die u deze verzendt.

In deze zelfstudie [blijft vanuit de snelstartgids](ui-quickstart-run-experiment.md) en **deel één van een tweedelige reeks**. U moet echter niet de Quick Start te voltooien voordat u begint.

In deel één van de serie zelfstudies leert u hoe u:

> [!div class="checklist"]
> * Importeren en het opschonen van gegevens (dezelfde stappen als de Quick Start)
> * Een machine learning-model trainen
> * Beoordelen en evalueren van een model

In [deel twee](ui-tutorial-automobile-price-deploy.md) van de serie zelfstudies leert u hoe u uw Voorspellend model als een Azure-web-service hebt geïmplementeerd.

> [!NOTE]
> Een voltooide versie van deze zelfstudie is beschikbaar als een voorbeeldexperiment.
> Op de pagina experimenten, gaat u naar **nieuwe toevoegen** > **voorbeeld 1: regressie: Auto's prijs Prediction(Basic)**

> [!VIDEO https://www.youtube.com/embed/VDKpDNX96aA]


## <a name="create-a-workspace"></a>Een werkruimte maken

Als u een Azure Machine Learning-service-werkruimte hebt, gaat u naar de [volgende sectie](#open-the-visual-interface-webpage). Anders wordt er nu een maken.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="open-the-visual-interface-webpage"></a>Open de webpagina van de visuele interface

1. Open de werkruimte in de [Azure-portal](https://portal.azure.com/).  

1. Selecteer in uw werkruimte **visuele interface**.  Selecteer vervolgens **starten visuele interface**.  

    ![Schermafbeelding van de Azure portal waarin wordt getoond hoe voor toegang tot de visuele interface van de werkruimte van een Machine Learning-service](./media/ui-tutorial-automobile-price-train-score/launch-ui.png)

    De interface-webpagina wordt geopend in een nieuwe browserpagina.  

## <a name="import-and-clean-your-data"></a>Importeren en uw gegevens opruimen

Het eerste wat dat u moet zijn nieuwe gegevens. Als u de Snelstartgids hebt voltooid, kunt u uw gegevens prep experiment hier opnieuw gebruiken. Als u de Snelstartgids nog niet hebt voltooid, gaat u de volgende sectie en [starten vanaf een nieuw experiment](#start-from-a-new-experiment).

### <a name="reuse-the-quickstart-experiment"></a>De Quick Start-experiment opnieuw gebruiken

1. Open uw experiment Quick Start.

1. Selecteer **OpslaanAls** aan de onderkant van het venster.

1. Geef deze een nieuwe naam in het pop-upvenster dat wordt weergegeven.

    ![Schermopname die laat zien hoe de naam wijzigen van een experiment dat moet worden 'Zelfstudie--voorspellen Automobile Price'](./media/ui-tutorial-automobile-price-train-score/save-a-copy.png)

1. Het experiment zou er nu ongeveer zo uit moeten zien:

    ![Schermafbeelding van de verwachte status van het experiment. De gegevensset ' Automobile ' maakt verbinding met de module Select Columns die verbinding met de Clean Missing Data maakt](./media/ui-tutorial-automobile-price-train-score/save-copy-result.png)

Als u uw experiment Quick Start is opnieuw gebruikt, gaat u de volgende sectie om te beginnen met [trainen van uw model](#train-the-model).

### <a name="start-from-a-new-experiment"></a>Starten van een nieuw experiment

Als u hebt de Snelstartgids niet hebt voltooid, volgt u deze stappen voor het snel maken van een nieuw experiment die worden geïmporteerd en opschonen van de gegevensset ' Automobile '.

1. Maak een nieuw experiment door te selecteren **+ nieuw** onderaan het venster van de visuele interface.

1. Selecteer **EXPERIMENT** >  **Blank Experiment**.

1. Selecteer de naam van de standaard-experiment **'geëxperimenteerd gemaakt op...** ' aan de bovenkant van het canvas en wijzig de naam in een herkenbare naam. Bijvoorbeeld, **prijzen auto's voorspellen**. De naam hoeft niet uniek te zijn.

1. Aan de linkerkant van het experimentcanvas bevindt zich een palet met gegevenssets en modules. Gebruik het zoekvak boven aan het modulepalet modules vindt. Type **auto** in het zoekvak om te vinden van de gegevensset met het label **Automobile prijs data (Raw)**. Sleep deze gegevensset naar het experimentcanvas.

    ![Schermafbeelding van de gegevensset ' Automobile ' prijs zoeken](./media/ui-tutorial-automobile-price-train-score/automobile-dataset.png)

    Nu dat u uw gegevens hebt, u kunt toevoegen aan een module die Hiermee verwijdert u de **normalized-losses** kolom volledig. Vervolgens voegt u een andere module waarmee rijen met ontbrekende gegevens worden verwijderd.

1. Type **kolommen selecteren** in het zoekvak om te zoeken de **Select Columns in Dataset** module. Sleep deze vervolgens naar het experimentcanvas. Deze module kunt u selecteren welke kolommen met gegevens die u wilt opnemen of uitsluiten in het model.

1. Koppel de uitvoerpoort van de gegevensset **Automobile price data (Raw)** aan de invoerpoort van de module Select Columns in Dataset.

    ![Waarin wordt getoond hoe de module Automobile Price Data verbinden met de module Select Columns GIF-animatie](./media/ui-tutorial-automobile-price-train-score/connect-modules.gif)

1. Selecteer de Select-Columns in Dataset module en selecteer **kolomkiezer lancering** in de **eigenschappen** deelvenster.

   1. Selecteer aan de linkerkant **met regels**

   1. Naast **Begin With**, selecteer **alle kolommen**. Deze regels direct **Select Columns in Dataset** doorgeven aan de kolommen (met uitzondering van de kolommen die we dadelijk zullen uitsluiten).

   1. Selecteer in de vervolgkeuzelijsten **uitsluiten** en **kolomnamen**, en typ vervolgens **normalized-losses** in het tekstvak.

   1. Selecteer de knop OK om te sluiten van de kolomkiezer (in de rechterbenedenhoek).

     Het deelvenster met eigenschappen van de module **Select Columns in Dataset** geeft nu aan dat alle kolommen uit de gegevensset worden doorgegeven, met uitzondering van **normalized-losses**.

1. Een opmerking toevoegen de **Select Columns in Dataset** module door te dubbelklikken op de module en in te voeren "Uitsluiten genormaliseerd verliezen.". Dit kunt u zien in een oogopslag wat de module in uw experiment doet.

    ![Schermopname die laat zien juiste configuratie van de module Select Columns](./media/ui-tutorial-automobile-price-train-score/select-columns.png)

1. Type **schoon** in het zoekvak om te zoeken de **Clean Missing Data** module. Sleep de **Clean Missing Data** module naar het experimentcanvas en koppel deze aan de **Select Columns in Dataset** module.

1. Selecteer in het deelvenster **Properties** de optie **Remove entire row** onder **Cleaning mode**. Deze opties voor directe **Clean Missing Data** schoont nu de gegevens op door rijen met ontbrekende waarden te verwijderen. Dubbelklik op de module en typ de opmerking 'Rijen met ontbrekende waarde verwijderen'.

![Schermopname die laat zien juiste configuratie van de module Clean Missing Data](./media/ui-tutorial-automobile-price-train-score/clean-missing-data.png)

## <a name="train-the-model"></a>Het model trainen

Nu dat de gegevens klaar is, kunt u een Voorspellend model kunt maken. Uw gegevens kunt u het model te trainen. Vervolgens moet u het model om te zien hoe precies deze prijzen te voorspellen kunnen testen.

**Classificatie** en **regressie** zijn twee soorten beheerde machine learning-algoritmen. **Classificatie** voorspelt een antwoord uit een gedefinieerde set categorieën, zoals een kleur (rood, blauw of groen). **Regressie** wordt gebruikt voor het voorspellen van een getal.

Omdat u prijs voorspellen, wat een getal is willen, kunt u een regressiealgoritme. In dit voorbeeld gebruikt u een lineair regressiemodel.

Het model te trainen met behulp van een set gegevens die de prijs bevat. Het model scant de gegevens en zoekt naar correlaties tussen onderdelen van een auto en de prijs. Vervolgens het model door te geven deze een set functies voor auto's, die het is bekend bent met testen en Zie hoe dicht het model wordt geleverd voor het voorspellen van de bekende prijs.

Uw gegevens gebruiken voor het model te trainen en testen van het door u te splitsen in afzonderlijke trainings- en testsets.

1. Type **gegevens splitsen** in het zoekvak om te zoeken de **Split Data** module en verbind deze met de linker-poort van de **Clean Missing Data** module.

1. Selecteer de **Split Data** module u zojuist hebt verbonden om deze te selecteren. In het deelvenster met eigenschappen, stel de van rijen in de eerste uitvoergegevensset op 0,7. Deze manier kunnen we 70 procent van de gegevens gebruiken voor het model te trainen en 30 procent voor het testen.

    ![Schermafbeelding van de juiste configuratie van het deelvenster met eigenschappen. Waarden van 'Split Data' moet worden "Split rijen", 0,7, ingedeeld splitsen, 0, False.](./media/ui-tutorial-automobile-price-train-score/split-data.png)

1. Dubbelklik op de **Split Data** en typ de opmerking 'Splits de gegevensset in set(0.7) en test set(0.3) training'

1. Als u wilt het learning-algoritme selecteren, schakelt u het zoekvak van de modulepalet.

1. Vouw de **Machine Learning** Vouw **Initialize Model**. Er worden verschillende categorieën weergegeven die kunnen worden gebruikt om de machine learning-algoritmen te initialiseren.

1. Selecteer voor dit experiment **regressie** > **lineaire regressie** en sleep deze naar het experimentcanvas.

    ![Schermafbeelding van de juiste configuratie van het deelvenster met eigenschappen. Waarden van 'Split Data' moet worden "Split rijen", 0,7, ingedeeld splitsen, 0, False.](./media/ui-tutorial-automobile-price-train-score/linear-regression-module.png)

1. Zoek en sleep de **Train Model** module naar het experimentcanvas. Koppel de uitvoer van de lineaire regressie-module aan de linkerinvoer van de module Train Model en verbinding maken met de trainingsgegevensuitvoer (linkerpoort) van de **Split Data** module aan de rechterinvoer van de **Train Model**module.

    ![Schermafbeelding van de juiste configuratie van de module Train Model. De module lineaire regressie maakt verbinding met de module links poort Train Model en de module Split Data verbindt naar rechts poort van Train Model](./media/ui-tutorial-automobile-price-train-score/train-model.png)

1. Selecteer de **Train Model** module. Selecteer de kolomkiezer lancering in het deelvenster met eigenschappen en typ vervolgens **prijs** naast **bevatten de namen van kolommen**. De prijs is de waarde die u uw model om te voorspellen

    ![Schermafbeelding van de juiste configuratie voor de kolom selector-module. Aan de regels van > bevatten de namen van kolommen > 'prijs'](./media/ui-tutorial-automobile-price-train-score/select-price.png)

    Nu het experiment er uitzien moet.
    ![Schermafbeelding van de juiste configuratie van het experiment na het toevoegen van de module Train Model.](./media/ui-tutorial-automobile-price-train-score/train-graph.png)

### <a name="run-the-training-experiment"></a>Het trainingsexperiment uitvoeren

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

## <a name="score-and-evaluate-the-model"></a>Beoordelen en het model evalueren

Nu dat u het model met behulp van 70 procent van uw gegevens hebben getraind, kunt u het beoordelen van de andere 30 procent van de gegevens te zien hoe goed uw model werkt.

1. Type **score-model** in het zoekvak om te zoeken de **Score Model** -module en sleep de module naar het experimentcanvas. Koppel de uitvoer van de **Train Model** poort van de module aan de linkerkant invoer **Score Model**. Koppel de testgegevensuitvoer (rechterpoort) van de **Split Data** poort van de module aan de rechterkant invoer **Score Model**.

1. Type **evalueren** in het zoekvak om te zoeken de **Evaluate Model** en sleept u de it module naar het experimentcanvas. Koppel de uitvoer van de **Score Model** module aan de linkerinvoer van **Evaluate Model**. Het laatste experiment ziet er ongeveer als volgt uit:

    ![Schermafbeelding van de laatste juiste configuratie van het experiment.](./media/ui-tutorial-automobile-price-train-score/final-graph.png)

1. Voer het experiment met behulp van de dezelfde compute-doel eerder gebruikt.

1. Geef de uitvoer van de **Score Model** module door het selecteren van de uitvoerpoort van **Score Model** en selecteer **Visualize**. De uitvoer bevat de voorspelde waarden voor de prijs en de bekende waarden uit de testgegevens.

    ![Schermafbeelding van de visualisatie van de uitvoer de kolom 'Beoordeeld Label' markeren](./media/ui-tutorial-automobile-price-train-score/score-result.png)

1. Als u wilt de uitvoer van de module Evaluate Model weergeven, selecteert u de uitvoerpoort en selecteer vervolgens visualiseren.

    ![Schermafbeelding van de evaluatieresultaten van het laatste experiment.](./media/ui-tutorial-automobile-price-train-score/evaluate-result.png)

De volgende statistieken worden weergegeven voor het model:

* **Mean Absolute Error (MAE)**: Het gemiddelde aan absolute fouten (een fout is het verschil tussen de voorspelde waarde en de werkelijke waarde).
* **Root Mean Squared fout (RMSE)**: De vierkantswortel uit het gemiddelde aan kwadratische fouten voor de voorspellingen op basis van de testgegevensset.
* **Relatieve absolute fout**: Het gemiddelde aan absolute fouten ten opzichte van het absolute verschil tussen de werkelijke waarden en het gemiddelde van alle werkelijke waarden.
* **Relatieve kwadratische fout**: Het gemiddelde aan kwadratische fouten ten opzichte van het kwadratische verschil tussen de werkelijke waarden en het gemiddelde van alle werkelijke waarden.
* **Determinatiecoëfficiënt**: Ook wel bekend als de r²-waarde, is dit een statistische meetwaarde die aangeeft hoe goed het model is de gegevens.

Voor elk van de foutstatistieken geldt: hoe kleiner hoe beter. Een kleine waarde geeft aan dat de voorspelling dichter bij de werkelijke waarde ligt. Voor de determinatiecoëfficiënt, hoe dichter de waarde ervan is bij één (1.0), hoe beter de voorspellingen.

## <a name="manage-experiments-in-azure-machine-learning-service-workspace"></a>Experimenten in de werkruimte van Azure Machine Learning-service beheren

De experimenten die u in de visuele interface maakt kunnen worden beheerd vanuit de werkruimte van de Azure Machine Learning-service. Gebruik de werkruimte voor meer gedetailleerde informatie, zoals personen experiment wordt uitgevoerd, diagnostische logboeken en uitvoering van grafieken.

1. Open de werkruimte in de [Azure-portal](https://portal.azure.com/).  

1. Selecteer in uw werkruimte **experimenten**. Selecteer vervolgens het experiment dat u hebt gemaakt.

    ![Schermopname die laat zien hoe u om te navigeren naar experimenten in de Azure-portal](./media/ui-tutorial-automobile-price-train-score/portal-experiments.png)

    Op deze pagina ziet u een overzicht van het experiment en de meest recente wordt uitgevoerd.

    ![Schermafbeelding die laat zien overzicht van de statistieken experiment in Azure portal](./media/ui-tutorial-automobile-price-train-score/experiment-overview.png)

1. Selecteer een uitvoering voor meer informatie over een bepaalde uitvoering.

    ![Schermafbeelding van gedetailleerde rapport](./media/ui-tutorial-automobile-price-train-score/run-details.png)

    Het rapport wordt in realtime bijgewerkt. Als u gebruikt een **Execute Python Script** -module in uw experiment, kunt u zich aanmeldt om uit te voeren de **logboeken** tabblad.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deel één van deze zelfstudie, moet u deze stappen voltooid:

* Het experiment gemaakt in de Quick Start gebruiken
* De gegevens voorbereiden
* Het model trainen
* Beoordelen en het model evalueren

In deel twee leert u hoe u uw modellen vervolgens als een Azure-web-service implementeert.

> [!div class="nextstepaction"]
> [Doorgaan met het implementeren van modellen](ui-tutorial-automobile-price-deploy.md)
