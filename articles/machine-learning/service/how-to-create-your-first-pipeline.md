---
title: Maken, uitvoeren en bijhouden van ML-pijplijnen
titleSuffix: Azure Machine Learning service
description: Maken en uitvoeren van een machine learning-pijplijn met de Azure Machine Learning-SDK voor Python. U kunt pijplijnen maken en beheren van de werkstromen die fasen geniet samen machine learning (ML). Deze fasen bevatten gegevens voor te bereiden, modeltraining modelimplementatie en inferentietaken.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.author: sanpil
author: sanpil
ms.date: 01/08/2019
ms.custom: seodec18
ms.openlocfilehash: 5d0fe5d010fc53db3ca280620d224d405d0ccf42
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54478192"
---
# <a name="create-and-run-a-machine-learning-pipeline-by-using-azure-machine-learning-sdk"></a>Maken en uitvoeren van een machine learning-pijplijn met behulp van Azure Machine Learning-SDK

In dit artikel leert u hoe u maken, publiceren, uitvoeren en volgen een [machine learning-pijplijn](concept-ml-pipelines.md) met behulp van de [Azure Machine Learning-SDK](https://aka.ms/aml-sdk).  Deze pijplijnen kunnen maken en beheren van de werkstromen die verschillende fasen van de machine learning samen te voegen. Elke fase van een pijplijn, zoals gegevens voor te bereiden en trainen van het model, kan een of meer stappen bevatten.

De pijplijnen die u maakt zijn zichtbaar voor de leden van de service Azure Machine Learning [werkruimte](how-to-manage-workspace.md). 

Externe compute-doelen pijplijnen gebruiken voor berekeningen en opslag van de tussenliggende en laatste gegevens die zijn gekoppeld aan die pijplijn. Pijplijnen kunnen lezen en schrijven gegevens van en naar ondersteund [Azure Storage](https://docs.microsoft.com/azure/storage/) locaties.

Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer de [gratis of betaalde versie van Azure Machine Learning-service](http://aka.ms/AMLFree).

## <a name="prerequisites"></a>Vereisten

* [Uw ontwikkelomgeving configureren](how-to-configure-environment.md) voor het installeren van de SDK van Azure Machine Learning.

* Maak een [Azure Machine Learning-werkruimte](how-to-configure-environment.md#workspace) voor het opslaan van alle resources van uw pijplijn. 

 ```python
 ws = Workspace.create(
     name = '<workspace-name>',
     subscription_id = '<subscription-id>',
     resource_group = '<resource-group>',
     location = '<workspace_region>',
     exist_ok = True)
 ```

## <a name="set-up-machine-learning-resources"></a>Machine learning-resources instellen

De resources die vereist voor het uitvoeren van een pijplijn maken:

* Instellen van een gegevensarchief gebruikt voor toegang tot de gegevens die nodig zijn in de stappen van de pijplijn.

* Configureer een `DataReference` object verwijst naar gegevens die zich bevinden of toegankelijk is in een gegevensopslag bevinden.

* Instellen van de [compute-doelen](concept-azure-machine-learning-architecture.md#compute-target) op waarmee de stappen van de pijplijn wordt uitgevoerd.

### <a name="set-up-a-datastore"></a>Instellen van een gegevensarchief
De gegevens voor de pijplijn voor toegang tot worden opgeslagen in een gegevensarchief. Elke werkruimte heeft een standaard-gegevensopslag. U kunt aanvullende gegevensopslag registreren. 

Wanneer u uw werkruimte maakt [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) en [Azure Blob-opslag](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) zijn gekoppeld aan de werkruimte standaard. Azure Files is de standaard gegevensopslag voor een werkruimte, maar u kunt Blob storage ook gebruiken als gegevensopslag. Zie voor meer informatie, [beslissen wanneer u het gebruik van Azure Files, Azure-Blobs of Azure-schijven](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks). 

```python
# Default datastore (Azure file storage)
def_data_store = ws.get_default_datastore() 

# The above call is equivalent to this 
def_data_store = Datastore(ws, "workspacefilestore")

# Get blob storage associated with the workspace
def_blob_store = Datastore(ws, "workspaceblobstore")
```

Upload bestanden of mappen met het gegevensarchief zodat deze toegankelijk is vanaf uw pijplijnen. In dit voorbeeld wordt de Blob-opslag-versie van het gegevensarchief gebruikt:

```python
def_blob_store.upload_files(
    ["./data/20news.pkl"],
    target_path="20newsgroups", 
    overwrite=True)
```

Een pijplijn bestaat uit een of meer stappen. Een stap is een eenheid worden uitgevoerd op een compute-doel. Stappen mogelijk gegevensbronnen gebruiken en 'tussenliggende' gegevens produceren. Een stap kunt maken, zoals een model, een map met het model en de afhankelijke bestanden of tijdelijke gegevens. Deze gegevens zijn vervolgens beschikbaar voor andere stappen die verderop in de pijplijn.

### <a name="configure-data-reference"></a>Naslaginformatie voor data configureren

U alleen een gegevensbron die kan worden verwezen in een pijplijn gemaakt als invoer voor een stap. Een gegevensbron in een pijplijn wordt vertegenwoordigd door een [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference) object. De `DataReference` object verwijst naar gegevens die zich bevinden of uit een gegevensarchief toegankelijk is.

```python
blob_input_data = DataReference(
    datastore=def_blob_store,
    data_reference_name="test_data",
    path_on_datastore="20newsgroups/20news.pkl")
```

Tussenliggende gegevens (of de uitvoer van een stap) wordt vertegenwoordigd door een [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) object. `output_data1` wordt gegenereerd als de uitvoer van een stap en gebruikt als de invoer van een of meer toekomstige stappen. `PipelineData` is van een gegevensafhankelijkheid tussen fasen en maakt u een order impliciete worden uitgevoerd in de pijplijn.

```python
output_data1 = PipelineData(
    "output_data1",
    datastore=def_blob_store,
    output_name="output_data1")
```

## <a name="set-up-compute-target"></a>Compute-doel instellen

In Azure Machine Learning, de term __compute__ (of __compute-doel__) verwijst naar de machines of clusters die de rekenkundige stappen in uw machine learning-pijplijn uitvoeren.   Zie [compute-doelen voor modeltraining](how-to-set-up-training-targets.md) voor een volledige lijst van de compute-doelen en hoe u maakt en koppelt u ze aan uw werkruimte.  Het proces voor het maken en of het koppelen van een compute-doel is hetzelfde, ongeacht of u een model te trainen of een pijplijn stap wordt uitgevoerd. Nadat u maken en koppelen van de compute-doel, gebruikt u de `ComputeTarget` van het object in uw [pijplijn stap](#steps).

Hieronder volgen enkele voorbeelden van het maken en koppelen van compute-doelen voor:

* Azure Machine Learning-Computing
* Azure Databricks 
* Azure Data Lake Analytics

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning-Computing

U kunt een Azure Machine Learning-Computing voor het uitvoeren van uw werk kunt maken.

    ```python
    compute_name = "aml-compute"
     if compute_name in ws.compute_targets:
        compute_target = ws.compute_targets[compute_name]
        if compute_target and type(compute_target) is AmlCompute:
            print('Found compute target: ' + compute_name)
    else:
        print('Creating a new compute target...')
        provisioning_config = AmlCompute.provisioning_configuration(vm_size = vm_size, # NC6 is GPU-enabled
                                                                    min_nodes = 1, 
                                                                    max_nodes = 4)
         # create the compute target
        compute_target = ComputeTarget.create(ws, compute_name, provisioning_config)
        
        # Can poll for a minimum number of nodes and for a specific timeout. 
        # If no min node count is provided it will use the scale settings for the cluster
        compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
        
         # For a more detailed view of current cluster status, use the 'status' property    
        print(compute_target.status.serialize())
    ```

### <a id="databricks"></a>Azure Databricks

Azure Databricks is een omgeving op basis van Apache Spark in de Azure-cloud. Het kan worden gebruikt als een compute-doel met een Azure Machine Learning-pijplijn.

Een Azure Databricks-werkruimte maken voordat u deze gebruikt. Zie voor het maken van deze resource, de [een Spark-taak uitvoeren op Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) document.

Als u wilt koppelen Azure Databricks als een compute-doel, geef de volgende informatie:

* __Databricks compute naam__: De naam die u wilt toewijzen aan deze compute-resource.
* __De naam van de Databricks-werkruimte__: De naam van de Azure Databricks-werkruimte.
* __Databricks-toegangstoken__: Het toegangstoken dat wordt gebruikt om te verifiëren met Azure Databricks. Zie voor het genereren van een toegangstoken de [verificatie](https://docs.azuredatabricks.net/api/latest/authentication.html) document.

De volgende code ziet u hoe u Azure Databricks koppelen als een compute-doel met de SDK van Azure Machine Learning:

```python
import os
from azureml.core.compute import ComputeTarget, DatabricksCompute
from azureml.exceptions import ComputeTargetException

databricks_compute_name = os.environ.get("AML_DATABRICKS_COMPUTE_NAME", "<databricks_compute_name>")
databricks_workspace_name = os.environ.get("AML_DATABRICKS_WORKSPACE", "<databricks_workspace_name>")
databricks_resource_group = os.environ.get("AML_DATABRICKS_RESOURCE_GROUP", "<databricks_resource_group>")
databricks_access_token = os.environ.get("AML_DATABRICKS_ACCESS_TOKEN", "<databricks_access_token>")

try:
    databricks_compute = ComputeTarget(workspace=ws, name=databricks_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('databricks_compute_name {}'.format(databricks_compute_name))
    print('databricks_workspace_name {}'.format(databricks_workspace_name))
    print('databricks_access_token {}'.format(databricks_access_token))

    # Create attach config
    attach_config = DatabricksCompute.attach_configuration(resource_group = databricks_resource_group,
                                                           workspace_name = databricks_workspace_name,
                                                           access_token = databricks_access_token)
    databricks_compute = ComputeTarget.attach(
             ws,
             databricks_compute_name,
             attach_config
         )
    
    databricks_compute.wait_for_completion(True)
```
### <a id="adla"></a>Azure Data Lake Analytics

Azure Data Lake Analytics is een platform voor big data-analyses in de Azure-cloud. Het kan worden gebruikt als een compute-doel met een Azure Machine Learning-pijplijn.

Een Azure Data Lake Analytics-account maken voordat u deze gebruikt. Zie voor het maken van deze resource, de [aan de slag met Azure Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal) document.

Als u wilt koppelen Data Lake Analytics als een compute-doel, moet u de Azure Machine Learning-SDK gebruiken en geef de volgende informatie:

* __De naam van COMPUTE__: De naam die u wilt toewijzen aan deze compute-resource.
* __Resourcegroep__: De resourcegroep met de Data Lake Analytics-account.
* __Accountnaam__: De naam van de Data Lake Analytics-account.

De volgende code ziet u hoe u Data Lake Analytics als een compute-doel toevoegen:

```python
import os
from azureml.core.compute import ComputeTarget, AdlaCompute
from azureml.exceptions import ComputeTargetException


adla_compute_name = os.environ.get("AML_ADLA_COMPUTE_NAME", "<adla_compute_name>")
adla_resource_group = os.environ.get("AML_ADLA_RESOURCE_GROUP", "<adla_resource_group>")
adla_account_name = os.environ.get("AML_ADLA_ACCOUNT_NAME", "<adla_account_name>")

try:
    adla_compute = ComputeTarget(workspace=ws, name=adla_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('adla_compute_name {}'.format(adla_compute_name))
    print('adla_resource_id {}'.format(adla_resource_group))
    print('adla_account_name {}'.format(adla_account_name))
    # create attach config
    attach_config = AdlaCompute.attach_configuration(resource_group = adla_resource_group,
                                                     account_name = adla_account_name)
    # Attach ADLA
    adla_compute = ComputeTarget.attach(
             ws,
             adla_compute_name,
             attach_config
         )
    
    adla_compute.wait_for_completion(True)
```

> [!TIP]
> Azure Machine Learning-pijplijnen kunnen uitsluitend worden gebruikt met gegevens die zijn opgeslagen in het standaardarchief van gegevens van het Data Lake Analytics-account. Als de gegevens die u nodig hebt om te werken met in een niet-standaard-archief is, kunt u een [ `DataTransferStep` ](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) om de gegevens voordat een training te kopiëren.

## <a id="steps"></a>De stappen van uw pijplijn bouwen

Als u maken en koppelen van een compute-doel aan uw werkruimte, bent u klaar voor het definiëren van een stap van de pijplijn. Er zijn veel ingebouwde stappen beschikbaar via de SDK van Azure Machine Learning. De meeste algemene van deze stappen is een [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py), die een Python-script wordt uitgevoerd in een opgegeven compute-doel.

```python
trainStep = PythonScriptStep(
    script_name="train.py",
    arguments=["--input", blob_input_data, "--output", processed_data1],
    inputs=[blob_input_data],
    outputs=[processed_data1],
    compute_target=compute_target,
    source_directory=project_folder
)
```

Nadat u de stappen hebt gedefinieerd, kunt u de pijplijn bouwen met behulp van sommige of alle van deze stappen.

>[!NOTE]
>Er is geen bestand of de gegevens wordt geüpload naar de Azure Machine Learning-service wanneer u de stappen definieert of de pijplijn bouwen.

```python
# list of steps to run
compareModels = [trainStep, extractStep, compareStep]

# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=[compareModels])
```

Het volgende voorbeeld wordt de Azure Databricks-compute-doel eerder hebt gemaakt: 

```python
dbStep = DatabricksStep(
    name="databricksmodule",
    inputs=[step_1_input],
    outputs=[step_1_output],
    num_workers=1,
    notebook_path=notebook_path,
    notebook_params={'myparam': 'testparam'},
    run_name='demo run name',
    databricks_compute=databricks_compute,
    allow_reuse=False
)
# List of steps to run
steps = [dbStep]

# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=steps)
```

## <a name="submit-the-pipeline"></a>Verzenden van de pijplijn

Wanneer u de pijplijn verzendt, wordt Azure Machine Learning-service controleert de afhankelijkheden voor elke stap en uploadt een momentopname van de bronmap die u hebt opgegeven. Als er geen bronmap is opgegeven, wordt de huidige lokale map geüpload.


```python
# Submit the pipeline to be run
pipeline_run1 = Experiment(ws, 'Compare_Models_Exp').submit(pipeline1)
pipeline_run1.wait_for_completion()
```

Wanneer u eerst een pijplijn uitvoert, Azure Machine Learning:

* De momentopname van het project downloadt naar de compute-doel van de Blob-opslag die is gekoppeld aan de werkruimte.
* Een Docker-installatiekopie overeenkomt met elke stap in de pijplijn bouwt.
* De docker-installatiekopie voor elke stap downloadt naar de compute-doel van het containerregister.
* Hiermee koppelt u het gegevensarchief als een `DataReference` object is opgegeven in een stap. Als het koppelpunt wordt niet ondersteund, worden de gegevens worden in plaats daarvan gekopieerd naar de compute-doel.
* Voert de stap in de compute-doel opgegeven in de stapdefinitie van de. 
* Artefacten, zoals Logboeken, stdout en stderr, metrische gegevens en uitvoerwaarde zijn opgegeven in de stap maakt. Deze artefacten worden vervolgens geüpload en opgeslagen in de standaard gegevensopslag van de gebruiker.

![Diagram van het uitvoeren van een experiment als een pijplijn](./media/how-to-create-your-first-pipeline/run_an_experiment_as_a_pipeline.png)

## <a name="publish-a-pipeline"></a>Publiceren van een pijplijn

U kunt een pijplijn uit te voeren met verschillende soorten invoer later publiceren. Voor de REST-eindpunt van een al gepubliceerde pijplijn parameters accepteert, moet u parameter van de pijplijn voordat u publiceert. 

1. Voor het maken van een pijplijnparameter gebruikt een [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py) object met een standaardwaarde.

 ```python
 pipeline_param = PipelineParameter(
     name="pipeline_arg", 
     default_value=10)
 ```

2. Voeg deze toe `PipelineParameter` object als parameter voor een van de stappen in de pijplijn als volgt:

 ```python
 compareStep = PythonScriptStep(
     script_name="compare.py",
     arguments=["--comp_data1", comp_data1, "--comp_data2", comp_data2, "--output_data", out_data3, "--param1", pipeline_param],
     inputs=[ comp_data1, comp_data2],
     outputs=[out_data3],    
     target=compute_target, 
     source_directory=project_folder)
 ```

3. Publiceer deze pijplijn waarmee een parameter wanneer aangeroepen worden geaccepteerd.

   ```python
   published_pipeline1 = pipeline1.publish(
       name="My_Published_Pipeline", 
       description="My Published Pipeline Description")
   ```

## <a name="run-a-published-pipeline"></a>Een gepubliceerde pijplijn uitvoeren

Alle gepubliceerde pijplijnen hebben een REST-eindpunt. Dit eindpunt roept de uitvoering van de pijplijn in externe systemen, zoals niet-Python-clients. Dit eindpunt kunt 'herhaalbaarheid beheerd' in batch scoren en opnieuw trainen van scenario's.

Voor het aanroepen van de uitvoering van de voorgaande pijplijn, moet u een token van de koptekst in de Azure Active Directory-verificatie, zoals beschreven in [AzureCliAuthentication klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.azurecliauthentication?view=azure-ml-py).

```python
response = requests.post(published_pipeline1.endpoint, 
    headers=aad_token, 
    json={"ExperimentName": "My_Pipeline",
        "ParameterAssignments": {"pipeline_arg": 20}})
```
## <a name="view-results"></a>Resultaten weergeven

Zie de lijst van alle uw pijplijnen en de details van de uitvoering:
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).  

1. [Uw werkruimte weergeven](how-to-manage-workspace.md#view-a-workspace) te vinden van de lijst met pijplijnen.
 ![lijst met machine learning-pijplijnen](./media/how-to-create-your-first-pipeline/list_of_pipelines.png)
 
1. Selecteer een specifieke pijplijn om te zien van de resultaten van de uitvoering.

## <a name="next-steps"></a>Volgende stappen
- Gebruik [deze Jupyter-notebooks in GitHub](https://aka.ms/aml-pipeline-readme) machine learning-pijplijnen verder verkennen.
- Raadpleeg de help van de SDK-verwijzing voor de [azureml-pijplijnen-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) pakket en de [azureml-pijplijnen-stappen](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) pakket.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
