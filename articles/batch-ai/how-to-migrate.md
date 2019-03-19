---
title: Migreren van Batch AI naar Azure Machine Learning-service
description: Meer informatie over het migreren naar Azure Machine Learning-service voor de AMLcompute en hoe uw code wordt toegewezen aan de code in Azure Machine Learning-service.
ms.service: batch-ai
services: batch-ai
ms.topic: overview
ms.author: jmartens
author: j-martens
ms.date: 2/28/2019
ms.openlocfilehash: e495ed06c640601c0500d14b42070a264fd687a9
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57862111"
---
# <a name="migrating-from-batch-ai-to-azure-machine-learning-service"></a>Migreren van Batch AI naar Azure Machine Learning-service

**De Azure Batch AI-service wordt in maart buiten gebruik gesteld.** De op-schaal-training en scoremogelijkheden van Batch AI zijn nu beschikbaar in de [Azure Machine Learning-service](../machine-learning/service/overview-what-is-azure-ml.md), die algemeen beschikbaar werd op 4 december 2018.

Samen met veel andere machine learning-mogelijkheden bevat de Azure Machine Learning-service een cloud-gebaseerd, beheerd rekendoel voor het trainen, implementeren en scoren van Machine Learning-modellen. Dit rekendoel heet [Azure Machine Learning Compute](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute). Beginnen met migreren en vandaag gebruikt. U kunt met de Azure Machine Learning-service communiceren via de [Python-SDK's](../machine-learning/service/quickstart-create-workspace-with-python.md), opdrachtregelinterface en de [Azure Portal](../machine-learning/service/quickstart-get-started.md).

Een upgrade uitvoert van de Preview-versie Batch AI met de GA'ed Azure Machine Learning-service biedt u een betere ervaring via concepten die gemakkelijker zijn te gebruiken, zoals loopt en gegevensopslag. Bovendien profiteert u van Azure-SLA's en klantondersteuning op algemeen beschikbaar niveau.

Azure Machine-Learning wordt service ook in de nieuwe functionaliteit, zoals brengt geautomatiseerde machine learning, hyperparameter afstemmen en ML-pijplijnen die nuttig in de meeste grootschalige AI-workloads zijn. De mogelijkheid tot het implementeren van een getraind model over te schakelen op een afzonderlijke service kunt u de data science-lus van voorbereiden van gegevens (met behulp van de SDK voor Data Prep) voltooien helemaal uitoefening en model bewaking.

## <a name="start-migrating"></a>Beginnen met het migreren
Als u onderbrekingen in uw toepassingen wilt voorkomen en wilt profiteren van de nieuwste functies, moet u de volgende stappen uitvoeren vóór 31 maart 2019:

1. Een werkruimte maken in de Azure Machine Learning-service en aan de slag gaan:
    + [Snelstart op basis van Python](../machine-learning/service/quickstart-create-workspace-with-python.md)
    + [Snelstart op basis van Azure Portal](../machine-learning/service/quickstart-get-started.md)

1. Installeer de [Azure Machine Learning-SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) en [Dataprep SDK](https://docs.microsoft.com/python/api/overview/azure/dataprep/intro?view=azure-dataprep-py#install). 

1. Een [Azure Machine Learning Compute](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute)-instantie installeren voor modeltraining.

1. Werk uw scripts bij voor het gebruik van Azure Machine Learning Compute. De volgende secties ziet u hoe algemene code die u voor Batch AI-toegewezen aan de code voor Azure Machine Learning gebruiken. 


## <a name="create-workspaces"></a>Werkruimten maken
Het concept van een werkruimte met behulp van een configuration.json in Azure Batch AI tijdens de initialisatie van kaarten op dezelfde manier voor het gebruik van een configuratiebestand in Azure Machine Learning-service.

In **Batch AI** hebt u het als volgt gedaan:

```python
sys.path.append('../../..')
import utilities as utils

cfg = utils.config.Configuration('../../configuration.json')
client = utils.config.create_batchai_client(cfg)

utils.config.create_resource_group(cfg)
_ = client.workspaces.create(cfg.resource_group, cfg.workspace, cfg.location).result()
```

In **Azure Machine Learning Service** kunt u het als volgt doen:

```python
from azureml.core.workspace import Workspace

ws = Workspace.from_config()
print('Workspace name: ' + ws.name, 
      'Azure region: ' + ws.location, 
      'Subscription id: ' + ws.subscription_id, 
      'Resource group: ' + ws.resource_group, sep = '\n')
```

Bovendien kunt u rechtstreeks een werkruimte maken door de configuratieparameters op te geven, bijvoorbeeld

```python
from azureml.core import Workspace
# Create the workspace using the specified parameters
ws = Workspace.create(name = workspace_name,
                      subscription_id = subscription_id,
                      resource_group = resource_group, 
                      location = workspace_region,
                      create_resource_group = True,
                      exist_ok = True)
ws.get_details()

# write the details of the workspace to a configuration file to the notebook library
ws.write_config()
```

Meer informatie over de klasse Azure Machine Learning-werkruimte in de [SDK-referentiedocumentatie](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py).


## <a name="create-compute-clusters"></a>Computerclusters maken
Azure Machine Learning ondersteunt meerdere rekendoelen, waarvan sommige door de service worden beheerd; andere kunnen aan uw werkruimte worden gekoppeld (bijvoorbeeld een HDInsight-cluster of een externe VM. Lees meer over de verschillende [rekendoelen](../machine-learning/service/how-to-set-up-training-targets.md). Het concept van het maken van een Azure Batch AI compute cluster is toegewezen aan een AmlCompute-cluster maken in Azure Machine Learning-service. Het maken van de Amlcompute duurt in een vergelijkbaar met hoe u kunt parameters in Azure Batch AI doorgeven compute-configuratie. Eén ding te weten is dat automatisch schalen is standaard ingeschakeld in uw cluster AmlCompute terwijl deze standaard in Azure Batch AI is uitgeschakeld.

In **Batch AI** hebt u het als volgt gedaan:

```python
nodes_count = 2
cluster_name = 'nc6'

parameters = models.ClusterCreateParameters(
    vm_size='STANDARD_NC6',
    scale_settings=models.ScaleSettings(
        manual=models.ManualScaleSettings(target_node_count=nodes_count)
    ),
    user_account_settings=models.UserAccountSettings(
        admin_user_name=cfg.admin,
        admin_user_password=cfg.admin_password or None,
        admin_user_ssh_public_key=cfg.admin_ssh_key or None,
    )
)
_ = client.clusters.create(cfg.resource_group, cfg.workspace, cluster_name, parameters).result()
```

In **Azure Machine Learning Service** kunt u het volgende doen:

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
gpu_cluster_name = "nc6"

# Verify that cluster does not exist already
try:
    gpu_cluster = ComputeTarget(workspace=ws, name=gpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6',
                                                           vm_priority='lowpriority',
                                                           min_nodes=1,
                                                           max_nodes=2,
                                                           idle_seconds_before_scaledown='300',
                                                           vnet_resourcegroup_name='<my-resource-group>',
                                                           vnet_name='<my-vnet-name>',
                                                           subnet_name='<my-subnet-name>')
    gpu_cluster = ComputeTarget.create(ws, gpu_cluster_name, compute_config)

gpu_cluster.wait_for_completion(show_output=True)
```

Meer informatie over de AMLCompute-klasse vindt u in de [SDK-naslagdocumentatie](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py) (Engelstalig). Merk op dat in de bovenstaande configuratie alleen vm_size en max_nodes verplicht zijn. De overige eigenschappen, zoals VNet's, zijn alleen bedoeld voor het instellen van geavanceerde clusters.

## <a name="monitor-status-of-your-cluster"></a>De bewakingsstatus van uw cluster
Dit is eenvoudiger in Azure Machine Learning-service, u hieronder ziet.

In **Batch AI** hebt u het als volgt gedaan:

```python
cluster = client.clusters.get(cfg.resource_group, cfg.workspace, cluster_name)
utils.cluster.print_cluster_status(cluster)
```

In **Azure Machine Learning Service** kunt u het volgende doen:

```python
gpu_cluster.get_status().serialize()
```

## <a name="get-reference-to-a-storage-account"></a>Verwijzing naar een opslagaccount ophalen
Het concept van een gegevensopslag zoals blob, haalt vereenvoudigd in Azure Machine Learning-service met behulp van het object gegevensopslag. Uw werkruimte van Azure Machine Learning-service maakt standaard een storage-account, maar kunt u ook uw eigen opslag koppelen als onderdeel van een werkruimte maken. 

In **Batch AI** hebt u het als volgt gedaan:

```python
azure_blob_container_name = 'batchaisample'
blob_service = BlockBlobService(cfg.storage_account_name, cfg.storage_account_key)
blob_service.create_container(azure_blob_container_name, fail_on_exist=False)
```

In **Azure Machine Learning Service** kunt u het volgende doen:

```python
ds = ws.get_default_datastore()
print(ds.datastore_type, ds.account_name, ds.container_name)
```

Meer informatie over het registreren van extra opslagaccounts, of voor het verkrijgen van een verwijzing naar een andere geregistreerde gegevensopslag in de [documentatie voor Azure Machine Learning-service](../machine-learning/service/how-to-access-data.md#access).


## <a name="download-and-upload-data"></a>Download en gegevens uploaden 
Met beide services kunt u de gegevens eenvoudig naar het opslagaccount uploaden door middel van de hierboven genoemde verwijzing naar de gegevensopslag. Voor Azure Batch AI implementeren we ook het trainingsscript als onderdeel van de bestandsshare, hoewel u zult zien hoe u het kunt opgeven als onderdeel van de taakconfiguratie van uw in het geval van Azure Machine Learning-service.

In **Batch AI** hebt u het als volgt gedaan:

```python
mnist_dataset_directory = 'mnist_dataset'
utils.dataset.download_and_upload_mnist_dataset_to_blob(
    blob_service, azure_blob_container_name, mnist_dataset_directory)

script_directory = 'tensorflow_samples'
script_to_deploy = 'mnist_replica.py'

blob_service.create_blob_from_path(azure_blob_container_name,
                                   script_directory + '/' + script_to_deploy, 
                                   script_to_deploy)
```


In **Azure Machine Learning Service** kunt u het volgende doen:

```python
import os
import urllib
os.makedirs('./data', exist_ok=True)
download_url = 'https://s3.amazonaws.com/img-datasets/mnist.npz'
urllib.request.urlretrieve(download_url, filename='data/mnist.npz')

ds.upload(src_dir='data', target_path='mnist_dataset', overwrite=True, show_progress=True)

path_on_datastore = ' mnist_dataset/mnist.npz' ds_data = ds.path(path_on_datastore) print(ds_data)
```

## <a name="create-experiments"></a>Experimenten maken
Azure Machine Learning-service beschikt over een concept van een experiment die vergelijkbaar is met Azure Batch AI zoals hierboven vermeld. Elke experiment kan vervolgens afzonderlijke wordt uitgevoerd, vergelijkbaar met hoe we taken in Azure Batch AI hebben hebben. Azure Machine Learning-service kunt u de hiërarchie onder elk bovenliggend item uitvoeren voor afzonderlijke onderliggende worden uitgevoerd.

In **Batch AI** hebt u het als volgt gedaan:

```python
experiment_name = 'tensorflow_experiment'
experiment = client.experiments.create(cfg.resource_group, cfg.workspace, experiment_name).result()
```

In **Azure Machine Learning Service** kunt u het volgende doen:

```python
from azureml.core import Experiment

experiment_name = 'tensorflow_experiment'
experiment = Experiment(ws, name=experiment_name)
```


## <a name="submit-jobs"></a>Verzenden van taken
Als u een experiment maakt, kunt u een uitvoering op meerdere manieren indienen. In dit voorbeeld we probeert te maken van een deep learning-model met behulp van TensorFlow en wordt een Azure Machine Learning-service Estimator gebruiken om dat te doen. Een [Estimator](../machine-learning/service/how-to-train-ml-models.md) is eenvoudigweg een wrapperfunctie voor de onderliggende uitvoeringsconfiguratie. Hierdoor wordt het makkelijker om uitvoeringen in te dienen. De functie wordt momenteel alleen ondersteund voor Pytorch en TensorFlow. Dankzij het concept voor gegevensopslag ziet u bovendien hoe eenvoudig het nu is om de koppelingspaden op te geven 

In **Batch AI** hebt u het als volgt gedaan:

```python
azure_file_share = 'afs'
azure_blob = 'bfs'
args_fmt = '--job_name={0} --num_gpus=1 --train_steps 10000 --checkpoint_dir=$AZ_BATCHAI_OUTPUT_MODEL --log_dir=$AZ_BATCHAI_OUTPUT_TENSORBOARD --data_dir=$AZ_BATCHAI_INPUT_DATASET --ps_hosts=$AZ_BATCHAI_PS_HOSTS --worker_hosts=$AZ_BATCHAI_WORKER_HOSTS --task_index=$AZ_BATCHAI_TASK_INDEX'

parameters = models.JobCreateParameters(
     cluster=models.ResourceId(id=cluster.id),
     node_count=2,
     input_directories=[
        models.InputDirectory(
            id='SCRIPT',
            path='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}/{1}'.format(azure_blob, script_directory)),
        models.InputDirectory(
            id='DATASET',
            path='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}/{1}'.format(azure_blob, mnist_dataset_directory))],
     std_out_err_path_prefix='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}'.format(azure_file_share),
     output_directories=[
        models.OutputDirectory(
            id='MODEL',
            path_prefix='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}'.format(azure_file_share),
            path_suffix='Models'),
        models.OutputDirectory(
            id='TENSORBOARD',
            path_prefix='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}'.format(azure_file_share),
            path_suffix='Logs')
     ],
     mount_volumes=models.MountVolumes(
            azure_file_shares=[
                models.AzureFileShareReference(
                    account_name=cfg.storage_account_name,
                    credentials=models.AzureStorageCredentialsInfo(
                        account_key=cfg.storage_account_key),
                    azure_file_url='https://{0}.file.core.windows.net/{1}'.format(
                        cfg.storage_account_name, azure_file_share_name),
                    relative_mount_path=azure_file_share)
            ],
            azure_blob_file_systems=[
                models.AzureBlobFileSystemReference(
                    account_name=cfg.storage_account_name,
                    credentials=models.AzureStorageCredentialsInfo(
                        account_key=cfg.storage_account_key),
                    container_name=azure_blob_container_name,
                    relative_mount_path=azure_blob)
            ]
        ),
     container_settings=models.ContainerSettings(
         image_source_registry=models.ImageSourceRegistry(image='tensorflow/tensorflow:1.8.0-gpu')),
     tensor_flow_settings=models.TensorFlowSettings(
         parameter_server_count=1,
         worker_count=nodes_count,
         python_script_file_path='$AZ_BATCHAI_INPUT_SCRIPT/'+ script_to_deploy,
         master_command_line_args=args_fmt.format('worker'),
         worker_command_line_args=args_fmt.format('worker'),
         parameter_server_command_line_args=args_fmt.format('ps'),
     )
)
```

Het indienen van de taak is zelf in Azure Batch AI via de functie maken.

```python
job_name = datetime.utcnow().strftime('tf_%m_%d_%Y_%H%M%S')
job = client.jobs.create(cfg.resource_group, cfg.workspace, experiment_name, job_name, parameters).result()
print('Created Job {0} in Experiment {1}'.format(job.name, experiment.name))
```

De volledige informatie over deze training codefragment (met inbegrip van de mnist_replica.py-bestand dat we hadden geüpload naar de bovenstaande bestandsshare) vindt u de [github-opslagplaats voor Azure Batch AI-voorbeeld-notebook](https://github.com/Azure/BatchAI/tree/2238607d5a028a0c5e037168aefca7d7bb165d5c/recipes/TensorFlow/TensorFlow-GPU-Distributed).

In **Azure Machine Learning Service** kunt u het volgende doen:

```python
from azureml.train.dnn import TensorFlow

script_params={
    '--num_gpus': 1,
    '--train_steps': 500,
    '--input_data': ds_data.as_mount()

}

estimator = TensorFlow(source_directory=project_folder,
                       compute_target=gpu_cluster,
                       script_params=script_params,
                       entry_script='tf_mnist_replica.py',
                       node_count=2,
                       worker_count=2,
                       parameter_server_count=1,   
                       distributed_backend='ps',
                       use_gpu=True)
```

De volledige informatie over deze training codefragment (met inbegrip van het bestand tf_mnist_replica.py) vindt u de [github-opslagplaats voor Azure Machine Learning-service voorbeeld notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/distributed-tensorflow-with-parameter-server). De gegevensopslag zelf kan worden gekoppeld aan de afzonderlijke knooppunten, maar de trainingsgegevens kunnen ook in het knooppunt zelf worden gedownload. Meer informatie over die verwijst naar het gegevensarchief in uw estimator is in de [documentatie voor Azure Machine Learning-service](../machine-learning/service/how-to-access-data.md#access). 

Indienen van een uitvoering in Azure Machine Learning is-service via de functie verzenden.

```python
run = experiment.submit(estimator)
print(run)
```

U kunt de parameters voor de uitvoering ook opgeven met een uitvoeringsconfiguratie. Dit is met name handig voor het definiëren van een aangepaste trainingsomgeving. Meer informatie vindt u hier: [sample AmlCompute notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-amlcompute/train-on-amlcompute.ipynb) (AmlCompute-voorbeeldnotebook). 

## <a name="monitor-runs"></a>Monitor wordt uitgevoerd
Zodra u een uitvoering verzendt, kunt u ofwel wachten om te voltooien of bewaken in Azure Machine Learning-service met behulp van handige Jupyter-widgets die u rechtstreeks vanuit uw code kunt aanroepen. U kunt ook context uit een voorgaande uitvoering halen door de verschillende experimenten in een werkruimte en de individuele uitvoeringen in elk experiment te doorlopen.

In **Batch AI** hebt u het als volgt gedaan:

```python
utils.job.wait_for_job_completion(client, cfg.resource_group, cfg.workspace, 
                                  experiment_name, job_name, cluster_name, 'stdouterr', 'stdout-wk-0.txt')

files = client.jobs.list_output_files(cfg.resource_group, cfg.workspace, experiment_name, job_name,
                                      models.JobsListOutputFilesOptions(outputdirectoryid='stdouterr')) 
for f in list(files):
    print(f.name, f.download_url or 'directory')
```


In **Azure Machine Learning Service** kunt u het volgende doen:

```python
run.wait_for_completion(show_output=True)

from azureml.widgets import RunDetails
RunDetails(run).show()
```

Hier ziet u een momentopname van hoe de widget in uw notebook wordt geladen, zodat u uw logboeken in realtime kunt bekijken: ![Diagram met widgetbewaking](./media/overview-what-happened-batch-ai/monitor.png)



## <a name="edit-clusters"></a>Clusters bewerken
U kunt een cluster eenvoudige verwijderen. Bovendien kunt Azure Machine Learning-service u ook een cluster op basis van binnen het notitieblok bijwerken als u wilt schalen naar een hoger aantal knooppunten of vergroot de niet-actieve tijd voordat u het cluster schalen. U mag de VM-grootte van het cluster zelf niet wijzigen, omdat hiervoor een nieuwe implementatie aan de back-end noodzakelijk is.

In **Batch AI** hebt u het als volgt gedaan:
```python
_ = client.clusters.delete(cfg.resource_group, cfg.workspace, cluster_name)
```

In **Azure Machine Learning Service** kunt u het volgende doen:

```python
gpu_cluster.delete()

gpu_cluster.update(min_nodes=2, max_nodes=4, idle_seconds_before_scaledown=600)
```

## <a name="get-support"></a>Ondersteuning krijgen

Batch AI staat gepland buiten gebruik stellen op 31 maart en blokkeert al nieuwe abonnementen op basis van de service wordt geregistreerd, tenzij het wordt in de whitelist opgenomen door een uitzondering door middel van ondersteuning.  Als u vragen of feedback hebt over de migratie naar Azure Machine Learning Service, kunt u contact met ons opnemen via [Azure Batch AI Training Preview](mailto:AzureBatchAITrainingPreview@service.microsoft.com).

Azure Machine Learning Service is een algemeen beschikbare service. Dit betekent dat de service wordt geleverd met een bindende SLA en een keuze uit diverse ondersteuningsplannen.

Prijzen voor het gebruik van Azure-infrastructuur via de Azure Batch AI-service of via de Azure Machine Learning-service moet niet verschillen, omdat we alleen de prijs voor de onderliggende berekeningen in beide gevallen berekenen. Zie de [prijscalculator](https://azure.microsoft.com/pricing/details/machine-learning-service/) voor meer informatie.

Bekijk de regionale beschikbaarheid voor de twee services in de [Azure-portal](https://azure.microsoft.com/global-infrastructure/services/?products=batch-ai,machine-learning-service&regions=all).


## <a name="next-steps"></a>Volgende stappen

+ Lees [Overzicht van de Azure Machine Learning-service](../machine-learning/service/overview-what-is-azure-ml.md).

+ [Configureer een rekendoel voor modeltraining](../machine-learning/service/how-to-set-up-training-targets.md) met de Azure Machine Learning-service.

+ Controleer de [Azure-roadmap](https://azure.microsoft.com/updates/) voor meer informatie over andere Azure-service-updates.
