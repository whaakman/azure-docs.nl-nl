---
title: T-SQL-weergaven gebruiken in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het gebruik van T-SQL-weergaven in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: e8d516cfd764f947bd2fe7fc25f6394c313c0d9a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595503"
---
# <a name="views-in-azure-sql-data-warehouse"></a>Weergaven in Azure SQL datawarehouse
Tips voor het gebruik van T-SQL-weergaven in Azure SQL Data Warehouse om oplossingen te ontwikkelen. 

## <a name="why-use-views"></a>Waarom weergaven gebruiken?
Weergaven kunnen worden gebruikt in een aantal verschillende manieren om de kwaliteit van uw oplossing te verbeteren.  In dit artikel ziet u enkele voorbeelden van hoe u uw oplossing met weergaven, evenals de beperkingen die moeten worden overwogen verrijken.


> [!IMPORTANT]
> Zie de nieuwe syntaxis van de gerealiseerde weergave op [CREATE gerealiseerde weergave AS SELECT](/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?view=azure-sqldw-latest).  Zie voor meer informatie de [opmerkingen bij de release](/azure/sql-data-warehouse/release-notes-10-0-10106-0).
>


> [!NOTE]
> Syntaxis voor CREATE VIEW is niet in dit artikel besproken. Zie voor meer informatie de [CREATE VIEW](/sql/t-sql/statements/create-view-transact-sql) documentatie.
> 

## <a name="architectural-abstraction"></a>Architectuur abstractie

Er is een algemeen patroon opnieuw om tabellen te maken met behulp van CREATE TABLE AS SELECT (CTAS) gevolgd door een object patroon hernoemen tijdens het laden van gegevens.

Het volgende voorbeeld wordt een nieuwe datum records naar een datumdimensie. Houd er rekening mee hoe een nieuwe tabel, DimDate_New, is het eerst wordt gemaakt en vervolgens ter vervanging van de oorspronkelijke versie van de tabel is gewijzigd.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Deze aanpak kan echter resulteren in tabellen die wordt weergegeven en in de weergave van een gebruiker als "tabel bestaat niet" foutberichten verdwijnen. Weergaven kunnen worden gebruikt om gebruikers te bieden een consistente presentatielaag terwijl de onderliggende objecten worden gewijzigd. Met toegang tot gegevens via weergaven, hoeven gebruikers geen zichtbaarheid van de onderliggende tabellen. Deze laag biedt een consistente gebruikerservaring tijdens het gegevensmodel ervoor te zorgen dat de ontwerpers van het datawarehouse kunt ontwikkelen. Kunnen betekent ontwikkelen van de onderliggende tabellen ontwerpers CTAS kunnen gebruiken om de prestaties tijdens het laden van de gegevens te maximaliseren.   

## <a name="performance-optimization"></a>Optimalisatie van prestaties
Weergaven kunnen ook worden gebruikt om af te dwingen joins tussen de tabellen geoptimaliseerd voor prestaties. Een weergave kunt bijvoorbeeld een redundante distributiesleutel opnemen als onderdeel van het lid te worden criteria om te beperken van gegevensverplaatsing. Een ander voordeel van een weergave kan worden om af te dwingen een bepaalde query of een geheugensteun voor het lid te worden. Weergaven gebruiken op deze manier zorgt ervoor dat joins altijd worden uitgevoerd op optimale wijze de noodzaak van gebruikers te onthouden juist om voor te bereiden voor hun joins te voorkomen.

## <a name="limitations"></a>Beperkingen
Weergaven in SQL Data Warehouse worden opgeslagen als alleen metagegevens. De volgende opties zijn als gevolg hiervan niet beschikbaar:

* Er is geen schema binding-optie
* Basistabellen kunnen niet worden bijgewerkt via de weergave
* Weergaven kunnen niet worden gemaakt over tijdelijke tabellen
* Er is geen ondersteuning voor UITVOUWEN / NOEXPAND-hints
* Er zijn geen geïndexeerde weergaven in SQL Data Warehouse

## <a name="next-steps"></a>Volgende stappen
Zie [Overzicht van SQL Data Warehouse voor ontwikkelaars](sql-data-warehouse-overview-develop.md) voor meer tips voor ontwikkelaars.


