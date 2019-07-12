---
title: Training en registreren van scikit-meer modellen
titleSuffix: Azure Machine Learning service
description: Dit artikel leest u hoe u trainen en registreert een scikit-meer model gemaakt met behulp van Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.date: 06/30/2019
ms.custom: seodec18
ms.openlocfilehash: c9e983f7981c1155964617694d2cce86aba741b7
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840025"
---
# <a name="train-and-register-scikit-learn-models-at-scale-with-azure-machine-learning-service"></a>Trainen en Scikit meer modellen op schaal registreren met Azure Machine Learning-service

Dit artikel leest u hoe te trainen en een Scikit meer model met behulp van Azure Machine Learning-service te registreren. Hierbij de populaire [Iris-gegevensset](https://archive.ics.uci.edu/ml/datasets/iris) voor het classificeren van iris bloem installatiekopieën met de aangepaste [scikit-meer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) klasse.

Scikit-informatie is een open source-framework rekenkundige doorgaans gebruikt voor het machine learning. Met Azure Machine Learning-service, kunt u snel schalen van open-source trainingstaken met behulp van flexibele cloud compute-resources. U kunt ook bijhouden uw trainingsuitvoeringen, de versie-modellen implementeren van modellen en nog veel meer.

Of u een model Scikit meer vanaf de grond van ontwikkelt of u een bestaand model naar de cloud brengen, kunt Azure Machine Learning-service u gereed is voor productie-modellen bouwen.

## <a name="prerequisites"></a>Vereisten

Deze code in elk van deze omgevingen worden uitgevoerd:
 - Azure Machine Learning-Notebook VM - geen downloads of installatie nodig

    - Voltooid de [cloud-gebaseerde notebook snelstartgids](quickstart-run-cloud-notebook.md) maken een toegewezen notebookserver vooraf geladen met de SDK en de opslagplaats met voorbeelden.
    - Zoeken in de map samples op de notebookserver, een laptop voltooide en uitgebreide door te navigeren naar deze map: **How-to-naar-gebruik-azureml > training > train-hyperparameter-tune-deploy-with-sklearn** map.

 - Uw eigen Jupyter-Notebook-server

    - [De Azure Machine Learning-SDK voor Python installeren](setup-create-workspace.md#sdk)
    - [Het configuratiebestand van een werkruimte maken](setup-create-workspace.md#write-a-configuration-file)
    - [Het script-voorbeeldbestand downloaden](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/train-hyperparameter-tune-deploy-with-sklearn) `train_iris.py`
    - U vindt hier ook een voltooide [Jupyter-Notebook versie](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-hyperparameter-tune-deploy-with-keras/train-hyperparameter-tune-deploy-with-sklearn.ipynb) van deze handleiding op de pagina van de GitHub-voorbeelden. De notebook bevat een uitgebreide sectie die betrekking hebben op intelligente hyperparameter afstemmen en het beste model met primaire metrische gegevens worden opgehaald.

## <a name="set-up-the-experiment"></a>Instellen van het experiment

In deze sectie stelt u het trainingsexperiment door het laden van de vereiste python-pakketten, het initialiseren van een werkruimte, een experiment maken en uploaden van de trainingsgegevens en trainingsscripts.

### <a name="import-packages"></a>Pakketten importeren

Importeer eerst de benodigde Python-bibliotheken.

```Python
import os
import urllib
import shutil
import azureml

from azureml.core import Experiment
from azureml.core import Workspace, Run

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>Een werkruimte initialiseren

De [werkruimte van Azure Machine Learning-service](concept-workspace.md) is de resource op het hoogste niveau voor de service. Het biedt u met een centrale locatie voor het werken met alle artefacten die u maakt. In de Python-SDK, opent u de werkruimte-artefacten met het maken van een [ `workspace` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) object.

Maken van een werkruimteobject uit de `config.json` bestand dat is gemaakt de [sectie vereisten](#prerequisites).

```Python
ws = Workspace.from_config()
```

### <a name="create-an-experiment"></a>Een experiment maken

Een experiment en een map voor het opslaan van uw trainingsscripts maken. In dit voorbeeld maakt u een experiment met de naam 'sklearn-iris'.

```Python
project_folder = './sklearn-iris'
os.makedirs(project_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='sklearn-iris')
```

### <a name="upload-dataset-and-scripts"></a>Gegevensset en -scripts uploaden

De [gegevensopslag](how-to-access-data.md) is een plek waar gegevens kunnen worden opgeslagen en geopend door te koppelen of kopiëren van de gegevens naar de compute-doel. Elke werkruimte biedt een standaard-gegevensopslag. Upload de gegevens en trainingsscripts naar het gegevensarchief zodat ze eenvoudig kunnen worden geopend tijdens de training.

1. Maak de map voor uw gegevens.

    ```Python
    os.makedirs('./data/iris', exist_ok=True)
    ```

1. Upload de iris-gegevensset naar de standaard-gegevensopslag.

    ```Python
    ds = ws.get_default_datastore()
    ds.upload(src_dir='./data/iris', target_path='iris', overwrite=True, show_progress=True)
    ```

1. Upload de Scikit-meer trainingsscript, `train_iris.py`.

    ```Python
    shutil.copy('./train_iris.py', project_folder)
    ```

## <a name="create-or-get-a-compute-target"></a>Maken of ophalen van een compute-doel

Maak een compute-doel voor de taak Scikit-informatie uit te voeren op. Scikit alleen ondersteunt één knooppunt, meer CPU computing.

De volgende code maakt een Azure Machine Learning beheerd compute (AmlCompute) voor uw externe training compute-resource. Het maken van AmlCompute plaats ongeveer 5 minuten. Als de AmlCompute met die naam al in uw werkruimte is, slaat deze code over het aanmaakproces.

```Python
cluster_name = "cpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

Zie voor meer informatie over de compute-doelen, de [wat is er een compute-doel](concept-compute-target.md) artikel.

## <a name="create-a-scikit-learn-estimator"></a>Maken van een estimator Scikit-informatie

De [Scikit meer estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) biedt een eenvoudige manier van het starten van een Scikit-meer trainingstaak op een compute-doel. Deze is geïmplementeerd via de [ `SKLearn` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) klasse, die kan worden gebruikt voor de ondersteuning van één knooppunt CPU-training.

Als uw trainingsscript moet extra pip of conda-pakketten om uit te voeren, kunt u hebt de pakketten die zijn geïnstalleerd op de resulterende docker-installatiekopie door door te geven hun namen door middel van de `pip_packages` en `conda_packages` argumenten.

```Python
from azureml.train.sklearn import SKLearn

script_params = {
    '--kernel': 'linear',
    '--penalty': 1.0,
}

estimator = SKLearn(source_directory=project_folder, 
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='train_iris.py'
                    pip_packages=['joblib']
                   )
```

## <a name="submit-a-run"></a>Een uitvoering verzenden

De [object uitvoeren](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) biedt de interface voor de uitvoeringsgeschiedenis terwijl de taak wordt uitgevoerd en wanneer deze is voltooid.

```Python
run = experiment.submit(estimator)
run.wait_for_completion(show_output=True)
```

Als de uitvoering wordt uitgevoerd, er wordt de volgende fasen:

- **Voorbereiden van**: Een docker-installatiekopie wordt gemaakt op basis van de estimator TensorFlow. De afbeelding is geüpload naar het containerregister van de werkruimte en hoger wordt uitgevoerd in de cache opgeslagen. Logboeken ook worden gestreamd naar de uitvoeringsgeschiedenis en kunnen worden weergegeven om voortgang te controleren.

- **Schalen**: Het cluster probeert om omhoog te schalen als de Batch AI-cluster meer knooppunten vereist voor het uitvoeren van de uitvoering dan er op dit moment beschikbaar zijn.

- **Uitvoeren**: Alle scripts in de scriptmap worden geüpload naar de compute-doel, data-archieven worden gekoppeld of worden gekopieerd en de entry_script wordt uitgevoerd. Uitvoer van stdout en. / map logboeken worden gestreamd naar de uitvoeringsgeschiedenis en kan worden gebruikt voor het bewaken van de uitvoering.

- **Nabewerken**: De. / map van de uitvoering is gekopieerd naar de uitvoeringsgeschiedenis levert.

## <a name="save-and-register-the-model"></a>Opslaan en Registreer het model

Nadat u het model hebt getraind, kunt u deze kunt opslaan en deze aan uw werkruimte te registreren. Registratie van het model kunt u store en versie uw modellen in de werkruimte voor het vereenvoudigen van [model voor beheer en de implementatie](concept-model-management-and-deployment.md).

Voeg de volgende code naar uw trainingsscript, train_iris.py, om op te slaan van het model. 

``` Python
import joblib

joblib.dump(svm_model_linear, 'model.joblib')
```

Registreer het model aan uw werkruimte met de volgende code.

```Python
model = run.register_model(model_name='sklearn-iris', model_path='model.joblib')
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt getraind en een model Scikit-informatie voor Azure Machine Learning-service geregistreerd.

* Als u wilt weten hoe u een model implementeren, gaat u naar onze [deployment model](how-to-deploy-and-where.md) artikel.

* [Afstemmen van hyperparameters](how-to-tune-hyperparameters.md)

* [Metrische gegevens over uitvoeren tijdens de training bijhouden](how-to-track-experiments.md)