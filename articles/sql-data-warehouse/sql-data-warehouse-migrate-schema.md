---
title: Uw schema migreren naar SQL Data Warehouse | Microsoft Docs
description: Tips voor het migreren van uw schema naar Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: jrowlandjones
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: jrj
ms.reviewer: igorstan
ms.openlocfilehash: 51ad7eed0bf37194b1e5ff2c605b39246e9a1191
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43301179"
---
# <a name="migrate-your-schemas-to-sql-data-warehouse"></a>Uw schema migreren naar SQL Data Warehouse
Richtlijnen voor het migreren van uw SQL-schema's met SQL Data Warehouse. 

## <a name="plan-your-schema-migration"></a>De schemamigratie plannen

Als u van plan een migratie bent, Zie de [tabeloverzicht] [ table overview] om vertrouwd te raken met overwegingen bij het ontwerpen van tabel, zoals statistieken, distributie, partitionering en indexeren.  Het bevat ook enkele [niet-ondersteunde tabelfuncties] [ unsupported table features] en hun oplossingen.

## <a name="use-user-defined-schemas-to-consolidate-databases"></a>Gebruiker gedefinieerde schema's gebruiken om te consolideren databases

Uw bestaande workload heeft waarschijnlijk meer dan één database. Een SQL Server datawarehouse kan bijvoorbeeld een staging-database, een datawarehouse-database en sommige datamart-databases. In deze topologie wordt elke database uitgevoerd als een afzonderlijke workload met behulp van afzonderlijke beveiligingsbeleid.

Daarentegen, voert SQL Data Warehouse de gehele datawarehouse-workload binnen één database. Cross-database zijn joins niet toegestaan. Daarom SQL Data Warehouse wordt verwacht dat alle tabellen die worden gebruikt door het datawarehouse binnen één database worden opgeslagen.

U wordt aangeraden met behulp van de gebruiker gedefinieerde schema's om te consolideren van uw bestaande werkbelasting in een database. Zie voor voorbeelden van [gebruiker gedefinieerde schema's](sql-data-warehouse-develop-user-defined-schemas.md)

## <a name="use-compatible-data-types"></a>Compatibele gegevenstypen gebruiken
Wijzig de gegevenstypen voor compatibiliteit met SQL Data Warehouse. Zie voor een lijst van ondersteunde en niet-ondersteunde gegevenstypen, [gegevenstypen][data types]. Dit onderwerp biedt oplossingen voor de niet-ondersteunde typen. Het biedt ook een query voor het identificeren van bestaande typen die niet worden ondersteund in SQL Data Warehouse.

## <a name="minimize-row-size"></a>Rijgrootte minimaliseren
Minimaliseer de rijlengte van uw tabellen voor de beste prestaties. Aangezien kortere rij lengtes tot betere prestaties leiden, gebruikt u de kleinste gegevenstypen die werken voor uw gegevens. 

PolyBase is voor de breedte van de rij tabel, een limiet van 1 MB.  Als u van plan bent om gegevens te laden in SQL Data Warehouse met PolyBase, werkt u uw tabellen voor het maximumaantal rijen breedte van minder dan 1 MB. 

<!--
- For example, this table uses variable length data but the largest possible size of the row is still less than 1 MB. PolyBase will load data into this table.

- This table uses variable length data and the defined row width is less than one MB. When loading rows, PolyBase allocates the full length of the variable-length data. The full length of this row is greater than one MB.  PolyBase will not load data into this table.  

-->

## <a name="specify-the-distribution-option"></a>De distributieoptie opgeven
SQL Data Warehouse is een gedistribueerde database-systeem. Elke tabel is gedistribueerd of gerepliceerd naar de rekenknooppunten. Er is een tabeloptie waarmee u kunt opgeven hoe de gegevens wilt verdelen. De opties zijn round-robin wordt gerepliceerd, of hash gedistribueerd. Elk heeft voor- en nadelen. Als u de optie-distributie niet opgeeft, gebruikt SQL Data Warehouse round robin als de standaardwaarde.

- Round robin is de standaardinstelling. Het is het eenvoudigst te gebruiken en laadt de gegevens zo snel mogelijk, maar joins moet verplaatsing van gegevens die prestaties van query's wordt vertraagd.
- Gerepliceerde slaat een kopie van de tabel op elk knooppunt. Gerepliceerde tabellen zijn goed presterende omdat ze niet verplaatsen van gegevens voor samenvoegingen en aggregaties hoeven. Deze extra opslagruimte nodig, en daarom het meest geschikt voor kleinere tabellen.
- De rijen verdeelt hash gedistribueerd over alle knooppunten via een hash-functie. Gedistribueerde hashtabellen zijn de kern van SQL Data Warehouse, omdat ze zijn ontworpen voor hoge queryprestaties op grote tabellen. Deze optie vereist sommige plannen om de aanbevolen kolom waarop u wilt distribueren van de gegevens te selecteren. Echter, als u niet de aanbevolen kolom de eerste keer kiest, kunt u eenvoudig opnieuw distribueren de gegevens op een andere kolom. 

Als u de beste optie voor distributie voor elke tabel, Zie [gedistribueerde tabellen](sql-data-warehouse-tables-distribute.md).


## <a name="next-steps"></a>Volgende stappen
Nadat u hebt uw databaseschema is gemigreerd naar SQL Data Warehouse, gaat u verder met een van de volgende artikelen:

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
