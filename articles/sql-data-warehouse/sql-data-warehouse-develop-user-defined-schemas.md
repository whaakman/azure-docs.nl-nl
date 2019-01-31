---
title: Met behulp van de gebruiker gedefinieerde schema's in SQL Data Warehouse | Microsoft Docs
description: Tips voor het gebruik van T-SQL-gebruiker gedefinieerde schema's in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: ae017461767a207deae1d990980258a1f661df3d
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55468441"
---
# <a name="using-user-defined-schemas-in-sql-data-warehouse"></a>Met behulp van de gebruiker gedefinieerde schema's in SQL Data Warehouse
Tips voor het gebruik van T-SQL-gebruiker gedefinieerde schema's in Azure SQL Data Warehouse om oplossingen te ontwikkelen.

## <a name="schemas-for-application-boundaries"></a>Schema's voor de toepassingsgrenzen van de

Traditionele datawarehouses maken vaak gebruik van afzonderlijke databases te maken van de toepassingsgrenzen van de is op basis van de werkbelasting, domein of beveiliging. Een traditionele SQL Server datawarehouse kan bijvoorbeeld een staging-database, een datawarehouse-database en sommige datamart-databases. In deze topologie wordt elke database fungeert als een werkbelasting en een beveiligingsgrens in de architectuur.

Daarentegen, voert SQL Data Warehouse de gehele datawarehouse-workload binnen één database. Cross-database zijn joins niet toegestaan. Daarom SQL Data Warehouse wordt verwacht dat alle tabellen die worden gebruikt door de datawarehouse binnen één database worden opgeslagen.

> [!NOTE]
> SQL Data Warehouse biedt geen ondersteuning voor query's tussen databases van welke aard dan ook. Datawarehouse-implementaties die gebruikmaken van dit patroon wordt als gevolg hiervan moet worden herzien.
> 
> 

## <a name="recommendations"></a>Aanbevelingen
Dit zijn aanbevelingen voor de consolidatie van werkbelastingen, beveiliging, domein en functionele grenzen met behulp van de gebruiker gedefinieerde schema 's

1. Een SQL Data Warehouse-database gebruiken om uit te voeren van uw hele datawarehouse-workload
2. Consolideren van uw bestaande datawarehouse-omgeving voor het gebruik van een SQL Data Warehouse-database
3. Maak gebruik van **gebruikers gedefinieerde schema** voor de grens eerder hebt geïmplementeerd met behulp van databases.

Als de gebruiker gedefinieerde schema's zijn niet eerder is gebruikt, hebt u een schone lei. Gewoon de naam van de oude database gebruiken als basis voor de gebruiker gedefinieerde schema's in de SQL Data Warehouse-database.

Als het schema's hebben al is gebruikt, hebt u een aantal opties:

1. De schemanamen van de verouderde verwijderen en opnieuw beginnen
2. De verouderde schemanamen moeten worden behouden door vooraf in behandeling de verouderde schemanaam aan de naam van de tabel
3. De verouderde schemanamen moeten worden behouden door het implementeren van weergaven in de tabel in een extra schema om de structuur van de oude schema opnieuw te maken.

> [!NOTE]
> Optie 3 lijken op de eerste controle, zoals de meest interessante optie. De duivelse is echter in de details. Weergaven zijn in SQL Data Warehouse alleen-lezen. Wijzigingen van gegevens of de tabel moet worden uitgevoerd op basis van de basistabel. Optie 3 introduceert ook een laag van weergaven in uw systeem. Kunt u nadenken over dit enkele aanvullende als u weergaven in uw architectuur al.
> 
> 

### <a name="examples"></a>Voorbeelden:
Implementeren van de gebruiker gedefinieerde schema's op basis van de databasenamen van de

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Behouden verouderde schemanamen moeten worden weergegeven door vooraf in behandeling aan de naam van de tabel. Schema's gebruiken voor de grens van de werkbelasting.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Verouderde schemanamen moeten worden weergegeven met behulp van weergaven behouden

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [!NOTE]
> Eventuele wijzigingen in de strategie voor schema moet een overzicht van het beveiligingsmodel voor de database. In veel gevallen is het mogelijk voor het vereenvoudigen van het beveiligingsmodel machtigingen op het schemaniveau van het toe te wijzen. Als u gedetailleerdere machtigingen zijn vereist, kunt u databaserollen gebruiken.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkelaars [overzicht voor ontwikkelaars](sql-data-warehouse-overview-develop.md).

