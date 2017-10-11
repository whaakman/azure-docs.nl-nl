---
title: In het geheugen OLTP verbetert de prestaties van SQL-transacties | Microsoft Docs
description: Hanteer In het geheugen OLTP transactionele prestaties in een bestaande SQL-database te verbeteren.
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: MightyPen
ms.assetid: c2bf14a0-905b-47b4-afb6-efe9a61147d5
ms.service: sql-database
ms.custom: develop databases
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2016
ms.author: jodebrui
ms.openlocfilehash: 50eed9aed417778bd497f55e20c8e732fdae9cf9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-in-memory-oltp-to-improve-your-application-performance-in-sql-database"></a>Gebruik In het geheugen OLTP voor het verbeteren van de toepassingsprestaties van uw in SQL-Database
[In het geheugen OLTP](sql-database-in-memory.md) kan worden gebruikt voor het verbeteren van de prestaties van transactieverwerking gegevensopname en tijdelijke gegevensscenario, in [Premium](sql-database-service-tiers.md) Azure SQL-Databases zonder te verhogen van de prijscategorie. 

> [!NOTE] 
> Meer informatie over hoe [Quorum wordt sleutel database werkbelasting verdubbeld tijdens DTU verlagen door 70% met SQL Database](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)


Volg deze stappen vast te stellen In het geheugen OLTP in uw bestaande database.

## <a name="step-1-ensure-you-are-using-a-premium-database"></a>Stap 1: Zorg ervoor dat u gebruikt een Premium-database
In het geheugen OLTP wordt alleen ondersteund in Premium-databases. In het geheugen wordt ondersteund als het geretourneerde resultaat 1 (niet 0):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* staat voor *Extreme transactieverwerking*



## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a>Stap 2: Migreren naar een In-geheugen OLTP-objecten identificeren
SSMS bevat een **transactie prestaties Analysis overzicht** rapport dat u op een database met een actieve werkbelasting uitvoeren kunt. Het rapport identificeert de tabellen en opgeslagen procedures die geschikt voor migratie naar de In-geheugen OLTP zijn.

In SSMS om het rapport te genereren:

* In de **Objectverkenner**, met de rechtermuisknop op het databaseknooppunt van uw.
* Klik op **rapporten** > **standaardrapporten** > **transactie prestaties Analysis overzicht**.

Zie voor meer informatie [vaststellen als een tabel of een opgeslagen Procedure moet worden overgezet naar de In-geheugen OLTP](http://msdn.microsoft.com/library/dn205133.aspx).

## <a name="step-3-create-a-comparable-test-database"></a>Stap 3: Een vergelijkbare testdatabase maken
Stel dat het rapport geeft aan dat de database bevat een tabel die kunnen profiteren zouden van wordt geconverteerd naar een tabel geoptimaliseerd voor geheugen. Het is raadzaam dat u eerst testen om te controleren van de vermelding door te testen.

U moet een test-kopie van de productiedatabase. De testdatabase moet op dezelfde service laag niveau als uw productiedatabase.

Om te vereenvoudigen testen, moet u uw testdatabase als volgt aanpassen:

1. Verbinding maken met de testdatabase met behulp van SSMS.
2. Om te voorkomen dat de optie WITH (SNAPSHOT) in query's, stelt u de databaseoptie zoals aangegeven in de volgende T-SQL-instructie:
   
   ```
   ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
   ```

## <a name="step-4-migrate-tables"></a>Stap 4: Tabellen migreren
U moet maken en vullen van een kopie geoptimaliseerd voor geheugen van de tabel die u wilt testen. U kunt deze maken met behulp van:

* De handige geheugen Wizard optimalisatie in SSMS.
* Handmatige T-SQL.

#### <a name="memory-optimization-wizard-in-ssms"></a>Wizard van geheugen optimalisatie in SSMS
Deze migratieoptie wilt gebruiken:

1. Verbinding maken met de testdatabase met SSMS.
2. In de **Objectverkenner**, met de rechtermuisknop op de tabel en klik vervolgens op **geheugen optimalisatie Advisor**.
   
   * De **tabel geheugen optimalisatie van Advisor** wizard wordt weergegeven.
3. Klik in de wizard op **migratie validatie** (of de **volgende** knop) om te zien als de tabel heeft een niet-ondersteunde functies die niet worden ondersteund in tabellen geoptimaliseerd voor geheugen. Zie voor meer informatie:
   
   * De *geheugen optimalisatie controlelijst* in [geheugen optimalisatie Advisor](http://msdn.microsoft.com/library/dn284308.aspx).
   * [Transact-SQL-Constructs niet wordt ondersteund door In het geheugen OLTP](http://msdn.microsoft.com/library/dn246937.aspx).
   * [Migreren naar een In-geheugen OLTP](http://msdn.microsoft.com/library/dn247639.aspx).
4. Als de tabel geen niet-ondersteunde onderdelen heeft, kan de advisor de werkelijke schema en de gegevensmigratie voor u uitvoeren.

#### <a name="manual-t-sql"></a>Handmatige T-SQL
Deze migratieoptie wilt gebruiken:

1. Verbinding maken met uw testdatabase met behulp van SSMS (of een vergelijkbaar hulpprogramma).
2. Het volledige T-SQL-script voor de tabel en de bijbehorende indexen verkrijgen.
   
   * SSMS, met de rechtermuisknop op het knooppunt van uw tabel.
   * Klik op **tabel als een Script** > **maken naar** > **nieuwe queryvenster**.
3. Voeg in het scriptvenster WITH (MEMORY_OPTIMIZED = ON) in de instructie CREATE TABLE.
4. Als er een GECLUSTERDE index, wijzigen in NONCLUSTERED.
5. Wijzig de naam van de bestaande tabel met behulp van SP_RENAME.
6. Maak de nieuwe kopie geoptimaliseerd voor geheugen van de tabel de bewerkte CREATE TABLE-script uit te voeren.
7. Kopieer de gegevens naar de tabel geoptimaliseerd voor geheugen met behulp van INSERT... SELECTEER * IN:

```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>Stap 5 (optioneel): opgeslagen procedures migreren
De In-Memory-functie kunt ook een opgeslagen procedure voor verbeterde prestaties wijzigen.

### <a name="considerations-with-natively-compiled-stored-procedures"></a>Overwegingen voor systeemeigen, gecompileerde, opgeslagen procedures
Een systeemeigen, gecompileerde, opgeslagen procedure moet hebben op de component T-SQL met de volgende opties:

* NATIVE_COMPILATION
* SCHEMABINDING: wat betekent dat tabellen die de opgeslagen procedure hun kolomdefinities gewijzigd op een manier waardoor beïnvloedt de opgeslagen procedure hebben kan, tenzij u de opgeslagen procedure verwijderen.

Een systeemeigen module moet een gebruiken big [ATOMIC-blokken](http://msdn.microsoft.com/library/dn452281.aspx) voor het transactiebeheer van de. Er is geen rol voor een expliciete BEGIN TRANSACTION of ROLLBACK TRANSACTION. Als uw code een schending van een bedrijfsregel detecteert, deze beëindigen atomic-blok met een [THROW](http://msdn.microsoft.com/library/ee677615.aspx) instructie.

### <a name="typical-create-procedure-for-natively-compiled"></a>Typische CREATE PROCEDURE voor systeemeigen, gecompileerde
De T-SQL te maken van een systeemeigen, gecompileerde, opgeslagen procedure is normaal gesproken vergelijkbaar met de volgende sjabloon:

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

* Voor de TRANSACTION_ISOLATION_LEVEL is MOMENTOPNAME de meest voorkomende waarde voor de systeemeigen, gecompileerde, opgeslagen procedure. Een subset van de andere waarden zijn, worden ook ondersteund:
  
  * REPEATABLE READ
  * SERIALISEERBAAR
* Waarde van de taal zijn in de weergave sys.languages aanwezig.

### <a name="how-to-migrate-a-stored-procedure"></a>Het migreren van een opgeslagen procedure
Migratiestappen zijn:

1. Haal het script CREATE PROCEDURE aan de reguliere geïnterpreteerde opgeslagen procedure.
2. Herschrijf de header zodat deze overeenkomen met de vorige sjabloon.
3. Controleren of de opgeslagen procedure T-SQL-code alle functies die worden niet ondersteund voor systeemeigen, gecompileerde, opgeslagen procedures gebruikt. Tijdelijke oplossingen implementeren indien nodig.
   
   * Zie voor meer informatie [migratieproblemen voor systeemeigen gecompileerde opgeslagen Procedures](http://msdn.microsoft.com/library/dn296678.aspx).
4. Wijzig de naam van de oude opgeslagen procedure met behulp van SP_RENAME. Of u gewoon.
5. De bewerkte CREATE PROCEDURE T-SQL-script uitvoeren.

## <a name="step-6-run-your-workload-in-test"></a>Stap 6: Uw werkbelasting uitvoeren in de test
Voer een werkbelasting in uw testdatabase die vergelijkbaar is met de werkbelasting die wordt uitgevoerd in de productiedatabase. Dit zou moeten uitwijzen de prestatieverbetering bereikt door uw gebruik van de In-Memory-functie voor tabellen en opgeslagen procedures.

Belangrijke kenmerken van de werkbelasting zijn:

* Het aantal gelijktijdige verbindingen.
* Verhouding tussen lezen/schrijven.

Als u wilt aanpassen en de werkbelasting van de test uitvoert, kunt u overwegen het hulpprogramma handige ostress.exe geïllustreerd in [hier](sql-database-in-memory.md).

Om te beperken netwerklatentie, uw test worden uitgevoerd in dezelfde Azure geografische regio waar de database bestaat.

## <a name="step-7-post-implementation-monitoring"></a>Stap 7: Na de implementatie controleren
Houd rekening met controle van de gevolgen van de prestaties van uw implementaties In het geheugen in productie:

* [In het geheugen, opslag bewaken](sql-database-in-memory-oltp-monitoring.md).
* [Bewaking van Azure SQL-database met behulp van de dynamische beheerweergave](sql-database-monitoring-with-dmvs.md)

## <a name="related-links"></a>Verwante koppelingen
* [In het geheugen OLTP (optimalisatie In het geheugen)](http://msdn.microsoft.com/library/dn133186.aspx)
* [Inleiding tot systeemeigen, gecompileerde, opgeslagen Procedures](http://msdn.microsoft.com/library/dn133184.aspx)
* [Geheugen optimalisatie Advisor](http://msdn.microsoft.com/library/dn284308.aspx)

