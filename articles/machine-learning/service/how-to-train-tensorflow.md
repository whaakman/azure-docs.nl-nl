---
title: TensorFlow-modellen trainen en registreren
titleSuffix: Azure Machine Learning service
description: Dit artikel leest u hoe te trainen en een TensorFlow-model met behulp van Azure Machine Learning-service te registreren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.date: 06/10/2019
ms.custom: seodec18
ms.openlocfilehash: 67263df319063cdf21dadea257dcab05ba0d5f7b
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839999"
---
# <a name="train-and-register-tensorflow-models-at-scale-with-azure-machine-learning-service"></a>Trainen en TensorFlow-modellen op schaal registreren met Azure Machine Learning-service

Dit artikel leest u hoe te trainen en een TensorFlow-model met behulp van Azure Machine Learning-service te registreren. Hierbij de populaire [MNIST gegevensset](http://yann.lecun.com/exdb/mnist/) voor het classificeren van handgeschreven cijfers met behulp van een deep neural network is gebouwd met behulp van de [TensorFlow Python-bibliotheek](https://www.tensorflow.org/overview).

TensorFlow is een open source-framework rekenkundige meestal gebruikt om (DNN) deep neural networks. Met Azure Machine Learning-service, kunt u snel schalen van open-source trainingstaken met behulp van flexibele cloud compute-resources. U kunt ook bijhouden uw trainingsuitvoeringen, de versie-modellen implementeren van modellen en nog veel meer.

Of u een TensorFlow-model van de grond-up ontwikkelt of je een [bestaande model](how-to-deploy-existing-model.md) naar de cloud, Azure Machine Learning-service kunt u gereed is voor productie-modellen bouwen.

## <a name="prerequisites"></a>Vereisten

Deze code in elk van deze omgevingen worden uitgevoerd:

 - Azure Machine Learning-Notebook VM - geen downloads of installatie nodig

     - Voltooid de [cloud-gebaseerde notebook snelstartgids](quickstart-run-cloud-notebook.md) maken een toegewezen notebookserver vooraf geladen met de SDK en de opslagplaats met voorbeelden.
    - Zoeken in de map samples op de notebookserver, een laptop voltooide en uitgebreide door te navigeren naar deze map: **How-to-naar-gebruik-azureml > training met deep learning > train-hyperparameter-tune-deploy-with-tensorflow**map. 
 
 - Uw eigen Jupyter-Notebook-server

     - [De Azure Machine Learning-SDK voor Python installeren](setup-create-workspace.md#sdk)
    - [Het configuratiebestand van een werkruimte maken](setup-create-workspace.md#write-a-configuration-file)
    - [Downloaden van de voorbeeld-scriptbestanden](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow) `mnist-tf.py` en `utils.py`
     
    U vindt hier ook een voltooide [Jupyter-Notebook versie](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow/train-hyperparameter-tune-deploy-with-tensorflow.ipynb) van deze handleiding op de pagina van de GitHub-voorbeelden. De notebook bevat uitgebreide secties die betrekking hebben op intelligente hyperparameter afstemmen, modelimplementatie en laptop widgets.

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

Een experiment en een map voor het opslaan van uw trainingsscripts maken. In dit voorbeeld maakt u een experiment met de naam 'tf-mnist'.

```Python
script_folder = './tf-mnist'
os.makedirs(script_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='tf-mnist')
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

1. Uploaden van het script TensorFlow-training `tf_mnist.py`, en het helperbestand `utils.py`.

    ```Python
    shutil.copy('./tf_mnist.py', script_folder)
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

## <a name="create-a-tensorflow-estimator"></a>Een estimator TensorFlow maken

De [TensorFlow estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) biedt een eenvoudige manier om een taak TensorFlow-training op een compute-doel starten.

De estimator TensorFlow wordt geïmplementeerd via de algemene [ `estimator` ](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) klasse, die kan worden gebruikt voor de ondersteuning van elk gewenst framework. Zie voor meer informatie over het trainen van modellen met behulp van de algemene estimator [trainen van modellen met Azure Machine Learning met behulp van estimator](how-to-train-ml-models.md)

Als uw trainingsscript moet extra pip of conda-pakketten om uit te voeren, kunt u hebt de pakketten die zijn geïnstalleerd op de resulterende docker-installatiekopie door door te geven hun namen door middel van de `pip_packages` en `conda_packages` argumenten.

```Python
script_params = {
    '--data-folder': ws.get_default_datastore().as_mount(),
    '--batch-size': 50,
    '--first-layer-neurons': 300,
    '--second-layer-neurons': 100,
    '--learning-rate': 0.01
}

est = TensorFlow(source_directory=script_folder,
                 entry_script='tf_mnist.py',
                 script_params=script_params,
                 compute_target=compute_target,
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

## <a name="register-or-download-a-model"></a>Registreren of te downloaden van een model

Nadat u het model hebt getraind, kunt u deze kunt registreren in uw werkruimte. Registratie van het model kunt u store en versie uw modellen in de werkruimte voor het vereenvoudigen van [model voor beheer en de implementatie](concept-model-management-and-deployment.md).

```Python
model = run.register_model(model_name='tf-dnn-mnist', model_path='outputs/model')
```

U kunt ook een lokale kopie van het model downloaden met behulp van het object uitvoeren. In het trainingsscript `mnist-tf.py`, een object van de schermbeveiliging TensorFlow zich blijft voordoen het model naar een lokale map (lokaal naar de compute-doel). U kunt de Run-object gebruiken om een kopie te downloaden.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

for f in run.get_file_names():
    if f.startswith('outputs/model'):
        output_file_path = os.path.join('./model', f.split('/')[-1])
        print('Downloading from {} to {} ...'.format(f, output_file_path))
        run.download_file(name=f, output_file_path=output_file_path)
```

## <a name="distributed-training"></a>Gedistribueerde training

De [ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) estimator biedt ook ondersteuning voor gedistribueerde training over CPU en GPU-clusters. U kunt eenvoudig gedistribueerde TensorFlow-taken uitvoeren en Azure Machine Learning-service de indeling u wilt beheren.

Azure Machine Learning-service ondersteunt twee methoden van gedistribueerde training in TensorFlow:

- [Op basis van een MPI](https://www.open-mpi.org/) gedistribueerd training met behulp van de [Horovod](https://github.com/uber/horovod) framework
- Systeemeigen [gedistribueerde TensorFlow](https://www.tensorflow.org/deploy/distributed) met behulp van de methode van de parameter-server

### <a name="horovod"></a>Horovod

[Horovod](https://github.com/uber/horovod) is een open-source framework voor gedistribueerde cursussen ontwikkeld door Uber. Het biedt een eenvoudige manier gedistribueerde TensorFlow GPU-taken.

Voor het gebruik van Horovod, Geef een [ `MpiConfiguration` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration?view=azure-ml-py) -object voor de `distributed_training` parameter in de constructor TensorFlow. Deze parameter zorgt ervoor dat Horovod bibliotheek is geïnstalleerd voor gebruik in uw trainingsscript.

```Python
from azureml.train.dnn import TensorFlow

# Tensorflow constructor
estimator= TensorFlow(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_training=MpiConfiguration(),
                      framework_version='1.13',
                      use_gpu=True)
```

### <a name="parameter-server"></a>Parameterserver

U kunt ook uitvoeren [native gedistribueerde TensorFlow](https://www.tensorflow.org/deploy/distributed), waarbij het servermodel parameter worden gebruikt. Bij deze methode hoeft trainen u in een cluster van de parameter-servers en werknemers. De werknemers Bereken de verlopen tijdens de training, terwijl de parameter-servers de verlopen aggregeren.

Geef voor het gebruik van de methode van de parameter-server, een [ `TensorflowConfiguration` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.tensorflowconfiguration?view=azure-ml-py) -object voor de `distributed_training` parameter in de constructor TensorFlow.

```Python
from azureml.train.dnn import TensorFlow

distributed_training = TensorflowConfiguration()
distributed_training.worker_count = 2

# Tensorflow constructor
estimator= TensorFlow(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_training=distributed_training,
                      use_gpu=True)

# submit the TensorFlow job
run = exp.submit(tf_est)
```

#### <a name="define-cluster-specifications-in-tfconfig"></a>Cluster-specificaties in 'TF_CONFIG' definiëren

U moet ook de netwerkadressen en poorten van het cluster voor de [ `tf.train.ClusterSpec` ](https://www.tensorflow.org/api_docs/python/tf/train/ClusterSpec), zodat Azure Machine Learning wordt de `TF_CONFIG` omgevingsvariabele voor u.

De `TF_CONFIG` omgevingsvariabele is een JSON-tekenreeks. Hier volgt een voorbeeld van de variabele voor de parameterserver:

```JSON
TF_CONFIG='{
    "cluster": {
        "ps": ["host0:2222", "host1:2222"],
        "worker": ["host2:2222", "host3:2222", "host4:2222"],
    },
    "task": {"type": "ps", "index": 0},
    "environment": "cloud"
}'
```

Voor de hoge mate van TensorFlow [ `tf.estimator` ](https://www.tensorflow.org/api_docs/python/tf/estimator) API, TensorFlow parseert de `TF_CONFIG` variabele en het cluster specificaties voor u builds.

Parseren voor TensorFlow van lager niveau core API's voor training, de `TF_CONFIG` variabele en bouwen de `tf.train.ClusterSpec` in uw trainingen-code.

```Python
import os, json
import tensorflow as tf

tf_config = os.environ.get('TF_CONFIG')
if not tf_config or tf_config == "":
    raise ValueError("TF_CONFIG not found.")
tf_config_json = json.loads(tf_config)
cluster_spec = tf.train.ClusterSpec(cluster)

```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt getraind en een model TensorFlow geregistreerd. Ga verder met onze GPU model implementatie artikel voor informatie over het implementeren van een model in een cluster met GPU.

[Over het implementeren van voor inferentietaken met GPU's](how-to-deploy-inferencing-gpus.md)
[met Tensorboard controleren](how-to-monitor-tensorboard.md)
