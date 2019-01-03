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
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 6c6472b824eefdd1954f3645c69090d1fb5455de
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2018
ms.locfileid: "53754455"
---
# <a name="create-and-run-a-machine-learning-pipeline-by-using-azure-machine-learning-sdk"></a>Maken en uitvoeren van een machine learning-pijplijn met behulp van Azure Machine Learning-SDK

In dit artikel leert u hoe u maken, publiceren, uitvoeren en volgen een [machine learning-pijplijn](concept-ml-pipelines.md) met behulp van de [Azure Machine Learning-SDK](https://aka.ms/aml-sdk).  Deze pijplijnen kunnen maken en beheren van de werkstromen die verschillende fasen van de machine learning samen te voegen. Elke fase van een pijplijn, zoals gegevens voor te bereiden en trainen van het model, kan een of meer stappen bevatten.

De pijplijnen die u maakt zijn zichtbaar voor de leden van de service Azure Machine Learning [werkruimte](how-to-manage-workspace.md). 

Externe compute-doelen pijplijnen gebruiken voor berekeningen en opslag van de tussenliggende en laatste gegevens die zijn gekoppeld aan die pijplijn. Pijplijnen kunnen lezen en schrijven gegevens van en naar ondersteund [Azure Storage](https://docs.microsoft.com/azure/storage/) locaties.

>[!Note]
>Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer de [gratis of betaalde versie van Azure Machine Learning-service](http://aka.ms/AMLFree).

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

### <a name="set-up-compute"></a>Compute instellen

In Azure Machine Learning, de term *compute* (of *compute-doel*) verwijst naar de machines of clusters die de rekenkundige stappen in uw machine learning-pijplijn uitvoeren. Bijvoorbeeld, kunt u een Azure Machine Learning-Computing voor het uitvoeren van uw werk.

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

## <a name="construct-your-pipeline-steps"></a>De stappen van uw pijplijn bouwen

U bent nu klaar voor het definiëren van een stap van de pijplijn. Er zijn veel ingebouwde stappen beschikbaar via de SDK van Azure Machine Learning. De meeste algemene van deze stappen is een `PythonScriptStep`, die een Python-script wordt uitgevoerd in een opgegeven compute-doel.

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

## <a name="submit-the-pipeline"></a>Verzenden van de pijplijn

Wanneer u de pijplijn verzendt, wordt Azure Machine Learning-service controleert de afhankelijkheden voor elke stap en uploadt een momentopname van de bronmap die u hebt opgegeven. Als er geen bronmap is opgegeven, wordt de huidige lokale map geüpload.

```python
# Submit the pipeline to be run
pipeline_run1 = Experiment(ws, 'Compare_Models_Exp').submit(pipeline1)
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
