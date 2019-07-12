---
title: Trainen en Chainer modellen registreren
titleSuffix: Azure Machine Learning service
description: Dit artikel leest u hoe te trainen en een Chainer-model met behulp van Azure Machine Learning-service te registreren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.reviewer: sdgilley
ms.date: 06/15/2019
ms.openlocfilehash: 8ecefccbdf5f02652e935858b6ae8fb4cdfde640
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840040"
---
# <a name="train-and-register-chainer-models-at-scale-with-azure-machine-learning-service"></a>Trainen en Chainer modellen op schaal registreren met Azure Machine Learning-service

Dit artikel leest u hoe te trainen en een Chainer-model met behulp van Azure Machine Learning-service te registreren. Hierbij de populaire [MNIST gegevensset](http://yann.lecun.com/exdb/mnist/) voor het classificeren van handgeschreven cijfers met behulp van een deep neural network (DNN) die zijn gemaakt met de [Chainer Python-bibliotheek](https://Chainer.org) uitgevoerd boven [numpy](https://www.numpy.org/).

Chainer is een op hoog niveau neural network API kan worden uitgevoerd op de hoogte van andere populaire DNN-frameworks om ontwikkeling te vereenvoudigen. Met Azure Machine Learning-service, kunt u snel met behulp van flexibele cloud-rekenresources trainingstaken uitschalen. U kunt ook bijhouden uw trainingsuitvoeringen, de versie-modellen implementeren van modellen en nog veel meer.

Of u een Chainer-model van de grond-up ontwikkelt of u een bestaand model naar de cloud brengen, kunt Azure Machine Learning-service u gereed is voor productie-modellen bouwen.

Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

## <a name="prerequisites"></a>Vereisten

Deze code in elk van deze omgevingen worden uitgevoerd:

- Azure Machine Learning-Notebook VM - geen downloads of installatie nodig

    - Voltooid de [cloud-gebaseerde notebook snelstartgids](quickstart-run-cloud-notebook.md) maken een toegewezen notebookserver vooraf geladen met de SDK en de opslagplaats met voorbeelden.
    - Zoeken in de map samples op de notebookserver, een voltooide laptop- en bestanden in de **how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer** map.  De notebook bevat uitgebreide secties die betrekking hebben op intelligente hyperparameter afstemmen, modelimplementatie en laptop widgets.

- Uw eigen Jupyter-Notebook-server

    - [De Azure Machine Learning-SDK voor Python installeren](setup-create-workspace.md#sdk)
    - [Het configuratiebestand van een werkruimte maken](setup-create-workspace.md#write-a-configuration-file)
    - Download het script-voorbeeldbestand [chainer_mnist.py](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/chainer_mnist.py)
     - U vindt hier ook een voltooide [Jupyter-Notebook versie](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb) van deze handleiding op de pagina met GitHub-voorbeelden. De notebook bevat uitgebreide secties die betrekking hebben op intelligente hyperparameter afstemmen, modelimplementatie en laptop widgets.

## <a name="set-up-the-experiment"></a>Instellen van het experiment

In deze sectie stelt u het trainingsexperiment door het laden van de vereiste python-pakketten, het initialiseren van een werkruimte, een experiment maken en uploaden van de trainingsgegevens en trainingsscripts.

### <a name="import-packages"></a>Pakketten importeren

Importeer eerst de azureml.core Python-bibliotheek ad weergave het versienummer.

```
# Check core SDK version number
import azureml.core

print("SDK version:", azureml.core.VERSION)
```

### <a name="initialize-a-workspace"></a>Een werkruimte initialiseren

De [werkruimte van Azure Machine Learning-service](concept-workspace.md) is de resource op het hoogste niveau voor de service. Het biedt u met een centrale locatie voor het werken met alle artefacten die u maakt. In de Python-SDK, opent u de werkruimte-artefacten met het maken van een [ `workspace` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) object.

Maken van een werkruimteobject uit de `config.json` bestand dat is gemaakt de [sectie vereisten](#prerequisites).

```Python
ws = Workspace.from_config()
```

### <a name="create-a-project-directory"></a>Maak een projectmap
Maak een map waarin u de benodigde code van uw lokale computer die u toegang hebben tot op de externe resource moet. Dit omvat het trainingsscript en eventuele aanvullende bestanden afhankelijk van uw trainingsscript.

```
import os

project_folder = './chainer-mnist'
os.makedirs(project_folder, exist_ok=True)
```

### <a name="prepare-training-script"></a>Trainingsscript voorbereiden

In deze zelfstudie, het trainingsscript **chainer_mnist.py** reeds wordt geboden voor u. In de praktijk moet u een aangepaste trainingsscript meenemen en uitvoeren met Azure ML zonder uw code te wijzigen.

Als u het bijhouden en de mogelijkheden voor metrische gegevens van Azure ML, moet u een kleine hoeveelheid Azure ML-code in uw trainingsscript toevoegen.  Het trainingsscript **chainer_mnist.py** laat zien hoe u aan te melden sommige metrische gegevens over uw Azure ML worden uitgevoerd. Om dit te doen, u toegang tot de Azure ML `Run` object in het script.

Kopieer het trainingsscript **chainer_mnist.py** in uw projectmap.

```
import shutil

shutil.copy('chainer_mnist.py', project_folder)
```

### <a name="create-an-experiment"></a>Een experiment maken

Een experiment en een map voor het opslaan van uw trainingsscripts maken. In dit voorbeeld maakt u een experiment met de naam 'chainer-mnist'.

```
from azureml.core import Experiment

experiment_name = 'chainer-mnist'
experiment = Experiment(ws, name=experiment_name)
```


## <a name="create-or-get-a-compute-target"></a>Maken of ophalen van een compute-doel

U moet een [compute-doel](concept-compute-target.md) voor het trainen van uw model. In deze zelfstudie gebruikt u Azure ML beheerd compute (AmlCompute) voor uw externe training compute-resource.

**Het maken van AmlCompute duurt ongeveer 5 minuten**. Als de AmlCompute met die naam al in uw werkruimte is, slaat deze code over het aanmaakproces.  

```Python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
cluster_name = "gpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target.')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    # create the cluster
    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True)

# use get_status() to get a detailed status for the current cluster. 
print(compute_target.get_status().serialize())
```

Zie voor meer informatie over de compute-doelen, de [wat is er een compute-doel](concept-compute-target.md) artikel.

## <a name="create-a-chainer-estimator"></a>Maken van een estimator Chainer

De [Chainer estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) biedt een eenvoudige manier van Chainer trainingstaken op uw compute-doel starten.

De Chainer estimator wordt geïmplementeerd via de algemene [ `estimator` ](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) klasse, die kan worden gebruikt voor de ondersteuning van elk gewenst framework. Zie voor meer informatie over het trainen van modellen met behulp van de algemene estimator [trainen van modellen met Azure Machine Learning met behulp van estimator](how-to-train-ml-models.md)

```Python
from azureml.train.dnn import Chainer

script_params = {
    '--epochs': 10,
    '--batchsize': 128,
    '--output_dir': './outputs'
}

estimator = Chainer(source_directory=project_folder, 
                    script_params=script_params,
                    compute_target=compute_target,
                    pip_packages=['numpy', 'pytest'],
                    entry_script='chainer_mnist.py',
                    use_gpu=True)
```

## <a name="submit-a-run"></a>Een uitvoering verzenden

De [object uitvoeren](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) biedt de interface voor de uitvoeringsgeschiedenis terwijl de taak wordt uitgevoerd en wanneer deze is voltooid.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Als de uitvoering wordt uitgevoerd, er wordt de volgende fasen:

- **Voorbereiden van**: Een docker-installatiekopie wordt gemaakt op basis van de Chainer estimator. De afbeelding is geüpload naar het containerregister van de werkruimte en hoger wordt uitgevoerd in de cache opgeslagen. Logboeken ook worden gestreamd naar de uitvoeringsgeschiedenis en kunnen worden weergegeven om voortgang te controleren.

- **Schalen**: Het cluster probeert om omhoog te schalen als de Batch AI-cluster meer knooppunten vereist voor het uitvoeren van de uitvoering dan er op dit moment beschikbaar zijn.

- **Uitvoeren**: Alle scripts in de scriptmap worden geüpload naar de compute-doel, data-archieven worden gekoppeld of worden gekopieerd en de entry_script wordt uitgevoerd. Uitvoer van stdout en. / map logboeken worden gestreamd naar de uitvoeringsgeschiedenis en kan worden gebruikt voor het bewaken van de uitvoering.

- **Nabewerken**: De. / map van de uitvoering is gekopieerd naar de uitvoeringsgeschiedenis levert.

## <a name="save-and-register-the-model"></a>Opslaan en Registreer het model

Nadat u het model hebt getraind, kunt u deze kunt opslaan en deze aan uw werkruimte te registreren. Registratie van het model kunt u store en versie uw modellen in de werkruimte voor het vereenvoudigen van [model voor beheer en de implementatie](concept-model-management-and-deployment.md).

Voeg de volgende code naar uw trainingsscript **chainer_mnist.py**, om op te slaan van het model. 

``` Python
    serializers.save_npz(os.path.join(args.output_dir, 'model.npz'), model)
```

Registreer het model aan uw werkruimte met de volgende code.

```Python
model = run.register_model(model_name='chainer-dnn-mnist', model_path='outputs/model.npz')
```



## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een model Chainer op Azure Machine Learning-service getraind. 

* Als u wilt weten hoe u een model implementeren, gaat u naar onze [deployment model](how-to-deploy-and-where.md) artikel.

* [Afstemmen van hyperparameters](how-to-tune-hyperparameters.md)

* [Metrische gegevens over uitvoeren tijdens de training bijhouden](how-to-track-experiments.md)
