---
title: Data sets maken voor toegang tot gegevens met azureml-gegevens sets
titleSuffix: Azure Machine Learning service
description: Meer informatie over het maken van gegevens sets van verschillende bronnen en het registreren van gegevens sets met uw werk ruimte
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/21/2019
ms.openlocfilehash: 765ec8291ba873c6b200cf330d82e6e2ab53357d
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68423110"
---
# <a name="create-and-access-datasets-preview-in-azure-machine-learning"></a>Gegevens sets (preview) maken en openen in Azure Machine Learning

In dit artikel leert u hoe u Azure Machine Learning gegevens sets (preview) maakt en hoe u toegang krijgt tot gegevens uit lokale of externe experimenten.

Met Azure Machine Learning gegevens sets kunt u het volgende doen: 

* **Eén kopie van gegevens bewaren in uw opslag** waarnaar wordt verwezen door gegevens sets

* **Gegevens analyseren** door middel van experimentele gegevens analyse 

* **Eenvoudig toegang krijgen tot gegevens tijdens model training** zonder dat u zich zorgen hoeft te maken over Connection String of gegevenspad

* **Gegevens delen & samen werken** met andere gebruikers

## <a name="prerequisites"></a>Vereisten

Als u gegevens sets wilt maken en gebruiken, hebt u het volgende nodig:

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Een [Azure Machine Learning Services-werk ruimte](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace)

* De [Azure machine learning SDK voor python is geïnstalleerd](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), waaronder het pakket met de azureml-gegevens sets.

> [!Note]
> Sommige klassen van gegevensset (preview) hebben afhankelijkheden voor het [dataprep-](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) pakket (ga). Voor Linux-gebruikers worden deze klassen alleen ondersteund in de volgende distributies:  Red Hat Enterprise Linux, Ubuntu, Fedora en CentOS.

## <a name="data-formats"></a>Gegevens indelingen

U kunt een Azure Machine Learning-gegevensset maken op basis van de volgende indelingen:
+ [gescheiden](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset#from-delimited-files-path--separator------header--promoteheadersbehavior-all-files-have-same-headers--3---encoding--fileencoding-utf8--0---quoting-false--infer-column-types-true--skip-rows-0--skip-mode--skiplinesbehavior-no-rows--0---comment-none--include-path-false--archive-options-none-)
+ [json](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-json-files-path--encoding--fileencoding-utf8--0---flatten-nested-arrays-false--include-path-false-)
+ [Excel](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-excel-files-path--sheet-name-none--use-column-headers-false--skip-rows-0--include-path-false--infer-column-types-true-)
+ [Parquet](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-parquet-files-path--include-path-false-)
+ [Panda data frame](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-pandas-dataframe-dataframe--path-none--in-memory-false-)
+ [SQL-query](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-sql-query-data-source--query-)
+ [waarde](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-binary-files-path-)

## <a name="create-datasets"></a>Gegevenssets maken 

Door een gegevensset te maken, maakt u een verwijzing naar de locatie van de gegevens bron, samen met een kopie van de meta gegevens ervan. De gegevens blijven op de bestaande locatie, waardoor er geen extra opslag kosten in rekening worden gebracht.

### <a name="create-from-local-files"></a>Maken op basis van lokale bestanden

Laad bestanden van uw lokale computer door het pad naar het bestand of de map [`auto_read_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#auto-read-files-path--include-path-false-) op te geven `Dataset` met de methode van de-klasse.  Met deze methode worden de volgende stappen uitgevoerd zonder dat u het bestands type of het parseren van argumenten hoeft op te geven:

* Het scheidings teken wordt uitgesteld en ingesteld.
* Lege records worden overs Laan boven aan het bestand.
* Het uitstellen en instellen van de koprij.
* De kolom gegevens typen worden uitgesteld en geconverteerd.

```Python
from azureml.core.dataset import Dataset

dataset = Dataset.auto_read_files('./data/crime.csv')
```

U kunt ook de specifieke functies gebruiken om het parseren van het bestand expliciet te beheren. 


### <a name="create-from-azure-datastores"></a>Maken op basis van Azure-gegevens opslag

Gegevens sets maken op basis van een [Azure-gegevens opslag](how-to-access-data.md):

* Controleer of `contributor` `owner` u toegang hebt tot het geregistreerde Azure-gegevens archief.

* De gegevensset maken door te verwijzen naar een pad in het gegevens archief 

```Python
from azureml.core.workspace import Workspace
from azureml.core.datastore import Datastore
from azureml.core.dataset import Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()

# retrieve an existing datastore in the workspace by name
dstore = Datastore.get(workspace, datastore_name)
```

Gebruik de `from_delimited_files()` methode om gescheiden bestanden van een [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py)te lezen en een niet-geregistreerde gegevensset te maken.

```Python
# create an in-memory Dataset on your local machine
dataset = Dataset.from_delimited_files(dstore.path('data/src/crime.csv'))

# returns the first 5 rows of the Dataset as a pandas Dataframe.
dataset.head(5)
```

## <a name="register-datasets"></a>Gegevens sets registreren

Als u het aanmaak proces wilt volt ooien, registreert u uw gegevens sets met de werk ruimte:

Gebruik de [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--visible-true--exist-ok-false--update-if-exist-false-) methode om gegevens sets te registreren in uw werk ruimte, zodat deze kunnen worden gedeeld met anderen en opnieuw worden gebruikt in verschillende experimenten.

```Python
dataset = dataset.register(workspace = workspace,
                           name = 'dataset_crime',
                           description = 'Training data',
                           exist_ok = False
                           )
```

>[!NOTE]
> Als `exist_ok = False` (standaard) en u probeert een gegevensset met dezelfde naam als een andere te registreren, treedt er een fout op. Stel in `True` op om bestaande te overschrijven.

## <a name="access-data-in-datasets"></a>Toegang tot gegevens in gegevens sets

Geregistreerde gegevens sets zijn lokaal en extern toegankelijk op reken clusters, zoals de Azure Machine Learning compute. Als u toegang wilt krijgen tot uw geregistreerde gegevensset voor experimenten, gebruikt u de volgende code om uw werk ruimte en geregistreerde gegevensset op naam op te halen.

```Python
workspace = Workspace.from_config()

# See list of datasets registered in workspace.
print(Dataset.list(workspace))

# Get dataset by name
dataset = Dataset.get(workspace, 'dataset_crime')

# Load data into pandas DataFrame
dataset.to_pandas_dataframe()
```

## <a name="next-steps"></a>Volgende stappen

* [Gegevens sets verkennen en voorbereiden](how-to-explore-prepare-data.md).
* Zie de [voorbeeld notitieblokken](https://aka.ms/dataset-tutorial)voor een voor beeld van het gebruik van gegevens sets.
