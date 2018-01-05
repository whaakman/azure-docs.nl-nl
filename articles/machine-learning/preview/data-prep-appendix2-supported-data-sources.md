---
title: Ondersteunde gegevensbronnen beschikbaar met voorbereiden van gegevens van Azure Machine Learning | Microsoft Docs
description: Dit document bevat een volledige lijst met ondersteunde gegevensbronnen beschikbaar voor het voorbereiden van gegevens van Azure Machine Learning.
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: 32707a8089abef6caebedea168f5891161b1b480
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="supported-data-sources-for-azure-machine-learning-data-preparation"></a>Ondersteunde gegevensbronnen voor Azure Machine Learning gegevens voorbereiden 
In dit artikel bevat een overzicht van de momenteel ondersteunde gegevensbronnen voor Azure Machine Learning gegevens voorbereiden.

De ondersteunde gegevensbronnen voor deze release zijn als volgt.

## <a name="types"></a>Typen 

### <a name="sql-server"></a>SQL Server
Lezen van on-premises SQL server of Azure SQL-database.

#### <a name="options"></a>Opties
- Serveradres
- Servers (Even wanneer het certificaat op de server niet geldig is. Wees voorzichtig)
- Verificatietype (Windows, Server)
- Gebruikersnaam
- Wachtwoord
- Verbinding maken met database
- SQL-Query

#### <a name="notes"></a>Opmerkingen
- SQL-variant-kolommen worden niet ondersteund
- Time-kolom wordt geconverteerd naar datetime door toe te voegen tijd uit de database tot datum 1970/1/1
- Uitgevoerd op Spark-cluster, alle gegevens van de bijbehorende kolommen (date, datetime, datetime2, datetimeoffset) onjuiste waarden voor datums vóór 1583 evalueren
- Waarden in decimale kolommen mogelijk minder nauwkeurig vanwege conversie naar decimaal

### <a name="directory-vs-file"></a>Directory versus bestand
Kies één bestand en het in het voorbereiden van gegevens gelezen. Het bestandstype wordt geparseerd om te bepalen van de standaardparameters voor de bestand-verbinding op het volgende scherm wordt weergegeven.

Kies een map of een aantal bestanden in een map (de bestandskiezer is meervoudige selectie). Met de methode worden de bestanden worden gelezen als een enkele gegevensstroom en worden toegevoegd aan elkaar met kopteksten verwijderd, indien nodig.

De ondersteunde bestandstypen zijn:
- Gescheiden (CSV, .tsv, txt, enz.)
- Vaste breedte
- Tekst zonder opmaak
- JSON-bestand

### <a name="csv-file"></a>CSV-bestand
Een bestand met door komma's gescheiden waarden lezen uit de opslag.

#### <a name="options"></a>Opties
- Scheidingsteken
- Opmerking
- Kopteksten
- Decimaalteken
- Bestandscodering
- Regels over te slaan

### <a name="tsv-file"></a>TSV bestand
Een bestand tabblad's gescheiden waarden lezen uit de opslag.

#### <a name="options"></a>Opties
- Opmerking
- Kopteksten
- Bestandscodering
- Regels over te slaan

### <a name="excel-xlsxlsx"></a>Excel (.xls/.xlsx)
Een Excel-bestand een blad tegelijk lezen door bladnaam of nummer te geven.

#### <a name="options"></a>Opties
- Bladnaam of een cijfer
- Kopteksten
- Regels over te slaan

### <a name="json-file"></a>JSON-bestand
Een JSON-bestand gelezen uit de opslag. Het bestand wordt "afgevlakt" bij lezen.

#### <a name="options"></a>Opties
- Geen

### <a name="parquet"></a>Parketvloeren
Lees een gegevensset parketvloeren ofwel één bestand of map.

Parketvloeren als een indeling verschillende vormen in de opslag aannemen kan. Een bestand één .parquet wordt soms gebruikt voor kleinere gegevenssets. Verschillende Python-bibliotheken ondersteuning voor lezen of schrijven naar één .parquet bestanden. Momenteel worden gegevens voorbereiden voor Azure Machine Learning afhankelijk van de PyArrow Python-bibliotheek voor het lezen van parketvloeren tijdens het gebruik van lokale interactieve. Het ondersteunt één .parquet-bestanden (zolang ze zijn geschreven als zodanig en niet als onderdeel van een grotere set gegevens), evenals parketvloeren gegevenssets.

Een gegevensset parketvloeren is een verzameling van meer dan één .parquet-bestand, die elk een kleinere partitie van een grotere set gegevens vertegenwoordigen. Gegevenssets gewoonlijk zijn opgenomen in een map en de indeling standaard parketvloeren uitvoer voor platforms zoals Spark en Hive zijn.

>[!NOTE]
>Wanneer u parketvloeren gegevens die zich in een map met meerdere .parquet bestanden leest, is het veiligste selecteren de map voor lezen, en de **parketvloeren gegevensset** optie. Hierdoor kunt u de hele map in plaats van de afzonderlijke bestanden lezen PyArrow. Dit zorgt ervoor ondersteuning voor het ingewikkelder manieren van het parketvloeren opslaan op schijf, zoals het partitioneren van de map lezen.

Uitvoering van de scale-out is afhankelijk van de Spark parketvloeren lezen van de mogelijkheden en ondersteunt afzonderlijke bestanden en mappen, vergelijkbaar met lokale interactief gebruik.

#### <a name="options"></a>Opties
- Parketvloeren gegevensset. Deze optie wordt bepaald of gegevens voorbereiden voor Azure Machine Learning wordt een bepaalde directory uitgebreid en probeert dan te lezen van elk bestand afzonderlijk (de niet-geselecteerde modus), of of de map worden beschouwd als de volledige set gegevens (de geselecteerde modus). Met de geselecteerde modus kiest PyArrow de beste manier om de bestanden worden geïnterpreteerd.


## <a name="locations"></a>Locaties
### <a name="local"></a>Lokaal
Een lokale vaste schijf of een toegewezen netwerkstation opslaglocatie.

### <a name="sql-server"></a>SQL Server
On-premises SQL Server gebruikt, of Azure SQL-database.

### <a name="azure-blob-storage"></a>Azure Blob Storage
Azure Blob storage, een Azure-abonnement vereist.

