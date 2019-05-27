---
title: Definitie van de gegevensset en versiebeheer met azureml-gegevenssets
titleSuffix: Azure Machine Learning service
description: Meer informatie over het bijwerken van definities van de gegevensset en beheren van de levenscyclus van definities
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
ms.reviewer: larryfr
ms.date: 05/02/2019
ms.openlocfilehash: e58ce156deaaad259ea7b74521bcf9b79afbd183
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66146210"
---
# <a name="update-and-manage-the-lifecycle-of-dataset-definitions"></a>Bijwerken en beheren van de levenscyclus van definities van de gegevensset

Informatie over het bijwerken en beheren van definities van de gegevensset met Azure Machine Learning-gegevenssets (preview).

Definities van de gegevensset zijn de recepten gebruikt voor het voorbereiden van uw gegevenssets, waaronder zowel de verwijzing naar uw gegevens en de transformatiestappen die worden uitgevoerd. Een gegevensset kan veel definities, en elke definitie heeft een eigen levenscyclus.

De volgende scenario is een voorbeeld van het gebruik van definities van de gegevensset:

1. Bouwt u een machine learning-pijplijn die de huidige definitie van uw gegevensset verbruikt.
1. De onderliggende gegevens veranderen; nieuwe kenmerken zijn toegevoegd.
1. U maakt een nieuwe versie van uw definitie waarmee extra transformaties voor het afhandelen van de nieuwe kenmerken worden toegevoegd.

In dit voorbeeld blijft de bestaande pijplijn gebruiken van de oorspronkelijke versie van de definitie. De nieuwe versie van de definitie van kan worden gebruikt voor nieuwe scenario's, zoals het trainen van een model of het maken van een pijplijn.

## <a name="prerequisites"></a>Vereisten

U moet beschikken over een Azure-abonnement en een werkruimte om u te registreren van uw gegevensset om te kunnen beheren van de levenscyclus van definities van de gegevensset.

Het voorbeeldbestand dat wordt gebruikt in de voorbeelden in dit document is beschikbaar op [ https://dprepdata.blob.core.windows.net/dataset-sample-files/crime.csv ](https://dprepdata.blob.core.windows.net/dataset-sample-files/crime.csv).

## <a name="update-dataset-definitions"></a>Bijwerken van definities van de gegevensset

Er wordt eerst maken en registreren van een gegevensset met uw werkruimte.

```python
from azureml.core import Workspace, Datastore, Dataset

# change the configuration for workspace and Datastore
subscription_id = 'your subscription id'
resource_group = 'your resource group name'
workspace_name = 'your workspace name'
datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace(subscription_id, resource_group, workspace_name)

# get a Datastore that has already been created in the workspace
dstore = Datastore.get(workspace, datastore_name)

# create an in-memory Dataset from Datastore
datapath = dstore.path('data/src/crime.csv')
dataset = Dataset.from_delimited_files(datapath)

# register the Dataset with the workspace. if a Dataset with the same name already exists, retrieve it by turning `exist_ok = True`
dataset_name = 'crime dataset'
dataset = dataset.register(workspace = workspace, 
                 name = dataset_name, 
                 description = 'crime dataset for demo', 
                 tags = {'year':'2019', 'month':'Apr'},
                 exist_ok = True)
```

De definitie van de eerste gegevensset (`version_id` = 1) wordt gemaakt wanneer de gegevensset is gemaakt. Telkens wanneer u de definitie van de gegevensset bijwerkt, wordt er vervolgens een nieuwe version_id worden toegewezen aan de meest recente definitie.

```python
# get the Dataset from the workspace by name
dataset = workspace.datasets['dataset_name']

# get the latest Dataset definition
ds_def = dataset.get_definition()

# update the Dataset definition to select only the columns I'm interested in
ds_def = ds_def.keep_columns(['ID', 'Arrest', 'Latitude', 'Longitude'])

# with this update, the latest definition for dataset now has `version_id = 2`
dataset = dataset.update_definition(ds_def, 'select useful column')

dataset = workspace.datasets['dataset_name']
dataset.head(5)
```
<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>Aanhoudingsbevel</th>
      <th>Breedtegraad</th>
      <th>Lengtegraad</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10498554</td>
      <td>DE WAARDE FALSE</td>
      <td>41.69283384</td>
      <td>-87.60431945</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10516598</td>
      <td>DE WAARDE FALSE</td>
      <td>41.74410697</td>
      <td>-87.66449429</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10519196</td>
      <td>DE WAARDE FALSE</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>10519591</td>
      <td>DE WAARDE FALSE</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>10534446</td>
      <td>DE WAARDE FALSE</td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>

Deze definitie-update worden de definities van de vorige gegevensset niet gewist. U kunt nog steeds openen en gebruiken van de vorige definities.

```python
# specify `version_id` to get a previous definition 
ds_def_old = dataset.get_definition(version_id = 1)
ds_def_old.head(5)
```
<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>Nummer van de aanvraag</th>
      <th>Date</th>
      <th>Blokkeren</th>
      <th>IUCR</th>
      <th>Het primaire Type</th>
      <th>Description</th>
      <th>Beschrijving van locatie</th>
      <th>Aanhoudingsbevel</th>
      <th>Binnenlandse</th>
      <th>...</th>
      <th>Ward</th>
      <th>Community-gebied</th>
      <th>Code van de FBI</th>
      <th>X-coördinaat</th>
      <th>Y-coördinaat</th>
      <th>Jaar</th>
      <th>Bijgewerkt op</th>
      <th>Breedtegraad</th>
      <th>Lengtegraad</th>
      <th>Locatie</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10498554</td>
      <td>HZ239907</td>
      <td>4/4/2016 23:56</td>
      <td>007XX E 111TH ST</td>
      <td>1153</td>
      <td>MISLEIDENDE PRAKTIJK</td>
      <td>DIEFSTAL VAN FINANCIËLE IDENTITEITEN VIA $ 300</td>
      <td>ANDERE</td>
      <td>DE WAARDE FALSE</td>
      <td>DE WAARDE FALSE</td>
      <td>...</td>
      <td>9</td>
      <td>50</td>
      <td>11</td>
      <td>1183356</td>
      <td>1831503</td>
      <td>2016</td>
      <td>5/11/2016 15:48</td>
      <td>41.69283384</td>
      <td>-87.60431945</td>
      <td>(41.692833841, -87.60431945)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10516598</td>
      <td>HZ258664</td>
      <td>4/15/2016 17:00</td>
      <td>082XX S MARSHFIELD OPSLAAN</td>
      <td>890</td>
      <td>DIEFSTAL</td>
      <td>VANAF HET OPSTELLEN</td>
      <td>WOONPLAATS</td>
      <td>DE WAARDE FALSE</td>
      <td>DE WAARDE FALSE</td>
      <td>...</td>
      <td>21</td>
      <td>71</td>
      <td>6</td>
      <td>1166776</td>
      <td>1850053</td>
      <td>2016</td>
      <td>5/12/2016 15:48</td>
      <td>41.74410697</td>
      <td>-87.66449429</td>
      <td>(41.744106973, -87.664494285)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10519196</td>
      <td>HZ261252</td>
      <td>4/15/2016 10:00</td>
      <td>104XX S SACRAMENTO OPSLAAN</td>
      <td>1154</td>
      <td>MISLEIDENDE PRAKTIJK</td>
      <td>DIEFSTAL VAN FINANCIËLE IDENTITEITEN $300 EN KLIKT U ONDER</td>
      <td>WOONPLAATS</td>
      <td>DE WAARDE FALSE</td>
      <td>DE WAARDE FALSE</td>
      <td>...</td>
      <td>19</td>
      <td>74</td>
      <td>11</td>
      <td></td>
      <td></td>
      <td>2016</td>
      <td>5/12/2016 15:50</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>10519591</td>
      <td>HZ261534</td>
      <td>4/15/2016 9:00</td>
      <td>113XX S PRAIRIE OPSLAAN</td>
      <td>1120</td>
      <td>MISLEIDENDE PRAKTIJK</td>
      <td>KUNNEN WORDEN VERVALST</td>
      <td>WOONPLAATS</td>
      <td>DE WAARDE FALSE</td>
      <td>DE WAARDE FALSE</td>
      <td>...</td>
      <td>9</td>
      <td>49</td>
      <td>10</td>
      <td></td>
      <td></td>
      <td>2016</td>
      <td>5/13/2016 15:51</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>10534446</td>
      <td>HZ277630</td>
      <td>4/15/2016 10:00</td>
      <td>055XX N KEDZIE OPSLAAN</td>
      <td>890</td>
      <td>DIEFSTAL</td>
      <td>VANAF HET OPSTELLEN</td>
      <td>SCHOOL, OPENBAAR, HET BOUWEN VAN</td>
      <td>DE WAARDE FALSE</td>
      <td>DE WAARDE FALSE</td>
      <td>...</td>
      <td>40</td>
      <td>13</td>
      <td>6</td>
      <td></td>
      <td></td>
      <td>2016</td>
      <td>5/25/2016 15:59</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>

Zoals we in het resultaat zien kunt, blijft de eerste versie van de definitie van de gegevensset nog steeds alle kolommen uit de crime-gegevens, ongeacht de latere update. Bijvoorbeeld, hebt u een machine learning model momenteel de eerste versie van de gegevensset verbruikt en het gebruik van `Date` kolom uit de gegevensset als een van de functies, wordt u niet fouten met de meest recente definitie-update, zodat alleen gevonden`ID`, `Arrest`, `Latitude`, `Longitude` kolommen uit de crime-gegevens.

## <a name="how-to-access-dataset-definitions"></a>Toegang tot de definities van de gegevensset

Als u een lijst met alle definities, gebruikt `get_definitions()`. Voor een specifieke versie van de definitie van een gebruikt `get_definition()`. Het volgende voorbeeld ziet u een lijst met alle definities ophalen en vervolgens bij het ophalen van versie 1:

```python
# list all definitions for the dataset
dataset.get_definitions()
# get version ID 1
dataset.get_definition(version_id=1)
```

De uitvoer van `get_definitions()` is vergelijkbaar met het volgende voorbeeld:

```text
{'2': VersionID: 2, State: active, Created: 2019-04-19 16:43:52.439890+00:00, Modified: 2019-04-19 16:43:52.439890+00:00, Notes: select useful column,
 '1': VersionID: 1, State: active, Created: 2019-04-19 16:39:14.112046+00:00, Modified: 2019-04-19 16:44:12.912663+00:00, Notes: None}
```

Wanneer u een definitie hebt opgehaald, kunt u deze met machine learning-modellen of in een machine learning-pijplijn.

## <a name="manage-lifecycle-of-dataset-definitions"></a>Levenscyclus van definities van de gegevensset beheren

U kunt de levenscyclus van elke definitie van de gegevensset onafhankelijk kunt beheren. Er zijn drie fasen van de levenscyclus: actief, vervangen, of te archiveren.

### <a name="active"></a>Actief

Wanneer de gegevenssetdefinitie van een nieuwe wordt gemaakt, is het standaard actief. 

### <a name="deprecate"></a>Afschaffen

Definities van de gegevensset kunnen worden afgeschaft als gebruik niet meer wordt aanbevolen, een vervanging is beschikbaar. Wanneer een afgeschafte definitie van de gegevensset wordt gebruikt in machine learning-pijplijnen, een waarschuwingsbericht wordt geretourneerd, maar kan worden uitgevoerd worden niet geblokkeerd.

Wanneer de gegevenssetdefinitie van een beëindigde, geef de gegevensset-ID en de ID van de gegevensset definitie voor de definitie van de vervanging. Deze informatie wordt opgeslagen en gebruikt in een waarschuwing weergegeven wanneer u probeert te gebruiken een afgeschafte definitie van de gegevensset.

```python
# get the definition that you want to deprecate
ds_def = dataset.get_definition(version_id = 1)

# deprecate it by providing the information for the replacement definition, which is recommended to be used in machine learning scenarios
ds_def.deprecate(deprecate_by_dataset_id=dataset.id, deprecated_by_definition_version=2)
```

### <a name="archive"></a>Archief

Definities van de gegevensset kunnen worden gearchiveerd wanneer definities mag niet worden gebruikt voor een bepaalde reden (zoals de onderliggende gegevens niet meer beschikbaar). Wanneer een gearchiveerde definitie van de gegevensset wordt gebruikt in machine learning-pijplijnen, worden uitvoering geblokkeerd vanwege een fout.

```python
# archive the definition with `version_id = 1`
ds_def = dataset.get_definition(version_id = 1)
ds_def.archive()
```

### <a name="reactivate"></a>Opnieuw activeren

U kunt definitie van de afgeschafte of gearchiveerde eenvoudig opnieuw activeren.

```python
# reactivate Dataset definition with `version_id =1` which was archived in the previous step
ds_def = dataset.get_definition(version_id = 1)
ds_def.reactivate()
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het werken met gegevenssets [maken en registreren Azure Machine Learning gegevenssets](how-to-create-register-datasets.md).

Zie voor een voorbeeld van het gebruik van gegevenssets, de [voorbeeld notitieblokken](https://aka.ms/dataset-tutorial).
