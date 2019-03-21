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
ms.date: 01/08/2019
ms.custom: seodec18
ms.openlocfilehash: 79d013e5836555547cbf254bb25c06add0a717e1
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58295382"
---
# <a name="configure-automated-machine-learning-experiments"></a>Geautomatiseerde machine learning-experimenten configureren

Geautomatiseerde machine learning, kiest een algoritme en hyperparameters voor u en genereert een model dat gereed is voor implementatie. Er zijn diverse opties, kunt u geautomatiseerde machine learning-experimenten configureren. In deze handleiding leert u hoe u diverse configuratie-instellingen te definiëren.

Voor voorbeelden van een geautomatiseerde voor machine learning gebruikt, raadpleegt u [zelfstudie: Een model classificatie met geautomatiseerde machine learning te trainen](tutorial-auto-train-models.md) of [trainen van modellen met geautomatiseerde machine learning in de cloud](how-to-auto-train-remote.md).

Configuratie-opties zijn beschikbaar in geautomatiseerde machine learning:

* Selecteer het type experiment: Classificatie, regressie of prognoses
* De gegevensbron, indelingen en ophalen van gegevens
* Kies uw compute-doel: lokale of externe
* Geautomatiseerde machine learning-experiment-instellingen
* Een geautomatiseerde machine learning-experiment uitvoeren
* Model metrische gegevens verkennen
* Registreer en implementeer model

## <a name="select-your-experiment-type"></a>Selecteer het type experiment
Voordat u uw experiment, moet u het type van machine learning probleem, u het oplossen van bepalen. Geautomatiseerde machine learning ondersteunt taaktypen classificatie, regressie en prognose.

Geautomatiseerde machine learning-mogelijkheden zijn algemeen beschikbaar, **prognoses is nog steeds in openbare preview.**

Geautomatiseerde machine learning ondersteunt de volgende algoritmen tijdens de automatisering en het afstemmen van proces. Als een gebruiker is er niet nodig voor u het algoritme opgeven.

Classificatie | Regressie | Prognose
|-- |-- |--
[Logistic Regression](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)| [Elastische Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)| [Elastische Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)
[De toolkit leren met stochastische Gradiëntdaling (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)|[Licht GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Licht GBM](https://lightgbm.readthedocs.io/en/latest/index.html)
[Naive Bayes](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)|[Versterking van kleurovergang](https://scikit-learn.org/stable/modules/ensemble.html#regression)|[Versterking van kleurovergang](https://scikit-learn.org/stable/modules/ensemble.html#regression)
[C-Support Vector classificatie (SVC)](https://scikit-learn.org/stable/modules/svm.html#classification)|[Beslissingsstructuur](https://scikit-learn.org/stable/modules/tree.html#regression)|[Beslissingsstructuur](https://scikit-learn.org/stable/modules/tree.html#regression)
[Lineaire SVC](https://scikit-learn.org/stable/modules/svm.html#classification)|[K dichtstbijzijnde Neighbors](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K dichtstbijzijnde Neighbors](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)
[K dichtstbijzijnde Neighbors](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors)|[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)|[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)
[Beslissingsstructuur](https://scikit-learn.org/stable/modules/tree.html#decision-trees)|[De toolkit leren met stochastische Gradiëntdaling (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)|[De toolkit leren met stochastische Gradiëntdaling (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)
[Willekeurige Forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Willekeurige Forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Willekeurige Forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)
[Zeer willekeurige structuren](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Zeer willekeurige structuren](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Zeer willekeurige structuren](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)
[Versterking van kleurovergang](https://scikit-learn.org/stable/modules/ensemble.html#classification)|
[Licht GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|


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
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"')
    # get integer labels
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

Sleutel | Type |    Sluiten elkaar wederzijds uit met | Description
---|---|---|---
X | Pandas Dataframe of Numpy matrix | data_train, label, kolommen |  Alle functies te trainen met
Y | Pandas Dataframe of Numpy matrix |   label   | Voeg een label te trainen met gegevens. Voor de classificatie moet een matrix van gehele getallen zijn.
X_valid | Pandas Dataframe of Numpy matrix   | data_train, label | _Optionele_ alle functies waarmee moet worden gevalideerd. Indien niet opgegeven, X wordt verdeeld tussen train en valideren
y_valid |   Pandas Dataframe of Numpy matrix | data_train, label | _Optionele_ de labelgegevens te valideren met. Indien niet opgegeven, y wordt verdeeld tussen train en valideren
sample_weight | Pandas Dataframe of Numpy matrix |   data_train, label, kolommen| _Optionele_ een gewicht voor elk voorbeeld. Gebruik deze sjabloon wanneer u wilt toewijzen van verschillende gewichten voor uw data-punten
sample_weight_valid | Pandas Dataframe of Numpy matrix | data_train, label, kolommen |    _Optionele_ een gewicht voor elk Validatievoorbeeld. Indien niet opgegeven, sample_weight wordt verdeeld tussen train en valideren
data_train |    Pandas Dataframe |  X, y, X_valid, y_valid |    Alle gegevens (functies en label) te trainen met
label | string  | X, y, X_valid, y_valid |  Welke kolom in data_train Hiermee geeft u het label
Kolommen | matrix van tekenreeksen  ||  _Optionele_ Whitelist van kolommen moet worden gebruikt voor functies
cv_splits_indices   | Matrix van gehele getallen ||  _Optionele_ lijst van indexen in het splitsen van de gegevens voor cross-validatie

### <a name="load-and-prepare-data-using-dataprep-sdk"></a>Laden en voorbereiden van gegevens met behulp van DataPrep-SDK
Geautomatiseerde experimenten voor machine learning biedt ondersteuning voor het laden van gegevens en transformaties met behulp van de dataprep-SDK. Met de SDK biedt de mogelijkheid om

>* Laden uit veel bestandstypen met het parseren van de parameter Deductie (codering, scheidingsteken, headers)
>* Type converteren met behulp van Deductie tijdens het laden van bestand
>* Ondersteuning voor MS SQL Server en Azure Data Lake Storage-verbindingen
>* Met behulp van een expressie kolom toevoegen
>* Ontbrekende waarden worden toegerekend
>* Kolom afleiden per voorbeeld
>* Filteren
>* Aangepaste Python-transformaties

Voor meer informatie over de gegevens prep sdk verwijzen de [over het voorbereiden van gegevens voor het modelleren van artikel](how-to-load-data.md).
Hieronder volgt een voorbeeld van het laden van gegevens met behulp van data prep sdk.
```python
# The data referenced here was pulled from `sklearn.datasets.load_digits()`.
simple_example_data_root = 'https://dprepdata.blob.core.windows.net/automl-notebook-data/'
X = dprep.auto_read_file(simple_example_data_root + 'X.csv').skip(1)  # Remove the header row.
# You can use `auto_read_file` which intelligently figures out delimiters and datatypes of a file.

# Here we read a comma delimited file and convert all columns to integers.
y = dprep.read_csv(simple_example_data_root + 'y.csv').to_long(dprep.ColumnSelector(term='.*', use_regex = True))
```

## <a name="train-and-validation-data"></a>Train en validatie

Kunt u afzonderlijke train en validatie ingesteld via get_data() of rechtstreeks in de `AutoMLConfig` methode.

## <a name="cross-validation-split-options"></a>Opties voor cross-validatie splitsen

### <a name="k-folds-cross-validation"></a>K-vouwen Kruisvalidatie

Gebruik `n_cross_validations` instelling om op te geven van het aantal cross-bevestigingen. De set trainingsgegevens wordt willekeurig worden gesplitst in `n_cross_validations` vouwen van gelijke grootte. Tijdens elke cross validatie ronde, een van de vouwen dat wordt gebruikt voor de validatie van het model is getraind op de resterende vouwen. Dit proces wordt herhaald voor `n_cross_validations` totdat elke vouwen één keer wordt gebruikt als validatie afgerond. De gemiddelde scores voor alle `n_cross_validations` Rondt worden gerapporteerd, en de bijbehorende model zal opnieuw worden getraind op de gehele training gegevensset.

### <a name="monte-carlo-cross-validation-aka-repeated-random-sub-sampling"></a>Monte Carlo Kruisvalidatie (ook wel) Onderliggende steekproeven herhaald)

Gebruik `validation_size` om op te geven van het percentage van de training-gegevensset die moet worden gebruikt voor validatie en gebruik `n_cross_validations` om op te geven van het aantal cross-bevestigingen. Tijdens elke cross-validatie afgerond, een subset van de grootte van `validation_size` willekeurig worden geselecteerd voor de validatie van het model is getraind op de resterende gegevens. Ten slotte het gemiddelde voor alle beoordeelt `n_cross_validations` Rondt worden gerapporteerd, en de bijbehorende model zal opnieuw worden getraind op de gehele training gegevensset.

### <a name="custom-validation-dataset"></a>Aangepaste validatie-gegevensset

Gebruik aangepaste validatie-gegevensset als willekeurige splitsen niet geaccepteerd wordt (meestal time series-gegevens of imbalanced gegevens). U kunt uw eigen gegevensset validatie opgeven. Het model wordt geëvalueerd op basis van de gegevensset validatie is opgegeven in plaats van willekeurige gegevensset.

## <a name="compute-to-run-experiment"></a>COMPUTE experiment uitvoeren

Vervolgens kunt u bepalen waar u het model wordt getraind. Een geautomatiseerde machine learning-trainingsexperiment kunt uitvoeren op de volgende compute-opties:
*   Uw lokale machine, zoals een lokale desktop of laptop – algemeen wanneer u kleine gegevensset hebt en u bent nog steeds in de fase verkennen.
*   Een externe computer in de cloud, [Azure Machine Learning-beheerd Compute](concept-azure-machine-learning-architecture.md#managed-and-unmanaged-compute-targets) is een beheerde service die het mogelijk om te trainen op clusters virtuele machines van Azure machine learning-modellen.

Zie de [GitHub-site](https://github.com/Azure/MachineLearningNotebooks/tree/master/automl) bijvoorbeeld-notebooks met lokale en externe compute-doelen.

<a name='configure-experiment'></a>

## <a name="configure-your-experiment-settings"></a>Uw experiment-instellingen configureren

Er zijn diverse opties, kunt u uw geautomatiseerde machine learning-experiment configureren. Deze parameters zijn ingesteld door het instantiëren van een `AutoMLConfig` object.

Voorbeelden zijn:

1.  Classificatie-experiment met behulp van AUC gewogen als de primaire metrische gegevens met een maximale tijd van 12.000 seconden per herhaling, met het experiment te beëindigen nadat u hebt 50 iteraties en 2 cross validatie vouwen.

    ```python
    automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        max_time_sec=12000,
        iterations=50,
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
        primary_metric='r2_score',
        X=X,
        y=y,
        n_cross_validations=5)
    ```

Er zijn drie verschillende `task` parameterwaarden die bepalen van de lijst met algoritmen om toe te passen.  Gebruik de `whitelist` of `blacklist` parameters voor het verder aanpassen iteraties met de beschikbare algoritmen wilt opnemen of uitsluiten.
* Classificatie
    * LogisticRegression
    * SGD
    * MultinomialNaiveBayes
    * BernoulliNaiveBayes
    * SVM
    * LinearSVM
    * KNN
    * DecisionTree
    * RandomForest
    * ExtremeRandomTrees
    * LightGBM
    * GradientBoosting
    * TensorFlowDNN
    * TensorFlowLinearClassifier
* Regressie
    * ElasticNet
    * GradientBoosting
    * DecisionTree
    * KNN
    * LassoLars
    * SGD 
    * RandomForest
    * ExtremeRandomTree
    * LightGBM
    * TensorFlowLinearRegressor
    * TensorFlowDNN
* Prognose
    * ElasticNet
    * GradientBoosting
    * DecisionTree
    * KNN
    * LassoLars
    * SGD 
    * RandomForest
    * ExtremeRandomTree
    * LightGBM
    * TensorFlowLinearRegressor
    * TensorFlowDNN

Zie de [AutoMLConfig klasse](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig.automlconfig?view=azure-ml-py) voor een volledige lijst met parameters.  

## <a name="data-pre-processing-and-featurization"></a>Gegevens vooraf verwerken en parametrisatie

Als u `preprocess=True`, de volgende gegevens voorverwerkingsstappen automatisch voor u worden uitgevoerd:
1.  Verwijder hoge kardinaliteit of er zijn geen functies afwijking
    * Functies met geen nuttige informatie niet verwijderen uit de trainings- en validatie sets. Deze omvatten functies met alle waarden ontbreken, dezelfde waarde in alle tabelrijen of met zeer hoge kardinaliteit (zoals hashes, id's of GUID's).
1.  Ontbrekende waarde toerekening
    *   Voor numerieke functies, rekenen ontbrekende waarden met gemiddelde van waarden in de kolom.
    *   Rekenen ontbrekende waarden met de meest voorkomende waarde voor de categorische functies.
1.  Extra functies genereren
    * Voor datum/tijd-functies: Jaar, maand, dag, dag van week, dag van jaar, kwartaal, de Week van het jaar, uur, minuut, seconde.
    * Voor tekst-functies: De frequentie van de termijn is op basis van word unigram, bi-g en drie-gram, aantal vectorizer.
1.  Transformaties en coderingen
    * Numerieke functies met heel weinig unieke waarden die worden omgezet in categorische functies.
    * Afhankelijk van de kardinaliteit van categorische functies uitvoeren label coderings- of -(hashing) een hot-codering.

## <a name="run-experiment"></a>Experiment uit te voeren

Het experiment uitvoeren en het genereren van een model verzenden. Geeft de `AutoMLConfig` naar de `submit` methode voor het genereren van het model.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Afhankelijkheden zijn geïnstalleerd op een nieuwe virtuele machine.  Het kan maximaal 10 minuten duren voordat uitvoer wordt weergegeven.
>Instellen van `show_output` naar `True` resulteert in de uitvoer wordt weergegeven op de console.


## <a name="explore-model-metrics"></a>Model metrische gegevens verkennen
U kunt uw resultaten weergeven in een widget of een inline als u zich in een notitieblok. Zie [bijhouden en evalueren van modellen](how-to-track-experiments.md#view-run-details) voor meer informatie.


### <a name="classification-metrics"></a>Classificatie metrische gegevens
De volgende metrische gegevens worden opgeslagen in elke iteratie van een classificatietaak.

|Primaire metrische gegevens|Description|Berekening|Extra Parameters
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
norm_macro_recall|Genormaliseerde Macro intrekken is Macro intrekken genormaliseerd zodat willekeurige prestaties een score van 0 krijgt en perfecte prestaties een score van 1 krijgt. Dit wordt bereikt door norm_macro_recall: (recall_score_macro - R) = /(1-R), waarbij R de verwachte waarde van recall_score_macro voor willekeurige voorspellingen (dat wil zeggen, R = 0,5 voor binaire classificatie) en R=(1/C) voor C-klasse classificatie problemen|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|gemiddelde = "macro" en vervolgens (recall_score_macro - R) /(1-R), waarbij R de verwachte waarde van recall_score_macro voor willekeurige voorspellingen (dat wil zeggen, R = 0,5 voor binaire classificatie) en R=(1/C) voor C-klasse classificatie problemen|
precision_score_macro|De precisie is het percentage van de elementen die worden aangeduid als een bepaalde klasse die daadwerkelijk in die klasse. Macro is het rekenkundige gemiddelde van precisie voor elke objectklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|gemiddelde = "macro"|
precision_score_micro|De precisie is het percentage van de elementen die worden aangeduid als een bepaalde klasse die daadwerkelijk in die klasse. Micro wordt wereldwijd berekend door het tellen van de totale echt positieven en fout-positieven|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|gemiddelde = "micro"|
precision_score_weighted|De precisie is het percentage van de elementen die worden aangeduid als een bepaalde klasse die daadwerkelijk in die klasse. Gewogen is het rekenkundige gemiddelde van precisie voor elke objectklasse, gewogen door het aantal waar elke klasse-instanties|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|gemiddelde = "gewogen"|
recall_score_macro|Intrekken is het percentage van de elementen in een bepaalde klasse daadwerkelijk die goed zijn gelabeld. Macro is het rekenkundige gemiddelde van terugroeping voor elke objectklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "macro"|
recall_score_micro|Intrekken is het percentage van de elementen in een bepaalde klasse daadwerkelijk die goed zijn gelabeld. Micro wordt wereldwijd berekend door het totaal aantal echt positieven, false negatieven tellen|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "micro"|
recall_score_weighted|Intrekken is het percentage van de elementen in een bepaalde klasse daadwerkelijk die goed zijn gelabeld. Gewogen is het rekenkundige gemiddelde van terugroeping voor elke objectklasse, gewogen door het aantal waar elke klasse-instanties|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "gewogen"|
weighted_accuracy|Gewogen nauwkeurigheid is nauwkeurigheid waarin het gewicht gegeven aan elk voorbeeld gelijk is aan het aandeel van true-exemplaren in dat de waarde true voorbeeldklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|sample_weight is gelijk aan het aandeel van die klasse voor elk element in de doel-vector|

### <a name="regression-and-forecasting-metrics"></a>Regressie worden gevolgd en voorspeld metrische gegevens
De volgende metrische gegevens worden opgeslagen in elke iteratie een regressie of prognoses taak.

|Primaire metrische gegevens|Description|Berekening|Extra Parameters
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

## <a name="explain-the-model"></a>Leg uit het model

Geautomatiseerde machine learning-mogelijkheden zijn algemeen beschikbaar, **het model explainability-functie is nog steeds in openbare preview.**

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

```python
from azureml.widgets import RunDetails
RunDetails(local_run).show()
```
![functie belang graph](./media/how-to-configure-auto-train/feature-importance.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [hoe en waar u kunt een model implementeren](how-to-deploy-and-where.md).

Meer informatie over [hoe u een regressiemodel met automatische machine learning te trainen](tutorial-auto-train-models.md) of [hoe u met behulp van de trein geautomatiseerde machine learning in een externe bron](how-to-auto-train-remote.md).
