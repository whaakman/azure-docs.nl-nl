---
title: Machine learning-modellen met behulp van een klasse Estimator met Azure Machine Learning te trainen
description: Meer informatie over het uitvoeren van één knooppunt en gedistribueerde training van traditionele machine learning- en deep learning-modellen met behulp van Azure Machine Learning services Estimator klasse
ms.author: minxia
author: mx-iao
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 8c2f4ad9acc789a23eb951265b0464aee6caaa6c
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48815032"
---
# <a name="how-to-train-models-with-azure-machine-learning"></a>Hoe u met het trainen van modellen met Azure Machine Learning

Training machine learning-modellen, met name deep neural networks is vaak een tijd - en rekenintensieve taak. Wanneer u klaar bent met het schrijven van uw scripts training en wordt uitgevoerd op een kleine subset van gegevens op uw lokale computer, wilt u waarschijnlijk uw workload opschalen.

Training in het kader, biedt de Azure Machine Learning Python SDK een abstractie op hoog niveau, de klasse estimator, zodat gebruikers eenvoudig modellen kunt trainen hun in het Azure-ecosysteem. U kunt maken en gebruiken een `Estimator` object om in te dienen alle training-code die u uitvoeren op externe compute, wilt ongeacht of dit een één knooppunt uitvoeren of gedistribueerde training in een GPU-cluster. Voor PyTorch en TensorFlow-taken, biedt Azure Machine Learning ook respectieve aangepaste `PyTorch` en `TensorFlow` loopt te vereenvoudigen met behulp van deze frameworks.

## <a name="train-with-an-estimator"></a>Trainen met een estimator

Als u hebt gemaakt uw [werkruimte](concept-azure-machine-learning-architecture.md#workspace) en stel uw [ontwikkelomgeving](how-to-configure-environment.md), trainen van een model in Azure Machine Learning omvat de volgende stappen:  
1. Maak een [externe compute-doel](how-to-set-up-training-targets.md)
2. Upload uw [trainingsgegevens](how-to-access-data.md) (optioneel)
3. Maak uw [trainingsscript](tutorial-train-models-with-aml.md#create-a-training-script)
4. Maak een `Estimator` object
5. Uw trainingstaak verzenden

In dit artikel richt zich op de stappen 4 en 5. Voor stappen 1-3, naar verwijzen de [zelfstudie: een model trainen](tutorial-train-models-with-aml.md) voor een voorbeeld.

### <a name="single-node-training"></a>Training voor één knooppunt

Gebruik een `Estimator` voor de training van een één knooppunt worden uitgevoerd op externe compute in Azure voor een scikit-model meer. U moet al hebt gemaakt uw [compute-doel](how-to-set-up-training-targets.md#batch) object `compute_target` en uw [gegevensopslag](how-to-access-data.md) object `ds`.

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

Parameter | Beschrijving
--|--
`source_directory`| Lokale map waarin alle uw code die nodig zijn voor de trainingstaak. Deze map wordt op uw lokale machine gekopieerd naar de externe compute 
`script_params`| Woordenlijst voor de opdrachtregelargumenten voor uw trainingsscript op te geven `entry_script`, in de vorm van < opdrachtregelargument, waarde > paren
`compute_target`| Externe compute die uw trainingsscript uitgevoerd op, in dit geval een [Batch AI](how-to-set-up-training-targets.md#batch) cluster
`entry_script`| FilePath (relatief aan de `source_directory`) van het trainingsscript in de berekening die extern worden uitgevoerd. Dit bestand en eventuele aanvullende bestanden dat hangt ervan af, moeten zich bevinden in deze map
`conda_packages`| Lijst met Python-pakketten worden geïnstalleerd via conda die nodig zijn voor uw trainingsscript.  
De constructor heeft een andere parameter met de naam `pip_packages` die u gebruikt voor een pip-pakketten die nodig zijn

Nu dat u hebt uw `Estimator` object, verzenden van de trainingstaak om te worden uitgevoerd op de externe compute met een aanroep naar de `submit` functioneren in uw [Experiment](concept-azure-machine-learning-architecture.md#experiment) object `experiment`. 

```Python
run = experiment.submit(sk_est)
print(run.get_details().status)
```

> [!IMPORTANT]
> **Speciale mappen** twee mappen *levert* en *logboeken*, speciale behandeling door Azure Machine Learning. Tijdens de training, bij het schrijven van bestanden in mappen met de naam *levert* en *logboeken* die zijn ten opzichte van de hoofdmap (`./outputs` en `./logs`respectievelijk), de bestanden worden automatisch uploaden naar uw uitvoeringsgeschiedenis zodat u de toegang tot hebben als uw uitvoering is voltooid.
>
> Artefacten tijdens de training (zoals modelbestanden, controlepunten, gegevensbestanden of uitgezette afbeeldingen) schrijven om te maken aan de `./outputs` map.
>
> Op dezelfde manier het schrijven van alle logboeken van uw training uitvoeren naar de `./logs` map. Gebruikmaken van Azure Machine Learning [TensorBoard integratie](https://aka.ms/aml-notebook-tb) Zorg ervoor dat u uw logboeken TensorBoard schrijven naar deze map. Terwijl uw uitvoering uitgevoerd wordt, kunt u zich TensorBoard starten en deze logboeken streamen.  Later, ook mogelijk om terug te zetten van de logboeken van een van uw eerdere uitvoeringen.
>
> Bijvoorbeeld, voor het downloaden van een bestand geschreven naar de *levert* map op uw lokale computer na uw externe training uitvoeren: `run.download_file(name='outputs/my_output_file', output_file_path='my_destination_path')`

### <a name="distributed-training-and-custom-docker-images"></a>Gedistribueerde trainings- en aangepaste Docker-installatiekopieën

Er zijn twee aanvullende scenario's u met uitvoeren kunt de `Estimator`:
* Met behulp van een aangepaste Docker-installatiekopie
* Gedistribueerde training op een cluster met meerdere knooppunten

De volgende code toont hoe u bij het uitvoeren van gedistribueerde training voor een CNTK-model. Bovendien, in plaats van de standaard Azure Machine Learning-afbeeldingen, wordt ervan uitgegaan dat u uw eigen aangepaste docker-installatiekopie gebruikt voor trainingen.

U moet al hebt gemaakt uw [compute-doel](how-to-set-up-training-targets.md#batch) object `compute_target`. U maken als volgt de estimator:

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

De bovenstaande code wordt aangegeven dat de volgende nieuwe parameters voor de `Estimator` constructor:

Parameter | Beschrijving | Normaal
--|--|--
`custom_docker_base_image`| Naam van de installatiekopie die u wilt gebruiken. Geef alleen installatiekopieën die beschikbaar zijn in de openbare docker-opslagplaatsen (in dit geval Docker Hub). Gebruik van de constructor voor het gebruik van een installatiekopie van een privé-docker-opslagplaats, `environment_definition` parameter in plaats daarvan | `None`
`node_count`| Het aantal knooppunten moet worden gebruikt voor de trainingstaak. | `1`
`process_count_per_node`| Het aantal processen (of 'werknemers') om uit te voeren op elk knooppunt. In dit geval gebruikt u de `2` GPU's die beschikbaar zijn op elk knooppunt.| `1`
`distributed_backend`| Back-end voor het starten distributed opleiding, die de Estimator via MPI biedt.  Bij het uitvoeren van parallelle en gedistribueerde training (bijvoorbeeld `node_count`> 1 of `process_count_per_node`> 1 of beide) en stel `distributed_backend='mpi'`. De MPI-implementatie die worden gebruikt door AML [Open MPI](https://www.open-mpi.org/).| `None`

Ten slotte verzendt u de trainingstaak:
```Python
run = experiment.submit(cntk_est)
```

## <a name="examples"></a>Voorbeelden
Zie voor een zelfstudie voor het trainen van een model sklearn:
* `tutorials/01.train-models.ipynb`

Voor een zelfstudie over gedistribueerde CNTK met behulp van aangepaste docker, Zie:
* `training/06.distributed-cntk-with-custom-docker`

Deze laptops ophalen:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Volgende stappen

* [Metrische gegevens over uitvoeren tijdens de training bijhouden](how-to-track-experiments.md)
* [PyTorch-modellen trainen](how-to-train-pytorch.md)
* [TensorFlow-modellen trainen](how-to-train-tensorflow.md)
* [Afstemmen van hyperparameters](how-to-tune-hyperparameters.md)
* [Een getraind model implementeren](how-to-deploy-and-where.md)
