---
title: Uw schema migreren naar SQL Data Warehouse | Microsoft Docs
description: Tips voor het migreren van uw schema naar Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 538b60c9-a07f-49bf-9ea3-1082ed6699fb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: 07ca2321852e276502187e768177e7e82bdfd080
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-your-schemas-to-sql-data-warehouse"></a>Uw schema's migreren naar SQL Data Warehouse
Richtlijnen voor het migreren van uw SQL-schema's met SQL Data Warehouse. 

## <a name="plan-your-schema-migration"></a>De schema-migratie plannen

Als u van plan een migratie bent, Zie de [tabel overzicht] [ table overview] om vertrouwd te raken met overwegingen bij het ontwerpen van tabel zoals statistieken, distributie, partitioneren en indexeren.  Ook worden enkele [niet-ondersteunde tabelfuncties] [ unsupported table features] en de tijdelijke oplossingen.

## <a name="use-user-defined-schemas-to-consolidate-databases"></a>Gebruiker gedefinieerde schema's gebruiken om te consolideren databases

De werkbelasting van uw bestaande heeft waarschijnlijk meer dan één database. Een datawarehouse van SQL Server kan bijvoorbeeld een faseringsdatabase, een datawarehouse-database en sommige gegevens datamart-databases. In deze topologie wordt elke database uitgevoerd als een afzonderlijke werkbelasting met afzonderlijke beveiligingsbeleid.

SQL Data Warehouse wordt daarentegen de hele datawarehouse-workload binnen één database uitgevoerd. Joins zijn niet toegestaan cross-database. SQL Data Warehouse verwacht daarom alle tabellen die door het datawarehouse gebruikt om te worden opgeslagen in een database.

U wordt aangeraden de gebruiker gedefinieerde schema's gebruiken om te consolideren van uw bestaande werkbelasting in een database. Zie voor voorbeelden [gebruiker gedefinieerde schema's](sql-data-warehouse-develop-user-defined-schemas.md)

## <a name="use-compatible-data-types"></a>Compatibele gegevenstypen gebruiken
Wijzig de gegevenstypen compatibel zijn met SQL Data Warehouse. Zie voor een lijst van ondersteunde en niet-ondersteunde gegevenstypen [gegevenstypen][data types]. Dit onderwerp biedt tijdelijke oplossingen voor de niet-ondersteunde typen. Het bevat ook een query voor het identificeren van bestaande typen die niet worden ondersteund in SQL Data Warehouse.

## <a name="minimize-row-size"></a>Rijgrootte minimaliseren
Minimaliseer de rijlengte van de tabellen voor de beste prestaties. Omdat korter rij lengten tot betere prestaties leiden, gebruikt u de kleinste gegevenstypen die werken voor uw gegevens. 

PolyBase heeft voor de breedte van de rij tabel, een limiet van 1 MB.  Als u van plan bent om gegevens te laden in SQL Data Warehouse met PolyBase, moet u uw tabellen om rij maximale breedte van minder dan 1 MB bijwerken. 

<!--
- For example, this table uses variable length data but the largest possible size of the row is still less than 1 MB. PolyBase will load data into this table.

- This table uses variable length data and the defined row width is less than one MB. When loading rows, PolyBase allocates the full length of the variable-length data. The full length of this row is greater than one MB.  PolyBase will not load data into this table.  

-->

## <a name="specify-the-distribution-option"></a>Geef de distributie-optie
SQL Data Warehouse is een gedistribueerde database-systeem. Elke tabel is gedistribueerd of gerepliceerd naar de rekenknooppunten. Er is een tabeloptie waarmee u kunt opgeven hoe de gegevens te distribueren. De opties zijn round-robin wordt gerepliceerd, of hash gedistribueerd. Elk heeft voor- en nadelen. Als u de optie voor distributie niet opgeeft, gebruikt SQL Data Warehouse round robin als de standaardwaarde.

- Round robin is de standaardinstelling. Het is het eenvoudigst te gebruiken en laadt de gegevens zo snel mogelijk, maar joins waarvoor gegevensverplaatsing waardoor queryprestaties wordt vertraagd.
- Gerepliceerde winkels een kopie van de tabel op elk rekenknooppunt. Gerepliceerde tabellen zijn zodat omdat ze geen verplaatsing van gegevens voor samenvoegingen en aggregaties behoeven. Extra opslag vereist, hetgeen daarom het meest geschikt voor kleinere tabellen.
- De rijen verdeelt hash gedistribueerd over alle knooppunten via een hash-functie. Gedistribueerd hash-tabellen zijn de kern van SQL Data Warehouse omdat ze zijn ontworpen voor hoge queryprestaties op grote tabellen. Deze optie vereist een aantal planning om de aanbevolen kolom waarop u wilt distribueren van de gegevens te selecteren. Echter, als u niet de aanbevolen kolom voor het eerst kiest, kunt u eenvoudig opnieuw distribueren de gegevens op een andere kolom. 

Als u de beste optie distributiepunten voor elke tabel, Zie [tabellen gedistribueerd](sql-data-warehouse-tables-distribute.md).


## <a name="next-steps"></a>Volgende stappen
Nadat u uw databaseschema is gemigreerd naar SQL Data Warehouse, gaat u verder met een van de volgende artikelen:

* [Uw gegevens migreren][Migrate your data]
* [Uw code migreren][Migrate your code]

Zie voor meer informatie over aanbevolen procedures voor SQL Data Warehouse, de [aanbevolen procedures] [ best practices] artikel.

<!--Image references-->

<!--Article references-->
[Migrate your code]: ./sql-data-warehouse-migrate-code.md
[Migrate your data]: ./sql-data-warehouse-migrate-data.md
[best practices]: ./sql-data-warehouse-best-practices.md
[table overview]: ./sql-data-warehouse-tables-overview.md
[unsupported table features]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[data types]: ./sql-data-warehouse-tables-data-types.md
[unsupported data types]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types

<!--MSDN references-->


<!--Other Web references-->
