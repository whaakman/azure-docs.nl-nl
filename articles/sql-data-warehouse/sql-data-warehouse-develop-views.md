---
title: T-SQL-weer gaven gebruiken in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het gebruik van T-SQL-weer gaven in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 8a770e66120e69271744942899186ece39b2a3c3
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479524"
---
# <a name="views-in-azure-sql-data-warehouse"></a>Weer gaven in Azure SQL Data Warehouse
Tips voor het gebruik van T-SQL-weer gaven in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. 

## <a name="why-use-views"></a>Waarom weer gaven gebruiken?
Weer gaven kunnen op verschillende manieren worden gebruikt om de kwaliteit van uw oplossing te verbeteren.  In dit artikel worden enkele voor beelden gegeven van hoe u uw oplossing kunt verrijken met weer gaven, evenals de beperkingen die moeten worden overwogen.


> [!IMPORTANT]
> Zie de nieuwe syntaxis voor gerealiseerde weer gave op een [gerealiseerde weer gave maken als selecteren](/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?view=azure-sqldw-latest).  Zie de [release opmerkingen](/azure/sql-data-warehouse/release-notes-10-0-10106-0)voor meer informatie.
>


> [!NOTE]
> De syntaxis voor de weer gave CREATE wordt niet beschreven in dit artikel. Zie de documentatie voor het [maken van weer gaven](/sql/t-sql/statements/create-view-transact-sql) voor meer informatie.
> 

## <a name="architectural-abstraction"></a>Samen vatting van architectuur

Een algemeen toepassings patroon is het opnieuw maken van tabellen met CREATE TABLE als SELECT (CTAS), gevolgd door het wijzigen van het naam patroon van een object tijdens het laden van gegevens.

In het volgende voor beeld worden nieuwe datum records toegevoegd aan een datum dimensie. U ziet hoe een nieuwe tabel, DimDate_New, wordt gemaakt en de naam ervan wordt gewijzigd om de oorspronkelijke versie van de tabel te vervangen.

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

Deze benadering kan echter leiden tot tabellen die worden weer gegeven en weer gegeven in de gebruikers weergave, evenals de fout berichten ' tabel bestaat niet '. Weer gaven kunnen worden gebruikt om gebruikers een consistente presentatielaag te bieden, terwijl de namen van de onderliggende objecten worden gewijzigd. Door toegang te bieden tot gegevens via weer gaven, hebben gebruikers geen zicht baarheid nodig voor de onderliggende tabellen. Deze laag biedt een consistente gebruikers ervaring en zorgt ervoor dat de Data Warehouse-ontwerpers het gegevens model kunnen ontwikkelen. Het is mogelijk om de onderliggende tabellen te ontwikkelen. ontwerpers kunnen CTAS gebruiken om de prestaties tijdens het proces voor het laden van gegevens te maximaliseren.   

## <a name="performance-optimization"></a>Optimalisatie van prestaties
Weer gaven kunnen ook worden gebruikt voor het afdwingen van prestaties geoptimaliseerde samen voegingen tussen tabellen. Een weer gave kan bijvoorbeeld een redundante distributie sleutel bevatten als onderdeel van de samenvoegings criteria om de verplaatsing van gegevens te minimaliseren. Een ander voor deel van een weer gave is het afdwingen van een specifieke query of hint voor samen voegen. Als u weer gaven op deze manier gebruikt, zorgt u ervoor dat samen voegingen altijd worden uitgevoerd op een optimale manier om te voor komen dat gebruikers de juiste construct voor hun deelname kunnen onthouden.

## <a name="limitations"></a>Beperkingen
Weer gaven in SQL Data Warehouse worden alleen als meta gegevens opgeslagen. De volgende opties zijn daarom niet beschikbaar:

* Er is geen optie voor schema bindingen
* Basis tabellen kunnen niet worden bijgewerkt via de weer gave
* Er kunnen geen weer gaven worden gemaakt over tijdelijke tabellen
* Er wordt geen ondersteuning geboden voor de instructie EXPAND/deexpand.
* Er zijn geen geïndexeerde weer gaven in SQL Data Warehouse

## <a name="next-steps"></a>Volgende stappen
Zie [Overzicht van SQL Data Warehouse voor ontwikkelaars](sql-data-warehouse-overview-develop.md) voor meer tips voor ontwikkelaars.


