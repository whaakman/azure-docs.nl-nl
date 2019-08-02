---
title: Gegevens verkennen en transformeren (klasse dataset)
titleSuffix: Azure Machine Learning service
description: Verken gegevens met behulp van samenvattings statistieken en bereid gegevens voor op basis van gegevens reiniging, trans formatie en functie techniek
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/23/2019
ms.openlocfilehash: f680a1cb15edf0141897c74da3b7c7afa01acae0
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68699120"
---
# <a name="explore-and-prepare-data-with-the-dataset-class-preview"></a>Gegevens verkennen en voorbereiden met de klasse DataSet (preview)

Meer informatie over het verkennen en voorbereiden van gegevens met het pakket voor azureml-gegevens sets in de [Azure machine learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py). Met de klasse [DataSet](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py) (preview) kunt u uw gegevens verkennen en voorbereiden door functies te bieden zoals: steek proeven, samenvattings statistieken en intelligente trans formaties. Transformatie stappen worden opgeslagen in [gegevensset-definities](how-to-manage-dataset-definitions.md) met de mogelijkheid om meerdere grote bestanden van verschillende schema's op een zeer schaal bare manier te verwerken.

> [!Important]
> Sommige klassen van gegevensset (preview) hebben afhankelijkheden voor het [dataprep-](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) pakket (ga). Hoewel transformatie functies direct kunnen worden uitgevoerd met de GA'ed- [functies voor gegevens](how-to-transform-data.md)voorbereiding, raden we u aan de wrapper-pakketten in dit artikel te maken als u een nieuwe oplossing bouwt. Met Azure Machine Learning gegevens sets (preview) kunt u niet alleen uw gegevens transformeren, maar ook [momentopname gegevens](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_snapshot.datasetsnapshot?view=azure-ml-py) en [gegevensset-definities](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset?view=azure-ml-py)van de versie opgeslagen. Gegevens sets is de volgende versie van de data prep SDK, die uitgebreide functionaliteit biedt voor het beheren van gegevens sets in AI-oplossingen.

## <a name="prerequisites"></a>Vereisten

Om uw gegevens te verkennen en voor te bereiden, hebt u het volgende nodig:

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Een werkruimte van Azure Machine Learning-service. Zie [een Azure machine learning service-werk ruimte maken](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace).

* De Azure Machine Learning SDK voor python (versie 1.0.21 of hoger), die het pakket met de azureml-gegevens sets bevat. Zie [de SDK installeren of bijwerken](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)om de nieuwste versie van de SDK te installeren of bij te werken.

* De Azure Machine Learning data prep SDK. Zie [de data prep-SDK installeren of bijwerken](https://docs.microsoft.com/python/api/overview/azure/dataprep/intro?view=azure-dataprep-py#install)om de nieuwste versie te installeren of bij te werken.

* Down load de voorbeeld bestanden om samen te gaan met de voor beelden: [criminaliteit. CSV](https://dprepdata.blob.core.windows.net/dataset-sample-files/crime.csv) en [City. json](https://dprepdata.blob.core.windows.net/dataset-sample-files/city.json).

## <a name="sampling"></a>Steekproeven

Bekijk een voor beeld van uw gegevens, zodat u een eerste uitleg krijgt van uw gegevens architectuur en-inhoud. Op dit moment ondersteunt de [`sample()`](https://docs.microsoft.com//python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#sample-sample-strategy--arguments-) methode van de klasse dataset de bemonsterings strategieën top N, eenvoudige wille keurig en stratified.

```Python
from azureml.core.dataset import Dataset
import random

# create an in-memory Dataset from a local file
dataset = Dataset.auto_read_files('./data/crime.csv')

# create seed for Simple Random and Stratified sampling
seed = random.randint(0, 4294967295)
```

### <a name="top-n-sample"></a>Eerste N-voor beeld

De eerste n records van uw gegevensset zijn uw voor beeld. Dit is handig als u alleen probeert een idee te krijgen van de manier waarop uw gegevens records eruit zien of als u wilt zien welke velden er in uw gegevens staan.

```Python
top_n_sample_dataset = dataset.sample('top_n', {'n': 5})
top_n_sample_dataset.to_pandas_dataframe()
```

||id|Nummer van de aanvraag|Date|Blokkeren|IUCR|Het primaire Type|...|
-|--|-----------|----|-----|----|------------|---
0|10498554|HZ239907|4/4/2016 23:56|007XX E 111TH ST|1153|MISLEIDENDE PROCEDURE|...
1|10516598|HZ258664|4/15/2016 17:00|082XX S MARSHFIELD LAAN|890|DIEFSTAL|...
2|10519196|HZ261252|4/15/2016 10:00|104XX S SACRAMENTO LAAN|1154|MISLEIDENDE PROCEDURE|...
3|10519591|HZ261534|4/15/2016 9:00|113XX S PRAIRIE LAAN|1120|MISLEIDENDE PROCEDURE|...
4|10534446|HZ277630|4/15/2016 10:00|055XX N KEDZIE LAAN|890|DIEFSTAL|...

### <a name="simple-random-sample"></a>Eenvoudig wille keurig voor beeld

Bij eenvoudige wille keurige steek proeven is elk lid van de gegevens populatie gelijk aan het selecteren van een deel van het voor beeld. In de `simple_random` voorbeeld strategie worden de records uit uw gegevensset geselecteerd op basis van de opgegeven kans en wordt een gewijzigde gegevensset geretourneerd. De Seed-para meter is optioneel.

```Python
simple_random_sample_dataset = dataset.sample('simple_random', {'probability':0.3, 'seed': seed})
simple_random_sample_dataset.to_pandas_dataframe()
```

||id|Nummer van de aanvraag|Date|Blokkeren|IUCR|Het primaire Type|...|
-|--|-----------|----|-----|----|------------|---
0|10516598|HZ258664|4/15/2016 17:00|082XX S MARSHFIELD LAAN|890|DIEFSTAL|...
1|10519196|HZ261252|4/15/2016 10:00|104XX S SACRAMENTO LAAN|1154|MISLEIDENDE PROCEDURE|...
2|10534446|HZ277630|4/15/2016 10:00|055XX N KEDZIE LAAN|890|DIEFSTAL|...

### <a name="stratified-sample"></a>Stratified-voor beeld

Met stratified-voor beelden wordt ervoor gezorgd dat bepaalde groepen van een populatie in het voor beeld worden weer gegeven. In de `stratified` voorbeeld strategie wordt de populatie onderverdeeld in Strata, of subgroepen, op basis van overeenkomsten, en worden records wille keurig geselecteerd uit elke Strata, afhankelijk van de Strata-gewichten `fractions` die worden aangegeven door de para meter.

In het volgende voor beeld groeperen we elke record op de opgegeven kolommen en bevatten ze een record op basis van de Strata X-informatie `fractions`in. Als er geen Strata is opgegeven of als de record niet kan worden gegroepeerd, is het standaard gewicht voor het voor beeld 0.

```Python
# take 50% of records with `Primary Type` as `THEFT` and 20% of records with `Primary Type` as `DECEPTIVE PRACTICE` into sample Dataset
fractions = {}
fractions[('THEFT',)] = 0.5
fractions[('DECEPTIVE PRACTICE',)] = 0.2

sample_dataset = dataset.sample('stratified', {'columns': ['Primary Type'], 'fractions': fractions, 'seed': seed})

sample_dataset.to_pandas_dataframe()
```

||id|Nummer van de aanvraag|Date|Blokkeren|IUCR|Het primaire Type|...|
-|--|-----------|----|-----|----|------------|---
0|10516598|HZ258664|4/15/2016 17:00|082XX S MARSHFIELD LAAN|890|DIEFSTAL|...
1|10534446|HZ277630|4/15/2016 10:00|055XX N KEDZIE LAAN|890|DIEFSTAL|...
2|10535059|HZ278872|4/15/2016 4:30|004XX S KILBOURN LAAN|810|DIEFSTAL|...

## <a name="explore-with-summary-statistics"></a>Verkennen met samenvattings statistieken

 Detecteer afwijkingen, ontbrekende waarden of fout aantallen met de [`get_profile()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-profile-arguments-none--generate-if-not-exist-true--workspace-none--compute-target-none-) methode. Deze functie haalt het profiel en de samenvattings statistieken op van uw gegevens, die op zijn beurt de benodigde bewerkingen voor gegevens voorbereiding kunnen bepalen.

```Python
dataset.get_profile()
```

||type|Min.|Max.|Count|Ontbrekend aantal|Niet-ontbrekend aantal|Ontbrekend percentage|Aantal fouten|Leeg aantal|0,1%-kwantiel|1%-kwantiel|5%-kwantiel|25%-kwantiel|50%-kwantiel|75%-kwantiel|95%-kwantiel|99%-kwantiel|99,9%-kwantiel|Gemiddelde|Standaardafwijking|Variantie|Asymmetrie|Kurtosis
-|----|---|---|-----|-------------|-----------------|---------------|-----------|-----------|-------------|-----------|-----------|------------|------------|------------|------------|------------|--------------|----|------------------|--------|--------|--------
id|Field Type. INTEGER|1.04986e+07|1.05351e+07|10,0|0.0|10,0|0.0|0,0|0.0|1.04986e+07|1.04992e+07|1.04986e+07|1.05166 e + 07|1.05209 e + 07|1.05259 e + 07|1.05351e+07|1.05351e+07|1.05351e+07|1.05195 e + 07|12302,7|1.51358e+08|-0,495701|-1,02814
Nummer van de aanvraag|FieldType.STRING|HZ239907|HZ278872|10,0|0.0|10,0|0.0|0,0|0.0||||||||||||||
Date|FieldType.DATE|2016-04-04 23:56:00 + 00:00|2016-04-15 17:00:00 + 00:00|10,0|0.0|10,0|0.0|0,0|0.0||||||||||||||
Blokkeren|FieldType.STRING|004XX S KILBOURN LAAN|113XX S PRAIRIE LAAN|10,0|0.0|10,0|0.0|0,0|0.0||||||||||||||
IUCR|Field Type. INTEGER|810|1154|10,0|0.0|10,0|0.0|0,0|0.0|810|850|810|890|1136|1153|1154|1154|1154|1058,5|137,285|18847.2|-0,785501|-1,3543
Het primaire Type|FieldType.STRING|MISLEIDENDE PROCEDURE|DIEFSTAL|10,0|0.0|10,0|0.0|0,0|0.0||||||||||||||
Description|FieldType.STRING|LOZE CONTROLE|MEER DAN $500|10,0|0.0|10,0|0.0|0,0|0.0||||||||||||||
Beschrijving van locatie|FieldType.STRING||SCHOOL, OPENBAAR, GEBOUW|10,0|0.0|10,0|0.0|0.0|1.0||||||||||||||
Aanhoudingsbevel|FieldType.BOOLEAN|False|False|10,0|0.0|10,0|0.0|0,0|0.0||||||||||||||
Binnenlandse|FieldType.BOOLEAN|False|False|10,0|0.0|10,0|0.0|0,0|0.0||||||||||||||
Maat|Field Type. INTEGER|531|2433|10,0|0.0|10,0|0.0|0,0|0.0|531|531|531|614|1318,5|1911|2433|2433|2433|1371.1|692,094|478994|0,105418|-1,60684
Regio|Field Type. INTEGER|5|24|10,0|0.0|10,0|0.0|0,0|0.0|5|5|5|6|13|19|24|24|24|13.5|6.94822|48,2778|0,0930109|-1,62325
Ward|Field Type. INTEGER|1|48|10,0|0.0|10,0|0.0|0,0|0,0|1|5|1|9|22.5|40|48|48|48|24.5|16,2635|264,5|0,173723|-1,51271
Community-gebied|Field Type. INTEGER|4|77|10,0|0.0|10,0|0.0|0,0|0.0|4|8.5|4|24|37,5|71|77|77|77|41,2|26,6366|709,511|0,112157|-1,73379
Code van de FBI|Field Type. INTEGER|6|11|10,0|0.0|10,0|0.0|0,0|0.0|6|6|6|6|11|11|11|11|11|9.4|2,36643|5.6|-0,702685|-1,59582
X-coördinaat|Field Type. INTEGER|1.16309e+06|1.18336e+06|10,0|7.0|3.0|0,7|0.0|0.0|1.16309e+06|1.16309e+06|1.16309e+06|1.16401e+06|1.16678e+06|1.17921e+06|1.18336e+06|1.18336e+06|1.18336e+06|1.17108 e + 06|10793,5|1.165 e + 08|0,335126|-2,33333
Y-coördinaat|Field Type. INTEGER|1.8315 e + 06|1.908e+06|10,0|7.0|3.0|0,7|0.0|0.0|1.8315 e + 06|1.8315 e + 06|1.8315 e + 06|1.83614e+06|1.85005 e + 06|1.89352e+06|1.908e+06|1.908e+06|1.908e+06|1.86319e+06|39905,2|1.59243 e + 09|0,293465|-2,33333
Jaar|Field Type. INTEGER|2016|2016|10,0|0.0|10,0|0.0|0,0|0.0|2016|2016|2016|2016|2016|2016|2016|2016|2016|2016|0|0|NaN|NaN
Bijgewerkt op|FieldType.DATE|2016-05-11 15:48:00 + 00:00|2016-05-27 15:45:00 + 00:00|10,0|0.0|10,0|0.0|0,0|0.0||||||||||||||
Breedtegraad|FieldType.DECIMAL|41,6928|41,9032|10,0|7.0|3.0|0,7|0.0|0.0|41,6928|41,6928|41,6928|41,7057|41,7441|41,8634|41,9032|41,9032|41,9032|41,78|0.109695|0,012033|0,292478|-2,33333
Lengtegraad|FieldType.DECIMAL|-87,6764|-87,6043|10,0|7.0|3.0|0,7|0.0|0.0|-87,6764|-87,6764|-87,6764|-87,6734|-87,6645|-87,6194|-87,6043|-87,6043|-87,6043|-87,6484|0.0386264|0,001492|0,344429|-2,33333
Location|FieldType.STRING||(41,903206037,-87,676361925)|10,0|0.0|10,0|0.0|0.0|7.0||||||||||||||

## <a name="impute-missing-values"></a>Ontbrekende waarden worden toegerekend

In gegevens sets, null-waarden, NaN en waarden die geen inhoud bevatten, worden beschouwd als ontbrekende waarden. Dit kan invloed hebben op de prestaties van uw machine learning modellen of tot ongeldige conclusies. De toerekenings methode is een algemene benadering voor het adresseren van ontbrekende waarden en is nuttig wanneer u een hoog percentage records mist die u niet eenvoudigweg kunt verwijderen.

Vanuit het gegevensset-profiel dat in de voor gaande sectie is gegenereerd `Latitude` , `Longitude` zien we dat en kolommen een hoog percentage ontbrekende waarden hebben. In dit voor beeld berekenen we het gemiddelde en de ontbrekende waarden voor deze twee kolommen.

Haal eerst de meest recente definitie van de gegevensset op [`get_definition()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-definition-version-id-none-) met een haakje voor de gegevens [`keep_columns()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow#keep-columns-columns--multicolumnselection--validate-column-exists--bool---false-----azureml-dataprep-api-dataflow-dataflow)met, zodat we alleen de kolommen die we willen adresseren weer geven.

```Python
from azureml.core.dataset import Dataset
import azureml.dataprep as dprep

# get the latest definition of Dataset
ds_def = dataset.get_definition()

# keep useful columns for this example
ds_def = ds_def.keep_columns(['ID', 'Arrest', 'Latitude', 'Longitude'])
ds_def.head(3)
```

||id|Aanhoudingsbevel| Breedtegraad|Lengtegraad|
-|---------|-----|---------|----------|
|0|10498554|False|41,692834|-87,604319|
|1|10516598|False| 41,744107 |-87,664494|
|2|10519196|False| NaN|NaN|

Controleer vervolgens de `MEAN` waarde van de kolom breedte met behulp [`summarize()`](/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-ml-py#summarize-summary-columns--typing-union-typing-list-azureml-dataprep-api-dataflow--summarycolumnsvalue---nonetype----none--group-by-columns--typing-union-typing-list-str---nonetype----none--join-back--bool---false--join-back-columns-prefix--typing-union-str--nonetype----none-----azureml-dataprep-api-dataflow-dataflow) van de functie. Deze functie accepteert een matrix van kolommen in de `group_by_columns` parameter opgeven voor het aggregatieniveau van. De `summary_columns` para meter accepteert `SummaryColumnsValue` de functie, waarmee de huidige kolom naam, de nieuwe berekende veld naam en de `SummaryFunction` uit te voeren waarde worden opgegeven.

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
|0|False|41,780049|

Wanneer we de waarden controleren die moeten worden toegerekend [`ImputeMissingValuesBuilder`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputemissingvaluesbuilder?view=azure-dataprep-py) , gebruikt u voor het leren van een vaste expressie die de kolommen met `MIN`een `MAX`berekende `MEAN` ,, waarde of `CUSTOM` een waarde toegerekend. Wanneer `group_by_columns` is opgegeven, worden ontbrekende waarden worden toegeschreven per groep met `MIN`, `MAX`, en `MEAN` berekend per groep.

De [`ImputeColumnArguments`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputecolumnarguments?view=azure-dataprep-pyfunction) accepteert een column_id-teken reeks en `ReplaceValueFunction` een om het type toerekening op te geven. Voor de ontbrekende lengte waarde, toegerekend met-87 op basis van externe kennis.

```Python
# impute with MEAN
impute_mean = dprep.ImputeColumnArguments(column_id = 'Latitude',
                                          impute_function = dprep.ReplaceValueFunction.MEAN)

# impute with custom value -87
impute_custom = dprep.ImputeColumnArguments(column_id='Longitude',
                                            custom_impute_value=-87)
```

Toegerekende stappen kunnen in een `ImputeMissingValuesBuilder` object worden gekoppeld met behulp van de [`impute_missing_values()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.builders?view=azure-dataprep-py#impute-missing-values-impute-columns--typing-list-azureml-dataprep-api-builders-imputecolumnarguments----none--group-by-columns--typing-union-typing-list-str---nonetype----none-----azureml-dataprep-api-builders-imputemissingvaluesbuilder) [`Builders`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.builders?view=azure-dataprep-py) functie klasse. De `impute_columns` parameter accepteert een matrix van `ImputeColumnArguments` objecten.

```Python
# get instance of ImputeMissingValuesBuilder
impute_builder = ds_def.builders.impute_missing_values(impute_columns=[impute_mean, impute_custom],
                                                   group_by_columns=['Arrest'])
```

Roep de [`learn()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputemissingvaluesbuilder?view=azure-dataprep-py#learn------none) functie aan voor het opslaan van de toegerekende stappen en pas deze toe op [`to_dataflow()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputemissingvaluesbuilder?view=azure-dataprep-py#to-dataflow------azureml-dataprep-api-dataflow-dataflow)een gegevensstroom object met behulp van.

```Python
impute_builder.learn()
ds_def = impute_builder.to_dataflow()
ds_def.head(3)
```

Zoals in de volgende uitvoer tabel wordt weer gegeven, werd de ontbrekende Latitude toegerekend `MEAN` met de `Arrest==False` waarde van de groep en de ontbrekende lengte graad werd toegerekend met-87.

||id|Aanhoudingsbevel|Breedtegraad|Lengtegraad
-|---------|-----|---------|----------
0|10498554|False|41,692834|-87,604319
1|10516598|False|41,744107|-87,664494
2|10519196|False|41,780049|-87,000000

Werk de gegevensset definitie bij met [`update_definition()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#update-definition-definition--definition-update-message-) om de uitgevoerde transformatie stappen te blijven uitvoeren.

```Python
dataset = dataset.update_definition(ds_def, 'Impute Missing')
dataset.head(3)
```

||id|Aanhoudingsbevel|Breedtegraad|Lengtegraad
-|---------|-----|---------|----------
0|10498554|False|41,692834|-87,604319
1|10516598|False|41,744107|-87,664494
2|10519196|False|41,780049|-87,000000

## <a name="create-assertion-rules"></a>Bevestigings regels maken

De gegevens die we gebruiken tijdens het opschonen en voorbereiden van gegevens zijn vaak een subset van de totale gegevens die we nodig hebben voor productie. Als gevolg hiervan is het mogelijk dat een aantal van de veronderstellingen die we maken als onderdeel van onze reiniging, worden ingesteld op ONWAAR. In een gegevensset die voortdurend wordt bijgewerkt, kan een kolom die oorspronkelijk alleen getallen in een bepaald bereik bevat, bijvoorbeeld een breder scala van waarden bevatten in latere uitvoeringen. Deze fouten leiden vaak tot beschadigde pijp lijnen of beschadigde gegevens.

Gegevens sets bieden ondersteuning voor het maken van bevestigingen op Data, die worden geëvalueerd als de pijp lijn wordt uitgevoerd. Met deze verklaringen kunnen we controleren of de veronderstellingen op de gegevens nauw keurig zijn en, wanneer dat niet het geval is, om fouten dienovereenkomstig af te handelen.

Als u bijvoorbeeld `Longitude` waarden in uw gegevensset wilt `Latitude` beperken tot specifieke numerieke bereiken, zorgt de [`assert_value()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#assert-value-columns--multicolumnselection--expression--azureml-dataprep-api-expressions-expression--policy--azureml-dataprep-api-engineapi-typedefinitions-assertpolicy----assertpolicy-errorvalue--1---error-code--str----assertionfailed------azureml-dataprep-api-dataflow-dataflow) methode ervoor dat dit altijd het geval is.

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

||type|Min.|Max.|Count|Ontbrekend aantal|Niet-ontbrekend aantal|Ontbrekend percentage|Aantal fouten|Leeg aantal|0,1%-kwantiel|1%-kwantiel|5%-kwantiel|25%-kwantiel|50%-kwantiel|75%-kwantiel|95%-kwantiel|99%-kwantiel|99,9%-kwantiel|Gemiddelde|Standaardafwijking|Variantie|Asymmetrie|Kurtosis
-|----|---|---|-----|-------------|-----------------|---------------|-----------|-----------|-------------|-----------|-----------|------------|------------|------------|------------|------------|--------------|----|------------------|--------|--------|--------
id|Field Type. INTEGER|1.04986e+07|1.05351e+07|10,0|0.0|10,0|0.0|0,0|0.0|1.04986e+07|1.04992e+07|1.04986e+07|1.05166 e + 07|1.05209 e + 07|1.05259 e + 07|1.05351e+07|1.05351e+07|1.05351e+07|1.05195 e + 07|12302,7|1.51358e+08|-0,495701|-1,02814
Aanhoudingsbevel|FieldType.BOOLEAN|False|False|10,0|0.0|10,0|0.0|0,0|0.0||||||||||||||
Breedtegraad|FieldType.DECIMAL|41,6928|41,9032|10,0|0.0|10,0|0.0|0,0|0.0|41,6928|41,7185|41,6928|41,78|41,78|41,78|41,9032|41,9032|41,9032|41,78|0,0517107|0.002674|0,837593|1,05
Lengtegraad|Field Type. INTEGER|-87|-87|10,0|0.0|10,0|0.0|3.0|0.0|-87|-87|-87|-87|-87|-87|-87|-87|-87|-87|0|0|NaN|NaN

In het profiel ziet u dat de `Error Count` `Longitude` kolom is 3. Met de volgende code wordt de gegevensset gefilterd, wordt de fout opgehaald en ziet u welke waarde ervoor zorgt dat de bevestiging mislukt. Pas uw code aan en reinig uw gegevens op de juiste manier.

```Python
from azureml.dataprep import col

ds_error = ds_def.filter(col('Longitude').is_error())
error = ds_error.head(10)['Longitude'][0]

print(error.originalValue)
```

    -87.60431945

## <a name="derive-columns-by-example"></a>Kolommen afleiden per voor beeld

Een van de meer geavanceerde hulp middelen voor gegevens sets is de mogelijkheid om kolommen af te leiden met voor beelden van de gewenste resultaten. Zo kunt u de SDK een voor beeld geven, zodat het code kan genereren om de beoogde trans formaties te verkrijgen.

```Python
from azureml.core.dataset import Dataset

# create an in-memory Dataset from a local file
dataset = Dataset.auto_read_files('./data/crime.csv')
dataset.head(3)
```

||id|Nummer van de aanvraag|Date|Blokkeren|...|
-|---------|-----|---------|----|---
0|10498554|HZ239907|2016-04-04 23:56:00|007XX E 111TH ST|...
1|10516598|HZ258664|2016-04-15 17:00:00|082XX S MARSHFIELD LAAN|...
2|10519196|HZ261252|2016-04-15 10:00:00|104XX S SACRAMENTO LAAN|...

Stel dat u de datum-en tijd notatie wilt transformeren naar 2016-04-04 10PM-12AM. Geef in [`derive_column_by_example()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#derive-column-by-example-source-columns--sourcecolumns--new-column-name--str--example-data--exampledata-----azureml-dataprep-api-dataflow-dataflow) het argument voor beelden van de gewenste uitvoer op in `example_data` de para meter in deze notatie: *(oorspronkelijke uitvoer, gewenste uitvoer)* .

De volgende code bevat twee voor beelden van de gewenste uitvoer: ("2016-04-04 23:56:00", "2016-04-04 10PM-12AM") en ("2016-04-15 17:00:00", "2016-04-15 4 P.M.-18:00 uur")

```Python
ds_def = dataset.get_definition()
ds_def = ds_def.derive_column_by_example(
        source_columns = "Date",
        new_column_name = "Date_Time_Range",
        example_data = [("2016-04-04 23:56:00", "2016-04-04 10PM-12AM"), ("2016-04-15 17:00:00", "2016-04-15 4PM-6PM")]
    )
ds_def.keep_columns(['ID','Date','Date_Time_Range']).head(3)
```

In de volgende tabel ziet u dat een nieuwe kolom Date_Time_Range records bevat in de opgegeven notatie.

||id|Date|Date_Time_Range
-|--------|-----|----
0|10498554|2016-04-04 23:56:00|2016-04-04 10PM-12AM
1|10516598|2016-04-15 17:00:00|2016-04-15 4 P.M.-18:00 UUR
2|10519196|2016-04-15 10:00:00|2016-04-15 10AM-12 P.M.

```Python
# update Dataset definition to keep the transformation steps performed.
dataset = dataset.update_definition(ds_def, 'Derive Date_Time_Range')
```

## <a name="fuzzy-groupings"></a>Fuzzy groeperingen

Wanneer u gegevens uit verschillende bronnen verzamelt, kunnen er variaties optreden in de spelling, het hoofdletter gebruik of afkortingen van dezelfde entiteiten. Standaardiseren en afstemmen van deze varianten met de fuzzy groepering van de SDK, of tekst Clustering, functionaliteit.

De kolom `inspections.business.city` bevat bijvoorbeeld verschillende formulieren van de plaatsnaam "San Francisco".

```Python
from azureml.core.dataset import Dataset

# create an in-memory Dataset from a local json file
dataset = Dataset.auto_read_files('./data/city.json')
dataset.head(5)
```

||inspecties. business. business_id|inspecties. business_name|inspecties. business. Address|inspecties. business. City|...|
-|-----|-------------------------|------------|--|---
0|16162|Quick-N-Ezeee Indiase voedings middelen|3861 24 St|AFHANKELIJK|...
1|67565|King van Thai noodles Cafe|1541 TARAVAL St|SAN FRANCISCO|...
2|67565|King van Thai noodles Cafe|1541 TARAVAL St|SAN FRANCISCO|...
3|68701|Grindz|832 Clement St|AFHANKELIJK|...
4|69186|& Evenementen, Inc. voor de premier-Cate ring|1255 22 St|S.F.|...

Laten we de [`fuzzy_group_column()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#fuzzy-group-column-source-column--str--new-column-name--str--similarity-threshold--float---0-8--similarity-score-column-name--str---none-----azureml-dataprep-api-dataflow-dataflow) methode gebruiken om een kolom toe te voegen met de automatisch gedetecteerde canonieke vorm van "San Francisco". De argumenten `source_column` en `new_column_name` zijn vereist. U kunt ook het volgende doen:

* Maak een nieuwe kolom, `similarity_score_column_name`die de vergelijk bare score tussen elk paar van de oorspronkelijke en canonieke waarden weergeeft.

* Geef een `similarity_threshold`, dit is de minimale vergelijk bare score voor de waarden die bij elkaar moeten worden gegroepeerd.

```Python
# get the latest Dataset definition
ds_def = dataset.get_definition()
ds_def = ds_def.fuzzy_group_column(source_column='inspections.business.city',
                                       new_column_name='city_grouped',
                                       similarity_threshold=0.8,
                                       similarity_score_column_name='similarity_score')
ds_def.head(5)
```

||inspecties. business. business_id|inspecties. business_name|inspecties. business. Address|inspecties. business. City|city_grouped|similarity_score|...|
-|-----|-------------------------|------------|--|---|---|---
0|16162|Quick-N-Ezeee Indiase voedings middelen|3861 24 St|AFHANKELIJK|San Francisco|0,814806|...
1|67565|King van Thai noodles Cafe|1541 TARAVAL St|SAN FRANCISCO|San Francisco|1,000000|...
2|67565|King van Thai noodles Cafe|1541 TARAVAL St|SAN FRANCISCO|San Francisco|1,000000|...
3|68701|Grindz|832 Clement St|AFHANKELIJK|San Francisco|0,814806|...
4|69186|& Evenementen, Inc. voor de premier-Cate ring|1255 22 St|S.F.|San Francisco|0,814806|...

In de definitie van de gegevensset zijn alle verschillende variaties van "San Francisco" in de gegevens genormaliseerd naar dezelfde teken reeks, "San Francisco".

Sla deze fuzzy groepering op Step Into de laatste gegevensset definitie `update_definition()`met.

```Python
dataset = dataset.update_definition(ds_def, 'fuzzy grouping')
```

## <a name="next-steps"></a>Volgende stappen

* Raadpleeg de [zelf studie](tutorial-auto-train-models.md) over geautomatiseerde machine learning voor een regressie model-voor beeld.

* Zie het [overzicht](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) van de SDK voor ontwerp patronen en gebruiks voorbeelden.

* Zie de [voorbeeld notitieblokken](https://aka.ms/dataset-tutorial)voor een voor beeld van het gebruik van gegevens sets.
