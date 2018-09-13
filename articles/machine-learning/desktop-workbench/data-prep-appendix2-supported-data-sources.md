---
title: Ondersteunde gegevensbronnen beschikbaar voor gegevensvoorbereiding van Azure Machine Learning | Microsoft Docs
description: Dit document bevat een volledige lijst met ondersteunde gegevensbronnen beschikbaar voor gegevensvoorbereiding van Azure Machine Learning.
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 88ed4fa43e5724cfe1d6f1555db947d77045cd2e
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35646469"
---
# <a name="supported-data-sources-for-azure-machine-learning-data-preparation"></a>Ondersteunde gegevensbronnen voor gegevensvoorbereiding van Azure Machine Learning 
In dit artikel bevat een overzicht van de momenteel ondersteunde gegevensbronnen voor gegevensvoorbereiding van Azure Machine Learning.

De ondersteunde gegevensbronnen voor deze release zijn als volgt.

## <a name="types"></a>Typen 

### <a name="sql-server"></a>SQL Server
Lezen van on-premises SQL server of Azure SQL-database.

#### <a name="options"></a>Opties
- Serveradres
- Vertrouwen van de Server (zelfs wanneer het certificaat op de server niet geldig is. Wees voorzichtig)
- Verificatietype (Windows, Server)
- Gebruikersnaam
- Wachtwoord
- Database verbinding maken met
- SQL-Query

#### <a name="notes"></a>Opmerkingen
- SQL-variant kolommen worden niet ondersteund
- Time-kolom wordt geconverteerd naar datum/tijd door toe te voegen van de tijd van de database op datum 1970/1/1
- Uitgevoerd op Apache Spark-cluster, alle gegevens van de bijbehorende kolommen (datum, datum/tijd, datetime2, datetimeoffset) onjuiste waarden voor datums voordat 1583 evalueren
- Waarden in decimale kolommen mogelijk minder nauwkeurig vanwege de conversie naar een decimaal

### <a name="directory-vs-file"></a>Map en bestand
Kies een enkel bestand en lezen in de gegevens voor te bereiden. Het bestandstype wordt geparseerd om te bepalen van de standaardparameters voor de verbinding van de bestanden in het volgende scherm wordt weergegeven.

Kies een map of een set van bestanden in een map (de bestandenkiezer is meervoudige selectie). Met beide benaderingen, worden de bestanden worden gelezen als een enkel gegevensstroom en worden toegevoegd aan elkaar worden verbonden, met de headers verwijderd, indien nodig.

De ondersteunde bestandstypen zijn:
- Gescheiden (CSV, .tsv, txt, enz.)
- Vaste breedte
- Tekst zonder opmaak
- JSON-bestand

### <a name="csv-file"></a>CSV-bestand
Leest een bestand met door komma's gescheiden waarden uit de opslag.

#### <a name="options"></a>Opties
- Scheidingsteken
- Opmerking
- Headers
- Decimaalteken
- Bestandscodering
- Regels om over te slaan

### <a name="tsv-file"></a>TSV-bestand
Leest een bestand tabblad gescheiden waarden uit de opslag.

#### <a name="options"></a>Opties
- Opmerking
- Headers
- Bestandscodering
- Regels om over te slaan

### <a name="excel-xlsxlsx"></a>Excel (.xls/.xlsx)
Een Excel-werkblad een op het bestand op een tijdstip gelezen door het de bladnaam of nummer op te geven.

#### <a name="options"></a>Opties
- Bladnaam of nummer
- Headers
- Regels om over te slaan

### <a name="json-file"></a>JSON-bestand
Lees een JSON-bestand uit de opslag. Het bestand is "afgevlakt" op lezen.

#### <a name="options"></a>Opties
- Geen

### <a name="parquet"></a>Parquet
Lees een verzameling Parquet, ofwel een enkel bestand of een map.

Parquet als een indeling verschillende vormen in de opslag aannemen kan. Voor kleinere gegevenssets, wordt een bestand één .parquet soms gebruikt. Verschillende Python-bibliotheken ondersteuning voor lezen of schrijven naar één .parquet bestanden. Dit moment is gegevensvoorbereiding van Azure Machine Learning afhankelijk van de PyArrow Python-clientbibliotheek voor het lezen van Parquet tijdens lokale interactief gebruik. Deze biedt ondersteuning voor één .parquet bestanden (zolang ze zijn geschreven als zodanig en niet als onderdeel van een grotere set gegevens), evenals Parquet-gegevenssets.

Een gegevensset Parquet is een verzameling van meer dan één .parquet-bestand, die elk een kleinere partitie van een grotere set gegevens vertegenwoordigt. Data-sets gewoonlijk zijn opgenomen in een map en zijn de standaard parquet-uitvoerindeling voor platforms als Spark en Hive.

>[!NOTE]
>Wanneer u Parquet-gegevens die zich in een map met meerdere .parquet bestanden leest, is het verstandig om te selecteren van de map om te lezen, en de **Parquet-gegevensset** optie. Hierdoor kunt u PyArrow lezen van de hele map in plaats van de afzonderlijke bestanden. Dit zorgt ervoor ondersteuning voor het lezen van complexe berekeningen manieren van het Parquet opslaan op schijf, zoals het partitioneren van de map.

Uitvoering van de scale-out is afhankelijk van de Spark-Parquet lezen van de mogelijkheden en biedt ondersteuning voor afzonderlijke bestanden en mappen, vergelijkbaar met lokale interactief gebruik.

#### <a name="options"></a>Opties
- Parquet-gegevensset. Deze optie bepaalt u of gegevensvoorbereiding van Azure Machine Learning gaat verder in een bepaalde directory en probeert te lezen van elk bestand afzonderlijk (de niet-geselecteerde modus), of of de map wordt beschouwd als de volledige set gegevens (de geselecteerde modus). Met de geselecteerde modus kiest PyArrow de beste manier om de bestanden worden geïnterpreteerd.


## <a name="locations"></a>Locaties
### <a name="local"></a>Lokaal
Een lokale vaste schijf of een toegewezen netwerkstation opslaglocatie.

### <a name="sql-server"></a>SQL Server
On-premises SQL Server, of Azure SQL-database.

### <a name="azure-blob-storage"></a>Azure Blob Storage
Azure Blob storage, waarmee een Azure-abonnement vereist.

