---
title: Machine learning-modellen met Azure Machine Learning te trainen
description: Meer informatie over het uitvoeren van één knooppunt en gedistribueerde training van traditionele machine learning- en deep learning-modellen met Azure Machine Learning-services
ms.author: minxia
author: mx-iao
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 50b808b5769f2160d765ecdb431d71856e651b33
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46983579"
---
# <a name="how-to-train-models-with-azure-machine-learning"></a>Hoe u met het trainen van modellen met Azure Machine Learning

Training machine learning-modellen, met name deep neural networks is vaak een tijd - en rekenintensieve taak. Wanneer u klaar bent met het schrijven van uw scripts training en wordt uitgevoerd op een kleine subset van gegevens op uw lokale computer, wilt u waarschijnlijk uw workload opschalen.

Training in het kader, biedt de Azure Machine Learning Python SDK een abstractie op hoog niveau, de klasse Estimator, zodat gebruikers eenvoudig modellen kunt trainen hun in het Azure-ecosysteem. U kunt maken en een object Estimator gebruiken voor het indienen van een training-code die u uitvoeren op externe compute, wilt ongeacht of dit een één knooppunt uitvoeren of gedistribueerde training in een GPU-cluster. Azure Machine Learning biedt ook de respectieve aangepaste PyTorch en TensorFlow loopt die gemakkelijk te gebruiken deze frameworks voor PyTorch en TensorFlow-taken.

## <a name="train-with-an-estimator"></a>Trainen met een Estimator

Als u hebt gemaakt uw [werkruimte](https://docs.microsoft.com/azure/machine-learning/service/concept-azure-machine-learning-architecture#workspace) en stel uw [ontwikkelomgeving](how-to-configure-environment.md), trainen van een model in Azure Machine Learning omvat de volgende stappen:  
1. [Maak een externe compute-doel](how-to-set-up-training-targets.md)
2. [Uw trainingsgegevens uploaden](how-to-access-data.md) (optioneel)
3. Uw trainingsscript maken
4. Maken van een object Estimator
5. Uw trainingstaak verzenden

In dit artikel richt zich op de stappen 4 en 5. Voor de stappen 1-3, verwijzen naar dit [zelfstudie](tutorial-train-models-with-aml.md) voor een voorbeeld.

### <a name="single-node-training"></a>Training voor één knooppunt

De volgende code helpt bij de training van een één knooppunt worden uitgevoerd op externe compute in Azure voor een scikit-model meer. U moet al hebt gemaakt uw [compute-doel](how-to-set-up-training-targets.md#batch) object `compute_target` en uw [gegevensopslag](how-to-access-data.md) object `ds`.

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

Het bovenstaande codefragment bevat de volgende parameters voor de kostenraming-constructor:
* `source_directory`: De lokale map waarin alle uw code die nodig zijn voor de trainingstaak. Deze map wordt op uw lokale machine gekopieerd naar de externe compute 
* `script_params`: Een woordenlijst op te geven de opdrachtregelargumenten voor uw trainingsscript `entry_script`, in de vorm van < opdrachtregelargument, waarde > paren
* `compute_target`: De externe compute die uw trainingsscript uitgevoerd op, in dit geval een [Batch AI](how-to-set-up-training-targets.md#batch) cluster
* `entry_script`: De filepath (relatief aan de `source_directory`) van het trainingsscript in de berekening die extern worden uitgevoerd. Dit bestand en eventuele aanvullende bestanden dat hangt ervan af, moeten zich bevinden in deze map
* `conda_packages`: De lijst met Python-pakketten worden geïnstalleerd via conda die nodig zijn voor uw trainingsscript.  
De constructor heeft een andere parameter met de naam `pip_packages` die u kunt gebruiken voor een pip-pakketten die nodig zijn

Nu dat u het object Estimator hebt gemaakt, kunt u de trainingstaak in de berekening die extern worden uitgevoerd via een aanroep naar verzenden de `submit` functioneren in uw [Experiment](concept-azure-machine-learning-architecture.md#experiment) object `experiment`. 

```Python
run = experiment.submit(sk_est)
print(run.get_details().status)
```

> [!IMPORTANT]
> **Speciale mappen** twee mappen *levert* en *logboeken*, speciale behandeling door Azure Machine Learning. Tijdens de training, als u bestanden naar de mappen met de naam schrijven *levert* en *logboeken* die zijn ten opzichte van de hoofdmap (`./outputs` en `./logs`respectievelijk), krijgt deze bestanden automatisch geüpload naar uw uitvoeringsgeschiedenis zodat u de toegang tot hebben als de uitvoering is voltooid. 
>
> Voor toegang tot artefacten die zijn gemaakt tijdens de training (zoals modelbestanden, controlepunten, gegevensbestanden of uitgezette afbeeldingen) schrijft deze naar de `./outputs` map.
>
> Op deze manier kunt u alle logboeken schrijven van uw training uitvoeren naar de `./logs` map. Gebruikmaken van Azure Machine Learning [TensorBoard integratie](https://aka.ms/aml-notebook-tb) Zorg ervoor dat u uw logboeken TensorBoard schrijven naar deze map. Terwijl uw uitvoering uitgevoerd wordt, kunt u zich TensorBoard starten en deze logboeken streamen.  Later, ook mogelijk om terug te zetten van de logboeken van een van uw eerdere uitvoeringen.
>
> Bijvoorbeeld, voor het downloaden van een bestand geschreven naar de *levert* map op uw lokale computer na uw externe training uitvoeren: `run.download_file(name='outputs/my_output_file', output_file_path='my_destination_path')`

### <a name="distributed-training-and-custom-docker-images"></a>Gedistribueerde trainings- en aangepaste Docker-installatiekopieën

Er zijn twee aanvullende scenario's die u met de Estimator uitvoeren kunt:
1. Met behulp van een aangepaste Docker-installatiekopie
2. Gedistribueerde training op een cluster met meerdere knooppunten

De volgende code toont hoe u bij het uitvoeren van gedistribueerde training voor een CNTK-model. Bovendien, in plaats van de standaard Azure Machine Learning-afbeeldingen, wordt ervan uitgegaan dat u hebt uw eigen aangepaste docker-installatiekopie die u wilt gebruiken voor training.

U moet al hebt gemaakt uw [compute-doel](how-to-set-up-training-targets.md#batch) object `compute_target`. U kunt als volgt de estimator maken:

```Python
from azureml.train.estimator import Estimator

estimator = Estimator(source_directory='./my-cntk-proj',
                      compute_target=compute_target,
                      entry_script='train.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_backend='mpi',     
                      pip_packages=['cntk==2.5.1'],
                      custom_docker_base_image='microsoft/mmlspark:0.12')
```

De bovenstaande code wordt aangegeven dat de volgende nieuwe parameters voor de kostenraming-constructor:
* `custom_docker_base_image`: De naam van de installatiekopie die u wilt gebruiken. U kunt alleen installatiekopieën die beschikbaar zijn in de openbare docker-opslagplaatsen (in dit geval Docker Hub) opgeven. Gebruik van de constructor voor het gebruik van een installatiekopie van een privé-docker-opslagplaats, `environment_definition` parameter in plaats daarvan
* `node_count`: Het aantal knooppunten moet worden gebruikt voor de trainingstaak. Dit argument standaard ingesteld op `1`
* `process_count_per_node`: Het aantal processen (of 'werknemers') om uit te voeren op elk knooppunt. In dit geval gebruikt u de `2` GPU's die beschikbaar zijn op elk knooppunt. Dit argument standaard ingesteld op `1`
* `distributed_backend`: De back-end voor het starten distributed opleiding, die de Estimator via MPI biedt. Dit argument standaard ingesteld op `None`. Als u wilt uitvoeren van parallelle en gedistribueerde training (bijvoorbeeld `node_count`> 1 of `process_count_per_node`> 1 of beide) en stel `distributed_backend='mpi'`. De MPI-implementatie die worden gebruikt door AML [Open MPI](https://www.open-mpi.org/).

Ten slotte verzendt u de trainingstaak:
```Python
run = experiment.submit(cntk_est)
```

## <a name="examples"></a>Voorbeelden
Zie voor een zelfstudie voor het trainen van een model sklearn:
* `tutorials/01.train-models.ipynb`

Voor een zelfstudie over gedistribueerde CNTK met behulp van aangepaste docker, Zie:
* `training/06.distributed-cntk-with-custom-docker/06.distributed-cntk-with-custom-docker.ipynb`

Deze laptops ophalen:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Volgende stappen

* [Metrische gegevens over uitvoeren tijdens de training bijhouden](how-to-track-experiments.md)
* [PyTorch-modellen trainen](how-to-train-pytorch.md)
* [TensorFlow-modellen trainen](how-to-train-tensorflow.md)
* [Afstemmen van hyperparameters](how-to-tune-hyperparameters.md)
* [Een getraind model implementeren](how-to-deploy-and-where.md)
