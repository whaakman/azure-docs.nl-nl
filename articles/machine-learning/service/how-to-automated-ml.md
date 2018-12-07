---
title: Wat is geautomatiseerde Machine Learning - service van Azure Machine Learning
description: Meer informatie over hoe Azure Machine Learning-service kan automatisch een algoritme kiezen voor u en genereert een model op basis van het op te slaan dat met behulp van de parameters en de criteria die u opgeeft om te selecteren van het beste algoritme voor het model van uw tijd.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 4b9356c7cba14cf5bd112c7d7ae1aab2842fb9d1
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53010889"
---
# <a name="what-is-automated-machine-learning"></a>Wat is geautomatiseerde machine learning?

In dit artikel, kunt u meer informatie over automatische leerprocessen. De Azure Machine Learning-service kan automatisch een algoritme kiezen voor u en genereert een model op basis van het. Geautomatiseerde machine learning-bespaart u tijd door het genereren van modellen die zijn afgestemd op van de doelstellingen en beperkingen die u hebt gedefinieerd voor uw experiment, zoals de tijd voor het experiment dat moet worden uitgevoerd of die op de zwarte-modellen.

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

1. Configureer de configuratie van de geautomatiseerde machine learning. Hiermee bepaalt u de parameters die worden gebruikt als Azure Machine Learning over verschillende modellen, hyperparameter instellingen doorloopt, en het bepalen van welke metrische gegevens om te kijken wanneer het beste model. 

1. Dien een training uitvoeren.

Tijdens de training maakt de Azure Machine Learning-service een aantal pijplijnen die verschillende algoritmen en parameters probeert. Deze wordt gestopt zodra deze treffers in de limiet voor iteraties die u opgeven of wanneer het de doelwaarde voor de metrische gegevens die u opgeeft is bereikt.

[ ![Geautomatiseerde Machine learning](./media/how-to-automated-ml/automated-machine-learning.png) ](./media/how-to-automated-ml/automated-machine-learning.png#lightbox)

U kunt de run logboekgegevens, waarin de metrische gegevens die zijn verzameld tijdens de uitvoering inspecteren. De uitvoering van de training levert ook een Python geserialiseerd object (.pkl-bestand) met het model en de voorverwerking van gegevens.

## <a name="next-steps"></a>Volgende stappen

Zie de voorbeelden en informatie over het bouwen van modellen met behulp van geautomatiseerde Machine Learning:

+ [Zelfstudie: Automatisch een classificatie model trainen met Azure geautomatiseerde Machine Learning](tutorial-auto-train-models.md)

+ [Instellingen voor automatische training configureren](how-to-configure-auto-train.md)

+ [Het gebruik van automatische training op een externe bron](how-to-auto-train-remote.md) 
