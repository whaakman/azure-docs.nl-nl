---
title: "Voorbeeldexperimenten van Machine Learning kopiëren - Azure | Microsoft Docs"
description: Leer hoe u voorbeeldexperimenten voor Machine Learning gebruikt om met Cortana Intelligence Gallery en Microsoft Azure Machine Learning nieuwe experimenten te maken.
keywords: machine learning voorbeelden, voorbeeldexperiment, machine learning voorbeeld
services: machine-learning
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: 81e6c1d8-682c-4db3-bfd5-d7bfb1150ff3
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/28/2017
ms.author: cgronlun
ms.openlocfilehash: f798ac1b46d702dbb96a2384d96f2d14eb3cac0e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="copy-example-experiments-to-create-new-machine-learning-experiments"></a>Voorbeeldexperimenten kopiëren om nieuwe experimenten voor Machine Learning te maken
Leer hoe u met voorbeeldexperimenten van [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) aan de slag gaat in plaats van experimenten voor Machine Learning helemaal opnieuw te maken. U kunt de voorbeelden gebruiken om uw eigen Machine Learning-oplossing te maken.

De galerie bevat voorbeeldexperimenten van het Microsoft Azure Machine Learning-team en voorbeelden die door de Machine Learning-community zijn gedeeld. Hier kunt u ook vragen stellen of opmerkingen plaatsen over experimenten.

Bekijk de video [Copy other people's work to do data science](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) (Werk van anderen kopiëren om gegevenswetenschap uit te voeren) uit de serie [Data Science for Beginners](data-science-for-beginners-the-5-questions-data-science-answers.md) (Gegevenswetenschap voor beginners). Deze video duurt 3 minuten.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="find-an-experiment-to-copy-in-cortana-intelligence-gallery"></a>Een experiment in Cortana Intelligence Gallery zoeken om te kopiëren
Als u wilt zien welke experimenten beschikbaar zijn, gaat u naar [Gallery](https://gallery.cortanaintelligence.com/) en klikt u boven aan de pagina op **Experiments**.

### <a name="find-the-newest-or-most-popular-experiments"></a>De nieuwste of meest populaire experimenten zoeken
Op deze pagina ziet u **onlangs toegevoegde** experimenten, of scroll omlaag om te zien **wat populair** is of wat de nieuwste, **populaire Microsoft-experimenten** zijn.

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a>Naar een experiment zoeken dat aan specifieke vereisten voldoet
Door alle experimenten bladeren:

1. Klik boven aan de pagina op **Browse all**.
2. Selecteer aan de linkerkant onder **Refine by**, in het gedeelte **Categories**, de optie **Experiment** om alle experimenten in de galerie weer te geven.
3. U kunt op verschillende manieren experimenten vinden die aan uw vereisten voldoen:
   * **Selecteer filters aan de linkerkant.** Als u bijvoorbeeld door experimenten wilt bladeren die gebruikmaken van een PCA-algoritme voor de detectie van afwijkingen, selecteert u **Experiment** onder **Categories** en klikt u op **Show all**. Kies vervolgens onder **Algorithms Used** de optie **PCA-Based Anomaly Detection**. <br></br>
     ![Filters selecteren](./media/sample-experiments/refine-the-view.png)
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
