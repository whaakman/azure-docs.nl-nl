---
title: 'Transformaties: dataprep Python-SDK'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het transformeren van nettolading en opschonen van gegevens met Azure Machine Learning Data Prep SDK. Transformatie-methoden gebruiken voor kolommen toevoegen, filteren ongewenste rijen of kolommen en rekenen ontbrekende waarden.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: d32244cd49ebd42192b2388215f79a64cacb3caa
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53186137"
---
# <a name="transform-data-with-the-azure-machine-learning-data-prep-sdk"></a>Gegevens transformeren met de Azure Machine Learning Data Prep SDK

In dit artikel leert u verschillende methoden voor het laden van gegevens met de [SDK van Azure Machine Learning Data Prep](https://aka.ms/data-prep-sdk). De SDK biedt functies kunnen u eenvoudig aan kolommen toevoegen, filteren ongewenste rijen of kolommen, en rekenen ontbrekende waarden.

Momenteel zijn er functies voor de volgende taken:

- [Met behulp van een expressie kolom toevoegen](#column)
- [Ontbrekende waarden worden toegerekend](#impute-missing-values)
- [Kolom afleiden per voorbeeld](#derive-column-by-example)
- [Filteren](#filtering)
- [Aangepaste Python-transformaties](#custom-python-transforms)

## <a name="add-column-using-an-expression"></a>Met behulp van een expressie kolom toevoegen

De Azure Machine Learning Data Prep SDK bevat `substring` expressies die u kunt gebruiken om een waarde uit bestaande kolommen te berekenen en vervolgens samengesteld die waarde in een nieuwe kolom. In dit voorbeeld moet u gegevens laden en probeer kolommen toevoegen aan die invoergegevens.

```python
import azureml.dataprep as dprep

# loading data
dataflow = dprep.read_csv(path=r'data\crime0-10.csv')
dataflow.head(3)
```

||Id|Nummer van de aanvraag|Date|Blokkeren|IUCR|Het primaire Type|Description|Beschrijving van locatie|Aanhoudingsbevel|Binnenlandse|...|Ward|Community-gebied|Code van de FBI|X-coördinaat|Y-coördinaat|Jaar|Bijgewerkt op|Breedtegraad|Lengtegraad|Locatie|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|
|0|10140490|HY329907|07-05-2015 23:50:00 UUR|050XX N NEWLAND OPSLAAN|0820|DIEFSTAL|$500 EN KLIKT U ONDER|ADRES|false|false|...|41|10|06|1129230|1933315|2015|07-12-2015 |12:42:46 UUR|41.973309466|-87.800174996|(41.973309466,-87.800174996)|
|1|10139776|HY329265|07-05-2015 23:30:00 UUR|011XX W MORSE OPSLAAN|0460|ACCU|EENVOUDIGE|ADRES|false|true|...|49|1|08B|1167370|1946271|2015|07-12-2015 12:42:46 UUR|42.008124017|-87.65955018|(42.008124017,-87.65955018)|
|2|10140270|HY329253|07-05-2015 23:20:00 UUR|121XX S FRONT AVE|0486|ACCU|EENVOUDIGE VAN BINNENLANDSE ACCU|ADRES|false|true|...|9|53|08B|||2015|07-12-2015 12:42:46 UUR|


Gebruik de `substring(start, length)` expressie voor het extraheren van het voorvoegsel van de kolom aanvraag en die tekenreeks in een nieuwe kolom geplaatst `Case Category`. Doorgeven van de `substring_expression` variabele de `expression` parameter maakt u een nieuwe berekende kolom die de expressie op elke record wordt uitgevoerd.

```python
substring_expression = dprep.col('Case Number').substring(0, 2)
case_category = dataflow.add_column(new_column_name='Case Category',
                                    prior_column='Case Number',
                                    expression=substring_expression)
case_category.head(3)
```

||Id|Nummer van de aanvraag|Aanvraagcategorie|Date|Blokkeren|IUCR|Het primaire Type|Description|Beschrijving van locatie|Aanhoudingsbevel|...|Ward|Community-gebied|Code van de FBI|X-coördinaat|Y-coördinaat|Jaar|Bijgewerkt op|Breedtegraad|Lengtegraad|Locatie|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|------|
|0|10140490|HY329907|AAROM|07-05-2015 23:50:00 UUR|050XX N NEWLAND OPSLAAN|0820|DIEFSTAL|$500 EN KLIKT U ONDER|ADRES|false|false|...|41|10|06|1129230|1933315|2015|07-12-2015 |12:42:46 UUR|41.973309466|-87.800174996|(41.973309466,-87.800174996)|
|1|10139776|HY329265|AAROM|07-05-2015 23:30:00 UUR|011XX W MORSE OPSLAAN|0460|ACCU|EENVOUDIGE|ADRES|false|true|...|49|1|08B|1167370|1946271|2015|07-12-2015 12:42:46 UUR|42.008124017|-87.65955018|(42.008124017,-87.65955018)|
|2|10140270|HY329253|AAROM|07-05-2015 23:20:00 UUR|121XX S FRONT AVE|0486|ACCU|EENVOUDIGE VAN BINNENLANDSE ACCU|ADRES|false|true|...|9|53|08B|||2015|07-12-2015 12:42:46 UUR|



Gebruik de `substring(start)` expressie voor het extraheren van alleen het nummer van de kolom aanvraag en maak een nieuwe kolom. Converteren naar een numerieke gegevens met behulp van de `to_number()` functioneren en de naam van de kolom als een parameter doorgeven.

```python
substring_expression2 = dprep.col('Case Number').substring(2)
case_id = dataflow.add_column(new_column_name='Case Id',
                              prior_column='Case Number',
                              expression=substring_expression2)
case_id = case_id.to_number('Case Id')
case_id.head(3)
```

||Id|Nummer van de aanvraag|Aanvraag-Id|Date|Blokkeren|IUCR|Het primaire Type|Description|Beschrijving van locatie|Aanhoudingsbevel|...|Ward|Community-gebied|Code van de FBI|X-coördinaat|Y-coördinaat|Jaar|Bijgewerkt op|Breedtegraad|Lengtegraad|Locatie|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|------|
|0|10140490|HY329907|329907.0|07-05-2015 23:50:00 UUR|050XX N NEWLAND OPSLAAN|0820|DIEFSTAL|$500 EN KLIKT U ONDER|ADRES|false|false|...|41|10|06|1129230|1933315|2015|07-12-2015 |12:42:46 UUR|41.973309466|-87.800174996|(41.973309466,-87.800174996)|
|1|10139776|HY329265|329265.0|07-05-2015 23:30:00 UUR|011XX W MORSE OPSLAAN|0460|ACCU|EENVOUDIGE|ADRES|false|true|...|49|1|08B|1167370|1946271|2015|07-12-2015 12:42:46 UUR|42.008124017|-87.65955018|(42.008124017,-87.65955018)|
|2|10140270|HY329253|329253.0|07-05-2015 23:20:00 UUR|121XX S FRONT AVE|0486|ACCU|EENVOUDIGE VAN BINNENLANDSE ACCU|ADRES|false|true|...|9|53|08B|||2015|07-12-2015 12:42:46 UUR|

## <a name="impute-missing-values"></a>Ontbrekende waarden worden toegerekend

De SDK kunt rekenen ontbrekende waarden in de opgegeven kolommen. In dit voorbeeld u waarden voor breedtegraad en lengtegraad laden en vervolgens probeert te rekenen ontbrekende waarden in de ingevoerde gegevens.

```python
import azureml.dataprep as dprep

# loading input data
df = dprep.read_csv(r'data\crime0-10.csv')
df = df.keep_columns(['ID', 'Arrest', 'Latitude', 'Longitude'])
df = df.to_number(['Latitude', 'Longitude'])
df.head(5)
```

||Id|Aanhoudingsbevel|Breedtegraad|Lengtegraad|
|-----|------|-----|------|-----|
|0|10140490|false|41.973309|-87.800175|
|1|10139776|false|42.008124|-87.659550|
|2|10140270|false|NaN|NaN|
|3|10139885|false|41.902152|-87.754883|
|4|10140379|false|41.885610|-87.657009|

De derde record ontbreken waarden voor breedtegraad en lengtegraad. Als u wilt deze ontbrekende waarden worden toegerekend, gebruikt u `ImputeMissingValuesBuilder` voor meer informatie over een vaste-expressie. Deze kolommen met een van beide een berekende kunt rekenen `MIN`, `MAX`, `MEAN` waarde, of een `CUSTOM` waarde. Wanneer `group_by_columns` is opgegeven, worden ontbrekende waarden worden toegeschreven per groep met `MIN`, `MAX`, en `MEAN` berekend per groep.

Controleer de `MEAN` waarde van de breedtegraad kolom met behulp van de `summarize()` functie. Deze functie accepteert een matrix van kolommen in de `group_by_columns` parameter opgeven voor het aggregatieniveau van. De `summary_columns` parameter accepteert een `SummaryColumnsValue` aanroepen. Deze aanroep van de functie geeft de huidige kolomnaam, de nieuwe naam voor het berekende veld en de `SummaryFunction` om uit te voeren.

```python
df_mean = df.summarize(group_by_columns=['Arrest'],
                       summary_columns=[dprep.SummaryColumnsValue(column_id='Latitude',
                                                                 summary_column_name='Latitude_MEAN',
                                                                 summary_function=dprep.SummaryFunction.MEAN)])
df_mean = df_mean.filter(dprep.col('Arrest') == 'false')
df_mean.head(1)
```

||Aanhoudingsbevel|Latitude_MEAN|
|-----|-----|----|
|0|false|41.878961|

De `MEAN` waarde van Latitude nauwkeurige uiterlijk, gebruikt u de `ImputeColumnArguments` functie voor het rekenen. Deze functie accepteert een `column_id` tekenreeks, en een `ReplaceValueFunction` om op te geven van het type impute. Voor de ontbrekende Lengtegraadwaarde, moet u het rekenen met 42 op basis van externe kennis.

Rekenen stappen kunnen een keten worden samengevoegd in een `ImputeMissingValuesBuilder` object met behulp van de functie builder `impute_missing_values()`. De `impute_columns` parameter accepteert een matrix van `ImputeColumnArguments` objecten. Roep de `learn()` functie voor het opslaan van de stappen impute, en vervolgens toepassen op een gegevensstroom object met `to_dataflow()`.

```python
# impute with MEAN
impute_mean = dprep.ImputeColumnArguments(column_id='Latitude',
                                          impute_function=dprep.ReplaceValueFunction.MEAN)
# impute with custom value 42
impute_custom = dprep.ImputeColumnArguments(column_id='Longitude',
                                            custom_impute_value=42)
# get instance of ImputeMissingValuesBuilder
impute_builder = df.builders.impute_missing_values(impute_columns=[impute_mean, impute_custom],
                                                   group_by_columns=['Arrest'])
# call learn() to learn a fixed program to impute missing values
impute_builder.learn()
# call to_dataflow() to get a data flow with impute step added
df_imputed = impute_builder.to_dataflow()

# check impute result
df_imputed.head(5)
```

||Id|Aanhoudingsbevel|Breedtegraad|Lengtegraad|
|-----|------|-----|------|-----|
|0|10140490|false|41.973309|-87.800175|
|1|10139776|false|42.008124|-87.659550|
|2|10140270|false|41.878961|42.000000|
|3|10139885|false|41.902152|-87.754883|
|4|10140379|false|41.885610|-87.657009|

Zoals weergegeven in het bovenstaande resultaat, de ontbrekende breedtegraad is toegerekende met de `MEAN` waarde van `Arrest=='false'` groep. De ontbrekende lengtegraad is toegerekende met 42.

```python
imputed_longitude = df_imputed.to_pandas_dataframe()['Longitude'][2]
assert imputed_longitude == 42
```

## <a name="derive-column-by-example"></a>Kolom afleiden per voorbeeld

Een van de meer geavanceerde hulpprogramma's in de SDK van Azure Machine Learning Data Prep is de mogelijkheid voor het afleiden van kolommen met behulp van voorbeelden van de gewenste resultaten. Hiermee kunt u de SDK een voorbeeld geven, zodat deze code voor het bereiken van de gewenste transformatie kunt genereren.

```python
import azureml.dataprep as dprep
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/BostonWeather.csv')
dataflow.head(10)
```

||DATE|REPORTTPYE|HOURLYDRYBULBTEMPF|HOURLYRelativeHumidity|HOURLYWindSpeed|
|----|----|----|----|----|----|
|0|1/1/2015 0:54|FM-15|22|50|10|
|1|1/1/2015 1:00 uur|FM-12|22|50|10|
|2|1/1/2015 1:54|FM-15|22|50|10|
|3|1/1/2015 2:54|FM-15|22|50|11|
|4|1/1/2015 3:54|FM-15|24|46|13|
|5|1/1/2015 4:00 uur|FM-12|24|46|13|
|6|1/1/2015 4:54|FM-15|22|52|15|
|7|1/1/2015 5:54|FM-15|23|48|17|
|8|1/1/2015 6:54|FM-15|23|50|14|
|9|1/1/2015 7:00 uur|FM-12|23|50|14|

Wordt ervan uitgegaan dat u wilt deelnemen aan dit bestand met een gegevensset waarin datum en tijd in een indeling zijn ' 10 maart 2018 | 2 AM - 4 AM'.

```python
builder = dataflow.builders.derive_column_by_example(source_columns=['DATE'], new_column_name='date_timerange')
builder.add_example(source_data=df.iloc[1], example_value='Jan 1, 2015 12AM-2AM')
builder.preview() 
```

||DATE|date_timerange|
|----|----|----|
|0|1/1/2015 0:54|Vanaf 1 januari 2015 12 AM - 2 AM|
|1|1/1/2015 1:00 uur|Vanaf 1 januari 2015 12 AM - 2 AM|
|2|1/1/2015 1:54|Vanaf 1 januari 2015 12 AM - 2 AM|
|3|1/1/2015 2:54|Vanaf 1 januari 2015 2 AM - 4 AM|
|4|1/1/2015 3:54|Vanaf 1 januari 2015 2 AM - 4 AM|
|5|1/1/2015 4:00 uur|Vanaf 1 januari 2015 4 AM - 6 AM|
|6|1/1/2015 4:54|Vanaf 1 januari 2015 4 AM - 6 AM|
|7|1/1/2015 5:54|Vanaf 1 januari 2015 4 AM - 6 AM|
|8|1/1/2015 6:54|Vanaf 1 januari 2015 06: 00 - 8: 00 uur|
|9|1/1/2015 7:00 uur|Vanaf 1 januari 2015 06: 00 - 8: 00 uur|

De bovenstaande code maakt eerst een opbouwfunctie voor de afgeleide kolom. Bieden van een matrix van kolommen in de gegevensbron te houden (`DATE`), en een naam voor de nieuwe kolom die moet worden toegevoegd. Als het eerste voorbeeld doorgeven in de tweede rij (index 1) en geef een verwachte waarde voor de afgeleide kolom.

Roep ten slotte `builder.preview()` en de afgeleide kolom naast de bronkolom kunt zien. De indeling lijkt correct, maar u ziet alleen waarden voor dezelfde datum "1 januari 2015".

Nu in het aantal rijen dat u wilt doorgeven `skip` vanaf de bovenkant om te zien van rijen verder omlaag.

```
builder.preview(skip=30)
```

||DATE|date_timerange|
|-----|-----|-----|
|30|1/1/2015 22:54|Vanaf 1 januari 2015 10 PM - 12 AM|
|31|1/1/2015 23:54|Vanaf 1 januari 2015 10 PM - 12 AM|
|32|1/1/2015 23:59|Vanaf 1 januari 2015 10 PM - 12 AM|
|33|1/2/2015 0:54|Vanaf 1 februari 2015 12 AM - 2 AM|
|34|1/2/2015 1:00 uur|Vanaf 1 februari 2015 12 AM - 2 AM|
|35|1/2/2015 1:54|Vanaf 1 februari 2015 12 AM - 2 AM|
|36|1/2/2015 2:54|Vanaf 1 februari 2015 2 AM - 4 AM|
|37|1/2/2015 3:54|Vanaf 1 februari 2015 2 AM - 4 AM|
|38|1/2/2015 4:00 uur|Vanaf 1 februari 2015 4 AM - 6 AM|
|39|1/2/2015 4:54|Vanaf 1 februari 2015 4 AM - 6 AM|

Hier ziet u een probleem met de gegenereerde programma. Uitsluitend zijn gebaseerd op de een voorbeeld dat u hierboven hebt opgegeven, het programma afleiden hebt gekozen voor het parseren van de datum als 'Dag/maand/jaar', die niet wat u wilt dat in dit geval is. U kunt dit probleem oplossen, bieden met behulp van een ander voorbeeld de `add_example()` functioneren in de `builder` variabele.

```python
builder.add_example(source_data=preview_df.iloc[3], example_value='Jan 2, 2015 12AM-2AM')
builder.preview(skip=30, count=10)
```

||DATE|date_timerange|
|-----|-----|-----|
|30|1/1/2015 22:54|Vanaf 1 januari 2015 10 PM - 12 AM|
|31|1/1/2015 23:54|Vanaf 1 januari 2015 10 PM - 12 AM|
|32|1/1/2015 23:59|Vanaf 1 januari 2015 10 PM - 12 AM|
|33|1/2/2015 0:54|Jan 2, 2015 12 AM - 2 AM|
|34|1/2/2015 1:00 uur|Jan 2, 2015 12 AM - 2 AM|
|35|1/2/2015 1:54|Jan 2, 2015 12 AM - 2 AM|
|36|1/2/2015 2:54|2 januari 2015 2 AM - 4 AM|
|37|1/2/2015 3:54|2 januari 2015 2 AM - 4 AM|
|38|1/2/2015 4:00 uur|2 januari 2015 4 AM - 6 AM|
|39|1/2/2015 4:54|2 januari 2015 4 AM - 6 AM|

Nu rijen goed verwerkt ' 1/2/2015' als 'Jan 2, 2015', maar als u verder te naar de afgeleide kolom zoeken ziet u dat de waarden aan het einde niets in afgeleide kolom hebben. Om op te lossen die, moet u een ander voorbeeld voor rij 66 opgeven.

```python
builder.add_example(source_data=preview_df.iloc[66], example_value='Jan 29, 2015 8PM-10PM')
builder.preview(count=10)
```

||DATE|date_timerange|
|-----|-----|-----|
|0|1/1/2015 22:54|Vanaf 1 januari 2015 10 PM - 12 AM|
|1|1/1/2015 23:54|Vanaf 1 januari 2015 10 PM - 12 AM|
|2|1/1/2015 23:59|Vanaf 1 januari 2015 10 PM - 12 AM|
|3|1/2/2015 0:54|Jan 2, 2015 12 AM - 2 AM|
|4|1/2/2015 1:00 uur|Jan 2, 2015 12 AM - 2 AM|
|5|1/2/2015 1:54|Jan 2, 2015 12 AM - 2 AM|
|6|1/2/2015 2:54|2 januari 2015 2 AM - 4 AM|
|7|1/2/2015 3:54|2 januari 2015 2 AM - 4 AM|
|8|1/2/2015 4:00 uur|2 januari 2015 4 AM - 6 AM|
|9|1/2/2015 4:54|2 januari 2015 4 AM - 6 AM|

Afzonderlijke datum en tijd met ' |', toevoegen van een ander voorbeeld. Deze tijd, in plaats van doorgeven in een rij van de Preview-versie, maken van een woordenlijst met de naam van de kolom op de waarde voor de `source_data` parameter.

```python
builder.add_example(source_data={'DATE': '11/11/2015 0:54'}, example_value='Nov 11, 2015 | 12AM-2AM')
builder.preview(count=10)
```

||DATE|date_timerange|
|-----|-----|-----|
|0|1/1/2015 22:54|Geen|
|1|1/1/2015 23:54|Geen|
|2|1/1/2015 23:59|Geen|
|3|1/2/2015 0:54|Geen|
|4|1/2/2015 1:00 uur|Geen|
|5|1/2/2015 1:54|Geen|
|6|1/2/2015 2:54|Geen|
|7|1/2/2015 3:54|Geen|
|8|1/2/2015 4:00 uur|Geen|
|9|1/2/2015 4:54|Geen|

Dit was duidelijk negatieve gevolgen nu alleen rijen met waarden in afgeleide kolom de waarden die overeenkomen met precies met de voorbeelden die zijn er beschikbaar zijn. Bel `list_examples()` op het object opbouwfunctie voor een overzicht van het huidige voorbeeld afleidingen.

```python
examples = builder.list_examples()
```

| |DATE|Voorbeeld|example_id|
| -------- | -------- | -------- | -------- |
|0|1/1/2015 1:00 uur|Vanaf 1 januari 2015 12 AM - 2 AM|-1|
|1|1/2/2015 0:54|Jan 2, 2015 12 AM - 2 AM|-2|
|2|1/29/2015 20:54|29 januari 2015 8 PM - 10 PM|-3|
|3|11/11/2015 0:54|11 november 2015 \| 12 AM - 2 AM|-4|

In dit geval hebt inconsistent voorbeelden opgegeven. Los het probleem, vervangt u de eerste drie voorbeelden met de juiste (met inbegrip van ' |' tussen de datum en tijd).

Inconsistente voorbeelden oplossen door het verwijderen van voorbeelden die onjuist zijn (door een van beide `example_row` van de pandas DataFrame, of door te geven in `example_id` waarde) en vervolgens toe te voegen nieuwe voorbeelden terug gewijzigd.

```python
builder.delete_example(example_id=-1)
builder.delete_example(example_row=examples.iloc[1])
builder.delete_example(example_row=examples.iloc[2])
builder.add_example(examples.iloc[0], 'Jan 1, 2015 | 12AM-2AM')
builder.add_example(examples.iloc[1], 'Jan 2, 2015 | 12AM-2AM')
builder.add_example(examples.iloc[2], 'Jan 29, 2015 | 8PM-10PM')
builder.preview()
```

| | DATE | date_timerange |
| -------- | -------- | -------- |
| 0 | 1/1/2015 0:54 | Vanaf 1 januari 2015 \| 12 AM - 2 AM |
| 1 | 1/1/2015 1:00 uur | Vanaf 1 januari 2015 \| 12 AM - 2 AM |
| 2 | 1/1/2015 1:54 | Vanaf 1 januari 2015 \| 12 AM - 2 AM |
| 3 | 1/1/2015 2:54 | Vanaf 1 januari 2015 \| 2 AM - 4 AM |
| 4 | 1/1/2015 3:54 | Vanaf 1 januari 2015 \| 2 AM - 4 AM |
| 5 | 1/1/2015 4:00 uur | Vanaf 1 januari 2015 \| 4 AM - 6 AM|
| 6 | 1/1/2015 4:54 | Vanaf 1 januari 2015 \| 4 AM - 6 AM|
| 7 | 1/1/2015 5:54 | Vanaf 1 januari 2015 \| 4 AM - 6 AM|
| 8 | 1/1/2015 6:54 | Vanaf 1 januari 2015 \| 06: 00 - 8: 00 uur|
| 9 | 1/1/2015 7:00 uur | Vanaf 1 januari 2015 \| 06: 00 - 8: 00 uur|

Nu de gegevens is gelukt en u kunt aanroepen `to_dataflow()` op de opbouwfunctie voor, waarmee een gegevensstroom wordt geretourneerd met de gewenste afgeleide kolommen toegevoegd.

```python
dataflow = builder.to_dataflow()
df = dataflow.to_pandas_dataframe()
```

## <a name="filtering"></a>Filteren

De SDK bevat de methoden `Dataflow.drop_columns` en `Dataflow.filter` zodat u kolommen of rijen filteren.

### <a name="initial-setup"></a>Eerste installatie

```python
import azureml.dataprep as dprep
from datetime import datetime
dataflow = dprep.read_csv(path='https://dprepdata.blob.core.windows.net/demo/green-small/*')
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Store_and_fwd_flag|RateCodeID|Pickup_longitude|Pickup_latitude|Dropoff_longitude|Dropoff_latitude|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|0|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|N|1|0|0|0|0|1|.00|0|0|21,25|
|2|2013-08-01 09:13:00|2013-08-01 11:38:00|N|1|0|0|0|0|2|.00|0|0|75|
|3|2013-08-01-09:48:00 uur|2013-08-01 09:49:00|N|5|0|0|0|0|1|.00|0|1|2.1|
|4|2013-08-01-10:38:35|2013-08-01 10:38:51|N|1|0|0|0|0|1|.00|0|0|3.25|

### <a name="filtering-columns"></a>Kolommen filteren

Kolommen wilt filteren, gebruikt u `Dataflow.drop_columns`. Deze methode heeft een lijst met kolommen om te verwijderen of een complexere argument met de naam `ColumnSelector`.

#### <a name="filtering-columns-with-list-of-strings"></a>Filteren van kolommen met een lijst met tekenreeksen

In dit voorbeeld `drop_columns` neemt een lijst met tekenreeksen. Elke tekenreeks moet exact overeenkomen met de gewenste kolom te verwijderen.

```python
dataflow = dataflow.drop_columns(['Store_and_fwd_flag', 'RateCodeID'])
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Pickup_longitude|Pickup_latitude|Dropoff_longitude|Dropoff_latitude|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|0|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|Geen|
|1|2013-08-01 08:14:37|2013-08-01-09:09:06|0|0|0|0|1|.00|0|0|21,25|
|2|2013-08-01 09:13:00|2013-08-01-11:38:00 uur|0|0|0|0|2|.00|0|0|75|
|3|2013-08-01-09:48:00 uur|2013-08-01-09:49:00 uur|0|0|0|0|1|.00|0|1|2.1|
|4|2013-08-01-10:38:35|2013-08-01-10:38:51|0|0|0|0|1|.00|0|0|3.25|

#### <a name="filtering-columns-with-regex"></a>Filteren van kolommen met een reguliere expressie

U kunt ook de `ColumnSelector` expressie om te verwijderen van kolommen die overeenkomen met een bepaalde reguliere expressie. In dit voorbeeld u de kolommen die overeenkomen met de expressie neerzetten `Column*|.*longitude|.*latitude`.

```python
dataflow = dataflow.drop_columns(dprep.ColumnSelector('Column*|.*longitud|.*latitude', True, True))
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|Geen|Geen|Geen|Geen|Geen|Geen|Geen|
|1|2013-08-01 08:14:37|2013-08-01-09:09:06|1|.00|0|0|21,25|
|2|2013-08-01 09:13:00|2013-08-01-11:38:00 uur|2|.00|0|0|75|
|3|2013-08-01-09:48:00 uur|2013-08-01-09:49:00 uur|1|.00|0|1|2.1|
|4|2013-08-01-10:38:35|2013-08-01-10:38:51|1|.00|0|0|3.25|

## <a name="filtering-rows"></a>Rijen filteren

Rijen filteren, gebruikt u `DataFlow.filter`. Deze methode een Azure Machine Learning Data Prep SDK-expressie als een argument neemt en een nieuwe gegevensstroom met de rijen die de expressie wordt geëvalueerd als waar retourneert. Expressies zijn gebouwd met behulp van de expressie builders (`col`, `f_not`, `f_and`, `f_or`) en reguliere operators (>, <>, =, < =, ==,! =).

### <a name="filtering-rows-with-simple-expressions"></a>Rijen met eenvoudige expressies filteren

De opbouwfunctie voor expressies gebruiken `col`, geef de naam van de kolom als een tekenreeksargument `col('column_name')`. Deze expressie gebruiken in combinatie met een van de volgende standaardoperators >, <>, =, < =, ==,! = voor het bouwen van een expressie zoals `col('Tip_amount') > 0`. Tot slot geven de samengestelde expressie in de `Dataflow.filter` functie.

In dit voorbeeld `dataflow.filter(col('Tip_amount') > 0)` retourneert een nieuwe gegevensstroom met de rijen waarin de waarde van `Tip_amount` is groter dan 0.

> [!NOTE] 
> `Tip_amount` eerst wordt geconverteerd naar een numerieke waarde, waarmee je een expressie die wordt vergeleken met andere numerieke waarden maken.

```python
dataflow = dataflow.to_number(['Tip_amount'])
dataflow = dataflow.filter(dprep.col('Tip_amount') > 0)
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-01 19:33:28|2013-08-01 19:35:21|5|.00|0,08|0|4,58|
|1|2013-08-05 13:16:38|2013-08-05 13:18:24 uur per dag|1|.00|0,30|0|3.8|
|2|2013-08-05 14:11:42|2013-08-05 14:12:47|1|.00|1,05|0|4.55|
|3|2013-08-05 14:15:56|2013-08-05 14:18:04|5|.00|2,22|0|5,72|
|4|2013-08-05 14:42:14|2013-08-05 14:42:38|1|.00|0.88|0|4.38|

### <a name="filtering-rows-with-complex-expressions"></a>Rijen met complexe expressies filteren

Combineren om te filteren met behulp van complexe expressies, een of meer eenvoudige expressies met de expressie-opbouwfuncties `f_not`, `f_and`, of `f_or`.

In dit voorbeeld `Dataflow.filter` retourneert een nieuwe gegevensstroom met de rijen waar `'Passenger_count'` is kleiner dan 5 en `'Tolls_amount'` is groter dan 0.

```python
dataflow = dataflow.to_number(['Passenger_count', 'Tolls_amount'])
dataflow = dataflow.filter(dprep.f_and(dprep.col('Passenger_count') < 5, dprep.col('Tolls_amount') > 0))
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-08-12:16:00 uur|2013-08-08-12:16:00 uur|1.0|.00|2.25|5,00|19.75|
|1|2013-08-12-14:43:53|2013-08-12 15:04:50|1.0|5.28|6.46|5.33|32.29|
|2|2013-08-12 19:48:12|2013-08-12 20:03:42|1.0|5.50|1,00|10.66|30.66|
|3|13-08-2013 06:11:06|13-08-2013 06:30:28|1.0|9,57|7.47|5.33|44,8|
|4|2013-08-16 20:33:50|2013-08-16 20:48:50|1.0|5.63|3,00|5.33|27.83|

Het is ook mogelijk om te filteren van rijen die meer dan één opbouwfunctie voor het maken van een geneste expressie combineren.

> [!NOTE]
> `lpep_pickup_datetime` en `Lpep_dropoff_datetime` eerst worden geconverteerd naar datum/tijd, waarmee je een expressie die wordt vergeleken met andere datum / tijdwaarden maken.

```python
dataflow = dataflow.to_datetime(['lpep_pickup_datetime', 'Lpep_dropoff_datetime'], ['%Y-%m-%d %H:%M:%S'])
dataflow = dataflow.to_number(['Total_amount', 'Trip_distance'])
mid_2013 = datetime(2013,7,1)
dataflow = dataflow.filter(
    dprep.f_and(
        dprep.f_or(
            dprep.col('lpep_pickup_datetime') > mid_2013,
            dprep.col('Lpep_dropoff_datetime') > mid_2013),
        dprep.f_and(
            dprep.col('Total_amount') > 40,
            dprep.col('Trip_distance') < 10)))
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|13-08-2013 06:11:06 + 00:00 uur|13-08-2013 06:30:28 + 00:00 uur|1.0|9,57|7.47|5.33|44.80|
|1|23-08-2013 12:28:20 + 00:00 uur|23-08-2013 12:50:28 + 00:00 uur|2.0|8.22|8.08|5.33|40.41|
|2|2013-08-25-09:12:52 + 00:00 uur|2013-08-25-09:34:34 + 00:00 uur|1.0|8,80|8.33|5.33|41.66|
|3|2013-08-25 16:46:51 + 00:00 uur|2013-08-25 17:13:55 + 00:00 uur|2.0|9.66|7.37|5.33|44.20|
|4|2013-08-25 17:42:11 + 00:00 uur|2013-08-25 18:02:57 + 00:00 uur|1.0|9.60|6.87|5.33|41.20|

## <a name="custom-python-transforms"></a>Aangepaste Python-transformaties

Er wordt altijd zijn scenario's wanneer de eenvoudigste manier voor het maken van een transformatie is uw eigen script schrijven. De SDK biedt drie Extensiepunten die u voor aangepaste Python-scripts gebruiken kunt.

- Script van de nieuwe kolom
- Nieuw script filter
- Partitie transformeren

Elk van de extensies wordt ondersteund in de runtime scale-up en scale-out. Een groot voordeel van het gebruik van deze extensie is dat u niet nodig voor het ophalen van alle gegevens om te kunnen maken van een gegevensframe. Uw aangepaste Python code wordt uitgevoerd, net zoals andere transformaties, op schaal door partitie en meestal parallel.

### <a name="initial-data-preparation"></a>Initiële gegevens voor te bereiden

Begin met het laden van gegevens uit Azure Blob.

```python
import azureml.dataprep as dprep
col = dprep.col

df = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv', skip_rows=1)
df.head(5)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0|ALABAMA|1|101710|Hale County|10171002158| |
|1|ALABAMA|1|101710|Hale County|10171002162| |
|2|ALABAMA|1|101710|Hale County|10171002156| |
|3|ALABAMA|1|101710|Hale County|10171000588|2|
|4|ALABAMA|1|101710|Hale County|10171000589| |

Verwijder in de gegevensset en enkele algemene transformaties doen.

```python
df = df.keep_columns(['stnam', 'leanm10', 'ncessch', 'MAM_MTH00numvalid_1011'])
df = df.replace_na(columns=['leanm10', 'MAM_MTH00numvalid_1011'], custom_na_list='.')
df = df.to_number(['ncessch', 'MAM_MTH00numvalid_1011'])
df.head(5)
```

| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale County|1.017100e + 10|Geen|
|1|ALABAMA|Hale County|1.017100e + 10|Geen|
|2|ALABAMA|Hale County|1.017100e + 10|Geen|
|3|ALABAMA|Hale County|1.017100e + 10|2|
|4|ALABAMA|Hale County|1.017100e + 10|Geen|

Zoek naar null-waarden met behulp van de volgende filters.

```python
df.filter(col('MAM_MTH00numvalid_1011').is_null()).head(5)
```

| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale County|1.017100e + 10|Geen|
|1|ALABAMA|Hale County|1.017100e + 10|Geen|
|2|ALABAMA|Hale County|1.017100e + 10|Geen|
|3|ALABAMA|Hale County|1.017100e + 10|Geen|
|4|ALABAMA|Hale County|1.017100e + 10|Geen|

### <a name="transform-partition"></a>Partitie transformeren

Een functie pandas gebruiken alle null-waarden vervangen door een 0. Deze code wordt uitgevoerd door partitie, niet op in één keer de volledige gegevensset. Dit betekent dat op een grote gegevensset met deze code kan parallel worden uitgevoerd als de runtime de gegevens, partitie door partitie verwerkt.

Het Python-script moet een aangeroepen functie definiëren `transform()` die neemt twee argumenten, `df` en `index`. De `df` argument is een pandas dataframe dat de gegevens voor de partitie bevat en de `index` argument is een unieke id van de partitie. De omzetfunctie het doorgegeven gegevensframe volledig kunt bewerken, maar een dataframe moet retourneren. Alle bibliotheken die het Python-script importeert, moeten zich in de omgeving waar de gegevensstroom wordt uitgevoerd.

```python
df = df.transform_partition("""
def transform(df, index):
    df['MAM_MTH00numvalid_1011'].fillna(0,inplace=True)
    return df
""")
df.head(5)
```

||stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale County|1.017100e + 10|0.0|
|1|ALABAMA|Hale County|1.017100e + 10|0.0|
|2|ALABAMA|Hale County|1.017100e + 10|0.0|
|3|ALABAMA|Hale County|1.017100e + 10|2.0|
|4|ALABAMA|Hale County|1.017100e + 10|0.0|

### <a name="new-script-column"></a>Script van de nieuwe kolom

U kunt Python-code gebruiken om te maken van een nieuwe kolom met de naam van de regio en de Statusnaam en ook om de Statusnaam van de van hun bedrijfsactiviteiten. U doet dit door gebruik van de `new_script_column()` methode voor de gegevensstroom.

Het Python-script moet een aangeroepen functie definiëren `newvalue()` die een één argument `row`. De `row` argument is een dict (`key`: naam van kolom, `val`: huidige waarde) en voor elke rij in de gegevensset worden doorgegeven aan deze functie. Deze functie moet een waarde die moet worden gebruikt in de nieuwe kolom retourneren. Alle bibliotheken die het Python-script importeert, moeten zich in de omgeving waar de gegevensstroom wordt uitgevoerd.

```python
df = df.new_script_column(new_column_name='county_state', insert_after='leanm10', script="""
def newvalue(row):
    return row['leanm10'] + ', ' + row['stnam'].title()
""")
df.head(5)
```

||stnam|leanm10|county_state|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale County|Hale regio, Alabama|1.017100e + 10|0.0|
|1|ALABAMA|Hale County|Hale regio, Alabama|1.017100e + 10|0.0|
|2|ALABAMA|Hale County|Hale regio, Alabama|1.017100e + 10|0.0|
|3|ALABAMA|Hale County|Hale regio, Alabama|1.017100e + 10|2.0|
|4|ALABAMA|Hale County|Hale regio, Alabama|1.017100e + 10|0.0|

### <a name="new-script-filter"></a>Nieuw Script Filter

Bouw een Python-expressie om te filteren van de gegevens is ingesteld op alleen rijen waar 'Hale' niet in de nieuwe is `county_state` kolom. De expressie retourneert `True` als we willen houden van de rij en `False` verwijderen van de rij.

```python
df = df.new_script_filter("""
def includerow(row):
    val = row['county_state']
    return 'Hale' not in val
""")
df.head(5)
```

||stnam|leanm10|county_state|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Jefferson County|Jefferson regio, Alabama|1.019200e + 10|1.0|
|1|ALABAMA|Jefferson County|Jefferson regio, Alabama|1.019200e + 10|0.0|
|2|ALABAMA|Jefferson County|Jefferson regio, Alabama|1.019200e + 10|0.0|
|3|ALABAMA|Jefferson County|Jefferson regio, Alabama|1.019200e + 10|0.0|
|4|ALABAMA|Jefferson County|Jefferson regio, Alabama|1.019200e + 10|0.0|
