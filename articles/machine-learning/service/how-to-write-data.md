---
title: Schrijven van gegevens met de Azure Machine Learning Data Prep SDK - Python
description: Meer informatie over het schrijven van gegevens met Azure Machine Learning Data Prep SDK. U kunt schrijven van gegevens op elk gewenst moment in een gegevensstroom en bestanden in elk van onze ondersteunde locaties (lokale bestandssysteem, Azure Blob Storage en Azure Data Lake Storage).
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: da5823473b7f69fa0a6c65d5ea7319bfd2e92720
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946762"
---
# <a name="write-data-with-the-azure-machine-learning-data-prep-sdk"></a>Schrijven van gegevens met de Azure Machine Learning Data Prep SDK
U kunt schrijven van gegevens op elk gewenst moment in een gegevensstroom. Deze schrijfbewerkingen worden toegevoegd als stappen om de resulterende gegevensstroom en worden uitgevoerd telkens wanneer de gegevensstroom is. Aangezien er geen beperkingen voor het aantal schrijven stappen er zijn in een pijplijn, is het eenvoudig tussenliggende resultaten voor het oplossen van schrijven of om te worden opgehaald door andere pijplijnen. Het is belangrijk te weten dat de uitvoering van elke stap schrijven in een volledige pull van de gegevens in de gegevensstroom resulteert. Een gegevensstroom met drie stappen voor schrijven wordt bijvoorbeeld lezen en te verwerken van elke record in de gegevensset drie keer.

## <a name="writing-to-files"></a>Schrijven naar bestanden
Met de [SDK van Azure Machine Learning Data Prep](https://docs.microsoft.com/python/api/overview/azure/dataprep?view=azure-dataprep-py), kunt u gegevens schrijven naar bestanden in elk van onze ondersteunde locaties (lokale bestandssysteem, Azure Blob Storage en Azure Data Lake Storage). Gegevens worden naar meerdere partitiebestanden naar parallelle schrijfbewerkingen geschreven. Als een schrijfbewerking is voltooid, wordt ook een sentinel-bestand met de naam SUCCES gegenereerd. Hiermee kunt u identificeren wanneer een tussenliggende schrijven is voltooid zonder te wachten op de hele pijplijn om te voltooien.

Bij het uitvoeren van een gegevensstroom in Spark, moet u naar een lege map schrijven. bij het uitvoeren van een schrijfbewerking naar een bestaande map mislukken. Zorg ervoor dat de doelmap is leeg of gebruik een andere doellocatie voor elke uitvoering, of het schrijven mislukt.

De Azure Machine Learning Data Prep SDK ondersteunt de volgende bestandsindelingen:
-   Bestanden met scheidingstekens (CSV, TSV, enz.)
-   Parquet-bestanden

Start het laden van gegevens in een gegevensstroom voor dit voorbeeld. Er wordt opnieuw gebruikt deze gegevens met verschillende indelingen.

```

import azureml.dataprep as dprep
t = dprep.smart_read_file('./data/fixed_width_file.txt')
t = t.to_number('Column3')
t.head(10)
```   

|   |  Kolom1 |    Kolom2 | Kolom3 | Kolom4  |Column5   | Kolom6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   Geen|       NEE|     NEE  |   ENRS    |NaN    |   NaN |   NaN|    
|   1|      10003.0 |   99999.0 |   Geen|       NEE|     NEE  |   ENSO|       NaN|        NaN |NaN|   
|   2|  10010.0|    99999.0|    Geen|   NEE| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0|    99999.0|    Geen|   NEE| NEE| |   NaN|    NaN|    NaN|
|4| 10014.0|    99999.0|    Geen|   NEE| NEE| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    Geen|   NEE| NEE| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   Geen|   NEE| NEE|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    Geen|   NEE| NEE| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    Geen|   NEE| SV|     |80050.0|   16250.0|    80,0|
|9| 10030.0|    99999.0|    Geen|   NEE| SV|     |77000.0|   15500.0|    120,0|

## <a name="delimited-files"></a>Bestanden met scheidingstekens
De onderstaande regel maakt een nieuwe gegevensstroom met een stap schrijven, maar nog niet de werkelijke schrijven heeft plaatsgevonden. Wanneer de gegevensstroom wordt uitgevoerd, wordt het schrijven en uitvoeren.

```
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'))
```
De gegevensstroom, die wordt uitgevoerd de schrijfbewerking nu worden uitgevoerd.
```
write_t.run_local()

written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```
|   |  Kolom1 |    Kolom2 | Kolom3 | Kolom4  |Column5   | Kolom6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   FOUT |       NEE|     NEE  |   ENRS    |FOUT    |   FOUT |   FOUT|    
|   1|      10003.0 |   99999.0 |   FOUT |       NEE|     NEE  |   ENSO|       FOUT|        FOUT |FOUT|   
|   2|  10010.0|    99999.0|    FOUT |   NEE| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0|    99999.0|    FOUT |   NEE| NEE| |   FOUT|    FOUT|    FOUT|
|4| 10014.0|    99999.0|    FOUT |   NEE| NEE| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    FOUT |   NEE| NEE| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   FOUT |   NEE| NEE|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    FOUT |   NEE| NEE| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    FOUT |   NEE| SV|     |80050.0|   16250.0|    80,0|
|9| 10030.0|    99999.0|    FOUT |   NEE| SV|     |77000.0|   15500.0|    120,0|

De geschreven gegevens bevat verschillende fouten in de numerieke kolommen vanwege de cijfers die zijn niet juist geparseerd. Als naar CSV geschreven, worden deze null-waarden vervangen door de tekenreeks "ERROR" standaard. U kunt parameters als onderdeel van uw schrijven aanroepen en geef een tekenreeks die moet worden gebruikt voor null-waarden toevoegen.

```
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'), 
                         error='BadData',
                         na='NA')
write_t.run_local()
written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```
|   |  Kolom1 |    Kolom2 | Kolom3 | Kolom4  |Column5   | Kolom6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   BadData |       NEE|     NEE  |   ENRS    |BadData    |   BadData |   BadData|    
|   1|      10003.0 |   99999.0 |   BadData |       NEE|     NEE  |   ENSO|       BadData|        BadData |BadData|   
|   2|  10010.0|    99999.0|    BadData |   NEE| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0|    99999.0|    BadData |   NEE| NEE| |   BadData|    BadData|    BadData|
|4| 10014.0|    99999.0|    BadData |   NEE| NEE| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    BadData |   NEE| NEE| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   BadData |   NEE| NEE|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    BadData |   NEE| NEE| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    BadData |   NEE| SV|     |80050.0|   16250.0|    80,0|
|9| 10030.0|    99999.0|    BadData |   NEE| SV|     |77000.0|   15500.0|    120,0|
## <a name="parquet-files"></a>Parquet-bestanden

 Net als de `write_to_csv` functie bovenstaande `write_to_parquet` retourneert een nieuwe gegevensstroom met een schrijfbewerking Parquet stap die wordt uitgevoerd wanneer de gegevensstroom wordt uitgevoerd.

```
write_parquet_t = t.write_to_parquet(directory_path=dprep.LocalFileOutput('./test_parquet_out/'),
error='MiscreantData')
```
 We nu uitvoeren de gegevensstroom, die wordt uitgevoerd de write-bewerking.

```
write_parquet_t.run_local()

written_parquet_files = dprep.read_parquet_file('./test_parquet_out/part-*')
written_parquet_files.head(10)
```
|   |  Kolom1 |    Kolom2 | Kolom3 | Kolom4  |Column5   | Kolom6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   MiscreantData |       NEE|     NEE  |   ENRS    |MiscreantData    |   MiscreantData |   MiscreantData|    
|   1|      10003.0 |   99999.0 |   MiscreantData |       NEE|     NEE  |   ENSO|       MiscreantData|        MiscreantData |MiscreantData|   
|   2|  10010.0|    99999.0|    MiscreantData |   NEE| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0|    99999.0|    MiscreantData |   NEE| NEE| |   MiscreantData|    MiscreantData|    MiscreantData|
|4| 10014.0|    99999.0|    MiscreantData |   NEE| NEE| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    MiscreantData |   NEE| NEE| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   MiscreantData |   NEE| NEE|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    MiscreantData |   NEE| NEE| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    MiscreantData |   NEE| SV|     |80050.0|   16250.0|    80,0|
|9| 10030.0|    99999.0|    MiscreantData |   NEE| SV|     |77000.0|   15500.0|    120,0|
