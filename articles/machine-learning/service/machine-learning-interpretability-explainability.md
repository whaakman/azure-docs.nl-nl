---
title: Interpreteerbaarheid van modellen
titleSuffix: Azure Machine Learning service
description: Leer hoe u waarin wordt uitgelegd waarom uw model Hiermee worden voorspellingen gedaan met de Azure Machine Learning Interpretability SDK. Het kan worden gebruikt tijdens de trainings- en inferentietaken om te begrijpen hoe het model voorspellingen maakt.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: larryfr
ms.date: 04/09/2019
ms.openlocfilehash: 668551603dfa2a9c42f4538fd9a66ee646e1feb7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60817247"
---
# <a name="model-interpretability-with-azure-machine-learning-service"></a>Model interpretability met Azure Machine Learning-service

In dit artikel leert u hoe waarin wordt uitgelegd waarom de voorspellingen in uw model worden gemaakt die met behulp van Azure Machine Learning Interpretability SDK. De mogelijkheid om uit te leggen van uw model is het belangrijk om de volgende redenen:

* Klanten en belanghebbenden willen weten **als ze de voorspellingen kunnen vertrouwen kunt u uw model**.
* Als een gegevenswetenschapper, die u wilt weten **hoe u query's van het model om te zoeken insights**. U moet ook hulpprogramma's om geïnformeerde beslissingen op **over het verbeteren van uw model**.
* Als een bedrijf, moet u weten wat **het gedrag van het model met uiteenlopende invoer distributies** en **hoe het model gedragen bij het analyseren van specifieke invoer**.

Machine learning interpretability is het belangrijk in twee fasen van machine learning-ontwikkeling: 

* Tijdens de **training**: Modelontwerpers en evaluaties vereisen interpretability hulpprogramma's om uit te leggen van de uitvoer van een model belanghebbenden vertrouwen te bouwen. Ze moeten ook inzicht in het model zodat ze kunnen fouten opsporen in het model en beslissingen te nemen op of het gedrag komt overeen met hun doelstellingen. Tot slot moeten om ervoor te zorgen dat het model niet is gericht.

* Tijdens de **inferentietaken**: Voorspellingen moeten explainable aan de mensen die gebruikmaken van uw model. Bijvoorbeeld, waarom het model een lening een weigeren of voorspellen dat een beleggingsportefeuille een hoger risico uitvoert?

De Azure Machine Learning Interpretability SDK omvat technologieën die zijn ontwikkeld door Microsoft en bibliotheken van derden (bijvoorbeeld Shapegegevens en LICHTGROEN) een bewezen oplossing. De SDK maakt een gemeenschappelijke API in de geïntegreerde bibliotheken en Azure Machine Learning-services kan worden geïntegreerd. Met deze SDK, kunt u machine learning-modellen uitleggen **wereldwijd op alle gegevens**, of **lokaal op een bepaald gegevenspunt** met behulp van de geavanceerde technologieën in een eenvoudig te gebruiken en schaalbare manier.

## <a name="how-does-it-work"></a>Hoe werkt het?

Azure Machine Learning Interpretability kan worden toegepast voor meer informatie over de algemene gedrag of specifieke voorspellingen van het model. De oude heet algemene uitleg en de laatste heet lokale uitleg.

Azure Machine Learning Interpretability methoden kunnen ook worden ingedeeld op basis van of de methode model neutraal of specifieke model is. Sommige methoden gericht op bepaalde type modellen. Bijvoorbeeld, de Shapegegevens structuur uitleg alleen van toepassing op modellen op basis van een structuur. Bepaalde methoden voor behandelen het model als een zwart vak, zoals nabootsen uitleg of de Shapegegevens kernel uitleg. Azure Machine Learning Interpretability SDK maakt gebruik van deze verschillende methoden die zijn gebaseerd op gegevenssets, modeltypen en use-cases.

Azure Machine Learning Interpretability retourneert een set met informatie over hoe een model maakt voor de voorspelling. De informatie bevat items zoals:

* Globale/lokale relatieve functie urgentie
* Relatie tussen de functie en voorspelling van de globale/lokale

## <a name="architecture"></a>Architectuur

Azure Machine Learning Interpretability SDK is gestructureerd in twee Python-pakketten:

* [azureml.explain.model](https://docs.microsoft.com/python/api/azureml-explain-model/?view=azure-ml-py) -de belangrijkste, dit pakket bevat de functies die worden ondersteund door Microsoft.
* `azureml.contrib.explain.model` -Bekijken en experimentele functies die u kunt proberen.

    > [!IMPORTANT]
    > Dingen in contrib niet volledig worden ondersteund. Zodra de experimentele functies volwassen, zullen ze geleidelijk worden verplaatst naar het belangrijkste pakket.

### <a name="explainers"></a>Explainers

De Azure Machine Learning Interpretability SDK introduceert twee sets explainers: Directe Explainers en metagegevens Explainers.

__Directe explainers__ afkomstig zijn van geïntegreerde bibliotheken. De SDK wordt verpakt alle explainers zodat ze een gemeenschappelijke API en de indeling van uitvoer weergeven. Hier volgen een lijst van de directe explainers beschikbaar in de SDK:

> [!TIP]
> Als u meer vertrouwd rechtstreeks met behulp van deze explainers bent, kunt u deze rechtstreeks in plaats van de algemene API en de uitvoerindeling aanroepen.

* **Tree Explainer**: De Shapegegevens structuur uitleg, dat is gericht op polynomiale time snelle Shapegegevens waarde schatting van de algoritme specifiek voor bomen en ensembles van structuren.
* **Uitgebreide uitleg**: Op basis van de uitleg van Shapegegevens, diepgaande uitleg "is een snelle schattingsalgoritme dat de waarden van de Shapegegevens in deep learning-modellen die gebruikmaakt van een verbinding met die worden beschreven in het document Shapegegevens NIPS DeepLIFT. TensorFlow-modellen en Keras-modellen met behulp van de TensorFlow back-end worden ondersteund (Er is ook voorlopige ondersteuning voor PyTorch) '.
* **Kernel Explainer**: De Shapegegevens Kernel uitleg maakt gebruik van een speciaal gewogen lokale lineaire regressie kunt u schatten Shapegegevens waarden voor een model.
* **Uitleg nabootsen**: Nabootsen uitleg is gebaseerd op het idee van globale surrogate modellen. Een globale surrogate-model is een intrinsiek interpreteerbare die naar schatting zo nauwkeurig mogelijk maken van de voorspellingen van een zwart vak model wordt getraind. Gegevenswetenschapper kan het model surrogate als u wilt tekenen conclusies over de zwarte doos-model worden geïnterpreteerd.
* **LICHTGROEN uitleg**: LICHTGROEN uitleg op basis van LICHTGROEN, gebruikt het status-of-the-art lokale interpreteerbare model-agnostische uitleg (LICHTGROEN)-algoritme om lokale surrogate modellen te maken. In tegenstelling tot de modellen globale surrogate is LICHTGROEN gericht op het lokale surrogate modellen om uit te leggen van afzonderlijke voorspellingen training.
* **HAN tekst uitleg**: HAN tekst uitleg gebruikt een hiërarchische aandacht-netwerk voor het ophalen van model uitleg van de gegevens voor een bepaalde zwarte doos tekst-model. We het model van de vervangende HAN op de verwachte uitvoer van een bepaalde docent-model te trainen. Na de training wereldwijd in het corpus tekst, hebben we een stap fine-tune voor een bepaald document toegevoegd om te verbeteren van de nauwkeurigheid van de uitleg. HAN maakt gebruik van een in twee richtingen RNN met twee lagen van aandacht, voor de zin en word aandacht. Zodra de DNN is getraind model van de docent en zorgvuldig afgestemd op een bepaald document, kunnen we de importances word extraheren uit de lagen aandacht. We hebben HAN nauwkeuriger is dan LICHTGROEN of Shapegegevens voor tekstgegevens, maar duurder in termen van training, evenals de tijd worden gevonden. We hebben echter verbeteringen in de trainingstijd gemaakt door middel van de gebruiker de optie voor het initialiseren van het netwerk met GloVe woordinsluitingen, hoewel deze nog steeds traag is. De trainingstijd kan aanzienlijk worden verbeterd door het uitvoeren van HAN op een externe Azure-GPU VM. De implementatie van HAN wordt beschreven in 'Hiërarchische aandacht netwerken voor Document-classificatie (Yang markt, 2016)' ([https://www.cs.cmu.edu/~diyiy/docs/naacl16.pdf](https://www.cs.cmu.edu/~diyiy/docs/naacl16.pdf)).

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

Modellen die zijn getraind op gegevenssets in Python `numpy.array`, `pandas.DataFrame`, `iml.datatypes.DenseData`, of `scipy.sparse.csr_matrix` indeling worden ondersteund door de Machine Learning Interpretability SDK.

De uitleg-functies accepteren pijplijnen en modellen als invoer. Als een model is opgegeven, het model moet worden gebruikt voor het implementeren van de voorspellingsfunctie `predict` of `predict_proba` die voldoet aan de Scikit-overeenkomst. Als een pijplijn (naam van het script pijplijn) is opgegeven, de functie uitleg wordt ervan uitgegaan dat het script uit te voeren pijplijn een voorspelling retourneert.

### <a name="local-and-remote-compute-target"></a>Lokale en externe compute-doel

De SDK Interpretability van Machine Learning is ontworpen voor gebruik met zowel lokale als externe compute-doelen. Als lokaal uitvoert, wordt de SDK-functies niet contact op met de Azure-services. U kunt een uitleg op afstand uitvoeren in Azure Machine Learning-Computing en meld u aan de gegevens van de uitleg bij Azure Machine Learning uitvoeren geschiedenis van Services. Zodra deze informatie wordt geregistreerd, zijn rapporten en visualisaties uit de uitleg gemakkelijk beschikbaar zijn op de Portal van Azure Machine Learning-werkruimte voor analyse van gebruikers.

## <a name="train-and-explain-locally"></a>Trainen en wordt uitgelegd lokaal

1. Uw model in een lokaal Jupyter-notitieblok te trainen. 

    ``` python
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

2. Roep de uitleg: Voor het starten van een object uitleg, moet u het model, de trainingsgegevens, de functies van belang (optioneel) en klassenamen uitvoer doorgeven (als classificatie) naar de uitleg. Hier wordt beschreven hoe u exemplaar maken van een uitleg object met [TabularExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.tabularexplainer?view=azure-ml-py), [MimicExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.mimic.mimicexplainer?view=azure-ml-py), en `LimeExplainer` lokaal. `TabularExplainer` roept een van de drie explainers onder (`TreeExplainer`, `DeepExplainer`, of `KernelExplainer`), en is automatisch selecteren van de meest geschikt is voor uw situatie. U kunt echter elk van de drie onderliggende explainers rechtstreeks aanroepen.

    ```python
    from azureml.explain.model.tabular_explainer import TabularExplainer
    explainer = TabularExplainer(model, x_train, features=breast_cancer_data.feature_names, classes=classes)
    ```
    of
    ```python
    from azureml.explain.model.mimic.mimic_explainer import MimicExplainer
    from azureml.explain.model.mimic.models.lightgbm_model import LGBMExplainableModel
    explainer = MimicExplainer(model, x_train, LGBMExplainableModel, features=breast_cancer_data.feature_names, classes=classes)
    ```

3. De algemene functie belang waarden ophalen.

    ```python
    # You can use the training data or the test data here. 
    global_explanation = explainer.explain_global(x_train)
    # Sorted feature importance values and feature names
    sorted_global_importance_values = global_explanation.get_ranked_global_values()
    sorted_global_importance_names = global_explanation.get_ranked_global_names()
    dict(zip(sorted_global_importance_names, sorted_global_importance_values))
    ```

4. Lokale functie belang waarden: Gebruik de volgende functieaanroepen om een afzonderlijk exemplaar of een groep exemplaren van te leggen.

    ```python
    # explain the first data point in the test set
    local_explanation = explainer.explain_local(x_test[0,:])
    
    # sorted feature importance values and feature names
    sorted_local_importance_names = local_explanation.get_ranked_local_names()
    sorted_local_importance_values = local_explanation.get_ranked_local_values()
    ```
    of
    ```python
    # explain the first five data points in the test set
    local_explanation = explainer.explain_local(x_test[0:4,:])
    
    # sorted feature importance values and feature names
    sorted_local_importance_names = local_explanation.get_ranked_local_names()
    sorted_local_importance_values = local_explanation.get_ranked_local_values()
    ```

## <a name="train-and-explain-remotely"></a>Trainen en wordt uitgelegd op afstand

Terwijl u op de diverse compute-doelen die door Azure Machine Learning-service worden ondersteund trainen kunt, ziet in het voorbeeld in deze sectie u hoe u dit doet met behulp van AMLCompute.

1. Maak een trainingsscript in een lokaal Jupyter-notitieblok (bijvoorbeeld run_explainer.py).

    ``` python  
    run = Run.get_context()
    client = ExplanationClient.from_run(run)
    
    # Train your model here

    # explain predictions on your local machine    
    explainer = TabularExplainer(model, x_train, features=breast_cancer_data.feature_names, classes=classes)
    # explain overall model predictions (global explanation)
    global_explanation = explainer.explain_global(x_test)
    # explain local data points (individual instances)
    local_explanation = explainer.explain_local(x_test[0,:])
    # upload global and local explanation objects to Run History
    upload_model_explanation(run, local_explanation, top_k=2, comment='local explanation: top 2 features')
    # Uploading global model explanation data for storage or visualization in webUX
    # The explanation can then be downloaded on any compute
    # Multiple explanations can be uploaded
    client.upload_model_explanation(global_explanation, comment='global explanation: all features')
    # Or you can only upload the explanation object with the top k feature info
    #client.upload_model_explanation(global_explanation, top_k=2, comment='global explanation: Only top 2 features')
    ```

2. Volg de instructies op [instellen van compute-doelen voor modeltraining](how-to-set-up-training-targets.md#amlcompute) voor meer informatie over het instellen van een Azure Machine Learning-Computing als uw compute-doel en het verzenden van uw training uitvoeren.

3. Download de uitleg in uw lokaal Jupyter-notitieblok. 
    > [!IMPORTANT]
    > Dingen in contrib niet volledig worden ondersteund. Zodra de experimentele functies volwassen, zullen ze geleidelijk worden verplaatst naar het belangrijkste pakket.

    ``` python
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

## <a name="next-steps"></a>Volgende stappen

Zie voor een verzameling van Jupyter-notitieblokken die laten zien van de bovenstaande instructies, de [Azure Machine Learning Interpretability voorbeeldnotitieblokken](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model).
