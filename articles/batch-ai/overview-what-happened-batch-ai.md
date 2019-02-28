---
title: Wat gebeurt er met Azure Batch AI? | Microsoft Docs
description: Meer informatie over wat er met Azure Batch AI en de compute-optie van de Azure Machine Learning-service gebeurt.
services: batch-ai
author: garyericson
ms.service: batch-ai
ms.topic: overview
ms.date: 2/14/2019
ms.author: garye
ms.openlocfilehash: fb1114e94c227ce6787532c6059186399d0f57f0
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961348"
---
# <a name="whats-happening-to-azure-batch-ai"></a>Wat gebeurt er met Azure Batch AI?

**De Azure Batch AI-service wordt in maart buiten gebruik gesteld.** De op-schaal-training en scoremogelijkheden van Batch AI zijn nu beschikbaar in de [Azure Machine Learning-service](../machine-learning/service/overview-what-is-azure-ml.md), die algemeen beschikbaar werd op 4 december 2018.

Samen met veel andere machine learning-mogelijkheden bevat de Azure Machine Learning-service een cloud-gebaseerd, beheerd rekendoel voor het trainen, implementeren en scoren van Machine Learning-modellen. Dit rekendoel heet [Azure Machine Learning Compute](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute). [Begin met migreren en maak er vandaag nog gebruik van](#migrate). U kunt met de Azure Machine Learning-service communiceren via de [Python-SDK's](../machine-learning/service/quickstart-create-workspace-with-python.md), opdrachtregelinterface en de [Azure Portal](../machine-learning/service/quickstart-get-started.md).

## <a name="support-timeline"></a>Ondersteuningstijdlijn

| Date | Ondersteuningsgegevens Batch AI-service |
| ---- |-----------------|
| &nbsp;14&#x2c;&nbsp;december 2018| U kunt uw bestaande Azure Batch AI-abonnementen gebruiken als voorheen. Er zijn echter geen **nieuwe abonnementen** mogelijk en er worden geen nieuwe investeringen gedaan.|
| &nbsp;31&#x2c;&nbsp;maart 2019 | Na deze datum functioneren bestaande Batch AI-abonnementen niet meer. |

## <a name="how-does-azure-machine-learning-service-compare"></a>Hoe verhoudt zich Azure Machine Learning Service tot andere services?
Het is een cloudservice waarmee u Machine Learning-modellen kunt trainen, implementeren, automatiseren en beheren, en dit allemaal op de grote schaal van de cloud. [In dit overzicht](../machine-learning/service/overview-what-is-azure-ml.md) kunt u een dieper inzicht krijgen over Azure Machine Learning Service.
 

Bij een typische ontwikkelingslevenscyclus zijn gegevensvoorbereiding, training en experimenten,en een ontwikkelingsfase betrokken. Deze end-to-end-cyclus kan worden ingedeeld met behulp van Machine Learning-pijplijnen.

![Stroomdiagram](./media/overview-what-happened-batch-ai/lifecycle.png)


[Meer informatie over de werking en de belangrijkste concepten van de service](../machine-learning/service/concept-azure-machine-learning-architecture.md). Veel concepten in de werkstroom van de modeltraining zijn soortgelijk aan die in Batch AI. 

Hieronder is een aantal concepten naast elkaar geplaatst:
 
|Batch AI-service|  Azure Machine Learning-service|
|:--:|:---:|
|Werkruimte|Werkruimte|
|Cluster|   Compute van type `AmlCompute`|
|Bestandsservers|Gegevensopslag|
|Experimenten|Experimenten|
|Taken|Uitvoerbewerkingen (staat geneste uitvoerbewerkingen toe)|
 
Hier ziet u een andere weergave van dezelfde tabel, zodat u de zaken op een andere manier kunt visualiseren:
 
**Batch AI-hiërarchie**
![Stroomdiagram](./media/overview-what-happened-batch-ai/batchai-heirarchy.png) 
 
**Azure Machine Learning Service-hiërarchie**
![Stroomdiagram](./media/overview-what-happened-batch-ai/azure-machine-learning-service-heirarchy.png) 

### <a name="platform-capabilities"></a>Platformfunctionaliteiten
Azure Machine Learning Service bevat een geweldige reeks nieuwe functionaliteiten, waaronder een end-to-end-training -> implementatiestack, waarmee u AI kunt ontwikkelen zonder beheer van Azure-resources. In de onderstaande tabel wordt de ondersteuning van functies voor de training tussen de twee services vergeleken.

|Functie|BatchAI-service|Azure Machine Learning-service|
|-------|:-------:|:-------:|
|Keuze van VM-grootte |CPU/GPU    |CPU/GPU. Ondersteunt tevens FPGA voor inferentie|
|Cluster gereed voor AI (stuurprogramma's, Docker, enz.)|  Ja |Ja|
|Knooppuntvoorbereiding| Ja|    Nee|
|Keuze uit besturingssysteem   |Gedeeltelijk    |Nee|
|Dedicated en LowPriority VM's  |Ja    |Ja|
|Automatisch schalen   |Ja    |Ja (standaard)|
|Wachttijd voor automatisch schalen  |Nee |Ja|
|SSH    |Ja|   Ja|
|Koppeling op clusterniveau |Ja (bestandsshares, blobs, NFS, aangepast)   |Ja (uw gegevensopslag koppelen of downloaden)|
|Gedistribueerde training|  Ja |Ja|
|Modus voor taakuitvoering|    VM of container|    Container|
|Aangepaste containerinstallatiekopie|    Ja |Ja|
|Toolkit    |Ja    |Ja (Python-script uitvoeren)|
|JobPreparation|    Ja |Nog niet|
|Taakniveaukoppeling |Ja (bestandsshares, blobs, NFS, aangepast)   |Ja (bestandsshares, blobs)|
|Taakbewaking     |via GetJob|    via uitvoeringsgeschiedenis (uitgebreidere informatie, aangepaste runtime om meer metrische gegevens te pushen)|
|Taaklogboeken en -bestanden/modellen ophalen |   via ListFiles en opslag-API's  |via Artifact-service|
 |Ondersteuning voor Tensorboard   |Nee|    Ja|
|Quota op niveau van VM-serie |Ja    |Ja (met uw vorige capaciteit getransporteerd)|
 
Naast de functies in de vorige tabel, zijn er functies in Azure Machine Learning Service die voorheen niet in BatchAI werden ondersteund.

|Functie|BatchAI-service|Azure Machine Learning-service|
|-------|:-------:|:-------:|
|Omgevingsvoorbereiding    |Nee |Ja (Conda-voorbereiding en uploaden naar ACR)|
|Hyperparameter-afstemming  |Nee|    Ja|
|Modelbeheer   |Nee |Ja|
|Operationaliseren/implementeren| Nee  |Via AKS en ACI|
|Gegevensvoorbereiding   |Nee |Ja|
|Rekendoelen    |Azure-VM's  |Lokaal, BatchAI (als AmlCompute), DataBricks, HDInsight|
|Geautomatiseerde Machine Learning |Nee|    Ja|
|Pijplijnen  |Nee |Ja|
|Batch scoren  |Ja    |Ja|
|Portal-/CLI-ondersteuning|    Ja |Ja|


### <a name="programming-interfaces"></a>Programmeerinterfaces

In deze tabel worden de verschillende programmeerinterfaces gepresenteerd die voor elke service beschikbaar zijn.
    
|Functie|BatchAI-service|Azure Machine Learning-service|
|-------|:-------:|:-------:|
|SDK    |Java, C#, Python, Node.js   |Python (op configuratie en schatting gebaseerde uitvoering voor algemene frameworks)|
|CLI    |Ja    |Nog niet|
|Azure Portal   |Ja    |Ja (uitgezonderd indienen van taken)|
|REST-API   |Ja    |Ja, maar gedistribueerd over microservices|




## <a name="why-migrate"></a>Voordelen van migratie

Door een upgrade uit te voeren van de preview-versie van de Batch AI-service naar de algemeen beschikbare Azure Machine Learning Service krijgt u de beschikking over gebruikersvriendelijke concepten, zoals schattingen (Estimators) en gegevensopslag. Bovendien profiteert u van Azure-SLA's en klantondersteuning op algemeen beschikbaar niveau.

Azure Machine Learning Service biedt tevens nieuwe functionaliteit, zoals geautomatiseerde Machine Learning, Hyperparameter-afstemming en Machine Learning-pijplijnen, die handig zijn voor de meeste grootschalige AI-workloads. Dankzij de mogelijkheid een getraind model te operationaliseren zonder omschakeling naar een afzonderlijke service, kan de Data Science-lus van de gegevensvoorbereiding (met behulp van de gegevensvoorbereiding-SDK) tot aan operationalisering en modelbewaking worden voltooid.

<a name="migrate"></a>

## <a name="how-do-i-migrate"></a>Hoe migreer ik?

Voordat u de stappen uitvoert in de migratiehandleiding (waarmee u de opdrachten tussen de twee services kunt toewijzen), wordt u aangeraden enige tijd te besteden aan het vertrouwd raken met Azure Machine Learning Service aan de hand van de [documentatie](../machine-learning/service/overview-what-is-azure-ml.md), inclusief de [zelfstudie in Python](../machine-learning/service/tutorial-train-models-with-aml.md).

Als u onderbrekingen in uw toepassingen wilt voorkomen en wilt profiteren van de nieuwste functies, moet u de volgende stappen uitvoeren vóór 31 maart 2019:

1. Een werkruimte maken in de Azure Machine Learning-service en aan de slag gaan:
    + [Snelstart op basis van Python](../machine-learning/service/quickstart-create-workspace-with-python.md)
    + [Snelstart op basis van Azure Portal](../machine-learning/service/quickstart-get-started.md)

1. Een [Azure Machine Learning Compute](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute)-instantie installeren voor modeltraining.

1. Werk uw scripts bij voor het gebruik van Azure Machine Learning Compute.


### <a name="sdk-migration"></a>SDK-migratie

De huidige SDK-ondersteuning in Azure Machine Learning Service geschiedt via een aantal Python-SDK's. De primaire SDK wordt ongeveer elke twee weken bijgewerkt en kan vanaf PyPi worden geïnstalleerd met de volgende opdracht:

```python
pip install --upgrade azureml-sdk[notebooks]
```

Stel uw omgeving in en installeer de SDK met deze [quickstart-instructies](../machine-learning/service/quickstart-create-workspace-with-python.md#install-the-sdk)

Zodra u een Jupyter Notebook hebt geopend, waarbij de kernel naar de desbetreffende Conda-omgeving verwijst, ziet de toewijzing van opdrachten in de twee services er als volgt uit:


#### <a name="create-a-workspace"></a>Een werkruimte maken
Het concept voor het initialiseren van een werkruimte met behulp van een gegevensbestand van het type configuration.json in Batch AI, vindt op ongeveer dezelfde manier plaats als het gebruik van een configuratiebestand in Azure Machine Learning.

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

Meer informatie over de Azure Machine Learning-werkruimteklasse vindt u in de [SDK-naslagdocumentatie](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) (Engelstalig).


#### <a name="create-a-compute-cluster"></a>Een rekencluster maken
Azure Machine Learning ondersteunt meerdere rekendoelen, waarvan sommige door de service worden beheerd; andere kunnen aan uw werkruimte worden gekoppeld (bijvoorbeeld een HDInsight-cluster of een externe VM. Lees meer over de verschillende [rekendoelen](../machine-learning/service/how-to-set-up-training-targets.md). Het concept voor het maken van een Batch AI-rekencluster komt overeen met het maken van een AmlCompute-cluster in Azure Machine Learning. Bij het maken van een Amlcompute-cluster wordt gebruikgemaakt van een rekenconfiguratie die vergelijkbaar is met de manier waarop u parameters doorgeeft in Batch AI. Houd er rekening mee dat automatisch schalen op de AmlCompute-cluster standaard is ingeschakeld, terwijl het in Batch AI standaard is uitgeschakeld.

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


#### <a name="monitoring-status-of-your-cluster"></a>Bewakingsstatus van uw cluster
Dit is in Azure Machine Learning eenvoudiger, zoals u hieronder kunt zien.

In **Batch AI** hebt u het als volgt gedaan:

```python
cluster = client.clusters.get(cfg.resource_group, cfg.workspace, cluster_name)
utils.cluster.print_cluster_status(cluster)
```

In **Azure Machine Learning Service** kunt u het volgende doen:

```python
gpu_cluster.get_status().serialize()
```

#### <a name="getting-reference-to-a-storage-account"></a>Verwijzing voor een opslagaccount
Het concept voor een gegevensopslag, zoals een blob, wordt in Azure Machine Learning vereenvoudigd met behulp van het DataStore-object. Standaard wordt door uw Azure Machine Learning-werkruimte een opslagaccount gemaakt, maar u kunt uw eigen opslag ook koppelen tijdens het maken van de werkruimte. 

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

Meer informatie over het registreren van extra opslagaccounts of het ophalen van een verwijzing naar een andere geregistreerde gegevensopslag vindt u in de [documentatie van Azure Machine Learning Service](../machine-learning/service/how-to-access-data.md#access).


#### <a name="downloading-and-uploading-data"></a>Gegevens downloaden en uploaden 
Met beide services kunt u de gegevens eenvoudig naar het opslagaccount uploaden door middel van de hierboven genoemde verwijzing naar de gegevensopslag. Voor Batch AI maken we ook gebruik van het trainingsscript als onderdeel van de bestandsshare, hoewel u, in het geval van Azure Machine Learning, zult zien hoe u het kunt opgeven als deel van uw taakconfiguratie.

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

#### <a name="create-an-experiment"></a>Een experiment maken
Zoals hierboven vermeld, lijkt het experimentconcept van Azure Machine Learning op dat van Batch AI. Elk experiment kan uit meerdere uitvoeringen bestaan, soortgelijk aan de manier waarop taken in Batch AI worden uitgevoerd. In Azure Machine Learning is ook een hiërarchie met boven- en onderliggende uitvoeringen mogelijk.

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


#### <a name="submit-a-job"></a>Een taak indienen
Als u een experiment maakt, kunt u een uitvoering op meerdere manieren indienen. In dit voorbeeld gaan we een Deep Learning-model maken met TensorFlow en gebruiken hiervoor een Azure Machine Learning-Estimator. Een [Estimator](../machine-learning/service/how-to-train-ml-models.md) is eenvoudigweg een wrapperfunctie voor de onderliggende uitvoeringsconfiguratie. Hierdoor wordt het makkelijker om uitvoeringen in te dienen. De functie wordt momenteel alleen ondersteund voor Pytorch en TensorFlow. Dankzij het concept voor gegevensopslag ziet u bovendien hoe eenvoudig het nu is om de koppelingspaden op te geven 

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

Het indienen van de taak zelf in Batch AI vindt plaats met de maakfunctie.

```python
job_name = datetime.utcnow().strftime('tf_%m_%d_%Y_%H%M%S')
job = client.jobs.create(cfg.resource_group, cfg.workspace, experiment_name, job_name, parameters).result()
print('Created Job {0} in Experiment {1}'.format(job.name, experiment.name))
```

De volledige informatie voor dit codefragment voor de training (inclusief het bestand mnist_replica.py dat hierboven naar de bestandsshare is geüpload) vindt u hier: [BatchAI sample notebook github repo](https://github.com/Azure/BatchAI/tree/2238607d5a028a0c5e037168aefca7d7bb165d5c/recipes/TensorFlow/TensorFlow-GPU-Distributed) (GitHub-opslagplaats met de Batch AI-voorbeeldnotebook).

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

De volledige informatie voor dit codefragment voor de training (inclusief het bestand tf_mnist_replica.py) vindt u hier: [Azure ML sample notebook github repo](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/distributed-tensorflow-with-parameter-server) (GitHub-opslagplaats met de Azure Machine Learning-voorbeeldnotebook). De gegevensopslag zelf kan worden gekoppeld aan de afzonderlijke knooppunten, maar de trainingsgegevens kunnen ook in het knooppunt zelf worden gedownload. Meer informatie over het verwijzen naar de gegevensopslag in uw Estimator vindt u in de [documentatie van Azure Machine Learning Service](../machine-learning/service/how-to-access-data.md#access). 

U kunt een uitvoering in Azure Machine Learning Service indienen via de verzendfunctie.

```python
run = experiment.submit(estimator)
print(run)
```

U kunt de parameters voor de uitvoering ook opgeven met een uitvoeringsconfiguratie. Dit is met name handig voor het definiëren van een aangepaste trainingsomgeving. Meer informatie vindt u hier: [sample AmlCompute notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-amlcompute/train-on-amlcompute.ipynb) (AmlCompute-voorbeeldnotebook). 

#### <a name="monitor-your-run"></a>De uitvoering bewaken
Als u een uitvoering hebt ingediend, kunt u wachten tot deze is voltooid of bewaken in Azure Machine Learning met behulp van handige Jupyter-widgets, die u rechtstreeks vanuit de code kunt aanroepen. U kunt ook context uit een voorgaande uitvoering halen door de verschillende experimenten in een werkruimte en de individuele uitvoeringen in elk experiment te doorlopen.

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



#### <a name="editing-a-cluster"></a>Cluster bewerken
U kunt een cluster eenvoudige verwijderen. In Azure Machine Learning kunt u ook een cluster bijwerken vanuit het notebook, indien u het wilt opschalen naar een groter aantal knooppunten of de wachttijd bij inactiviteit wilt verhogen voordat u het cluster omlaag schaalt. U mag de VM-grootte van het cluster zelf niet wijzigen, omdat hiervoor een nieuwe implementatie aan de back-end noodzakelijk is.

In **Batch AI** hebt u het als volgt gedaan:
```python
_ = client.clusters.delete(cfg.resource_group, cfg.workspace, cluster_name)
```

In **Azure Machine Learning Service** kunt u het volgende doen:

```python
gpu_cluster.delete()

gpu_cluster.update(min_nodes=2, max_nodes=4, idle_seconds_before_scaledown=600)
```

## <a name="support"></a>Ondersteuning

Batch AI wordt op 31 maart buiten bedrijf gesteld. Momenteel kunnen geen nieuwe abonnementen voor de service meer worden geregistreerd, tenzij ze op een whitelist worden geplaatst door via de ondersteuning een uitzondering in te dienen.  Als u vragen of feedback hebt over de migratie naar Azure Machine Learning Service, kunt u contact met ons opnemen via [Azure Batch AI Training Preview](mailto:AzureBatchAITrainingPreview@service.microsoft.com).

Azure Machine Learning Service is een algemeen beschikbare service. Dit betekent dat de service wordt geleverd met een bindende SLA en een keuze uit diverse ondersteuningsplannen.

De prijzen voor het gebruik van de Azure-infrastructuur via de Batch AI-service of via Azure Machine Learning Service zijn gelijk, aangezien in beide gevallen alleen de kosten voor de onderliggende rekentijd in rekening worden gebracht. Zie de [prijscalculator](https://azure.microsoft.com/pricing/details/machine-learning-service/) voor meer informatie.

Bekijk de regionale beschikbaarheid voor de twee services in de [Azure-portal](https://azure.microsoft.com/global-infrastructure/services/?products=batch-ai,machine-learning-service&regions=all).


## <a name="next-steps"></a>Volgende stappen

+ Lees [Overzicht van de Azure Machine Learning-service](../machine-learning/service/overview-what-is-azure-ml.md).

+ [Configureer een rekendoel voor modeltraining](../machine-learning/service/how-to-set-up-training-targets.md) met de Azure Machine Learning-service.

+ Controleer de [Azure-roadmap](https://azure.microsoft.com/updates/) voor meer informatie over andere Azure-service-updates.
