---
title: 'Schrijven: dataprep Python-SDK'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het schrijven van gegevens met Azure Machine Learning Data Prep SDK. U kunt schrijven van gegevens op elk gewenst moment in een gegevensstroom en bestanden in elk van onze ondersteunde locaties (lokale bestandssysteem, Azure Blob Storage en Azure Data Lake Storage).
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
ms.openlocfilehash: 18c74a701da77ca06d3084a6f4af64dd4cc32628
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53186018"
---
# <a name="write-data-using-the-azure-machine-learning-data-prep-sdk"></a>Schrijven van gegevens met behulp van de Azure Machine Learning Data Prep SDK

In dit artikel leert u verschillende methoden voor het schrijven van gegevens met behulp van de Azure Machine Learning Data Prep SDK. Uitgevoerde gegevens kunnen worden geschreven op elk gewenst moment in een gegevensstroom en schrijfbewerkingen worden toegevoegd als stappen om de resulterende gegevensstroom en worden uitgevoerd telkens wanneer de gegevensstroom is. Gegevens worden naar meerdere partitiebestanden naar parallelle schrijfbewerkingen geschreven.

Aangezien er geen beperkingen voor het aantal schrijven stappen er zijn in een pijplijn, kunt u eenvoudig aanvullende schrijven stappen voor het ophalen van tussenliggende resultaten voor het oplossen van problemen of voor andere pijplijnen toevoegen.

Telkens wanneer die u een stap schrijven, uitvoeren een volledige pull van de gegevens in de gegevensstroom is opgetreden. Een gegevensstroom met drie stappen voor schrijven wordt bijvoorbeeld lezen en te verwerken van elke record in de gegevensset drie keer.

## <a name="supported-data-types-and-location"></a>Ondersteunde gegevenstypen en locatie

De volgende indelingen worden ondersteund
-   Bestanden met scheidingstekens (CSV, TSV, enz.)
-   Parquet-bestanden

Met behulp van de [gegevensvoorbereiding van Azure Machine Learning python-SDK](https://aka.ms/data-prep-sdk), kunt u gegevens te schrijven:
+ een lokaal bestandssysteem
+ Azure Blob Storage
+ Azure Data Lake Storage

## <a name="spark-considerations"></a>Overwegingen voor Spark

Bij het uitvoeren van een gegevensstroom in Spark, moet u naar een lege map schrijven. Er wordt geprobeerd om uit te voeren een schrijven met een bestaande map, resulteert dit in een fout. Zorg ervoor dat de doelmap is leeg of gebruik een andere doellocatie voor elke uitvoering, of het schrijven mislukt.

## <a name="monitoring-write-operations"></a>Bewaking van schrijfbewerkingen

Een sentinel-bestand met de naam geslaagd is voor uw gemak hebt gegenereerd als een schrijfbewerking is voltooid. De aanwezigheid kunt u identificeren wanneer een tussenliggende schrijven is voltooid zonder te wachten op de hele pijplijn om te voltooien.

## <a name="example-write-code"></a>Voorbeeld schrijven van code

Start het laden van gegevens in een gegevensstroom voor dit voorbeeld. U deze gegevens met verschillende indelingen opnieuw gebruiken.

```python
import azureml.dataprep as dprep
t = dprep.smart_read_file('./data/fixed_width_file.txt')
t = t.to_number('Column3')
t.head(10)
```

Voorbeelduitvoer:
|   |  Kolom1 |    Kolom2 | Kolom3 | Kolom4  |Column5   | Kolom6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   Geen|       NO|     NO  |   ENRS    |NaN    |   NaN |   NaN|    
|   1|      10003.0 |   99999.0 |   Geen|       NO|     NO  |   ENSO|       NaN|        NaN |NaN|   
|   2|  10010.0|    99999.0|    Geen|   NO| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0|    99999.0|    Geen|   NO| NO| |   NaN|    NaN|    NaN|
|4| 10014.0|    99999.0|    Geen|   NO| NO| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    Geen|   NO| NO| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   Geen|   NO| NO|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    Geen|   NO| NO| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    Geen|   NO| SV|     |80050.0|   16250.0|    80,0|
|9| 10030.0|    99999.0|    Geen|   NO| SV|     |77000.0|   15500.0|    120,0|

### <a name="delimited-file-example"></a>Voorbeeld van een bestand met scheidingstekens

De volgende code gebruikt de `write_to_csv` functie voor het schrijven van gegevens naar een bestand met scheidingstekens.

```python
# Create a new data flow using `write_to_csv` 
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'))

# Run the data flow to begin the write operation.
write_t.run_local()

written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```

Voorbeelduitvoer:
|   |  Kolom1 |    Kolom2 | Kolom3 | Kolom4  |Column5   | Kolom6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   FOUT |       NO|     NO  |   ENRS    |FOUT    |   FOUT |   FOUT|    
|   1|      10003.0 |   99999.0 |   FOUT |       NO|     NO  |   ENSO|       FOUT|        FOUT |FOUT|   
|   2|  10010.0|    99999.0|    FOUT |   NO| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0|    99999.0|    FOUT |   NO| NO| |   FOUT|    FOUT|    FOUT|
|4| 10014.0|    99999.0|    FOUT |   NO| NO| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    FOUT |   NO| NO| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   FOUT |   NO| NO|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    FOUT |   NO| NO| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    FOUT |   NO| SV|     |80050.0|   16250.0|    80,0|
|9| 10030.0|    99999.0|    FOUT |   NO| SV|     |77000.0|   15500.0|    120,0|

In de uitvoer van de voorgaande weergegeven verschillende fouten in de numerieke kolommen vanwege de cijfers die zijn niet juist geparseerd. Wanneer geschreven naar CSV, null-waarden vervangen door de tekenreeks "ERROR" standaard.

Parameters als onderdeel van uw schrijven aanroepen en geef een tekenreeks die moet worden gebruikt voor null-waarden toevoegen.

```python
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'), 
                         error='BadData',
                         na='NA')
write_t.run_local()
written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```

De bovenstaande code wordt deze uitvoer gegenereerd:
|   |  Kolom1 |    Kolom2 | Kolom3 | Kolom4  |Column5   | Kolom6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   BadData |       NO|     NO  |   ENRS    |BadData    |   BadData |   BadData|    
|   1|      10003.0 |   99999.0 |   BadData |       NO|     NO  |   ENSO|       BadData|        BadData |BadData|   
|   2|  10010.0|    99999.0|    BadData |   NO| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0|    99999.0|    BadData |   NO| NO| |   BadData|    BadData|    BadData|
|4| 10014.0|    99999.0|    BadData |   NO| NO| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    BadData |   NO| NO| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   BadData |   NO| NO|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    BadData |   NO| NO| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    BadData |   NO| SV|     |80050.0|   16250.0|    80,0|
|9| 10030.0|    99999.0|    BadData |   NO| SV|     |77000.0|   15500.0|    120,0|

### <a name="parquet-file-example"></a>Voorbeeld van een parquet

Vergelijkbaar met `write_to_csv`, wordt de `write_to_parquet` functie geeft als resultaat een nieuwe gegevensstroom met een schrijfbewerking Parquet stap die wordt uitgevoerd wanneer de gegevensstroom wordt uitgevoerd.

```python
write_parquet_t = t.write_to_parquet(directory_path=dprep.LocalFileOutput('./test_parquet_out/'),
error='MiscreantData')
```

De gegevensstroom voor het starten van de write-bewerking uitvoeren.

```python
write_parquet_t.run_local()

written_parquet_files = dprep.read_parquet_file('./test_parquet_out/part-*')
written_parquet_files.head(10)
```

De bovenstaande code wordt deze uitvoer gegenereerd:
|   |  Kolom1 |    Kolom2 | Kolom3 | Kolom4  |Column5   | Kolom6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   MiscreantData |       NO|     NO  |   ENRS    |MiscreantData    |   MiscreantData |   MiscreantData|    
|   1|      10003.0 |   99999.0 |   MiscreantData |       NO|     NO  |   ENSO|       MiscreantData|        MiscreantData |MiscreantData|   
|   2|  10010.0|    99999.0|    MiscreantData |   NO| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0|    99999.0|    MiscreantData |   NO| NO| |   MiscreantData|    MiscreantData|    MiscreantData|
|4| 10014.0|    99999.0|    MiscreantData |   NO| NO| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    MiscreantData |   NO| NO| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   MiscreantData |   NO| NO|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    MiscreantData |   NO| NO| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    MiscreantData |   NO| SV|     |80050.0|   16250.0|    80,0|
|9| 10030.0|    99999.0|    MiscreantData |   NO| SV|     |77000.0|   15500.0|    120,0|
