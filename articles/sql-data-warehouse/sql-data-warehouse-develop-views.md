---
title: T-SQL-weergaven gebruiken in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het gebruik van Transact-SQL-weergaven in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: b5208f32-8f4a-4056-8788-2adbb253d9fd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: d2a03be810bd7f792876607ec735eb578b65a3b5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="views-in-sql-data-warehouse"></a>Weergaven in SQL datawarehouse
Weergaven zijn vooral nuttig in SQL Data Warehouse. Ze kunnen worden gebruikt in een aantal verschillende manieren om de kwaliteit van uw oplossing te verbeteren.  In dit artikel worden enkele voorbeelden van hoe uw oplossing met weergaven, evenals de beperkingen die moeten worden overwogen verrijken gemarkeerd.

> [!NOTE]
> De syntaxis voor `CREATE VIEW` niet in dit artikel wordt besproken. Raadpleeg de [CREATE VIEW] [ CREATE VIEW] op MSDN-artikel voor deze referentie-informatie.
> 
> 

## <a name="architectural-abstraction"></a>Architectuur abstractie
Een zeer gangbaar patroon van toepassing is opnieuw om tabellen te maken met behulp van maken tabel AS selecteren (CTAS) gevolgd door een object patroon wijzigen tijdens het laden van gegevens.

Het volgende voorbeeld voegt nieuwe datum records naar een datumdimensie. Houd er rekening mee hoe een nieuwe tabble, DimDate_New, is het eerst hebt gemaakt en wordt de naam gewijzigd ter vervanging van de oorspronkelijke versie van de tabel.

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

Deze methode kan echter resulteren in tabellen die wordt weergegeven en verdwijnen uit de weergave van een gebruiker, evenals een "tabel bestaat niet" foutberichten. Weergaven kunnen worden gebruikt om gebruikers te geven met een laag consistente presentatie terwijl de onderliggende objecten worden gewijzigd. Dankzij de gebruikers toegang tot de gegevens via een weergaven, betekent dat gebruikers hoeven niet te hebben zichtbaarheid van de onderliggende tabellen. Dit biedt een consistente gebruikerservaring tijdens ervoor te zorgen dat de datawarehouse-designers kunt ontwikkelen van het gegevensmodel en prestaties te optimaliseren via CTAS tijdens het laden van gegevens.    

## <a name="performance-optimization"></a>Optimalisatie van prestaties
Weergaven kunnen ook worden gebruikt om af te dwingen voor optimale prestaties joins tussen de tabellen. Een weergave kunt bijvoorbeeld een redundante distributiesleutel opnemen als onderdeel van de gekoppelde criteria voor het beperken van gegevensverplaatsing.  Een ander voordeel van een weergave kan worden om af te dwingen een specifieke query of gekoppelde hint. Weergaven gebruiken op deze manier wordt gegarandeerd dat joins altijd worden uitgevoerd op optimale wijze de noodzaak van gebruikers te onthouden van de juiste construct voor hun joins te vermijden.

## <a name="limitations"></a>Beperkingen
Weergaven in SQL Data Warehouse zijn alleen metagegevens.  Als gevolg daarvan is de volgende opties zijn niet beschikbaar:

* Er is geen schema binding-optie
* Basistabellen kunnen niet via de weergave worden bijgewerkt
* Weergaven kunnen niet worden gemaakt via tijdelijke tabellen
* Er is geen ondersteuning voor UITVOUWEN / NOEXPAND-hints
* Er zijn geen geïndexeerde weergaven in SQL Data Warehouse

## <a name="next-steps"></a>Volgende stappen
Zie [Overzicht van SQL Data Warehouse voor ontwikkelaars][SQL Data Warehouse development overview] voor meer tips voor ontwikkelaars.
Voor `CREATE VIEW` syntaxis Raadpleeg [CREATE VIEW][CREATE VIEW].

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CREATE VIEW]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
