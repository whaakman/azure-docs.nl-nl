---
title: PyTorch-modellen met Azure Machine Learning te trainen
description: Meer informatie over het uitvoeren van één knooppunt en gedistribueerde training van PyTorch-modellen met de PyTorch estimator
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: e569b63f676fb750bcbab88dda6cda39156d41f5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46977025"
---
# <a name="how-to-train-pytorch-models"></a>Hoe PyTorch-modellen kunt trainen

Azure Machine Learning biedt voor deep neural network (DNN) training over het gebruik van PyTorch, een aangepaste PyTorch-klasse van de kostenraming. De Azure SDK PyTorch Estimator kunt u eenvoudig indienen PyTorch trainingstaken voor één knooppunt en het gedistribueerde wordt uitgevoerd op Azure-rekenen.

## <a name="single-node-training"></a>Training voor één knooppunt
Training met de PyTorch Estimator is vergelijkbaar met de [baseren Estimator](how-to-train-ml-models.md), dus eerst Lees het artikel met instructies en zorg ervoor dat u er de concepten begrijpen.
  
Als u wilt een PyTorch-taak uitvoert, instantiëren een `PyTorch` object. U moet al hebt gemaakt uw [compute-doel](how-to-set-up-training-targets.md#batch) object `compute_target` en uw [gegevensopslag](how-to-access-data.md) object `ds`.

```Python
from azureml.train.dnn import PyTorch

script_params = {
    '--data_dir': ds
}

pt_est = PyTorch(source_directory='./my-pytorch-proj',
                 script_params=script_params,
                 compute_target=compute_target,
                 entry_script='train.py',
                 use_gpu=True)
```

Hier geeft u de volgende parameters voor de PyTorch-constructor:
* `source_directory`: De lokale map waarin alle uw code die nodig zijn voor de trainingstaak. Deze map wordt op uw lokale machine gekopieerd naar de externe compute
* `script_params`: Een woordenlijst op te geven de opdrachtregelargumenten voor uw trainingsscript `entry_script`, in de vorm van < opdrachtregelargument, waarde > paren
* `compute_target`: De externe compute die uw trainingsscript uitgevoerd op, in dit geval een [Batch AI](how-to-set-up-training-targets.md#batch) cluster
* `entry_script`: De filepath (relatief aan de `source_directory`) van het trainingsscript in de berekening die extern worden uitgevoerd. Dit bestand en eventuele aanvullende bestanden dat hangt ervan af, moeten zich bevinden in deze map
* `conda_packages`: De lijst met Python-pakketten worden geïnstalleerd via conda die nodig zijn voor uw trainingsscript.
De constructor heeft een andere parameter met de naam `pip_packages` die u kunt gebruiken voor een pip-pakketten die nodig zijn
* `use_gpu`: Met deze markering wordt ingesteld op `True` gebruikmaken van de GPU voor training. Standaard ingesteld op `False`

Omdat u van de estimator PyTorch gebruikmaakt, de container die wordt gebruikt voor de training wordt standaard de PyTorch-pakket en de bijbehorende afhankelijkheden die nodig zijn voor de training over CPU's en GPU's omvatten.

Vervolgens verzenden van de taak PyTorch:
```Python
run = exp.submit(pt_est)
```

## <a name="distributed-training"></a>Gedistribueerde training
De PyTorch Estimator kunt u uw modellen op schaal te trainen voor CPU en GPU-clusters virtuele Azure-machines. U kunt eenvoudig gedistribueerde PyTorch-training uitvoeren met een aantal API-aanroepen, terwijl Azure Machine Learning achter de schermen beheert de infrastructuur en orchestration die nodig zijn voor het uitvoeren van deze werkbelastingen.

Azure Machine Learning ondersteunt momenteel MPI-gebaseerde gedistribueerde training van PyTorch met behulp van de Horovod-framework.

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) is een open-source ring-allreduce-framework voor gedistribueerde cursussen ontwikkeld door Uber.

Als u wilt uitvoeren van gedistribueerde PyTorch met behulp van de Horovod-framework, als volgt de PyTorch-object maken:

```Python
from azureml.train.dnn import PyTorch

pt_est = PyTorch(source_directory='./my-pytorch-project',
                 script_params={},
                 compute_target=compute_target,
                 entry_script='train.py',
                 node_count=2,
                 process_count_per_node=1,
                 distributed_backend='mpi',
                 use_gpu=True)
```

De bovenstaande code wordt aangegeven dat de volgende nieuwe parameters voor de PyTorch-constructor:
* `node_count`: Het aantal knooppunten moet worden gebruikt voor de trainingstaak. Dit argument standaard ingesteld op `1`
* `process_count_per_node`: Het aantal processen (of 'werknemers') om uit te voeren op elk knooppunt. Dit argument standaard ingesteld op `1`
* `distributed_backend`: De back-end voor het starten distributed opleiding, die de Estimator via MPI biedt. Dit argument standaard ingesteld op `None`. Als u wilt uitvoeren van parallelle en gedistribueerde training (bijvoorbeeld `node_count`> 1 of `process_count_per_node`> 1 of beide) instellen met MPI (en Horovod) `distributed_backend='mpi'`. De MPI-implementatie die worden gebruikt door Azure Machine Learning is [Open MPI](https://www.open-mpi.org/).

Het bovenstaande voorbeeld wordt gedistribueerd training worden uitgevoerd met twee werkrollen één werknemer per knooppunt.

Horovod en de bijbehorende afhankelijkheden wordt geïnstalleerd, zodat u deze eenvoudig in uw trainingsscript importeren kunt `train.py` als volgt:
```Python
import torch
import horovod
```

Ten slotte uw gedistribueerde PyTorch-taak verzenden:
```Python
run = exp.submit(pt_est)
```

## <a name="examples"></a>Voorbeelden
Zie voor een zelfstudie op één knooppunt PyTorch training:
* `training/01.train-tune-deploy-pytorch/01.train-tune-deploy-pytorch.ipynb`

Voor een zelfstudie over gedistribueerde PyTorch met Horovod, Zie:
* `training/02.distributed-pytorch-with-horovod/02.distributed-pytorch-with-horovod.ipynb`

Deze laptops ophalen:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Volgende stappen
* [Metrische gegevens over uitvoeren tijdens de training bijhouden](how-to-track-experiments.md)
* [Afstemmen van hyperparameters](how-to-tune-hyperparameters.md)
* [Een getraind model implementeren](how-to-deploy-and-where.md)
