---
title: PyTorch-modellen trainen en registreren
titleSuffix: Azure Machine Learning service
description: Dit artikel leest u hoe te trainen en een PyTorch-model met behulp van Azure Machine Learning-service te registreren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.reviewer: peterlu
ms.date: 06/18/2019
ms.custom: seodec18
ms.openlocfilehash: d9c953eeecedf14a8f3fae43c5d4713252d58b4c
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840087"
---
# <a name="train-and-register-pytorch-models-at-scale-with-azure-machine-learning-service"></a>Trainen en PyTorch-modellen op schaal registreren met Azure Machine Learning-service

Dit artikel leest u hoe te trainen en een PyTorch-model met behulp van Azure Machine Learning-service te registreren. Deze gebaseerd op [PyTorch van overdracht learning-zelfstudie](https://pytorch.org/tutorials/beginner/transfer_learning_tutorial.html) die een deep neural network (DNN)-classificatie voor afbeeldingen van kuikens en kalkoenen bouwt.

[PyTorch](https://pytorch.org/) is een open-source-framework rekenkundige meestal gebruikt om (DNN) deep neural networks. Met Azure Machine Learning-service, kunt u snel schalen van open-source trainingstaken met behulp van flexibele cloud compute-resources. U kunt ook bijhouden uw trainingsuitvoeringen, de versie-modellen implementeren van modellen en nog veel meer.

Of u een PyTorch-model van de grond-up ontwikkelt of u een bestaand model naar de cloud brengen, kunt Azure Machine Learning-service u gereed is voor productie-modellen bouwen.

## <a name="prerequisites"></a>Vereisten

Deze code in elk van deze omgevingen worden uitgevoerd:

 - Azure Machine Learning-Notebook VM - geen downloads of installatie nodig

    - Voltooid de [cloud-gebaseerde notebook snelstartgids](quickstart-run-cloud-notebook.md) maken een toegewezen notebookserver vooraf geladen met de SDK en de opslagplaats met voorbeelden.
    - Zoeken in de map samples op de notebookserver, een laptop voltooide en uitgebreide door te navigeren naar deze map: **How-to-naar-gebruik-azureml > training met deep learning > train-hyperparameter-tune-deploy-with-pytorch** de map. 
 
 - Uw eigen Jupyter-Notebook-server

    - [De Azure Machine Learning-SDK voor Python installeren](setup-create-workspace.md#sdk)
    - [Het configuratiebestand van een werkruimte maken](setup-create-workspace.md#write-a-configuration-file)
    - [De voorbeeldbestanden script downloaden](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch) `pytorch_train.py`
     
    U vindt hier ook een voltooide [Jupyter-Notebook versie](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch/train-hyperparameter-tune-deploy-with-pytorch.ipynb) van deze handleiding op de pagina van de GitHub-voorbeelden. De notebook bevat uitgebreide secties die betrekking hebben op intelligente hyperparameter afstemmen, modelimplementatie en laptop widgets.

## <a name="set-up-the-experiment"></a>Instellen van het experiment

In deze sectie stelt u het trainingsexperiment door het laden van de vereiste python-pakketten, het initialiseren van een werkruimte, een experiment maken en uploaden van de trainingsgegevens en trainingsscripts.

### <a name="import-packages"></a>Pakketten importeren

Importeer eerst de benodigde Python-bibliotheken.

```Python
import os
import shutil

from azureml.core.workspace import Workspace
from azureml.core import Experiment

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
from azureml.train.dnn import PyTorch
```

### <a name="initialize-a-workspace"></a>Een werkruimte initialiseren

De [werkruimte van Azure Machine Learning-service](concept-workspace.md) is de resource op het hoogste niveau voor de service. Het biedt u met een centrale locatie voor het werken met alle artefacten die u maakt. In de Python-SDK, opent u de werkruimte-artefacten met het maken van een [ `workspace` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) object.

Maken van een werkruimteobject uit de `config.json` bestand dat is gemaakt de [sectie vereisten](#prerequisites).

```Python
ws = Workspace.from_config()
```

### <a name="create-an-experiment"></a>Een experiment maken

Een experiment en een map voor het opslaan van uw trainingsscripts maken. In dit voorbeeld maakt u een experiment met de naam 'pytorch-dieren'.

```Python
project_folder = './pytorch-birds'
os.makedirs(project_folder, exist_ok=True)

experiment_name = 'pytorch-birds'
experiment = Experiment(ws, name=experiment_name)
```

### <a name="get-the-data"></a>De gegevens ophalen

De gegevensset bestaat uit ongeveer 120 training-installatiekopieën voor kalkoenen en kuikens, waarbij 100 validatie-afbeeldingen voor elke categorie. We zullen downloaden en uitpakken van de gegevensset als onderdeel van onze trainingsscript `pytorch_train.py`. De afbeeldingen zijn een subset van de [afbeeldingen openen v5 gegevensset](https://storage.googleapis.com/openimages/web/index.html).

### <a name="prepare-training-scripts"></a>Trainingsscripts voorbereiden

In deze zelfstudie, het trainingsscript `pytorch_train.py`, is al opgegeven. In de praktijk, kunt u neemt een trainingsscript aangepaste, zoals is en de App uitvoeren met Azure Machine Learning-service.

Uploaden van het script van de training Pytorch `pytorch_train.py`.

```Python
shutil.copy('pytorch_train.py', project_folder)
```

Echter, als u wilt het gebruik van Azure Machine Learning-service volgen en mogelijkheden voor metrische gegevens, u moet een kleine hoeveelheid code binnen uw trainingsscript toevoegen. Voorbeelden van metrische gegevens bijhouden kunnen u vinden in `pytorch_train.py`.

## <a name="create-a-compute-target"></a>Een compute-doel maken

Maak een compute-doel voor uw PyTorch-taak uit te voeren op. In dit voorbeeld maakt u een Azure Machine Learning met GPU rekencluster.

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

## <a name="create-a-pytorch-estimator"></a>Een estimator PyTorch maken

De [PyTorch estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py) biedt een eenvoudige manier om een PyTorch-trainingstaak op een compute-doel starten.

De estimator PyTorch wordt geïmplementeerd via de algemene [ `estimator` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) klasse, die kan worden gebruikt voor de ondersteuning van elk gewenst framework. Zie voor meer informatie over het trainen van modellen met behulp van de algemene estimator [trainen van modellen met Azure Machine Learning met behulp van estimator](how-to-train-ml-models.md)

Als uw trainingsscript moet extra pip of conda-pakketten om uit te voeren, kunt u hebt de pakketten die zijn geïnstalleerd op de resulterende docker-installatiekopie door door te geven hun namen door middel van de `pip_packages` en `conda_packages` argumenten.

```Python
script_params = {
    '--num_epochs': 30,
    '--output_dir': './outputs'
}

estimator = PyTorch(source_directory=project_folder, 
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='pytorch_train.py',
                    use_gpu=True,
                    pip_packages=['pillow==5.4.1'])
```

## <a name="submit-a-run"></a>Een uitvoering verzenden

De [object uitvoeren](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) biedt de interface voor de uitvoeringsgeschiedenis terwijl de taak wordt uitgevoerd en wanneer deze is voltooid.

```Python
run = experiment.submit(estimator)
run.wait_for_completion(show_output=True)
```

Als de uitvoering wordt uitgevoerd, er wordt de volgende fasen:

- **Voorbereiden van**: Een docker-installatiekopie wordt gemaakt op basis van de estimator PyTorch. De afbeelding is geüpload naar het containerregister van de werkruimte en hoger wordt uitgevoerd in de cache opgeslagen. Logboeken ook worden gestreamd naar de uitvoeringsgeschiedenis en kunnen worden weergegeven om voortgang te controleren.

- **Schalen**: Het cluster probeert om omhoog te schalen als de Batch AI-cluster meer knooppunten vereist voor het uitvoeren van de uitvoering dan er op dit moment beschikbaar zijn.

- **Uitvoeren**: Alle scripts in de scriptmap worden geüpload naar de compute-doel, data-archieven worden gekoppeld of worden gekopieerd en de entry_script wordt uitgevoerd. Uitvoer van stdout en. / map logboeken worden gestreamd naar de uitvoeringsgeschiedenis en kan worden gebruikt voor het bewaken van de uitvoering.

- **Nabewerken**: De. / map van de uitvoering is gekopieerd naar de uitvoeringsgeschiedenis levert.

## <a name="register-or-download-a-model"></a>Registreren of te downloaden van een model

Nadat u het model hebt getraind, kunt u deze kunt registreren in uw werkruimte. Registratie van het model kunt u store en versie uw modellen in de werkruimte voor het vereenvoudigen van [model voor beheer en de implementatie](concept-model-management-and-deployment.md).

```Python
model = run.register_model(model_name='pt-dnn', model_path='outputs/')
```

U kunt ook een lokale kopie van het model downloaden met behulp van het object uitvoeren. In het trainingsscript `pytorch_train.py`, een PyTorch opslaan object zich blijft voordoen het model naar een lokale map (lokaal naar de compute-doel). U kunt de Run-object gebruiken om een kopie te downloaden.

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

De [ `PyTorch` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py) estimator biedt ook ondersteuning voor gedistribueerde training over CPU en GPU-clusters. U kunt eenvoudig gedistribueerde PyTorch-taken uitvoeren en Azure Machine Learning-service de indeling u wilt beheren.

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) is een open-source, alle verminderen framework voor gedistribueerde cursussen ontwikkeld door Uber. Het biedt een eenvoudige manier gedistribueerde GPU PyTorch-taken.

Voor het gebruik van Horovod, Geef een [ `MpiConfiguration` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration?view=azure-ml-py) -object voor de `distributed_training` parameter in de constructor PyTorch. Deze parameter zorgt ervoor dat Horovod bibliotheek is geïnstalleerd voor gebruik in uw trainingsscript.


```Python
from azureml.train.dnn import PyTorch

estimator= PyTorch(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_training=MpiConfiguration(),
                      framework_version='1.13',
                      use_gpu=True)
```
Horovod en de bijbehorende afhankelijkheden wordt geïnstalleerd, zodat u deze in uw trainingsscript importeren kunt `train.py` als volgt:

```Python
import torch
import horovod
```
## <a name="export-to-onnx"></a>Exporteren naar de ONNX

Om te optimaliseren interferentie met de [ONNX-Runtime](concept-onnx.md), het getrainde model voor PyTorch converteren naar de ONNX-indeling. Deductie of het model scoren, is de fase waarin het gedistribueerde model wordt gebruikt voor voorspellingen, meestal op productiegegevens. Zie de [zelfstudie](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb) voor een voorbeeld.

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt getraind en een PyTorch-model van Azure Machine Learning-service geregistreerd. Ga verder dan het model implementeren-artikel voor meer informatie over het implementeren van een model.

> [!div class="nextstepaction"]
> [Hoe en waar u kunt modellen implementeren](how-to-deploy-and-where.md)
* [Metrische gegevens over uitvoeren tijdens de training bijhouden](how-to-track-experiments.md)
* [Afstemmen van hyperparameters](how-to-tune-hyperparameters.md)
* [Een getraind model implementeren](how-to-deploy-and-where.md)
