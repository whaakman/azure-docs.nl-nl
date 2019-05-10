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
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 136141f5b598fd080edf3254fd01200f2742c763
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65235828"
---
# <a name="what-is-automated-machine-learning"></a>Wat is geautomatiseerde machine learning?

Geautomatiseerde machine learning, ook wel AutoML, kan gegevenswetenschappers, bedrijfsanalisten en ontwikkelaars voor het bouwen van ML-modellen met grote schaal, efficiëntie en productiviteit terwijl sustaining kwaliteit van model. 

Geautomatiseerde ML bouwt een set met ML-modellen automatisch, op intelligente wijze de modellen voor training selecteren en vervolgens raadt het programma de beste is voor u. Traditionele machine learning-model ontwikkeling is resource-intensieve waarvoor aanzienlijke domeinkennis en tijd produceren en tientallen modellen met elkaar vergelijken. Met geautomatiseerde ML, zult u de tijd die nodig zijn om op te halen gereed is voor productie ML-modellen met geweldige eenvoudig en efficiënt te versnellen.

Uw trainingsgegevens is achter de schermen die zijn gemaakt met een gedefinieerd doel-functie en intelligente herhaald na via combinaties van ML-algoritmes en -onderdelen. Vervolgens, op basis van scores van de training, het beste model is geïdentificeerd en aan u zijn aanbevolen. 

U hebt nog steeds controle over uw experiment en transparantie over wat er gebeurt. U kunt beperkingen definiëren en doelstellingen op basis van tijd, nauwkeurigheid, of het aantal iteraties, bijvoorbeeld experimenteren. U kunt elk model dat is gegenereerd voor uw experiment, de training-stroom voor elke herhaling en de meest invloedrijke functies voor een bepaald model bekijken.

## <a name="how-automated-ml-works"></a>Hoe geautomatiseerde ML werkt

Met behulp van **Azure Machine Learning-service**, u kunt ontwerpen en uitvoeren van uw geautomatiseerde experimenten voor ML-training met deze stappen:

1. **Het probleem ML** moet worden opgelost: classificatie, prognoses of regressie
   
1. **Geef de bron en de indeling van de gelabelde trainingsgegevens**: Numpy matrices of Pandas dataframe

1. **Configureren van de compute-doel voor modeltraining**, zoals uw [lokale computer, Azure Machine Learning berekent, externe virtuele machines of Azure Databricks](how-to-set-up-training-targets.md).  Meer informatie over geautomatiseerde training [op een externe bron](how-to-auto-train-remote.md).

1. **Configureer de geautomatiseerde machine learning-parameters** om te bepalen wanneer het aantal iteraties via verschillende modellen, hyperparameter instellingen, geavanceerde voorverwerking/parametrisatie en welke metrische gegevens om te kijken bij het bepalen van het beste model.  U kunt de instellingen voor automatische trainingsexperiment configureren [in Azure portal](how-to-create-portal-experiments.md) of [met de SDK](how-to-configure-auto-train.md).

1. **Dien de training uitvoeren.** 


[![Geautomatiseerde Machine learning](./media/how-to-automated-ml/automated-machine-learning.png)](./media/how-to-automated-ml/automated-machine-learning.png#lightbox)

Tijdens de training maakt de Azure Machine Learning-service een aantal pijplijnen die verschillende algoritmen en parameters probeert. Deze wordt gestopt zodra dit bij het afsluitcriteria die zijn gedefinieerd in het experiment aankomt. 

U kunt ook de geregistreerde uitvoeren gegevens, metrische gegevens die zijn verzameld tijdens de uitvoering met inspecteren. De uitvoering van de training levert een Python-geserialiseerd object (`.pkl` bestand) dat het model en de voorverwerking van de gegevens bevat.


Terwijl de modelopbouw is geautomatiseerd, kunt u ook [informatie over hoe belangrijk of desbetreffende functies zijn](how-to-configure-auto-train.md#explain) naar de gegenereerde modellen. 

> [!VIDEO https://www.youtube.com/embed/l8c-4iDPE0M]

<a name="preprocess"></a>

## <a name="preprocessing"></a>Voorverwerking

In elke geautomatiseerde machine learning-experiment, is uw gegevens met behulp van de standaardmethoden en eventueel via geavanceerde voorverwerking voorverwerkte.

### <a name="automatic-preprocessing-standard"></a>Automatische voorverwerking (standaard)
Uw gegevens is in elke geautomatiseerde machine learning-experiment, automatisch geschaald of genormaliseerd zodat algoritmen presteren.  Tijdens het trainen van het model, wordt een van de volgende schalen of normalisering technieken worden toegepast op elk model.

|Schalen&nbsp;&&nbsp;normalisering| Description |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Functies standaardiseren door te verwijderen van het gemiddelde en schalen naar eenheid afwijking  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Functies door het schalen van elke functie door de minimum en maximum van die kolom getransformeerd  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |    Elke functie schalen door de maximale absolute waarde |  
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |   Deze Scaler-functies voor het bereik dat hun kwantiel |
| [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) | Lineaire dimensionaliteitsvermindering met behulp van enkelvoudige waarde ontleding van de gegevens aan het project deze tot een lagere dimensionale ruimte | 
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |    Lineaire dimensionaliteitsvermindering uitvoert deze transformator met behulp van de afgekapte enkelvoudige waarde ontleding (SVD). Niet overeenkomstig PCA center deze estimator de gegevens voordat u de enkelvoudige waarde ontleding computing niet. Dit betekent dat het efficiënt kunt werken met scipy.sparse matrices | 
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | Elk voorbeeld (dat wil zeggen, elke rij van de matrix gegevens) met ten minste één niet-nul-component is schaal gebracht weglating onafhankelijk van andere voorbeelden zodat een gelijk is aan de norm (l1 of l2) | 

### <a name="advanced-preprocessing-optional-featurization"></a>Geavanceerde voorverwerking: optionele parametrisatie

Als u meer geavanceerde voorverwerking en parametrisatie zijn ook beschikbaar is, zoals ontbrekende waarden toerekening, codering en -transformaties. [Meer informatie over welke parametrisatie opgenomen is](how-to-create-portal-experiments.md#preprocess). Schakel deze instelling met:
+ Azure Portal: Selecteren van de **voorverwerken** selectievakje in de **geavanceerde instellingen** [met deze stappen](how-to-create-portal-experiments.md). 
+ Python-SDK: Op te geven `"preprocess": True` voor de [ `AutoMLConfig` klasse](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py).

## <a name="ensemble-models"></a>Ensembles modellen

U kunt met behulp van geautomatiseerde machine learning met ensembles-modellen trainen de [Caruana ensembles selectie algoritme met gesorteerde ensembles initialisatie](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf). Ensembles learning verbetert de resultaten van machine learning en predictive prestaties door te veel modellen in plaats van met één modellen zoekfunctionaliteit door te combineren. De iteratie ensembles weergegeven als de laatste iteratie van de uitvoering.

## <a name="use-with-onnx-in-c-apps"></a>Gebruik met ONNX in C# apps

U kunt met Azure Machine Learning, geautomatiseerde ML gebruiken om te maken van een Python-model en deze naar de ONNX-indeling geconverteerd. Biedt ondersteuning voor de ONNX-runtime C#, zodat u kunt het model gebouwd automatisch in uw C# apps zonder dat hoeven te worden RE-architecting of een van de netwerklatenties waardoor er sprake van REST-eindpunten. Probeer een voorbeeld van deze stroom [in deze Jupyter-notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-with-onnx/auto-ml-classification-with-onnx.ipynb).

## <a name="automated-ml-across-microsoft"></a>Geautomatiseerde ML overal bij Microsoft

Geautomatiseerde ML is ook beschikbaar in andere Microsoft-oplossingen, zoals:
+ In .NET-apps met behulp van Visual Studio en Visual Studio Code met [ML.NET](https://docs.microsoft.com/dotnet/machine-learning/automl-overview)
+ [Op HDInsight](../../hdinsight/spark/apache-spark-run-machine-learning-automl.md), waar u uw geautomatiseerde ML trainingstaken op Spark in HDInsight-clusters parallel uitschalen. 
+ [In Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-automated)

## <a name="next-steps"></a>Volgende stappen

Zie de voorbeelden en informatie over het bouwen van modellen met behulp van geautomatiseerde Machine Learning:

+ Ga als volgt de [zelfstudie: Automatisch een classificatie model trainen met Azure geautomatiseerde Machine Learning](tutorial-auto-train-models.md)

+ Configureer de instellingen voor automatische trainingsexperiment: 
   + In Azure portal-interface, [Volg deze stappen](how-to-create-portal-experiments.md).
   + Met de Python-SDK [Volg deze stappen](how-to-configure-auto-train.md).
  
 + Meer informatie over het automatisch van de trein met behulp van time series-gegevens, [Volg deze stappen](how-to-auto-train-forecast.md).

+ Probeer [Jupyter-Notebook-voorbeelden](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/)
