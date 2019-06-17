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
ms.date: 05/30/2019
ms.openlocfilehash: 4f1dd0bfc0637c6681b513860a63df2b8c35fe5e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66430853"
---
# <a name="model-interpretability-with-azure-machine-learning-service"></a>Model interpretability met Azure Machine Learning-service

In dit artikel leert u hoe u waarin wordt uitgelegd waarom het model de voorspellingen gedaan zodanig met de verschillende interpretability pakketten van de Azure Machine Learning Python SDK.

Met behulp van de klassen en methoden in de SDK, kunt u krijgen:
+ Functie van waarden van belang voor onbewerkte en Social engineering-functies
+ Interpretability op echte gegevenssets op schaal, tijdens de training en Deductie.
+ Interactieve visualisaties om u te helpen de detectie van patronen in gegevens en uitleg over training tegelijk

Tijdens de fase van de training van het ontwikkelingsproces doet, modelontwerpers en evaluaties interpretability uitvoer van een model gebruiken om te controleren hypothesen en bouw een vertrouwensrelatie met belanghebbenden.  Ze het inzicht in het model ook gebruiken voor het opsporen van fouten, valideren model gedrag komt overeen met hun doelstellingen, en om te controleren op de afwijking.

In machine learning, **functies** de gegevensvelden gebruikt om te voorspellen van een gegevenspunt doel. Bijvoorbeeld, om te voorspellen kredietrisico, kunnen gegevensvelden voor leeftijd, de accountgrootte en leeftijd van de account worden gebruikt. In dit geval, leeftijd, de accountgrootte en account leeftijd zijn **functies**. Urgentie van de functie kunt u zien hoe elk gegevensveld van het model voorspellingen beïnvloed. Bijvoorbeeld, kan leeftijd worden veel gebruikt in de voorspelling terwijl de accountgrootte van het en de leeftijd hebben geen invloed op de nauwkeurigheid aanzienlijk. Dit proces kan gegevenswetenschappers om uit te leggen van de resulterende voorspellingen zodat belanghebbenden inzicht hebben in welke gegevenspunten in het model het belangrijkst zijn.

Met deze hulpprogramma's, kunt u machine learning-modellen uitleggen **wereldwijd op alle gegevens**, of **lokaal op een specifieke gegevenspunten** met behulp van de geavanceerde technologieën in een eenvoudig te gebruiken en schaalbare manier.

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

* **Tree Explainer**: De Shapegegevens structuur uitleg, dat is gericht op polynomiale time snelle Shapegegevens waarde schatting van de algoritme specifiek voor bomen en ensembles van structuren.
* **Uitgebreide uitleg**: Op basis van de uitleg van Shapegegevens, diepgaande uitleg "is een snelle schattingsalgoritme dat de waarden van de Shapegegevens in deep learning-modellen die gebruikmaakt van een verbinding met die worden beschreven in het document Shapegegevens NIPS DeepLIFT. TensorFlow-modellen en Keras-modellen met behulp van de TensorFlow back-end worden ondersteund (Er is ook voorlopige ondersteuning voor PyTorch) '.
* **Kernel Explainer**: De Shapegegevens Kernel uitleg maakt gebruik van een speciaal gewogen lokale lineaire regressie kunt u schatten Shapegegevens waarden voor een model.
* **Uitleg nabootsen**: Nabootsen uitleg is gebaseerd op het idee van globale surrogate modellen. Een globale surrogate-model is een intrinsiek interpreteerbare die naar schatting zo nauwkeurig mogelijk maken van de voorspellingen van een zwart vak model wordt getraind. Gegevenswetenschapper kan het model surrogate als u wilt tekenen conclusies over de zwarte doos-model worden geïnterpreteerd.
* **LICHTGROEN uitleg** (`contrib`): LICHTGROEN uitleg op basis van LICHTGROEN, gebruikt het status-of-the-art lokale interpreteerbare model-agnostische uitleg (LICHTGROEN)-algoritme om lokale surrogate modellen te maken. In tegenstelling tot de modellen globale surrogate is LICHTGROEN gericht op het lokale surrogate modellen om uit te leggen van afzonderlijke voorspellingen training.
* **HAN tekst uitleg** (`contrib`): HAN tekst uitleg gebruikt een hiërarchische aandacht-netwerk voor het ophalen van model uitleg van de gegevens voor een bepaalde zwarte doos tekst-model. We het model van de vervangende HAN op de verwachte uitvoer van een bepaalde docent-model te trainen. Na de training wereldwijd in het corpus tekst, hebben we een stap fine-tune voor een bepaald document toegevoegd om te verbeteren van de nauwkeurigheid van de uitleg. HAN maakt gebruik van een in twee richtingen RNN met twee lagen van aandacht, voor de zin en word aandacht. Zodra de DNN is getraind model van de docent en zorgvuldig afgestemd op een bepaald document, kunnen we de importances word extraheren uit de lagen aandacht. We hebben HAN nauwkeuriger is dan LICHTGROEN of Shapegegevens voor tekstgegevens, maar duurder in termen van training, evenals de tijd worden gevonden. We hebben echter verbeteringen in de trainingstijd gemaakt door middel van de gebruiker de optie voor het initialiseren van het netwerk met GloVe woordinsluitingen, hoewel deze nog steeds traag is. De trainingstijd kan aanzienlijk worden verbeterd door het uitvoeren van HAN op een externe Azure-GPU VM. De implementatie van HAN wordt beschreven in 'Hiërarchische aandacht netwerken voor Document-classificatie (Yang markt, 2016)' ([https://www.cs.cmu.edu/~diyiy/docs/naacl16.pdf](https://www.cs.cmu.edu/~diyiy/docs/naacl16.pdf)).

__Metagegevens explainers__ automatisch een geschikte direct uitleg selecteren en genereren van de gegevens van de beste uitleg op basis van het opgegeven model en gegevenssets. De metagegevens explainers gebruikmaken van alle bibliotheken (Shapegegevens, LICHTGROEN, nagebootst, enzovoort) die we hebben geïntegreerd of die zijn ontwikkeld. Hier volgen de meta-explainers beschikbaar in de SDK:

* **Tabular Explainer**: Gebruikt in combinatie met tabellaire gegevenssets.
* **Text Explainer**: Met de tekst gegevenssets gebruikt.

Verder naar META te selecteren van de directe explainers, meta explainers ontwikkelen van aanvullende functies boven op de onderliggende bibliotheken en de snelheid en schaalbaarheid verbeteren via de rechtstreekse explainers.

Op dit moment `TabularExplainer` maakt gebruik van de volgende logica voor het aanroepen van de directe Explainers:

1. Als dit een model op basis van een structuur is, toepassing `TreeExplainer`, anders
2. Als dit een model DNN is, toepassing `DeepExplainer`, anders
3. Deze behandelen als een zwarte doos-model en toe te passen `KernelExplainer`

De informatie die is ingebouwd in `TabularExplainer` meer geavanceerde worden meer bibliotheken zijn geïntegreerd in de SDK en we meer informatie over de voordelen en nadelen van elke uitleg.

`TabularExplainer` heeft ook belangrijke functie en de prestaties zijn verbeteringen aangebracht via de rechtstreekse Explainers:

* **Samenvatting van de gegevensset initialisatie**. In gevallen waar de snelheid van uitleg belangrijkste is we samenvatten van de gegevensset initialisatie en genereren van een klein aantal representatieve voorbeelden die globale en lokale uitleg wordt versneld.
* **Sampling van de gegevensset evaluatie**. Als de gebruiker wordt doorgegeven in een groot aantal voorbeelden van de evaluatie, maar niet daadwerkelijk nodig voor al deze moet worden geëvalueerd, kunt u de parameter steekproeven instellen op true om de globale uitleg te versnellen.

Het volgende diagram toont de relatie tussen de twee sets met direct en metagegevens explainers.

[![Machine Learning Interpretability architectuur](./media/machine-learning-interpretability-explainability/interpretability-architecture.png)](./media/machine-learning-interpretability-explainability/interpretability-architecture.png#lightbox)

### <a name="models-supported"></a>Modellen dat wordt ondersteund

Modellen die zijn getraind op gegevenssets in Python `numpy.array`, `pandas.DataFrame`, `iml.datatypes.DenseData`, of `scipy.sparse.csr_matrix` indeling worden ondersteund door de interpretability `explain` pakket van de SDK.

De uitleg-functies accepteren pijplijnen en modellen als invoer. Als een model is opgegeven, het model moet worden gebruikt voor het implementeren van de voorspellingsfunctie `predict` of `predict_proba` die voldoet aan de Scikit-overeenkomst. Als een pijplijn (naam van het script pijplijn) is opgegeven, de functie uitleg wordt ervan uitgegaan dat het script uit te voeren pijplijn een voorspelling retourneert.

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
    # Split data into train and test
    from sklearn.model_selection import train_test_split
    x_train, x_test, y_train, y_test = train_test_split(breast_cancer_data.data, breast_cancer_data.target, test_size=0.2, random_state=0)
    clf = svm.SVC(gamma=0.001, C=100., probability=True)
    model = clf.fit(x_train, y_train)
    ```

2. Roep de uitleg: Voor het initialiseren van een object uitleg, moet u uw model en sommige trainingsgegevens doorgeven aan de constructor van de uitleg. U kunt eventueel ook in de onderdeelnamen en uitvoer klassenamen (als classificatie) dat wordt gebruikt om de uitleg en visualisaties meer informatieve doorgeven. Hier wordt beschreven hoe u exemplaar maken van een uitleg object met [TabularExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.tabularexplainer?view=azure-ml-py) en [MimicExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.mimic.mimicexplainer?view=azure-ml-py) lokaal. `TabularExplainer` roept een van de drie explainers onder (`TreeExplainer`, `DeepExplainer`, of `KernelExplainer`), en is automatisch selecteren van de meest geschikt is voor uw situatie. U kunt echter elk van de drie onderliggende explainers rechtstreeks aanroepen.

    ```python
    from azureml.explain.model.tabular_explainer import TabularExplainer
    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, x_train, features=breast_cancer_data.feature_names, classes=classes)
    ```

    of

    ```python
    from azureml.explain.model.mimic.mimic_explainer import MimicExplainer
    from azureml.explain.model.mimic.models.lightgbm_model import LGBMExplainableModel

    # "features" and "classes" fields are optional
    explainer = MimicExplainer(model, x_train, LGBMExplainableModel, features=breast_cancer_data.feature_names, classes=classes)
    ```

3. De algemene functie belang waarden ophalen.

    ```python
    # You can use the training data or the test data here
    global_explanation = explainer.explain_global(x_train)
    # Sorted feature importance values and feature names
    sorted_global_importance_values = global_explanation.get_ranked_global_values()
    sorted_global_importance_names = global_explanation.get_ranked_global_names()
    dict(zip(sorted_global_importance_names, sorted_global_importance_values))
    ```

4. Lokale functie belang waarden: Gebruik de volgende functieaanroepen om een afzonderlijk exemplaar of een groep exemplaren van te leggen.

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
    run = Run.get_context()
    client = ExplanationClient.from_run(run)

    # Train your model here

    # explain predictions on your local machine
    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, x_train, features=breast_cancer_data.feature_names, classes=classes)
    # explain overall model predictions (global explanation)
    global_explanation = explainer.explain_global(x_test)
    # explain local data points (individual instances)
    local_explanation = explainer.explain_local(x_test[0])
    # upload global and local explanation objects to Run History
    client.upload_model_explanation(run, local_explanation, top_k=2, comment='local explanation: top 2 features')
    # Uploading global model explanation data for storage or visualization in webUX
    # The explanation can then be downloaded on any compute
    # Multiple explanations can be uploaded
    client.upload_model_explanation(global_explanation, comment='global explanation: all features')
    # Or you can only upload the explanation object with the top k feature info
    #client.upload_model_explanation(global_explanation, top_k=2, comment='global explanation: Only top 2 features')
    ```

2. Volg de instructies op [instellen van compute-doelen voor modeltraining](how-to-set-up-training-targets.md#amlcompute) voor meer informatie over het instellen van een Azure Machine Learning-Computing als uw compute-doel en het verzenden van uw training uitvoeren.

3. Download de uitleg in uw lokaal Jupyter-notitieblok.

    ```python
    from azureml.contrib.explain.model.explanation.explanation_client import ExplanationClient
    # Get model explanation data
    client = ExplanationClient.from_run(run)
    explanation = client.download_model_explanation()
    local_importance_values = explanation.local_importance_values
    expected_values = explanation.expected_values
    # Or you can use the saved run.id to retrive the feature importance values
    client = ExplanationClient.from_run_id(ws, experiment_name, run.id)
    explanation = client.download_model_explanation()
    local_importance_values = explanation.local_importance_values
    expected_values = explanation.expected_values
    # Get the top k (e.g., 4) most important features with their importance values
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

[![Visualisatie Dashboard lokale](./media/machine-learning-interpretability-explainability/local-charts.png)](./media/machine-learning-interpretability-explainability/local-charts.png#lightbox)

Gebruik de volgende code voor het laden van de visualisatie-dashboard:

```python
from azureml.contrib.explain.model.visualize import ExplanationDashboard

ExplanationDashboard(global_explanation, model, x_test)
```

## <a name="raw-feature-transformations"></a>Onbewerkte functie transformaties

U kunt eventueel uw functie transformation pipeline doorgeven aan de uitleg voor het ontvangen van uitleg wat betreft de onbewerkte functies voordat u de transformatie (in plaats dat functies). Als u deze stap overslaan, biedt de uitleg uitleg in termen van Social engineering functies.

De indeling van de ondersteunde transformaties is dezelfde als die wordt beschreven in [sklearn pandas](https://github.com/scikit-learn-contrib/sklearn-pandas). Alle transformaties worden in het algemeen worden ondersteund, zolang ze met één kolom werken en daarom duidelijk op-veel zijn.

```python
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.linear_model import LogisticRegression
from sklearn_pandas import DataFrameMapper

# Assume that we have created two arrays, numerical and categorical, which holds the numerical and categorical feature names

numeric_transformations = [([f], Pipeline(steps=[('imputer', SimpleImputer(strategy='median')), ('scaler', StandardScaler())])) for f in numerical]

categorical_transformations = [([f], OneHotEncoder(handle_unknown='ignore', sparse=False)) for f in categorical]

transformations = numeric_transformations + categorical_transformations

# Append model to preprocessing pipeline.
# Now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', DataFrameMapper(transformations)),
                    ('classifier', LogisticRegression(solver='lbfgs'))])

# clf.steps[-1][1] returns the trained classification model
# Pass transformation as an input to create the explanation object
# "features" and "classes" fields are optional
tabular_explainer = TabularExplainer(clf.steps[-1][1], initialization_examples=x_train, features=dataset_feature_names, classes=dataset_classes, transformations=transformations)
```

## <a name="interpretability-in-inference"></a>Interpretability in Deductie

De uitleg samen met het oorspronkelijke model kan worden geïmplementeerd en kan worden gebruikt op het scoring-tijd uitleg van lokale gegevens op te geven. Het proces voor het implementeren van een scoring uitleg is vergelijkbaar met het implementeren van een model en bevat de volgende stappen uit:

1. Een uitleg-object maken:

   ```python
   from azureml.contrib.explain.model.tabular_explainer import TabularExplainer

   explainer = TabularExplainer(model, x_test)
   ```

1. Maak een scoring uitleg met behulp van het object uitleg:

   ```python
   scoring_explainer = explainer.create_scoring_explainer(x_test)

   # Pickle scoring explainer
   scoring_explainer_path = scoring_explainer.save('scoring_explainer_deploy')
   ```

1. Configureren en registreren van een installatiekopie die gebruikmaakt van het scoring uitleg-model.

   ```python
   # Register explainer model using the path from ScoringExplainer.save - could be done on remote compute
   run.upload_file('breast_cancer_scoring_explainer.pkl', scoring_explainer_path)
   model = run.register_model(model_name='breast_cancer_scoring_explainer', model_path='breast_cancer_scoring_explainer.pkl')
   print(model.name, model.id, model.version, sep = '\t')
   ```

1. [Optioneel] De score uitleg ophalen uit de cloud en de uitleg testen

   ```python
   from azureml.contrib.explain.model.scoring.scoring_explainer import ScoringExplainer

   # Retrieve the scoring explainer model from cloud"
   scoring_explainer_model = Model(ws, 'breast_cancer_scoring_explainer')
   scoring_explainer_model_path = scoring_explainer_model.download(target_dir=os.getcwd(), exist_ok=True)

   # Load scoring explainer from disk
   scoring_explainer = ScoringExplainer.load(scoring_explainer_model_path)

   # Test scoring explainer locally
   preds = scoring_explainer.explain(x_test)
   print(preds)
   ```

1. De installatiekopie implementeert op een compute-doel:

   1. Maak een scoring-bestand (voordat u deze stap maakt u de stappen in [Implementeer modellen met de service Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where) voor het registreren van uw oorspronkelijke voorspellingsmodel)

        ```python
        %%writefile score.py
        import json
        import numpy as np
        import os
        import pickle
        from sklearn.externals import joblib
        from sklearn.linear_model import LogisticRegression
        from azureml.core.model import Model

        def init():

            global original_model
            global scoring_model

            # Retrieve the path to the model file using the model name
            # Assume original model is named original_prediction_model
            original_model_path = Model.get_model_path('original_prediction_model')
            scoring_explainer_path = Model.get_model_path('breast_cancer_scoring_explainer')

            original_model = joblib.load(original_model_path)
            scoring_explainer = joblib.load(scoring_explainer_path)

        def run(raw_data):
            # Get predictions and explanations for each data point
            data = np.array(json.loads(raw_data)['data'])
            # Make prediction
            predictions = original_model.predict(data)
            # Retrieve model explanations
            local_importance_values = scoring_explainer.explain(data)
            # You can return any data type as long as it is JSON-serializable
            return {'predictions': predictions.tolist(), 'local_importance_values': local_importance_values}
        ```

   1. Definieer de configuratie van de implementatie (deze configuratie is afhankelijk van de vereisten van uw model. Het volgende voorbeeld definieert een configuratie die gebruikmaakt van één CPU-kern en 1 GB geheugen)

        ```python
        from azureml.core.webservice import AciWebservice

        aciconfig = AciWebservice.deploy_configuration(cpu_cores=1,
                                                       memory_gb=1,
                                                       tags={"data": "breastcancer",
                                                             "method" : "local_explanation"},
                                                       description='Get local explanations for breast cancer data')
        ```

   1. Maak een bestand met de omgeving afhankelijkheden

        ```python
        from azureml.core.conda_dependencies import CondaDependencies

        # WARNING: to install this, g++ needs to be available on the Docker image and is not by default (look at the next cell)


        myenv = CondaDependencies.create(pip_packages=["azureml-defaults", "azureml-explain-model", "azureml-contrib-explain-model"],
                                        conda_packages=["scikit-learn"])

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

        # Use the custom scoring, docker, and conda files we created above
        image_config = ContainerImage.image_configuration(execution_script="score.py",
                                                        docker_file="dockerfile",
                                                        runtime="python",
                                                        conda_file="myenv.yml")

        # Use configs and models generated above
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

    # Create data to test service with
    x_list = x_test.tolist()
    examples = x_list[:4]
    input_data = "{\"data\": " + str(examples) + "}"

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
