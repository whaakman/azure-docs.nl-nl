---
title: Gebruik van opgeslagen procedures in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het implementeren van opgeslagen procedures in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: ckarst
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: 0ad8a599065a44469a3151813972b3d2561782c6
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
ms.locfileid: "32774655"
---
# <a name="using-stored-procedures-in-sql-data-warehouse"></a>Gebruik van opgeslagen procedures in SQL Data Warehouse
Tips voor het implementeren van opgeslagen procedures in Azure SQL Data Warehouse om oplossingen te ontwikkelen.

## <a name="what-to-expect"></a>Wat u kunt verwachten

SQL Data Warehouse biedt ondersteuning voor veel van de T-SQL-functies die worden gebruikt in SQL Server. Er zijn echter scale-out specifieke functies waarmee u kunt de prestaties van uw oplossing.

Als u wilt behouden zijn de schaal en prestaties van SQL Data Warehouse echter ook sommige functies en functionaliteit die u hebt doorgevoerd verschillen en anderen die niet worden ondersteund.


## <a name="introducing-stored-procedures"></a>Introductie van opgeslagen procedures
Opgeslagen procedures zijn een goede manier voor het inkapselen van uw SQL-code het opslaan van dicht bij uw gegevens in het datawarehouse. Opgeslagen procedures kunt ontwikkelaars hun oplossingen modularize door de code encapsulating in beheerbare eenheden; vergemakkelijken groter hergebruik van code. Elke opgeslagen procedure kan ook parameters zodat ze zelfs flexibelere accepteren.

SQL Data Warehouse biedt een vereenvoudigd en gestroomlijnd opgeslagen procedure-implementatie. Het belangrijkste verschil in vergelijking met SQL Server is dat de opgeslagen procedure niet vooraf gecompileerde code. In datawarehouses is het tijdstip van compilatie kleine in vergelijking met de tijd die nodig is voor query's uitvoeren op grote hoeveelheden gegevens. Het is belangrijker om te controleren of dat de code van de opgeslagen procedure correct is geoptimaliseerd voor grote query's. Het doel is om op te slaan uren, minuten en seconden, niet milliseconden. Daarom is het handiger om na te denken van opgeslagen procedures als containers voor SQL-logica.     

Wanneer SQL Data Warehouse wordt uitgevoerd voor de opgeslagen procedure, zijn de SQL-instructies geparseerd vertaald en geoptimaliseerd tijdens runtime. Tijdens dit proces wordt elke instructie omgezet in gedistribueerde query's. De SQL-code die wordt uitgevoerd op basis van de gegevens is anders dan de query verzonden.

## <a name="nesting-stored-procedures"></a>Nesten van opgeslagen procedures
Als opgeslagen procedures andere opgeslagen procedures aanroepen of dynamische SQL-instructies uitvoeren, klikt u vervolgens de binnenste opgeslagen procedure of code aanroepen genoemd worden genest.

SQL Data Warehouse ondersteunt maximaal acht geneste niveaus. Dit verschilt enigszins met SQL Server. Het nest-niveau in SQL Server is 32.

De aanroep op het hoogste niveau van de opgeslagen procedure is om te worden genest niveau 1 gelijk.

```sql
EXEC prc_nesting
```
Als de opgeslagen procedure wordt ook een andere EXEC aanroepen doet, wordt het nest-niveau verhoogd naar twee.

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Als de tweede procedure wordt vervolgens uitgevoerd voor bepaalde dynamische SQL, verhoogt het nest-niveau tot drie.

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Houd er rekening mee SQL Data Warehouse biedt momenteel geen ondersteuning voor [@@NESTLEVEL](/sql/t-sql/functions/nestlevel-transact-sql). U moet volgen het nest-niveau. Het lijkt onwaarschijnlijk voor u het niveau acht nest overschreden, maar als u dit doet, moet u uw code aanpassen aan het aantal geneste niveaus binnen deze limiet bijwerken.

## <a name="insertexecute"></a>INSERT... UITVOEREN
SQL Data Warehouse staat niet toe dat u gebruiken voor de resultatenset van een opgeslagen procedure met een instructie INSERT. Er is echter een alternatieve methode die u kunt gebruiken. Zie het artikel op voor een voorbeeld [tijdelijke tabellen](sql-data-warehouse-tables-temporary.md). 

## <a name="limitations"></a>Beperkingen
Er zijn bepaalde aspecten van Transact-SQL opgeslagen procedures die niet zijn geïmplementeerd in SQL Data Warehouse.

Dit zijn:

* tijdelijke opgeslagen procedures
* genummerde opgeslagen procedures
* uitgebreide opgeslagen procedures
* CLR opgeslagen procedures
* versleutelingsoptie
* replicatie-optie
* Table-valued parameters
* alleen-lezen parameters
* standaardparameters
* contexten worden uitgevoerd
* instructie return

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [overzicht voor ontwikkelaars](sql-data-warehouse-overview-develop.md).

