---
title: Automatische selectie van ML-algoritme en afstemmen
titleSuffix: Azure Machine Learning service
description: Meer informatie over Azure Machine Learning-service kan automatisch een algoritme kiezen voor u, en genereert een model op basis van dat u tijd besparen met behulp van de parameters en de criteria die u opgeeft op de beste algoritme voor het model te selecteren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 12/12/2018
ms.custom: seodec18
ms.openlocfilehash: 88eb7052c394d41c861ffc422738a4a650101ea7
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56819306"
---
# <a name="what-is-automated-machine-learning"></a>Wat is geautomatiseerde machine learning?

Geautomatiseerde machine learning is het proces van het trainen van gegevens met de functie van een gedefinieerd doel nemen en combinaties van algoritmen en functie van selecties automatisch het beste model selecteren voor uw gegevens op basis van de scores training doorlopen. De traditionele machine learning-model ontwikkelingsproces is zeer resource-intensieve en aanzienlijke domein kennis en het tijdstip investering uitvoeren en vergelijk de resultaten van tientallen modellen vereist. Geautomatiseerde machine learning vereenvoudigt u dit proces door het genereren van modellen die zijn afgestemd op van de doelstellingen en beperkingen die u hebt gedefinieerd voor uw experiment, zoals de tijd voor het experiment dat moet worden uitgevoerd of die op de zwarte-modellen.

## <a name="how-it-works"></a>Hoe werkt het?

1. Configureren van het type machine learning probleem dat u probeert op te lossen. Categorieën van leren met supervisie worden ondersteund:
   + Classificatie
   + Regressie
   + Prognose

   Terwijl geautomatiseerde machine learning algemeen beschikbaar is, **met de prognosefunctie is nog steeds in openbare preview.**

   Zie de [lijst met modellen](how-to-configure-auto-train.md#select-your-experiment-type) Azure Machine Learning kunt proberen bij het trainen van.

1. U geeft de bron en de indeling voor het trainen van gegevens. De gegevens moeten worden gemarkeerd en kunnen worden opgeslagen op uw ontwikkelomgeving of in Azure Blob Storage. Als de gegevens worden opgeslagen op uw ontwikkelomgeving, moet deze zich in dezelfde map als uw trainingsscripts. Deze map wordt gekopieerd naar de compute-doel dat u voor de training selecteert.

    In uw trainingsscript, kunnen de gegevens worden gelezen in Numpy matrices of een Pandas dataframe.

    Kunt u Splitsingsopties voor het selecteren van training en validatie gegevens configureren of kunt u afzonderlijke trainings- en validatie van gegevenssets.

1. Configureer de [compute-doel](how-to-set-up-training-targets.md) die wordt gebruikt voor het model te trainen.

1. Configureer de configuratie van de geautomatiseerde machine learning. Hiermee bepaalt u de parameters die worden gebruikt als Azure Machine Learning over verschillende modellen, hyperparameter instellingen doorloopt, en het bepalen van welke metrische gegevens om te kijken wanneer het beste model

1. Dien een training uitvoeren.

Tijdens de training maakt de Azure Machine Learning-service een aantal pijplijnen die verschillende algoritmen en parameters probeert. Deze wordt gestopt zodra deze treffers in de limiet voor iteraties die u opgeven of wanneer het de doelwaarde voor de metrische gegevens die u opgeeft is bereikt.

[ ![Geautomatiseerde Machine learning](./media/how-to-automated-ml/automated-machine-learning.png) ](./media/how-to-automated-ml/automated-machine-learning.png#lightbox)

U kunt de run logboekgegevens, waarin de metrische gegevens die zijn verzameld tijdens de uitvoering inspecteren. De uitvoering van de training levert ook een Python-geserialiseerd object (`.pkl` bestand) dat het model en de voorverwerking van de gegevens bevat.

## <a name="model-explainability"></a>Model explainability

Een algemene zich van geautomatiseerde machine learning is onvermogen om te zien van de end-to-end-proces. Azure Machine Learning kunt u gedetailleerde informatie weergeven over de modellen transparantie in wat wordt uitgevoerd op de back-end te vergroten. Sommige modellen, zoals lineaire regressie, worden beschouwd als relatief eenvoudige manier en daarom gemakkelijk te begrijpen. Maar als we meer functies toevoegen en gebruiken van meer complexe machine learning-modellen, informatie over deze moeilijker opgehaald. Er zijn twee belangrijke aspecten om transparantie in machine learning:

1. Kennis van de machine learning-pijplijn en alle de stappen die betrokken zijn, waaronder gegevens voorverwerken/parametrisatie en hyperparameter waarden.
1. De relatie tussen invoervariabelen (ook wel bekend als ' functies') en de model-uitvoer.  De omvang en de richting van de impact van elke functie van de voorspelde waarde te weten, kunt beter te begrijpen en leg uit het model. Dit staat bekend als functie urgentie.

U kunt globale functie belang op aanvraag post-training voor de pijplijn van uw keuze, of deze inschakelen voor alle pijplijnen als onderdeel van geautomatiseerde machine learning-cursussen. In veel gereguleerde branches, zoals de gezondheidszorg en bankieren is dit essentieel om te voldoen aan regelgeving en best practices.  Hier volgen enkele echte scenario's om te illustreren:

1. Een productiebedrijf met behulp van machine learning om te voorspellen van toekomstige instrument fout, zodat ze kunnen proactief uitvoeren onderhoudsactiviteiten. Zodra u weet dat een instrument is mislukt dat, wat de meest waarschijnlijke oorzaak er dus preventief onderhoud kan snel worden uitgevoerd?
1. Een financiële instelling met behulp van machine learning voor proces lening of creditcard toepassingen. Hoe wilt u weten als het model de juiste dingen doet, en als een klant gevraagd voor meer informatie over waarom de aanvraag is afgewezen, hoe u op deze reageert?
1. Een online winkel of een onafhankelijke softwareleverancier met behulp van machine learning om te voorspellen klantverloop. Wat zijn de belangrijkste bijdragers aan de klant verloop, en hoe kunt u voorkomen dat klanten herhaald?

Dit is een preview-functie en we blijven investeren in uitgebreidere informatie waarmee u beter inzicht in uw machine learning-modellen bieden. Volg deze [voorbeeld notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/model-explanation/auto-ml-model-explanation.ipynb) om te experimenteren met uitleg model in Azure Machine Learning.

## <a name="next-steps"></a>Volgende stappen

Zie de voorbeelden en informatie over het bouwen van modellen met behulp van geautomatiseerde Machine Learning:
+ [Voorbeelden: Jupyter-notebooks gebruiken om te verkennen van Azure Machine Learning-service](samples-notebooks.md#automated-ml-setup)

+ [Zelfstudie: Automatisch een classificatie model trainen met Azure geautomatiseerde Machine Learning](tutorial-auto-train-models.md)

+ [Gebruik automatische training op een externe bron](how-to-auto-train-remote.md)

+ [Instellingen configureren voor automatische training](how-to-configure-auto-train.md)
