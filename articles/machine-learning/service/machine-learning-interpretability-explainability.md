---
title: Interpreteerbaarheid van modellen
titleSuffix: Azure Machine Learning service
description: Leer hoe u waarin wordt uitgelegd waarom uw model Hiermee worden voorspellingen gedaan met behulp van de SDK van Azure Machine Learning. Het kan worden gebruikt tijdens de training en Deductie om te begrijpen hoe het model voorspellingen maakt.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: larryfr
ms.date: 06/21/2019
ms.openlocfilehash: cba46a277dfce93d0080d8f04a26fd135407de15
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67536736"
---
# <a name="model-interpretability-with-azure-machine-learning-service"></a>Model interpretability met Azure Machine Learning-service

In dit artikel leert u hoe u waarin wordt uitgelegd waarom het model de voorspellingen gedaan zodanig met de verschillende interpretability pakketten van de Azure Machine Learning Python SDK.

Met behulp van de klassen en methoden in de SDK, kunt u krijgen:
+ Functie van waarden van belang voor onbewerkte en Social engineering-functies
+ Interpretability op echte gegevenssets op schaal, tijdens de training en Deductie.
+ Interactieve visualisaties om u te helpen de detectie van patronen in gegevens en uitleg over training tegelijk

Tijdens de fase van de training van het ontwikkelingsproces doet, modelontwerpers en evaluaties interpretability uitvoer van een model gebruiken om te controleren hypothesen en bouw een vertrouwensrelatie met belanghebbenden.  Ze het inzicht in het model ook gebruiken voor het opsporen van fouten, valideren model gedrag komt overeen met hun doelstellingen, en om te controleren op de afwijking.

In machine learning, **functies** de gegevensvelden gebruikt om te voorspellen van een gegevenspunt doel. Bijvoorbeeld, om te voorspellen kredietrisico, kunnen gegevensvelden voor leeftijd, de accountgrootte en leeftijd van de account worden gebruikt. In dit geval, leeftijd, de accountgrootte en account leeftijd zijn **functies**. Urgentie van de functie kunt u zien hoe elk gegevensveld van het model voorspellingen beïnvloed. Bijvoorbeeld, kan leeftijd worden veel gebruikt in de voorspelling terwijl de accountgrootte van het en de leeftijd hebben geen invloed op de nauwkeurigheid aanzienlijk. Dit proces kan gegevenswetenschappers om uit te leggen van de resulterende voorspellingen zodat belanghebbenden inzicht hebben in welke gegevenspunten in het model het belangrijkst zijn.

Met deze hulpprogramma's, kunt u machine learning-modellen uitleggen **wereldwijd op alle gegevens**, of **lokaal op een bepaald gegevenspunt** met behulp van de geavanceerde technologieën in een eenvoudig te gebruiken en schaalbare manier.

De klassen interpretability zijn beschikbaar gemaakt via meerdere SDK-pakketten. Meer informatie over het [SDK-pakketten installeren voor Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* [`azureml.explain.model`](https://docs.microsoft.com/python/api/azureml-explain-model/?view=azure-ml-py), de belangrijkste pakket, met functies die worden ondersteund door Microsoft.

* `azureml.contrib.explain.model`, preview en experimentele functies die u kunt proberen.

* `azureml.train.automl.automlexplainer` pakket voor de interpretatie van geautomatiseerde machine learning-modellen.

> [!IMPORTANT]
> De inhoud van de `contrib` naamruimte is niet volledig worden ondersteund. Zodra de experimentele functies volwassen, worden ze geleidelijk worden verplaatst naar de primaire naamruimte.

## <a name="how-to-interpret-your-model"></a>Hoe u uw model interpreteren

U kunt de interpretability klassen en methoden om te begrijpen van de globale gedrag van het model of voorspellingen op specifieke toepassen. De oude heet algemene uitleg en de laatste heet lokale uitleg.

De methoden kunnen ook worden ingedeeld op basis van of de methode model neutraal of specifieke model is. Sommige methoden gericht op bepaalde type modellen. Bijvoorbeeld, de Shapegegevens structuur uitleg alleen van toepassing op modellen op basis van een structuur. Bepaalde methoden voor behandelen het model als een zwart vak, zoals nabootsen uitleg of de Shapegegevens kernel uitleg. De `explain` pakket maakt gebruik van deze verschillende methoden die zijn gebaseerd op gegevenssets, modeltypen en use-cases.

De uitvoer is een verzameling van informatie over hoe een bepaald model in staat de voorspelling, zoals stelt:
* Globale/lokale relatieve functie urgentie

* Relatie tussen de functie en voorspelling van de globale/lokale

### <a name="explainers"></a>Explainers

Er zijn twee sets explainers: Directe Explainers en metagegevens Explainers in de SDK.

__Directe explainers__ afkomstig zijn van geïntegreerde bibliotheken. De SDK wordt verpakt alle explainers zodat ze een gemeenschappelijke API en de indeling van uitvoer weergeven. Als u meer vertrouwd rechtstreeks met behulp van deze explainers bent, kunt u deze rechtstreeks in plaats van de algemene API en de uitvoerindeling aanroepen. Hier volgen een lijst van de directe explainers beschikbaar in de SDK:

* **Shapegegevens structuur uitleg**: De Shapegegevens structuur uitleg, dat is gericht op polynomiale time snelle Shapegegevens waarde schatting van de algoritme specifiek voor bomen en ensembles van structuren.
* **SHAP Deep Explainer**: Op basis van de uitleg van Shapegegevens, diepgaande uitleg "is een snelle schattingsalgoritme dat de waarden van de Shapegegevens in deep learning-modellen die gebruikmaakt van een verbinding met die worden beschreven in het document Shapegegevens NIPS DeepLIFT. TensorFlow-modellen en Keras-modellen met behulp van de TensorFlow back-end worden ondersteund (Er is ook voorlopige ondersteuning voor PyTorch) '.
* **SHAP Kernel Explainer**: De Shapegegevens Kernel uitleg maakt gebruik van een speciaal gewogen lokale lineaire regressie kunt u schatten Shapegegevens waarden voor een model.
* **Uitleg nabootsen**: Nabootsen uitleg is gebaseerd op het idee van globale surrogate modellen. Een globale surrogate-model is een intrinsiek interpreteerbare die naar schatting zo nauwkeurig mogelijk maken van de voorspellingen van een zwart vak model wordt getraind. Gegevenswetenschapper kan het model surrogate als u wilt tekenen conclusies over de zwarte doos-model worden geïnterpreteerd. U kunt een van de volgende interpreteerbare modellen gebruiken als uw model surrogate: LightGBM (LinearExplainableModel), lineaire regressie (LinearExplainableModel), stochastische leren met stochastische Gradiëntdaling explainable model (SGDExplainableModel) en beslissingsstructuur (DecisionTreeExplainableModel).


* **Permutatie functie belang uitleg**: Permutatie functie urgentie is een techniek die wordt gebruikt om uit te leggen van de classificatie- en regressiemodellen modellen die is laat u inspireren door [Breiman van willekeurige Forests papier](https://www.stat.berkeley.edu/%7Ebreiman/randomforest2001.pdf) (Zie de sectie 10). Op hoog niveau is de manier waarop die deze werkt door willekeurige volgorde wijzigen van een functie van de gegevens op een tijdstip voor de volledige gegevensset en het berekenen van hoeveel de metrische gegevens voor prestaties van belang afneemt. Hoe groter de wijziging, het belangrijker is om deze functie.

* **LICHTGROEN uitleg** (`contrib`): LICHTGROEN uitleg op basis van LICHTGROEN, gebruikt het status-of-the-art lokale interpreteerbare model-agnostische uitleg (LICHTGROEN)-algoritme om lokale surrogate modellen te maken. In tegenstelling tot de modellen globale surrogate is LICHTGROEN gericht op het lokale surrogate modellen om uit te leggen van afzonderlijke voorspellingen training.
* **HAN tekst uitleg** (`contrib`): HAN tekst uitleg gebruikt een hiërarchische aandacht-netwerk voor het ophalen van model uitleg van de gegevens voor een bepaalde zwarte doos tekst-model. We het model van de vervangende HAN op de verwachte uitvoer van een bepaalde docent-model te trainen. Na de training wereldwijd in het corpus tekst, hebben we een stap fine-tune voor een bepaald document toegevoegd om te verbeteren van de nauwkeurigheid van de uitleg. HAN maakt gebruik van een in twee richtingen RNN met twee lagen van aandacht, voor de zin en word aandacht. Zodra de DNN is getraind model van de docent en zorgvuldig afgestemd op een bepaald document, kunnen we de importances word extraheren uit de lagen aandacht. We hebben HAN nauwkeuriger is dan LICHTGROEN of Shapegegevens voor tekstgegevens, maar duurder in termen van training, evenals de tijd worden gevonden. We hebben echter verbeteringen in de trainingstijd gemaakt door middel van de gebruiker de optie voor het initialiseren van het netwerk met GloVe woordinsluitingen, hoewel deze nog steeds traag is. De trainingstijd kan aanzienlijk worden verbeterd door het uitvoeren van HAN op een externe Azure-GPU VM. De implementatie van HAN wordt beschreven in 'Hiërarchische aandacht netwerken voor Document-classificatie (Yang markt, 2016)' ([https://www.cs.cmu.edu/~diyiy/docs/naacl16.pdf](https://www.cs.cmu.edu/~diyiy/docs/naacl16.pdf)).


__Metagegevens explainers__ automatisch een geschikte direct uitleg selecteren en genereren van de gegevens van de beste uitleg op basis van het opgegeven model en gegevenssets. De metagegevens explainers gebruikmaken van alle bibliotheken (Shapegegevens, LICHTGROEN, nagebootst, enzovoort) die we hebben geïntegreerd of die zijn ontwikkeld. Hier volgen de meta-explainers beschikbaar in de SDK:

* **Tabular Explainer**: Gebruikt in combinatie met tabellaire gegevenssets.
* **Text Explainer**: Met de tekst gegevenssets gebruikt.
* **Afbeelding van uitleg**: Met de installatiekopie van gegevenssets gebruikt.

Verder naar META te selecteren van de directe explainers, meta explainers ontwikkelen van aanvullende functies boven op de onderliggende bibliotheken en de snelheid en schaalbaarheid verbeteren via de rechtstreekse explainers.

Op dit moment `TabularExplainer` maakt gebruik van de volgende logica voor het aanroepen van de directe Shapegegevens Explainers:

1. Als dit een model op basis van een structuur is, van toepassing Shapegegevens `TreeExplainer`, anders
2. Als dit een model DNN is, van toepassing Shapegegevens `DeepExplainer`, anders
3. Deze behandelen als een zwarte doos-model en Shapegegevens toepassen `KernelExplainer`

De informatie die is ingebouwd in `TabularExplainer` meer geavanceerde worden meer bibliotheken zijn geïntegreerd in de SDK en we meer informatie over de voordelen en nadelen van elke uitleg.

`TabularExplainer` heeft ook belangrijke functie en de prestaties zijn verbeteringen aangebracht via de rechtstreekse Explainers:

* **Samenvatting van de gegevensset initialisatie**. In gevallen waar de snelheid van uitleg belangrijkste is we samenvatten van de gegevensset initialisatie en genereren van een klein aantal representatieve voorbeelden die globale en lokale uitleg wordt versneld.
* **Sampling van de gegevensset evaluatie**. Als de gebruiker wordt doorgegeven in een groot aantal voorbeelden van de evaluatie, maar niet daadwerkelijk nodig voor al deze moet worden geëvalueerd, kunt u de parameter steekproeven instellen op true om de globale uitleg te versnellen.

Het volgende diagram toont de huidige structuur van de directe en metagegevens explainers.

[![Machine Learning Interpretability architectuur](./media/machine-learning-interpretability-explainability/interpretability-architecture.png)](./media/machine-learning-interpretability-explainability/interpretability-architecture.png#lightbox)


### <a name="models-supported"></a>Modellen dat wordt ondersteund

Modellen die zijn getraind op gegevenssets in Python `numpy.array`, `pandas.DataFrame`, `iml.datatypes.DenseData`, of `scipy.sparse.csr_matrix` indeling worden ondersteund door de interpretability `explain` pakket van de SDK.

De uitleg-functies accepteren pijplijnen en modellen als invoer. Als een model is opgegeven, het model moet worden gebruikt voor het implementeren van de voorspellingsfunctie `predict` of `predict_proba` die voldoet aan de Scikit-overeenkomst. Als een pijplijn (naam van het script pijplijn) is opgegeven, de functie uitleg wordt ervan uitgegaan dat het script uit te voeren pijplijn een voorspelling retourneert. We ondersteunen modellen die zijn getraind via PyTorch, TensorFlow en Keras deep learning-frameworks.

### <a name="local-and-remote-compute-target"></a>Lokale en externe compute-doel

De `explain` pakket is ontworpen voor gebruik met zowel lokale als externe compute-doelen. Als lokaal uitvoert, wordt de SDK-functies niet contact op met de Azure-services. U kunt een uitleg op afstand uitvoeren in Azure Machine Learning-Computing en meld u aan de gegevens van de uitleg bij Azure Machine Learning uitvoeren geschiedenis van Services. Zodra deze informatie wordt geregistreerd, zijn rapporten en visualisaties uit de uitleg gemakkelijk beschikbaar zijn op de portal van Azure Machine Learning-werkruimte voor analyse van gebruikers.

## <a name="interpretability-in-training"></a>Interpretability in training

### <a name="train-and-explain-locally"></a>Trainen en wordt uitgelegd lokaal

1. Uw model in een lokaal Jupyter-notitieblok te trainen.

    ```python
    # load breast cancer dataset, a well-known small dataset that comes with scikit-learn
    from sklearn.datasets import load_breast_cancer
    from sklearn import svm
    from sklearn.model_selection import train_test_split
    breast_cancer_data = load_breast_cancer()
    classes = breast_cancer_data.target_names.tolist()
    
    # split data into train and test
    from sklearn.model_selection import train_test_split
    x_train, x_test, y_train, y_test = train_test_split(breast_cancer_data.data,            
                                                        breast_cancer_data.target,  
                                                        test_size=0.2,
                                                        random_state=0)
    clf = svm.SVC(gamma=0.001, C=100., probability=True)
    model = clf.fit(x_train, y_train)
    ```

2. Roep de uitleg: Voor het initialiseren van een object uitleg, moet u uw model en sommige trainingsgegevens doorgeven aan de constructor van de uitleg. U kunt eventueel ook in de onderdeelnamen en uitvoer klassenamen (als classificatie) dat wordt gebruikt om de uitleg en visualisaties meer informatieve doorgeven. Hier wordt beschreven hoe u exemplaar maken van een uitleg object met [TabularExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.tabularexplainer?view=azure-ml-py), [MimicExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.mimic.mimicexplainer?view=azure-ml-py), en [PFIExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.permutation.permutation_importance.pfiexplainer?view=azure-ml-py) lokaal. `TabularExplainer` roept een van de drie Shapegegevens explainers onder (`TreeExplainer`, `DeepExplainer`, of `KernelExplainer`), en is automatisch selecteren van de meest geschikt is voor uw situatie. U kunt echter elk van de drie onderliggende explainers rechtstreeks aanroepen.

    ```python
    from azureml.explain.model.tabular_explainer import TabularExplainer
    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, 
                                 x_train, 
                                 features=breast_cancer_data.feature_names, 
                                 classes=classes)
    ```

    of

    ```python
    from azureml.explain.model.mimic.mimic_explainer import MimicExplainer
    
    # you can use one of the following four interpretable models as a global surrogate to the black box model
    from azureml.explain.model.mimic.models.lightgbm_model import LGBMExplainableModel
    from azureml.explain.model.mimic.models.linear_model import LinearExplainableModel
    from azureml.explain.model.mimic.models.linear_model import SGDExplainableModel
    from azureml.explain.model.mimic.models.tree_model import DecisionTreeExplainableModel

    # "features" and "classes" fields are optional
    # augment_data is optional and if true, oversamples the initialization examples to improve surrogate model accuracy to fit original model.  Useful for high-dimensional data where the number of rows is less than the number of columns. 
    # max_num_of_augmentations is optional and defines max number of times we can increase the input data size.
    # LGBMExplainableModel can be replaced with LinearExplainableModel, SGDExplainableModel, or DecisionTreeExplainableModel
    explainer = MimicExplainer(model, 
                               x_train, 
                               LGBMExplainableModel, 
                               augment_data=True, 
                               max_num_of_augmentations=10, 
                               features=breast_cancer_data.feature_names, 
                               classes=classes)
    ```
   of

    ```python
    from azureml.explain.model.permutation.permutation_importance import PFIExplainer 
    
    # "features" and "classes" fields are optional
    explainer = PFIExplainer(model, 
                             features=breast_cancer_data.feature_names, 
                             classes=classes)
    ```

3. De algemene functie belang waarden ophalen.

    ```python
    # you can use the training data or the test data here
    global_explanation = explainer.explain_global(x_train)
    
    # if you used the PFIExplainer in the previous step, use the next line of code instead
    # global_explanation = explainer.explain_global(x_train, true_labels=y_test)

    # sorted feature importance values and feature names
    sorted_global_importance_values = global_explanation.get_ranked_global_values()
    sorted_global_importance_names = global_explanation.get_ranked_global_names()
    dict(zip(sorted_global_importance_names, sorted_global_importance_values))

    # alternatively, you can print out a dictionary that holds the top K feature names and values
    global_explanation.get_feature_importance_dict()
    ```

4. De functie voor lokale belang waarden ophalen: Gebruik de volgende functieaanroepen om een afzonderlijk exemplaar of een groep exemplaren van te leggen. Houd er rekening mee PFIExplainer biedt geen ondersteuning voor lokale uitleg.

    ```python
    # explain the first data point in the test set
    local_explanation = explainer.explain_local(x_test[0])

    # sorted feature importance values and feature names
    sorted_local_importance_names = local_explanation.get_ranked_local_names()
    sorted_local_importance_values = local_explanation.get_ranked_local_values()
    ```

    of

    ```python
    # explain the first five data points in the test set
    local_explanation = explainer.explain_local(x_test[0:4])

    # sorted feature importance values and feature names
    sorted_local_importance_names = local_explanation.get_ranked_local_names()
    sorted_local_importance_values = local_explanation.get_ranked_local_values()
    ```

### <a name="train-and-explain-remotely"></a>Trainen en wordt uitgelegd op afstand

Terwijl u op de diverse compute-doelen die door Azure Machine Learning-service worden ondersteund trainen kunt, ziet het voorbeeld in deze sectie u hoe u doet dit met een Azure Machine Learning-Computing-doel.

1. Maak een trainingsscript in een lokaal Jupyter-notitieblok (bijvoorbeeld run_explainer.py).

    ```python
    from azureml.contrib.explain.model.explanation.explanation_client import ExplanationClient
    from azureml.core.run import Run

    run = Run.get_context()
    client = ExplanationClient.from_run(run)

    # write code to get and split your data into train and test sets here
    # write code to train your model here 

    # explain predictions on your local machine
    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, 
                                x_train, 
                                features=feature_names, 
                                classes=classes)

    # explain overall model predictions (global explanation)
    global_explanation = explainer.explain_global(x_test)
    
    # uploading global model explanation data for storage or visualization in webUX
    # the explanation can then be downloaded on any compute
    # multiple explanations can be uploaded
    client.upload_model_explanation(global_explanation, comment='global explanation: all features')
    # or you can only upload the explanation object with the top k feature info
    #client.upload_model_explanation(global_explanation, top_k=2, comment='global explanation: Only top 2 features')
    ```

2. Volg de instructies op [instellen van compute-doelen voor modeltraining](how-to-set-up-training-targets.md#amlcompute) voor meer informatie over het instellen van een Azure Machine Learning-Computing als uw compute-doel en het verzenden van uw training uitvoeren.

3. Download de uitleg in uw lokaal Jupyter-notitieblok.

    ```python
    from azureml.contrib.explain.model.explanation.explanation_client import ExplanationClient
    
    client = ExplanationClient.from_run(run)
    
    # get model explanation data
    explanation = client.download_model_explanation()
    # or only get the top k (e.g., 4) most important features with their importance values
    explanation = client.download_model_explanation(top_k=4)
    
    global_importance_values = explanation.get_ranked_global_values()
    global_importance_names = explanation.get_ranked_global_names()
    print('global importance values: {}'.format(global_importance_values))
    print('global importance names: {}'.format(global_importance_names))
    ```

## <a name="visualizations"></a>Visualisaties

Gebruik het dashboard van de visualisatie om te begrijpen en interpreteren van uw model:

### <a name="global-visualizations"></a>Globale visualisaties

De volgende grafieken geven een algemeen overzicht van het getrainde model samen met de voorspellingen en uitleg.

|Tekengebied|Description|
|----|-----------|
|Data Exploration| Een overzicht van de gegevensset, samen met de voorspelling waarden.|
|Globale urgentie|Geeft de top K (configureerbare K) belangrijke functies wereldwijd. In deze grafiek is handig om te begrijpen van de globale gedrag van het onderliggende model.|
|Explanation Exploration|Laat zien hoe een functie is verantwoordelijk voor het maken van een wijziging in van het model prediction waarden (of kans op voorspelling waarden). |
|Samenvatting| Maakt gebruik van de waarden van een ondertekende lokale functie belang voor alle gegevenspunten om weer te geven van de distributie van de impact die elke functie op de waarde voor de voorspelling heeft.|

[![Globale visualisatie-Dashboard](./media/machine-learning-interpretability-explainability/global-charts.png)](./media/machine-learning-interpretability-explainability/global-charts.png#lightbox)

### <a name="local-visualizations"></a>Lokale visualisaties

U kunt op elk gewenst moment afzonderlijke klikken op elk gewenst moment van de voorgaande grafieken laden van de lokale functie belang plot voor het opgegeven gegevenspunt.

|Tekengebied|Description|
|----|-----------|
|Lokale urgentie|Geeft de top K (configureerbare K) belangrijke functies wereldwijd. In deze grafiek is handig om te begrijpen van de lokale gedrag van het onderliggende model op een bepaald gegevenspunt.|
|Perturbation verkennen|Hiermee kunt u de functie waarden van de geselecteerde gegevens verwijzen en bekijk hoe deze wijzigingen invloed is op voorspelling waarde wijzigen.|
|Afzonderlijke voorwaardelijke verwachting (IJS)| Hiermee kunt u een functie-waarde wijzigen van een minimale waarde in een maximumwaarde om te zien hoe de voorspelling van het gegevenspunt verandert wanneer een functie wordt gewijzigd.|

[![Visualisatie Dashboard lokale functie urgentie](./media/machine-learning-interpretability-explainability/local-charts.png)](./media/machine-learning-interpretability-explainability/local-charts.png#lightbox)


[![Visualisatie Dashboard functie Perturbation](./media/machine-learning-interpretability-explainability/perturbation.gif)](./media/machine-learning-interpretability-explainability/perturbation.gif#lightbox)


[![Visualisatie Dashboard ICE grafieken](./media/machine-learning-interpretability-explainability/ice-plot.png)](./media/machine-learning-interpretability-explainability/ice-plot.png#lightbox)

Houd er rekening mee moet u beschikken over uitbreidingen voor widget van het dashboard visualisatie is ingeschakeld voordat het starten van een Jupyter-kernel.

* Jupyter-notebooks

    ```shell
    jupyter nbextension install --py --sys-prefix azureml.contrib.explain.model.visualize
    jupyter nbextension enable --py --sys-prefix azureml.contrib.explain.model.visualize
    ```



* Jupyter Labs

    ```shell
    jupyter labextension install @jupyter-widgets/jupyterlab-manager
    jupyter labextension install microsoft-mli-widget
    ```
Gebruik de volgende code voor het laden van de visualisatie-dashboard:

```python
from azureml.contrib.explain.model.visualize import ExplanationDashboard

ExplanationDashboard(global_explanation, model, x_test)
```

## <a name="raw-feature-transformations"></a>Onbewerkte functie transformaties

U kunt eventueel uw functie transformation pipeline doorgeven aan de uitleg voor het ontvangen van uitleg wat betreft de onbewerkte functies voordat u de transformatie (in plaats dat functies). Als u deze stap overslaan, biedt de uitleg uitleg in termen van Social engineering functies.

De indeling van de ondersteunde transformaties is dezelfde als die wordt beschreven in [sklearn pandas](https://github.com/scikit-learn-contrib/sklearn-pandas). Alle transformaties worden in het algemeen worden ondersteund, zolang ze met één kolom werken en daarom duidelijk op-veel zijn. 

We uitgelegd onbewerkte functies met behulp van een `sklearn.compose.ColumnTransformer` of een lijst met gemonteerd transformator tuples. De cel hieronder wordt `sklearn.compose.ColumnTransformer`. 

```python
from sklearn.compose import ColumnTransformer

numeric_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='median')),
    ('scaler', StandardScaler())])

categorical_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='constant', fill_value='missing')),
    ('onehot', OneHotEncoder(handle_unknown='ignore'))])

preprocessor = ColumnTransformer(
    transformers=[
        ('num', numeric_transformer, numeric_features),
        ('cat', categorical_transformer, categorical_features)])

# append classifier to preprocessing pipeline.
# now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', preprocessor),
                      ('classifier', LogisticRegression(solver='lbfgs'))])



# append classifier to preprocessing pipeline.
# now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', preprocessor),
                      ('classifier', LogisticRegression(solver='lbfgs'))])


# clf.steps[-1][1] returns the trained classification model
# pass transformation as an input to create the explanation object
# "features" and "classes" fields are optional
tabular_explainer = TabularExplainer(clf.steps[-1][1], 
                                    initialization_examples=x_train, 
                                    features=dataset_feature_names, 
                                    classes=dataset_classes, 
                                    transformations=preprocessor) 
```

Als u uitvoeren in het voorbeeld met de lijst met gemonteerd transformator tuples wilt, gebruikt u de volgende code: 
```python
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.linear_model import LogisticRegression
from sklearn_pandas import DataFrameMapper

# assume that we have created two arrays, numerical and categorical, which holds the numerical and categorical feature names

numeric_transformations = [([f], Pipeline(steps=[('imputer', SimpleImputer(strategy='median')), ('scaler', StandardScaler())])) for f in numerical]

categorical_transformations = [([f], OneHotEncoder(handle_unknown='ignore', sparse=False)) for f in categorical]

transformations = numeric_transformations + categorical_transformations

# append model to preprocessing pipeline.
# now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', DataFrameMapper(transformations)),
                    ('classifier', LogisticRegression(solver='lbfgs'))])

# clf.steps[-1][1] returns the trained classification model
# pass transformation as an input to create the explanation object
# "features" and "classes" fields are optional
tabular_explainer = TabularExplainer(clf.steps[-1][1], 
                                     initialization_examples=x_train, 
                                     features=dataset_feature_names, 
                                     classes=dataset_classes, 
                                     transformations=transformations)
```

## <a name="interpretability-at-inferencing-time"></a>Interpretability inferentietaken tegelijk

De uitleg samen met het oorspronkelijke model kan worden geïmplementeerd en kan worden gebruikt op het scoring-tijd uitleg van lokale gegevens op te geven. We bieden ook de scoring explainers lichtere zodat interpretability op inferentietaken keer beter. Het proces voor het implementeren van een lichtere scoring uitleg is vergelijkbaar met het implementeren van een model en bevat de volgende stappen uit:




1. Maken van een uitleg-object (bijvoorbeeld met behulp van TabularExplainer):

   ```python
   from azureml.contrib.explain.model.tabular_explainer import TabularExplainer

   explainer = TabularExplainer(model, 
                                initialization_examples=x_train, 
                                features=dataset_feature_names, 
                                classes=dataset_classes, 
                                transformations=transformations)
   ```

1. Maak een scoring uitleg met behulp van het object uitleg:

   ```python
   from azureml.contrib.explain.model.scoring.scoring_explainer import KernelScoringExplainer, save

   # create a lightweight explainer at scoring time
   scoring_explainer = KernelScoringExplainer(explainer)

   # pickle scoring explainer
   # pickle scoring explainer locally
   OUTPUT_DIR = 'my_directory'
   save(scoring_explainer, directory=OUTPUT_DIR, exist_ok=True)
   ```

1. Configureren en registreren van een installatiekopie die gebruikmaakt van het scoring uitleg-model.

   ```python
   # register explainer model using the path from ScoringExplainer.save - could be done on remote compute
   # scoring_explainer.pkl is the filename on disk, while my_scoring_explainer.pkl will be the filename in cloud storage
   run.upload_file('my_scoring_explainer.pkl', os.path.join(OUTPUT_DIR, 'scoring_explainer.pkl'))
   
   scoring_explainer_model = run.register_model(model_name='my_scoring_explainer', 
                                                model_path='my_scoring_explainer.pkl')
   print(scoring_explainer_model.name, scoring_explainer_model.id, scoring_explainer_model.version, sep = '\t')
   ```

1. [Optioneel] De score uitleg ophalen uit de cloud en de uitleg testen

   ```python
   from azureml.contrib.explain.model.scoring.scoring_explainer import load

   # retrieve the scoring explainer model from cloud"
   scoring_explainer_model = Model(ws, 'my_scoring_explainer')
   scoring_explainer_model_path = scoring_explainer_model.download(target_dir=os.getcwd(), exist_ok=True)

   # load scoring explainer from disk
   scoring_explainer = load(scoring_explainer_model_path)

   # test scoring explainer locally
   preds = scoring_explainer.explain(x_test)
   print(preds)
   ```

1. De installatiekopie implementeert op een compute-doel:

   1. Maak een scoring-bestand (voordat u deze stap maakt u de stappen in [Implementeer modellen met de service Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where) voor het registreren van uw oorspronkelijke voorspellingsmodel)

        ```python
        %%writefile score.py
        import json
        import numpy as np
        import pandas as pd
        import os
        import pickle
        from sklearn.externals import joblib
        from sklearn.linear_model import LogisticRegression
        from azureml.core.model import Model

        def init():

            global original_model
            global scoring_model

            # retrieve the path to the model file using the model name
            # assume original model is named original_prediction_model
            original_model_path = Model.get_model_path('original_prediction_model')
            scoring_explainer_path = Model.get_model_path('my_scoring_explainer')

            original_model = joblib.load(original_model_path)
            scoring_explainer = joblib.load(scoring_explainer_path)

        def run(raw_data):
            # get predictions and explanations for each data point
            data = pd.read_json(raw_data)
            # make prediction
            predictions = original_model.predict(data)
            # retrieve model explanations
            local_importance_values = scoring_explainer.explain(data)
            # you can return any data type as long as it is JSON-serializable
            return {'predictions': predictions.tolist(), 'local_importance_values': local_importance_values}
        ```

   1. Definieer de configuratie van de implementatie (deze configuratie is afhankelijk van de vereisten van uw model. Het volgende voorbeeld definieert een configuratie die gebruikmaakt van één CPU-kern en 1 GB geheugen)

        ```python
        from azureml.core.webservice import AciWebservice

        aciconfig = AciWebservice.deploy_configuration(cpu_cores=1,
                                                       memory_gb=1,
                                                       tags={"data": "NAME_OF_THE_DATASET",
                                                             "method" : "local_explanation"},
                                                       description='Get local explanations for NAME_OF_THE_PROBLEM')
        ```

   1. Maak een bestand met de omgeving afhankelijkheden

        ```python
        from azureml.core.conda_dependencies import CondaDependencies

        # WARNING: to install this, g++ needs to be available on the Docker image and is not by default (look at the next cell)

        azureml_pip_packages = ['azureml-defaults', 'azureml-contrib-explain-model', 'azureml-core', 'azureml-telemetry', 'azureml-explain-model']
 

        # specify CondaDependencies obj
        myenv = CondaDependencies.create(conda_packages=['scikit-learn', 'pandas'],
                                         pip_packages=['sklearn-pandas'] + azureml_pip_packages,
                                         pin_sdk_version=False)


        with open("myenv.yml","w") as f:
            f.write(myenv.serialize_to_string())

        with open("myenv.yml","r") as f:
            print(f.read())
        ```

   1. Een aangepaste docker-bestand maken met g ++ geïnstalleerd

        ```python
        %%writefile dockerfile
        RUN apt-get update && apt-get install -y g++
        ```

   1. De gemaakte installatiekopie implementeert (geschatte tijd: 5 minuten)

        ```python
        from azureml.core.webservice import Webservice
        from azureml.core.image import ContainerImage

        # use the custom scoring, docker, and conda files we created above
        image_config = ContainerImage.image_configuration(execution_script="score.py",
                                                        docker_file="dockerfile",
                                                        runtime="python",
                                                        conda_file="myenv.yml")

        # use configs and models generated above
        service = Webservice.deploy_from_model(workspace=ws,
                                            name='model-scoring-service',
                                            deployment_config=aciconfig,
                                            models=[scoring_explainer_model, original_model],
                                            image_config=image_config)

        service.wait_for_deployment(show_output=True)
        ```

1. De implementatie testen

    ```python
    import requests

    # create data to test service with
    examples = x_list[:4]
    input_data = examples.to_json()

    headers = {'Content-Type':'application/json'}

    # send request to service
    resp = requests.post(service.scoring_uri, input_data, headers=headers)

    print("POST to url", service.scoring_uri)
    # can covert back to Python objects from json string if desired
    print("prediction:", resp.text)
    ```

1. Opschonen van: Als u wilt verwijderen van een geïmplementeerde webservice, gebruikt u `service.delete()`.

## <a name="interpretability-in-automated-ml"></a>Interpretability in geautomatiseerde ML

Geautomatiseerde machine learning bevat-pakketten voor de functie belangrijk voor het automatisch getrainde modellen interpreteren. Bovendien kunnen classificatie scenario's u om op te halen op klasseniveau functie belang. Er zijn twee methoden voor het inschakelen van dit gedrag in geautomatiseerde machine learning:

* Om in te schakelen belang van de functie voor een model getrainde ensembles, gebruikt u de [ `explain_model()` ](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlexplainer?view=azure-ml-py) functie.

    ```python
    from azureml.train.automl.automlexplainer import explain_model

    shap_values, expected_values, overall_summary, overall_imp, \
        per_class_summary, per_class_imp = explain_model(fitted_model, X_train, X_test)
    ```

* Om in te schakelen functie belang voor elke afzonderlijke uitvoering voordat een training, stel de `model_explainability` parameter `True` in de `AutoMLConfig` -object, samen met validatiegegevens levert. Gebruik vervolgens de [ `retrieve_model_explanation()` ](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlexplainer?view=azure-ml-py) functie.

    ```python
    from azureml.train.automl.automlexplainer import retrieve_model_explanation

    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, \
        per_class_imp = retrieve_model_explanation(best_run)
    ```

Zie voor meer informatie de [procedures](how-to-configure-auto-train.md#explain-the-model-interpretability) over het inschakelen van interpretability functies in automatische leerprocessen.

## <a name="next-steps"></a>Volgende stappen

Zie voor een verzameling van Jupyter-notitieblokken die laten zien van de bovenstaande instructies, de [Azure Machine Learning Interpretability voorbeeldnotitieblokken](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model).
