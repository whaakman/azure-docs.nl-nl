---
title: Gebruik van transacties in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het implementeren van transacties in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: ckarst
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: 7fa3d19cc0fca81616969773a40c3d3dbccc4a26
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
ms.locfileid: "31596922"
---
# <a name="using-transactions-in-sql-data-warehouse"></a>Gebruik van transacties in SQL Data Warehouse
Tips voor het implementeren van transacties in Azure SQL Data Warehouse om oplossingen te ontwikkelen.

## <a name="what-to-expect"></a>Wat u kunt verwachten
Zoals u verwachten zou, SQL Data Warehouse biedt ondersteuning voor transacties als onderdeel van de datawarehouse-workload. Om te controleren of dat de prestaties van SQL Data Warehouse wordt onderhouden op grote schaal zijn sommige functies echter beperkt in vergelijking tot SQL Server. In dit artikel worden de verschillen gemarkeerd en geeft een lijst van de andere. 

## <a name="transaction-isolation-levels"></a>Transactie-isolatieniveaus
SQL Data Warehouse implementeert ACID-transactions. Het isolatieniveau van de transactionele ondersteuning is echter beperkt tot READ UNCOMMITTED; Dit niveau kan niet worden gewijzigd. Als READ UNCOMMITTED belangrijk is, kunt u een aantal methoden om te voorkomen dat niet-weggeschreven gelezen gegevens van de gegevens te coderen implementeren. De meest populaire methoden CTAS en tabel partitie overschakelen (vaak het sliding venster-patroon genoemd) gebruiken om te voorkomen dat gebruikers een query die nog steeds wordt voorbereid. Weergaven die vooraf filter de gegevens zijn ook een populaire benadering.  

## <a name="transaction-size"></a>De grootte van de transactie
Er is een wijziging één transactie in grootte beperkt. De limiet is per distributiepunt toegepast. Daarom kan de totale toewijzing worden berekend door de limiet vermenigvuldigen met het aantal distributiepunten. Naar geschatte het maximale aantal rijen in de transactie deelt het kapje distributie door de totale grootte van elke rij. Voor kolommen met variabele lengte, kunt u een gemiddelde kolomlengte duurt dan de maximumgrootte.

In de tabel hieronder de volgende veronderstellingen zijn aangebracht:

* Een gelijkmatige verdeling van gegevens is opgetreden 
* De gemiddelde rijlengte is 250 bytes

| [DWU](sql-data-warehouse-overview-what-is.md) | Cap per distributiepunt (GiB) | Aantal distributies | Transactie maximumgrootte (GiB) | # Rijen per distributiepunt | Maximumaantal rijen per transactie |
| --- | --- | --- | --- | --- | --- |
| DW100 |1 |60 |60 |4,000,000 |240,000,000 |
| DW200 |1.5 |60 |90 |6,000,000 |360,000,000 |
| DW300 |2.25 |60 |135 |9,000,000 |540,000,000 |
| DW400 |3 |60 |180 |12,000,000 |720,000,000 |
| DW500 |3.75 |60 |225 |15,000,000 |900,000,000 |
| DW600 |4.5 |60 |270 |18,000,000 |1,080,000,000 |
| DW1000 |7.5 |60 |450 |30,000,000 |1,800,000,000 |
| DW1200 |9 |60 |540 |36,000,000 |2,160,000,000 |
| DW1500 |11.25 |60 |675 |45,000,000 |2,700,000,000 |
| DW2000 |15 |60 |900 |60,000,000 |3,600,000,000 |
| DW3000 |22.5 |60 |1,350 |90,000,000 |5,400,000,000 |
| DW6000 |45 |60 |2,700 |180,000,000 |10,800,000,000 |

De transactie maximale grootte wordt per transactie of bewerking toegepast. Het is niet toegepast op alle gelijktijdige transacties. Elke transactie mag daarom deze hoeveelheid gegevens naar het logboek schrijven. 

Om te optimaliseren en de hoeveelheid gegevens geschreven naar het logboek te minimaliseren, raadpleegt u de [transacties aanbevolen procedures](sql-data-warehouse-develop-best-practices-transactions.md) artikel.

> [!WARNING]
> De grootte van de maximale transactie kan alleen worden bereikt voor HASH of ROUND_ROBIN gedistribueerd tabellen waarbij de spreiding van de gegevens is zelfs. Als de transactie is schrijven van gegevens op een wijze vertekende naar de distributies zijn de limiet is waarschijnlijk worden bereikt voordat de transactie maximale grootte.
> <!--REPLICATED_TABLE-->
> 
> 

## <a name="transaction-state"></a>Transactiestatus
SQL Data Warehouse maakt gebruik van de functie XACT_STATE() voor het rapporteren van een mislukte transactie met de waarde -2. Deze waarde betekent dat de transactie is mislukt en is gemarkeerd voor alleen terugdraaien.

> [!NOTE]
> Het gebruik van -2 door de functie XACT_STATE om aan te geven van een mislukte transactie vertegenwoordigt verschillend gedrag met SQL Server. SQL Server gebruikt de waarde -1 vertegenwoordigt een Onuitvoerbare transactie. SQL Server kan een aantal fouten binnen een transactie zonder zijn gemarkeerd als een Onuitvoerbare tolereren. Bijvoorbeeld `SELECT 1/0` wordt een fout veroorzaken, maar niet afdwingen van een transactie in een Onuitvoerbare staat. SQL Server wordt ook gelezen kan in een Onuitvoerbare transactie. Echter kunt SQL Data Warehouse u niet doen. Als er een fout optreedt in een SQL Data Warehouse-transactie wordt automatisch overgeschakeld naar de status-2 en u zich niet Breng een select-instructies verder totdat de instructie is teruggedraaid. Daarom is het belangrijk om te controleren dat de toepassingscode om te zien als XACT_STATE() wordt gebruikt als u moet mogelijk codewijzigingen aanbrengen.
> 
> 

Bijvoorbeeld, in SQL Server mogelijk ziet u een transactie die op het volgende lijkt:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;

        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

De bovenstaande code kunt het volgende foutbericht weergegeven:

Bericht 111233, 16 niveau status 1, regel 1 111233; De huidige transactie is afgebroken en eventuele wijzigingen in behandeling zijn teruggedraaid. Oorzaak: Een transactie in een status terugdraaien van de alleen-lezen is niet expliciet teruggedraaid voordat een DDL-, DML- of SELECT-instructie.

U kunt de uitvoer van de functies ERROR_ * won't ophalen.

In SQL Data Warehouse moet de code enigszins worden gewijzigd:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Het verwachte gedrag is nu waargenomen. De fout in de transactie wordt beheerd en de functies ERROR_ * Geef waarden op zoals verwacht.

Dat is gewijzigd, is dat het TERUGDRAAIEN van de transactie moesten gebeuren voor het lezen van de informatie over de fout in het CATCH-blok.

## <a name="errorline-function"></a>De functie Error_Line()
Het is ook opgemerkt dat SQL Data Warehouse niet implementeert of ondersteuning voor de functie ERROR_LINE(). Als u dit in uw code hebt, moet u verwijderen om te voldoen aan de SQL Data Warehouse. Query labels in uw code in plaats daarvan gebruiken voor het implementeren van dezelfde functionaliteit. Zie voor meer informatie de [LABEL](sql-data-warehouse-develop-label.md) artikel.

## <a name="using-throw-and-raiserror"></a>Met behulp van THROW en RAISERROR
THROW is de moderne implementatie voor uitzonderingen in SQL Data Warehouse maar RAISERROR wordt ook ondersteund. Er zijn een paar verschillen waarde Let echter op.

* Gebruiker gedefinieerde fout berichten getallen kan niet binnen het bereik 100.000 150.000 voor WEGGOOIEN
* Foutberichten voor RAISERROR worden opgelost op 50.000
* Gebruik van sys.messages wordt niet ondersteund.

## <a name="limitations"></a>Beperkingen
SQL Data Warehouse heeft enkele andere beperkingen met betrekking tot transacties.

Ze zijn als volgt:

* Er is geen gedistribueerde transacties
* Er is geen geneste transacties toegestaan
* Er is geen opslaan punten toegestaan
* Er zijn geen benoemde transacties
* Er is geen gemarkeerde transacties
* Biedt geen ondersteuning voor DDL zoals CREATE TABLE binnen een door de gebruiker gedefinieerde transactie

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het optimaliseren van transacties, [transacties aanbevolen procedures](sql-data-warehouse-develop-best-practices-transactions.md). Zie voor meer informatie over andere aanbevelingen voor SQL Data Warehouse, [aanbevolen procedures voor SQL Data Warehouse](sql-data-warehouse-best-practices.md).

