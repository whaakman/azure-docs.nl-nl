---
title: Gebruiker gedefinieerde schema's in SQL Data Warehouse | Microsoft Docs
description: Tips voor het gebruik van Transact-SQL-schema's in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 52af5bd5-d5d3-4f9b-8704-06829fb924e3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: dfb58956ad6637cf0f50b4c052ab98fb7c26139d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="user-defined-schemas-in-sql-data-warehouse"></a>Gebruiker gedefinieerde schema's in SQL Data Warehouse
Afzonderlijke databases gebruik traditionele datawarehouses vaak te maken van de toepassingsgrenzen op basis van de werkbelasting, domein of beveiliging. Een traditionele SQL Server datawarehouse kan bijvoorbeeld een faseringsdatabase, een datawarehouse-database en sommige gegevens datamart-databases. In deze topologie wordt elke database uitgevoerd als een werkbelasting en de beveiligingsgrens in de architectuur.

SQL Data Warehouse wordt daarentegen de hele datawarehouse-workload binnen één database uitgevoerd. Joins zijn niet toegestaan cross-database. SQL Data Warehouse verwacht daarom alle tabellen die door het datawarehouse gebruikt om te worden opgeslagen in een database.

> [!NOTE]
> SQL Data Warehouse biedt geen ondersteuning voor meerdere databasequery's van elk type. Als gevolg daarvan kan moet datawarehouse-implementaties die gebruikmaken van dit patroon worden herzien.
> 
> 

## <a name="recommendations"></a>Aanbevelingen
Dit zijn aanbevelingen voor de consolidatie van werkbelastingen, beveiliging, domein en functionele grenzen met behulp van de gebruiker gedefinieerde schema

1. Een SQL Data Warehouse-database gebruiken om uit te voeren van uw hele datawarehouse-workload
2. Consolideren van uw bestaande datawarehouse-omgeving voor het gebruik van een SQL Data Warehouse-database
3. Hefboomwerking **gebruiker gedefinieerde schema's** voor de grens eerder geïmplementeerd met behulp van databases.

Als de gebruiker gedefinieerde schema's zijn niet eerder is gebruikt, hebt u een schone lei. Gewoon de naam van de oude database gebruiken als basis voor de gebruiker gedefinieerde schema's in de SQL Data Warehouse-database.

Als de schema's al zijn gebruikt hebt u een aantal opties:

1. De schemanamen van de verouderde verwijderen en opnieuw beginnen
2. De schemanamen van de verouderde behouden door vóór de naam van het oude schema aan de naam van de tabel
3. De schemanamen van de verouderde behouden door het implementeren van weergaven in de tabel in een extra schema om de structuur van het oude schema opnieuw te maken.

> [!NOTE]
> Optie 3 lijkt op de eerste inspectie zoals de meest interessante optie. De duivelse is echter in de details. Weergaven zijn alleen-lezen in SQL Data Warehouse. Wijzigingen van gegevens of de tabel moet worden uitgevoerd op de basistabel. Optie 3 introduceert bovendien een laag van weergaven in uw systeem. Mogelijk wilt nadenken over dit een aantal extra als u weergaven in uw architectuur al.
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

Behouden verouderde schemanamen door vóór ze aan de naam van de tabel. Schema's gebruiken voor de grens van de werkbelasting.

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

Verouderde schemanamen met weergaven behouden

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
> Eventuele wijzigingen in de strategie voor schema moet een overzicht van het beveiligingsmodel voor de database. In veel gevallen is het mogelijk voor het vereenvoudigen van het beveiligingsmodel door machtigingen op het schemaniveau van het te wijzen. Als u meer gedetailleerde machtigingen zijn vereist, kunt u databaserollen gebruiken.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [overzicht voor ontwikkelaars][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
