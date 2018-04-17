---
title: T-SQL-weergaven gebruiken in Azure SQL Data Warehouse | Microsoft Docs
description: ccccc
services: Tips for using T-SQL views in Azure SQL Data Warehouse for developing solutions.
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/12/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 249eaf07c5cd4ae918b6a95b1555f7198c7a23a2
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="views-in-azure-sql-data-warehouse"></a>Weergaven in Azure SQL datawarehouse
Tips voor het gebruik van T-SQL-weergaven in Azure SQL Data Warehouse om oplossingen te ontwikkelen. 


## <a name="why-use-views"></a>Waarom weergaven gebruiken?
Weergaven kunnen worden gebruikt in een aantal verschillende manieren om de kwaliteit van uw oplossing te verbeteren.  In dit artikel worden enkele voorbeelden van hoe uw oplossing met weergaven, evenals de beperkingen die moeten worden overwogen verrijken gemarkeerd.

> [!NOTE]
> De syntaxis voor CREATE VIEW wordt niet beschreven in dit artikel. Zie voor meer informatie de [CREATE VIEW](/sql/t-sql/statements/create-view-transact-sql) documentatie.
> 
> 

## <a name="architectural-abstraction"></a>Architectuur abstractie
Een algemene toepassing patroon is opnieuw om tabellen te maken met behulp van maken tabel AS selecteren (CTAS) gevolgd door een object patroon wijzigen tijdens het laden van gegevens.

Het volgende voorbeeld wordt een nieuwe datum records naar een datumdimensie. Houd er rekening mee hoe een nieuwe tabel DimDate_New, is het eerst hebt gemaakt en wordt de naam gewijzigd ter vervanging van de oorspronkelijke versie van de tabel.

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

Deze methode kan echter resulteren in tabellen die wordt weergegeven en verdwijnen uit de weergave van een gebruiker, evenals een "tabel bestaat niet" foutberichten. Weergaven kunnen worden gebruikt om gebruikers te geven met een laag consistente presentatie terwijl de onderliggende objecten worden gewijzigd. Dankzij de toegang tot gegevens via weergaven, hoeven gebruikers geen zichtbaarheid in de onderliggende tabellen. Deze laag biedt een consistente gebruikerservaring tijdens het gegevensmodel ervoor te zorgen dat de datawarehouse-designers kunt ontwikkelen. Kunnen betekent ontwikkelen van de onderliggende tabellen ontwerpers CTAS kunnen gebruiken om de prestaties tijdens het laden van gegevens te optimaliseren.   

## <a name="performance-optimization"></a>Optimalisatie van prestaties
Weergaven kunnen ook worden gebruikt om af te dwingen voor optimale prestaties joins tussen de tabellen. Een weergave kunt bijvoorbeeld een redundante distributiesleutel opnemen als onderdeel van de gekoppelde criteria voor het beperken van gegevensverplaatsing. Een ander voordeel van een weergave kan worden om af te dwingen een specifieke query of gekoppelde hint. Weergaven gebruiken op deze manier wordt gegarandeerd dat joins altijd worden uitgevoerd op optimale wijze de noodzaak van gebruikers te onthouden van de juiste construct voor hun joins te vermijden.

## <a name="limitations"></a>Beperkingen
Weergaven in SQL Data Warehouse worden opgeslagen als bestaan alleen uit metagegevens. Als gevolg daarvan kan zijn de volgende opties niet beschikbaar:

* Er is geen schema binding-optie
* Basistabellen kunnen niet via de weergave worden bijgewerkt
* Weergaven kunnen niet worden gemaakt via tijdelijke tabellen
* Er is geen ondersteuning voor UITVOUWEN / NOEXPAND-hints
* Er zijn geen geïndexeerde weergaven in SQL Data Warehouse

## <a name="next-steps"></a>Volgende stappen
Zie [Overzicht van SQL Data Warehouse voor ontwikkelaars](sql-data-warehouse-overview-develop.md) voor meer tips voor ontwikkelaars.


