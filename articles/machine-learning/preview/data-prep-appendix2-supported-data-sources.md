---
title: Ondersteunde gegevensbronnen die beschikbaar zijn met Azure Machine Learning gegevens voorbereiden | Microsoft Docs
description: Dit document bevat een volledige lijst met ondersteunde gegevensbronnen beschikbaar voor Azure Machine Learning gegevens voorbereiden
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: 1ef4c5c33d98cfeb566e8fe23bda9e0d3f041781
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="supported-data-sources-for-this-release"></a>Ondersteunde gegevensbronnen voor deze release 
Het volgende document bevat een overzicht van de lijst met ondersteunde gegevensbronnen in Azure Machine Learning gegevens voorbereiden.

De ondersteunde gegevensbronnen voor deze release zijn als volgt.

## <a name="types"></a>Typen 
### <a name="directory-versus-file"></a>Directory versus bestand
*Bestanden/mappen*: één bestand kiezen en lezen in het voorbereiden van gegevens. Het bestandstype wordt geparseerd om te bepalen van de standaardparameters voor de bestand-verbinding die is op het volgende scherm. Kies een directory of een set bestanden in een map (de bestandskiezer is meervoudige selectie). Een benadering resultaten in de bestanden wordt gelezen als een enkel gegevensstroomblok met de bestanden die zijn toegevoegd aan elkaar (met kopteksten verwijderd, indien nodig).

De bestandstypen zijn als volgt:
- Gescheiden (CSV, .tsv, txt, enzovoort) 
- Vaste breedte
- Tekst zonder opmaak
- JSON-bestand

### <a name="csv-file"></a>CSV-bestand
Hiermee wordt een CSV-bestand gelezen uit de opslag.

#### <a name="options"></a>Opties
- Scheidingsteken
- Opmerking
- Headers
- Decimaalteken
- Bestandscodering
- Regels over te slaan

### <a name="tsv-file"></a>TSV bestand
Een bestand TSV waarden leest uit de opslag.

#### <a name="options"></a>Opties
- Opmerking
- Headers
- Bestandscodering
- Regels over te slaan

### <a name="excel-xlsxlsx"></a>Excel (.xls/.xlsx)
Een Excel-bestand, een blad tegelijk leest door bladnaam of nummer te geven.

#### <a name="options"></a>Opties
- Blad naam/nummer
- Headers
- Regels over te slaan

### <a name="json-file"></a>JSON-bestand
Een JSON-bestand gelezen uit de opslag. Houd er rekening mee dat het bestand "afgevlakt" bij lezen.

#### <a name="options"></a>Opties
Geen

### <a name="parquet"></a>Parketvloeren
Lees een gegevensset parketvloeren ofwel een enkel bestand of map.

Parketvloeren als een indeling verschillende vormen in de opslag aannemen kan. Een bestand één .parquet wordt soms gebruikt voor kleinere gegevenssets. Verschillende Python-bibliotheken ondersteuning voor lezen of schrijven naar één .parquet-bestand. Azure Machine Learning-Workbench is op dit moment is afhankelijk van de PyArrow Python-bibliotheek voor het lezen van parketvloeren tijdens het gebruik van lokale interactieve. Het ondersteunt één .parquet-bestanden (zo lang ze zijn geschreven als zodanig niet als onderdeel van een grotere gegevensset). Het ondersteunt ook parketvloeren gegevenssets. 

Een gegevensset parketvloeren is een verzameling van meer dan één .parquet-bestand, die elk een kleinere partitie van een grotere gegevensset vertegenwoordigen. Gegevenssets zijn gewoonlijk opgenomen in een map. De indeling standaard parketvloeren uitvoer voor algemene platforms zoals Spark en Hive zijn.

>[!NOTE]
>Wanneer u parketvloeren gegevens die zich in een map met meerdere .parquet bestanden leest, is het veiligste selecteren de map voor lezen en maatstreepjes de **parketvloeren gegevensset** optie. Hierdoor kunt u de hele map in plaats van de afzonderlijke bestanden lezen PyArrow. Dit zorgt ervoor ondersteuning voor het lezen van gecompliceerdere manieren voor het opslaan van parketvloeren op schijf (zoals de map partitioneren.)

Uitvoering van de scale-out is afhankelijk van de Spark parketvloeren lezen van de mogelijkheden en ondersteunt afzonderlijke bestanden en mappen.

#### <a name="options"></a>Opties
*Parketvloeren gegevensset*: deze optie wordt bepaald als Azure Machine Learning Workbench de unticked modus of in de modus ticked gebruikt. De unticked modus wordt een bepaalde directory uitgebreid en probeert dan te lezen van elk bestand in deze afzonderlijk. De modus voor ticked de map beschouwd als de volledige set gegevens en PyArrow kunt nagaan wat de beste manier om de bestanden worden geïnterpreteerd.


## <a name="locations"></a>Locaties
### <a name="local"></a>Lokaal
Lokale harde schijf of locatie voor de opslag toegewezen netwerkpad.

### <a name="azure-blob-storage"></a>Azure Blob Storage
Een Azure-abonnement vereist.

