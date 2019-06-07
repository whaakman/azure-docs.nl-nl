---
title: Gegevenssets voor toegang tot gegevens met azureml-gegevenssets maken
titleSuffix: Azure Machine Learning service
description: Meer informatie over het maken van gegevenssets uit verschillende bronnen en gegevenssets registreren met uw werkruimte
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/21/2019
ms.openlocfilehash: b4c22caae86e20b8379db2b7feffb1ca82001239
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66753152"
---
# <a name="create-and-access-datasets-preview-in-azure-machine-learning"></a>Maken en toegang tot gegevenssets (Preview) in Azure Machine Learning

In dit artikel leert u hoe u Azure Machine Learning-gegevenssets (preview) maakt en hoe u toegang tot de gegevens van lokale en externe experimenten.

Met beheerde gegevenssets, kunt u het volgende doen: 
* **Eenvoudig toegang krijgen tot gegevens tijdens het trainen van het model** zonder opnieuw verbinding maken met de onderliggende winkels

* **Zorg ervoor dat de consistentie van gegevens en reproduceerbaarheid** met behulp van de dezelfde aanwijzer over experimenten: laptops, geautomatiseerde ml, pijplijnen, visuele interface

* **Gegevens delen en samenwerken** met andere gebruikers

* **Gegevens verkennen** & levenscyclus van momentopnamen en versies beheren

* **Gegevens vergelijken** in training naar productie


## <a name="prerequisites"></a>Vereisten

Als u wilt maken en werken met gegevenssets, hebt u het volgende nodig:

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Een [werkruimte van Azure Machine Learning-service](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace)

* De [Azure Machine Learning-SDK voor Python geïnstalleerd](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), waaronder het pakket azureml-gegevenssets.

> [!Note]
> Bepaalde klassen gegevensset (preview) zijn afhankelijk van de [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) pakket (GA). Voor Linux-gebruikers, worden deze klassen alleen op de volgende distributies ondersteund:  Red Hat Enterprise Linux, Ubuntu, Fedora en CentOS.

## <a name="data-formats"></a>Opmaak van gegevens

U kunt een Azure Machine Learning-gegevensset maken van de volgende gegevens:
+ [gescheiden](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset#from-delimited-files-path--separator------header--promoteheadersbehavior-all-files-have-same-headers--3---encoding--fileencoding-utf8--0---quoting-false--infer-column-types-true--skip-rows-0--skip-mode--skiplinesbehavior-no-rows--0---comment-none--include-path-false--archive-options-none-)
+ [binary](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-binary-files-path-)
+ [json](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-json-files-path--encoding--fileencoding-utf8--0---flatten-nested-arrays-false--include-path-false-)
+ [Excel](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-excel-files-path--sheet-name-none--use-column-headers-false--skip-rows-0--include-path-false--infer-column-types-true-)
+ [Parquet](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-parquet-files-path--include-path-false-)
+ [Azure SQL Database](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-sql-query-data-source--query-)
+ [Azure Data Lake gen. 1](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-sql-query-data-source--query-)

## <a name="create-datasets"></a>Gegevenssets maken 

U kunt werken met uw gegevenssets met de azureml-gegevenssets-pakket in de [Azure Machine Learning Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) en met name [de `Dataset` klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py).

### <a name="create-from-local-files"></a>Maken van lokale bestanden

Bestanden laden vanaf uw lokale computer door op te geven het pad van het bestand of map met de [ `auto_read_files()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#auto-read-files-path--include-path-false-) methode van de `Dataset` klasse.  Deze methode voert de volgende stappen uit zonder dat u het bestandstype opgeven of het parseren van de argumenten:

* Voorbeeldgrootte en het instellen van het scheidingsteken.
* Lege records aan de bovenkant van het bestand wordt overgeslagen.
* Voorbeeldgrootte en het instellen van de rij met koppen.
* Voorbeeldgrootte en het converteren van kolom-gegevenstypen.

```Python
from azureml.core.dataset import Dataset

dataset = Dataset.auto_read_files('./data/crime.csv')
```

De bestand-specifieke functies ook gebruiken voor het beheren van expliciet het parseren van het bestand. 


### <a name="create-from-azure-datastores"></a>Maken op basis van Azure-gegevensopslag

Gegevenssets uit een Azure-gegevensarchief maken:

* Controleer of u hebt `contributor` of `owner` toegang tot de geregistreerde Azure-gegevensopslag.

* Importeren van de [ `Workspace` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) en [ `Datastore` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#definition) en `Dataset` pakketten van de SDK.

```Python
from azureml.core.workspace import Workspace
from azureml.core.datastore import Datastore
from azureml.core.dataset import Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()
```

 De `get()` methode wordt een bestaand gegevensarchief in de werkruimte opgehaald.

```
dstore = Datastore.get(workspace, datastore_name)
```

Gebruik de `from_delimited_files()` methode om te lezen in bestanden met scheidingstekens en een niet-geregistreerde gegevensset maken.

```Python
# create an in-memory Dataset on your local machine
datapath = dstore.path('data/src/crime.csv')
dataset = Dataset.from_delimited_files(datapath)

# returns the first 5 rows of the Dataset as a pandas Dataframe.
dataset.head(5)
```

## <a name="register-datasets"></a>Gegevenssets registreren

Registreren voor het voltooien van het proces voor het maken, uw gegevenssets met werkruimte:

Gebruik de [ `register()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--visible-true--exist-ok-false--update-if-exist-false-) methode voor het registreren van gegevenssets aan uw werkruimte, zodat ze kunnen worden gedeeld met anderen en voor verschillende experimenten hergebruikt.

```Python
dataset = dataset.register(workspace = workspace,
                           name = 'dataset_crime',
                           description = 'Training data',
                           exist_ok = False
                           )
```

>[!NOTE]
> Als `exist_ok = False` (standaard), en u probeert te registreren van een gegevensset met dezelfde naam als een andere, een fout optreedt. Ingesteld op `True` overschrijven bestaande.

## <a name="access-data-in-datasets"></a>Toegang tot gegevens in gegevenssets

Geregistreerde gegevenssets zijn toegankelijk en analyserapporten lokaal, extern en op rekenclusters zoals de Azure Machine Learning-berekeningen. Naar uw geregistreerde gegevensset gebruiken voor experimenten en compute-omgevingen, gebruik de volgende code om op te halen van uw werkruimte en de geregistreerde gegevensset met de naam.

```Python
workspace = Workspace.from_config()

# See list of datasets registered in workspace.
Dataset.list(workspace)

# Get dataset by name
dataset = workspace.datasets['dataset_crime']
```

## <a name="next-steps"></a>Volgende stappen

* [Verken en gegevenssets voorbereiden](how-to-explore-prepare-data.md).
* [Beheren van de levenscyclus van definities van de gegevensset](how-to-manage-dataset-definitions.md).
* Zie voor een voorbeeld van het gebruik van gegevenssets, de [voorbeeld notitieblokken](https://aka.ms/dataset-tutorial).
