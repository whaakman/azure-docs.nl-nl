---
title: Wat is geautomatiseerde ML / automl
titleSuffix: Azure Machine Learning service
description: Meer informatie over Azure Machine Learning-service kan automatisch een algoritme kiezen voor u, en genereert een model op basis van dat u tijd besparen met behulp van de parameters en de criteria die u opgeeft op de beste algoritme voor het model te selecteren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 06/20/2019
ms.custom: seodec18
ms.openlocfilehash: 137ef1ad78548053f3c5b8f30b7d83f2370f62da
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442439"
---
# <a name="what-is-automated-machine-learning"></a>Wat is geautomatiseerde machine learning?

Geautomatiseerde voor machine learning, is ook wel autoML, het proces van de tijd in beslag nemen, iteratieve Taakautomatisering van machine learning-model-ontwikkeling. Hierdoor kan gegevenswetenschappers, bedrijfsanalisten en ontwikkelaars voor het bouwen van ML-modellen met grote schaal, efficiëntie en productiviteit terwijl sustaining kwaliteit van model.

Traditionele machine learning-model ontwikkeling is resource-intensieve, waarvoor aanzienlijke domeinkennis en tijd produceren en tientallen modellen met elkaar vergelijken. Geautomatiseerde ML van toepassing als u wilt dat Azure Machine Learning te trainen en afstemmen van een model voor u gemaakt met de doel-metric die u opgeeft. ML-algoritmen die zijn gekoppeld aan de functie selecties, waarbij elke herhaling een model met een score training levert doorloopt de service vervolgens. Hoe hoger de score, hoe beter het model wordt beschouwd als "passen" uw gegevens.

Met geautomatiseerde machine learning, zult u de tijd die nodig zijn om op te halen gereed is voor productie ML-modellen met geweldige eenvoudig en efficiënt te versnellen.

## <a name="when-to-use-automated-ml"></a>Wanneer u geautomatiseerde ML

Geautomatiseerde ML democratizes van de machine learning-model ontwikkelingsproces en biedt uitgebreide mogelijkheden met de gebruikers, ongeacht hun data science-ervaring voor het identificeren van een end-to-end machine learning-pijplijn voor elk probleem.

Gegevenswetenschappers, bedrijfsanalisten en ontwikkelaars in branches kunnen geautomatiseerde ML te gebruiken:

+ Machine learning-oplossingen zonder uitgebreide kennis van programmeren implementeren
+ Bespaar tijd en resources
+ Gebruikmaken van best practices voor data science
+ Geef flexibele oplossen van problemen

## <a name="how-automated-ml-works"></a>Hoe geautomatiseerde ML werkt

Met behulp van **Azure Machine Learning-service**, u kunt ontwerpen en uitvoeren van uw geautomatiseerde experimenten voor ML-training met deze stappen:

1. **Het probleem ML** moet worden opgelost: classificatie, prognoses of regressie

1. **Geef de bron en de indeling van de gelabelde trainingsgegevens**: Numpy matrices of Pandas dataframe

1. **Configureren van de compute-doel voor modeltraining**, zoals uw [lokale computer, Azure Machine Learning berekent, externe virtuele machines of Azure Databricks](how-to-set-up-training-targets.md).  Meer informatie over geautomatiseerde training [op een externe bron](how-to-auto-train-remote.md).

1. **Configureer de geautomatiseerde machine learning-parameters** om te bepalen wanneer het aantal iteraties via verschillende modellen, hyperparameter instellingen, geavanceerde voorverwerking/parametrisatie en welke metrische gegevens om te kijken bij het bepalen van het beste model.  U kunt de instellingen voor automatische trainingsexperiment configureren [in Azure portal](how-to-create-portal-experiments.md) of [met de SDK](how-to-configure-auto-train.md).

1. **Dien de training uitvoeren.**

  ![Geautomatiseerde Machine learning](./media/how-to-automated-ml/automl-concept-diagram2.png)

Tijdens de training maakt de Azure Machine Learning-service een aantal in parallelle pijplijnen die verschillende algoritmen en parameters. Deze wordt gestopt zodra dit bij het afsluitcriteria die zijn gedefinieerd in het experiment aankomt.

U kunt ook de run logboekgegevens inspecteren die [metrische gegevens bevat](how-to-understand-accuracy-metrics.md) die zijn verzameld tijdens de uitvoering. De uitvoering van de training levert een Python-geserialiseerd object (`.pkl` bestand) dat het model en de voorverwerking van de gegevens bevat.

Terwijl de modelopbouw is geautomatiseerd, kunt u ook [informatie over hoe belangrijk of desbetreffende functies zijn](how-to-configure-auto-train.md#explain) naar de gegenereerde modellen.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]

<a name="preprocess"></a>

## <a name="preprocessing"></a>Voorverwerking

In elke geautomatiseerde machine learning-experiment, is uw gegevens met behulp van de standaardmethoden en eventueel via geavanceerde voorverwerking voorverwerkte.

### <a name="automatic-preprocessing-standard"></a>Automatische voorverwerking (standaard)

Uw gegevens is in elke geautomatiseerde machine learning-experiment, automatisch geschaald of genormaliseerd zodat algoritmen presteren.  Tijdens het trainen van het model, wordt een van de volgende schalen of normalisering technieken worden toegepast op elk model.

|Schalen&nbsp;&&nbsp;normalisering| Description |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Functies standaardiseren door te verwijderen van het gemiddelde en schalen naar eenheid afwijking  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Functies door het schalen van elke functie door de minimum en maximum van die kolom getransformeerd  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |Elke functie schalen door de maximale absolute waarde |
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |Deze Scaler-functies voor het bereik dat hun kwantiel |
| [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) |Lineaire dimensionaliteitsvermindering met behulp van enkelvoudige waarde ontleding van de gegevens aan het project deze tot een lagere dimensionale ruimte |
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |Lineaire dimensionaliteitsvermindering uitvoert deze transformator met behulp van de afgekapte enkelvoudige waarde ontleding (SVD). Niet overeenkomstig PCA center deze estimator de gegevens voordat u de enkelvoudige waarde ontleding computing niet. Dit betekent dat het efficiënt kunt werken met scipy.sparse matrices |
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | Elk voorbeeld (dat wil zeggen, elke rij van de matrix gegevens) met ten minste één niet-nul-component is opnieuw geschaald onafhankelijk van andere voorbeelden, zodat een gelijk is aan de norm (l1 of l2) |

### <a name="advanced-preprocessing-optional-featurization"></a>Geavanceerde voorverwerking: optionele parametrisatie

Als u meer geavanceerde voorverwerking en parametrisatie zijn ook beschikbaar is, zoals ontbrekende waarden toerekening, codering en -transformaties. [Meer informatie over welke parametrisatie opgenomen is](how-to-create-portal-experiments.md#preprocess). Schakel deze instelling met:

+ Azure Portal: Selecteren van de **voorverwerken** selectievakje in de **geavanceerde instellingen** [met deze stappen](how-to-create-portal-experiments.md).

+ Python-SDK: Op te geven `"preprocess": True` voor de [ `AutoMLConfig` klasse](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py).


## <a name="time-series-forecasting"></a>Prognoses met tijdreeksen
Het bouwen van prognoses is een integraal onderdeel van een bedrijf, of het omzet, voorraad, verkoop of klant aanvraag. Geautomatiseerde ML kunt u technieken en benaderingen combineren en ontvang een aanbevolen, hoogwaardige time series prognose. 

Een geautomatiseerde time series-experiment wordt beschouwd als een multidimensionale regressie-probleem. Afgelopen time series-waarden zijn "draaien" om extra dimensies voor de regressor zijn samen met andere voorspellingsfactoren geworden. Deze benadering, in tegenstelling tot klassieke time series-methoden, heeft een voordeel van het opnemen van op een natuurlijke manier meerdere contextuele variabelen en hun relatie tot elkaar tijdens de training. Geautomatiseerde ML leert een enkele, maar vaak intern vertakkingen model voor alle items in de gegevensset en voorspelling horizon. Meer gegevens is beschikbaar om te schatten Modelparameters en generalisatie op van de reeks niet waren herkend mogelijk wordt. 

Meer informatie en bekijk een voorbeeld van [geautomatiseerde van machine learning voor time series-prognoses](how-to-auto-train-forecast.md).

## <a name="ensemble-models"></a>Ensembles modellen

U kunt met behulp van geautomatiseerde machine learning met ensembles-modellen trainen de [Caruana ensembles selectie algoritme met gesorteerde ensembles initialisatie](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf). Ensembles learning verbetert de resultaten van machine learning en predictive prestaties door te veel modellen in plaats van met één modellen zoekfunctionaliteit door te combineren. De iteratie ensembles weergegeven als de laatste iteratie van de uitvoering.

## <a name="use-with-onnx-in-c-apps"></a>Gebruik met ONNX in C# apps

U kunt met Azure Machine Learning, geautomatiseerde ML gebruiken om te maken van een Python-model en deze naar de ONNX-indeling geconverteerd. Biedt ondersteuning voor de ONNX-runtime C#, zodat u kunt het model gebouwd automatisch in uw C# apps zonder dat hoeven te worden RE-architecting of een van de netwerklatenties waardoor er sprake van REST-eindpunten. Probeer een voorbeeld van deze stroom [in deze Jupyter-notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-with-onnx/auto-ml-classification-with-onnx.ipynb).

## <a name="automated-ml-across-microsoft"></a>Geautomatiseerde ML overal bij Microsoft

Geautomatiseerde ML is ook beschikbaar in andere Microsoft-oplossingen, zoals:

|Integraties|Description|
|------------|-----------|
|[ML.NET](https://docs.microsoft.com/dotnet/machine-learning/automl-overview)|Automatische modelselectie en training in .NET-apps met behulp van Visual Studio en Visual Studio Code met ML.NET geautomatiseerde ML (Preview).|
|[HDInsight](../../hdinsight/spark/apache-spark-run-machine-learning-automl.md)|Scale-out uw geautomatiseerde ML-trainingstaken op Spark in HDInsight-clusters tegelijk.|
|[PowerBI](https://docs.microsoft.com/power-bi/service-machine-learning-automated)|Machine learning-modellen rechtstreeks in Power BI (Preview) worden aangeroepen.|
|[SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/01/09/how-to-automate-machine-learning-on-sql-server-2019-big-data-clusters/)|Maak nieuwe machine learning-modellen voor uw gegevens in SQL Server 2019 big data-clusters.|

## <a name="next-steps"></a>Volgende stappen

Zie de voorbeelden en informatie over het bouwen van modellen met behulp van geautomatiseerde machine learning:

+ Ga als volgt de [zelfstudie: Automatisch een regressiemodel met Azure geautomatiseerde Machine Learning te trainen](tutorial-auto-train-models.md)

+ Configureer de instellingen voor automatische trainingsexperiment:
  + In Azure portal-interface, [Volg deze stappen](how-to-create-portal-experiments.md).
  + Met de Python-SDK [Volg deze stappen](how-to-configure-auto-train.md).

+ Meer informatie over het automatisch van de trein met behulp van time series-gegevens, [Volg deze stappen](how-to-auto-train-forecast.md).

+ Probeer [Jupyter-Notebook voor geautomatiseerde machine learning-voorbeelden](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/)
