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
ms.openlocfilehash: f489abeab0e1374d2d40ade79c4eb55fd633b909
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57443280"
---
# <a name="access-data-from-your-datastores"></a>Toegang tot gegevens uit uw gegevensopslag
In dit artikel leert u verschillende manieren om te openen en te communiceren met uw gegevens in Azure Machine Learning-werkstromen via de gegevensopslag.

Deze procedure ziet u voorbeelden voor de volgende taken: 
* [Een gegevensarchief kiezen](#access)
* [Een gegevensopslag ophalen](#get)
* [Uploaden en downloaden van gegevens naar gegevensopslag](#upload-and-download-data)
* Toegang tot gegevensopslag tijdens de training

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van gegevensopslag, moet u een [werkruimte](concept-azure-machine-learning-architecture.md#workspace) eerste. 

Begin door een van beide [het maken van een nieuwe werkruimte](quickstart-create-workspace-with-python.md) of bij het ophalen van een bestaande:

```Python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()
```

Of, [uitvoeren van deze quickstart Python](quickstart-create-workspace-with-python.md) naar de SDK gebruiken om uw werkruimte maken en aan de slag.

<a name="access"></a>

## <a name="choose-a-datastore"></a>Een gegevensarchief kiezen

U kunt het standaard-gegevensarchief gebruiken of Voeg uw eigen.

### <a name="use-the-default-datastore-in-your-workspace"></a>Gebruik de standaard gegevensopslag in uw werkruimte

Hoeft te maken of configureren van een storage-account, omdat elke werkruimte een standaard-gegevensopslag heeft. U kunt gebruiken die gegevensopslag direct omdat deze al is geregistreerd in de werkruimte. 

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

## <a name="get-data-in-your-datastore"></a>Gegevens in de gegevensopslag ophalen

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

## <a name="upload-and-download-data"></a>Uploaden en downloaden van gegevens
De [ `upload()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) en [ `download()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) methoden die worden beschreven in de volgende voorbeelden zijn specifiek voor en werken identiek voor de [AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py) en [AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py) klassen.

### <a name="upload"></a>Uploaden

 Een map of afzonderlijke bestanden uploaden naar de gegevensopslag met behulp van de Python-SDK.

Het uploaden van een map naar een gegevensarchief `ds`:

```Python
import azureml.data
from azureml.data import AzureFileDatastore, AzureBlobDatastore

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

## <a name="access-datastores-during-training"></a>Toegang tot gegevensopslag tijdens de training
U hebt toegang tot een gegevensarchief tijdens een training uitgevoerd (bijvoorbeeld voor trainingen of validatie van gegevens) op een externe compute-doel via de Python-SDK met de [ `DataReference` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py) klasse.

Er zijn verschillende manieren om uw gegevensopslag beschikbaar maken op de externe compute.

Manier|Methode|Description
----|-----|--------
Koppelen| [`as_mount()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--)| Gebruik om te koppelen van een gegevensarchief in de berekening die externe.
Downloaden|[`as_download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-)|Gebruiken om gegevens te downloaden vanaf de locatie die is opgegeven door `path_on_compute` op uw gegevensopslag op de externe compute.
Uploaden|[`as_upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)| Gebruiken om gegevens te uploaden naar de hoofdmap van de gegevensopslag van de opgegeven locatie `path_on_compute`.

```Python
import azureml.data
from azureml.data import DataReference

ds.as_mount()
ds.as_download(path_on_compute='your path on compute')
ds.as_upload(path_on_compute='yourfilename')
```  

### <a name="reference-filesfolders"></a>Naslaginformatie over bestanden/mappen
Als u wilt verwijzen naar een specifieke map of bestand op uw gegevensopslag, gebruik van het gegevensarchief [ `path()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#path-path-none--data-reference-name-none-) functie.

```Python
#download the contents of the `./bar` directory from the datastore 
ds.path('./bar').as_download()
```


### <a name="examples"></a>Voorbeelden 

Alle `ds` of `ds.path` object wordt omgezet naar de naam van een omgevingsvariabele van de indeling `"$AZUREML_DATAREFERENCE_XXXX"` waarvan de waarde vertegenwoordigt het pad voor het koppelpunt/downloaden op de externe compute. Het pad van het gegevensarchief in de berekening die externe mogelijk niet hetzelfde als uitvoeringspad voor de van het script.

Voor toegang tot uw gegevensopslag tijdens de training, geven u deze in uw trainingsscript als een opdrachtregelargument via `script_params` uit de [ `Estimator` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) klasse.

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data_dir': ds.as_mount()
}

est = Estimator(source_directory='your code directory',
                script_params=script_params,
                compute_target=compute_target,
                entry_script='train.py')
```
`as_mount()` is de standaardmodus voor een gegevensopslag, zodat u kunt ook rechtstreeks doorgeven `ds` naar de `'--data_dir'` argument.

Of doorgeven in een lijst van gegevensopslag aan de constructor Estimator `inputs` parameter om te koppelen of kopiëren naar/van uw elke gegevensopslag. Dit codevoorbeeld:
* Downloadt de inhoud in het gegevensarchief `ds1` naar de externe compute voordat uw trainingsscript `train.py` wordt uitgevoerd
* De map downloads `'./foo'` in het gegevensarchief `ds2` naar de externe compute voordat `train.py` wordt uitgevoerd
* Het bestand wordt geüpload `'./bar.pkl'` van de externe compute tot het gegevensarchief `d3` nadat het script is uitgevoerd

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[ds1.as_download(), ds2.path('./foo').as_download(), ds3.as_upload(path_on_compute='./bar.pkl')])
```


## <a name="next-steps"></a>Volgende stappen

* [Een model te trainen](how-to-train-ml-models.md)

* [Een model implementeren](how-to-deploy-and-where.md)
