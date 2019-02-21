---
title: Machine Learning Studio-experimenten maken van voorbeelden
titleSuffix: Azure Machine Learning Studio
description: Leer hoe u voorbeeldexperimenten voor machine learning gebruikt nieuwe experimenten te maken met Azure AI Gallery en Azure Machine Learning Studio gebruiken.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18, previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 01/05/2018
ms.openlocfilehash: 221f78dbeb6bfb0c76ac12280511a8d5a5138c38
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56453406"
---
# <a name="create-azure-machine-learning-studio-experiments-from-working-examples-in-azure-ai-gallery"></a>Azure Machine Learning Studio-experimenten maken van werkende voorbeelden in Azure AI Gallery

Leer hoe u met voorbeeldexperimenten uit [Azure AI Gallery](https://gallery.azure.ai/) aan de slag gaat in plaats van experimenten voor Machine Learning helemaal opnieuw te maken. U kunt de voorbeelden gebruiken om uw eigen Machine Learning-oplossing te maken.

De galerie heeft voorbeeldexperimenten door het team van Microsoft Azure Machine Learning Studio, evenals de voorbeelden die worden gedeeld door de community van Machine Learning. Hier kunt u ook vragen stellen of opmerkingen plaatsen over experimenten.

Bekijk de video [Copy other people's work to do data science](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) (Werk van anderen kopiëren om gegevenswetenschap uit te voeren) uit de serie [Data Science for Beginners](data-science-for-beginners-the-5-questions-data-science-answers.md) (Gegevenswetenschap voor beginners). Deze video duurt 3 minuten.



## <a name="find-an-experiment-to-copy-in-azure-ai-gallery"></a>Een experiment zoeken om in Azure AI Gallery te kopiëren
Als u wilt zien welke experimenten beschikbaar zijn, gaat u naar [Gallery](https://gallery.azure.ai/) en klikt u boven aan de pagina op **Experiments**.

### <a name="find-the-newest-or-most-popular-experiments"></a>De nieuwste of meest populaire experimenten zoeken
Op deze pagina ziet u **onlangs toegevoegde** experimenten, of scroll omlaag om te zien **wat populair** is of wat de nieuwste, **populaire Microsoft-experimenten** zijn.

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a>Naar een experiment zoeken dat aan specifieke vereisten voldoet
Door alle experimenten bladeren:

1. Klik boven aan de pagina op **Browse all**.
2. Selecteer aan de linkerkant onder **Refine by**, in het gedeelte **Categories**, de optie **Experiment** om alle experimenten in de galerie weer te geven.
3. U kunt op verschillende manieren experimenten vinden die aan uw vereisten voldoen:
   * **Selecteer filters aan de linkerkant.** Bijvoorbeeld, om te bladeren-experimenten die gebruikmaken van een PCA-gebaseerd anomaliedetectie-algoritme: Onder **categorieën** klikt u op **Experiment**. Klik vervolgens onder **Algorithms Used** op **Show All** en kies in het dialoogvenster de optie **PCA-Based Anomaly Detection**. Misschien moet u scrollen om de optie te zien.<br></br>
     ![Filters selecteren](./media/sample-experiments/choose-an-algorithm.png)
   * **Het zoekvak gebruiken.** Als u bijvoorbeeld experimenten wilt vinden die door Microsoft zijn aangedragen en die betrekking hebben op cijferherkenning waarbij gebruik wordt gemaakt van een algoritme van twee klassen voor ondersteuningsvectormachines, voert u 'digit recognition' (cijferherkenning) in het zoekvak in. Selecteer vervolgens de filters **Experiment**, **Microsoft content only** en **Two-Class Support Vector Machine**:<br></br>
     ![Het zoekvak gebruiken](./media/sample-experiments/search-for-experiments.png)
4. Klik op een experiment voor meer informatie.
5. Als u het experiment wilt uitvoeren of wijzigen, klikt u op **Open in Studio** op de pagina van het experiment. <br></br>

    ![Voorbeeldexperiment](./media/sample-experiments/example-experiment.png)

    > [!NOTE]
    > Wanneer u voor het eerst een experiment in Machine Learning Studio opent, kunt u de service gratis proberen of een Azure-abonnement aanschaffen. [Meer informatie over de gratis proefversie van Machine Learning Studio versus de betaalde service](https://azure.microsoft.com/pricing/details/machine-learning/)
    >
    >

## <a name="create-a-new-experiment-using-an-example-as-a-template"></a>Een nieuw experiment maken op basis van een sjabloon
U kunt ook een nieuw experiment in Machine Learning Studio maken op basis van een sjabloon uit de Galerie.

1. Meld u bij [Studio](https://studio.azureml.net) aan met uw Microsoft-accountreferenties en klik vervolgens op **New** om een nieuw experiment te maken.
2. Blader door de voorbeelden en klik op een van de voorbeelden.

Er wordt een nieuw experiment gemaakt in de Machine Learning Studio-werkruimte op basis van het voorbeeldexperiment als sjabloon.

## <a name="next-steps"></a>Volgende stappen
* [Gegevens importeren uit diverse bronnen](import-data.md)
* [Snelstartzelfstudie voor de programmeertaal R in Machine Learning](r-quickstart.md)
* [Een Machine Learning-webservice implementeren](publish-a-machine-learning-web-service.md)
