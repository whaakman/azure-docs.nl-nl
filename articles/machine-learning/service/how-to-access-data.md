---
title: Toegang tot gegevens in de gegevensopslag / blobs voor training
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruiken van gegevensopslag voor toegang tot blob-opslag voor gegevens tijdens de training met Azure Machine Learning-service
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 02/25/2019
ms.custom: seodec18
ms.openlocfilehash: 85910e2f422ea45b2468f20b4ff9425f64ca3cbe
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793407"
---
# <a name="access-data-from-your-datastores"></a>Toegang tot gegevens uit uw gegevensopslag

 In Azure Machine Learning-service zijn gegevensopslag compute-locatie-onafhankelijke mechanismen voor toegang tot opslag zonder wijzigingen aan uw broncode. Of u training code schrijven om te worden van een pad als een parameter of geef een gegevensopslag rechtstreeks naar een estimator, Azure Machine Learning-werkstromen Controleer of uw locaties voor gegevensopslag zijn toegankelijk en beschikbaar gesteld aan uw compute-context.

Deze procedure ziet u voorbeelden van de volgende taken:
* [Een gegevensarchief kiezen](#access)
* [Gegevens ophalen](#get)
* [Uploaden en downloaden van gegevens naar gegevensopslag](#up-and-down)
* [Toegang tot gegevensopslag tijdens de training](#train)

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van gegevensopslag, moet u eerst een [werkruimte](concept-azure-machine-learning-architecture.md#workspace).

Begin door een van beide [het maken van een nieuwe werkruimte](setup-create-workspace.md#sdk) of bij het ophalen van een bestaande:

```Python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()
```

<a name="access"></a>

## <a name="choose-a-datastore"></a>Een gegevensarchief kiezen

U kunt het standaard-gegevensarchief gebruiken of Voeg uw eigen.

### <a name="use-the-default-datastore-in-your-workspace"></a>Gebruik de standaard gegevensopslag in uw werkruimte

 Elke werkruimte heeft een geregistreerde, standaard-gegevensopslag die u meteen kunt gebruiken.

Ophalen van de werkruimte standaard gegevensopslag:

```Python
ds = ws.get_default_datastore()
```

### <a name="register-your-own-datastore-with-the-workspace"></a>Uw eigen gegevensarchief registreren bij de werkruimte

Hebt u een bestaande Azure-opslag, kunt u deze registreren als een gegevensarchief in uw werkruimte.   Alle methoden voor het registreren zich op de [ `Datastore` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) klasse en hebben de vorm register_azure_ *. 

De volgende voorbeelden ziet u voor het registreren van een Azure Blob-Container of een Azure-bestandsshare als gegevensopslag.

+ Voor een **Azure Blob-Container gegevensopslag**, gebruiken [`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py)

  ```Python
  ds = Datastore.register_azure_blob_container(workspace=ws, 
                                               datastore_name='your datastore name', 
                                               container_name='your azure blob container name',
                                               account_name='your storage account name', 
                                               account_key='your storage account key',
                                               create_if_not_exists=True)
  ```

+ Voor een **Azure File Share gegevensopslag**, gebruikt u [ `register_azure_file_share()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-). Bijvoorbeeld: 
  ```Python
  ds = Datastore.register_azure_file_share(workspace=ws, 
                                           datastore_name='your datastore name', 
                                           container_name='your file share name',
                                           account_name='your storage account name', 
                                           account_key='your storage account key',
                                           create_if_not_exists=True)
  ```

<a name="get"></a>

## <a name="find--define-datastores"></a>Zoeken en gegevensopslag definiëren

Voor een opgegeven gegevensarchief in de huidige werkruimte hebt geregistreerd, gebruikt [ `get()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-) :

```Python
#get named datastore from current workspace
ds = Datastore.get(ws, datastore_name='your datastore name')
```

Als u een lijst met alle gegevensopslag in een opgegeven werkruimte, gebruikt u deze code:

```Python
#list all datastores registered in current workspace
datastores = ws.datastores
for name, ds in datastores.items():
    print(name, ds.datastore_type)
```

Gebruik het definiëren van een andere standaard gegevensopslag voor de huidige werkruimte [ `set_default_datastore()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#set-default-datastore-name-):

```Python
#define default datastore for current workspace
ws.set_default_datastore('your datastore name')
```

<a name="up-and-down"></a>
## <a name="upload--download-data"></a>Uploaden en downloaden van gegevens
De [ `upload()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) en [ `download()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) methoden die worden beschreven in de volgende voorbeelden zijn specifiek voor en werken identiek voor de [AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py) en [AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py) klassen.

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
## <a name="access-datastores-during-training"></a>Toegang tot gegevensopslag tijdens de training

Nadat u uw gegevensopslag beschikbaar op de compute-doel maken, kunt u deze openen tijdens trainingsuitvoeringen (bijvoorbeeld training of validatie van gegevens) door gewoon het pad naar deze wordt doorgegeven als een parameter in uw trainingsscript.

De volgende tabel bevat de [ `DataReference` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py) methoden die de compute-doel vertellen over het gebruik van het gegevensarchief tijdens wordt uitgevoerd.

Manier|Methode|Description|
----|-----|--------
Koppelen| [`as_mount()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--)| Gebruiken om te koppelen van het gegevensarchief in de compute-doel.
Downloaden|[`as_download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-)|Gebruik voor het downloaden van de inhoud van uw gegevensopslag naar de locatie die is opgegeven door `path_on_compute`. <br> Voor de context van de training uitvoert gebeurt dit downloaden voordat de uitvoering.
Uploaden|[`as_upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)| Gebruiken voor het uploaden van een bestand van de opgegeven locatie `path_on_compute` aan uw gegevensarchief. <br> Voor de context van de training uitvoert gebeurt dit uploaden na de uitvoering.

 ```Python
import azureml.data
from azureml.data.data_reference import DataReference

ds.as_mount()
ds.as_download(path_on_compute='your path on compute')
ds.as_upload(path_on_compute='yourfilename')
```  

Als u wilt verwijzen naar een specifieke map of bestand op uw gegevensopslag en beschikbaar maken op de compute-doel, gebruik van het gegevensarchief [ `path()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#path-path-none--data-reference-name-none-) functie.

```Python
#download the contents of the `./bar` directory in ds to the compute target
ds.path('./bar').as_download()
```

> [!NOTE]
> Alle `ds` of `ds.path` object wordt omgezet naar de naam van een omgevingsvariabele van de indeling `"$AZUREML_DATAREFERENCE_XXXX"` waarvan de waarde vertegenwoordigt het pad voor het koppelpunt/downloaden in de berekening die doel. Het pad van het gegevensarchief in de berekening die doel mogelijk niet hetzelfde als uitvoeringspad voor de van het trainingsscript.

### <a name="compute-context-and-datastore-type-matrix"></a>COMPUTE-context en gegevensopslag type matrix

De volgende matrix geeft de beschikbare gegevens toegang tot functies voor de andere compute-context en gegevensopslag scenario's. De term "Pipeline" in deze matrix verwijst naar de mogelijkheid voor het gebruik van gegevensopslag als invoer of uitvoer in [Azure Machine Learning-pijplijnen](https://docs.microsoft.com/azure/machine-learning/service/concept-ml-pipelines).

||Lokale Compute|Azure Machine Learning-Computing|Gegevensoverdracht|Databricks|HDInsight|Azure Batch|Azure DataLake Analytics|Virtuele machines|
-|--|-----------|----------|---------|-----|--------------|---------|---------|
|AzureBlobDatastore|[`as_download()`] [`as_upload()`]|[`as_mount()`]<br> [`as_download()`] [`as_upload()`] <br> Pijplijn|Pijplijn|Pijplijn|[`as_download()`] <br> [`as_upload()`]|Pijplijn||[`as_download()`] <br> [`as_upload()`]|
|AzureFileDatastore|[`as_download()`] [`as_upload()`]|[`as_mount()`]<br> [`as_download()`] [`as_upload()`] Pipeline |||[`as_download()`] [`as_upload()`]|||[`as_download()`] [`as_upload()`]|
|AzureDataLakeDatastore|||Pijplijn|Pijplijn|||Pijplijn||
|AzureDataLakeGen2Datastore|||Pijplijn||||||
|AzureDataPostgresSqlDatastore|||Pijplijn||||||
|AzureSqlDatabaseDataDatastore|||Pijplijn||||||


> [!NOTE]
> Mogelijk zijn er scenario's waarin zeer iteratieve, grote hoeveelheden gegevensprocessen worden uitgevoerd met behulp van snellere [`as_download()`] in plaats van [`as_mount()`]; dit experimenteel kan worden gevalideerd.

### <a name="examples"></a>Voorbeelden 

De volgende codevoorbeelden zijn specifiek voor de [ `Estimator` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) klasse voor toegang tot uw gegevensopslag tijdens de training.

Deze code maakt een Estimator ontwikkeld met behulp van het trainingsscript `train.py`, uit de opgegeven bronmap met behulp van de gedefinieerde parameters in `script_params`, alles op de opgegeven compute-doel.

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

U kunt ook in een lijst van gegevensopslag doorgeven aan de constructor Estimator `inputs` parameter om te koppelen of kopiëren naar/van uw elke gegevensopslag. Dit codevoorbeeld:
* Downloadt de inhoud in het gegevensarchief `ds1` naar de compute-doel voordat u uw trainingsscript `train.py` wordt uitgevoerd
* De map downloads `'./foo'` in het gegevensarchief `ds2` naar de compute-doel voordat `train.py` wordt uitgevoerd
* Het bestand wordt geüpload `'./bar.pkl'` van de compute-doel tot het gegevensarchief `ds3` nadat het script is uitgevoerd

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[ds1.as_download(), ds2.path('./foo').as_download(), ds3.as_upload(path_on_compute='./bar.pkl')])
```

## <a name="next-steps"></a>Volgende stappen

* [Een model te trainen](how-to-train-ml-models.md)

* [Een model implementeren](how-to-deploy-and-where.md)
