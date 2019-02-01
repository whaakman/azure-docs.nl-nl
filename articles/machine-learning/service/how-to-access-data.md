---
title: Toegang tot gegevens in de gegevensopslag voor training
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van gegevensopslag voor toegang tot gegevensopslag tijdens de training met Azure Machine Learning-service
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.custom: seodec18
ms.openlocfilehash: 74ee7c4bf041edff559c6026f92418a496959556
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55485854"
---
# <a name="access-data-during-training-from-your-datastores"></a>Toegang tot gegevens tijdens de training van uw gegevensopslag
Een gegevensarchief gebruiken voor toegang tot en interactie met uw gegevens in Azure Machine Learning-werkstromen.

De gegevensopslag is in Azure Machine Learning-service is een abstractie via [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction). Het gegevensarchief kan verwijzen naar een een [Azure Blob](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) container of [Azure-bestandsshare](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) als de onderliggende opslag. 

## <a name="create-a-datastore"></a>Maken van een gegevensarchief
Voor het gebruik van gegevensopslag, moet u eerst een [werkruimte](concept-azure-machine-learning-architecture.md#workspace). Begin door een van beide [het maken van een nieuwe werkruimte](quickstart-create-workspace-with-python.md) of bij het ophalen van een bestaande:

```Python
import azureml.core
from azureml.core import Workspace

ws = Workspace.from_config()
```

### <a name="use-the-default-datastore"></a>Gebruik de standaard gegevensopslag
Hoeft te maken of configureren van een storage-account.  Elke werkruimte heeft een standaard-gegevensopslag die u kunt beginnen met het onmiddellijk.

Ophalen van de werkruimte standaard gegevensopslag:
```Python
ds = ws.get_default_datastore()
```

### <a name="register-a-datastore"></a>Registreren van een gegevensarchief
Hebt u een bestaande Azure-opslag, kunt u deze registreren als een gegevensarchief in uw werkruimte. U kunt een Azure Blob-Container of een Azure-bestandsshare registreren als een gegevensarchief. Alle methoden voor het registreren zich op de `Datastore` klasse en hebben de volgende indeling `register_azure_*`.

#### <a name="azure-blob-container-datastore"></a>Azure Blob-Container gegevensopslag
Een Azure Blob-Container-gegevensarchief registreren:

```Python
ds = Datastore.register_azure_blob_container(workspace=ws, 
                                             datastore_name='your datastore name', 
                                             container_name='your azure blob container name',
                                             account_name='your storage account name', 
                                             account_key='your storage account key',
                                             create_if_not_exists=True)
```

#### <a name="azure-file-share-datastore"></a>Azure File Share gegevensopslag
Een Azure-bestandsshare gegevensopslag registreren:

```Python
ds = Datastore.register_azure_file_share(workspace=ws, 
                                         datastore_name='your datastore name', 
                                         container_name='your file share name',
                                         account_name='your storage account name', 
                                         account_key='your storage account key',
                                         create_if_not_exists=True)
```

### <a name="get-an-existing-datastore"></a>Een bestaand gegevensarchief ophalen
Zoeken naar een geregistreerde gegevensopslag met de naam:
```Python
ds = Datastore.get(ws, datastore_name='your datastore name')
```

U kunt ook alle datastores ophalen voor een werkruimte:
```Python
datastores = ws.datastores
for name, ds in datastores.items():
    print(name, ds.datastore_type)
```

Voor het gemak, moet u een van uw geregistreerde gegevensopslag als de standaard gegevensopslag voor uw werkruimte instellen:
```Python
ws.set_default_datastore('your datastore name')
```

## <a name="upload-and-download-data"></a>Uploaden en downloaden van gegevens
### <a name="upload"></a>Uploaden
Een map of afzonderlijke bestanden uploaden naar de gegevensopslag met behulp van de Python-SDK.

Het uploaden van een map naar een gegevensarchief `ds`:
```Python
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

## <a name="access-datastores-for-training"></a>Toegang tot gegevensopslag voor training
U kunt toegang tot een gegevensarchief tijdens een training (bijvoorbeeld voor trainingen of validatie van gegevens) op een externe compute-doel via de Python-SDK wordt uitgevoerd. 

Er zijn twee ondersteunde manieren om uw gegevensopslag beschikbaar maken in de berekening die externe:
* **Koppelen**  
`ds.as_mount()`: in deze modus koppelpunt opgeeft, het gegevensarchief zal worden gekoppeld voor u in de berekening die externe. 
* **Download/uploaden**  
    * `ds.as_download(path_on_compute='your path on compute')` gegevens uit uw gegevensarchief downloadt naar de externe compute naar de locatie die is opgegeven door `path_on_compute`.
    * `ds.as_upload(path_on_compute='yourfilename'` gegevens worden geüpload naar het gegevensarchief.  Stel dat uw trainingsscript maakt u een `foo.pkl` bestand in de huidige werkmap op de externe compute. Dit bestand uploaden naar uw gegevensopslag met `ds.as_upload(path_on_compute='./foo.pkl')` nadat het script maakt u het bestand. Het bestand is geüpload naar de hoofdmap van uw gegevensarchief.
    
Als u wilt verwijzen naar een specifieke map of bestand op uw gegevensopslag, gebruik van het gegevensarchief **`path`** functie. Bijvoorbeeld, voor het downloaden van de inhoud van de `./bar` map uit het gegevensarchief naar uw compute-doel, gebruik `ds.path('./bar').as_download()`.

Alle `ds` of `ds.path` object wordt omgezet naar de naam van een omgevingsvariabele van de indeling `"$AZUREML_DATAREFERENCE_XXXX"` waarvan de waarde vertegenwoordigt het pad voor het koppelpunt/downloaden op de externe compute. Het pad van het gegevensarchief in de berekening die externe mogelijk niet hetzelfde als uitvoeringspad voor de van het script.

Voor toegang tot uw gegevensopslag tijdens de training, geven u deze in uw trainingsscript als een opdrachtregelargument via `script_params`:

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
`as_mount()` is de standaardmodus voor een gegevensopslag, zodat u kunt ook rechtstreeks gewoon doorgeven `ds` naar de `'--data_dir'` argument.

Of doorgeven in een lijst van gegevensopslag aan de constructor Estimator `inputs` parameter om te koppelen of kopiëren naar/van uw elke gegevensopslag:

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[ds1.as_download(), ds2.path('./foo').as_download(), ds3.as_upload(path_on_compute='./bar.pkl')])
```
De bovenstaande code wordt:
* Download de inhoud in het gegevensarchief `ds1` naar de externe compute voordat uw trainingsscript `train.py` wordt uitgevoerd
* Download de map `'./foo'` in het gegevensarchief `ds2` naar de externe compute voordat `train.py` wordt uitgevoerd
* Upload het bestand `'./bar.pkl'` van de externe compute tot het gegevensarchief `d3` nadat het script is uitgevoerd

## <a name="next-steps"></a>Volgende stappen
* [Een model te trainen](how-to-train-ml-models.md)
