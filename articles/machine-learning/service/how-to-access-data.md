---
title: Toegang krijgen tot gegevens in gegevens opslag/blobs voor training
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van gegevens opslag voor toegang tot blobgegevens tijdens de training met Azure Machine Learning service
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 05/24/2019
ms.custom: seodec18
ms.openlocfilehash: 97a4bc20394553b97211763cedaa76c3711306f2
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68319313"
---
# <a name="access-data-from-your-datastores"></a>Toegang tot gegevens uit uw gegevens opslag

 In Azure Machine Learning service worden data stores reken locatie-onafhankelijke mechanismen voor toegang tot opslag zonder dat de bron code hoeft te worden gewijzigd. Als u trainings code schrijft om een pad op te halen als een para meter, of als u een gegevens opslag rechtstreeks aan een Estimator, Azure Machine Learning werk stromen controleren of uw gegevens opslag locaties toegankelijk zijn en beschikbaar zijn gesteld voor uw Compute-context.

In deze procedure worden voor beelden van de volgende taken weer gegeven:
* [Kies een gegevens opslag](#access)
* [Gegevens ophalen](#get)
* [Gegevens uploaden en downloaden naar gegevens opslag](#up-and-down)
* [Toegang tot Data Store tijdens training](#train)

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van gegevensopslag, moet u eerst een [werkruimte](concept-workspace.md).

Begin door een van beide [het maken van een nieuwe werkruimte](setup-create-workspace.md#sdk) of bij het ophalen van een bestaande:

```Python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()
```

<a name="access"></a>

## <a name="choose-a-datastore"></a>Kies een gegevens opslag

U kunt gebruikmaken van de standaard gegevens opslag of uw eigen Data Store.

### <a name="use-the-default-datastore-in-your-workspace"></a>De standaard gegevens opslag in uw werk ruimte gebruiken

 Elke werk ruimte heeft een geregistreerde, standaard-gegevens opslag die u direct kunt gebruiken.

Ophalen van de werkruimte standaard gegevensopslag:

```Python
ds = ws.get_default_datastore()
```

### <a name="register-your-own-datastore-with-the-workspace"></a>Uw eigen gegevens opslag registreren bij de werk ruimte

Hebt u een bestaande Azure-opslag, kunt u deze registreren als een gegevensarchief in uw werkruimte. 

<a name="store"></a>

####  <a name="storage-guidance"></a>Richtlijnen voor Azure Storage

We raden Blob Storage en BLOB-gegevens opslag. Zowel Standard-als Premium-opslag zijn beschikbaar voor blobs. Hoewel we nog duurder worden, worden Premium-opslag voorgesteld door snellere doorvoer snelheden die de snelheid van uw trainings uitvoeringen kunnen verbeteren, met name als u traint voor een grote gegevensset. Zie de [Azure-prijs calculator](https://azure.microsoft.com/pricing/calculator/?service=machine-learning-service) voor informatie over de kosten van het opslag account.

>[!NOTE]
> Azure Machine Learning-service ondersteunt andere typen gegevens opslag, wat nuttig kan zijn voor specifieke scenario's. Als u bijvoorbeeld wilt trainen met gegevens die zijn opgeslagen in een Data Base, kunt u de AzureSQLDatabaseDatastore of AzurePostgreSqlDatastore gebruiken. Zie [deze tabel](#matrix) voor de beschik bare typen gegevens opslag.

#### <a name="register-your-datastore"></a>Uw gegevens opslag registreren
Alle registratie methoden bevinden zich op [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) de klasse en hebben het formulier register_azure_ *.

In de volgende voor beelden ziet u hoe u een Azure Blob-container of een Azure-bestands share als een gegevens opslag registreert.

+ Gebruik voor een **Azure Blob container-gegevens opslag**[`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py)

  ```Python
  ds = Datastore.register_azure_blob_container(workspace=ws, 
                                               datastore_name='your datastore name', 
                                               container_name='your azure blob container name',
                                               account_name='your storage account name', 
                                               account_key='your storage account key',
                                               create_if_not_exists=True)
  ```

+ [Gebruik`register_azure_file_share()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-)voor een **Azure file share-gegevens opslag**. Bijvoorbeeld: 
  ```Python
  ds = Datastore.register_azure_file_share(workspace=ws, 
                                           datastore_name='your datastore name', 
                                           file_share_name='your file share name',
                                           account_name='your storage account name', 
                                           account_key='your storage account key',
                                           create_if_not_exists=True)
  ```

<a name="get"></a>

## <a name="find--define-datastores"></a>Zoeken & gegevens opslag vastleggen

Als u een opgegeven Data Store wilt ontvangen die in de huidige [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-) werk ruimte is geregistreerd, gebruikt u:

```Python
#get named datastore from current workspace
ds = Datastore.get(ws, datastore_name='your datastore name')
```

Gebruik de volgende code om een lijst op te halen van alle gegevens opslag in een bepaalde werk ruimte:

```Python
#list all datastores registered in current workspace
datastores = ws.datastores
for name, ds in datastores.items():
    print(name, ds.datastore_type)
```

Als u een andere standaard gegevens opslag voor de huidige werk ruimte [`set_default_datastore()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#set-default-datastore-name-)wilt definiëren, gebruikt u:

```Python
#define default datastore for current workspace
ws.set_default_datastore('your datastore name')
```

<a name="up-and-down"></a>
## <a name="upload--download-data"></a>Gegevens uploaden & downloaden
De [`upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) methoden [`download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) en die in de volgende voor beelden worden beschreven, zijn specifiek voor en worden identiek voor de klassen [AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py) en [AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py) .

### <a name="upload"></a>Uploaden

 Een map of afzonderlijke bestanden uploaden naar de gegevensopslag met behulp van de Python-SDK.

Het uploaden van een map naar een gegevensarchief `ds`:

```Python
import azureml.data
from azureml.data.azure_storage_datastore import AzureFileDatastore, AzureBlobDatastore

ds.upload(src_dir='your source directory',
          target_path='your target path',
          overwrite=True,
          show_progress=True)
```

`target_path` Hiermee geeft u de locatie in de bestandsshare (of de blob-container) om te uploaden. Wordt standaard `None`, in welk geval de gegevens wordt het geüpload naar de hoofdmap. `overwrite=True` overschrijft bestaande gegevens op `target_path`.

Of een lijst van afzonderlijke bestanden uploaden naar het gegevensarchief via van het gegevensarchief `upload_files()` methode.

### <a name="download"></a>Downloaden
Gegevens uit een gegevensarchief op dezelfde manier downloaden naar uw lokale bestandssysteem.

```Python
ds.download(target_path='your target path',
            prefix='your prefix',
            show_progress=True)
```

`target_path` is de locatie van de lokale map voor het downloaden van de gegevens. Geef het pad naar de map in de bestandsshare (of de blob-container) om te downloaden, geeft dat pad op naar `prefix`. Als `prefix` is `None`, de inhoud van uw bestandsshare (of de blob-container) wordt gedownload.

<a name="train"></a>
## <a name="access-datastores-during-training"></a>Toegang tot gegevens opslag in de training

Zodra u uw gegevens opslag beschikbaar hebt gemaakt op het doel van de trainings compute, hebt u toegang tot de Data Store tijdens de uitvoering van de training (bijvoorbeeld training of validatie gegevens) door het pad naar het bestand door te geven als een para meter in uw trainings script.

De volgende tabel bevat de [`DataReference`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py) methoden die het reken doel laten zien hoe de gegevens opslag tijdens uitvoeringen moet worden gebruikt.

Manier|Methode|Description|
----|-----|--------
Koppelen| [`as_mount()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--)| Gebruiken om de gegevens opslag te koppelen aan het berekenings doel.
Downloaden|[`as_download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-)|Gebruik om de inhoud van uw gegevens archief te downloaden naar de locatie `path_on_compute`die is opgegeven door. <br> Voor de context van de trainings uitvoering wordt deze down load uitgevoerd vóór de uitvoering.
Uploaden|[`as_upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)| Gebruiken om een bestand te uploaden vanaf de locatie die `path_on_compute` is opgegeven door naar uw gegevens opslag. <br> Voor de context van de trainings uitvoering vindt deze upload plaats na de uitvoering.

 ```Python
import azureml.data
from azureml.data.data_reference import DataReference

ds.as_mount()
ds.as_download(path_on_compute='your path on compute')
ds.as_upload(path_on_compute='yourfilename')
```  

Gebruik de [`path()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#path-path-none--data-reference-name-none-) functie Data Store om te verwijzen naar een specifieke map of bestand in uw gegevens opslag en deze beschikbaar te maken op het berekenings doel.

```Python
#download the contents of the `./bar` directory in ds to the compute target
ds.path('./bar').as_download()
```

> [!NOTE]
> Elk `ds` `"$AZUREML_DATAREFERENCE_XXXX"` of `ds.path` -object wordt omgezet in de naam van een omgevings variabele van de notatie waarvan de waarde het pad voor koppelen/downloaden op de doel comput vertegenwoordigt. Het Data Store-pad op de doel Compute kan niet hetzelfde zijn als het pad voor de uitvoering van het trainings script.

<a name="matrix"></a>
### <a name="training-compute-and-datastore-matrix"></a>Trainings Compute en Data Store-matrix

De volgende matrix geeft de beschik bare functies voor gegevens toegang voor de verschillende trainings Compute-doelen en Data Store-scenario's. Meer informatie over de [trainings Compute-doelen voor Azure machine learning](how-to-set-up-training-targets.md#compute-targets-for-training).

|Compute|[AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py)                                       |[AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py)                                      |[AzureDataLakeDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakedatastore?view=azure-ml-py) |[AzureDataLakeGen2Datastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakegen2datastore?view=azure-ml-py) [AzurePostgreSqlDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_postgre_sql_datastore.azurepostgresqldatastore?view=azure-ml-py) [AzureSqlDatabaseDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_sql_database_datastore.azuresqldatabasedatastore?view=azure-ml-py) |
|--------------------------------|----------------------------------------------------------|----------------------------------------------------------|------------------------|----------------------------------------------------------------------------------------|
| Lokaal|[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|N/A         |N/A                                                                         |
| Azure Machine Learning-Computing |[as_mount()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--), [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-), [ML&nbsp;pipelines](concept-ml-pipelines.md)|[as_mount()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--), [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-), [ML&nbsp;pipelines](concept-ml-pipelines.md)|N/A         |N/A                                                                         |
| Virtuele machines               |[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                           | [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |N/A         |N/A                                                                         |
| HDInsight                      |[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            | [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |N/A         |N/A                                                                         |
| Gegevensoverdracht                  |[ML&nbsp;-pijp lijnen](concept-ml-pipelines.md)                                               |N/A                                           |[ML&nbsp;-pijp lijnen](concept-ml-pipelines.md)            |[ML&nbsp;-pijp lijnen](concept-ml-pipelines.md)                                                                            |
| Databricks                     |[ML&nbsp;-pijp lijnen](concept-ml-pipelines.md)                                              |N/A                                           |[ML&nbsp;-pijp lijnen](concept-ml-pipelines.md)             |N/A                                                                         |
| Azure Batch                    |[ML&nbsp;-pijp lijnen](concept-ml-pipelines.md)                                               |N/A                                           |N/A         |N/A                                                                         |
| Azure DataLake Analytics       |N/A                                           |N/A                                           |[ML&nbsp;-pijp lijnen](concept-ml-pipelines.md)             |N/A                                                                         |

> [!NOTE]
> Er zijn mogelijk scenario's waarin zeer terugkerende gegevens processen sneller worden uitgevoerd met behulp `as_download()` van in `as_mount()`plaats van; dit kan experimenteel worden gevalideerd.

### <a name="examples"></a>Voorbeelden 

De volgende code voorbeelden zijn specifiek voor de [`Estimator`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) -klasse om toegang te krijgen tot uw Data Store tijdens de training.

Met deze code wordt een Estimator gemaakt met behulp `train.py`van het trainings script, vanuit de aangegeven bronmap met de `script_params`para meters die zijn gedefinieerd in, alle op het opgegeven trainings Compute-doel.

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data_dir': ds.as_mount()
}

est = Estimator(source_directory='your code directory',
                entry_script='train.py',
                script_params=script_params,
                compute_target=compute_target
                )
```

U kunt ook een lijst met gegevens opslag items door geven aan de `inputs` para meter van de Estimator-constructor om te koppelen aan of te kopiëren van uw gegevens opslag (s). Dit code voorbeeld:
* Hiermee wordt alle inhoud in de `ds1` gegevens opslag naar het reken doel gedownload voordat `train.py` het trainings script wordt uitgevoerd
* Hiermee wordt de `'./foo'` map in `ds2` de gegevens opslag naar het `train.py` Compute-doel gedownload voordat deze wordt uitgevoerd
* Uploadt het bestand `'./bar.pkl'` van het Compute-doel naar het gegevens `ds3` archief nadat het script is uitgevoerd

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[ds1.as_download(), ds2.path('./foo').as_download(), ds3.as_upload(path_on_compute='./bar.pkl')])
```

## <a name="access-datastores-during-for-scoring"></a>Toegang tot data stores tijdens het scoren

De Azure Machine Learning-service biedt verschillende manieren om uw modellen te gebruiken voor het scoren van punten. Sommige van deze methoden bieden geen toegang tot gegevens opslag. Gebruik de volgende tabel om inzicht te krijgen in de methoden waarmee u toegang hebt tot gegevens opslag in de Score:

| Methode | Toegang tot Data Store | Description |
| ----- | :-----: | ----- |
| [Batch-voor spelling](how-to-run-batch-predictions.md) | ✔ | Maak voor spellingen voor grote hoeveel heden gegevens asynchroon. |
| [Webservice](how-to-deploy-and-where.md) | &nbsp; | Model (len) implementeren als een webservice. |
| [Module IoT Edge](how-to-deploy-and-where.md) | &nbsp; | Model (len) implementeren op IoT Edge apparaten. |

In situaties waarin de SDK geen toegang biedt tot gegevens opslag, kunt u mogelijk aangepaste code maken met behulp van de relevante Azure SDK om toegang te krijgen tot de data. U kunt bijvoorbeeld de [Azure Storage SDK voor python](https://github.com/Azure/azure-storage-python) gebruiken om toegang te krijgen tot gegevens die zijn opgeslagen in blobs.

## <a name="next-steps"></a>Volgende stappen

* [Een model te trainen](how-to-train-ml-models.md)

* [Een model implementeren](how-to-deploy-and-where.md)
