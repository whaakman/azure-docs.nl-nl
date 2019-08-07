---
title: Uw eerste geautomatiseerde machine learning-experiment maken
titleSuffix: Azure Machine Learning service
description: Meer informatie over het trainen en implementeren van een classificatie model met geautomatiseerde machine learning in de Azure Portal.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: tzvikei
author: tsikiksr
ms.reviewer: nibaccam
ms.date: 07/23/2019
ms.openlocfilehash: 1d2a0c9e1531ace16335ea57acaccedfe2c1a3c8
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68815891"
---
# <a name="tutorial-use-automated-machine-learning-to-train-and-deploy-your-first-classification-model-preview"></a>Zelfstudie: Automatische machine learning gebruiken om uw eerste classificatie model te trainen en te implementeren (preview)

In deze zelf studie leert u hoe u uw eerste geautomatiseerde machine learning experiment maakt in de Azure Portal. In dit voor beeld wordt een classificatie model gemaakt om te voors pellen of een client zich abonneert op een termijn storting met de Bank.

Door gebruik te maken van de geautomatiseerde machine learning mogelijkheden van de service en de Azure Portal, begint u met het geautomatiseerde machine learning proces. De algoritme selectie en afstemming afstemming voor u worden uitgevoerd. De geautomatiseerde machine learning techniek doorloopt veel combi Naties van algoritmen en Hyper parameters totdat het beste model is gevonden op basis van uw criterium, zonder dat u slechts één regel code hoeft te schrijven.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een werkruimte configureren in Azure Machine Learning Service
> * Een experiment maken.
> * Een classificatie model automatisch trainen.
> * Details van de trainings uitvoering weer geven.
> * Het model implementeren.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://aka.ms/AMLFree).

* Het **bankmarketing_train. CSV** -gegevens bestand. [Down load het](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv).

## <a name="create-a-workspace"></a>Een werkruimte maken

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="create-an-experiment"></a>Een experiment maken

1. Ga naar het linkerdeel venster van uw werk ruimte. Selecteer **automatische machine learning** in het gedeelte **ontwerpen (preview)** .

    ![Navigatie deel venster Azure Portal](media/tutorial-1st-experiment-automated-ml/nav-pane.png)

    Omdat dit uw eerste experiment met automatische Machine Learning is, ziet u het scherm **Welkom bij automatisch machine learning** . 

1. Selecteer **experiment maken**. Voer vervolgens **mijn-1ste-automl-experiment** in als experiment naam.

1. Selecteer **een nieuwe berekening maken** en configureer uw berekenings context voor dit experiment.

    Veld| Value
    ---|---
    Compute-naam| Voer een unieke naam in die uw berekenings context identificeert. Voor dit voor beeld gebruiken we **automl-Compute**.
    Grootte van de virtuele machine| Selecteer de grootte van de virtuele machine voor de reken kracht. We gebruiken **Standard_DS12_V2**.
    Aanvullende instellingen| *Min. knoop punt*: 1. Als u gegevens profilering wilt inschakelen, moet u een of meer knoop punten hebben. <br> *Maximum knoop punt*: 6. 

    Selecteer **maken**om uw nieuwe Compute te maken. Dit duurt enkele minuten. 

    Wanneer het maken is voltooid, selecteert u de nieuwe reken kracht in de vervolg keuzelijst en selecteert u vervolgens **volgende**.

1. Voor deze zelf studie gebruiken we het standaard opslag account en de container die zijn gemaakt met uw nieuwe reken proces. Deze worden automatisch ingevuld in het formulier.

1. Selecteer **uploaden** en kies het bestand **bankmarketing_train. CSV** van uw lokale computer om het te uploaden naar de standaard container. Open bare preview ondersteunt alleen lokale bestands uploads en Azure Blob Storage-accounts. Wanneer het uploaden is voltooid, selecteert u het bestand in de lijst. 

    [![Gegevens bestand selecteren](media/tutorial-1st-experiment-automated-ml/select-data-file.png)](media/tutorial-1st-experiment-automated-ml/select-data-file-expanded.png#lightbox)

1. Op het tabblad **Preview** kunnen we onze gegevens voor dit experiment verder configureren.

    Op het tabblad **voor beeld** geeft u aan dat de gegevens kopteksten bevatten. De standaard waarden voor de service zijn alle functies (columns) voor training. Voor dit voor beeld schuift u naar rechts en negeert u de functie **day_of_week** .

    ![Configuratie van het tabblad voor beeld](media/tutorial-1st-experiment-automated-ml/preview-tab-config.gif)


    >[!NOTE]
    > Gegevens profilering is niet beschikbaar voor berekeningen met minimale knoop punten van nul.

1. Selecteer **classificatie** als de Voorspellings taak.

1. Selecteer **y** als doel kolom, waar we voor spellingen willen doen. Deze kolom geeft aan of de client is geabonneerd op een term deposit of niet.

1. Vouw **Geavanceerde instellingen** uit en vul de velden als volgt in.

    Geavanceerde instellingen|Value
    ------|------
    Primaire metriek| AUC_weighted 
    Afsluit criteria| Wanneer aan een van deze criteria wordt voldaan, wordt de trainings taak beëindigd voordat deze volledig is voltooid: <br> *Tijd trainings taak (minuten)* : 5  <br> *Maximum aantal herhalingen*: 10 
    Verwerking| Hiermee wordt de voor verwerking van automatische machine learning mogelijk gemaakt. Dit omvat het automatisch opschonen, voorbereiden en transformeren van gegevens voor het genereren van synthetische functies.
    Validatie| Selecteer Kruis validatie met K-vouwen en **2** voor het aantal kruisingen. 
    Gelijktijdigheid| Selecteer **5** voor het maximum aantal gelijktijdige herhalingen.

   >[!NOTE]
   > Voor dit experiment instellen we geen drempel waarde voor metrische gegevens of maximale iteraties. Het is ook niet blok keren dat algoritmen worden getest.

1. Selecteer **starten** om het experiment uit te voeren.

   Wanneer het experiment wordt gestart, ziet u een leeg **detail** scherm met de volgende status boven. 

      ![Voor bereiding uitvoeren](media/tutorial-1st-experiment-automated-ml/run-preparing.png)
      
Het experiment voorbereidende proces duurt enkele minuten. Wanneer het proces is voltooid, wordt de status bericht wijzigingen worden **uitgevoerd**.

##  <a name="view-experiment-details"></a>Details van experiment weer geven

Naarmate het experiment vordert, werkt het **detail scherm uitvoeren** het iteratie diagram en de lijst bij met de verschillende iteraties (modellen) die worden uitgevoerd. De lijst met herhalingen wordt gesorteerd op metrische Score. Standaard beoordeelt het model dat het hoogste verlaagt op basis van onze **AUC_weighted** -metriek, boven aan de lijst.

>[!TIP]
> Het duurt enkele minuten voordat de trainings taken zijn voltooid.

[![Dash board Details uitvoeren](media/tutorial-1st-experiment-automated-ml/run-details.png)](media/tutorial-1st-experiment-automated-ml/run-details-expanded.png#lightbox)

## <a name="deploy-the-model"></a>Het model implementeren

Voor dit experiment wordt **VotingEnsemble** beschouwd als het beste model, op basis van de **AUC_weighted** -metriek. Door automatische machine learning in het Azure Portal te gebruiken, kunnen we dit model als een webservice implementeren om te voors pellen op nieuwe gegevens. 

1. Selecteer op de pagina **detail gegevens uitvoeren** de knop **beste model implementeren** .

1. Vul het deel venster **beste model implementeren** als volgt in:

    Veld| Value
    ----|----
    Implementatienaam| mijn-automl-implementeren
    Beschrijving van implementatie| Mijn eerste geautomatiseerde machine learning-experiment implementatie
    Score script| Automatisch genereren
    Omgevings script| Automatisch genereren
    
1. Selecteer **Implementeren**. Het volt ooien van de implementatie duurt ongeveer 20 minuten.

    Het volgende bericht wordt weer gegeven wanneer de implementatie is voltooid:

    ![Implementatie voltooid](media/tutorial-1st-experiment-automated-ml/deploy-complete-status.png)
    
    Dat is alles. U hebt een Operational web service voor het genereren van voor spellingen.

## <a name="clean-up-resources"></a>Resources opschonen

Implementatie bestanden zijn groter dan gegevens en experimenteer bestanden, zodat ze meer kosten in beslaan. Verwijder alleen de implementatie bestanden om de kosten voor uw account te minimaliseren, of als u uw werk ruimte en de bestanden wilt laten experimenteren. U kunt ook de hele resource groep verwijderen als u niet van plan bent om een van de bestanden te gebruiken.  

### <a name="delete-the-deployment-instance"></a>Het implementatie-exemplaar verwijderen

Verwijder alleen het implementatie-exemplaar van de Azure Portal als u de resource groep en-werk ruimte wilt blijven gebruiken voor andere zelf studies en verkennen. 

1. Ga naar het deel venster **assets** aan de linkerkant enSelecteer implementaties. 

1. Selecteer de implementatie die u wilt verwijderen en selecteer **verwijderen**. 

1. Selecteer **door gaan**.

### <a name="delete-the-resource-group"></a>De resource groep verwijderen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelf studie voor automatisch machine learning hebt u de Azure Portal gebruikt voor het maken en implementeren van een classificatie model. Zie deze artikelen voor meer informatie en volgende stappen:

+ Meer informatie over het [gebruik van een webservice](how-to-consume-web-service.md).
+ Meer informatie over voor [verwerking](how-to-create-portal-experiments.md#preprocess).
+ Meer informatie over [gegevens profilering](how-to-create-portal-experiments.md#profile).
+ Meer informatie over [automatische machine learning](concept-automated-ml.md).

>[!NOTE]
> Deze gegevensset voor Bank marketing wordt beschikbaar gesteld onder [de Creative Commons (CCO: Open bare domein)](https://creativecommons.org/publicdomain/zero/1.0/). Alle rechten in de afzonderlijke inhoud van de Data Base worden in licentie gegeven onder de licentie voor de [Data Base-inhoud](https://creativecommons.org/publicdomain/zero/1.0/) en beschikbaar op [Kaggle](https://www.kaggle.com/janiobachmann/bank-marketing-dataset). Deze gegevensset is oorspronkelijk beschikbaar in de [icb machine learning-data base](https://archive.ics.uci.edu/ml/datasets/bank+marketing).<br><br>
> Geef het volgende werk op: <br> [Moro et al., 2014] S. Moro, P. Cortez en P. Rita. Een gegevensgestuurde benadering voor het voors pellen van het slagen van Bank telemarketing. Decision Support Systems, Elsevier, 62:22-31, juni 2014.
