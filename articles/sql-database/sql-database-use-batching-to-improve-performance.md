---
title: Batchverwerking gebruiken voor het verbeteren van de prestaties van toepassingen voor Azure SQL Database
description: Het onderwerp bevat bewijs dat batchverwerkingsindeling databasebewerkingen aanzienlijk imroves de snelheid en schaalbaarheid van uw Azure SQL Database-toepassingen. Hoewel deze batchverwerkingsindeling technieken voor een SQL Server-database werken, wordt de focus van het artikel op Azure.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: genemi
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: f347543bbea11329cf4bb7c03dac6ccf7f04ac77
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55455385"
---
# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a>Batchverwerking gebruiken voor het verbeteren van de prestaties van toepassingen voor SQL-Database

Batchverwerking van bewerkingen met Azure SQL Database aanzienlijk verbetert de prestaties en schaalbaarheid van uw toepassingen. Om te begrijpen van de voordelen, het eerste deel van dit artikel bevat informatie over sommige voorbeeld testresultaten die sequentieel en batch-aanvragen voor een SQL-Database vergelijken. De rest van het artikel ziet u de technieken, scenario's en overwegingen bij het gebruik van batchverwerking is in uw Azure-toepassingen.

## <a name="why-is-batching-important-for-sql-database"></a>Waarom is belangrijk batchverwerking voor SQL-Database

Batchverwerking van aanroepen naar een externe service is een bekende strategie voor het verhogen van de prestaties en schaalbaarheid. Er zijn opgelost verwerking kosten voor elke interactie met een externe service, zoals serialisatie netwerkoverdracht en deserialisatie. Veel afzonderlijke transacties in één batch minimaliseert deze kosten.

In dit artikel gaan we verschillende SQL-Database batchverwerking strategieën en scenario's te onderzoeken. Hoewel deze strategieën ook belangrijk voor on-premises toepassingen die gebruikmaken van SQL Server zijn, zijn er diverse redenen voor het gebruik van de batchverwerking voor SQL-Database te markeren:

* Er is mogelijk groter netwerklatentie bij het openen van de SQL-Database, met name als u toegang wilt van SQL-Database van buiten het datacenter van de dezelfde Microsoft Azure tot.
* De multitenant kenmerken van SQL Database betekent dat de efficiëntie van de data access laag is afgestemd op de algehele schaalbaarheid van de database. SQL-Database moet voorkomen dat een enkele tenant/gebruiker databaseresources ten koste van andere tenants belasten. In reactie op het gebruik dat vooraf gedefinieerde quota overschrijdt, SQL-Database doorvoer beperken of hierop reageren met beperking van uitzonderingen. Efficiëntie, zoals batchverwerking, kunnen u meer werk op de SQL-Database voordat deze limiet wordt bereikt. 
* Batchverwerking is ook geschikt voor architecturen met meerdere databases (sharding). De efficiëntie van uw interactie met elke eenheid van de database is nog steeds een belangrijke factor in uw algehele schaalbaarheid. 

Een van de voordelen van het gebruik van SQL-Database is dat u geen hebt voor het beheren van de servers die als host van de database. Deze beheerde infrastructuur ook betekent echter dat u moet denken anders over optimalisatie van de database. U kunt niet meer zoeken ter verbetering van de database-infrastructuur voor hardware- of netwerk. Microsoft Azure bepaalt deze omgevingen. De belangrijkste aspect die u kunt beheren, is hoe uw toepassing communiceert met SQL-Database. Batchverwerking is een van deze optimalisaties. 

Het eerste deel van het document onderzoekt verschillende batchverwerkingsindeling technieken voor het .NET-toepassingen die gebruikmaken van SQL-Database. De laatste twee secties behandelen batchverwerkingsindeling richtlijnen en scenario's.

## <a name="batching-strategies"></a>Strategieën voor batchverwerking

### <a name="note-about-timing-results-in-this-article"></a>Houd er rekening mee over de resultaten van de timing in dit artikel

> [!NOTE]
> Resultaten niet benchmarks, maar zijn bedoeld om weer te geven **relatieve prestaties**. Tijdsinstellingen zijn gebaseerd op een gemiddelde van ten minste 10 testuitvoeringen. Bewerkingen worden ingevoegd in een lege tabel. Deze tests zijn gemeten pre-V12, en ze niet per se overeenkomen met de doorvoer die u kunt tegenkomen in een V12-database met behulp van de nieuwe [DTU-Servicelagen](sql-database-service-tiers-dtu.md) of [vCore-servicelaag](sql-database-service-tiers-vcore.md). De relatieve voordelen van de batchverwerkingsindeling techniek moet vergelijkbaar zijn.

### <a name="transactions"></a>Transacties

Om te beginnen met een overzicht van door het bespreken van transacties batchverwerking vreemd lijkt. Maar het gebruik van de client-side transacties heeft een subtiel batchverwerkingsindeling effect voor serverzijde, die zorgt voor betere prestaties. En transacties kunnen worden toegevoegd met slechts een paar regels code, zodat ze bieden een snelle manier om de prestaties van opeenvolgende bewerkingen te verbeteren.

Houd rekening met de volgende C#-code met een reeks invoegen en bijwerken van bewerkingen op een eenvoudige tabel.

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

De volgende code in de ADO.NET uitvoert sequentieel deze bewerkingen.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();

        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

De beste manier om het optimaliseren van deze code is het implementeren van een vorm van de client-side batchverwerking van deze aanroepen. Maar er is een eenvoudige manier om de prestaties van deze code verhogen door gewoon de volgorde van de aanroepen in een transactie. Dit is dezelfde code die gebruikmaakt van een transactie.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
        SqlTransaction transaction = conn.BeginTransaction();

        foreach (string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
            cmd.ExecuteNonQuery();
        }

        transaction.Commit();
    }

Transacties worden daadwerkelijk in deze voorbeelden gebruikt. In het eerste voorbeeld is elke afzonderlijke aanroep een impliciete transactie. In het tweede voorbeeld wordt verpakt een expliciete transactie alle van de aanroepen. Per de documentatie voor de [write-ahead transactielogboek](https://msdn.microsoft.com/library/ms186259.aspx), logboekrecords worden weggeschreven naar de schijf als de transactie wordt doorgevoerd. Door op te nemen meer aanroepen in een transactie, kan het schrijven naar het transactielogboek dus uitstellen totdat de transactie doorgevoerd wordt. U stelt in feite batchverwerking voor het schrijven naar het transactielogboek van de server.

De volgende tabel ziet enkele tests ad-hoc-resultaten. De tests uitgevoerd de opeenvolgende invoegt met en zonder transacties. Voor meer perspectief, de eerste set met tests die worden uitgevoerd op afstand van een laptop met de database in Microsoft Azure. De tweede reeks tests uit vanaf een service in de cloud en de database dat beide zich in hetzelfde Microsoft Azure datacenter (VS West bevinden) uitgevoerd. De volgende tabel ziet u de duur in milliseconden van opeenvolgende ingevoegd met en zonder transacties.

**On-Premises naar Azure**:

| Bewerkingen | Er is geen transactie (ms) | Transactie (ms) |
| --- | --- | --- |
| 1 |130 |402 |
| 10 |1208 |1226 |
| 100 |12662 |10395 |
| 1000 |128852 |102917 |

**Azure naar Azure (hetzelfde datacenter)**:

| Bewerkingen | Er is geen transactie (ms) | Transactie (ms) |
| --- | --- | --- |
| 1 |21 |26 |
| 10 |220 |56 |
| 100 |2145 |341 |
| 1000 |21479 |2756 |

> [!NOTE]
> Geen zijn resultaten benchmarks. Zie de [opmerking over timing resultaten in dit artikel](#note-about-timing-results-in-this-article).

Op basis van de resultaten van de vorige, één bewerking in een transactie wrapping daadwerkelijk wordt verlaagd prestaties. Maar als u het aantal bewerkingen in één transactie verhoogt, de prestaties verbeteren meer wordt gemarkeerd. Het prestatieverschil is ook opvallender wanneer alle bewerkingen uitgevoerd binnen de Microsoft Azure-datacenter. De latentie van het gebruik van SQL-Database van buiten de Microsoft Azure-datacenter overshadows de prestatieverbetering van het gebruik van transacties.

Hoewel het gebruik van transacties de prestaties verbeteren kan, blijven [observeren aanbevolen procedures voor transacties en verbindingen](https://msdn.microsoft.com/library/ms187484.aspx). De transactie zo kort mogelijk te houden en sluit de databaseverbinding nadat het werk is voltooid. De met de instructie in het vorige voorbeeld zorgt ervoor dat de verbinding wordt gesloten wanneer het volgende codeblok is voltooid.

Het vorige voorbeeld ziet u dat u een lokale transactie aan een ADO.NET-code met twee regels toevoegen kunt. Transacties bieden een snelle manier om de prestaties van code waarmee opeenvolgende invoegen, bijwerken en verwijderen van bewerkingen worden verbeterd. U kunt echter voor de code verder profiteren van de client-side batches, zoals tabelwaardeparameters wijzigen voor de snelste prestaties.

Zie voor meer informatie over transacties in ADO.NET [lokale transacties in ADO.NET](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).

### <a name="table-valued-parameters"></a>tabelwaardeparameters

Tabelwaardeparameters ondersteunen de gebruiker gedefinieerde tabeltypen als parameters in de Transact-SQL-instructies en opgeslagen procedures en functies. Deze client-side batchverwerkingsindeling techniek kunt u meerdere rijen met gegevens in de tabelwaardeparameter verzenden. Voor het gebruik van tabelwaardeparameters, moet u eerst een tabeltype definiëren. De volgende Transact-SQL-instructie maakt een tabeltype met de naam **MyTableType**.

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );


In code, maakt u een **DataTable** met exact dezelfde namen en typen van het tabeltype. Deze wordt doorgegeven **DataTable** aanroepen in een parameter in een tekstquery of een opgeslagen procedure. Het volgende voorbeeld ziet u deze techniek:

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        DataTable table = new DataTable();
        // Add columns and rows. The following is a simple example.
        table.Columns.Add("mytext", typeof(string));
        table.Columns.Add("num", typeof(int));    
        for (var i = 0; i < 10; i++)
        {
            table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
        }

        SqlCommand cmd = new SqlCommand(
            "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
            connection);

        cmd.Parameters.Add(
            new SqlParameter()
            {
                ParameterName = "@TestTvp",
                SqlDbType = SqlDbType.Structured,
                TypeName = "MyTableType",
                Value = table,
            });

        cmd.ExecuteNonQuery();
    }

In het vorige voorbeeld de **SqlCommand** object voegt rijen uit een tabelwaardeparameter **@TestTvp**. De eerder gemaakte **DataTable** object is toegewezen aan deze parameter met de **SqlCommand.Parameters.Add** methode. De invoegingen in één aanroep aanzienlijk batchverwerking, verhoogt de prestaties via opeenvolgende ingevoegd.

Ter verbetering van het vorige voorbeeld meer, een opgeslagen procedure te gebruiken in plaats van een opdracht op basis van tekst. De volgende Transact-SQL-opdracht maakt u een opgeslagen procedure waarmee de **SimpleTestTableType** tabelwaardeparameter.

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

Wijzig de **SqlCommand** declaratie in het vorige codevoorbeeld in de volgende object.

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

In de meeste gevallen hebben tabelwaardeparameters equivalent of betere prestaties dan andere batchverwerkingsindeling technieken. Tabelwaardeparameters zijn vaak beter, omdat ze meer flexibiliteit dan andere opties zijn. Andere methoden, zoals bulkgewijs kopiëren voor SQL, kunnen bijvoorbeeld alleen het invoegen van nieuwe rijen. Maar met tabelwaardeparameters, kunt u logische gebruiken in de opgeslagen procedure om te bepalen welke rijen zijn updates en die zijn ingevoegd. Het tabeltype kan ook worden gewijzigd als u wilt een 'Bewerking' kolom bevatten die wordt aangegeven of de opgegeven rij moet worden ingevoegd, bijgewerkt of verwijderd.

De volgende tabel ziet de resultaten van ad-hoc voor het gebruik van tabelwaardeparameters in milliseconden.

| Bewerkingen | On-Premises naar Azure (ms) | Azure hetzelfde datacenter (ms) |
| --- | --- | --- |
| 1 |124 |32 |
| 10 |131 |25 |
| 100 |338 |51 |
| 1000 |2615 |382 |
| 10.000 |23830 |3586 |

> [!NOTE]
> Geen zijn resultaten benchmarks. Zie de [opmerking over timing resultaten in dit artikel](#note-about-timing-results-in-this-article).
> 
> 

De prestatieverbetering van batchverwerking is onmiddellijk zichtbaar. In de vorige opeenvolgende test duurde 1000 operations 129 seconden buiten het datacenter en enkele 21 seconden in het datacenter. Maar met tabelwaardeparameters, 1000 bewerkingen alleen 2,6 seconden buiten het datacenter en 0,4 seconden in het datacenter.

Zie voor meer informatie over tabelwaardeparameters [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).

### <a name="sql-bulk-copy"></a>Bulkgewijs kopiëren voor SQL

Bulkgewijs kopiëren voor SQL is een andere manier om grote hoeveelheden gegevens invoegen in een doeldatabase. .NET-toepassingen kunnen gebruikmaken van de **SqlBulkCopy uitvoert** bewerkingen klasse om uit te voeren bulksgewijs invoegen. **SqlBulkCopy uitvoert** lijkt in de functie op de opdrachtregel-hulpprogramma **Bcp.exe**, of de Transact-SQL-instructie **BULK INSERT**. Het volgende codevoorbeeld toont het bulksgewijs kopiëren de rijen in de bron **DataTable**, tabel, naar de doeltabel in SQL Server, MyTable.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
        {
            bulkCopy.DestinationTableName = "MyTable";
            bulkCopy.ColumnMappings.Add("mytext", "mytext");
            bulkCopy.ColumnMappings.Add("num", "num");
            bulkCopy.WriteToServer(table);
        }
    }

Er zijn enkele gevallen waarbij bulksgewijs kopiëren heeft de voorkeur boven tabelwaardeparameters. Zie de vergelijkingstabel met tabelwaardeparameters versus BULK INSERT-bewerkingen in het artikel [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).

De resultaten van de volgende ad-hoc geven de prestaties van de batchverwerking met **SqlBulkCopy uitvoert** in milliseconden.

| Bewerkingen | On-Premises naar Azure (ms) | Azure hetzelfde datacenter (ms) |
| --- | --- | --- |
| 1 |433 |57 |
| 10 |441 |32 |
| 100 |636 |53 |
| 1000 |2535 |341 |
| 10.000 |21605 |2737 |

> [!NOTE]
> Geen zijn resultaten benchmarks. Zie de [opmerking over timing resultaten in dit artikel](#note-about-timing-results-in-this-article).
> 
> 

In batch, kleinere, de parameters gebruiken tabelwaarden beter dan de **SqlBulkCopy uitvoert** klasse. Echter, **SqlBulkCopy uitvoert** 12-31% sneller zijn dan tabelwaardeparameters uitgevoerd voor de tests van 1000 en 10.000 rijen. Tabelwaardeparameters, zoals **SqlBulkCopy uitvoert** is een goede optie voor batch voegt, met name wanneer ten opzichte van de prestaties van bewerkingen niet batchgewijs.

Zie voor meer informatie over het bulksgewijs kopiëren in ADO.NET, [Bulksgewijze kopieerbewerkingen in SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).

### <a name="multiple-row-parameterized-insert-statements"></a>Meerdere rijen met parameters INSERT-instructies

Er is een alternatief voor kleine batches te maken van een grote, geparameteriseerde INSERT-instructie is die op meerdere rijen worden ingevoegd. Het volgende codevoorbeeld ziet u deze techniek.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
            "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";

        SqlCommand cmd = new SqlCommand(insertCommand, connection);

        for (int i = 1; i <= 10; i += 2)
        {
            cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
            cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
        }

        cmd.ExecuteNonQuery();
    }


In dit voorbeeld is bedoeld om het basisconcept weer te geven. Een realistischer scenario zou de vereiste entiteiten te maken van de query-tekenreeks en de opdrachtparameters tegelijkertijd doorlopen. U bent beperkt tot een totaal van 2100 queryparameters, zodat dit beperkt het totale aantal rijen dat op deze manier kan worden verwerkt.

De resultaten van de volgende ad-hoc geven de prestaties van dit type insert-instructie in milliseconden.

| Bewerkingen | Tabelwaardeparameters (ms) | Één instructie INSERT (ms) |
| --- | --- | --- |
| 1 |32 |20 |
| 10 |30 |25 |
| 100 |33 |51 |

> [!NOTE]
> Geen zijn resultaten benchmarks. Zie de [opmerking over timing resultaten in dit artikel](#note-about-timing-results-in-this-article).
> 
> 

Deze methode kan zijn voor batches die kleiner zijn dan 100 rijen zijn iets sneller. Hoewel de verbetering klein is, is deze techniek een andere optie waarbij ook in uw specifieke toepassingsscenario werken mogelijk.

### <a name="dataadapter"></a>DataAdapter

De **DataAdapter** klasse kunt u aanpassen een **gegevensset** object en vervolgens verzendt de wijzigingen Als INSERT, UPDATE en DELETE-bewerkingen. Als u de **DataAdapter** op deze manier is het belangrijk te weten dat afzonderlijke aanroepen worden gedaan voor elke bewerking distinct worden geleverd. Gebruik voor betere prestaties, de **UpdateBatchSize** eigenschap aan het aantal bewerkingen die op een tijdstip in batch moet worden opgenomen. Zie voor meer informatie, [uitvoeren van Batch-bewerkingen met behulp van DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).

### <a name="entity-framework"></a>Entity framework

Entity Framework ondersteunt momenteel geen batchverwerking. Andere ontwikkelaars in de community om te demonstreren oplossingen, zoals onderdrukking hebt geprobeerd de **SaveChanges** methode. Maar de oplossingen zijn normaal complexe en afgestemd op de toepassing en het gegevensmodel. Het Entity Framework codeplex project heeft momenteel een discussiepagina voor deze functieaanvraag. Als u deze discussie, Zie [notulen ontwerp - en met 2 augustus 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).

### <a name="xml"></a>XML

Voor de volledigheid dat we dat het is belangrijk om te praten over XML als een batchverwerkingsindeling strategie. Het gebruik van XML heeft echter geen voordelen ten opzichte van andere methoden en enkele nadelen. De aanpak is vergelijkbaar met tabelwaardeparameters, maar een XML-bestand of de tekenreeks wordt doorgegeven aan een opgeslagen procedure in plaats van een gebruiker gedefinieerde tabel. De opgeslagen procedure parseert de opdrachten in de opgeslagen procedure.

Er zijn enkele nadelen deze benadering:

* Werken met XML kan zijn verkeerd ingevoerde adressen en foutgevoelig.
* Het parseren van het XML-bestand op de database, kan CPU-intensief zijn.
* Deze methode is langzamer dan tabelwaardeparameters in de meeste gevallen.

Daarom is het gebruik van XML voor batch-query's niet aanbevolen.

## <a name="batching-considerations"></a>Overwegingen voor batchverwerking

De volgende secties bevatten meer richtlijnen voor het gebruik van de batchverwerking in SQL Database-toepassingen.

### <a name="tradeoffs"></a>Tradeoffs

Afhankelijk van uw architectuur batchverwerking kan betrekking hebben op een balans tussen prestaties en tolerantie. Bijvoorbeeld, kunt u het scenario waarbij uw rol onverwacht uitvalt. Als u één rij met gegevens verliest, is de impact kleiner dan de gevolgen van het verlies van een grote batch van niet-verzonden rijen. Er is een groter risico wanneer u de rijen buffer voordat ze worden verzonden naar de database in een opgegeven periode.

Evalueren vanwege dit negatieve gevolgen voor de, het type van de bewerkingen die partij u. Batch-agressiever (grotere batches en langer tijdvensters) met gegevens die minder belangrijk.

### <a name="batch-size"></a>Batchgrootte

In onze tests is er meestal geen voordeel te grote batches te verdelen in kleinere chunks. Deze onderverdeling vaak geresulteerd in feite in trager dan een enkele grote batch te verzenden. Neem bijvoorbeeld een scenario waarin u wilt 1000 rijen worden ingevoegd. De volgende tabel ziet u hoe lang het duurt tabelwaardeparameters gebruiken om in te voegen 1000 rijen als verdeeld in kleinere batches.

| Batchgrootte | Iteraties | Tabelwaardeparameters (ms) |
| --- | --- | --- |
| 1000 |1 |347 |
| 500 |2 |355 |
| 100 |10 |465 |
| 50 |20 |630 |

> [!NOTE]
> Geen zijn resultaten benchmarks. Zie de [opmerking over timing resultaten in dit artikel](#note-about-timing-results-in-this-article).
> 
> 

U kunt zien dat de beste prestaties voor 1000 rijen wordt deze in één keer verzenden. In andere tests (hier niet weergegeven), is er een kleine prestatieverbetering te bereiken om een batch 10000 rij in twee batches van 5000. Maar het tabelschema voor deze tests is vrij eenvoudig, zodat u tests moet uitvoeren op uw specifieke gegevens en de batch-grootten om te controleren of deze bevindingen.

Een andere factor te houden is dat als de totale batch te groot wordt, SQL-Database mogelijk beperken en weigeren om door te voeren van de batch. Test uw specifieke scenario om te bepalen of er een ideale batchgrootte voor de beste resultaten. De batchgrootte configureerbaar maken tijdens runtime om in te schakelen snelle aanpassingen op basis van prestaties of fouten.

Ten slotte taakverdeling tussen de grootte van de batch met de risico's die zijn gekoppeld aan de batchverwerking. Als er tijdelijke fouten of de rol is mislukt, kunt u de gevolgen van de bewerking opnieuw te proberen of verlies van de gegevens in de batch.

### <a name="parallel-processing"></a>Parallelle verwerking

Wat gebeurt er als u de aanpak van het verkleinen van de batch heeft maar meerdere threads gebruikt voor het uitvoeren van het werk? Nogmaals, hebt u onze tests geleerd dat verschillende kleinere met meerdere threads batches doorgaans uitgevoerd slechter dan één grotere batch. De volgende test probeert 1000 rijen invoegen in een of meer parallelle batches. Deze test laat zien hoe meer gelijktijdige batches nemen de prestaties daadwerkelijk af.

| Batchgrootte [iteraties] | Twee threads (ms) | Vier threads (ms) | Zes threads (ms) |
| --- | --- | --- | --- |
| 1000 [1] |277 |315 |266 |
| 500 [2] |548 |278 |256 |
| 250 [4] |405 |329 |265 |
| 100 [10] |488 |439 |391 |

> [!NOTE]
> Geen zijn resultaten benchmarks. Zie de [opmerking over timing resultaten in dit artikel](#note-about-timing-results-in-this-article).
> 
> 

Er zijn verschillende mogelijke redenen voor de afname van prestaties vanwege de parallelle uitvoering:

* Er zijn meerdere gelijktijdige netwerkaanroepen in plaats van een.
* Meerdere bewerkingen op een enkele tabel kunnen leiden tot conflicten en blokkeren.
* Er zijn overhead die is gekoppeld aan multithreading.
* De kosten van het openen van meerdere verbindingen belangrijker is dan het voordeel van parallelle verwerking.

Als u verschillende tabellen of databases als doel, is het mogelijk om te zien krijgen met deze strategie prestaties. Database sharding of Federatie is een scenario voor deze benadering. Sharding maakt gebruik van meerdere databases en andere gegevens routeert naar elke database. Als elke kleine batch wordt gebruikt met een andere database, kan vervolgens het uitvoeren van de bewerkingen gelijktijdig zijn efficiënter. De prestatieverbetering is echter geen aanzienlijke te gebruiken als basis voor een beslissing database sharding gebruiken in uw oplossing.

Parallelle uitvoering van de kleinere batches kan in sommige ontwerpen resulteren in betere doorvoer van aanvragen in een systeem belast. In dit geval, zelfs als het is sneller voor het verwerken van een enkele grotere batch, kan verwerken van meerdere batches parallel zijn efficiënter.

Als u parallelle uitvoering gebruikt, kunt u het maximum aantal werkthreads beheren. Een kleiner getal kan leiden tot minder conflicten en een snellere time-uitvoering. Overweeg ook de extra belasting die door dit in de doeldatabase in verbindingen en transacties geplaatst.

### <a name="related-performance-factors"></a>Prestaties gerelateerd aan factoren

Typische richtlijnen voor prestaties van de database is ook van invloed op de batchverwerking. Voeg bijvoorbeeld prestaties voor tabellen met een primaire sleutel voor grote of veel niet-geclusterde indexen wordt verminderd.

Als tabelwaardeparameters een opgeslagen procedure gebruikt, kunt u de opdracht **SET NOCOUNT ON** aan het begin van de procedure. Deze instructie onderdrukt het rendement van de telling van de beïnvloede rijen in de procedure. Echter, in onze tests, het gebruik van **SET NOCOUNT ON** of heeft geen effect nemen de prestaties af. De opgeslagen testprocedure is eenvoudig met één **invoegen** opdracht uit vanaf de tabelwaardeparameter. Het is mogelijk dat meer complexe opgeslagen procedures voordeel van deze instructie hebben veel. Maar niet vanuit gegaan dat toe te voegen **SET NOCOUNT ON** aan uw opgeslagen procedure automatisch zorgt voor betere prestaties. Voor meer informatie over het effect, test u de opgeslagen procedure met en zonder de **SET NOCOUNT ON** instructie.

## <a name="batching-scenarios"></a>Batchverwerking van scenario 's

De volgende secties wordt beschreven hoe u tabelwaardeparameters in drie scenario's voor application gebruiken. Het eerste scenario laat zien hoe buffer en batchverwerking kunnen samenwerken. Het tweede scenario verbetert de prestaties door het uitvoeren van de operations master-details in de aanroep van een enkele opgeslagen procedure. Het laatste scenario ziet u hoe u tabelwaardeparameters in een "UPSERT"-bewerking.

### <a name="buffering"></a>Buffer

Hoewel er enkele scenario's die voor de hand liggende kandidaat zijn voor batchverwerking zijn, zijn er veel scenario's die profiteren kunnen zou van de batchverwerking door vertraagde verwerking. Vertraagde verwerking wordt echter ook een groter risico dat de gegevens verloren gaan in het geval van een onverwachte fout is. Het is belangrijk om te begrijpen dit risico en houd rekening met de gevolgen.

Neem bijvoorbeeld een webtoepassing waarin de navigatiegeschiedenis van elke gebruiker worden bijgehouden. Op elke pagina-aanvraag kan de toepassing maken van een database aanroepen om vast te leggen van de weergave van de pagina van de gebruiker. Maar hogere prestaties en schaalbaarheid kunnen worden bereikt door het bufferen van de gebruikers navigatie activiteiten en vervolgens deze gegevens te verzenden naar de database in batches. U kunt de database bijwerken door de verstreken tijd en/of buffergrootte activeren. Een regel kan bijvoorbeeld opgeven dat de batch moet worden verwerkt na 20 seconden of wanneer de buffer 1000 items bereikt.

Het volgende codevoorbeeld wordt [Reactive Extensions - Rx](https://msdn.microsoft.com/data/gg577609) gebufferde gebeurtenissen die worden gegenereerd door een controleklasse te verwerken. Wanneer u de buffer of een time-out is bereikt, wordt de batch van gebruikersgegevens is verzonden naar de database met een tabelwaardeparameter.

De volgende NavHistoryData-klasse Gebruikersdetails voor de navigatie-modellen. Het bevat algemene informatie zoals de gebruikers-id, de URL geopend en de tijd van toegang.

```c#
    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }
```

De klasse NavHistoryDataMonitor is verantwoordelijk voor het bufferen van gegevens van de navigatie in de database. Deze bevat een methode, RecordUserNavigationEntry deze door in te dienen reageert een **OnAdded** gebeurtenis. De volgende code toont de constructor logica die gebruikmaakt van Rx om een waarneembare verzameling op basis van de gebeurtenis te maken. Deze zich abonneert op deze waarneembare verzameling met de methode Buffer. De overbelasting geeft aan dat de buffer elke 20 seconden of 1000 vermeldingen moet worden verzonden.

```c#
    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }
```

De handler alle gebufferde items converteert naar een type tabelwaarden en dit type wordt doorgegeven aan een opgeslagen procedure waarmee de batch wordt verwerkt. De volgende code toont de definitie van de volledige voor zowel de NavHistoryDataEventArgs en de klassen NavHistoryDataMonitor.

```c#
    public class NavHistoryDataEventArgs : System.EventArgs
    {
        public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
        public NavHistoryData Data { get; set; }
    }

    public class NavHistoryDataMonitor
    {
        public event EventHandler<NavHistoryDataEventArgs> OnAdded;

        public NavHistoryDataMonitor()
        {
            var observableData =
                Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

            observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
        }

        public void RecordUserNavigationEntry(NavHistoryData data)
        {    
            if (OnAdded != null)
                OnAdded(this, new NavHistoryDataEventArgs(data));
        }

        protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
        {
            DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
            navHistoryBatch.Columns.Add("UserId", typeof(int));
            navHistoryBatch.Columns.Add("URL", typeof(string));
            navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
            foreach (EventPattern<NavHistoryDataEventArgs> item in items)
            {
                NavHistoryData data = item.EventArgs.Data;
                navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
            }

            using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
            {
                connection.Open();

                SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
                cmd.CommandType = CommandType.StoredProcedure;

                cmd.Parameters.Add(
                    new SqlParameter()
                    {
                        ParameterName = "@NavHistoryBatch",
                        SqlDbType = SqlDbType.Structured,
                        TypeName = "NavigationHistoryTableType",
                        Value = navHistoryBatch,
                    });

                cmd.ExecuteNonQuery();
            }
        }
    }
```

De toepassing maakt voor het gebruik van deze buffer klasse, een statisch NavHistoryDataMonitor-object. Telkens wanneer die een gebruiker toegang heeft tot een pagina, aanroept de toepassing de NavHistoryDataMonitor.RecordUserNavigationEntry-methode. De buffer logica wordt voortgezet om te zorgen voor deze vermeldingen te verzenden naar de database in batches.

### <a name="master-detail"></a>Details van master

Tabelwaardeparameters zijn handig voor eenvoudige INSERT-scenario's. Het kan echter zijn moeilijker te batch voegt die betrekking hebben op meer dan één tabel. Het scenario ' master/Details' is een goed voorbeeld. De hoofdtabel identificeert de primaire entiteit. Een of meer detailtabellen opslaan meer gegevens over de entiteit. Refererende-sleutelrelaties afdwingen in dit scenario wordt de relatie van details aan een unieke master-entiteit. Houd rekening met een vereenvoudigde versie van een tabel PurchaseOrder en de bijbehorende OrderDetail-tabel. De volgende Transact-SQL maakt de tabel PurchaseOrder met vier kolommen: Order-id, OrderDate, CustomerID en Status.

```sql
    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))
```

Elke bestelling bevat een of meer kopen van een product. Deze informatie wordt vastgelegd in de tabel PurchaseOrderDetail. De volgende Transact-SQL maakt de PurchaseOrderDetail-tabel met vijf kolommen: Order-id, OrderDetailID, product-id, PrijsPerEenheid en Bestelhoev.

```sql
    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))
```

De kolom OrderID in de tabel PurchaseOrderDetail moet verwijzen naar een order uit de tabel PurchaseOrder. De volgende definitie van een refererende sleutel worden afgedwongen voor deze beperking.

```sql
    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])
```

Als u wilt gebruiken tabelwaardeparameters, moet u één gebruiker gedefinieerd tabeltype voor elke doeltabel hebben.

```sql
    CREATE TYPE PurchaseOrderTableType AS TABLE 
    ( OrderID INT,
      OrderDate DATETIME,
      CustomerID INT,
      Status NVARCHAR(50) );
    GO

    CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
    ( OrderID INT,
      ProductID INT,
      UnitPrice MONEY,
      OrderQty SMALLINT );
    GO
```

Vervolgens definieert u een opgeslagen procedure die tabellen van de volgende typen accepteert. Deze procedure kunt batch-lokaal een reeks orders en ordergegevens in één aanroep van een toepassing. De volgende Transact-SQL biedt de declaratie voltooid opgeslagen procedure in dit voorbeeld van de volgorde van aankoop.

```sql
    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;

    -- Table that connects the order identifiers in the @orders
    -- table with the actual order identifiers in the PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );

          -- Add new orders to the PurchaseOrder table, storing the actual
    -- order identifiers in the @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;

    -- Match the passed-in order identifiers with the actual identifiers
    -- and complete the @IdentityLink table for use with inserting the details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;

    -- Insert the order details into the PurchaseOrderDetail table, 
          -- using the actual order identifiers of the master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO
```

In dit voorbeeld wordt de lokaal gedefinieerde @IdentityLink tabel bevat de werkelijke waarden van de order-id van de ingevoegde rijen. Deze volgorde-id's wijken af van de tijdelijke waarden in de order-id in de @orders en @details tabelwaardeparameters. Om deze reden de @IdentityLink tabel maakt vervolgens verbinding met de order-id-waarden uit de @orders parameter voor de echte order-id-waarden voor de nieuwe rijen in de tabel PurchaseOrder. Na deze stap wordt de @IdentityLink tabel invoegen van de details van de order met de werkelijke OrderID die voldoet aan de foreign key-beperking kunt vereenvoudigen.

Deze opgeslagen procedure kan worden gebruikt vanuit code of uit andere Transact-SQL-aanroepen. Zie de sectie tabelwaardeparameters van dit document voor een codevoorbeeld van. De volgende Transact-SQL laat zien hoe de sp_InsertOrdersBatch aanroepen.

```sql
    declare @orders as PurchaseOrderTableType
    declare @details as PurchaseOrderDetailTableType

    INSERT @orders 
    ([OrderID], [OrderDate], [CustomerID], [Status])
    VALUES(1, '1/1/2013', 1125, 'Complete'),
    (2, '1/13/2013', 348, 'Processing'),
    (3, '1/12/2013', 2504, 'Shipped')

    INSERT @details
    ([OrderID], [ProductID], [UnitPrice], [OrderQty])
    VALUES(1, 10, $11.50, 1),
    (1, 12, $1.58, 1),
    (2, 23, $2.57, 2),
    (3, 4, $10.00, 1)

    exec sp_InsertOrdersBatch @orders, @details
```

Deze oplossing kunt elke batch voor gebruik van een set van order-id-waarden die bij 1 beginnen. Deze tijdelijke OrderID waarden beschrijven de relaties in de batch, maar de werkelijke waarden van de order-id op het moment van de bewerking insert worden bepaald. U kunt dezelfde instructies herhaaldelijk uitvoeren in het vorige voorbeeld en genereren van unieke orders in de database. Overweeg meer code of database logica waarmee wordt voorkomen dubbele orders dat wanneer u deze techniek batchverwerking om deze reden.

In dit voorbeeld ziet u dat nog meer complexe databasebewerkingen, zoals operations master-details, kunnen in batch worden opgenomen met tabelwaarden parameters.

### <a name="upsert"></a>UPSERT

Een ander batchverwerkingsindeling scenario omvat het tegelijkertijd bijwerken van bestaande rijen en nieuwe rijen invoegen. Met deze bewerking wordt ook wel een "UPSERT" (update + invoegen)-bewerking genoemd. In plaats van afzonderlijke aanroepen invoegen en bijwerken, is de instructie MERGE het meest geschikt is voor deze taak. De instructie MERGE kan uitvoeren beide insert en update-bewerkingen in één aanroep.

Tabelwaardeparameters kunnen worden gebruikt met de instructie MERGE om uit te voeren van updates en toevoegingen. Neem bijvoorbeeld een vereenvoudigde werknemerstabel met de volgende kolommen: EmployeeID, FirstName, LastName, SocialSecurityNumber:

```sql
    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))
```

In dit voorbeeld kunt u het feit dat de SocialSecurityNumber uniek is voor het uitvoeren van een SAMENVOEGING van meerdere werknemers. Maak eerst de gebruiker gedefinieerde tabeltype:

```sql
    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO
```

Vervolgens maakt u een opgeslagen procedure of code schrijven die gebruikmaakt van de instructie MERGE voor het uitvoeren van de update en invoegen. Het volgende voorbeeld wordt de instructie MERGE op een tabelwaardeparameter @employees, van het type EmployeeTableType. De inhoud van de @employees tabel niet hier worden weergegeven.

```sql
    MERGE Employee AS target
    USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
    AS source ([FirstName], [LastName], [SocialSecurityNumber])
    ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
    WHEN MATCHED THEN 
    UPDATE SET
    target.FirstName = source.FirstName, 
    target.LastName = source.LastName
    WHEN NOT MATCHED THEN
       INSERT ([FirstName], [LastName], [SocialSecurityNumber])
       VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);
```

Zie voor meer informatie de documentatie en voorbeelden voor de instructie MERGE. Hoewel hetzelfde werk kan worden uitgevoerd in een opgeslagen meerdere stappen procedureaanroep met afzonderlijke INSERT en UPDATE-bewerkingen, de instructie MERGE is efficiënter. Databasecode kan ook bouwen voor Transact-SQL-aanroepen die gebruikmaken van de instructie MERGE rechtstreeks zonder dat twee databaseaanroepen voor INSERT en UPDATE.

## <a name="recommendation-summary"></a>Overzicht van de aanbeveling

De volgende lijst bevat een samenvatting van de batchverwerkingsindeling aanbevelingen die in dit artikel worden besproken:

* Gebruiken buffer en batchverwerking voor het verhogen van de prestaties en schaalbaarheid van SQL Database-toepassingen.
* Krijg inzicht in de balans tussen de batchverwerking/bufferfunctie en meer flexibiliteit. Tijdens een storing van de rol optreedt, kan het risico dat een niet-verwerkte batch van essentiële gegevens verloren gaan opweegt tegen de prestatievoordelen van batchverwerking.
* Poging tot het behouden van alle aanroepen naar de database binnen één datacenter te verminderen latentie.
* Als u een enkele batchverwerkingsindeling techniek kiest, wordt tabelwaardeparameters bieden de beste prestaties en flexibiliteit.
* Volg deze algemene richtlijnen voor de snelste prestaties invoegen maar testen van uw scenario:
  * Gebruik voor < 100 rijen, één met parameters invoegopdracht.
  * Gebruik voor < 1000 rijen, tabelwaardeparameters.
  * Voor > = 1000 rijen, gebruikt u SqlBulkCopy uitvoert.
* Voor bijwerken en verwijderen van bewerkingen, gebruikt u tabelwaardeparameters met de logica van de opgeslagen procedure die de juiste werking op elke rij in de tabel-parameter bepaalt.
* Richtlijnen voor batch-grootte:
  * Gebruik de grootste batch-grootten die geschikt zijn voor uw toepassing en de zakelijke vereisten.
  * De prestatieverbetering van grote batches met de risico's van tijdelijke of onherstelbare fouten in balans brengen. Wat is het gevolg van nieuwe pogingen of verlies van de gegevens in de batch? 
  * Test de grootste batchgrootte om te controleren dat SQL-Database wordt niet afwijzen.
  * Configuratie-instellingen maken dat besturingselement batchverwerking, zoals de batchgrootte van de of de buffer tijdvenster. Deze instellingen bieden flexibiliteit. U kunt de batchverwerkingsindeling gedrag in de productieomgeving wijzigen zonder de cloudservice opnieuw te implementeren.
* Parallelle uitvoering van batches die worden uitgevoerd op een enkele tabel in de ene database voorkomen. Als u één batch verdelen over meerdere werkthreads kiest, voert u tests uit om te bepalen het ideale aantal threads. Na een niet-opgegeven drempel, meer threads worden de prestaties afnemen in plaats van verhogen.
* Houd rekening met buffer grootte en hetzelfde moment als een manier van de implementatie van batchverwerking voor meer scenario's.

## <a name="next-steps"></a>Volgende stappen

In dit artikel gericht op hoe databaseontwerp en de technieken die betrekking hebben op de batchverwerking code de prestaties van toepassingen en schaalbaarheid verbeteren kunnen. Maar dit is slechts één factor in uw algehele strategie. Zie voor meer manieren om prestaties en schaalbaarheid te verbeteren, [richtlijnen voor Azure SQL Database-prestaties voor individuele databases](sql-database-performance-guidance.md) en [prijs- en Prestatieoverwegingen voor een elastische pool](sql-database-elastic-pool-guidance.md).

