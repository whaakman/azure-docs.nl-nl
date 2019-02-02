---
title: In-Memory OLTP verbetert de prestaties van SQL-transacties | Microsoft Docs
description: Hanteer In-Memory OLTP transactionele prestaties in een bestaande SQL-database te verbeteren.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jodebrui
ms.author: jodebrui
ms.reviewer: MightyPen
manager: craigg
ms.date: 11/07/2018
ms.openlocfilehash: fbe05186b317d3c24dca55197c2989155b5543bd
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55565918"
---
# <a name="use-in-memory-oltp-to-improve-your-application-performance-in-sql-database"></a>Gebruik In-Memory OLTP voor het verbeteren van de prestaties van uw toepassing in SQL-Database

[In-Memory OLTP](sql-database-in-memory.md) kan worden gebruikt voor het verbeteren van de prestaties van transactieverwerking en gegevensopname scenario's voor tijdelijke gegevens, in [Premium en bedrijfskritiek laag](sql-database-service-tiers-vcore.md) databases zonder dat de prijscategorie toeneemt. 

> [!NOTE] 
> Informatie over hoe [Quorum verdubbelt de belangrijkste databaseworkload terwijl u tegelijkertijd DTU terugdringt met 70% met SQL Database](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)


Volg deze stappen voor het vaststellen van In-Memory OLTP in uw bestaande database.

## <a name="step-1-ensure-you-are-using-a-premium-and-business-critical-tier-database"></a>Stap 1: Zorg ervoor dat u gebruikmaakt van een database van de laag Premium en bedrijfskritiek

In-Memory OLTP wordt alleen ondersteund in databases van de laag Premium en bedrijfskritiek. In het geheugen wordt ondersteund als de geretourneerde resultaten 1 is (niet 0):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* staat voor *Extreme transactieverwerking*



## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a>Stap 2: Objecten om te migreren naar de In-Memory OLTP identificeren
SSMS bevat een **transactieoverzicht prestaties Analysis** rapport dat kan worden uitgevoerd voor een database met een actieve werkbelasting. Het rapport identificeert de tabellen en opgeslagen procedures die geschikt voor migratie naar de In-Memory OLTP zijn.

In SSMS, om het rapport te genereren:

* In de **Objectverkenner**, met de rechtermuisknop op het databaseknooppunt van uw.
* Klik op **rapporten** > **standaardrapporten** > **transactieoverzicht prestaties Analysis**.

Zie voor meer informatie, [vaststellen als een tabel of een opgeslagen Procedure moet worden overgezet naar de In-Memory OLTP](https://msdn.microsoft.com/library/dn205133.aspx).

## <a name="step-3-create-a-comparable-test-database"></a>Stap 3: Een vergelijkbare test maken
Stel dat het rapport geeft aan dat de database bevat een tabel die voordeel hebben veel van wordt geconverteerd naar een tabel geoptimaliseerd voor geheugen. Het is raadzaam dat u eerst testen om te bevestigen dat de vermelding door te testen.

U moet een test-kopie van de productiedatabase. De testdatabase moet op dezelfde service laag niveau als uw productiedatabase.

Om de testen, moet u uw testdatabase als volgt aanpassen:

1. Verbinding maken met de testdatabase met behulp van SSMS.
2. Om te voorkomen dat de optie WITH (SNAPSHOT) in query's, stelt u de databaseoptie zoals wordt weergegeven in de volgende T-SQL-instructie:
   
   ```
   ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
   ```

## <a name="step-4-migrate-tables"></a>Stap 4: Migreren van tabellen
U moet maken en vullen van een geoptimaliseerd voor geheugen kopie van de tabel die u wilt testen. U kunt deze maken met behulp van:

* De handige geheugen optimalisatie Wizard in SSMS.
* Handmatige T-SQL.

#### <a name="memory-optimization-wizard-in-ssms"></a>Wizard voor geheugen-optimalisatie in SSMS
Gebruik deze migratieoptie:

1. Verbinding maken met de testdatabase met SSMS.
2. In de **Objectverkenner**, met de rechtermuisknop op de tabel en klik vervolgens op **geheugen optimalisatie Advisor**.
   
   * De **tabel geheugen optimaliseren Advisor** wizard wordt weergegeven.
3. In de wizard, klikt u op **validatie van de migratie** (of de **volgende** knop) om te zien als de tabel heeft een niet-ondersteunde functies die niet worden ondersteund in tabellen geoptimaliseerd voor geheugen. Zie voor meer informatie:
   
   * De *controlelijst voor het optimaliseren van geheugen* in [geheugen optimalisatie Advisor](https://msdn.microsoft.com/library/dn284308.aspx).
   * [Transact-SQL-Constructs niet wordt ondersteund door de In-Memory OLTP](https://msdn.microsoft.com/library/dn246937.aspx).
   * [Migreren naar de In-Memory OLTP](https://msdn.microsoft.com/library/dn247639.aspx).
4. Als de tabel geen niet-ondersteunde functies heeft, kan de advisor het werkelijke schema en de migratie van gegevens voor u uitvoeren.

#### <a name="manual-t-sql"></a>Handmatige T-SQL
Gebruik deze migratieoptie:

1. Verbinding maken met uw testdatabase met behulp van SSMS (of een vergelijkbaar hulpprogramma).
2. Het volledige T-SQL-script voor de tabel en de indexen ervan verkrijgen.
   
   * In SSMS, met de rechtermuisknop op het tabelknooppunt voor de.
   * Klik op **tabel als een Script** > **maken naar** > **nieuwe queryvenster**.
3. In het scriptvenster toevoegen met (MEMORY_OPTIMIZED = ON) in de instructie CREATE TABLE.
4. Als er een GECLUSTERDE index is, kunt u deze naar NONCLUSTERED wijzigen.
5. Wijzig de naam van de bestaande tabel met behulp van SP_RENAME.
6. Maak de nieuwe geoptimaliseerd voor geheugen-kopie van de tabel de bewerkte CREATE TABLE-script uit te voeren.
7. De gegevens aan uw tabel geoptimaliseerd voor geheugen kopiëren met behulp van INSERT... SELECTEER * IN:

```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>Stap 5 (optioneel): Migreren van opgeslagen procedures
Een opgeslagen procedure voor betere prestaties kan ook wijzigen door de In-Memory-functie.

### <a name="considerations-with-natively-compiled-stored-procedures"></a>Overwegingen met systeemeigen, gecompileerde, opgeslagen procedures
Een systeemeigen, gecompileerde, opgeslagen procedure moet op de component T-SQL met de volgende opties:

* WITH NATIVE_COMPILATION
* SCHEMABINDING: wat betekent dat tabellen waarvoor de opgeslagen procedure kan niet de kolomdefinities gewijzigd op een manier die van invloed zijn op de opgeslagen procedure, tenzij u de opgeslagen procedure verwijderen.

Een systeemeigen module moet gebruiken een grote [ATOMIC-blokken](https://msdn.microsoft.com/library/dn452281.aspx) voor het transactiebeheer van de. Er is geen rol voor een expliciete BEGIN TRANSACTION of ROLLBACK TRANSACTION. Als uw code wordt gedetecteerd door een schending van een bedrijfsregel, kan het afsluiten atomic-blok met een [THROW](https://msdn.microsoft.com/library/ee677615.aspx) instructie.

### <a name="typical-create-procedure-for-natively-compiled"></a>Typische CREATE PROCEDURE voor systeemeigen, gecompileerde
Normaal gesproken is de T-SQL te maken van een systeemeigen, gecompileerde, opgeslagen procedure vergelijkbaar met de volgende sjabloon:

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
  
  * HERHAALBARE LEZEN
  * SERIALIZOVAT.
* De waarde van de taal moet aanwezig zijn in de weergave sys.languages.

### <a name="how-to-migrate-a-stored-procedure"></a>Het migreren van een opgeslagen procedure
De migratiestappen zijn:

1. Het script CREATE PROCEDURE om de normale geïnterpreteerde opgeslagen procedure te verkrijgen.
2. Herschrijf de header zodat deze overeenkomt met de vorige sjabloon.
3. Nagaan of de opgeslagen procedure T-SQL-code maakt gebruik van alle functies die worden niet ondersteund voor systeemeigen, gecompileerde, opgeslagen procedures. Implementeer oplossingen indien nodig.
   
   * Zie voor meer informatie [migratieproblemen voor systeemeigen, gecompileerde, opgeslagen Procedures](https://msdn.microsoft.com/library/dn296678.aspx).
4. Wijzig de naam van de oude opgeslagen procedure met behulp van SP_RENAME. Of u gewoon.
5. Voer uw bewerkte CREATE PROCEDURE T-SQL-script uit.

## <a name="step-6-run-your-workload-in-test"></a>Stap 6: Uw workload wordt uitgevoerd in de test
Een workload in de testdatabase die vergelijkbaar is met de werkbelasting die wordt uitgevoerd in de productiedatabase uitvoeren. Dit zou moeten uitwijzen de prestatieverbetering te bereiken bereikt door uw gebruik van de In-Memory-functie voor tabellen en opgeslagen procedures.

Belangrijke kenmerken van de werkbelasting zijn:

* Het aantal gelijktijdige verbindingen.
* Verhouding tussen lezen/schrijven.

Als u wilt aanpassen en de workload van de test wordt uitgevoerd, kunt u overwegen de handige ostress.exe-programma, waarmee geïllustreerd in [hier](sql-database-in-memory.md).

Voor minimale netwerklatentie, voert u uw test in dezelfde Azure geografische regio waarin de database bestaat.

## <a name="step-7-post-implementation-monitoring"></a>Stap 7: Na de implementatie controleren
Houd rekening met de effecten van de prestaties van uw implementaties In het geheugen in de productie controleren:

* [Opslag In het geheugen controleren](sql-database-in-memory-oltp-monitoring.md).
* [Bewaking van Azure SQL-database met behulp van de dynamische beheerweergave](sql-database-monitoring-with-dmvs.md)

## <a name="related-links"></a>Verwante koppelingen
* [In-Memory OLTP (optimalisatie In het geheugen)](https://msdn.microsoft.com/library/dn133186.aspx)
* [Inleiding tot systeemeigen, gecompileerde, opgeslagen Procedures](https://msdn.microsoft.com/library/dn133184.aspx)
* [Geheugen optimalisatie Advisor](https://msdn.microsoft.com/library/dn284308.aspx)

