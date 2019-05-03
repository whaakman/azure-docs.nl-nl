---
title: Maak en registreer gegevenssets met uw werkruimte
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
ms.date: 05/02/19
ms.openlocfilehash: 4b3fa69156146037ff59a41eab8c8373f6e01dc4
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029113"
---
# <a name="create-and-register-azure-machine-learning-datasets-preview"></a>Maken en registreren van Azure Machine Learning-gegevenssets (Preview)

In dit artikel leert u de Azure Machine Learning-werkstromen voor het maken en registreren van gegevenssets en hoe u toegang tot deze opnieuw kunt gebruiken voor lokale en externe experimenten.

Azure Machine Learning-gegevenssets (preview) kunt gemakkelijker toegang tot en met uw gegevens kunt werken. Gegevenssets beheren van gegevens in verschillende scenario's zoals modeltraining en pijplijn maken. Met behulp van de [SDK van Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py), u kunt werken met gegevens in veelgebruikte bestandsindelingen, toegang tot de onderliggende opslag, verkennen en voorbereiden van gegevens, beheren van de levenscyclus van definities van de verschillende gegevensset en vergelijken tussen gegevenssets die worden gebruikt training en in de productieomgeving.

## <a name="prerequisites"></a>Vereisten

U moet maken en registreren van gegevenssets:

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Een werkruimte van Azure Machine Learning-service. Zie [maken van een werkruimte van Azure Machine Learning-service](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace).

* De Azure Machine Learning-SDK voor Python. Als u wilt installeren of bijwerken naar de nieuwste versie van de SDK, Zie [installeren of bijwerken van de SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

## <a name="create-datasets-from-local-files"></a>Gegevenssets maken van lokale bestanden

Bestanden laden vanaf uw lokale computer door op te geven het pad van het bestand of map met de [ `auto_read_files()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#auto-read-files-path--include-path-false-) methode van de `Dataset` klasse.  Deze methode voert de volgende stappen uit zonder dat u het bestandstype opgeven of het parseren van de argumenten:

* Voorbeeldgrootte en het instellen van het scheidingsteken.
* Lege records aan de bovenkant van het bestand wordt overgeslagen.
* Voorbeeldgrootte en het instellen van de rij met koppen.
* Voorbeeldgrootte en het converteren van kolom-gegevenstypen.

```Python
from azureml.core import Dataset

dataset = Dataset.auto_read_files('./data/crime.csv')
```

De bestand-specifieke functies ook gebruiken voor het beheren van expliciet het parseren van het bestand. De SDK Datasets ondersteunt momenteel [gescheiden](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-delimited-files-path--separator------header--promoteheadersbehavior-allfileshavesameheaders--3---encoding--fileencoding-utf8--0---quoting-false--infer-column-types-true--skip-rows-0--skip-mode--skiplinesbehavior-norows--0---comment-none--include-path-false--archive-options-none-), [Excel](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-excel-files-path--sheet-name-none--use-column-headers-false--skip-rows-0--include-path-false--infer-column-types-true-), [Parquet](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-parquet-files-path--include-path-false-), [binaire](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-binary-files-path-), en [json](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-json-files-path--encoding--fileencoding-utf8--0---flatten-nested-arrays-false--include-path-false-) bestandsindelingen.

## <a name="create-datasets-from-azure-datastores"></a>Gegevenssets maken vanuit Azure gegevensopslag

Voor het maken van gegevenssets uit een Azure-gegevensarchief, moet u:

* Controleer of dat u hebt Inzender of eigenaar de toegang tot de geregistreerde Azure-gegevensopslag.

* Importeren van de [ `Workspace` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) en [ `Datastore` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#definition) en `Dataset` pakketten van de SDK.

```Python
from azureml.core import Workspace, Datastore, Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()
```

 De `get()` methode wordt een bestaand gegevensarchief in de werkruimte opgehaald.

```
dstore = Datastore.get(workspace, datastore_name)
```

Gebruik de `from_delimited_files()` methode om te lezen in bestanden met scheidingstekens en in-memory gegevenssets maken.

```Python
# create an in-memory Dataset on your local machine
datapath = dstore.path('data/src/crime.csv')
dataset = Dataset.from_delimited_files(datapath)

# returns the first 5 rows of the Dataset as a pandas Dataframe.
dataset.head(5)
```

||Id|Nummer van de aanvraag|Date|Blokkeren|IUCR|Het primaire Type|Description|Beschrijving van locatie|Aanhoudingsbevel|Binnenlandse|...|Ward|Community-gebied|Code van de FBI|X-coördinaat|Y-coördinaat|Jaar|Bijgewerkt op|Breedtegraad|Lengtegraad|Locatie|
|--|--|---|---|---|---|----|------|-------|------|-----|---|----|----|-----|-----|------|----|-----|----|----|-----
|0|10498554|HZ239907|4/4/2016 23:56|007XX E 111TH ST|1153|MISLEIDENDE PRAKTIJK|DIEFSTAL VAN FINANCIËLE IDENTITEITEN VIA $ 300|ANDERE|DE WAARDE FALSE|DE WAARDE FALSE|...|9|50|11|1183356|1831503|2016|5/11/2016 15:48|41.69283384|-87.60431945|(41.692833841, -87.60431945)|
1|10516598|HZ258664|4/15/2016 17:00|082XX S MARSHFIELD OPSLAAN|890|DIEFSTAL| VANAF HET OPSTELLEN|WOONPLAATS|DE WAARDE FALSE|DE WAARDE FALSE|...|21|71|6|1166776|1850053|2016|5/12/2016 15:48|41.74410697|-87.66449429|(41.744106973, -87.664494285)
2|10519196|HZ261252|4/15/2016 10:00|104XX S SACRAMENTO OPSLAAN|1154|MISLEIDENDE PRAKTIJK|DIEFSTAL VAN FINANCIËLE IDENTITEITEN $300 EN KLIKT U ONDER|WOONPLAATS|DE WAARDE FALSE|DE WAARDE FALSE|...|19|74|11|||2016|5/12/2016 15:50
3|10519591|HZ261534|4/15/2016 9:00|113XX S PRAIRIE OPSLAAN|1120|MISLEIDENDE PRAKTIJK|KUNNEN WORDEN VERVALST|WOONPLAATS|DE WAARDE FALSE|DE WAARDE FALSE|...|9|49|10|||2016|5/13/2016 15:51
4|10534446|HZ277630|4/15/2016 10:00|055XX N KEDZIE OPSLAAN|890|DIEFSTAL|VANAF HET OPSTELLEN|SCHOOL, OPENBAAR, HET BOUWEN VAN|DE WAARDE FALSE|DE WAARDE FALSE|...|40|13|6|||2016|5/25/2016 15:59|

## <a name="register-your-datasets-with-workspace"></a>Registreren van uw gegevenssets met de werkruimte

Gebruik de [ `register()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--visible-true--exist-ok-false--update-if-exist-false-) methode voor het registreren van gegevenssets aan uw werkruimte voor het delen en opnieuw te gebruiken binnen uw organisatie en tussen verschillende experimenten.

```Python
dataset = dataset.register(workspace = 'workspace_name',
                           name = "dataset_crime",
                           description = 'Training data',
                           exist_ok = False
                           )
```

>[!NOTE]
> De standaardinstelling voor de parameter voor `register()` is ' exist_ok = False'. Als u probeert te registreren van een gegevensset met dezelfde naam zonder deze instelling wordt een fout resulteert.

De `register()` methode werkt de definitie van een reeds geregistreerde gegevensset met de parameterinstelling van de `exist_ok = True`.

```Python
dataset = dataset.register(workspace = workspace_name,
                           name = "dataset_crime",
                           description = 'Training data',
                           exist_ok = True)
```

Gebruik `list()` om alle van de geregistreerde gegevenssets in uw werkruimte te bekijken.

```Python
Dataset.list(workspace_name)
```

De bovenstaande code resulteert in het volgende:

```Python
[Dataset(Name: dataset_crime,
         Workspace: workspace_name)]
```

## <a name="access-datasets-in-workspace"></a>Toegang tot gegevenssets in de werkruimte

Geregistreerde gegevenssets zijn toegankelijk en analyserapporten lokaal, extern en op rekenclusters zoals de Azure Machine Learning-berekeningen. Naar uw geregistreerde gegevensset gebruiken voor experimenten en compute-omgevingen, gebruik de volgende code om op te halen van uw werkruimte en de geregistreerde gegevensset met de naam.

```Python
workspace = Workspace.from_config()

dataset = workspace.Datasets['dataset_crime']
```

## <a name="next-steps"></a>Volgende stappen

* [Verken en gegevenssets voorbereiden](how-to-explore-prepare-data.md).
* [Beheren van de levenscyclus van definities van de gegevensset](how-to-manage-dataset-definitions.md).
* Zie voor een voorbeeld van het gebruik van gegevenssets, de [voorbeeld notitieblokken](https://aka.ms/dataset-tutorial).