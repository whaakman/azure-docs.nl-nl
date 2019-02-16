---
title: ML-modellen trainen met loopt
titleSuffix: Azure Machine Learning service
description: Meer informatie over het uitvoeren van één knooppunt en gedistribueerde training van traditionele machine learning- en deep learning-modellen met behulp van Azure Machine Learning services Estimator klasse
ms.author: minxia
author: mx-iao
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.date: 2/14/2019
ms.custom: seodec18
ms.openlocfilehash: 58dd96b079dda50faa17a52782a79db83a0141bd
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56330066"
---
# <a name="train-models-with-azure-machine-learning-using-estimator"></a>Modellen trainen met Azure Machine Learning met behulp van estimator

Met Azure Machine Learning, kunt u eenvoudig uw trainingsscript om indienen [diverse compute-doelen](how-to-set-up-training-targets.md#compute-targets-for-training), met [RunConfiguration object](how-to-set-up-training-targets.md#whats-a-run-configuration) en [ScriptRunConfig object](how-to-set-up-training-targets.md#submit). Dit patroon biedt veel flexibiliteit en maximale controle.

In het kader deep learning-modeltraining, Azure Machine Learning Python SDK biedt een alternatieve op een hoger niveau abstractie, de klasse estimator, zodat gebruikers eenvoudig constructie uitvoeren configuraties. U kunt maken en gebruik van een generieke [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) om in te dienen trainingsscript met behulp van elk learning framework dat u kiest (zoals scikit-meer) u wilt uitvoeren op een compute-doel of het nu uw lokale computer, een enkele virtuele machine in Azure of een GPU -cluster in Azure. Taken voor PyTorch, TensorFlow en Chainer, Azure Machine Learning biedt ook respectieve [PyTorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) en [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) loopt te vereenvoudigen met behulp van deze frameworks.

## <a name="train-with-an-estimator"></a>Trainen met een estimator

Als u hebt gemaakt uw [werkruimte](concept-azure-machine-learning-architecture.md#workspace) en stel uw [ontwikkelomgeving](how-to-configure-environment.md), trainen van een model in Azure Machine Learning omvat de volgende stappen:  
1. Maak een [externe compute-doel](how-to-set-up-training-targets.md) (opmerking die u kunt ook de lokale computer gebruiken als compute-doel)
2. Upload uw [trainingsgegevens](how-to-access-data.md) met gegevensarchief (optioneel)
3. Maak uw [trainingsscript](tutorial-train-models-with-aml.md#create-a-training-script)
4. Maak een `Estimator` object
5. Verzenden van de estimator naar een object experiment onder de werkruimte

In dit artikel richt zich op de stappen 4 en 5. Voor stappen 1-3, naar verwijzen de [zelfstudie: een model trainen](tutorial-train-models-with-aml.md) voor een voorbeeld.

### <a name="single-node-training"></a>Training voor één knooppunt

Gebruik een `Estimator` voor de training van een één knooppunt worden uitgevoerd op externe compute in Azure voor een scikit-model meer. U moet al hebt gemaakt uw [compute-doel](how-to-set-up-training-targets.md#amlcompute) object `compute_target` en uw [gegevensopslag](how-to-access-data.md) object `ds`.

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

sk_est = Estimator(source_directory='./my-sklearn-proj',
                   script_params=script_params,
                   compute_target=compute_target,
                   entry_script='train.py',
                   conda_packages=['scikit-learn'])
```

Dit codefragment bevat de volgende parameters voor de `Estimator` constructor.

Parameter | Description
--|--
`source_directory`| Lokale map waarin alle uw code die nodig zijn voor de trainingstaak. Deze map wordt op uw lokale machine gekopieerd naar de externe compute 
`script_params`| Woordenlijst voor de opdrachtregelargumenten voor uw trainingsscript op te geven `entry_script`, in de vorm van < opdrachtregelargument, waarde > paren
`compute_target`| Externe compute-doel dat uw trainingsscript wordt uitgevoerd op, in dit geval een Azure Machine Learning-Computing ([AmlCompute](how-to-set-up-training-targets.md#amlcompute)) cluster. (Houd er rekening mee Hoewel AmlCompute cluster het meest gebruikte doel is, het is ook mogelijk om te kiezen andere compute doeltypen, zoals virtuele Azure-machines of zelfs lokale computer.)
`entry_script`| FilePath (relatief aan de `source_directory`) van het trainingsscript in de berekening die extern worden uitgevoerd. Dit bestand en eventuele aanvullende bestanden dat hangt ervan af, moeten zich bevinden in deze map
`conda_packages`| Lijst met Python-pakketten worden geïnstalleerd via conda die nodig zijn voor uw trainingsscript.  
De constructor heeft een andere parameter met de naam `pip_packages` die u gebruikt voor een pip-pakketten die nodig zijn

Nu dat u hebt uw `Estimator` object, verzenden van de trainingstaak om te worden uitgevoerd op de externe compute met een aanroep naar de `submit` functioneren in uw [Experiment](concept-azure-machine-learning-architecture.md#experiment) object `experiment`. 

```Python
run = experiment.submit(sk_est)
print(run.get_portal_url())
```

> [!IMPORTANT]
> **Speciale mappen** twee mappen *levert* en *logboeken*, speciale behandeling door Azure Machine Learning. Tijdens de training, bij het schrijven van bestanden in mappen met de naam *levert* en *logboeken* die zijn ten opzichte van de hoofdmap (`./outputs` en `./logs`respectievelijk), de bestanden worden automatisch uploaden naar uw uitvoeringsgeschiedenis zodat u de toegang tot hebben als uw uitvoering is voltooid.
>
> Artefacten tijdens de training (zoals modelbestanden, controlepunten, gegevensbestanden of uitgezette afbeeldingen) schrijven om te maken aan de `./outputs` map.
>
> Op deze manier kunt u alle logboeken schrijven van uw training uitvoeren naar de `./logs` map. Gebruikmaken van Azure Machine Learning [TensorBoard integratie](https://aka.ms/aml-notebook-tb) Zorg ervoor dat u uw logboeken TensorBoard schrijven naar deze map. Terwijl uw uitvoering uitgevoerd wordt, kunt u zich TensorBoard starten en deze logboeken streamen.  Later, ook mogelijk om terug te zetten van de logboeken van een van uw eerdere uitvoeringen.
>
> Bijvoorbeeld, voor het downloaden van een bestand geschreven naar de *levert* map op uw lokale computer na uw externe training uitvoeren: `run.download_file(name='outputs/my_output_file', output_file_path='my_destination_path')`

### <a name="distributed-training-and-custom-docker-images"></a>Gedistribueerde trainings- en aangepaste Docker-installatiekopieën

Er zijn twee aanvullende scenario's u met uitvoeren kunt de `Estimator`:
* Met behulp van een aangepaste Docker-installatiekopie
* Gedistribueerde training op een cluster met meerdere knooppunten

De volgende code toont hoe u bij het uitvoeren van gedistribueerde training voor een Keras-model. Bovendien, in plaats van de standaard Azure Machine Learning-afbeeldingen, het Hiermee geeft u een aangepaste docker-installatiekopie uit Docker Hub `continuumio/miniconda` voor training.

U moet al hebt gemaakt uw [compute-doel](how-to-set-up-training-targets.md#amlcompute) object `compute_target`. U maken als volgt de estimator:

```Python
from azureml.train.estimator import Estimator

estimator = Estimator(source_directory='./my-keras-proj',
                      compute_target=compute_target,
                      entry_script='train.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_backend='mpi',     
                      conda_packages=['tensorflow', 'keras'],
                      custom_docker_base_image='continuumio/miniconda')
```

De bovenstaande code wordt aangegeven dat de volgende nieuwe parameters voor de `Estimator` constructor:

Parameter | Description | Standaard
--|--|--
`custom_docker_base_image`| Naam van de installatiekopie die u wilt gebruiken. Geef alleen installatiekopieën die beschikbaar zijn in de openbare docker-opslagplaatsen (in dit geval Docker Hub). Gebruik van de constructor voor het gebruik van een installatiekopie van een privé-docker-opslagplaats, `environment_definition` parameter in plaats daarvan. [Zie het voorbeeld](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/how-to-use-estimator/how-to-use-estimator.ipynb). | `None`
`node_count`| Het aantal knooppunten moet worden gebruikt voor de trainingstaak. | `1`
`process_count_per_node`| Het aantal processen (of 'werknemers') om uit te voeren op elk knooppunt. In dit geval gebruikt u de `2` GPU's die beschikbaar zijn op elk knooppunt.| `1`
`distributed_backend`| Back-end voor het starten distributed opleiding, die de Estimator via MPI biedt.  Bij het uitvoeren van parallelle en gedistribueerde training (bijvoorbeeld `node_count`> 1 of `process_count_per_node`> 1 of beide) en stel `distributed_backend='mpi'`. De MPI-implementatie die worden gebruikt door AML [Open MPI](https://www.open-mpi.org/).| `None`

Ten slotte verzendt u de trainingstaak:
```Python
run = experiment.submit(estimator)
print(run.get_portal_url())
```

## <a name="examples"></a>Voorbeelden
Zie voor een notitieblok waarin de basisbeginselen van estimator patroon:
* [how-to-use-azureml/training-with-deep-learning/how-to-use-estimator](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/how-to-use-estimator/how-to-use-estimator.ipynb)

Voor een laptop die binnen een scikit-model meer estimator gebruikt, Zie:
* [zelfstudies/img-classificatie-deel 1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

Voor laptops voor het trainen van modellen met behulp van specifieke loopt deep-learning-framework, Zie:
* [How-to-use-azureml/training-with-deep-Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Volgende stappen

* [Metrische gegevens over uitvoeren tijdens de training bijhouden](how-to-track-experiments.md)
* [PyTorch-modellen trainen](how-to-train-pytorch.md)
* [TensorFlow-modellen trainen](how-to-train-tensorflow.md)
* [Afstemmen van hyperparameters](how-to-tune-hyperparameters.md)
* [Een getraind model implementeren](how-to-deploy-and-where.md)
