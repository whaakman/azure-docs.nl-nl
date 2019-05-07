---
title: Trainen van modellen met TensorFlow & Keras
titleSuffix: Azure Machine Learning service
description: Meer informatie over het uitvoeren van één knooppunt en gedistribueerde TensorFlow- en Keras-modellen-training met de loopt TensorFlow en Keras
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 05/06/2019
ms.custom: seodec18
ms.openlocfilehash: c8865c851f394d73b5446ac159b5a7799c0c9ed2
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65192356"
---
# <a name="train-tensorflow-and-keras-models-with-azure-machine-learning-service"></a>Train TensorFlow en Keras-modellen met Azure Machine Learning-service

U kunt eenvoudig TensorFlow-trainingstaken uitvoeren op Azure-rekenen met behulp van de [ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) estimator-klasse in de SDK van Azure Machine Learning. De `TensorFlow` estimator zorgt ervoor dat de service Azure Machine Learning aan uw taak uitvoeren op een container TensorFlow-functionaliteit voor Deep Neural Network DNN ()-training.

De `TensorFlow` estimator biedt ook een laag van abstractie die gedurende de uitvoering, wat betekent dat u eenvoudig met parameters wordt uitgevoerd op verschillende compute-doelen configureren kunt zonder uw trainingsscripts te wijzigen.

## <a name="getting-started"></a>Aan de slag

Verzenden van taken met de `TensorFlow` estimator is vergelijkbaar met het gebruik van de base [ `Estimator` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py). Daarom aangeraden voor beginnen met het lezen van de [base artikel Estimator](how-to-train-ml-models.md) eerst de overkoepelende concepten begrijpen.

Als u wilt aan de slag met Azure Machine Learning-service, [voltooien van de snelstartgids](quickstart-run-cloud-notebook.md). Hebt u een werkomgeving geladen met al onze [voorbeeld notitieblokken](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml).

## <a name="single-node-training"></a>Training voor één knooppunt

Exemplaar om uit te voeren een taak TensorFlow, maken van een [ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) object en dient dit in als een experiment.

De volgende code wordt een estimator TensorFlow en verzendt deze als een experiment. Het trainingsscript `train.py` met behulp van de scriptparameters van het opgegeven wordt uitgevoerd. De taak wordt uitgevoerd op een GPU-functionaliteit [compute-doel](how-to-set-up-training-targets.md), en scikit-meer zullen worden geïnstalleerd als een afhankelijkheid voor `train.py`.

```Python
from azureml.train.dnn import TensorFlow

# training script parameters passed as command-line arguments
script_params = {
    '--batch-size': 50,
    '--learning-rate': 0.01,
}

# TensorFlow constructor
tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='train.py', # relative path to your TensorFlow job
                    conda_packages=['scikit-learn'],
                    use_gpu=True)

# submit the TensorFlow job
run = exp.submit(tf_est)
```

## <a name="distributed-training"></a>Gedistribueerde training

De [ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) estimator biedt ook ondersteuning voor gedistribueerde training over CPU en GPU-clusters. U kunt eenvoudig gedistribueerde TensorFlow-taken uitvoeren en Azure Machine Learning-service beheert de infrastructuur en indeling voor u.

Azure Machine Learning-service ondersteunt twee methoden van gedistribueerde training in TensorFlow:

* [Op basis van een MPI](https://www.open-mpi.org/) gedistribueerd training met behulp van de [Horovod](https://github.com/uber/horovod) framework
* Systeemeigen [gedistribueerde TensorFlow](https://www.tensorflow.org/deploy/distributed) met behulp van de methode van de parameter-server

### <a name="horovod"></a>Horovod

[Horovod](https://github.com/uber/horovod) is een open-source framework voor gedistribueerde cursussen ontwikkeld door Uber. Het biedt een eenvoudige manier gedistribueerde TensorFlow GPU-taken.

Het volgende voorbeeld wordt een gedistribueerde trainingstaak met behulp van Horovod met twee werkrollen verdeeld over twee knooppunten uitgevoerd.

```Python
from azureml.train.dnn import TensorFlow

# Tensorflow constructor
tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py', # relative path to your TensorFlow job
                    node_count=2,
                    process_count_per_node=1,
                    distributed_backend='mpi', # specifies Horovod backend
                    use_gpu=True)

# submit the TensorFlow job
run = exp.submit(tf_est)
```

Horovod en de bijbehorende afhankelijkheden wordt voor u worden geïnstalleerd, zodat u deze in uw trainingsscript importeren kunt.

```Python
import tensorflow as tf
import horovod
```

### <a name="parameter-server"></a>Parameterserver

U kunt ook uitvoeren [native gedistribueerde TensorFlow](https://www.tensorflow.org/deploy/distributed), waarbij het servermodel parameter worden gebruikt. Bij deze methode hoeft trainen u in een cluster van de parameter-servers en werknemers. De werknemers Bereken de verlopen tijdens de training, terwijl de parameter-servers de verlopen aggregeren.

Het volgende voorbeeld wordt een gedistribueerde trainingstaak met behulp van de methode van de parameter-server met vier werknemers die zijn verdeeld over twee knooppunten uitgevoerd.

```Python
from azureml.train.dnn import TensorFlow

# Tensorflow constructor
tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py', # relative path to your TensorFlow job
                    node_count=2,
                    worker_count=2,
                    parameter_server_count=1,
                    distributed_backend='ps', # specifies parameter server backend
                    use_gpu=True)

# submit the TensorFlow job
run = exp.submit(tf_est)
```

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

Voor de hoge mate van TensorFlow [ `tf.estimator` ](https://www.tensorflow.org/api_docs/python/tf/estimator) API, TensorFlow parseren dit `TF_CONFIG` variabele en het buildnummer van het cluster specificaties voor u.

Parseren voor TensorFlow van lager niveau core API's voor training, de `TF_CONFIG` variabele en bouwen de `tf.train.ClusterSpec` in uw trainingen-code. In [in dit voorbeeld](https://aka.ms/aml-notebook-tf-ps), doet u dit in **uw trainingsscript** als volgt:

```Python
import os, json
import tensorflow as tf

tf_config = os.environ.get('TF_CONFIG')
if not tf_config or tf_config == "":
    raise ValueError("TF_CONFIG not found.")
tf_config_json = json.loads(tf_config)
cluster_spec = tf.train.ClusterSpec(cluster)

```

## <a name="keras-support"></a>Ondersteuning van Keras

[Keras](https://keras.io/) is een populaire en op hoog niveau DNN Python API die ondersteuning biedt voor TensorFlow, CNTK en Theano als back-ends. Als u TensorFlow als back-end gebruikt, het toevoegen van Keras is net zo eenvoudig als met inbegrip van een `pip_package` constructorparameter.

Het volgende voorbeeld wordt een [ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) estimator en verzendt deze als een experiment. De estimator voert het script van de training Keras `keras_train.py`. De taak wordt uitgevoerd op een gpu-functionaliteit [compute-doel](how-to-set-up-training-targets.md) met Keras geïnstalleerd als een afhankelijkheid via pip.

```Python
from azureml.train.dnn import TensorFlow

keras_est = TensorFlow(source_directory='./my-keras-proj',
                       script_params=script_params,
                       compute_target=compute_target,
                       entry_script='keras_train.py', # relative path to your TensorFlow job
                       pip_packages=['keras'], # add keras through pip
                       use_gpu=True)
```

## <a name="export-to-onnx"></a>Exporteren naar de ONNX

Om op te halen geoptimaliseerde inferentietaken met de [ONNX-Runtime](concept-onnx.md), kunt u het getrainde model voor TensorFlow converteren naar de ONNX-indeling. Zie de [voorbeeld](https://github.com/onnx/tensorflow-onnx/blob/master/examples/call_coverter_via_python.py).

## <a name="examples"></a>Voorbeelden

U kunt werken-codevoorbeelden vinden voor één knooppunt en het gedistribueerde TensorFlow-uitvoeringen met behulp van verschillende frameworks op [onze GitHub-pagina](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning).

## <a name="next-steps"></a>Volgende stappen

* [Metrische gegevens over uitvoeren tijdens de training bijhouden](how-to-track-experiments.md)
* [Afstemmen van hyperparameters](how-to-tune-hyperparameters.md)
* [Een getraind model implementeren](how-to-deploy-and-where.md)