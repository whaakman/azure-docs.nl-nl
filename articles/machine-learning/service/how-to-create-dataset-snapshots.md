---
title: Gegevens met momentopnamen van de gegevensset reproduceren & vergelijkingen
titleSuffix: Azure Machine Learning service
description: Meer informatie over het vergelijken van gegevens na verloop van tijd en zorg ervoor dat reproduceerbaarheid met momentopnamen van de gegevensset
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
ms.date: 05/23/2019
ms.openlocfilehash: 525660be0f38c9458590e52cfcd575acb4cf5444
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66162062"
---
# <a name="compare-data-and-ensure-reproducibility-with-snapshots-preview"></a>Gegevens vergelijken en zorg ervoor dat reproduceerbaarheid met momentopnamen (preview)

In dit artikel hebt u meer informatie over het maken en beheren van momentopnamen van uw [Azure Machine Learning gegevenssets](how-to-create-register-datasets.md) (gegevenssets) zodat u kunt vastleggen of gegevens na verloop van tijd vergelijken. Gegevenssets wordt het eenvoudiger voor toegang tot en met uw gegevens in de cloud in verschillende scenario's werken.

**Momentopnamen van de gegevensset** een profiel (samenvattende statistieken) van de gegevens opslaan op het moment dat deze gemaakt. U kunt ook een kopie van de gegevens opslaan in de momentopname voor reproduceerbaarheid.

>[!Important]
> Momentopnamen worden kosten voor opslag. Opslaan van een kopie van gegevens in de momentopname, moet nog meer opslag. Gebruik [ `dataset.delete_snapshot()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#delete-snapshot-snapshot-name-) wanneer ze zijn niet meer nodig zijn.

## <a name="when-to-use-snapshots"></a>Wanneer u momentopnamen

Er zijn drie hoofddoelen voor momentopnamen van:

+ **Validatie van het model**: Vergelijk het gegevensprofiel van verschillende momentopnamen tussen trainingsuitvoeringen of op basis van productiegegevens.

+ **Model reproduceerbaarheid**: De resultaten door het aanroepen van een momentopname met gegevens tijdens de training reproduceren.

+ **Gegevens gedurende een periode bijhouden**: Zie hoe de gegevensset zich heeft ontwikkeld door [profielen vergelijken](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_snapshot.datasetsnapshot?view=azure-ml-py#compare-profiles-rhs-dataset-snapshot--include-columns-none--exclude-columns-none--histogram-compare-method--histogramcomparemethod-wasserstein--0--)
  
## <a name="prerequisites"></a>Vereisten

Voor het maken van momentopnamen van de gegevensset, moet u een geregistreerde Azure Machine Learning-gegevensset. Als u een hebt, raadpleegt u [gegevenssets maken en registreren](how-to-create-register-datasets.md).

## <a name="create-dataset-snapshots"></a>Maken van een momentopname van de gegevensset

Gebruik voor het maken van een momentopname van een gegevensset [ `dataset.create_snapshot()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?#create-snapshot-snapshot-name--compute-target-none--create-data-snapshot-false--target-datastore-none-) van het pakket azureml-gegevenssets van de SDK van Azure Machine Learning.

De momentopname slaat het profiel (samenvattende statistieken) van de gegevens met de meest recente [gegevenssetdefinitie](how-to-manage-dataset-definitions.md) toegepast. De gegevenssetdefinitie van een bevat een overzicht van alle stappen voor gegevenstransformatie gedefinieerd voor de gegevens. Het is een uitstekende manier om uw gegevensvoorbereiding werken reproduceerbare maken.

(Optioneel) u kunt ook een kopie van de gegevens in uw momentopname door toe te voegen `create_data_snapshot = True`.  Deze gegevens kan nuttig zijn voor reproduceerbaarheid.

In dit voorbeeld wordt [criminaliteit voorbeeldgegevens](https://dprepdata.blob.core.windows.net/dataset-sample-files/crime.csv) en een gegevensset met de naam `dataset_crime` gemaakt met behulp van het artikel ['gegevenssets maken en registreren'](how-to-create-register-datasets.md).

```Python
from azureml.core.workspace import Workspace
from azureml.core.dataset import Dataset
from azureml.data.dataset_snapshot import DatasetSnapshot
import datetime

# get existing workspace
workspace = Workspace.from_config()

# get existing, named dataset:
dataset = workspace.datasets['dataset_crime']

# assign name to snapshot
snapshot_name = 'snapshot_' + datetime.datetime.today().strftime('%Y%m%d%H%M%S')

# create snapshot to store profile from a remote environment
# and include copy of data
snapshot = dataset.create_snapshot(snapshot_name = snapshot_name,
                                   compute_target = remote_compute_target,
                                   create_data_snapshot = True)
```

Omdat momentopnamen asynchroon worden gemaakt, gebruikt u de [ `wait_for_completion()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_snapshot.datasetsnapshot?view=azure-ml-py#wait-for-completion-show-output-true--status-update-frequency-10-) methode voor het bewaken van het proces.

```Python
# monitor process every 10 seconds
snapshot.wait_for_completion(show_output=True, status_update_frequency=10)

# return snapshot just created
dataset.get_snapshot(snapshot_name)
```

Uitvoer:

```
Action status: Running
Action status: Running
Action status: Completed


DatasetSnapshot(Name: snapshot_20190411165420,
Dataset Id: 66cd0830-2f72-41e5-b677-d2d953f54821,
Workspace: "your_workspace_name",
Dataset definition version: 1,
Snapshot created date: 2019-05-11 17:24:00+00:00)
```

Gebruik [ `dataset.delete_snapshot()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#delete-snapshot-snapshot-name-) wanneer ze zijn niet meer nodig zijn.

## <a name="find-snapshots"></a>Momentopnamen vinden

Gebruiken om op te halen van een momentopname van een bestaande, [ `get_snapshot()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#get-snapshot-snapshot-name-).

Als u een lijst met uw opgeslagen momentopnamen van een bepaalde gegevensset, gebruikt [ `get_all_snapshots()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#get-all-snapshots--).

```Python
# Get named snapshot for this dataset
dataset.get_snapshot(snapshot_name)

# Get all snapshots for this dataset
dataset.get_all_snapshots()
```

## <a name="get-data-and-profiles-from-snapshots"></a>Ophalen van gegevens en -profielen van momentopnamen

Elke momentopname genereert een profiel van de gegevensset, wat samenvattende statistieken bevat, en biedt de mogelijkheid om op te slaan een kopie van uw gegevens.

### <a name="get-the-data-profile"></a>Het gegevensprofiel ophalen

Gebruik [ `get_profile()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#get-profile-arguments-none--generate-if-not-exist-true--workspace-none--compute-target-none-) voor toegang tot het profiel van uw gegevens.  Training en productie om gegevens te vergelijken, zo kunt u dit profiel.

```Python
snapshot.get_profile()
```

||Type|Min|Max|Count|Ontbrekend aantal|Niet-ontbrekend aantal|Ontbrekend percentage|Aantal fouten|Leeg aantal|0,1%-kwantiel|1%-kwantiel|5%-kwantiel|25%-kwantiel|50%-kwantiel|75%-kwantiel|95%-kwantiel|99%-kwantiel|99,9%-kwantiel|Gemiddelde|Standaardafwijking|Variantie|Asymmetrie|Kurtosis
-|----|---|---|-----|-------------|-----------------|---------------|-----------|-----------|-------------|-----------|-----------|------------|------------|------------|------------|------------|--------------|----|------------------|--------|--------|--------
Id|FieldType.INTEGER|1.04986e+07|1.05351e+07|10.0|0,0|10.0|0,0|0,0|0,0|1.04986e+07|1.04992e+07|1.04986e+07|1.05166e + 07|1.05209e+07|1.05259e+07|1.05351e+07|1.05351e+07|1.05351e+07|1.05195e + 07|12302.7|1.51358e+08|-0.495701|-1.02814
Nummer van de aanvraag|FieldType.STRING|HZ239907|HZ278872|10.0|0,0|10.0|0,0|0,0|0,0||||||||||||||
Date|FieldType.DATE|2016-04-04 23:56:00+00:00|2016-04-15 17:00:00+00:00|10.0|0,0|10.0|0,0|0,0|0,0||||||||||||||
Blok|FieldType.STRING|004XX S KILBOURN OPSLAAN|113XX S PRAIRIE OPSLAAN|10.0|0,0|10.0|0,0|0,0|0,0||||||||||||||
IUCR|FieldType.INTEGER|810|1154|10.0|0,0|10.0|0,0|0,0|0,0|810|850|810|890|1136|1153|1154|1154|1154|1058.5|137.285|18847.2|-0.785501|-1.3543
Het primaire Type|FieldType.STRING|MISLEIDENDE PRAKTIJK|DIEFSTAL|10.0|0,0|10.0|0,0|0,0|0,0||||||||||||||
Description|FieldType.STRING|ONJUISTE SELECTIEVAKJE|VIA $500|10.0|0,0|10.0|0,0|0,0|0,0||||||||||||||
Beschrijving van locatie|FieldType.STRING||SCHOOL, OPENBAAR, HET BOUWEN VAN|10.0|0,0|10.0|0,0|0,0|1.0||||||||||||||
Aanhoudingsbevel|FieldType.BOOLEAN|Onwaar|Onwaar|10.0|0,0|10.0|0,0|0,0|0,0||||||||||||||
Binnenlandse|FieldType.BOOLEAN|Onwaar|Onwaar|10.0|0,0|10.0|0,0|0,0|0,0||||||||||||||
Heartbeat|FieldType.INTEGER|531|2433|10.0|0,0|10.0|0,0|0,0|0,0|531|531|531|614|1318.5|1911|2433|2433|2433|1371.1|692.094|478994|0.105418|-1.60684
Regio|FieldType.INTEGER|5|24|10.0|0,0|10.0|0,0|0,0|0,0|5|5|5|6|13|19|24|24|24|13.5|6.94822|48.2778|0.0930109|-1.62325
Ward|FieldType.INTEGER|1|48|10.0|0,0|10.0|0,0|0,0|0,0|1|5|1|9|22.5|40|48|48|48|24.5|16.2635|264.5|0.173723|-1.51271
Community-gebied|FieldType.INTEGER|4|77|10.0|0,0|10.0|0,0|0,0|0,0|4|8.5|4|24|37.5|71|77|77|77|41.2|26.6366|709.511|0.112157|-1.73379

### <a name="get-the-data-from-the-snapshot"></a>De gegevens ophalen uit de momentopname

Als u een kopie van de gegevens die zijn opgeslagen in een momentopname van een gegevensset, genereert u een pandas DataFrame met de [ `to_pandas_dataframe()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#to-pandas-dataframe--) methode.

Deze methode mislukt als een kopie van de gegevens niet tijdens het maken van de momentopname is aangevraagd.

```Python
snapshot.to_pandas_dataframe().head(3)
```

||Id|Nummer van de aanvraag|Date|Blokkeren|IUCR|Het primaire Type|Description|Beschrijving van locatie|Aanhoudingsbevel|Binnenlandse|...|Ward|Community-gebied|Code van de FBI|X-coördinaat|Y-coördinaat|Jaar|Bijgewerkt op|Breedtegraad|Lengtegraad|Locatie
-|--|-----------|----|-----|----|------------|-----------|--------------------|------|--------|---|----|--------------|--------|------------|------------|----|----------|--------|---------|--------
0|10498554|HZ239907|2016-04-04 23:56:00|007XX E 111TH ST|1153|MISLEIDENDE PRAKTIJK|DIEFSTAL VAN FINANCIËLE IDENTITEITEN VIA $ 300|ANDERE|Onwaar|Onwaar|...|9|50|11|1183356.0|1831503.0|2016|2016-05-11 15:48:00|41.692834|-87.604319|(41.692833841, -87.60431945)
1|10516598|HZ258664|2016-04-15 17:00:00|082XX S MARSHFIELD OPSLAAN|890|DIEFSTAL|VANAF HET OPSTELLEN|WOONPLAATS|Onwaar|Onwaar|...|21|71|6|1166776.0|1850053.0|2016|2016-05-12 15:48:00|41.744107|-87.664494|(41.744106973, -87.664494285)
2|10519196|HZ261252|2016-04-15 10:00:00|104XX S SACRAMENTO OPSLAAN|1154|MISLEIDENDE PRAKTIJK|DIEFSTAL VAN FINANCIËLE IDENTITEITEN $300 EN KLIKT U ONDER|WOONPLAATS|Onwaar|Onwaar|...|19|74|11|NaN|NaN|2016|2016-05-12 15:50:00|NaN|NaN|

## <a name="next-steps"></a>Volgende stappen

* Zie de SDK [overzicht](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) voor ontwerppatronen en voorbeelden van het gebruik.

* Zie voor een voorbeeld van het gebruik van momentopnamen van de gegevensset, de [voorbeeld notitieblokken](https://aka.ms/dataset-tutorial).
