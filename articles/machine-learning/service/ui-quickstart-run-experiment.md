---
title: 'Quickstart: Voorbereiden en visualiseren van gegevens zonder code te schrijven'
titleSuffix: Azure Machine Learning service
description: Maak een machine learning-experiment, als u wilt voorbereiden en Visualiseer uw gegevens met een slepen-en-neerzetten-gebruikersinterface.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: quickstart
author: sdgilley
ms.author: sgilley
ms.date: 05/02/2019
ms.openlocfilehash: d14ef4a59e65ba432b5d04d9f531e4ba639deb8a
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65440945"
---
# <a name="quickstart-prepare-and-visualize-data-without-writing-code-in-azure-machine-learning"></a>Quickstart: Voorbereiden en visualiseren van gegevens zonder code te schrijven in Azure Machine Learning

Voorbereiden en Visualiseer uw gegevens in de visual slepen-en-neerzetten-interface (preview) voor Azure Machine Learning. De gegevens die u gebruikt bevat vermeldingen voor verschillende auto's, inclusief informatie over het merk, model, technische specificaties en prijs.  

In deze Quick Start u verkennen en voorbereiden van gegevens:

- Uw eerste experiment toevoegen en een voorbeeld van gegevens maken
- De gegevens voorbereiden door het verwijderen van de ontbrekende waarden
- Het experiment uitvoeren
- De resulterende gegevens visualiseren

Als u nieuwe bij machine learning, de videoserie [Gegevenswetenschap voor Beginners](https://docs.microsoft.com/azure/machine-learning/studio/data-science-for-beginners-the-5-questions-data-science-answers) is een goede inleiding tot machine learning.

## <a name="prerequisites"></a>Vereisten

Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

### <a name="create-a-workspace"></a>Een werkruimte maken

Als u een Azure Machine Learning-service-werkruimte hebt, gaat u naar de [volgende sectie](#start). Anders wordt er nu een maken.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="start"></a> Open de webpagina van de visuele interface

1. Open de werkruimte in de [Azure-portal](https://portal.azure.com/).  

1. Selecteer in uw werkruimte **visuele interface**.  Selecteer vervolgens **starten visuele interface**.  
 
    ![Starten van de visuele interface](./media/ui-quickstart-run-experiment/launch-ui.png)

    De interface-webpagina wordt geopend in een nieuwe browserpagina.  

## <a name="create-your-first-experiment"></a>Uw eerste experiment opzetten

Het hulpprogramma visuele interface biedt een interactieve, visuele kunt u eenvoudig bouwen, testen en herhalen op een predictive Analytics-model. U slepen-en-neerzetten-gegevenssets en -analyse modules naar een interactief canvas en verbindt deze met elkaar om een _experimenteren_.  Maak nu uw eerste experiment.

1. Selecteer in de linkerbenedenhoek **nieuwe toevoegen**.
![Nieuw experiment toevoegen](./media/ui-quickstart-run-experiment/add-new.png)

1. Selecteer **Blank Experiment**.

1. Uw experiment krijgt een standaardnaam. Selecteer deze tekst en wijzig de naam "Quick Start-verkennen gegevens." Deze naam hoeft niet uniek te zijn.

1. De **Mini-kaart** aan de onderkant van het scherm is handig voor het weergeven van grote experimenten.  U hoeft deze niet in deze Quick Start, klikt u op de pijl aan de bovenkant te minimaliseren.  

    ![De naam van het experiment wijzigen](./media/ui-quickstart-run-experiment/rename.png)

## <a name="add-data"></a>Gegevens toevoegen

Het eerste wat dat u nodig hebt voor machine learning zijn gegevens. Er zijn enkele voorbeelden van gegevenssets opgenomen in deze interface die u kunt gebruiken of u kunt gegevens importeren uit verschillende bronnen. Voor dit voorbeeld gebruikt u de voorbeeldgegevensset van **Automobile prijs data (Raw)**. 

1. Aan de linkerkant van het experimentcanvas bevindt zich een palet met gegevenssets en modules. Selecteer **opgeslagen gegevenssets** Selecteer **voorbeelden** om de beschikbare voorbeelden van gegevenssets weer te geven.

1. Selecteer de gegevensset **Automobile prijs data (raw)**, en sleep deze naar het canvas.

   ![Gegevens naar het canvas slepen](./media/ui-quickstart-run-experiment/drag-data.png)


## <a name="select-columns"></a>Kolommen selecteren

Selecteer welke kolommen met gegevens om te werken met.  Te beginnen met configureren van de module om alle beschikbare kolommen weer te geven.

> [!TIP]
> Als u bekend bent met de naam van de gegevens of module die u wilt, gebruikt u de zoekbalk aan de bovenkant van het palet te snel vinden.  De rest van de Quick Start gebruikt deze sneltoets.

1. Type **Selecteer** in het zoekvak om te zoeken de **Select Columns in Dataset** module.

1. Klik en sleep de **Select Columns in Dataset** naar het canvas. Verwijderen van de module onder de gegevensset die u eerder hebt toegevoegd.

1. Verbinding maken met de gegevensset in de **Select Columns in Dataset**: klik op de uitvoerpoort van de gegevensset, sleept u aan de invoerpoort van **Select Columns in Dataset**, laat de muisknop los. De gegevensset en de module blijven verbonden, zelfs als u een ervan op het canvas verplaatst.

    > [!TIP]
    > De invoer- en uitvoerpoorten van gegevenssets en modules worden aangeduid met kleine cirkels, waarbij de invoerpoorten zich boven en de uitvoerpoorten zich onder bevinden. U kunt een stroom van gegevens in uw experiment maken wanneer u de uitvoerpoort van één module verbinding met een invoerpoort van een andere maakt.
    >
    > Als u problemen ondervindt bij het verbinden van modules, probeert u helemaal naar het knooppunt dat u verbinding maakt te slepen.

    ![Verbinding maken met modules](./media/ui-quickstart-run-experiment/connect-modules.gif) 

    Het rode uitroepteken geeft aan dat u de eigenschappen voor de module nog niet hebt ingesteld. U gaat dat nu doen.
   
1. Selecteer de **Select Columns in Dataset** module.

1. In de **eigenschappen** deelvenster aan de rechterkant van het canvas, selecteert **kolomkiezer lancering**.

    In de **kolommen selecteren** dialoogvenster **alle kolommen** en **alle functies**. Het dialoogvenster zou er zo uit moeten zien:

     ![column-selector](./media/ui-quickstart-run-experiment/select-all.png)

1. Selecteer het vinkje (OK) om de kolomkiezer te sluiten in de rechterbenedenhoek.

## <a name="run-the-experiment"></a>Het experiment uitvoeren

Klik op de uitvoerpoort van een gegevensset of module klikken om te zien wat de gegevens eruitzien op dat moment in de gegevensstroom op elk gewenst moment.  Als de **Visualize** optie is uitgeschakeld, moet u eerst het experiment uit te voeren.  U gaat dat nu doen.

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

Nadat de compute-doel beschikbaar is, wordt het experiment wordt uitgevoerd. Wanneer de uitvoering voltooid is, wordt een groen vinkje weergegeven op elke module.

![Status weergeven](./media/ui-quickstart-run-experiment/status1.png)

## <a name="preview-the-data"></a>Voorbeeld van de gegevens

Nu dat u uw eerste experiment uitvoert hebt, kunt u de gegevens voor meer informatie over de informatie die u hebt voor het werken met visualiseren.

1. Selecteer de uitvoerpoort onder aan de **Select Columns in Dataset** Selecteer **Visualize**.

1. Klik op verschillende kolommen in het venster om informatie over die kolom weer te geven.  

    In deze gegevensset wordt elke auto weergegeven als een rij. De variabelen die aan elke auto zijn gekoppeld, worden weergegeven als kolommen.    Er zijn 205 rijen en 26 kolommen in deze gegevensset.

     Telkens wanneer u een kolom met gegevens, klikt u op de **statistieken** informatie en **visualisatie** afbeelding van die kolom wordt weergegeven aan de linkerkant.  Bijvoorbeeld, als u klikt op **num van deuren** ziet u dat er 2 unieke waarden en 2 ontbrekende waarden.  Schuif omlaag om de waarden te bekijken: twee en vier deuren.

     ![Voorbeeld van de gegevens](./media/ui-quickstart-run-experiment/preview-data.gif)

1. Klik op elke kolom meer informatie geven over uw gegevensset.

## <a name="prepare-data"></a>Gegevens voorbereiden

Normaal gesproken moet een gegevensset worden voorverwerkt voordat deze kan worden geanalyseerd. Het is u mogelijk opgevallen dat er in verschillende rijen ontbreken in de kolommen van verschillende rijen. Deze ontbrekende waarden moeten worden opgeschoond, zodat de gegevens correct kunnen worden geanalyseerd. Verwijdert u alle rijen met ontbrekende waarden. Ook de **normalized-losses** kolom heeft een groot deel van de ontbrekende waarden, zodat u die kolom van het model kan worden overgeslagen uitsluiten zult.

> [!TIP]
> Voor de meeste modules geldt dat voor het gebruik van de module de ontbrekende invoergegevens moeten worden opgeschoond.  

### <a name="remove-column"></a>Kolom verwijderen

Verwijder eerst de **normalized-losses** kolom volledig.

1. Selecteer de **Select Columns in Dataset** module.

1. In de **eigenschappen** deelvenster aan de rechterkant van het canvas, selecteert **kolomkiezer lancering**.

    * Laat **met regels** en **alle kolommen** geselecteerde.

    * Selecteer in de vervolgkeuzelijsten **Exclude** en **column names** en klik in het tekstvak. Type **normalized-losses**.

    * Selecteer het vinkje (OK) om de kolomkiezer te sluiten in de rechterbenedenhoek.

    ![Sluit een kolom](./media/ui-quickstart-run-experiment/exclude-column.png)
        
    Nu het deelvenster met eigenschappen voor Select Columns in Dataset geeft aan dat wordt doorgegeven alle kolommen uit de gegevensset, behalve **normalized-losses**.
        
    Het deelvenster met eigenschappen ziet u dat de **normalized-losses** kolom wordt uitgesloten.
        
    ![Eigenschappenvenster](./media/ui-quickstart-run-experiment/property-pane.png)
        
    U kunt een opmerking aan een module toevoegen door te dubbelklikken op de module en tekst in te voeren. Zodoende kunt u in één oogopslag zien wat de module in uw experiment doet. 

1. Dubbelklik op de **Select Columns in Dataset** -module en typ de opmerking ' normalized-losses uitsluiten." 
    
    Nadat u de opmerking hebt getypt, klikt u op buiten de module.  Er verschijnt een pijl-omlaag om weer te geven dat de module een opmerking bevat.

1. Klik op de pijl-omlaag om weer te geven van de opmerking.

    De module bevat nu een pijl aan de opmerking verbergen.
        
    ![Opmerkingen](./media/ui-quickstart-run-experiment/comments.png)

### <a name="clean-missing-data"></a>Schone, ontbrekende gegevens

Voeg nu een andere module die de overige rijen met ontbrekende gegevens verwijderd.

1. Type **schoon** in het zoekvak om te zoeken de **Clean Missing Data** module.

1. Sleep de **Clean Missing Data** module naar het experimentcanvas en koppel deze aan de **Select Columns in Dataset** module. 

1. Selecteer in het deelvenster Eigenschappen **hele rij verwijderen** onder **Cleaning mode**.

    Deze opties voor directe **Clean Missing Data** schoont nu de gegevens op door rijen met ontbrekende waarden te verwijderen.

1. Dubbelklik op de module en typ de opmerking 'Rijen met ontbrekende waarde verwijderen'.
 
    ![Rijen verwijderen](./media/ui-quickstart-run-experiment/remove-rows.png)

    Uw experiment ziet er ongeveer als volgt:
    
    ![kolom selecteren](./media/ui-quickstart-run-experiment/experiment-clean.png)

## <a name="visualize-the-results"></a>De resultaten te visualiseren

Nadat u wijzigingen hebt aangebracht aan de modules in uw experiment, wordt de status is gewijzigd in 'In draft'.  De nieuwe schone om gegevens te visualiseren, moet u eerst het experiment vervolgens opnieuw uit.

1. Selecteer **uitvoeren** onder aan het experiment uit te voeren.

    Deze tijd kunt u de compute-doel dat u eerder hebt gemaakt, hergebruiken.  

1. Selecteer **uitvoeren** in het dialoogvenster.

   ![Experiment uit te voeren](./media/ui-quickstart-run-experiment/select-compute.png)

1. Wanneer de uitvoering is voltooid, met de rechtermuisknop op de **Clean Missing Data** module om de nieuwe, schone gegevens te visualiseren.  

    ![Schoon gegevens visualiseren](./media/ui-quickstart-run-experiment/visualize-cleaned.png)

1. Klik op verschillende kolommen in het venster opgeschoonde gegevens om te zien hoe de gegevens zijn gewijzigd.  

    ![Schoon gegevens visualiseren](media/ui-quickstart-run-experiment/visualize-result.png)

    Er zijn nu 193 rijen en kolommen 25.

    Als u klikt op **num van deuren** ziet u dit nog steeds heeft 2 unieke waarden, maar nu 0 ontbrekende waarden.  

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u de volgende zaken geleerd:

- Uw eerste experiment toevoegen en een voorbeeld van gegevens maken
- De gegevens voorbereiden door het verwijderen van de ontbrekende waarden
- De resulterende gegevens visualiseren

Doorgaan naar de zelfstudie voor het gebruik van deze gegevens om te voorspellen de prijs van een auto.

> [!div class="nextstepaction"]
> [Zelfstudie: De prijs op met de visuele interface voorspellen](ui-tutorial-automobile-price-train-score.md)
