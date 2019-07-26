---
title: 'Schrijven: gegevens prep python SDK'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het schrijven van gegevens met Azure Machine Learning Data Prep SDK. U kunt schrijven van gegevens op elk gewenst moment in een gegevensstroom en bestanden in elk van onze ondersteunde locaties (lokale bestandssysteem, Azure Blob Storage en Azure Data Lake Storage).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: jmartens
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 6753be5613b10b64936cddaafbb9859aad837b02
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68358640"
---
# <a name="write-and-configure-data--with-the-azure-machine-learning-data-prep-sdk"></a>Gegevens schrijven en configureren met de Azure Machine Learning data prep SDK

In dit artikel leert u verschillende methoden voor het schrijven van gegevens met behulp van de [Azure machine learning data prep PYTHON SDK](https://aka.ms/data-prep-sdk) en hoe u die gegevens kunt configureren voor experimenten met de [Azure machine learning SDK voor python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).  Uitvoer gegevens kunnen op elk punt in een gegevensstroom worden geschreven. Schrijf bewerkingen worden toegevoegd als stappen voor de resulterende gegevens stroom en deze stappen worden uitgevoerd telkens wanneer de gegevens stroom wordt uitgevoerd. Gegevens worden naar meerdere partitiebestanden naar parallelle schrijfbewerkingen geschreven.

> [!Important]
> Als u een nieuwe oplossing bouwt, probeert u de [Azure machine learning gegevens sets](how-to-explore-prepare-data.md) (preview) om uw gegevens te transformeren, momentopname gegevens op te slaan en gegevensset-definities op basis van een archief te bewaren. Gegevens sets is de volgende versie van de data prep SDK, die uitgebreide functionaliteit biedt voor het beheren van gegevens sets in AI-oplossingen.
> Als u het `azureml-dataprep` pakket gebruikt om een gegevensstroom te maken met uw trans formaties in plaats `azureml-datasets` van het pakket te gebruiken om een gegevensset te maken, kunt u later geen moment opnamen of versie gegevens sets gebruiken.

Aangezien er geen beperkingen voor het aantal schrijven stappen er zijn in een pijplijn, kunt u eenvoudig aanvullende schrijven stappen voor het ophalen van tussenliggende resultaten voor het oplossen van problemen of voor andere pijplijnen toevoegen.

Telkens wanneer die u een stap schrijven, uitvoeren een volledige pull van de gegevens in de gegevensstroom is opgetreden. Een gegevensstroom met drie stappen voor schrijven wordt bijvoorbeeld lezen en te verwerken van elke record in de gegevensset drie keer.

## <a name="supported-data-types-and-location"></a>Ondersteunde gegevenstypen en locatie

De volgende indelingen worden ondersteund
-   Bestanden met scheidingstekens (CSV, TSV, enz.)
-   Parquet-bestanden

Met behulp van de Azure Machine Learning data prep python SDK kunt u gegevens schrijven naar:
+ een lokaal bestandssysteem
+ Azure Blob Storage
+ Azure Data Lake Storage

## <a name="spark-considerations"></a>Overwegingen voor Spark

Bij het uitvoeren van een gegevensstroom in Spark, moet u naar een lege map schrijven. Er wordt geprobeerd om uit te voeren een schrijven met een bestaande map, resulteert dit in een fout. Zorg ervoor dat de doelmap is leeg of gebruik een andere doellocatie voor elke uitvoering, of het schrijven mislukt.

## <a name="monitoring-write-operations"></a>Bewaking van schrijfbewerkingen

Een sentinel-bestand met de naam geslaagd is voor uw gemak hebt gegenereerd als een schrijfbewerking is voltooid. De aanwezigheid kunt u identificeren wanneer een tussenliggende schrijven is voltooid zonder te wachten op de hele pijplijn om te voltooien.

## <a name="example-write-code"></a>Voorbeeld schrijven van code

Voor dit voor beeld begint u met het laden van gegevens in een `auto_read_file()`gegevens stroom met behulp van. U deze gegevens met verschillende indelingen opnieuw gebruiken.

```python
import azureml.dataprep as dprep
t = dprep.auto_read_file('./data/fixed_width_file.txt')
t = t.to_number('Column3')
t.head(5)
```

Voorbeelduitvoer:

| | Kolom1 | Kolom2 | Kolom3 | Kolom4 | Column5 | Kolom6 | Column7 | Column8 | Column9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
|0| 10000.0 | 99999.0 | Geen | NO | NO | ENRS | NaN | NaN | NaN |   
|1| 10003.0 | 99999.0 | Geen | NO | NO | ENSO | NaN | NaN | NaN |   
|2| 10010.0 | 99999.0 | Geen | NO | JN | ENJA | 70933.0 | -8667.0 | 90.0 |
|3| 10013.0 | 99999.0 | Geen | NO | NO |      | NaN | NaN | NaN |
|4| 10014.0 | 99999.0 | Geen | NO | NO | ENSO | 59783.0 | 5350.0 |  500.0|

### <a name="delimited-file-example"></a>Voorbeeld van een bestand met scheidingstekens

De volgende code gebruikt de [`write_to_csv()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow#write-to-csv-directory-path--datadestination--separator--str--------na--str----na---error--str----error------azureml-dataprep-api-dataflow-dataflow) functie om gegevens te schrijven naar een bestand met scheidings tekens.

```python
# Create a new data flow using `write_to_csv`
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'))

# Run the data flow to begin the write operation.
write_t.run_local()

written_files = dprep.read_csv('./test_out/part-*')
written_files.head(5)
```

Voorbeelduitvoer:

| | Kolom1 | Kolom2 | Kolom3 | Kolom4 | Column5 | Kolom6 | Column7 | Column8 | Column9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
|0| 10000.0 | 99999.0 | FOUT | NO | NO | ENRS | NaN    | NaN | NaN |   
|1| 10003.0 | 99999.0 | FOUT | NO | NO | ENSO |    NaN | NaN | NaN |   
|2| 10010.0 | 99999.0 | FOUT | NO | JN | ENJA |    70933.0 | -8667.0 | 90.0 |
|3| 10013.0 | 99999.0 | FOUT | NO | NO |     | NaN | NaN | NaN |
|4| 10014.0 | 99999.0 | FOUT | NO | NO | ENSO |    59783.0 | 5350.0 |  500.0|

In de uitvoer van de voorgaande weergegeven verschillende fouten in de numerieke kolommen vanwege de cijfers die zijn niet juist geparseerd. Wanneer geschreven naar CSV, null-waarden vervangen door de tekenreeks "ERROR" standaard.

Parameters als onderdeel van uw schrijven aanroepen en geef een tekenreeks die moet worden gebruikt voor null-waarden toevoegen.

```python
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'),
                         error='BadData',
                         na='NA')
write_t.run_local()
written_files = dprep.read_csv('./test_out/part-*')
written_files.head(5)
```

De bovenstaande code wordt deze uitvoer gegenereerd:

| | Kolom1 | Kolom2 | Kolom3 | Kolom4 | Column5 | Kolom6 | Column7 | Column8 | Column9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
|0| 10000.0 | 99999.0 | BadData | NO | NO | ENRS | NaN  | NaN | NaN |   
|1| 10003.0 | 99999.0 | BadData | NO | NO | ENSO |  NaN | NaN | NaN |   
|2| 10010.0 | 99999.0 | BadData | NO | JN | ENJA |  70933.0 | -8667.0 | 90.0 |
|3| 10013.0 | 99999.0 | BadData | NO | NO |   | NaN | NaN | NaN |
|4| 10014.0 | 99999.0 | BadData | NO | NO | ENSO |  59783.0 | 5350.0 |  500.0|

### <a name="parquet-file-example"></a>Voorbeeld van een parquet

[`write_to_parquet()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow#write-to-parquet-file-path--typing-union--datadestination--nonetype----none--directory-path--typing-union--datadestination--nonetype----none--single-file--bool---false--error--str----error---row-groups--int---0-----azureml-dataprep-api-dataflow-dataflow) Vergelijkbaar met `write_to_csv()`, retourneert de functie een nieuwe gegevens stroom met een schrijf Parquet-stap die wordt uitgevoerd wanneer de gegevens stroom wordt uitgevoerd.

```python
write_parquet_t = t.write_to_parquet(directory_path=dprep.LocalFileOutput('./test_parquet_out/'),
                                     error='MiscreantData')
```

De gegevensstroom voor het starten van de write-bewerking uitvoeren.

```python
write_parquet_t.run_local()

written_parquet_files = dprep.read_parquet_file('./test_parquet_out/part-*')
written_parquet_files.head(5)
```

De bovenstaande code wordt deze uitvoer gegenereerd:

|   | Kolom1 | Kolom2 | Kolom3 | Kolom4 | Column5 | Kolom6 | Column7 | Column8 | Column9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |-------- |
|0| 10000.0 | 99999.0 | MiscreantData | NO | NO | ENRS | MiscreantData | MiscreantData | MiscreantData |
|1| 10003.0 | 99999.0 | MiscreantData | NO | NO | ENSO | MiscreantData | MiscreantData | MiscreantData |   
|2| 10010.0 | 99999.0 | MiscreantData | NO| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0 | 99999.0 | MiscreantData | NO| NO| |   MiscreantData|    MiscreantData|    MiscreantData|
|4| 10014.0 | 99999.0 | MiscreantData | NO| NO| ENSO|   59783.0|    5350.0| 500.0|

## <a name="configure-data-for-automated-machine-learning-training"></a>Gegevens configureren voor automatische machine learning training

Geef uw zojuist geschreven gegevens bestand door aan [`AutoMLConfig`](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py#automlconfig) een object ter voor bereiding op automatische machine learning training. 

In het volgende code voorbeeld ziet u hoe u uw gegevens stroom kunt converteren naar een Panda data frame en deze vervolgens splitsen in trainings-en test sets voor automatische machine learning training.

```Python
from azureml.train.automl import AutoMLConfig
from sklearn.model_selection import train_test_split

dflow = dprep.auto_read_file(path="")
X_dflow = dflow.keep_columns([feature_1,feature_2, feature_3])
y_dflow = dflow.keep_columns("target")

X_df = X_dflow.to_pandas_dataframe()
y_df = y_dflow.to_pandas_dataframe()

X_train, X_test, y_train, y_test = train_test_split(X_df, y_df, test_size=0.2, random_state=223)

# flatten y_train to 1d array
y_train.values.flatten()

#configure 
automated_ml_config = AutoMLConfig(task = 'regression',
                               X = X_train.values,  
                   y = y_train.values.flatten(),
                   iterations = 30,
                       Primary_metric = "AUC_weighted",
                       n_cross_validation = 5
                       )

```

Als u geen tussenliggende gegevens voorbereidings stappen nodig hebt, zoals in het voor gaande voor beeld, kunt u uw `AutoMLConfig`gegevens stroom rechtstreeks door geven naar.

```Python
automated_ml_config = AutoMLConfig(task = 'regression', 
                   X = X_dflow,   
                   y = y_dflow, 
                   iterations = 30, 
                   Primary_metric = "AUC_weighted",
                   n_cross_validation = 5
                   )
```

## <a name="next-steps"></a>Volgende stappen
* Zie het [overzicht](https://aka.ms/data-prep-sdk) van de SDK voor ontwerp patronen en gebruiks voorbeelden 
* Raadpleeg de [zelf studie](tutorial-auto-train-models.md) geautomatiseerde machine learning voor een regressie model-voor beeld
