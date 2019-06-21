---
title: Training en registreren van Keras-modellen die worden uitgevoerd op TensorFlow
titleSuffix: Azure Machine Learning service
description: Dit artikel leest u hoe te trainen en een die worden uitgevoerd op TensorFlow Keras-model te registreren met behulp van Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.date: 06/07/2019
ms.custom: seodec18
ms.openlocfilehash: e070b80f86cb6c8b1d9e7575e19022b5cb08f340
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165557"
---
# <a name="train-and-register-keras-models-at-scale-with-azure-machine-learning-service"></a>Trainen en Keras-modellen op schaal registreren met Azure Machine Learning-service

Dit artikel leest u hoe te trainen en een Keras-model dat is gebouwd op TensorFlow te registreren met behulp van Azure Machine Learning-service. Hierbij de populaire [MNIST gegevensset](http://yann.lecun.com/exdb/mnist/) voor het classificeren van handgeschreven cijfers met behulp van een deep neural network (DNN) die zijn gemaakt met de [Keras Python-bibliotheek](https://keras.io) uitgevoerd boven [TensorFlow](https://www.tensorflow.org/overview) .

Keras is een op hoog niveau neural network API kan worden uitgevoerd boven aan de andere populaire DNN-frameworks om ontwikkeling te vereenvoudigen. Met Azure Machine Learning-service, kunt u snel met behulp van flexibele cloud-rekenresources trainingstaken uitschalen. U kunt ook bijhouden uw trainingsuitvoeringen, de versie-modellen implementeren van modellen en nog veel meer.

Of u een Keras-model van de grond-up ontwikkelt of u een bestaand model naar de cloud brengen, kunt Azure Machine Learning-service u gereed is voor productie-modellen bouwen.

## <a name="prerequisites"></a>Vereisten

Deze code in elk van deze omgevingen worden uitgevoerd:

 - Azure Machine Learning-Notebook VM - geen downloads of installatie nodig

     - Voltooid de [cloud-gebaseerde notebook snelstartgids](quickstart-run-cloud-notebook.md) maken een toegewezen notebookserver vooraf geladen met de SDK en de opslagplaats met voorbeelden.
    - Zoeken in de map samples op de notebookserver, een laptop voltooide en uitgebreide door te navigeren naar deze map: **How-to-naar-gebruik-azureml > training met deep learning > train-hyperparameter-tune-deploy-with-keras** de map. 
 
 - Uw eigen Jupyter-Notebook-server

     - [De Azure Machine Learning-SDK voor Python installeren](setup-create-workspace.md#sdk)
    - [Het configuratiebestand van een werkruimte maken](setup-create-workspace.md#write-a-configuration-file)
    - [Downloaden van de voorbeeld-scriptbestanden](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras) `mnist-keras.py` en `utils.py`
     
    U vindt hier ook een voltooide [Jupyter-Notebook versie](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras/train-hyperparameter-tune-deploy-with-keras.ipynb) van deze handleiding op de pagina van de GitHub-voorbeelden. De notebook bevat uitgebreide secties die betrekking hebben op intelligente hyperparameter afstemmen, modelimplementatie en laptop widgets.

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

Een experiment en een map voor het opslaan van uw trainingsscripts maken. In dit voorbeeld maakt u een experiment met de naam 'keras-mnist'.

```Python
script_folder = './keras-mnist'
os.makedirs(script_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='keras-mnist')
```

### <a name="upload-dataset-and-scripts"></a>Gegevensset en -scripts uploaden

De [gegevensopslag](how-to-access-data.md) is een plek waar gegevens kunnen worden opgeslagen en geopend door te koppelen of kopiëren van de gegevens naar de compute-doel. Elke werkruimte biedt een standaard-gegevensopslag. Upload de gegevens en trainingsscripts naar het gegevensarchief zodat ze eenvoudig kunnen worden geopend tijdens de training.

1. Download de MNIST-gegevensset lokaal.

    ```Python
    os.makedirs('./data/mnist', exist_ok=True)

    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename = './data/mnist/train-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename = './data/mnist/train-labels.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename = './data/mnist/test-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename = './data/mnist/test-labels.gz')
    ```

1. Upload de MNIST-gegevensset naar de standaard-gegevensopslag.

    ```Python
    ds = ws.get_default_datastore()
    ds.upload(src_dir='./data/mnist', target_path='mnist', overwrite=True, show_progress=True)
    ```

1. Uploaden van het script van de training Keras `keras_mnist.py`, en het helperbestand `utils.py`.

    ```Python
    shutil.copy('./keras_mnist.py', script_folder)
    shutil.copy('./utils.py', script_folder)
    ```

## <a name="create-a-compute-target"></a>Een compute-doel maken

Maak een compute-doel voor uw TensorFlow-taak uit te voeren op. In dit voorbeeld maakt u een Azure Machine Learning met GPU rekencluster.

```Python
cluster_name = "gpucluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

Zie voor meer informatie over de compute-doelen, de [wat is er een compute-doel](concept-compute-target.md) artikel.

## <a name="create-a-tensorflow-estimator-and-import-keras"></a>Maken van een estimator TensorFlow en Keras importeren

De [TensorFlow estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) biedt een eenvoudige manier van TensorFlow-trainingstaken op compute-doel starten. Omdat Keras wordt uitgevoerd boven op TensorFlow, kunt u de estimator TensorFlow gebruiken en importeer de Keras-bibliotheek met de `pip_packages` argument.

De estimator TensorFlow wordt geïmplementeerd via de algemene [ `estimator` ](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) klasse, die kan worden gebruikt voor de ondersteuning van elk gewenst framework. Zie voor meer informatie over het trainen van modellen met behulp van de algemene estimator [trainen van modellen met Azure Machine Learning met behulp van estimator](how-to-train-ml-models.md)

```Python
script_params = {
    '--data-folder': ds.path('mnist').as_mount(),
    '--batch-size': 50,
    '--first-layer-neurons': 300,
    '--second-layer-neurons': 100,
    '--learning-rate': 0.001
}

est = TensorFlow(source_directory=script_folder,
                 entry_script='keras_mnist.py',
                 script_params=script_params,
                 compute_target=compute_target,
                 pip_packages=['keras', 'matplotlib'],
                 use_gpu=True)
```

## <a name="submit-a-run"></a>Een uitvoering verzenden

De [object uitvoeren](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) biedt de interface voor de uitvoeringsgeschiedenis terwijl de taak wordt uitgevoerd en wanneer deze is voltooid.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Als de uitvoering wordt uitgevoerd, er wordt de volgende fasen:

- **Voorbereiden van**: Een docker-installatiekopie wordt gemaakt op basis van de estimator TensorFlow. De afbeelding is geüpload naar het containerregister van de werkruimte en hoger wordt uitgevoerd in de cache opgeslagen. Logboeken ook worden gestreamd naar de uitvoeringsgeschiedenis en kunnen worden weergegeven om voortgang te controleren.

- **Schalen**: Het cluster probeert om omhoog te schalen als de Batch AI-cluster meer knooppunten vereist voor het uitvoeren van de uitvoering dan er op dit moment beschikbaar zijn.

- **Uitvoeren**: Alle scripts in de scriptmap worden geüpload naar de compute-doel, data-archieven worden gekoppeld of worden gekopieerd en de entry_script wordt uitgevoerd. Uitvoer van stdout en. / map logboeken worden gestreamd naar de uitvoeringsgeschiedenis en kan worden gebruikt voor het bewaken van de uitvoering.

- **Nabewerken**: De. / map van de uitvoering is gekopieerd naar de uitvoeringsgeschiedenis levert.

## <a name="register-the-model"></a>Registreer het model

Nadat u het model hebt getraind, kunt u deze kunt registreren in uw werkruimte. Registratie van het model kunt u store en versie uw modellen in de werkruimte voor het vereenvoudigen van [model voor beheer en de implementatie](concept-model-management-and-deployment.md).

```Python
model = run.register_model(model_name='keras-dnn-mnist', model_path='outputs/model')
```

U kunt ook een lokale kopie van het model downloaden. Dit kan nuttig zijn voor het uitvoeren van extra model validatie werk lokaal zijn. In het trainingsscript `mnist-keras.py`, een object van de schermbeveiliging TensorFlow zich blijft voordoen het model naar een lokale map (lokaal naar de compute-doel). Een kopie te downloaden van gegevensopslag kunt u het object uitvoeren.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

for f in run.get_file_names():
    if f.startswith('outputs/model'):
        output_file_path = os.path.join('./model', f.split('/')[-1])
        print('Downloading from {} to {} ...'.format(f, output_file_path))
        run.download_file(name=f, output_file_path=output_file_path)
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt getraind en een Keras-model van Azure Machine Learning-service geregistreerd. Ga verder dan het model implementeren-artikel voor meer informatie over het implementeren van een model.

> [!div class="nextstepaction"]
> [Hoe en waar u kunt modellen implementeren](how-to-deploy-and-where.md)
