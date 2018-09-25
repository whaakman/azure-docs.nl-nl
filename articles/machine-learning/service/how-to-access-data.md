---
title: Gegevensopslag in Azure Machine Learning gebruiken voor toegang tot gegevens
description: Het gebruik van gegevensopslag voor toegang tot gegevensopslag tijdens de training
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 615ab592c040eedf7d31e3a3036f558ea6c09740
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990804"
---
# <a name="how-to-access-data-during-training"></a>Toegang krijgen tot gegevens tijdens de training
Een gegevensopslag is in Azure Machine Learning-services, een abstractie via [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction). Het gegevensarchief kan verwijzen naar een een [Azure Blob](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) container of [Azure-bestandsshare](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) als de onderliggende opslag. Gegevensopslag kunt u eenvoudig toegang tot en interactie met de opslag van uw gegevens tijdens uw Azure Machine Learning-werkstromen via de Python-SDK of de CLI.

## <a name="create-a-datastore"></a>Maken van een gegevensarchief
Als u wilt gebruiken gegevensopslag, moet u eerst een [werkruimte](concept-azure-machine-learning-architecture.md#workspace). U kunt een nieuwe werkruimte maken of een bestaande resourcegroep ophalen:

```Python
import azureml.core
from azureml.core import Workspace

ws = Workspace.from_config()
```

### <a name="use-the-default-datastore"></a>Gebruik de standaard gegevensopslag
Elke werkruimte heeft een standaard-gegevensopslag die u kunt beginnen met het onmiddellijk Hiermee u het werk slaat van het maken en configureren uw eigen opslagaccounts.

Ophalen van de werkruimte standaard gegevensopslag:
```Python
ds = ws.get_default_datastore()
```

### <a name="register-a-datastore"></a>Registreren van een gegevensarchief
Als u de bestaande Azure Storage al hebt, kunt u het registreren als een gegevensarchief in uw werkruimte. Azure Machine Learning biedt de functionaliteit voor het registreren van een Azure Blob-Container of een Azure-bestandsshare als gegevensopslag. Alle methoden voor het registreren zich op de `Datastore` klasse en hebben de volgende indeling `register_azure_*`.

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
datastores = ws.datastores()
for name, ds in datastores.items(),
    print(name, ds.datastore_type)"
```

Voor het gemak, als u wilt een van uw geregistreerde gegevensopslag instellen als de standaard gegevensopslag voor uw werkruimte, kunt u doen als volgt:
```Python
ws.set_default_datastore('your datastore name')
```

## <a name="upload-and-download-data"></a>Uploaden en downloaden van gegevens
### <a name="upload"></a>Uploaden
U kunt een map of afzonderlijke bestanden uploaden naar de gegevensopslag met behulp van de Python-SDK.

Het uploaden van een map naar een gegevensarchief `ds`:
```Python
ds.upload(src_dir='your source directory',
          target_path='your target path',
          overwrite=True,
          show_progress=True)
```
`target_path` Hiermee geeft u de locatie in de bestandsshare (of de blob-container) om te uploaden. Wordt standaard `None`, in welk geval de gegevens wordt het geüpload naar de hoofdmap. `overwrite=True` overschrijft bestaande gegevens op `target_path`.

U kunt ook een lijst van afzonderlijke bestanden uploaden naar het gegevensarchief via van het gegevensarchief `upload_files()` methode.

### <a name="download"></a>Downloaden
Op deze manier kunt u gegevens uit een gegevensarchief downloaden naar uw lokale bestandssysteem.

```Python
ds.download(target_path='your target path',
            prefix='your prefix',
            show_progress=True)
```
`target_path` is de locatie van de lokale map voor het downloaden van de gegevens. Geef het pad naar de map in de bestandsshare (of de blob-container) om te downloaden, geeft dat pad op naar `prefix`. Als `prefix` is `None`, de inhoud van uw bestandsshare (of de blob-container) wordt gedownload.

## <a name="access-datastores-for-training"></a>Toegang tot gegevensopslag voor training
U kunt toegang tot een gegevensarchief tijdens een training (bijvoorbeeld voor trainingen of validatie van gegevens) uitgevoerd op een externe compute-doel via de Python-SDK. 

Er zijn twee ondersteunde manieren om uw gegevensopslag beschikbaar maken in de berekening die externe:
* **Koppelen**  
`ds.as_mount()`: in deze modus koppelpunt opgeeft, het gegevensarchief zal worden gekoppeld voor u in de berekening die externe. 
* **Download/uploaden**  
    * `ds.as_download(path_on_compute='your path on compute')`: met deze downloadmodus van de gegevens naar de externe compute naar de locatie die is opgegeven door zal ophalen gedownload uit het gegevensarchief `path_on_compute`.
    * U kunt de gegevens die is geproduceerd daarentegen ook uploaden van uw training uitgevoerd met een gegevensarchief. Bijvoorbeeld, als uw trainingsscript maakt u een `foo.pkl` bestand in de huidige werkmap op de externe compute-, u kunt opgeven voor het ophalen geüpload naar uw gegevensopslag nadat het script is uitgevoerd: `ds.as_upload(path_on_compute='./foo.pkl')`. Dit wordt het bestand uploaden naar de hoofdmap van uw gegevensarchief.
    
Als u verwijzen naar een specifieke map of bestand op uw gegevensopslag wilt, kunt u van het gegevensarchief **`path`** functie. Bijvoorbeeld, als uw gegevensarchief heeft een map met het relatieve pad `./bar`, en u wilt dat alleen de inhoud van deze map downloaden naar de compute-doel, kunt u doen als volgt: `ds.path('./bar').as_download()`

Alle `ds` of `ds.path` object wordt omgezet naar de naam van een omgevingsvariabele van de indeling `"$AZUREML_DATAREFERENCE_XXXX"` waarvan de waarde vertegenwoordigt het pad voor het koppelpunt/downloaden op de externe compute. Het pad van het gegevensarchief in de berekening die externe mogelijk niet hetzelfde als uitvoeringspad voor de van het script.

Voor toegang tot uw gegevensopslag tijdens de training, u kunt deze doorgeven aan uw trainingsscript als een opdrachtregelargument via `script_params`:

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

U kunt ook u kunt doorgeven in een lijst van gegevensopslag aan de `inputs` parameter van de constructor Estimator ontwikkeld om te koppelen of kopiëren naar/van uw elke gegevensopslag:

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
