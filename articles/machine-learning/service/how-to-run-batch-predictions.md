---
title: Batch voorspellingen op grote hoeveelheden gegevens uitvoeren
titleSuffix: Azure Machine Learning service
description: Leer hoe u batch om voorspellingen te doen asynchroon op grote hoeveelheden gegevens met behulp van Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens, garye
ms.author: jordane
author: jpe316
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: cd9b0a117439f603e4820cb2dec2984aa2d68e73
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53074329"
---
# <a name="run-batch-predictions-on-large-data-sets-with-azure-machine-learning-service"></a>Batch voorspellingen uitvoeren op grote gegevenssets met Azure Machine Learning-service

In dit artikel leert u hoe u snel en efficiënt om voorspellingen te doen van grote hoeveelheden gegevens asynchroon met behulp van Azure Machine Learning-service.

Batch voorspelling (of batchscore) biedt rendabele Deductie met ongeëvenaarde doorvoer voor asynchrone toepassingen. Batch voorspelling pijplijnen kunnen schalen om uit te voeren Deductie op terabytes aan productiegegevens. Voorspelling van de batch is geoptimaliseerd om een hoge doorvoersnelheid en voorspellingen fire-and-forgetstromen voor een grote verzameling van gegevens.

>[!NOTE]
> Als uw systeem met lage latentie verwerking (proces een document of een kleine documenten snel set) is vereist, gebruikt u [realtime scoren](how-to-consume-web-service.md) in plaats van batch voorspelling.

In de volgende stappen maakt u een [machine learning-pijplijn](concept-ml-pipelines.md) een dat computer vision-model te registreren ([oprichting V3](https://arxiv.org/abs/1512.00567)) en vervolgens het pretrained model gebruiken om u te batch scoring in afbeeldingen beschikbaar in uw Azure blob-account. Deze installatiekopieën gebruikt voor het scoren zijn niet-gelabelde afbeeldingen van de [ImageNet](http://image-net.org/) gegevensset.

## <a name="prerequisites"></a>Vereisten

- U hebt een Azure-abonnement nodig. Als u nog geen Azure-abonnement hebt, maakt u een  [gratis account](https://aka.ms/AMLfree) aan voordat u begint.

- Configureer uw ontwikkelomgeving voor het installeren van de SDK van Azure Machine Learning. Zie voor meer informatie, [een ontwikkelomgeving configureren voor Azure Machine Learning](how-to-configure-environment.md).

- Maak een Azure Machine Learning-werkruimte waarin alle resources van uw pijplijn. U kunt de volgende code gebruiken of Zie voor meer opties [maken van een configuratiebestand werkruimte](how-to-configure-environment.md#workspace).

  ```python
  ws = Workspace.create(
     name = '<workspace-name>',
     subscription_id = '<subscription-id>',
     resource_group = '<resource-group>',
     location = '<workspace_region>',
     exist_ok = True)
  ```

## <a name="set-up-machine-learning-resources"></a>Machine learning-resources instellen

De volgende stappen uit wordt om de resources die u wilt uitvoeren van een pijplijn stellen:

- Toegang tot het gegevensarchief die al heeft het model dat, invoer labels en afbeeldingen om te beoordelen (dit is al ingesteld voor u).
- Instellen van een gegevensarchief voor het opslaan van de uitvoer.
- Configureer DataReference objecten om te verwijzen naar de gegevens in de voorgaande gegevensopslag.
- Instellen van de compute-machines of clusters waarop de pijplijn-stappen uitvoert.

### <a name="access-the-datastores"></a>Toegang tot de gegevensopslag

Eerst toegang krijgen tot de gegevensopslag met het model, labels en afbeeldingen.

U gebruikt een openbare blob-container met de naam *sampledata* in de *pipelinedata* account waarin afbeeldingen uit de set ImageNet-evaluatie. De naam van de gegevensopslag voor deze openbare container is *images_datastore*. Registreer deze gegevensopslag met uw werkruimte:

```python
# Public blob container details
account_name = "pipelinedata"
datastore_name="images_datastore"
container_name="sampledata"
 
batchscore_blob = Datastore.register_azure_blob_container(ws,
                      datastore_name=datastore_name,
                      container_name= container_name,
                      account_name=account_name,
                      overwrite=True)
```

Installatie van de volgende, met de standaard gegevensopslag voor de uitvoer.

Wanneer u uw werkruimte maakt een [Azure filestorage](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) en een [blobopslag](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) zijn gekoppeld aan de werkruimte standaard. Azure file storage is de 'standaard-gegevensopslag' voor een werkruimte, maar u kunt ook de blob-opslag gebruiken als gegevensopslag. Meer informatie over [opslagopties van Azure](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks).

```python
def_data_store = ws.get_default_datastore()
```

### <a name="configure-data-references"></a>Gegevensverwijzingen configureren

Nu verwijzen naar de gegevens in de pijplijn als invoer voor de stappen van de pijplijn.

Een gegevensbron in een pijplijn wordt vertegenwoordigd door een [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference) object. Het object DataReference verwijst naar gegevens die zich bevinden of uit een gegevensarchief toegankelijk is. U DataReference objecten voor de map die wordt gebruikt voor invoer installatiekopieën van de directory waarin het pretrained model is opgeslagen, moet de map voor de labels en de uitvoermap.

```python
input_images = DataReference(datastore=batchscore_blob, 
                             data_reference_name="input_images",
                             path_on_datastore="batchscoring/images",
                             mode="download")
                           
model_dir = DataReference(datastore=batchscore_blob, 
                          data_reference_name="input_model",
                          path_on_datastore="batchscoring/models",
                          mode="download")                          
                         
label_dir = DataReference(datastore=batchscore_blob, 
                          data_reference_name="input_labels",
                          path_on_datastore="batchscoring/labels",
                          mode="download")                          
                         
output_dir = PipelineData(name="scores", 
                          datastore=def_data_store, 
                          output_path_on_compute="batchscoring/results")
```

### <a name="set-up-compute-target"></a>Compute-doel instellen

In Azure Machine Learning verwijst compute (of compute-doel) naar de machines of clusters die door de rekenkundige stappen worden uitgevoerd in de machine learning-pijplijn. Bijvoorbeeld, kunt u een `Azure Machine Learning compute`.

```python
compute_name = "gpucluster"
compute_min_nodes = 0
compute_max_nodes = 4
vm_size = "STANDARD_NC6"

if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('Found compute target. just use it. ' + compute_name)
else:
    print('Creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(
                     vm_size = vm_size, # NC6 is GPU-enabled
                     vm_priority = 'lowpriority', # optional
                     min_nodes = compute_min_nodes, 
                     max_nodes = compute_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(ws, 
                        compute_name, 
                        provisioning_config)
    
    compute_target.wait_for_completion(
                     show_output=True, 
                     min_node_count=None, 
                     timeout_in_minutes=20)
```

## <a name="prepare-the-model"></a>Voorbereiden van het model

Voordat u het pretrained model gebruiken kunt, moet u het model downloaden en te registreren bij uw werkruimte.

### <a name="download-the-pretrained-model"></a>Download het pretrained model

Download de dat computer vision-model (InceptionV3) van <http://download.tensorflow.org/models/inception_v3_2016_08_28.tar.gz>. Nadat u hebt gedownload, uitpakken naar de `models` submap.

```python
import os
import tarfile
import urllib.request

model_dir = 'models'
if not os.path.isdir(model_dir):
    os.mkdir(model_dir)

url="http://download.tensorflow.org/models/inception_v3_2016_08_28.tar.gz"
response = urllib.request.urlretrieve(url, "model.tar.gz")
tar = tarfile.open("model.tar.gz", "r:gz")
tar.extractall(model_dir)
```

### <a name="register-the-model"></a>Registreer het model

```python
import shutil
from azureml.core.model import Model

# register downloaded model 
model = Model.register(
        model_path = "models/inception_v3.ckpt",
        model_name = "inception", # This is the name of the registered model
        tags = {'pretrained': "inception"},
        description = "Imagenet trained tensorflow inception",
        workspace = ws)
```

## <a name="write-your-scoring-script"></a>De scoring-script schrijven

>[!Warning]
>De volgende code is slechts een voorbeeld van wat is opgenomen in de [batch_score.py](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline/batch_score.py) die worden gebruikt door de [voorbeeld notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline/pipeline-batch-scoring.ipynb) , moet u uw eigen scoring-script voor uw scenario te maken.

De `batch_score.py` script neemt invoer afbeeldingen *dataset_path*, modellen in dat *model_dir,* en levert *resultaten label.txt* naar *output_dir*.

```python
# Snippets from a sample scoring script
# Refer to the accompanying batch-scoring Notebook
# https://github.com/Azure/MachineLearningNotebooks/blob/master/pipeline/pipeline-batch-scoring.ipynb
# for the implementation script

# Get labels
def get_class_label_dict(label_file):
  label = []
  proto_as_ascii_lines = tf.gfile.GFile(label_file).readlines()
  for l in proto_as_ascii_lines:
    label.append(l.rstrip())
  return label

class DataIterator:
  # Definition of the DataIterator here
  
def main(_):
    # Refer to batch-scoring Notebook for implementation.
    label_file_name = os.path.join(args.label_dir, "labels.txt")
    label_dict = get_class_label_dict(label_file_name)
    classes_num = len(label_dict)
    test_feeder = DataIterator(data_dir=args.dataset_path)
    total_size = len(test_feeder.labels)

    # get model from model registry
    model_path = Model.get_model_path(args.model_name)
    with tf.Session() as sess:
        test_images = test_feeder.input_pipeline(batch_size=args.batch_size)
        with slim.arg_scope(inception_v3.inception_v3_arg_scope()):
            input_images = tf.placeholder(tf.float32, [args.batch_size, image_size, image_size, num_channel])
            logits, _ = inception_v3.inception_v3(input_images,
                                                        num_classes=classes_num,
                                                        is_training=False)
            probabilities = tf.argmax(logits, 1)

        sess.run(tf.global_variables_initializer())
        sess.run(tf.local_variables_initializer())
        coord = tf.train.Coordinator()
        threads = tf.train.start_queue_runners(sess=sess, coord=coord)
        saver = tf.train.Saver()
        saver.restore(sess, model_path)
        out_filename = os.path.join(args.output_dir, "result-labels.txt")
            
        # copy the file to artifacts
        shutil.copy(out_filename, "./outputs/")
```

## <a name="build-and-run-the-batch-scoring-pipeline"></a>Bouwen en uitvoeren van de batchscore-pijplijn

Hebt u alles wat die u moet de pijplijn bouwen, nu dus we alles combineren.

### <a name="prepare-the-run-environment"></a>Voorbereiden van de uitvoeringsomgeving

Geef het conda-afhankelijkheden voor het script. U moet dit object wanneer u de stap pijplijn later maakt.

```python
from azureml.core.runconfig import DEFAULT_GPU_IMAGE

cd = CondaDependencies.create(pip_packages=["tensorflow-gpu==1.10.0", "azureml-defaults"])

# Runconfig
amlcompute_run_config = RunConfiguration(conda_dependencies=cd)
amlcompute_run_config.environment.docker.enabled = True
amlcompute_run_config.environment.docker.gpu_support = True
amlcompute_run_config.environment.docker.base_image = DEFAULT_GPU_IMAGE
amlcompute_run_config.environment.spark.precache_packages = False
```

### <a name="specify-the-parameter-for-your-pipeline"></a>Geef de parameter voor de pijplijn

Maak een pijplijn parameter met een [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py) object met een standaardwaarde.

```python
batch_size_param = PipelineParameter(
                    name="param_batch_size", 
                    default_value=20)
```

### <a name="create-the-pipeline-step"></a>De stap pijplijn maken

Maak de pijplijn stap met behulp van het script, de configuratie van de omgeving en de parameters. Geef de compute-doel dat u al gekoppeld aan uw werkruimte als het doel van de uitvoering van het script. Gebruik [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py) om de stap van de pijplijn te maken.

```python
inception_model_name = "inception_v3.ckpt"

batch_score_step = PythonScriptStep(
    name="batch_scoring",
    script_name="batch_score.py",
    arguments=["--dataset_path", input_images, 
               "--model_name", "inception",
               "--label_dir", label_dir, 
               "--output_dir", output_dir, 
               "--batch_size", batch_size_param],
    compute_target=compute_target,
    inputs=[input_images, label_dir],
    outputs=[output_dir],
    runconfig=amlcompute_run_config,
    source_directory=scripts_folder
```

### <a name="run-the-pipeline"></a>De pijplijn uitvoeren

Nu de uitvoering van de pijplijn en bekijk de uitvoer geproduceerd. De uitvoer heeft een score die overeenkomt met elke afbeelding.

```python
# Run the pipeline
pipeline = Pipeline(workspace=ws, steps=[batch_score_step])
pipeline_run = Experiment(ws, 'batch_scoring').submit(pipeline, pipeline_params={"param_batch_size": 20})

# Wait for the run to finish (this may take several minutes)
pipeline_run.wait_for_completion(show_output=True)

# Download and review the output
step_run = list(pipeline_run.get_children())[0]
step_run.download_file("./outputs/result-labels.txt")

import pandas as pd
df = pd.read_csv("result-labels.txt", delimiter=":", header=None)
df.columns = ["Filename", "Prediction"]
df.head()
```

## <a name="publish-the-pipeline"></a>De pijplijn publiceren

Wanneer u tevreden met het resultaat van de uitvoering bent, publiceert u de pijplijn, zodat u deze later met verschillende waarden voor de invoer kunt uitvoeren. Als u een pijplijn publiceert, krijgt u een REST-eindpunt dat wordt aangeroepen met de set parameters die u hebt al verwerkt met behulp van de pijplijn accepteert [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py).

```python
published_pipeline = pipeline_run.publish_pipeline(
    name="Inception_v3_scoring", 
    description="Batch scoring using Inception v3 model", 
    version="1.0")
```

## <a name="rerun-the-pipeline-using-the-rest-endpoint"></a>De pijplijn met behulp van de REST-eindpunt opnieuw uitvoeren

Als u wilt de pijplijn opnieuw uitvoeren, moet u een Azure Active Directory-verificatietoken header zoals beschreven in [AzureCliAuthentication klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.azurecliauthentication?view=azure-ml-py).

```python
from azureml.pipeline.core import PublishedPipeline

rest_endpoint = published_pipeline.endpoint
# specify batch size when running the pipeline
response = requests.post(rest_endpoint, 
        headers=aad_token, 
        json={"ExperimentName": "batch_scoring",
               "ParameterAssignments": {"param_batch_size": 50}})

# Monitor the run
from azureml.pipeline.core.run import PipelineRun
published_pipeline_run = PipelineRun(ws.experiments["batch_scoring"], run_id)

RunDetails(published_pipeline_run).show()
```

## <a name="next-steps"></a>Volgende stappen

Als u wilt zien deze werkende end-to-end, probeert de batchscore-notebook in ([how-to-use-azureml/machine-learning-pipelines](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines). 

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

