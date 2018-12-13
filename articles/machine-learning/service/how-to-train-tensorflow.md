---
title: TensorFlow-modellen met Azure Machine Learning te trainen
description: Meer informatie over het uitvoeren van één knooppunt en gedistribueerde modellen TensorFlow-training met de TensorFlow estimator
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.openlocfilehash: 5746180b894da265dfc02176c13b5f9b60f4df96
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52872778"
---
# <a name="how-to-train-tensorflow-models"></a>Hoe TensorFlow modellen kunt trainen

Azure Machine Learning biedt voor deep neural network (DNN) training over het gebruik van TensorFlow, een aangepaste `TensorFlow` klasse van de `Estimator`. De Azure SDK `TensorFlow` estimator (niet te worden conflated met de [ `tf.estimator.Estimator` ](https://www.tensorflow.org/api_docs/python/tf/estimator/Estimator) klasse) kunt u eenvoudig TensorFlow-trainingstaken voor één knooppunt en het gedistribueerde wordt uitgevoerd op Azure-rekenen indienen.

## <a name="single-node-training"></a>Training voor één knooppunt
Training met de `TensorFlow` estimator is vergelijkbaar met de [basis `Estimator` ](how-to-train-ml-models.md), dus eerst Lees het artikel met instructies en zorg ervoor dat u er de concepten begrijpen.
  
Exemplaar om uit te voeren een taak TensorFlow, maken van een `TensorFlow` object. U moet al hebt gemaakt uw [compute-doel](how-to-set-up-training-targets.md#amlcompute) object `compute_target`.

```Python
from azureml.train.dnn import TensorFlow

script_params = {
    '--batch-size': 50,
    '--learning-rate': 0.01,
}

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='train.py',
                    conda_packages=['scikit-learn'],
                    use_gpu=True)
```

Hier geeft u de volgende parameters voor de TensorFlow-constructor:

Parameter | Beschrijving
--|--
`source_directory` | Lokale map waarin alle uw code die nodig zijn voor de trainingstaak. Deze map wordt op uw lokale machine gekopieerd naar de externe compute
`script_params` | Woordenlijst voor de opdrachtregelargumenten voor uw trainingsscript op te geven `entry_script`, in de vorm van < opdrachtregelargument, waarde > paren
`compute_target` | Externe compute-doel dat uw trainingsscript wordt uitgevoerd op, in dit geval een Azure Machine Learning-Computing ([AmlCompute](how-to-set-up-training-targets.md#amlcompute)) cluster
`entry_script` | FilePath (relatief aan de `source_directory`) van het trainingsscript in de berekening die extern worden uitgevoerd. Dit bestand en eventuele aanvullende bestanden dat hangt ervan af, moeten zich bevinden in deze map
`conda_packages` | Lijst met Python-pakketten worden geïnstalleerd via conda die nodig zijn voor uw trainingsscript. In dit geval trainingsscript gebruikt `sklearn` om de gegevens te laden, dus geef dit pakket kan worden geïnstalleerd.  De constructor heeft een andere parameter met de naam `pip_packages` die u kunt gebruiken voor een pip-pakketten die nodig zijn
`use_gpu` | Deze vlag ingesteld op `True` gebruikmaken van de GPU voor training. Standaard ingesteld op `False`.

Omdat u van de estimator TensorFlow gebruikmaakt, de container die wordt gebruikt voor de training wordt standaard de TensorFlow-pakket en de bijbehorende afhankelijkheden die nodig zijn voor de training over CPU's en GPU's omvatten.

Vervolgens verzenden van de taak TensorFlow:
```Python
run = exp.submit(tf_est)
```

## <a name="distributed-training"></a>Gedistribueerde training
De TensorFlow Estimator kunt u uw modellen op schaal te trainen voor CPU en GPU-clusters virtuele Azure-machines. U kunt eenvoudig gedistribueerde TensorFlow-training uitvoeren met een aantal API-aanroepen, terwijl Azure Machine Learning achter de schermen beheert de infrastructuur en orchestration die nodig zijn voor het uitvoeren van deze werkbelastingen.

Azure Machine Learning ondersteunt twee methoden van gedistribueerde training in TensorFlow:
* Op basis van een MPI gedistribueerd training met behulp van de [Horovod](https://github.com/uber/horovod) framework
* systeemeigen [gedistribueerde TensorFlow](https://www.tensorflow.org/deploy/distributed) via de methode van de parameter-server

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) is een open-source ring-allreduce-framework voor gedistribueerde cursussen ontwikkeld door Uber.

Als u wilt uitvoeren van gedistribueerde TensorFlow met behulp van de Horovod-framework, als volgt de TensorFlow-object maken:

```Python
from azureml.train.dnn import TensorFlow

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py',
                    node_count=2,
                    process_count_per_node=1,
                    distributed_backend='mpi',
                    use_gpu=True)
```

De bovenstaande code wordt aangegeven dat de volgende nieuwe parameters voor de TensorFlow-constructor:

Parameter | Beschrijving | Standaard
--|--|--
`node_count` | Het aantal knooppunten moet worden gebruikt voor de trainingstaak. | `1`
`process_count_per_node` | Het aantal processen (of 'werknemers') om uit te voeren op elk knooppunt.|`1`
`distributed_backend` | Back-end voor het starten distributed opleiding, die de Estimator via MPI biedt. Als u wilt uitvoeren van parallelle en gedistribueerde training (bijvoorbeeld `node_count`> 1 of `process_count_per_node`> 1 of beide) instellen met MPI (en Horovod) `distributed_backend='mpi'`. De MPI-implementatie die worden gebruikt door Azure Machine Learning is [Open MPI](https://www.open-mpi.org/). | `None`

Het bovenstaande voorbeeld wordt gedistribueerd training worden uitgevoerd met twee werkrollen één werknemer per knooppunt.

Horovod en de bijbehorende afhankelijkheden wordt geïnstalleerd, zodat u deze eenvoudig in uw trainingsscript importeren kunt `train.py` als volgt:

```Python
import tensorflow as tf
import horovod
```

Ten slotte de TensorFlow-taak verzenden:
```Python
run = exp.submit(tf_est)
```

### <a name="parameter-server"></a>Parameterserver
U kunt ook uitvoeren [native gedistribueerde TensorFlow](https://www.tensorflow.org/deploy/distributed), waarbij het servermodel parameter worden gebruikt. Bij deze methode hoeft trainen u in een cluster van de parameter-servers en werknemers. De werknemers Bereken de verlopen tijdens de training, terwijl de parameter-servers de verlopen aggregeren.

Bouw het TensorFlow-object:

```Python
from azureml.train.dnn import TensorFlow

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py',
                    node_count=2,
                    worker_count=2,
                    parameter_server_count=1,
                    distributed_backend='ps',
                    use_gpu=True)
```

Let op de volgende parameters voor de TensorFlow-constructor in de bovenstaande code:

Parameter | Beschrijving | Standaard
--|--|--
`worker_count` | Het aantal werknemers. | `1`
`parameter_server_count` | Het aantal parameter-servers. | `1`
`distributed_backend` | Back-end te gebruiken voor een gedistribueerde training. Hiervoor gedistribueerde training via de parameterserver instellen `distributed_backend='ps'` | `None`

#### <a name="note-on-tfconfig"></a>Houd er rekening mee op `TF_CONFIG`
U moet ook de netwerkadressen en poorten van het cluster voor de [ `tf.train.ClusterSpec` ](https://www.tensorflow.org/api_docs/python/tf/train/ClusterSpec), zodat Azure Machine Learning wordt de `TF_CONFIG` omgevingsvariabele voor u.

De `TF_CONFIG` omgevingsvariabele is een JSON-tekenreeks. Hier volgt een voorbeeld van de variabele voor de parameterserver:
```
TF_CONFIG='{
    "cluster": {
        "ps": ["host0:2222", "host1:2222"],
        "worker": ["host2:2222", "host3:2222", "host4:2222"],
    },
    "task": {"type": "ps", "index": 0},
    "environment": "cloud"
}'
```

Als u TensorFlow op hoog niveau de [ `tf.estimator` ](https://www.tensorflow.org/api_docs/python/tf/estimator) API, TensorFlow parseren dit `TF_CONFIG` variabele en het buildnummer van het cluster specificaties voor u. 

Als u in plaats daarvan TensorFlow van lager niveau core API's voor training gebruikt, moet u parseren de `TF_CONFIG` variabele en bouwen de `tf.train.ClusterSpec` zelf in uw trainingen-code. In [in dit voorbeeld](https://aka.ms/aml-notebook-tf-ps), doet u dit in **uw trainingsscript** als volgt:

```Python
import os, json
import tensorflow as tf

tf_config = os.environ.get('TF_CONFIG')
if not tf_config or tf_config == "":
    raise ValueError("TF_CONFIG not found.")
tf_config_json = json.loads(tf_config)
cluster_spec = tf.train.ClusterSpec(cluster)

```

Zodra u klaar bent met uw trainingsscript schrijven en het maken van het object TensorFlow, kunt u uw trainingstaak verzenden:
```Python
run = exp.submit(tf_est)
```

## <a name="examples"></a>Voorbeelden

Voor laptops op gedistribueerde deep learning, Zie:
* [How-to-use-azureml/training-with-deep-Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Volgende stappen
* [Metrische gegevens over uitvoeren tijdens de training bijhouden](how-to-track-experiments.md)
* [Afstemmen van hyperparameters](how-to-tune-hyperparameters.md)
* [Een getraind model implementeren](how-to-deploy-and-where.md)
