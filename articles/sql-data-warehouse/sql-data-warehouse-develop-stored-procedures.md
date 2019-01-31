---
title: Met behulp van opgeslagen procedures in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het implementeren van opgeslagen procedures in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: cee2eff3cd57e37fa0567cd406f3e6f20a7aa8ac
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55456546"
---
# <a name="using-stored-procedures-in-sql-data-warehouse"></a>Met behulp van opgeslagen procedures in SQL Data Warehouse
Tips voor het implementeren van opgeslagen procedures in Azure SQL Data Warehouse om oplossingen te ontwikkelen.

## <a name="what-to-expect"></a>Wat u kunt verwachten

SQL Data Warehouse biedt ondersteuning voor veel van de T-SQL-functies die worden gebruikt in SQL Server. Er zijn nog belangrijker, scale-out specifieke functies die u gebruiken kunt om de prestaties van uw oplossing te maximaliseren.

Als u wilt behouden zijn de schaalmogelijkheden en prestaties van SQL Data Warehouse echter ook sommige functies en functionaliteit die u hebt doorgevoerd verschillen en anderen die niet worden ondersteund.


## <a name="introducing-stored-procedures"></a>Maak kennis met opgeslagen procedures
Opgeslagen procedures zijn een goede manier voor het inkapselen van uw SQL-code; het opslaan van dicht bij uw gegevens in het datawarehouse. Opgeslagen procedures zodat ontwikkelaars hun oplossingen door de code in beheerbare eenheden; encapsulating modularize vergemakkelijken groter hergebruik van code. De opgeslagen procedure kan ook parameters zodat ze zelfs meer flexibele accepteren.

SQL Data Warehouse biedt een vereenvoudigd en gestroomlijnd opgeslagen procedure-implementatie. Het grootste verschil ten opzichte van SQL Server is dat de opgeslagen procedure niet vooraf gecompileerde code. De compilatietijd is in datawarehouses, klein in vergelijking met de tijd die nodig is voor query's uitvoeren op grote hoeveelheden gegevens. Het is belangrijker om te controleren of dat de code van de opgeslagen procedure correct is geoptimaliseerd voor grote query's. Het doel is om op te slaan, uren, minuten en seconden, geen milliseconden. Het is daarom meer nuttige opgeslagen procedures beschouwen als containers voor SQL-logica.     

Bij SQL Data Warehouse uitvoeren van de opgeslagen procedure, worden de SQL-instructies geparseerd, vertaald en geoptimaliseerd tijdens runtime. Tijdens dit proces wordt elke instructie omgezet in gedistribueerde query's. De SQL-code die wordt uitgevoerd op basis van de gegevens is anders dan de verzonden query.

## <a name="nesting-stored-procedures"></a>Nesten van opgeslagen procedures
Wanneer opgeslagen procedures aanroepen van andere opgeslagen procedures of dynamic SQL uitvoeren, klikt u vervolgens de binnenste opgeslagen procedure of code-aanroep wordt gezegd dat worden genest.

SQL Data Warehouse biedt ondersteuning voor maximaal acht geneste niveaus. Dit is iets anders dan bij SQL Server. Het niveau van de geneste in SQL Server is 32.

De aanroep van de opgeslagen procedure op het hoogste niveau is als u wilt nesten niveau 1 gelijk.

```sql
EXEC prc_nesting
```
Als de opgeslagen procedure ook een andere EXEC aanroepen maakt, wordt het niveau van de geneste verhoogd naar 2.

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Als de tweede procedure enkele dynamic SQL voert, wordt het niveau van de geneste verhoogd tot drie.

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Houd er rekening mee, SQL Data Warehouse biedt momenteel geen ondersteuning voor [@@NESTLEVEL](/sql/t-sql/functions/nestlevel-transact-sql). U moet het niveau van de geneste bijhouden. Is het onwaarschijnlijk is voor u de limiet voor het niveau van acht geneste overschrijden, maar als u dit doet, moet u uw code voor het aanpassen van de geneste niveaus binnen deze limiet nabewerking.

## <a name="insertexecute"></a>INSERT... UITVOEREN
SQL Data Warehouse staat niet toe dat u gebruiken voor de resultatenset van een opgeslagen procedure met een instructie INSERT. Er is echter een alternatieve methode die u kunt gebruiken. Zie het artikel op voor een voorbeeld [tijdelijke tabellen](sql-data-warehouse-tables-temporary.md). 

## <a name="limitations"></a>Beperkingen
Er zijn bepaalde aspecten van Transact-SQL opgeslagen procedures die niet zijn geïmplementeerd in SQL Data Warehouse.

Dit zijn:

* tijdelijke opgeslagen procedures
* genummerde opgeslagen procedures
* Uitgebreide opgeslagen procedures
* CLR opgeslagen procedures
* Optie voor opslagversleuteling
* replicatie-optie
* tabelwaardeparameters
* alleen-lezen parameters
* Standaard-parameters
* contexten worden uitgevoerd
* instructie return

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkelaars [overzicht voor ontwikkelaars](sql-data-warehouse-overview-develop.md).

