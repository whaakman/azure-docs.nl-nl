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
ms.date: 05/21/2019
ms.custom: seodec18
ms.openlocfilehash: df666843b8a42711aa860a6f8c5e916fb68f036d
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389903"
---
# <a name="what-is-automated-machine-learning"></a>Wat is geautomatiseerde machine learning?

Geautomatiseerde voor machine learning, is ook wel genoemd automatische ML, het proces van de tijd in beslag nemen, iteratieve Taakautomatisering van machine learning-model-ontwikkeling. Hierdoor kan gegevenswetenschappers, bedrijfsanalisten en ontwikkelaars voor het bouwen van ML-modellen met grote schaal, efficiëntie en productiviteit terwijl sustaining kwaliteit van model.

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

U kunt ook de geregistreerde uitvoeren gegevens, metrische gegevens die zijn verzameld tijdens de uitvoering met inspecteren. De uitvoering van de training levert een Python-geserialiseerd object (`.pkl` bestand) dat het model en de voorverwerking van de gegevens bevat.

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

## <a name="ensemble-models"></a>Ensembles modellen

U kunt met behulp van geautomatiseerde machine learning met ensembles-modellen trainen de [Caruana ensembles selectie algoritme met gesorteerde ensembles initialisatie](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf). Ensembles learning verbetert de resultaten van machine learning en predictive prestaties door te veel modellen in plaats van met één modellen zoekfunctionaliteit door te combineren. De iteratie ensembles weergegeven als de laatste iteratie van de uitvoering.

## <a name="training-metric-output"></a>Training metrische uitvoer

Er zijn meerdere manieren om training nauwkeurigheid metrische gegevens voor elke herhaling van de uitvoering weer te geven.

* Gebruik de widget Jupyter.
* Gebruik de `get_metrics()` functie op een `Run` object.
* De metrische gegevens weergeven in de Azure portal in uw experiment.

### <a name="classification-metrics"></a>Classificatie metrische gegevens

De volgende metrische gegevens worden opgeslagen in elke iteratie uitvoeren voor een classificatietaak.

|Gegevens|Description|Berekening|Extra Parameters
--|--|--|--|
AUC_Macro| AUC is het gebied onder de ontvanger operationele Characteristic-Curve. Macro is het rekenkundige gemiddelde van de AUC voor elke categorie.  | [Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | gemiddelde = "macro"|
AUC_Micro| AUC is het gebied onder de ontvanger operationele Characteristic-Curve. Micro wordt wereldwijd berekend door het echt positieven en fout-positieven van elke klasse te combineren| [Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | gemiddelde = "micro"|
AUC_Weighted  | AUC is het gebied onder de ontvanger operationele Characteristic-Curve. Gewogen is het rekenkundige gemiddelde van de score van elke klasse, gewogen door het aantal waar elke klasse-instanties| [Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|gemiddelde = "gewogen"
accuracy|Nauwkeurigheid is het percentage van de voorspelde labels die precies overeenkomen met de waarde true labels. |[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |Geen|
average_precision_score_macro|Gemiddelde precisie bevat een overzicht van een curve precisie-/ oproepdiagram als het gewogen gemiddelde van Precision-systemen op elke drempelwaarde, met de toename in het intrekken van de vorige drempel gebruikt als het gewicht behaald. Macro is het rekenkundige gemiddelde van de gemiddelde precisie score van elke klasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|gemiddelde = "macro"|
average_precision_score_micro|Gemiddelde precisie bevat een overzicht van een curve precisie-/ oproepdiagram als het gewogen gemiddelde van Precision-systemen op elke drempelwaarde, met de toename in het intrekken van de vorige drempel gebruikt als het gewicht behaald. Micro wordt wereldwijd berekend door het echt positieven en fout-positieven op elke afsluitdatum zoekfunctionaliteit door te combineren|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|gemiddelde = "micro"|
average_precision_score_weighted|Gemiddelde precisie bevat een overzicht van een curve precisie-/ oproepdiagram als het gewogen gemiddelde van Precision-systemen op elke drempelwaarde, met de toename in het intrekken van de vorige drempel gebruikt als het gewicht behaald. Gewogen is het rekenkundige gemiddelde van de gemiddelde precisie score voor elke objectklasse, gewogen door het aantal waar elke klasse-instanties|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|gemiddelde = "gewogen"|
balanced_accuracy|Met gelijke taakverdeling nauwkeurigheid is het rekenkundige gemiddelde van terugroeping voor elke categorie.|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "macro"|
f1_score_macro|F1 score is het gemiddelde harmonische van precisie- en intrekken. Macro is het rekenkundige gemiddelde van F1 score voor elke objectklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|gemiddelde = "macro"|
f1_score_micro|F1 score is het gemiddelde harmonische van precisie- en intrekken. Micro wordt wereldwijd berekend door het tellen van de totale echt positieven, false negatieven en fout-positieven|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|gemiddelde = "micro"|
f1_score_weighted|F1 score is het gemiddelde harmonische van precisie- en intrekken. Gewogen gemiddelde frequentie van de klasse van F1 score voor elke objectklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|gemiddelde = "gewogen"|
log_loss|Dit is de verlies-functie die wordt gebruikt voor logistieke regressie (wordt) en -extensies van het zoals neurale netwerken, gedefinieerd als de negatieve log kans van de waarde true labels een probabilistic classificatie voorspellingen gegeven. Label voor een enkele voorbeeld met waar yt in {0,1} en de verwachte kans yp die yt = 1, het verlies van het logboek is - P aanmelden (yt&#124;yp) =-(yt log(yp) + (1 - yt) logboek (1 - yp))|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|Geen|
norm_macro_recall|Genormaliseerde Macro intrekken is Macro intrekken genormaliseerd zodat willekeurige prestaties een score van 0 krijgt en perfecte prestaties een score van 1 krijgt. Dit wordt bereikt door norm_macro_recall: (recall_score_macro - R) = /(1-R), waarbij R de verwachte waarde van recall_score_macro voor willekeurige voorspellingen (dat wil zeggen, R = 0,5 voor binaire classificatie) en R=(1/C) voor C-klasse classificatie problemen|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "macro" en vervolgens (recall_score_macro - R) /(1-R), waarbij R de verwachte waarde van recall_score_macro voor willekeurige voorspellingen (dat wil zeggen, R = 0,5 voor binaire classificatie) en R=(1/C) voor C-klasse classificatie problemen|
precision_score_macro|De precisie is het percentage van de elementen die worden aangeduid als een bepaalde klasse die daadwerkelijk in die klasse. Macro is het rekenkundige gemiddelde van precisie voor elke objectklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|gemiddelde = "macro"|
precision_score_micro|De precisie is het percentage van de elementen die worden aangeduid als een bepaalde klasse die daadwerkelijk in die klasse. Micro wordt wereldwijd berekend door het tellen van de totale echt positieven en fout-positieven|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|gemiddelde = "micro"|
precision_score_weighted|De precisie is het percentage van de elementen die worden aangeduid als een bepaalde klasse die daadwerkelijk in die klasse. Gewogen is het rekenkundige gemiddelde van precisie voor elke objectklasse, gewogen door het aantal waar elke klasse-instanties|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|gemiddelde = "gewogen"|
recall_score_macro|Intrekken is het percentage van de elementen in een bepaalde klasse daadwerkelijk die goed zijn gelabeld. Macro is het rekenkundige gemiddelde van terugroeping voor elke objectklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "macro"|
recall_score_micro|Intrekken is het percentage van de elementen in een bepaalde klasse daadwerkelijk die goed zijn gelabeld. Micro wordt wereldwijd berekend door het totaal aantal echt positieven, false negatieven tellen|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "micro"|
recall_score_weighted|Intrekken is het percentage van de elementen in een bepaalde klasse daadwerkelijk die goed zijn gelabeld. Gewogen is het rekenkundige gemiddelde van terugroeping voor elke objectklasse, gewogen door het aantal waar elke klasse-instanties|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "gewogen"|
weighted_accuracy|Gewogen nauwkeurigheid is nauwkeurigheid waarin het gewicht gegeven aan elk voorbeeld gelijk is aan het aandeel van true-exemplaren in dat de waarde true voorbeeldklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|sample_weight is gelijk aan het aandeel van die klasse voor elk element in de doel-vector|

### <a name="regression-and-forecasting-metrics"></a>Regressie worden gevolgd en voorspeld metrische gegevens

De volgende metrische gegevens worden opgeslagen in elke iteratie uitvoeren voor een regressie of prognoses taak.

|Gegevens|Description|Berekening|Extra Parameters
--|--|--|--|
explained_variance|Uitgelegd afwijking is de verhouding die een wiskundige model gebruikersaccounts voor de variant van een bepaalde verzameling. Het is dat het percentage variantie van de oorspronkelijke gegevens om de afwijking van de fouten te verlagen. Wanneer het gemiddelde van de fouten 0 is, is het gelijk aan de afwijking van uitgelegd.|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|Geen|
r2_score|R2 is de coëfficiënt bepalen of de procent vermindering in gekwadrateerde fouten ten opzichte van een basislijn-model dat u het gemiddelde weergeeft. Wanneer het gemiddelde van de fouten 0 is, is het gelijk aan de afwijking van uitgelegd.|[Berekening](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|Geen|
spearman_correlation|Spearman correlatie is een nonparametric meting van de monotonicity van de relatie tussen twee gegevenssets. In tegenstelling tot de correlatie Pearson de correlatie Spearman wordt niet vanuit gegaan dat beide gegevenssets normaal gesproken worden gedistribueerd. Dit varieert net als andere coëfficiënten correlatie tussen-1 en + 1 met 0, wat geen correlatie impliceert. Correlaties op basis van -1 of + 1 houdt in dat een exacte monotone relatie. Positieve correlaties impliceren dat x toeneemt, dus y wordt. Negatieve correlaties impliceren dat x toeneemt, y afneemt.|[Berekening](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|Geen|
mean_absolute_error|Mean absolute fout is de verwachte waarde van de absolute waarde van het verschil tussen het doel en de voorspelling|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Geen|
normalized_mean_absolute_error|Genormaliseerde mean absolute fout is mean Absolute Error gedeeld door het bereik van de gegevens|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Delen door het bereik van de gegevens|
median_absolute_error|Mediaan absolute fout is de mediaan van alle absolute verschillen tussen het doel en de voorspelling. Dit verlies is robuuste naar uitbijters.|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Geen|
normalized_median_absolute_error|Genormaliseerde mediaan absolute fout is mediaan absolute fout gedeeld door het bereik van de gegevens|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Delen door het bereik van de gegevens|
root_mean_squared_error|Root mean squared fout is de vierkantswortel van het verwachte gekwadrateerde verschil tussen het doel en de voorspelling|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Geen|
normalized_root_mean_squared_error|Genormaliseerde hoofdmap mean squared fout root mean squared fout gedeeld door het bereik van de gegevens is|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Delen door het bereik van de gegevens|
root_mean_squared_log_error|Root mean squared log-fout is de vierkantswortel van de verwachte gekwadrateerde logaritmische fout|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Geen|
normalized_root_mean_squared_log_error|Genormaliseerde Root mean squared log-fout is root mean squared log fout gedeeld door het bereik van de gegevens|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Delen door het bereik van de gegevens|


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
