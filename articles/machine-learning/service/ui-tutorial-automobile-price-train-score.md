---
title: 'Zelfstudie: De prijs op met de visuele interface voorspellen'
titleSuffix: Azure Machine Learning service
description: Informatie over het trainen, score, en een machine learning-model met behulp van een visuele slepen en neerzetten-interface implementeren. In deze zelfstudie is deel één van een serie van twee op lineaire regressie gebruiken prijzen van auto's voorspellen.
author: peterclu
ms.author: peterclu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 04/06/2019
ms.openlocfilehash: 21f5a2d93b708e93f124bd44177bb7852dfbd86a
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67720510"
---
# <a name="tutorial-predict-automobile-price-with-the-visual-interface"></a>Zelfstudie: De prijs op met de visuele interface voorspellen

In deze zelfstudie maakt uitvoeren u een uitgebreide blik op het ontwikkelen van een voorspellende oplossing in de visuele interface voor Azure Machine Learning-service. Aan het einde van deze zelfstudie hebt u een oplossing die kunt voorspellen de prijs van een auto op basis van de technische specificaties die u deze verzendt.

In deel één van de zelfstudie leert u hoe u:

> [!div class="checklist"]
> * Schoon gegevens importeren en
> * Een machine learning-model trainen
> * Beoordelen en evalueren van een model

In [deel twee](ui-tutorial-automobile-price-deploy.md) van de zelfstudie leert u hoe u uw Voorspellend model als een Azure-web-service hebt geïmplementeerd.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GY]

Een voltooide versie van deze zelfstudie is beschikbaar als een voorbeeldexperiment.

Het vinden van de **experimenten pagina**, selecteer **nieuwe toevoegen**en selecteer vervolgens de **voorbeeld 1: regressie: Auto's prijs Prediction(Basic)** experimenteren.

## <a name="create-a-workspace"></a>Een werkruimte maken

Als u een Azure Machine Learning-service-werkruimte hebt, gaat u naar de [volgende sectie](#open-the-visual-interface-webpage).

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="open-the-visual-interface-webpage"></a>Open de webpagina van de visuele interface

1. Open de werkruimte in de [Azure-portal](https://portal.azure.com/).

1. Selecteer in uw werkruimte **visuele interface**. Selecteer vervolgens **starten visuele interface**. 

    ![Schermafbeelding van de Azure portal waarin wordt getoond hoe voor toegang tot de visuele interface van de werkruimte van een Machine Learning-service](./media/ui-tutorial-automobile-price-train-score/launch-ui.png)

## <a name="create-your-first-experiment"></a>Uw eerste experiment opzetten

Het hulpprogramma visuele interface biedt een interactieve, visuele plaats voor het bouwen van voorspellende analysemodellen. Gegevenssets en -analyse modules slepen en neerzetten naar een interactief canvas en verbinding maken met elkaar te maken een *experimenteren*.

1. Maak een nieuw experiment door te selecteren **+ nieuw** onderaan het venster van de visuele interface.

    ![Nieuw experiment toevoegen](./media/ui-tutorial-automobile-price-train-score/add-new.png)

1. Selecteer **Blank Experiment**.

1. Selecteer de naam van de standaard-experiment **'geëxperimenteerd gemaakt op...** ' aan de bovenkant van het canvas en wijzig de naam in een herkenbare naam. Bijvoorbeeld, **prijzen auto's voorspellen**. De naam hoeft niet uniek te zijn.

## <a name="add-data"></a>Gegevens toevoegen

Het eerste wat dat u nodig hebt voor machine learning zijn gegevens. Er zijn enkele voorbeelden van gegevenssets opgenomen in deze interface die u kunt gebruiken. U kunt ook gegevens importeren uit bestaande bronnen. Voor deze zelfstudie gebruikt u de voorbeeldgegevensset van **Automobile prijs data (Raw)** . 

1. Aan de linkerkant van het experimentcanvas bevindt zich een palet met gegevenssets en modules. Selecteer **opgeslagen gegevenssets** Selecteer **voorbeelden** om de beschikbare voorbeelden van gegevenssets weer te geven.

1. Selecteer de gegevensset **Automobile prijs data (raw)** , en sleep deze naar het canvas.

   ![Gegevens naar het canvas slepen](./media/ui-tutorial-automobile-price-train-score/drag-data.png)

## <a name="select-columns"></a>Kolommen selecteren

Selecteer welke kolommen met gegevens om te werken met. Te beginnen met configureren van de module om alle beschikbare kolommen weer te geven.

> [!TIP]
> Als u bekend bent met de naam van de gegevens of module die u wilt, gebruikt u de zoekbalk aan de bovenkant van het palet te snel vinden. De rest van de zelfstudie gebruikt u deze snelkoppeling.


1. Type **Selecteer** in het zoekvak om te zoeken de **Select Columns in Dataset** module.

1. Klik en sleep de **Select Columns in Dataset** naar het canvas. Verwijderen van de module onder de gegevensset die u eerder hebt toegevoegd.

1. Verbinding maken met de gegevensset in de **Select Columns in Dataset**: klik op de uitvoerpoort van de gegevensset, sleept u aan de invoerpoort van **Select Columns in Dataset**, laat de muisknop los. De gegevensset en de module blijven verbonden, zelfs als u een ervan op het canvas verplaatst.

    > [!TIP]
    > De invoer- en uitvoerpoorten van gegevenssets en modules worden aangeduid met kleine cirkels, waarbij de invoerpoorten zich boven en de uitvoerpoorten zich onder bevinden. U kunt een stroom van gegevens in uw experiment maken wanneer u de uitvoerpoort van één module verbinding met een invoerpoort van een andere maakt.
    >

    ![Verbinding maken met modules](./media/ui-tutorial-automobile-price-train-score/connect-modules.gif)

    Het rode uitroepteken geeft aan dat u de eigenschappen voor de module nog niet hebt ingesteld.

1. Selecteer de **Select Columns in Dataset** module.

1. In de **eigenschappen** deelvenster aan de rechterkant van het canvas, selecteert **kolommen bewerken**.

    In de **kolommen selecteren** dialoogvenster **alle kolommen** en **alle functies**. Het dialoogvenster zou er zo uit moeten zien:

     ![column-selector](./media/ui-tutorial-automobile-price-train-score/select-all.png)

1. Selecteer in de rechterbenedenhoek **OK** de kolomkiezer te sluiten.

## <a name="run-the-experiment"></a>Het experiment uitvoeren

Klik op de uitvoerpoort van een gegevensset of module klikken om te zien wat de gegevens eruitzien op dat moment in de gegevensstroom op elk gewenst moment. Als de **Visualize** optie is uitgeschakeld, moet u eerst het experiment uit te voeren.

Een experiment wordt uitgevoerd op een compute-doel is van een compute-resource die is gekoppeld aan uw werkruimte. Zodra u een compute-doel hebt gemaakt, kunt u het opnieuw gebruiken voor toekomstige planningen.

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

Nadat de compute-doel beschikbaar is, wordt het experiment wordt uitgevoerd. Wanneer de uitvoering voltooid is, wordt een groen vinkje weergegeven op elke module.


## <a name="preview-the-data"></a>Voorbeeld van de gegevens

Nu dat u uw eerste experiment uitvoert hebt, kunt u de gegevens voor meer informatie over de gegevensset die u hebt om te werken met visualiseren.

1. Selecteer de uitvoerpoort onder aan de **Select Columns in Dataset** Selecteer **Visualize**.

1. Klik op verschillende kolommen in het venster om informatie over die kolom weer te geven.

    In deze gegevensset wordt elke auto weergegeven als een rij. De variabelen die aan elke auto zijn gekoppeld, worden weergegeven als kolommen. Er zijn 205 rijen en 26 kolommen in deze gegevensset.

     Telkens wanneer u een kolom met gegevens, klikt u op de **statistieken** informatie en **visualisatie** afbeelding van die kolom wordt weergegeven aan de linkerkant. Bijvoorbeeld, als u klikt op **num van deuren** ziet u dat er 2 unieke waarden en 2 ontbrekende waarden. Schuif omlaag om de waarden te bekijken: twee en vier deuren.

     ![Voorbeeld van de gegevens](./media/ui-tutorial-automobile-price-train-score/preview-data.gif)

1. Klik op elke kolom voor meer informatie over uw gegevensset en denken over of deze kolommen is nuttig om te voorspellen de prijs van een auto.

## <a name="prepare-data"></a>Gegevens voorbereiden

Normaal gesproken moet een gegevensset worden voorverwerkt voordat deze kan worden geanalyseerd. Je misschien opgevallen sommige ontbrekende waarden bij het visualiseren van de gegevensset. Deze ontbrekende waarden moeten worden opgeschoond, zodat de gegevens correct kunnen worden geanalyseerd. Verwijdert u alle rijen met ontbrekende waarden. Ook de **normalized-losses** kolom heeft een groot deel van de ontbrekende waarden, zodat u die kolom van het model kan worden overgeslagen uitsluiten zult.

> [!TIP]
> Voor de meeste modules geldt dat voor het gebruik van de module de ontbrekende invoergegevens moeten worden opgeschoond.

### <a name="remove-column"></a>Kolom verwijderen

Verwijder eerst de **normalized-losses** kolom volledig.

1. Selecteer de **Select Columns in Dataset** module.

1. In de **eigenschappen** deelvenster aan de rechterkant van het canvas, selecteert **kolommen bewerken**.

    * Laat **met regels** en **alle kolommen** geselecteerde.

    * Selecteer in de vervolgkeuzelijsten **Exclude** en **column names** en klik in het tekstvak. Type **normalized-losses**.

    * Selecteer in de rechterbenedenhoek **OK** de kolomkiezer te sluiten.

    ![Sluit een kolom](./media/ui-tutorial-automobile-price-train-score/exclude-column.png)
        
    Nu het deelvenster met eigenschappen voor Select Columns in Dataset geeft aan dat wordt doorgegeven alle kolommen uit de gegevensset, behalve **normalized-losses**.
        
    Het deelvenster met eigenschappen ziet u dat de **normalized-losses** kolom wordt uitgesloten.
        
    ![Eigenschappenvenster](./media/ui-tutorial-automobile-price-train-score/property-pane.png)
        
    U kunt een opmerking aan een module toevoegen door te dubbelklikken op de module en tekst in te voeren. Zodoende kunt u in één oogopslag zien wat de module in uw experiment doet. 

1. Dubbelklik op de **Select Columns in Dataset** -module en typ de opmerking ' normalized-losses uitsluiten." 
    
    Nadat u de opmerking hebt getypt, klikt u op buiten de module. Er verschijnt een pijl-omlaag om weer te geven dat de module een opmerking bevat.

1. Klik op de pijl-omlaag om weer te geven van de opmerking.

    De module bevat nu een pijl aan de opmerking verbergen.
        
    ![Opmerkingen](./media/ui-tutorial-automobile-price-train-score/comments.png)

### <a name="clean-missing-data"></a>Schone, ontbrekende gegevens

Wanneer u een model te trainen, hebt u iets over de ontbrekende gegevens doen. In dit geval voegt u een module om te verwijderen van de overige rijen met ontbrekende gegevens toe.

1. Type **schoon** in het zoekvak om te zoeken de **Clean Missing Data** module.

1. Sleep de **Clean Missing Data** module naar het experimentcanvas en koppel deze aan de **Select Columns in Dataset** module. 

1. Selecteer in het deelvenster Eigenschappen **hele rij verwijderen** onder **Cleaning mode**.

    Deze opties voor directe **Clean Missing Data** schoont nu de gegevens op door rijen met ontbrekende waarden te verwijderen.

1. Dubbelklik op de module en typ de opmerking 'Rijen met ontbrekende waarde verwijderen'.
 
    ![Rijen verwijderen](./media/ui-tutorial-automobile-price-train-score/remove-rows.png)

    Uw experiment ziet er ongeveer als volgt:
    
    ![kolom selecteren](./media/ui-tutorial-automobile-price-train-score/experiment-clean.png)

## <a name="visualize-the-results"></a>De resultaten te visualiseren

Nadat u wijzigingen hebt aangebracht aan de modules in uw experiment, wordt de status is gewijzigd in 'In draft'.  De nieuwe schone om gegevens te visualiseren, moet u eerst het experiment vervolgens opnieuw uit.

1. Selecteer **uitvoeren** onder aan het experiment uit te voeren.

    Deze tijd kunt u de compute-doel dat u eerder hebt gemaakt, hergebruiken.

1. Selecteer **uitvoeren** in het dialoogvenster.

   ![Experiment uit te voeren](./media/ui-tutorial-automobile-price-train-score/select-compute.png)

1. Wanneer de uitvoering is voltooid, met de rechtermuisknop op de **Clean Missing Data** module om de nieuwe, schone gegevens te visualiseren.

    ![Schoon gegevens visualiseren](./media/ui-tutorial-automobile-price-train-score/visualize-cleaned.png)

1. Klik op verschillende kolommen in het venster opgeschoonde gegevens om te zien hoe de gegevens zijn gewijzigd.

    ![Schoon gegevens visualiseren](media/ui-tutorial-automobile-price-train-score/visualize-result.png)

    Er zijn nu 193 rijen en kolommen 25.

    Als u klikt op **num van deuren** ziet u dit nog steeds heeft 2 unieke waarden, maar nu 0 ontbrekende waarden. Klik in de rest van de kolommen om te zien dat er geen ontbrekende waarden links in de gegevensset. 

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

    Uw experiment ziet er als volgt:

    ![Schermafbeelding van de juiste configuratie van het experiment na het toevoegen van de module Train Model.](./media/ui-tutorial-automobile-price-train-score/train-graph.png)

### <a name="run-the-training-experiment"></a>Het trainingsexperiment uitvoeren

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

## <a name="score-and-evaluate-the-model"></a>Beoordelen en het model evalueren

Nu dat u het model met behulp van 70 procent van uw gegevens hebben getraind, kunt u het beoordelen van de andere 30 procent van de gegevens te zien hoe goed uw model werkt.

1. Type **score-model** in het zoekvak om te zoeken de **Score Model** -module en sleep de module naar het experimentcanvas. Koppel de uitvoer van de **Train Model** poort van de module aan de linkerkant invoer **Score Model**. Koppel de testgegevensuitvoer (rechterpoort) van de **Split Data** poort van de module aan de rechterkant invoer **Score Model**.

1. Type **evalueren** in het zoekvak om te zoeken de **Evaluate Model** en sleep de module naar het experimentcanvas. Koppel de uitvoer van de **Score Model** module aan de linkerinvoer van **Evaluate Model**. Het laatste experiment ziet er ongeveer als volgt uit:

    ![Schermafbeelding van de laatste juiste configuratie van het experiment.](./media/ui-tutorial-automobile-price-train-score/final-graph.png)

1. Voer het experiment met behulp van de dezelfde compute-doel eerder gebruikt.

1. Geef de uitvoer van de **Score Model** module door het selecteren van de uitvoerpoort van **Score Model** en selecteer **Visualize**. De uitvoer bevat de voorspelde waarden voor de prijs en de bekende waarden uit de testgegevens.

    ![Schermafbeelding van de visualisatie van de uitvoer de kolom 'Beoordeeld Label' markeren](./media/ui-tutorial-automobile-price-train-score/score-result.png)

1. Als u wilt de uitvoer van de module Evaluate Model weergeven, selecteert u de uitvoerpoort en selecteer vervolgens visualiseren.

    ![Schermafbeelding van de evaluatieresultaten van het laatste experiment.](./media/ui-tutorial-automobile-price-train-score/evaluate-result.png)

De volgende statistieken worden weergegeven voor het model:

* **Mean Absolute Error (MAE)** : Het gemiddelde aan absolute fouten (een fout is het verschil tussen de voorspelde waarde en de werkelijke waarde).
* **Root Mean Squared fout (RMSE)** : De vierkantswortel uit het gemiddelde aan kwadratische fouten voor de voorspellingen op basis van de testgegevensset.
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

* Een experiment hebt gemaakt
* De gegevens voorbereiden
* Het model trainen
* Beoordelen en het model evalueren

In deel twee leert u hoe u uw modellen vervolgens als een Azure-web-service implementeert.

> [!div class="nextstepaction"]
> [Doorgaan met het implementeren van modellen](ui-tutorial-automobile-price-deploy.md)
