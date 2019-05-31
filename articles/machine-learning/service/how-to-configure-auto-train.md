---
title: Geautomatiseerde ML-experimenten maken
titleSuffix: Azure Machine Learning service
description: Geautomatiseerde machine learning, kiest een algoritme voor u en genereert een model dat gereed is voor implementatie. Meer informatie over de opties die u kunt met geautomatiseerde machine learning-experimenten configureren.
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: c0f8a56df5b41236256115ced0d46a87c5ee91a5
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66400233"
---
# <a name="configure-automated-ml-experiments-in-python"></a>Geautomatiseerde ML experimenten in Python configureren

In deze handleiding informatie over het definiëren van verschillende configuratie-instellingen van uw geautomatiseerde voor machine learning gebruikt met de [Azure Machine Learning-SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py). Geautomatiseerde machine learning, kiest een algoritme en hyperparameters voor u en genereert een model dat gereed is voor implementatie. Er zijn diverse opties, kunt u geautomatiseerde machine learning-experimenten configureren.

Voor voorbeelden van een geautomatiseerde voor machine learning gebruikt, raadpleegt u [zelfstudie: Een model classificatie met geautomatiseerde machine learning te trainen](tutorial-auto-train-models.md) of [trainen van modellen met geautomatiseerde machine learning in de cloud](how-to-auto-train-remote.md).

Configuratie-opties zijn beschikbaar in geautomatiseerde machine learning:

* Selecteer het type experiment: Classificatie, regressie of Timeseries-prognoses
* De gegevensbron, indelingen en ophalen van gegevens
* Kies uw compute-doel: lokale of externe
* Geautomatiseerde machine learning-experiment-instellingen
* Een geautomatiseerde machine learning-experiment uitvoeren
* Model metrische gegevens verkennen
* Registreer en implementeer model

Als u liever een geen code-ervaring, kunt u ook [uw geautomatiseerde experimenten voor machine learning in Azure portal maken](how-to-create-portal-experiments.md).

## <a name="select-your-experiment-type"></a>Selecteer het type experiment

Voordat u uw experiment, moet u het type van machine learning probleem, u het oplossen van bepalen. Geautomatiseerde machine learning ondersteunt taaktypen classificatie, regressie en prognose.

Geautomatiseerde machine learning ondersteunt de volgende algoritmen tijdens de automatisering en het afstemmen van proces. Als een gebruiker is er niet nodig voor u het algoritme opgeven. DNN algoritmen zijn beschikbaar tijdens de training, stel geautomatiseerde ML DNN modellen niet samen.

Classificatie | Regressie | Time Series-prognoses
|-- |-- |--
[Logistic Regression](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)| [Elastische Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)| [Elastische Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)
[Licht GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Licht GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Licht GBM](https://lightgbm.readthedocs.io/en/latest/index.html)
[Versterking van kleurovergang](https://scikit-learn.org/stable/modules/ensemble.html#classification)|[Versterking van kleurovergang](https://scikit-learn.org/stable/modules/ensemble.html#regression)|[Versterking van kleurovergang](https://scikit-learn.org/stable/modules/ensemble.html#regression)
[Beslissingsstructuur](https://scikit-learn.org/stable/modules/tree.html#decision-trees)|[Beslissingsstructuur](https://scikit-learn.org/stable/modules/tree.html#regression)|[Beslissingsstructuur](https://scikit-learn.org/stable/modules/tree.html#regression)
[K dichtstbijzijnde Neighbors](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K dichtstbijzijnde Neighbors](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K dichtstbijzijnde Neighbors](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)
[Lineaire SVC](https://scikit-learn.org/stable/modules/svm.html#classification)|[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)|[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)
[C-Support Vector classificatie (SVC)](https://scikit-learn.org/stable/modules/svm.html#classification)|[De toolkit leren met stochastische Gradiëntdaling (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)|[De toolkit leren met stochastische Gradiëntdaling (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)
[Willekeurige Forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Willekeurige Forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Willekeurige Forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)
[Zeer willekeurige structuren](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Zeer willekeurige structuren](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Zeer willekeurige structuren](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)
[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)|[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)| [Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)
[DNN classificatie](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNClassifier)|[DNN regressor zijn](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor) | [DNN regressor zijn](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor)|
[DNN lineaire classificatie](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearClassifier)|[Lineaire regressor zijn](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)|[Lineaire regressor zijn](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)
[Naive Bayes](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)|
[De toolkit leren met stochastische Gradiëntdaling (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)|

Gebruik de `task` parameter in de `AutoMLConfig` constructor om op te geven van het type van uw experiment.

```python
from azureml.train.automl import AutoMLConfig

# task can be one of classification, regression, forecasting
automl_config = AutoMLConfig(task="classification")
```

## <a name="data-source-and-format"></a>Gegevensbron en indeling
Geautomatiseerde machine learning biedt ondersteuning voor gegevens die zich bevinden op het lokale bureaublad of in de cloud zoals Azure Blob Storage. De gegevens kunnen worden gelezen in scikit-informatie over ondersteunde gegevensindelingen. U kunt de gegevens in lezen:
* Numpy matrices X (kenmerken)- en y (doelvariabele of ook wel bekend als label)
* Pandas dataframe

Voorbeelden:

*   Numpy matrices

    ```python
    digits = datasets.load_digits()
    X_digits = digits.data
    y_digits = digits.target
    ```

*   Pandas dataframe

    ```python
    import pandas as pd
    from sklearn.model_selection import train_test_split
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"')
    # get integer labels
    y = df["Label"]
    df = df.drop(["Label"], axis=1)
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Ophalen van gegevens voor het experiment uitvoeren op externe compute

Als u van een externe compute gebruikmaakt om uit te voeren van uw experiment, het ophalen van gegevens moet worden verpakt in een afzonderlijke python-script `get_data()`. Met dit script wordt uitgevoerd op de externe compute waar de geautomatiseerde machine learning-experiment wordt uitgevoerd. `get_data` Elimineert de noodzaak voor het ophalen van de gegevens via de kabel voor elke iteratie. Zonder `get_data`, uw experiment mislukken wanneer u op de externe compute uitvoert.

Hier volgt een voorbeeld van `get_data`:

```python
%%writefile $project_folder/get_data.py
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder
def get_data(): # Burning man 2016 data
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"')
    # get integer labels
    le = LabelEncoder()
    le.fit(df["Label"].values)
    y = le.transform(df["Label"].values)
    df = df.drop(["Label"], axis=1)
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    return { "X" : df, "y" : y }
```

In uw `AutoMLConfig` -object, geeft u de `data_script` parameter en geef het pad naar de `get_data` scriptbestand vergelijkbaar met hieronder:

```python
automl_config = AutoMLConfig(****, data_script=project_folder + "/get_data.py", **** )
```

`get_data` script kan retourneren:

Sleutel | Type | Sluiten elkaar wederzijds uit met    | Description
---|---|---|---
X | Pandas Dataframe of Numpy matrix | data_train, label, kolommen |  Alle functies te trainen met
Y | Pandas Dataframe of Numpy matrix |   label   | Voeg een label te trainen met gegevens. Voor de classificatie moet een matrix van gehele getallen zijn.
X_valid | Pandas Dataframe of Numpy matrix   | data_train, label | _Optionele_ zijn uitgerust met gegevens die de set validatie vormen. Indien niet opgegeven, X wordt verdeeld tussen train en valideren
y_valid |   Pandas Dataframe of Numpy matrix | data_train, label | _Optionele_ de labelgegevens te valideren met. Indien niet opgegeven, y wordt verdeeld tussen train en valideren
sample_weight | Pandas Dataframe of Numpy matrix |   data_train, label, kolommen| _Optionele_ een gewicht voor elk voorbeeld. Gebruik deze sjabloon wanneer u wilt toewijzen van verschillende gewichten voor uw data-punten
sample_weight_valid | Pandas Dataframe of Numpy matrix | data_train, label, kolommen |    _Optionele_ een gewicht voor elk Validatievoorbeeld. Indien niet opgegeven, sample_weight wordt verdeeld tussen train en valideren
data_train |    Pandas Dataframe |  X, y, X_valid, y_valid |    Alle gegevens (functies en label) te trainen met
label | string  | X, y, X_valid, y_valid |  Welke kolom in data_train Hiermee geeft u het label
Kolommen | matrix van tekenreeksen  ||  _Optionele_ Whitelist van kolommen moet worden gebruikt voor functies
cv_splits_indices   | Matrix van gehele getallen ||  _Optionele_ lijst van indexen in het splitsen van de gegevens voor cross-validatie

## <a name="train-and-validation-data"></a>Train en validatie

Kunt u afzonderlijke train en validatie ingesteld via get_data() of rechtstreeks in de `AutoMLConfig` methode.

### <a name="k-folds-cross-validation"></a>K-vouwen Kruisvalidatie

Gebruik `n_cross_validations` instelling om op te geven van het aantal cross-bevestigingen. De set trainingsgegevens wordt willekeurig worden gesplitst in `n_cross_validations` vouwen van gelijke grootte. Tijdens elke cross validatie ronde, een van de vouwen dat wordt gebruikt voor de validatie van het model is getraind op de resterende vouwen. Dit proces wordt herhaald voor `n_cross_validations` totdat elke vouwen één keer wordt gebruikt als validatie afgerond. De gemiddelde scores voor alle `n_cross_validations` Rondt worden gerapporteerd, en de bijbehorende model zal opnieuw worden getraind op de gehele training gegevensset.

### <a name="monte-carlo-cross-validation-repeated-random-sub-sampling"></a>Validatie van Monte Carlo Cross (herhaalde onderliggende steekproeven)

Gebruik `validation_size` om op te geven van het percentage van de training-gegevensset die moet worden gebruikt voor validatie en gebruik `n_cross_validations` om op te geven van het aantal cross-bevestigingen. Tijdens elke cross-validatie afgerond, een subset van de grootte van `validation_size` willekeurig worden geselecteerd voor de validatie van het model is getraind op de resterende gegevens. Ten slotte het gemiddelde voor alle beoordeelt `n_cross_validations` Rondt worden gerapporteerd, en de bijbehorende model zal opnieuw worden getraind op de gehele training gegevensset. Monte Carlo wordt niet ondersteund voor time series-prognoses.

### <a name="custom-validation-dataset"></a>Aangepaste validatie-gegevensset

Aangepaste validatie-gegevensset als willekeurige splitsen niet geaccepteerd wordt, meestal time series-gegevens of imbalanced gegevens gebruiken. U kunt uw eigen gegevensset validatie opgeven. Het model wordt geëvalueerd op basis van de gegevensset validatie is opgegeven in plaats van willekeurige gegevensset.

## <a name="compute-to-run-experiment"></a>COMPUTE experiment uitvoeren

Vervolgens kunt u bepalen waar u het model wordt getraind. Een geautomatiseerde machine learning-trainingsexperiment kunt uitvoeren op de volgende compute-opties:
*   Uw lokale machine, zoals een lokale desktop of laptop – algemeen wanneer u kleine gegevensset hebt en u bent nog steeds in de fase verkennen.
*   Een externe computer in de cloud, [Azure Machine Learning-beheerd Compute](concept-azure-machine-learning-architecture.md#managed-and-unmanaged-compute-targets) is een beheerde service die het mogelijk om te trainen op clusters virtuele machines van Azure machine learning-modellen.

Zie de [GitHub-site](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning) bijvoorbeeld-notebooks met lokale en externe compute-doelen.

*   Een Azure Databricks-cluster in uw Azure-abonnement. U vindt meer informatie hier: [Setup Azure Databricks-cluster voor geautomatiseerde ML](how-to-configure-environment.md#azure-databricks)

Zie de [GitHub-site](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/azure-databricks/automl) bijvoorbeeld-notebooks met Azure Databricks.

<a name='configure-experiment'></a>

## <a name="configure-your-experiment-settings"></a>Uw experiment-instellingen configureren

Er zijn diverse opties, kunt u uw geautomatiseerde machine learning-experiment configureren. Deze parameters zijn ingesteld door het instantiëren van een `AutoMLConfig` object. Zie de [AutoMLConfig klasse](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py) voor een volledige lijst met parameters.

Voorbeelden zijn:

1.  Classificatie-experiment met behulp van AUC gewogen als de primaire metrische gegevens met een maximale tijd van 12.000 seconden per herhaling, met het experiment te beëindigen nadat u hebt 50 iteraties en 2 cross validatie vouwen.

    ```python
    automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        max_time_sec=12000,
        iterations=50,
        blacklist_models='XGBoostClassifier',
        X=X,
        y=y,
        n_cross_validations=2)
    ```
2.  Hieronder volgt een voorbeeld van een set regressie-experiment na 100 herhalingen, elke herhaling duurt maximaal 600 seconden met 5 validatie kruislings vouwen.

    ```python
    automl_regressor = AutoMLConfig(
        task='regression',
        max_time_sec=600,
        iterations=100,
        whitelist_models='kNN regressor'
        primary_metric='r2_score',
        X=X,
        y=y,
        n_cross_validations=5)
    ```

De drie verschillende `task` parameterwaarden bepalen de lijst met algoritmen om toe te passen.  Gebruik de `whitelist` of `blacklist` parameters voor het verder aanpassen iteraties met de beschikbare algoritmen wilt opnemen of uitsluiten. De lijst met ondersteunde modellen kan worden gevonden op [SupportedAlgorithms klasse](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.constants.supportedalgorithms?view=azure-ml-py).

### <a name="primary-metric"></a>Primaire metrische gegevens
De primaire metric; zoals wordt weergegeven in de bovenstaande voorbeelden bepaalt de metrische gegevens moet worden gebruikt tijdens het trainen van het model voor optimalisatie. De primaire metrische gegevens die u kunt selecteren, wordt bepaald door het taaktype dat u kiest. Hieronder vindt u een lijst met beschikbare metrische gegevens.

|Classificatie | Regressie | Time Series-prognoses
|-- |-- |--
|accuracy| spearman_correlation | spearman_correlation
|AUC_weighted | normalized_root_mean_squared_error | normalized_root_mean_squared_error
|average_precision_score_weighted | r2_score | r2_score
|norm_macro_recall | normalized_mean_absolute_error | normalized_mean_absolute_error
|precision_score_weighted |

### <a name="data-preprocessing--featurization"></a>Gegevens voorverwerken & parametrisatie

In elke geautomatiseerde machine learning-experiment, uw gegevens is [automatisch wordt geschaald en genormaliseerd](concept-automated-ml.md#preprocess) om u te helpen algoritmen presteren.  U kunt echter ook aanvullende voorverwerking/parametrisatie, zoals ontbrekende waarden toerekening, codering en -transformaties inschakelen. [Meer informatie over welke parametrisatie opgenomen is](how-to-create-portal-experiments.md#preprocess).

Geef zodat deze parametrisatie `"preprocess": True` voor de [ `AutoMLConfig` klasse](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py).

### <a name="time-series-forecasting"></a>Time Series-prognoses
Voor time series prognoses taaktype hebt u aanvullende parameters te definiëren.
1. time_column_name - dit is een vereiste parameter waarin de naam van de kolom zijn gedefinieerd in uw trainingen met datum/tijd gegevensreeks.
1. max_horizon - Hiermee definieert u de hoeveelheid tijd die u wilt om te voorspellen op basis van de periodiciteit van de trainingsgegevens. Zo hebt u trainingsgegevens met dagelijkse tijd korrels, definieert u hoe ver uit in de dagen die u wilt dat het model te trainen voor.
1. grain_column_names - Hiermee definieert u de naam van de kolommen die afzonderlijke time series-gegevens in uw trainingsgegevens bevatten. Als u bent verkoopprognoses van een bepaalde merk door store, zou u kolommen store en merk definiëren als uw kolommen tijdsinterval.

Zie het voorbeeld van deze instellingen hieronder wordt gebruikt, laptop-voorbeeld is beschikbaar [hier](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb).

```python
# Setting Store and Brand as grains for training.
grain_column_names = ['Store', 'Brand']
nseries = data.groupby(grain_column_names).ngroups

# View the number of time series data with defined grains
print('Data contains {0} individual time-series.'.format(nseries))
```

```python
time_series_settings = {
    'time_column_name': time_column_name,
    'grain_column_names': grain_column_names,
    'drop_column_names': ['logQuantity'],
    'max_horizon': n_test_periods
}

automl_config = AutoMLConfig(task='forecasting',
                             debug_log='automl_oj_sales_errors.log',
                             primary_metric='normalized_root_mean_squared_error',
                             iterations=10,
                             X=X_train,
                             y=y_train,
                             n_cross_validations=5,
                             path=project_folder,
                             verbosity=logging.INFO,
                             **time_series_settings)
```

## <a name="run-experiment"></a>Experiment uit te voeren

Het experiment uitvoeren en het genereren van een model verzenden. Geeft de `AutoMLConfig` naar de `submit` methode voor het genereren van het model.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Afhankelijkheden zijn geïnstalleerd op een nieuwe virtuele machine.  Het kan maximaal 10 minuten duren voordat uitvoer wordt weergegeven.
>Instellen van `show_output` naar `True` resulteert in de uitvoer wordt weergegeven op de console.

### <a name="exit-criteria"></a>Afsluitcriteria
Er enkele opties kunt u definiëren voor het voltooien van uw experiment.
1. Er zijn geen Criteria - als u niet dat een definieert afsluiten parameters die het experiment wordt voortgezet totdat er geen verdere voortgang is gemaakt op uw primaire metrische gegevens.
1. Nummer van iteraties - definieert u het aantal iteraties voor het experiment om uit te voeren. U kunt optioneel iteration_timeout_minutes voor het definiëren van een tijdslimiet in minuten per elke herhaling toevoegen.
1. Afgesloten na een tijdsduur - experiment_timeout_minutes gebruiken in de instellingen die u kunt opgeven hoe lang in minuten moet een experiment blijven uitvoeren.
1. Sluit af nadat een score is bereikt - met behulp van experiment_exit_score die kunt u het experiment uitvoeren nadat een score op basis van uw primaire meetwaarde is bereikt.

### <a name="explore-model-metrics"></a>Model metrische gegevens verkennen

U kunt de resultaten van uw training in een widget of een inline bekijken als u zich in een notitieblok. Zie [bijhouden en evalueren van modellen](how-to-track-experiments.md#view-run-details) voor meer informatie.

## <a name="understand-automated-ml-models"></a>Informatie over geautomatiseerde ML-modellen

Een model gemaakt met behulp van geautomatiseerde ML bevat de volgende stappen uit:
+ Geautomatiseerde feature-engineering (als voorverwerken = True)
+ Schaal/normalisering en het algoritme met hypermeter waarden

We maken het transparant voor deze informatie ophalen uit de uitvoer fitted_model van geautomatiseerde ML.

```python
automl_config = AutoMLConfig(…)
automl_run = experiment.submit(automl_config …)
best_run, fitted_model = automl_run.get_output()
```

### <a name="automated-feature-engineering"></a>Geautomatiseerde feature-engineering

Zie de lijst met vooraf en [geautomatiseerde feature-engineering](concept-automated-ml.md#preprocess) in dat geval wanneer voorverwerken = True.

Bekijk het volgende voorbeeld:
+ Er zijn 4 invoerfuncties: A (Numeric), B (Numeric), C (Numeric), D (DateTime)
+ Numerieke functie C is verwijderd omdat deze geen ID-kolom met alleen unieke waarden
+ Numerieke functies A en B ontbrekende waarden en daarom worden toegerekende door gemiddelde
+ Datum/tijd-functie D is boommodel in 11 verschillende social engineering-functies

Gebruik deze 2 API's op de eerste stap van gemonteerd model om meer inzicht in.  Zie [dit voorbeeldnotitieblok](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand).

+ API 1: `get_engineered_feature_names()` retourneert een lijst van Social engineering onderdeelnamen.

  Gebruik:
  ```python
  fitted_model.named_steps['timeseriestransformer']. get_engineered_feature_names ()
  ```

  ```
  Output: ['A', 'B', 'A_WASNULL', 'B_WASNULL', 'year', 'half', 'quarter', 'month', 'day', 'hour', 'am_pm', 'hour12', 'wday', 'qday', 'week']
  ```

  Deze lijst bevat alle Social engineering onderdeelnamen.

  >[!Note]
  >Gebruik 'timeseriestransformer' voor taak = 'prognoses', anders gebruik 'datatransformer' voor 'regressie' of 'categorie'-taak.

+ API 2: `get_featurization_summary()` parametrisatie samenvatting voor alle functies van de invoer retourneert.

  Gebruik:
  ```python
  fitted_model.named_steps['timeseriestransformer'].get_featurization_summary()
  ```

  >[!Note]
  >Gebruik 'timeseriestransformer' voor taak = 'prognoses', anders gebruik 'datatransformer' voor 'regressie' of 'categorie'-taak.

  Uitvoer:
  ```
  [{'RawFeatureName': 'A',
    'TypeDetected': 'Numeric',
    'Dropped': 'No',
    'EngineeredFeatureCount': 2,
    'Tranformations': ['MeanImputer', 'ImputationMarker']},
   {'RawFeatureName': 'B',
    'TypeDetected': 'Numeric',
    'Dropped': 'No',
    'EngineeredFeatureCount': 2,
    'Tranformations': ['MeanImputer', 'ImputationMarker']},
   {'RawFeatureName': 'C',
    'TypeDetected': 'Numeric',
    'Dropped': 'Yes',
    'EngineeredFeatureCount': 0,
    'Tranformations': []},
   {'RawFeatureName': 'D',
    'TypeDetected': 'DateTime',
    'Dropped': 'No',
    'EngineeredFeatureCount': 11,
    'Tranformations': ['DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime']}]
  ```

   Waar:

   |Uitvoer|Definitie|
   |----|--------|
   |RawFeatureName|De naam van de functie/invoerkolom uit de gegevensset die is opgegeven.|
   |TypeDetected|Gedetecteerde-gegevenstype van de functie voor invoer.|
   |Verwijderd|Hiermee wordt aangegeven als de functie voor invoer is verwijderd of gebruikt.|
   |EngineeringFeatureCount|Het aantal functies die worden gegenereerd door de geautomatiseerde functie engineering transformaties.|
   |Transformaties|Lijst van transformaties toegepast voor het invoeren van functies voor het genereren van Social engineering functies.|

### <a name="scalingnormalization-and-algorithm-with-hypermeter-values"></a>Schaal/normalisering en algoritme met hypermeter waarden:

Voor meer informatie over de schaal/normalisering en algoritme/hyperparameter waarden voor een pijplijn, gebruikt u fitted_model.steps. [Meer informatie over schaling/normalisering](concept-automated-ml.md#preprocess). Hier volgt een voorbeeld van uitvoer:

```
[('RobustScaler', RobustScaler(copy=True, quantile_range=[10, 90], with_centering=True, with_scaling=True)), ('LogisticRegression', LogisticRegression(C=0.18420699693267145, class_weight='balanced', dual=False, fit_intercept=True, intercept_scaling=1, max_iter=100, multi_class='multinomial', n_jobs=1, penalty='l2', random_state=None, solver='newton-cg', tol=0.0001, verbose=0, warm_start=False))
```

Voor meer informatie, gebruikt u deze Help-functie die wordt weergegeven in [dit voorbeeldnotitieblok](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification/auto-ml-classification.ipynb).

```python
from pprint import pprint
def print_model(model, prefix=""):
    for step in model.steps:
        print(prefix + step[0])
        if hasattr(step[1], 'estimators') and hasattr(step[1], 'weights'):
            pprint({'estimators': list(e[0] for e in step[1].estimators), 'weights': step[1].weights})
            print()
            for estimator in step[1].estimators:
                print_model(estimator[1], estimator[0]+ ' - ')
        else:
            pprint(step[1].get_params())
            print()

print_model(fitted_model)
```

Hier volgt een voorbeeld van de uitvoer van een pijplijn met een specifieke algoritme (LogisticRegression met RobustScalar, in dit geval).

```
RobustScaler
{'copy': True,
'quantile_range': [10, 90],
'with_centering': True,
'with_scaling': True}

LogisticRegression
{'C': 0.18420699693267145,
'class_weight': 'balanced',
'dual': False,
'fit_intercept': True,
'intercept_scaling': 1,
'max_iter': 100,
'multi_class': 'multinomial',
'n_jobs': 1,
'penalty': 'l2',
'random_state': None,
'solver': 'newton-cg',
'tol': 0.0001,
'verbose': 0,
'warm_start': False}
```

<a name="explain"></a>

## <a name="explain-the-model-interpretability"></a>Leg uit het model (interpretability)

Geautomatiseerde machine learning kunt u inzicht krijgt in de functie belang.  Tijdens de training krijgt u de algemene functie van belang voor het model.  U kunt ook op klasseniveau functie belang ophalen voor de classificatie-scenario's.  U moet een validatie-gegevensset (X_valid) om op te halen van de functie urgentie opgeven.

Er zijn twee manieren voor het genereren van functie belang.

*   Als een experiment voltooid is, kunt u `explain_model` methode bij elke iteratie.

    ```python
    from azureml.train.automl.automlexplainer import explain_model

    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
        explain_model(fitted_model, X_train, X_test)

    #Overall feature importance
    print(overall_imp)
    print(overall_summary)

    #Class-level feature importance
    print(per_class_imp)
    print(per_class_summary)
    ```

*   Als u wilt bekijken functie belang voor alle iteraties, `model_explainability` markering `True` in AutoMLConfig.

    ```python
    automl_config = AutoMLConfig(task = 'classification',
                                 debug_log = 'automl_errors.log',
                                 primary_metric = 'AUC_weighted',
                                 max_time_sec = 12000,
                                 iterations = 10,
                                 verbosity = logging.INFO,
                                 X = X_train,
                                 y = y_train,
                                 X_valid = X_test,
                                 y_valid = y_test,
                                 model_explainability=True,
                                 path=project_folder)
    ```

    Zodra u klaar bent, kunt u retrieve_model_explanation methode om op te halen van belang van de functie voor een specifieke versie.

    ```python
    from azureml.train.automl.automlexplainer import retrieve_model_explanation

    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
        retrieve_model_explanation(best_run)

    #Overall feature importance
    print(overall_imp)
    print(overall_summary)

    #Class-level feature importance
    print(per_class_imp)
    print(per_class_summary)
    ```

De grafiek voor het belang van functie in uw werkruimte in de Azure-portal, kunt u visualiseren. De grafiek wordt ook weergegeven wanneer u de Jupyter-widget in een notitieblok. Voor meer informatie over de grafieken verwijzen naar de [voorbeeld van Azure Machine Learning-service-laptops artikel.](samples-notebooks.md)

```Python
from azureml.widgets import RunDetails
RunDetails(local_run).show()
```
![functie belang graph](./media/how-to-configure-auto-train/feature-importance.png)

Zie voor meer informatie over hoe model uitleg en urgentie van de functie kunnen worden ingeschakeld in andere gebieden van de SDK buiten automatische leerprocessen, de [concept](machine-learning-interpretability-explainability.md) artikel op interpretability.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [hoe en waar u kunt een model implementeren](how-to-deploy-and-where.md).

Meer informatie over [hoe u een regressiemodel met automatische machine learning te trainen](tutorial-auto-train-models.md) of [hoe u met behulp van de trein geautomatiseerde machine learning in een externe bron](how-to-auto-train-remote.md).
