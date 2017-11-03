---
title: Opgeslagen procedures in SQL Data Warehouse | Microsoft Docs
description: Tips voor het implementeren van opgeslagen procedures in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 9b238789-6efe-4820-bf77-5a5da2afa0e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: e42d80f0ca35f3fbb67389c66d072bc40d8a8d2c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="stored-procedures-in-sql-data-warehouse"></a>Opgeslagen procedures in SQL Data Warehouse
Veel van de Transact-SQL-functies die gevonden in SQL Server biedt ondersteuning voor SQL Data Warehouse. Er zijn belangrijker scale-out specifieke functies die we willen gebruiken om de prestaties van uw oplossing.

Als u wilt behouden zijn de schaal en prestaties van SQL Data Warehouse echter ook sommige functies en functionaliteit die u hebt doorgevoerd verschillen en anderen die niet worden ondersteund.

In dit artikel wordt uitgelegd hoe u voor het implementeren van opgeslagen procedures in SQL Data Warehouse.

## <a name="introducing-stored-procedures"></a>Introductie van opgeslagen procedures
Opgeslagen procedures zijn een goede manier voor het inkapselen van uw SQL-code het opslaan van dicht bij uw gegevens in het datawarehouse. Door de code in beheerbare eenheden encapsulating opgeslagen procedures ontwikkelaars helpen bij het modularize hun oplossingen; vergemakkelijken groter herbruikbaarheid van code. Elke opgeslagen procedure kan ook parameters zodat ze zelfs flexibelere accepteren.

SQL Data Warehouse biedt een vereenvoudigd en gestroomlijnd opgeslagen procedure-implementatie. Het belangrijkste verschil in vergelijking met SQL Server is dat de opgeslagen procedure niet vooraf gecompileerde code. In datawarehouses we in het algemeen minder betrokken zijn bij het tijdstip van compilatie. Het is belangrijk dat de code van de opgeslagen procedure correct is geoptimaliseerd, als het wordt uitgevoerd op basis van grote hoeveelheden gegevens. Het doel is om op te slaan uren, minuten en seconden niet milliseconden. Daarom is het handiger om na te denken van opgeslagen procedures als containers voor SQL-logica.     

Wanneer SQL Data Warehouse wordt uitgevoerd voor de opgeslagen procedure zijn de SQL-instructies geparseerd, vertaald en geoptimaliseerde tijdens runtime. Tijdens dit proces wordt elke instructie omgezet in gedistribueerde query's. De SQL-code die daadwerkelijk wordt uitgevoerd op basis van de gegevens is anders dan bij de query verzonden.

## <a name="nesting-stored-procedures"></a>Nesten van opgeslagen procedures
Als opgeslagen procedures andere opgeslagen procedures aanroepen of dynamische sql uitvoeren en vervolgens het binnenste opgeslagen procedure of code aanroepen genoemd worden genest.

SQL Data Warehouse ondersteunt maximaal 8 geneste niveaus. Dit verschilt enigszins met SQL Server. Het nest-niveau in SQL Server is 32.

De aanroep van het hoogste niveau opgeslagen procedure is gelijk als u wilt nesten niveau 1

```sql
EXEC prc_nesting
```
Als de opgeslagen procedure kan ook een andere EXEC Roep vervolgens neemt hierdoor het nest-niveau 2

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Als de tweede procedure enkele dynamische sql voert vervolgens neemt hierdoor het nest-niveau 3

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Opmerking SQL Data Warehouse ondersteunt momenteel geen@NESTLEVEL. U moet een om bij te houden van uw niveau nest zelf. Het lijkt onwaarschijnlijk bereikt u de limiet voor het niveau van 8 nest, maar als u dit doet u moet uw code opnieuw te laten werken en 'plat' deze zodat het past binnen deze limiet.

## <a name="insertexecute"></a>INSERT... UITVOEREN
SQL Data Warehouse staat niet toe dat u gebruiken voor de resultatenset van een opgeslagen procedure met een instructie INSERT. Er is echter een alternatieve methode die u kunt gebruiken.

Raadpleeg het volgende artikel over [tijdelijke tabellen] voor een voorbeeld van hoe u dit doet.

## <a name="limitations"></a>Beperkingen
Er zijn bepaalde aspecten van Transact-SQL opgeslagen procedures die niet zijn geïmplementeerd in SQL Data Warehouse.

Ze zijn:

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
Zie voor meer tips voor ontwikkeling, [overzicht voor ontwikkelaars][development overview].

<!--Image references-->

<!--Article references-->
[tijdelijke tabellen]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
