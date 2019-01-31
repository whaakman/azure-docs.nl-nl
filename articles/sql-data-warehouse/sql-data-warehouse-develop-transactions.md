---
title: Met behulp van transacties in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het implementeren van transacties in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: 2f463620f43ae95535a55005ebe9732495b89dc9
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55456660"
---
# <a name="using-transactions-in-sql-data-warehouse"></a>Met behulp van transacties in SQL Data Warehouse
Tips voor het implementeren van transacties in Azure SQL Data Warehouse om oplossingen te ontwikkelen.

## <a name="what-to-expect"></a>Wat u kunt verwachten
Zoals u verwachten zou, SQL Data Warehouse biedt ondersteuning voor transacties als onderdeel van de datawarehouse-workload. Om te controleren of dat de prestaties van SQL Data Warehouse wordt onderhouden op schaal zijn sommige functies echter beperkt in vergelijking tot SQL Server. In dit artikel worden de verschillen gemarkeerd en geeft een lijst van de andere. 

## <a name="transaction-isolation-levels"></a>Transactie-isolatieniveau niveaus
SQL Data Warehouse implementeert ACID-transactions. Het isolatieniveau van de transactionele ondersteuning is echter beperkt tot READ UNCOMMITTED; Dit niveau kan niet worden gewijzigd. Als niet-DOORGEVOERDE lezen een probleem is, kunt u een aantal methoden om te voorkomen dat vervuild leesbewerkingen van de gegevens te coderen kunt implementeren. De meest populaire methoden gebruiken CTAS en tabel partitie overschakelen (vaak het sliding venster-patroon genoemd) om te voorkomen dat gebruikers bij het opvragen van gegevens die nog steeds wordt voorbereid. Weergaven die vooraf filter de gegevens zijn ook een populaire benadering.  

## <a name="transaction-size"></a>Grootte van de transactie
Een transactie van één wijziging is in grootte beperkt. De limiet wordt per distributie toegepast. Daarom kan de totale toewijzing worden berekend door te vermenigvuldigen met de limiet voor het aantal distributiepunten. Om te bij benadering het maximum aantal rijen in de transactie deelt u de limiet voor de distributie door de totale grootte van elke rij. Voor kolommen met variabele lengte, houd rekening met een gemiddelde kolomlengte duurt in plaats van met behulp van de maximale grootte.

In de tabel onder de volgende veronderstellingen zijn aangebracht:

* Een gelijkmatige verdeling van gegevens is opgetreden 
* De rijlengte van de gemiddelde is 250 bytes

| [DWU](sql-data-warehouse-overview-what-is.md) | Gegevenslimiet per distributie (GiB) | Aantal distributies | Maximumgrootte van transactie (GiB) | Aantal rijen per distributie | Maximum aantal rijen per transactie |
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

De limiet transactiegrootte wordt per transactie of bewerking toegepast. Het wordt niet toegepast op alle gelijktijdige transacties. Elke transactie mag daarom deze hoeveelheid gegevens naar het logboek schrijven. 

Als u wilt optimaliseren en de hoeveelheid gegevens geschreven naar het logboek te minimaliseren, Raadpleeg de [aanbevolen procedures voor transacties](sql-data-warehouse-develop-best-practices-transactions.md) artikel.

> [!WARNING]
> De grootte van de maximale transactie kan alleen worden bereikt voor HASH of zelfs ROUND_ROBIN gedistribueerde tabellen waarbij de verspreiding van de gegevens is. Als de transactie is schrijven van gegevens in een ongelijke manier aan de distributies zijn de limiet is waarschijnlijk worden bereikt voordat de grootte van de maximale transactie.
> <!--REPLICATED_TABLE-->
> 
> 

## <a name="transaction-state"></a>Status van de transactie
SQL Data Warehouse maakt gebruik van de functie XACT_STATE() voor het rapporteren van een mislukte transactie met behulp van de waarde -2. Deze waarde betekent dat de transactie is mislukt en is gemarkeerd voor alleen terugdraaien.

> [!NOTE]
> Het gebruik van -2 door de functie XACT_STATE om aan te geven van een mislukte transactie vertegenwoordigt verschillend gedrag met SQL Server. SQL Server gebruikt de waarde -1 om weer te geven van een Onuitvoerbare transactie. SQL Server kan enkele fouten binnen een transactie zonder moet worden gemarkeerd als Onuitvoerbare tolereren. Bijvoorbeeld `SELECT 1/0` wordt een fout veroorzaken, maar niet afdwingen dat een transactie in een Onuitvoerbare staat. SQL Server kan ook leesbewerkingen in de Onuitvoerbare transactie. Echter, SQL Data Warehouse kiest, u dit kunt doen. Als er een fout optreedt in een SQL Data Warehouse-transactie wordt automatisch ingevoerd, wordt de status-2 en u niet mogelijk om een select-instructies verder totdat de instructie is teruggedraaid. Daarom is het belangrijk om te controleren dat de code van uw toepassing om te controleren of deze XACT_STATE() worden gebruikt als u mogelijk moet aanbrengen code moet wijzigen.
> 
> 

In SQL Server ziet u mogelijk een transactie die lijkt op het volgende:

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

De bovenstaande code biedt de volgende strekking weergegeven:

Msg 111233, 16 niveau, status 1, regel 1 111233; De huidige transactie is afgebroken en eventuele wijzigingen in behandeling zijn teruggedraaid. Oorzaak: Een transactie in een alleen-terugdraaien van de status is niet expliciet teruggedraaid voordat een DDL-, DML- of SELECT-instructie.

U kunt de uitvoer van de functies ERROR_ * wordt niet ophalen.

De code moet worden enigszins gewijzigd in SQL Data Warehouse:

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

Dat is gewijzigd, is dat het TERUGDRAAIEN van de transactie al worden uitgevoerd voordat het lezen van de gegevens van de fout in het CATCH-blok.

## <a name="errorline-function"></a>De functie Error_Line()
Het is ook het vermelden waard dat SQL Data Warehouse niet implementeren of de functie ERROR_LINE() ondersteunen. Als u dit in uw code hebt, moet u verwijderen om te conformeren met SQL Data Warehouse. Labels voor query's in uw code in plaats daarvan gebruiken voor het implementeren van dezelfde functionaliteit. Zie voor meer informatie de [LABEL](sql-data-warehouse-develop-label.md) artikel.

## <a name="using-throw-and-raiserror"></a>Met behulp van THROW en RAISERROR
THROW is de moderne implementatie voor het verhogen van uitzonderingen in SQL Data Warehouse maar RAISERROR wordt ook ondersteund. Er zijn enkele verschillen die zijn het noemen waard Let echter op.

* Gebruiker-gedefinieerde fout berichten getallen mag niet in het bereik 100.000 150.000 voor WEGGOOIEN
* RAISERROR-foutberichten worden opgelost op 50.000
* Gebruik van sys.messages wordt niet ondersteund.

## <a name="limitations"></a>Beperkingen
SQL Data Warehouse beschikt over een paar andere beperkingen die gerelateerd aan transacties zijn.

Ze zijn als volgt:

* Er zijn geen gedistribueerde transacties
* Er zijn geen geneste transacties toegestaan
* Geen opslaan punten toegestaan
* Er zijn geen benoemde transacties
* Er zijn geen gemarkeerde transacties
* Biedt geen ondersteuning voor DDL zoals CREATE TABLE binnen een door de gebruiker gedefinieerde transactie

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het optimaliseren van transacties, [aanbevolen procedures voor transacties](sql-data-warehouse-develop-best-practices-transactions.md). Zie voor meer informatie over andere best practices voor SQL Data Warehouse, [best practices voor SQL Data Warehouse](sql-data-warehouse-best-practices.md).

