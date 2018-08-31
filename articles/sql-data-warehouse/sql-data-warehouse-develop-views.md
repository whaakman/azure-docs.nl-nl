---
title: T-SQL-weergaven gebruiken in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het gebruik van T-SQL-weergaven in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 796a549c3a01c235422667dc63b729244b1917bf
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43306500"
---
# <a name="views-in-azure-sql-data-warehouse"></a>Weergaven in Azure SQL datawarehouse
Tips voor het gebruik van T-SQL-weergaven in Azure SQL Data Warehouse om oplossingen te ontwikkelen. 

## <a name="why-use-views"></a>Waarom weergaven gebruiken?
Weergaven kunnen worden gebruikt in een aantal verschillende manieren om de kwaliteit van uw oplossing te verbeteren.  In dit artikel ziet u enkele voorbeelden van hoe u uw oplossing met weergaven, evenals de beperkingen die moeten worden overwogen verrijken.

> [!NOTE]
> Syntaxis voor CREATE VIEW is niet in dit artikel besproken. Zie voor meer informatie de [CREATE VIEW](/sql/t-sql/statements/create-view-transact-sql) documentatie.
> 
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


