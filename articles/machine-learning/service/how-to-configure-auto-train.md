---
title: Configureren van uw geautomatiseerde machine learning-experiment - Azure Machine Learning
description: Geautomatiseerde machine learning, kiest een algoritme voor u en genereert een model dat gereed is voor implementatie. Meer informatie over de opties die u kunt met geautomatiseerde machine learning-experimenten configureren.
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: f4a8ff272e498871f4a31ce76487509673f48328
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034243"
---
# <a name="configure-your-automated-machine-learning-experiment"></a>Configureren van uw geautomatiseerde machine learning-experiment

Geautomatiseerde machine learning, kiest een algoritme voor u en genereert een model dat gereed is voor implementatie. Het model kan worden gedownload als u wilt ook verder worden aangepast. Er zijn diverse opties, kunt u geautomatiseerde machine learning-experimenten configureren. In deze handleiding leert u hoe u diverse configuratie-instellingen te definiëren.

Voorbeelden van een geautomatiseerde machine learning, Zie [zelfstudie: een model classificatie automatisch de trein](tutorial-auto-train-models.md) of [trainen van modellen automatisch in de cloud](how-to-auto-train-remote.md).

Configuratie-opties zijn beschikbaar in geautomatiseerde machine learning:

* Selecteer het type experiment, bijvoorbeeld, classificatie, regressie 
* De gegevensbron, indelingen en ophalen van gegevens
* Kies uw compute-doel (lokaal of extern)
* `AutoML` instellingen voor experiment
* Voer `AutoML` experimenteren
* Model metrische gegevens verkennen
* Registreer en implementeer model

## <a name="select-your-experiment-type"></a>Selecteer het type experiment
Voordat u uw experiment, moet u het type van machine learning probleem, u het oplossen van bepalen. Geautomatiseerde machine learning biedt ondersteuning voor twee categorieën van leren met supervisie: classificatie- en Regressiemodellen. Geautomatiseerde machine learning ondersteunt de volgende algoritmen tijdens de automatisering en het afstemmen van proces. Als een gebruiker is er niet nodig voor u het algoritme opgeven.
Classificatie | Regressie
--|--
sklearn.linear_model. LogisticRegression | sklearn.linear_model. ElasticNet
sklearn.linear_model. SGDClassifier  | sklearn.ensemble.GradientBoostingRegressor
sklearn.naive_bayes. BernoulliNB | sklearn.tree.DecisionTreeRegressor
sklearn.naive_bayes. MultinomialNB | sklearn.neighbors.KNeighborsRegressor
sklearn.svm.SVC | sklearn.linear_model. LassoLars
sklearn.svm.LinearSVC | sklearn.linear_model. SGDRegressor
sklearn.calibration.CalibratedClassifierCV |    sklearn.ensemble.RandomForestRegressor
sklearn.neighbors.KNeighborsClassifier |    sklearn.ensemble.ExtraTreesRegressor
sklearn.tree.DecisionTreeClassifier |   lightgbm. LGBMRegressor
sklearn.ensemble.RandomForestClassifier |
sklearn.ensemble.ExtraTreesClassifier   |
sklearn.ensemble.GradientBoostingClassifier |
lightgbm. LGBMClassifier |


## <a name="data-source-and-format-for-automl-experiment"></a>De gegevensbron en de indeling voor `AutoML` experimenteren
`AutoML` biedt ondersteuning voor gegevens die zich bevinden op het lokale bureaublad of in de cloud in Azure Blob Storage. De gegevens kunnen worden gelezen in scikit-informatie over ondersteunde gegevensindelingen. U kunt de gegevens in (1) Numpy matrices X (kenmerken) en y (doelvariabele of ook wel bekend als label) of 2) Pandas dataframe lezen. 

Voorbeelden:

1.  Numpy matrices

    ```python
    digits = datasets.load_digits()
    X_digits = digits.data 
    y_digits = digits.target
    ```

2.  Pandas dataframe

    ```python
    Import pandas as pd
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"') 
    # get integer labels 
    le = LabelEncoder() 
    le.fit(df["Label"].values) 
    y = le.transform(df["Label"].values) 
    df = df.drop(["Label"], axis=1) 
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Ophalen van gegevens voor het experiment uitvoeren op externe compute

Als u van een externe compute gebruikmaakt om uit te voeren van uw experiment AutoML, het ophalen van gegevens moet worden verpakt in een afzonderlijke python-script `GetData()`. Met dit script wordt uitgevoerd op de externe compute waar het experiment AutoML wordt uitgevoerd. `GetData` Elimineert de noodzaak voor het ophalen van de gegevens via de kabel voor elke iteratie. Zonder `GetData`, uw experiment mislukken wanneer u op de externe compute uitvoert.

Hier volgt een voorbeeld van `GetData`:

```python
%%writefile $project_folder/get_data.py 
import pandas as pd 
from sklearn.model_selection 
import train_test_split 
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

In uw `AutoMLConfig` -object, geeft u de `data_script` parameter en geef het pad naar de `GetData` scriptbestand vergelijkbaar met hieronder:

```python
automl_config = AutoMLConfig(****, data_script=project_folder + "./get_data.py", **** )
```

`GetData` script kan retourneren de volgende:
Sleutel | Type |    Sluiten elkaar wederzijds uit met | Beschrijving
---|---|---|---
X | Pandas Dataframe of Numpy matrix | data_train, label, kolommen |  Alle functies te trainen met
Y | Pandas Dataframe of Numpy matrix |   label   | Voeg een label te trainen met gegevens. Voor de classificatie moet een matrix van gehele getallen zijn.
X_valid | Pandas Dataframe of Numpy matrix   | data_train, label | _Optionele_ alle functies waarmee moet worden gevalideerd. Indien niet opgegeven, X wordt verdeeld tussen train en valideren
y_valid |   Pandas Dataframe of Numpy matrix | data_train, label | _Optionele_ de labelgegevens te valideren met. Indien niet opgegeven, y wordt verdeeld tussen train en valideren
sample_weight | Pandas Dataframe of Numpy matrix |   data_train, label, kolommen| _Optionele_ een gewicht voor elk voorbeeld. Gebruik deze sjabloon wanneer u wilt toewijzen van verschillende gewichten voor uw data-punten 
sample_weight_valid | Pandas Dataframe of Numpy matrix | data_train, label, kolommen |    _Optionele_ een gewicht voor elk Validatievoorbeeld. Indien niet opgegeven, sample_weight wordt verdeeld tussen train en valideren
data_train |    Pandas Dataframe |  X, y, X_valid, y_valid |    Alle gegevens (functies en label) te trainen met
label | tekenreeks  | X, y, X_valid, y_valid |  Welke kolom in data_train Hiermee geeft u het label
Kolommen | matrix van tekenreeksen  ||  _Optionele_ Whitelist van kolommen moet worden gebruikt voor functies
cv_splits_indices   | Matrix van gehele getallen ||  _Optionele_ lijst van indexen in het splitsen van de gegevens voor cross-validatie

## <a name="train-and-validation-data"></a>Train en validatie

Kunt u afzonderlijke train en validatie ingesteld via get_data() of rechtstreeks in de `AutoMLConfig` methode.

## <a name="cross-validation-split-options"></a>Opties voor cross-validatie splitsen

### <a name="k-folds-cross-validation"></a>K-vouwen Kruisvalidatie

Gebruik `n_cross_validations` instelling om op te geven van het aantal cross-bevestigingen. De set trainingsgegevens wordt willekeurig worden gesplitst in `n_cross_validations` vouwen van gelijke grootte. Tijdens elke cross validatie ronde, een van de vouwen dat wordt gebruikt voor de validatie van het model is getraind op de resterende vouwen. Dit proces wordt herhaald voor `n_cross_validations` totdat elke vouwen één keer wordt gebruikt als validatie afgerond. Ten slotte het gemiddelde voor alle beoordeelt `n_cross_validations` Rondt worden gerapporteerd, en de bijbehorende model zal opnieuw worden getraind op de gehele training gegevensset.

### <a name="monte-carlo-cross-validation-aka-repeated-random-sub-sampling"></a>Monte Carlo Kruisvalidatie (ook wel) Onderliggende steekproeven herhaald)

Gebruik `validation_size` om op te geven van het percentage van de training-gegevensset die moet worden gebruikt voor validatie en gebruik `n_cross_validations` om op te geven van het aantal cross-bevestigingen. Tijdens elke cross-validatie afgerond, een subset van de grootte van `validation_size` willekeurig worden geselecteerd voor de validatie van het model is getraind op de resterende gegevens. Ten slotte het gemiddelde voor alle beoordeelt `n_cross_validations` Rondt worden gerapporteerd, en de bijbehorende model zal opnieuw worden getraind op de gehele training gegevensset.

### <a name="custom-validation-dataset"></a>Aangepaste validatie-gegevensset

Gebruik aangepaste validatie-gegevensset als willekeurige splitsen niet geaccepteerd wordt (meestal time series-gegevens of imbalanced gegevens). Hiermee kunt u uw eigen gegevensset validatie. Het model wordt geëvalueerd op basis van de gegevensset validatie is opgegeven in plaats van willekeurige gegevensset.

## <a name="compute-to-run-experiment"></a>COMPUTE experiment uitvoeren

Vervolgens kunt u bepalen waar u het model wordt getraind. Er wordt een geautomatiseerde machine learning-trainingsexperiment uitgevoerd op een compute-doel dat u bezit en beheren. 

Ondersteunde COMPUTE-opties zijn:
1.  Uw lokale machine, zoals een lokale desktop of laptop – algemeen wanneer u kleine gegevensset hebt en u bent nog steeds in de fase verkennen.
2.  Een externe computer in de cloud, [Azure Data Science Virtual Machine](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) waarop Linux – u een grote gegevensset hebt en wilt opschalen naar een grote virtuele machine die beschikbaar is in de Azure-Cloud. 
3.  Azure Batch AI-cluster: een beheerd cluster dat u niet meer en werken met AutoML iteraties schalen parallel kunt instellen. 


## <a name="configure-your-experiment-settings"></a>Uw experiment-instellingen configureren

Er zijn enkele knoppen waarmee u kunt uw experiment AutoML configureren. Deze parameters zijn ingesteld door het instantiëren van een `AutoMLConfig` object.

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

    ````python
    automl_regressor = AutoMLConfig(
        task='regression',
        max_time_sec=600,
        iterations=100,
        primary_metric='r2_score',
        X=X, 
        y=y,
        n_cross_validations=5)
    ````

Deze tabel bevat de parameterinstellingen die beschikbaar zijn voor uw experiment en hun standaardwaarden.

Eigenschap |  Beschrijving | Standaardwaarde
--|--|--
`task`  |Geef het type van machine learning-probleem. Toegestane waarden zijn <li>Classificatie</li><li>Regressie</li>    | Geen |
`primary_metric` |Metrische gegevens die u optimaliseren wilt bij het bouwen van uw model. Als u de nauwkeurigheid van de gegevens als de primary_metric opgeeft, geautomatiseerde ML, ziet er bijvoorbeeld als u wilt zoeken van een model met maximale nauwkeurigheid. U kunt slechts één primary_metric per experiment opgeven. Toegestane waarden zijn <br/>**Classificatie**:<br/><li> nauwkeurigheid  </li><li> AUC_weighted</li><li> precision_score_weighted </li><li> balanced_accuracy </li><li> average_precision_score_weighted </li><br/>**Regressie**: <br/><li> normalized_mean_absolute_error </li><li> spearman_correlation </li><li> normalized_root_mean_squared_error </li><li> normalized_root_mean_squared_log_error</li><li> R2_score    </li> | Voor classificatie: nauwkeurigheid <br/>Voor regressie: spearman_correlation <br/> |
`exit_score` |  U kunt een doelwaarde instellen voor uw primary_metric. Nadat een model wordt gevonden die voldoet aan het doel primary_metric, geautomatiseerde ML stopt doorlopen en het experiment wordt beëindigd. Als deze waarde is niet ingesteld (standaard), wordt automatisch ML experiment blijft om uit te voeren van het aantal iteraties opgegeven in de herhalingen. Neemt een double-waarde. Als het doel is nooit bereikt, klikt u vervolgens blijven automatisch ML totdat het aantal iteraties opgegeven in de herhalingen wordt bereikt.|   Geen
`iterations` |Maximum aantal iteraties. Elke herhaling is gelijk aan een trainingstaak die in een pijplijn resulteert. Pijplijn is voorverwerking van gegevens en het model. Als u een model van hoge kwaliteit 250 of meer gebruikt | 100
`Concurrent_iterations`|    Maximumaantal iteraties parallel worden uitgevoerd. Deze instelling werkt alleen voor externe compute.|    1
`max_cores_per_iteration`   | Geeft het aantal kernen op de compute-doel moeten worden gebruikt met het trainen van één pijplijn. Als het algoritme kan gebruikmaken van meerdere kernen, verhoogt dit de prestaties op een computer meerdere kernen. U kunt dit instellen op-1 voor het gebruik van de beschikbare cores op de machine.|  1
`max_time_sec` |    Beperkt de hoeveelheid tijd (seconden) een bepaalde iteratie nodig. Als een iteratie hoger is dan de opgegeven hoeveelheid, wordt die iteratie wordt geannuleerd. Als dat niet ingesteld, worden de iteratie blijft actief totdat deze is voltooid. |   Geen
`n_cross_validations`   |Aantal cross-validatie splitsingen| Geen
`validation_size`   |Grootte van de validatie is ingesteld als percentage van alle training-voorbeeld.|  Geen
`preprocess` | Waar/onwaar <br/>De waarde True schakelt experimenteren om uit te voeren voorverwerking van de invoer. Hieronder volgt een subset van voorverwerking<li>Ontbrekende gegevens: Imputes de ontbrekende gegevens numerieke met gemiddelde, tekst met de meeste exemplaar </li><li>Categorische waarden: Als het gegevenstype en het aantal unieke numerieke waarden minder dan 5 procent, omgezet in een hot-codering is </li><li>Enzovoort voor controle van de volledige lijst [de GitHub-opslagplaats](https://aka.ms/aml-notebooks)</li><br/>Opmerking: als gegevens sparse is u niet gebruiken voorverwerken = true |  False | 
`blacklist_algos`   | Geautomatiseerde ML-experiment heeft veel verschillende algoritmen die er wordt geprobeerd. Configureer Automatische ML als u wilt uitsluiten van bepaalde algoritmen van het experiment. Dit is handig als u zich bewust bent verifiëringsalgoritme (s) werken niet goed voor uw gegevensset. Met uitzondering van de algoritmen kunt bespaart u compute-resources en trainingstijd.<br/>Toegestane waarden voor classificatie<br/><li>Logistieke regressie</li><li>SGD classificatie</li><li>MultinomialNB</li><li>BernoulliNB</li><li>SVM</li><li>LinearSVM</li><li>kNN</li><li>DT</li><li>RF</li><li>extra structuren</li><li>versterking van kleurovergang</li><li>lgbm_classifier</li><br/>Toegestane waarden voor regressie<br/><li>Elastische netto</li><li>Kleurovergang Gradient boosting regressor zijn</li><li>DT regressor zijn</li><li>kNN regressor zijn</li><li>Lasso lars</li><li>SGD regressor zijn</li><li>RF regressor zijn</li><li>extra structuren regressor zijn</li>|   Geen
`verbosity` |Hiermee bepaalt u het niveau van logboekregistratie met gegevens wordt de meest uitgebreide en kritieke die de minste.<br/>Toegestane waarden zijn:<br/><li>logging.INFO</li><li>logboekregistratie. WAARSCHUWING</li><li>logboekregistratie. FOUT</li><li>logboekregistratie. KRITIEKE</li>  | logging.INFO</li> 
`X` | Alle functies te trainen met |  Geen
`y` |   Voeg een label te trainen met gegevens. Voor de classificatie moet een matrix van gehele getallen zijn.|  Geen
`X_valid`|_Optionele_ alle functies waarmee moet worden gevalideerd. Indien niet opgegeven, X wordt verdeeld tussen train en valideren |   Geen
`y_valid`   |_Optionele_ de labelgegevens te valideren met. Indien niet opgegeven, y wordt verdeeld tussen train en valideren    | Geen
`sample_weight` |   _Optionele_ een gewicht voor elk voorbeeld. Gebruik deze sjabloon wanneer u wilt toewijzen van verschillende gewichten voor uw data-punten |   Geen
`sample_weight_valid`   |   _Optionele_ een gewicht voor elk Validatievoorbeeld. Indien niet opgegeven, sample_weight wordt verdeeld tussen train en valideren   | Geen
`run_configuration` |   RunConfiguration-object.  Gebruikt voor extern worden uitgevoerd. |Geen
`data_script`  |    Pad naar een bestand met de methode get_data.  Vereist voor extern worden uitgevoerd.   |Geen


## <a name="run-automl-experiment"></a>Voer `AutoML` experimenteren

Vervolgens kunnen we het experiment uitvoeren en het genereren van een model voor ons initiëren. Geeft de `AutoMLConfig` naar de `submit` methode voor het genereren van het model.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Afhankelijkheden zijn geïnstalleerd op een nieuwe DSVM.  Het kan maximaal 10 minuten duren voordat uitvoer wordt weergegeven.
>Instellen van `show_output` met de waarde True, resulteert dit in de uitvoer wordt weergegeven op de console.


## <a name="explore-model-metrics"></a>Model metrische gegevens verkennen
U kunt uw resultaten weergeven in een widget of een inline als u zich in een notitieblok. Zie de details op 'Bijhouden en evalueren van modellen'. (Zorg ervoor dat AML-inhoud bevat relevante informatie die u AutoML)

De volgende metrische gegevens worden opgeslagen in elke iteratie
* AUC_macro
* AUC_micro
* AUC_weighted
* AUC_weighted_max
* nauwkeurigheid
* average_precision_score_macro
* average_precision_score_micro
* average_precision_score_weighted
* balanced_accuracy
* f1_score_macro
* f1_score_micro
* f1_score_weighted
* log_loss
* norm_macro_recall
* precision_score_macro
* precision_score_micro
* precision_score_weighted
* recall_score_macro
* recall_score_micro
* recall_score_weighted
* weighted_accuracy

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [hoe en waar u kunt een model implementeren](how-to-deploy-and-where.md).
