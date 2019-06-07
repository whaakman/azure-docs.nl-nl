---
title: Verken en transformeren van gegevens (klasse gegevensset)
titleSuffix: Azure Machine Learning service
description: Verken gegevens met behulp van de samenvattende statistieken en voorbereiden van gegevens via het opschonen van gegevens, transformatie en feature-engineering
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/23/2019
ms.openlocfilehash: e29ef2616a43223ec582575ca6363f78b26e5f22
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66753056"
---
# <a name="explore-and-prepare-data-with-the-dataset-class-preview"></a>Verken en voorbereiden van gegevens met de gegevensset-klasse (Preview)

Meer informatie over het verkennen en voorbereiden van gegevens met de azureml-gegevenssets-pakket in de [SDK van Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py). De [gegevensset](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py) klasse (preview) kunt u om te verkennen en voorbereiden van uw gegevens door te geven van functies, zoals: steekproeven, samenvattende statistieken en intelligente transformaties. Stappen voor gegevenstransformatie worden opgeslagen in [definities van de gegevensset](how-to-manage-dataset-definitions.md) met de mogelijkheid voor het afhandelen van meerdere grote bestanden van verschillende schema's in een zeer schaalbare manier.

> [!Important]
> Bepaalde klassen gegevensset (preview) zijn afhankelijk van de [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) pakket (GA). Terwijl transformatiefuncties u rechtstreeks met de GA'ed doen kunnen [gegevensvoorbereiding functies](how-to-transform-data.md), wordt aangeraden de gegevensset pakket wrappers die worden beschreven in dit artikel als u een nieuwe oplossing bouwt. Azure Machine Learning-gegevenssets (preview) kunt u niet alleen uw om gegevens te transformeren, maar ook [momentopnamegegevens](how-to-create-dataset-snapshots.md) en op te slaan [versioned gegevenssetdefinities](how-to-manage-dataset-definitions.md). Gegevenssets is de volgende versie van de Data Prep-SDK biedt uitgebreide functionaliteit voor het beheren van gegevenssets in AI-oplossingen.

## <a name="prerequisites"></a>Vereisten

Om te verkennen en uw gegevens voorbereiden, hebt u het volgende nodig:

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Een werkruimte van Azure Machine Learning-service. Zie [maken van een werkruimte van Azure Machine Learning-service](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace).

* De Azure Machine Learning-SDK voor Python (versie 1.0.21 of hoger), waaronder het pakket azureml-gegevenssets. Als u wilt installeren of bijwerken naar de nieuwste versie van de SDK, Zie [installeren of bijwerken van de SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* The Azure Machine Learning Data Prep SDK. Als u wilt installeren of bijwerken naar de nieuwste versie, Zie [installeren of bijwerken van de SDK voor Data Prep](https://docs.microsoft.com/python/api/overview/azure/dataprep/intro?view=azure-dataprep-py#install).

* Download de voorbeeldbestanden te volgen, samen met de voorbeelden: [crime.csv](https://dprepdata.blob.core.windows.net/dataset-sample-files/crime.csv) en [city.json](https://dprepdata.blob.core.windows.net/dataset-sample-files/city.json).

## <a name="sampling"></a>Steekproeven

Nemen een voorbeeld van uw gegevens om op te halen van een eerste inzicht in uw data-architectuur en de inhoud. Op dit moment de [ `sample()` ](https://docs.microsoft.com//python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#sample-sample-strategy--arguments-) methode van de klasse van de gegevensset ondersteunt de populairste N, eenvoudige willekeurige en Stratified steekproeven strategieën.

```Python
from azureml.core.dataset import Dataset
import random

# create an in-memory Dataset from a local file
dataset = Dataset.auto_read_files('./data/crime.csv')

# create seed for Simple Random and Stratified sampling
seed = random.randint(0, 4294967295)
```

### <a name="top-n-sample"></a>Voorbeeld van de bovenste N

De eerste n-records van uw gegevensset zijn voor de Top N-sampling, uw voorbeeld. Dit is handig als u alleen voor een beter beeld van welke uw gegevens records zoeken, zoals of om te zien welke velden worden in uw gegevens.

```Python
top_n_sample_dataset = dataset.sample('top_n', {'n': 5})
top_n_sample_dataset.to_pandas_dataframe()
```

||Id|Nummer van de aanvraag|Date|Blokkeren|IUCR|Het primaire Type|...|
-|--|-----------|----|-----|----|------------|---
0|10498554|HZ239907|4/4/2016 23:56|007XX E 111TH ST|1153|MISLEIDENDE PRAKTIJK|...
1|10516598|HZ258664|4/15/2016 17:00|082XX S MARSHFIELD OPSLAAN|890|DIEFSTAL|...
2|10519196|HZ261252|4/15/2016 10:00|104XX S SACRAMENTO OPSLAAN|1154|MISLEIDENDE PRAKTIJK|...
3|10519591|HZ261534|4/15/2016 9:00|113XX S PRAIRIE OPSLAAN|1120|MISLEIDENDE PRAKTIJK|...
4|10534446|HZ277630|4/15/2016 10:00|055XX N KEDZIE OPSLAAN|890|DIEFSTAL|...

### <a name="simple-random-sample"></a>Eenvoudige steekproef

In eenvoudige steekproeven heeft elk lid van het invullen van de gegevens evenveel kans wordt geselecteerd als onderdeel van het voorbeeld. In de `simple_random` voorbeeld strategie, de records van uw gegevensset zijn geselecteerd op basis van de kans dat is opgegeven en retourneert een gewijzigde gegevensset. De seedparameter is optioneel.

```Python
simple_random_sample_dataset = dataset.sample('simple_random', {'probability':0.3, 'seed': seed})
simple_random_sample_dataset.to_pandas_dataframe()
```

||Id|Nummer van de aanvraag|Date|Blokkeren|IUCR|Het primaire Type|...|
-|--|-----------|----|-----|----|------------|---
0|10516598|HZ258664|4/15/2016 17:00|082XX S MARSHFIELD OPSLAAN|890|DIEFSTAL|...
1|10519196|HZ261252|4/15/2016 10:00|104XX S SACRAMENTO OPSLAAN|1154|MISLEIDENDE PRAKTIJK|...
2|10534446|HZ277630|4/15/2016 10:00|055XX N KEDZIE OPSLAAN|890|DIEFSTAL|...

### <a name="stratified-sample"></a>Toepassing stratificatie voorbeeld

Voorbeelden van toepassing stratificatie ervoor te zorgen dat bepaalde groepen van een populatie worden weergegeven in het voorbeeld. In de `stratified` voorbeeld strategie, de populatie is onderverdeeld in strata of subgroepen, op basis van overeenkomsten, en records willekeurig geselecteerd uit elke strata op basis van de strata gewichten aangegeven door de `fractions` parameter.

In het volgende voorbeeld, we elke record groeperen op de opgegeven kolommen en bevatten deze record op basis van de informatie voor het gewicht van strata X in `fractions`. Als een strata is niet opgegeven of de record kan niet worden gegroepeerd, is het standaardgewicht steekproef 0.

```Python
# take 50% of records with `Primary Type` as `THEFT` and 20% of records with `Primary Type` as `DECEPTIVE PRACTICE` into sample Dataset
fractions = {}
fractions[('THEFT',)] = 0.5
fractions[('DECEPTIVE PRACTICE',)] = 0.2

sample_dataset = dataset.sample('stratified', {'columns': ['Primary Type'], 'fractions': fractions, 'seed': seed})

sample_dataset.to_pandas_dataframe()
```

||Id|Nummer van de aanvraag|Date|Blokkeren|IUCR|Het primaire Type|...|
-|--|-----------|----|-----|----|------------|---
0|10516598|HZ258664|4/15/2016 17:00|082XX S MARSHFIELD OPSLAAN|890|DIEFSTAL|...
1|10534446|HZ277630|4/15/2016 10:00|055XX N KEDZIE OPSLAAN|890|DIEFSTAL|...
2|10535059|HZ278872|4/15/2016 4:30|004XX S KILBOURN OPSLAAN|810|DIEFSTAL|...

## <a name="explore-with-summary-statistics"></a>Verkennen met samenvattende statistieken

 Detecteer afwijkingen, waarden, ontbreken of fout telt met de [ `get_profile()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-profile-arguments-none--generate-if-not-exist-true--workspace-none--compute-target-none-) methode. Deze functie haalt het profiel en samenvattende statistieken over uw gegevens, die op zijn beurt helpt bepalen welke bewerkingen voor het voorbereiden van de benodigde gegevens om toe te passen.

```Python
dataset.get_profile()
```

||Type|Min.|Max.|Count|Ontbrekend aantal|Niet-ontbrekend aantal|Ontbrekend percentage|Aantal fouten|Leeg aantal|0,1%-kwantiel|1%-kwantiel|5%-kwantiel|25%-kwantiel|50%-kwantiel|75%-kwantiel|95%-kwantiel|99%-kwantiel|99,9%-kwantiel|Gemiddelde|Standaardafwijking|Variantie|Asymmetrie|Kurtosis
-|----|---|---|-----|-------------|-----------------|---------------|-----------|-----------|-------------|-----------|-----------|------------|------------|------------|------------|------------|--------------|----|------------------|--------|--------|--------
Id|FieldType.INTEGER|1.04986e+07|1.05351e+07|10.0|0,0|10.0|0,0|0,0|0,0|1.04986e+07|1.04992e+07|1.04986e+07|1.05166e + 07|1.05209e+07|1.05259e+07|1.05351e+07|1.05351e+07|1.05351e+07|1.05195e + 07|12302.7|1.51358e+08|-0.495701|-1.02814
Nummer van de aanvraag|FieldType.STRING|HZ239907|HZ278872|10.0|0,0|10.0|0,0|0,0|0,0||||||||||||||
Date|FieldType.DATE|2016-04-04 23:56:00+00:00|2016-04-15 17:00:00+00:00|10.0|0,0|10.0|0,0|0,0|0,0||||||||||||||
Blokkeren|FieldType.STRING|004XX S KILBOURN OPSLAAN|113XX S PRAIRIE OPSLAAN|10.0|0,0|10.0|0,0|0,0|0,0||||||||||||||
IUCR|FieldType.INTEGER|810|1154|10.0|0,0|10.0|0,0|0,0|0,0|810|850|810|890|1136|1153|1154|1154|1154|1058.5|137.285|18847.2|-0.785501|-1.3543
Het primaire Type|FieldType.STRING|MISLEIDENDE PRAKTIJK|DIEFSTAL|10.0|0,0|10.0|0,0|0,0|0,0||||||||||||||
Description|FieldType.STRING|ONJUISTE SELECTIEVAKJE|VIA $500|10.0|0,0|10.0|0,0|0,0|0,0||||||||||||||
Beschrijving van locatie|FieldType.STRING||SCHOOL, OPENBAAR, HET BOUWEN VAN|10.0|0,0|10.0|0,0|0,0|1.0||||||||||||||
Aanhoudingsbevel|FieldType.BOOLEAN|False|False|10.0|0,0|10.0|0,0|0,0|0,0||||||||||||||
Binnenlandse|FieldType.BOOLEAN|False|False|10.0|0,0|10.0|0,0|0,0|0,0||||||||||||||
Heartbeat|FieldType.INTEGER|531|2433|10.0|0,0|10.0|0,0|0,0|0,0|531|531|531|614|1318.5|1911|2433|2433|2433|1371.1|692.094|478994|0.105418|-1.60684
District|FieldType.INTEGER|5|24|10.0|0,0|10.0|0,0|0,0|0,0|5|5|5|6|13|19|24|24|24|13.5|6.94822|48.2778|0.0930109|-1.62325
Ward|FieldType.INTEGER|1|48|10.0|0,0|10.0|0,0|0,0|0,0|1|5|1|9|22.5|40|48|48|48|24.5|16.2635|264.5|0.173723|-1.51271
Community-gebied|FieldType.INTEGER|4|77|10.0|0,0|10.0|0,0|0,0|0,0|4|8.5|4|24|37.5|71|77|77|77|41.2|26.6366|709.511|0.112157|-1.73379
Code van de FBI|FieldType.INTEGER|6|11|10.0|0,0|10.0|0,0|0,0|0,0|6|6|6|6|11|11|11|11|11|9.4|2.36643|5.6|-0.702685|-1.59582
X-coördinaat|FieldType.INTEGER|1.16309e+06|1.18336e+06|10.0|7.0|3.0|0.7|0,0|0,0|1.16309e+06|1.16309e+06|1.16309e+06|1.16401e+06|1.16678e+06|1.17921e+06|1.18336e+06|1.18336e+06|1.18336e+06|1.17108e + 06|10793.5|1.165e+08|0.335126|-2.33333
Y-coördinaat|FieldType.INTEGER|1.8315e+06|1.908e+06|10.0|7.0|3.0|0.7|0,0|0,0|1.8315e+06|1.8315e+06|1.8315e+06|1.83614e+06|1.85005e + 06|1.89352e+06|1.908e+06|1.908e+06|1.908e+06|1.86319e+06|39905.2|1.59243e+09|0.293465|-2.33333
Jaar|FieldType.INTEGER|2016|2016|10.0|0,0|10.0|0,0|0,0|0,0|2016|2016|2016|2016|2016|2016|2016|2016|2016|2016|0|0|NaN|NaN
Bijgewerkt op|FieldType.DATE|2016-05-11 15:48:00+00:00|2016-05-27 15:45:00+00:00|10.0|0,0|10.0|0,0|0,0|0,0||||||||||||||
Breedtegraad|FieldType.DECIMAL|41.6928|41.9032|10.0|7.0|3.0|0.7|0,0|0,0|41.6928|41.6928|41.6928|41.7057|41.7441|41.8634|41.9032|41.9032|41.9032|41.78|0.109695|0.012033|0.292478|-2.33333
Lengtegraad|FieldType.DECIMAL|-87.6764|-87.6043|10.0|7.0|3.0|0.7|0,0|0,0|-87.6764|-87.6764|-87.6764|-87.6734|-87.6645|-87.6194|-87.6043|-87.6043|-87.6043|-87.6484|0.0386264|0.001492|0.344429|-2.33333
Locatie|FieldType.STRING||(41.903206037, -87.676361925)|10.0|0,0|10.0|0,0|0,0|7.0||||||||||||||

## <a name="impute-missing-values"></a>Ontbrekende waarden worden toegerekend

In gegevenssets, null-waarden, van NaN en waarden die geen inhoud bevatten die worden beschouwd als ontbrekende waarden. Dit kunnen invloed hebben op de prestaties van uw machine learning-modellen of leiden tot een ongeldige conclusies. Toerekening is een veelgebruikte manier om de ontbrekende waarden en is handig wanneer u een hoog percentage van ontbrekende waarde records die u kunt gewoon niet verwijderen.

Van het profiel voor een gegevensset gegenereerd in de vorige sectie, zien we dat `Latitude` en `Longitude` kolommen hebben een hoog percentage van de ontbrekende waarden. In dit voorbeeld we berekent het gemiddelde en worden toegerekend ontbrekende waarden voor deze twee kolommen.

Haal eerst de meest recente definitie van de gegevensset met [ `get_definition()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-definition-version-id-none-) en de gegevens met verkleinen [ `keep_columns()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow#keep-columns-columns--multicolumnselection--validate-column-exists--bool---false-----azureml-dataprep-api-dataflow-dataflow), zodat we alleen de kolommen die we adres willen weergeven.

```Python
from azureml.core.dataset import Dataset
import azureml.dataprep as dprep

# get the latest definition of Dataset
ds_def = dataset.get_definition()

# keep useful columns for this example
ds_def = ds_def.keep_columns(['ID', 'Arrest', 'Latitude', 'Longitude'])
ds_def.head(3)
```

||Id|Aanhoudingsbevel| Breedtegraad|Lengtegraad|
-|---------|-----|---------|----------|
|0|10498554|False|41.692834|-87.604319|
|1|10516598|False| 41.744107 |-87.664494|
|2|10519196|False| NaN|NaN|

Controleer vervolgens de `MEAN` waarde van de breedtegraad kolom met behulp van de [ `summarize()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#summarize-summary-columns--typing-union-typing-list-azureml-dataprep-api-dataflow-summarycolumnsvalue---nonetype----none--group-by-columns--typing-union-typing-list-str---nonetype----none--join-back--bool---false--join-back-columns-prefix--typing-union-str--nonetype----none-----azureml-dataprep-api-dataflow-dataflow) functie. Deze functie accepteert een matrix van kolommen in de `group_by_columns` parameter opgeven voor het aggregatieniveau van. De `summary_columns` parameter accepteert de `SummaryColumnsValue` functie, die Hiermee geeft u de huidige kolomnaam, de nieuwe naam voor het berekende veld en de `SummaryFunction` om uit te voeren.

```Python
lat_mean = ds_def.summarize(group_by_columns = ['Arrest'],
                            summary_columns = [dprep.SummaryColumnsValue(column_id = 'Latitude',
                                                                         summary_column_name = 'Latitude_MEAN',
                                                                         summary_function = dprep.SummaryFunction.MEAN)])
lat_mean = lat_mean.filter(lat_mean['Arrest'] == False)
lat_mean.head(1)
```

||Aanhoudingsbevel|Latitude_MEAN|
--|-----|--------|
|0|False|41.780049|

Zodra we controleren of de waarden die moeten worden toegerekend, gebruikt u [ `ImputeMissingValuesBuilder` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputemissingvaluesbuilder?view=azure-dataprep-py) voor meer informatie over een vaste-expressie die de kolommen met een van beide een berekende imputes `MIN`, `MAX`, `MEAN` waarde, of een `CUSTOM` waarde. Wanneer `group_by_columns` is opgegeven, worden ontbrekende waarden worden toegeschreven per groep met `MIN`, `MAX`, en `MEAN` berekend per groep.

De [ `ImputeColumnArguments` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputecolumnarguments?view=azure-dataprep-pyfunction) accepteert een column_id-tekenreeks en een `ReplaceValueFunction` om op te geven van het type impute. Voor de ontbrekende Lengtegraadwaarde, moet u het rekenen met-87 op basis van externe kennis.

```Python
# impute with MEAN
impute_mean = dprep.ImputeColumnArguments(column_id = 'Latitude',
                                          impute_function = dprep.ReplaceValueFunction.MEAN)

# impute with custom value -87
impute_custom = dprep.ImputeColumnArguments(column_id='Longitude',
                                            custom_impute_value=-87)
```

Rekenen stappen kunnen een keten worden samengevoegd in een `ImputeMissingValuesBuilder` object met de [ `Builders` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.builders?view=azure-dataprep-py) klasse functie [ `impute_missing_values()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.builders?view=azure-dataprep-py#impute-missing-values-impute-columns--typing-list-azureml-dataprep-api-builders-imputecolumnarguments----none--group-by-columns--typing-union-typing-list-str---nonetype----none-----azureml-dataprep-api-builders-imputemissingvaluesbuilder). De `impute_columns` parameter accepteert een matrix van `ImputeColumnArguments` objecten.

```Python
# get instance of ImputeMissingValuesBuilder
impute_builder = ds_def.builders.impute_missing_values(impute_columns=[impute_mean, impute_custom],
                                                   group_by_columns=['Arrest'])
```

Roep de [ `learn()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputemissingvaluesbuilder?view=azure-dataprep-py#learn------none) functie voor het opslaan van de stappen impute, en deze toepassen op een gegevensstroom object met [ `to_dataflow()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputemissingvaluesbuilder?view=azure-dataprep-py#to-dataflow------azureml-dataprep-api-dataflow-dataflow).

```Python
impute_builder.learn()
ds_def = impute_builder.to_dataflow()
ds_def.head(3)
```

Zoals u in de volgende uitvoertabel, de ontbrekende breedtegraad is toegerekende met de `MEAN` waarde van `Arrest==False` groep en de ontbrekende lengtegraad is toegerekende met-87.

||Id|Aanhoudingsbevel|Breedtegraad|Lengtegraad
-|---------|-----|---------|----------
0|10498554|False|41.692834|-87.604319
1|10516598|False|41.744107|-87.664494
2|10519196|False|41.780049|-87.000000

Bijwerken van de definitie van de gegevensset, [ `update_definition()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#update-definition-definition--definition-update-message-) te houden van de transformatiestappen uitgevoerd.

```Python
dataset = dataset.update_definition(ds_def, 'Impute Missing')
dataset.head(3)
```

||Id|Aanhoudingsbevel|Breedtegraad|Lengtegraad
-|---------|-----|---------|----------
0|10498554|False|41.692834|-87.604319
1|10516598|False|41.744107|-87.664494
2|10519196|False|41.780049|-87.000000

## <a name="create-assertion-rules"></a>Bewering regels maken

Vaak de gegevens met werken tijdens het opschonen en voorbereiden van gegevens is slechts een subset van de totale hoeveelheid gegevens die we nodig hebben voor productie. Als gevolg hiervan enkele van de veronderstellingen die we als onderdeel van onze reinigen maken kan het zo zijn ingesteld op false. Bijvoorbeeld, in een gegevensset die voortdurend wordt bijgewerkt, kan bevatten een kolom die oorspronkelijk alleen uit cijfers binnen een bepaald bereik een groot aantal waarden in latere uitvoeringen. Deze fouten leiden vaak tot verbroken pijplijnen of beschadigde gegevens.

Gegevenssets ondersteuning voor het maken van asserties op gegevens die worden geëvalueerd omdat de pijplijn wordt uitgevoerd. Deze asserties zodat we verifiëren dat onze aannames over de gegevens blijven wel correct en, indien dit niet het geval is, voor het afhandelen van fouten dienovereenkomstig.

Bijvoorbeeld, als u wilt beperken `Latitude` en `Longitude` waarden in uw gegevensset op specifieke numerieke bereiken, de [ `assert_value()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#assert-value-columns--multicolumnselection--expression--azureml-dataprep-api-expressions-expression--policy--azureml-dataprep-api-engineapi-typedefinitions-assertpolicy----assertpolicy-errorvalue--1---error-code--str----assertionfailed------azureml-dataprep-api-dataflow-dataflow) methode zorgt ervoor dat dit is altijd het geval is.

```Python
from azureml.dataprep import value
from azureml.core.dataset import Dataset

# get the latest definition of the Dataset
ds_def = dataset.get_definition()

# set assertion rules for `Latitude` and `Longitude` columns
ds_def = ds_def.assert_value('Latitude', (value <= 90) & (value >= -90), error_code='InvalidLatitude')
ds_def = ds_def.assert_value('Longitude', (value <= 180) & (value >= -87), error_code='InvalidLongitude')

ds_def.get_profile()
```

||Type|Min.|Max.|Count|Ontbrekend aantal|Niet-ontbrekend aantal|Ontbrekend percentage|Aantal fouten|Leeg aantal|0,1%-kwantiel|1%-kwantiel|5%-kwantiel|25%-kwantiel|50%-kwantiel|75%-kwantiel|95%-kwantiel|99%-kwantiel|99,9%-kwantiel|Gemiddelde|Standaardafwijking|Variantie|Asymmetrie|Kurtosis
-|----|---|---|-----|-------------|-----------------|---------------|-----------|-----------|-------------|-----------|-----------|------------|------------|------------|------------|------------|--------------|----|------------------|--------|--------|--------
Id|FieldType.INTEGER|1.04986e+07|1.05351e+07|10.0|0,0|10.0|0,0|0,0|0,0|1.04986e+07|1.04992e+07|1.04986e+07|1.05166e + 07|1.05209e+07|1.05259e+07|1.05351e+07|1.05351e+07|1.05351e+07|1.05195e + 07|12302.7|1.51358e+08|-0.495701|-1.02814
Aanhoudingsbevel|FieldType.BOOLEAN|False|False|10.0|0,0|10.0|0,0|0,0|0,0||||||||||||||
Breedtegraad|FieldType.DECIMAL|41.6928|41.9032|10.0|0,0|10.0|0,0|0,0|0,0|41.6928|41.7185|41.6928|41.78|41.78|41.78|41.9032|41.9032|41.9032|41.78|0.0517107|0.002674|0.837593|1,05
Lengtegraad|FieldType.INTEGER|-87|-87|10.0|0,0|10.0|0,0|3.0|0,0|-87|-87|-87|-87|-87|-87|-87|-87|-87|-87|0|0|NaN|NaN

Van het profiel, ziet u dat de `Error Count` voor de `Longitude` kolom is 3. De volgende code filters van de gegevensset, worden opgehaald van de fout en krijgt te zien welke waarde zorgt ervoor dat de verklaring mislukken. Hier past u uw code en schoon de gegevens op de juiste wijze.

```Python
from azureml.dataprep import col

ds_error = ds_def.filter(col('Longitude').is_error())
error = ds_error.head(10)['Longitude'][0]

print(error.originalValue)
```

    -87.60431945

## <a name="derive-columns-by-example"></a>Kolommen afleiden per voorbeeld

Een van de meer geavanceerde hulpprogramma's voor gegevenssets is de mogelijkheid voor het afleiden van kolommen met behulp van voorbeelden van de gewenste resultaten. Hiermee kunt u de SDK een voorbeeld geven, zodat deze code voor het bereiken van de beoogde transformaties kunt genereren.

```Python
from azureml.core.dataset import Dataset

# create an in-memory Dataset from a local file
dataset = Dataset.auto_read_files('./data/crime.csv')
dataset.head(3)
```

||Id|Nummer van de aanvraag|Date|Blokkeren|...|
-|---------|-----|---------|----|---
0|10498554|HZ239907|2016-04-04 23:56:00|007XX E 111TH ST|...
1|10516598|HZ258664|2016-04-15 17:00:00|082XX S MARSHFIELD OPSLAAN|...
2|10519196|HZ261252|2016-04-15 10:00:00|104XX S SACRAMENTO OPSLAAN|...

Stel dat u wilt transformeren van de datum en tijd-indeling naar ' 2016-04-04 10 PM - 12 AM'. In de [ `derive_column_by_example()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#derive-column-by-example-source-columns--sourcecolumns--new-column-name--str--example-data--exampledata-----azureml-dataprep-api-dataflow-dataflow) argument, vindt u voorbeelden van de gewenste uitvoer in de `example_data` parameter in deze indeling: *(oorspronkelijke uitvoer, gewenste uitvoer)* .

De volgende code ziet u twee voorbeelden van de gewenste uitvoer. (' 2016-04-04 23:56:00 ', ' 2016-04-04 10 uur-12 AM ") en (' 2016-04-15 17:00:00 ', ' 2016-04-15 16: 00 - 18: 00 uur ')

```Python
ds_def = dataset.get_definition()
ds_def = ds_def.derive_column_by_example(
        source_columns = "Date",
        new_column_name = "Date_Time_Range",
        example_data = [("2016-04-04 23:56:00", "2016-04-04 10PM-12AM"), ("2016-04-15 17:00:00", "2016-04-15 4PM-6PM")]
    )
ds_def.keep_columns(['ID','Date','Date_Time_Range']).head(3)
```

In de volgende tabel ziet u dat een nieuwe kolom Date_Time_Range records in de indeling die is opgegeven bevat.

||Id|Date|Date_Time_Range
-|--------|-----|----
0|10498554|2016-04-04 23:56:00|2016-04-04-10 PM-12 AM
1|10516598|2016-04-15 17:00:00|2016-04-15 16: 00 - 18: 00 UUR
2|10519196|2016-04-15 10:00:00|2016-04-15: 10: 00:00 UUR

```Python
# update Dataset definition to keep the transformation steps performed.
dataset = dataset.update_definition(ds_def, 'Derive Date_Time_Range')
```

## <a name="fuzzy-groupings"></a>Fuzzy groeperingen

U kunt variaties faciliteren voor de spellingcontrole, hoofdletters of afkortingen van dezelfde entiteiten tegenkomen wanneer u gegevens uit verschillende bronnen verzamelen. Automatisch te standaardiseren en deze varianten afstemmen met de SDK fuzzy groepering of tekst clustering, functionaliteit.

Bijvoorbeeld, de kolom `inspections.business.city` bevat verschillende vormen van de stad naam "San Francisco'.

```Python
from azureml.core.dataset import Dataset

# create an in-memory Dataset from a local json file
dataset = Dataset.auto_read_files('./data/city.json')
dataset.head(5)
```

||inspections.business.business_id|inspections.business_name|inspections.business.address|inspections.Business.City|...|
-|-----|-------------------------|------------|--|---
0|16162|De Indiase Foods Quick-N-Ezee|3861 24th St|SF|...
1|67565|King van Thais noedels Cafe|1541 TARAVAL St|SAN FRANCISCO|...
2|67565|King van Thais noedels Cafe|1541 TARAVAL St|SAN FRANCISCO|...
3|68701|Grindz|832 clement St|SF|...
4|69186|Premier Catering & gebeurtenissen, Inc.|1255 22e St|S.F.|...

We gebruiken de [ `fuzzy_group_column()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#fuzzy-group-column-source-column--str--new-column-name--str--similarity-threshold--float---0-8--similarity-score-column-name--str---none-----azureml-dataprep-api-dataflow-dataflow) methode voor het toevoegen van een kolom met de automatisch gedetecteerde canonieke vorm van 'San Francisco'. De argumenten `source_column` en `new_column_name` zijn vereist. U hebt ook de optie voor:

* Maak een nieuwe kolom `similarity_score_column_name`, die de score overeenkomsten tussen elk paar van de oorspronkelijke en canonieke waarden bevat.

* Geef een `similarity_threshold`, dit is de minimale vergelijkbaarheid score voor de waarden die moeten worden gegroepeerd.

```Python
# get the latest Dataset definition
ds_def = dataset.get_definition()
ds_def = ds_def.fuzzy_group_column(source_column='inspections.business.city',
                                       new_column_name='city_grouped',
                                       similarity_threshold=0.8,
                                       similarity_score_column_name='similarity_score')
ds_def.head(5)
```

||inspections.business.business_id|inspections.business_name|inspections.business.address|inspections.Business.City|city_grouped|similarity_score|...|
-|-----|-------------------------|------------|--|---|---|---
0|16162|De Indiase Foods Quick-N-Ezee|3861 24th St|SF|San Francisco|0.814806|...
1|67565|King van Thais noedels Cafe|1541 TARAVAL St|SAN FRANCISCO|San Francisco|1.000000|...
2|67565|King van Thais noedels Cafe|1541 TARAVAL St|SAN FRANCISCO|San Francisco|1.000000|...
3|68701|Grindz|832 clement St|SF|San Francisco|0.814806|...
4|69186|Premier Catering & gebeurtenissen, Inc.|1255 22e St|S.F.|San Francisco|0.814806|...

In het definitie van de resulterende gegevensset zijn de verschillende variaties van de 'San Francisco' in de gegevens die op dezelfde tekenreeks, "San Francisco" genormaliseerd.

Sla deze stap fuzzy groepering in de definitie van de meest recente met `update_definition()`.

```Python
dataset = dataset.update_definition(ds_def, 'fuzzy grouping')
```

## <a name="next-steps"></a>Volgende stappen

* [Beheren van de levenscyclus van definities van de gegevensset](how-to-manage-dataset-definitions.md).

* Zie de geautomatiseerde machine learning [zelfstudie](tutorial-auto-train-models.md) voor een voorbeeld van een regressie model.

* Zie de SDK [overzicht](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) voor ontwerppatronen en voorbeelden van het gebruik.

* Zie voor een voorbeeld van het gebruik van gegevenssets, de [voorbeeld notitieblokken](https://aka.ms/dataset-tutorial).
