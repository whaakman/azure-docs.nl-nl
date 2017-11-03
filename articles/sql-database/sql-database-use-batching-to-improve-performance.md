---
title: Het gebruik van batchverwerking voor het verbeteren van de toepassingsprestaties Azure SQL Database
description: Het onderwerp bevat bewijs dat batchen databasebewerkingen aanzienlijk imroves de snelheid en schaalbaarheid van uw Azure SQL Database-toepassingen. Hoewel deze batchen technieken voor elke SQL Server-database werken, is de focus van het artikel op Azure.
services: sql-database
documentationcenter: na
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 563862ca-c65a-46f6-975d-10df7ff6aa9c
ms.service: sql-database
ms.custom: develop apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 07/12/2016
ms.author: sstein
ms.openlocfilehash: 8622bddc809c9d95f7acf359ff708d5ab31cf620
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a>Het gebruik van batchverwerking voor het verbeteren van de toepassingsprestaties SQL-Database
Batchverwerking van bewerkingen met Azure SQL Database aanzienlijk verbetert de prestaties en schaalbaarheid van uw toepassingen. Om te begrijpen wat de voordelen, het eerste deel van dit artikel bevat informatie over sommige voorbeeld testresultaten die sequentieel en batch aanvragen voor een SQL-Database te vergelijken. De rest van het artikel ziet u de technieken, scenario's en om te gebruiken met succes batchverwerking in uw Azure-toepassingen.

## <a name="why-is-batching-important-for-sql-database"></a>Waarom is batchverwerking belangrijk voor SQL-Database?
Batchverwerking aanroepen naar een externe service is een bekende strategie voor prestaties en schaalbaarheid te verhogen. Er worden opgelost verwerking kosten voor alle interacties met een externe service, zoals serialisatie netwerkoverdracht en deserialisatie. Veel afzonderlijke transacties in één batch minimaliseert deze kosten.

In dit artikel willen we verschillende scenario's en de strategieën batchverwerking SQL-Database te onderzoeken. Hoewel deze strategieën ook belang voor on-premises toepassingen die gebruikmaken van SQL Server zijn, zijn er diverse redenen voor het gebruik van batchverwerking voor SQL-Database te markeren:

* Er is mogelijk groter netwerklatentie bij het openen van de SQL-Database, met name als u krijgt u toegang tot SQL-Database van buiten de hetzelfde Microsoft Azure-datacenter.
* De multitenant kenmerken van de SQL-Database betekent dat de efficiëntie van de data access laag standaarddocumentnaam voor de algehele schaalbaarheid van de database. SQL-Database moet voorkomen dat een single-tenant/gebruiker databaseresources ten koste van andere tenants belasten. In het antwoord op gebruik boven het vooraf gedefinieerde quota SQL-Database verminderen doorvoer of reageren met een beperking van uitzonderingen. Efficiëntie, zoals batches, kunnen u meer werk op de SQL-Database voordat deze limiet is bereikt. 
* Batchverwerking is ook geschikt voor architecturen die gebruikmaken van meerdere databases (sharding). De efficiëntie van uw interactie met elke eenheid database nog steeds een belangrijke factor in uw algehele schaalbaarheid. 

Een van de voordelen van het gebruik van SQL-Database is die u niet hebt voor het beheren van de servers die de database hosten. Deze beheerde infrastructuur ook betekent echter dat u moet denken dat zij anders optimalisaties van de database. U kunt niet meer zoeken ter verbetering van de database-hardware- of -infrastructuur. Microsoft Azure bepaalt deze omgevingen. Het hoofdgedeelte die u kunt beheren, is de interactie van uw toepassing met de SQL-Database. Batchverwerking is een van deze optimalisaties. 

Het eerste deel van het papier onderzoekt verschillende batchen technieken voor .NET-toepassingen die gebruikmaken van SQL-Database. De laatste twee secties hebben betrekking op batchen scenario's en richtlijnen.

## <a name="batching-strategies"></a>Batchverwerking strategieën
### <a name="note-about-timing-results-in-this-topic"></a>Houd er rekening mee over timing resultaten in dit onderwerp
> [!NOTE]
> Resultaten niet benchmarks, maar zijn bedoeld om weer te geven **relatieve prestaties**. Tijdsinstellingen zijn gebaseerd op een gemiddelde van ten minste 10 test wordt uitgevoerd. Bewerkingen worden ingevoegd in een lege tabel. Deze tests zijn gemeten pre-V12 en ze niet per se overeenkomen met de doorvoer die mogelijk optreden in een V12-database met behulp van de nieuwe [Servicelagen](sql-database-service-tiers.md). Het relatieve voordeel van de batchen techniek moet vergelijkbaar zijn.
> 
> 

### <a name="transactions"></a>Transacties
Om te beginnen met een overzicht van door het bespreken van transacties batchverwerking vreemd lijkt. Maar het gebruik van transacties aan clientzijde heeft een subtiele serverzijde batchen-effect dat zorgt voor betere prestaties. En transacties kunnen worden toegevoegd met slechts een paar regels code, zodat ze bieden een snelle manier om de prestaties van opeenvolgende bewerkingen te verbeteren.

Houd rekening met de volgende C#-code die een reeks van insert bevat en bewerkingen op een eenvoudige tabel bijwerken.

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

De volgende code van de ADO.NET voert sequentieel deze bewerkingen.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();

        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

De beste manier om deze code te optimaliseren is voor het implementeren van een vorm van client-side batchverwerking van deze aanroepen. Maar er is een eenvoudige manier om de prestaties van deze code te verhogen door gewoon de volgorde van de aanroepen in een transactie. Dit is de dezelfde code die gebruikmaakt van een transactie.

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

In beide voorbeelden worden daadwerkelijk transacties gebruikt. In het eerste voorbeeld is elke aanroep van afzonderlijke een impliciete transactie. Een expliciete transactie verpakt in het tweede voorbeeld alle van de oproepen. Per de documentatie voor de [vooraf geschreven transactielogboek](https://msdn.microsoft.com/library/ms186259.aspx), logboekrecords worden leeggemaakt op de schijf als de transactie is doorgevoerd. Door meer aanroepen in een transactie, kan het schrijven naar het transactielogboek dus uitstellen totdat de transactie doorgevoerd is. In feite inschakelt u voor schrijfbewerkingen naar de server-transactielogboek batchverwerking.

De volgende tabel bevat enkele ad-hoc-testresultaten. De tests die de opeenvolgende invoegt met en zonder transacties worden uitgevoerd. Voor meer perspectief de eerste reeks tests uitgevoerd op afstand vanaf een laptop met de database in Microsoft Azure. De tweede reeks tests uit vanaf een cloudservice en de database beide zich bevond binnen hetzelfde Microsoft Azure-datacenter (VS-West) uitgevoerd. De volgende tabel ziet de tijdsduur in milliseconden van opeenvolgende voegt met en zonder transacties.

**On-Premises naar Azure**:

| Bewerkingen | Er is geen transactie (ms) | Transactie (ms) |
| --- | --- | --- |
| 1 |130 |402 |
| 10 |1208 |1226 |
| 100 |12662 |10395 |
| 1000 |128852 |102917 |

**Azure-Azure (hetzelfde datacenter)**:

| Bewerkingen | Er is geen transactie (ms) | Transactie (ms) |
| --- | --- | --- |
| 1 |21 |26 |
| 10 |220 |56 |
| 100 |2145 |341 |
| 1000 |21479 |2756 |

> [!NOTE]
> Geen zijn resultaten benchmarks. Zie de [opmerking over timing resultaten in dit onderwerp](#note-about-timing-results-in-this-topic).
> 
> 

Op basis van de testresultaten van de vorige, één bewerking in een transactie wrapping neemt af prestaties. Maar als u het aantal bewerkingen binnen een transactie verhoogt, de prestatieverbeteringen meer wordt gemarkeerd. Prestaties is ook meer merkbare bij alle bewerkingen in de Microsoft Azure-datacenter plaatsvinden. De langere latentie van het gebruik van SQL-Database van buiten de Microsoft Azure-datacenter overshadows de prestatieverbetering van het gebruik van transacties.

Hoewel het gebruik van transacties de prestaties verbeteren kan, blijven [observeren best practices voor transacties en verbindingen](https://msdn.microsoft.com/library/ms187484.aspx). De transactie zo kort mogelijk te houden en sluiten van verbinding met de database nadat het werk is voltooid. De met de instructie in het vorige voorbeeld zorgt ervoor dat de verbinding wordt gesloten wanneer het volgende codeblok is voltooid.

Het vorige voorbeeld laat zien dat u een lokale transactie aan een ADO.NET-code met twee regels toevoegen kunt. Transacties bieden een snelle manier om de prestaties van programmacode waarmee sequentiële invoegen, bijwerken en verwijderen van bewerkingen. U kunt echter voor wijzigen van de code verder profiteren van de client-side batches, zoals table-valued parameters voor de snelste prestaties.

Zie voor meer informatie over transacties in ADO.NET [lokale transacties in ADO.NET](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).

### <a name="table-valued-parameters"></a>Table-valued parameters
Tabeltypen die door de gebruiker gedefinieerde ondersteunen table-valued parameters als parameters in de Transact-SQL-instructies, opgeslagen procedures en functies. Deze client-side batchen techniek kunt u meerdere rijen van gegevens binnen de tabelwaardeparameter verzenden. Tabelwaarden als parameters wilt gebruiken, moet u eerst een tabeltype definiëren. De volgende Transact-SQL-instructie maakt een tabeltype met de naam **MyTableType**.

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );


In de code, maakt u een **DataTable** met exact dezelfde namen en typen van het tabeltype. Dit geeft **DataTable** aanroepen in een parameter van een tekstquery of een opgeslagen procedure. Het volgende voorbeeld ziet u deze techniek:

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

In het vorige voorbeeld de **SqlCommand** object voegt rijen uit een tabelwaardeparameter  **@TestTvp** . De eerder gemaakte **DataTable** object is toegewezen aan deze parameter met de **SqlCommand.Parameters.Add** methode. De invoegt in één aanroep aanzienlijk batchverwerking, verhoogt de prestaties via sequentiële ingevoegd.

Ter verbetering van het vorige voorbeeld verder een opgeslagen procedure te gebruiken in plaats van een opdracht op basis van tekst. De volgende Transact-SQL-opdracht maakt u een opgeslagen procedure waarmee de **SimpleTestTableType** tabelwaardeparameter.

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

Wijzig de **SqlCommand** declaratie in het vorige codevoorbeeld voor de volgende object.

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

In de meeste gevallen hebben table-valued parameters gelijkwaardige of betere prestaties dan andere batchen technieken. Table-valued parameters zijn vaak beter, omdat ze meer flexibiliteit dan andere opties. Andere technieken, zoals SQL bulksgewijs kopiëren, staan bijvoorbeeld alleen het invoegen van nieuwe rijen. Maar met tabelwaarden parameters, kunt u logica in de opgeslagen procedure om te bepalen welke rijen zijn updates en die zijn ingevoegd. Het tabeltype kan ook worden aangepast voor een 'Bewerking' kolom bevatten die aangeeft of de opgegeven rij moet worden ingevoegd, bijgewerkt of verwijderd.

De volgende tabel ziet de testresultaten ad-hoc voor het gebruik van parameters tabelwaarden in milliseconden.

| Bewerkingen | On-Premises naar Azure (ms) | Azure hetzelfde datacenter (ms) |
| --- | --- | --- |
| 1 |124 |32 |
| 10 |131 |25 |
| 100 |338 |51 |
| 1000 |2615 |382 |
| 10.000 |23830 |3586 |

> [!NOTE]
> Geen zijn resultaten benchmarks. Zie de [opmerking over timing resultaten in dit onderwerp](#note-about-timing-results-in-this-topic).
> 
> 

De prestatieverbetering van batchverwerking is onmiddellijk zichtbaar. In de vorige sequentiële test 1000 bewerkingen zijn in 129 seconden buiten het datacenter en 21 seconden van binnen het datacenter. Maar met tabelwaarden parameters 1000 operations alleen 2.6 seconden buiten het datacenter en 0,4 seconden in het datacenter.

Zie voor meer informatie over table-valued parameters [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).

### <a name="sql-bulk-copy"></a>SQL bulksgewijs kopiëren
SQL bulksgewijs kopiëren is een andere manier om grote hoeveelheden gegevens in een doeldatabase invoegen. .NET-toepassingen kunnen gebruikmaken van de **SqlBulkCopy** bewerkingen klasse om uit te voeren bulksgewijs invoegen. **SqlBulkCopy** is vergelijkbaar in de functie met het opdrachtregelprogramma **Bcp.exe**, of de Transact-SQL-instructie **BULK INSERT**. De volgende voorbeeldcode laat zien hoe voor bulksgewijs kopiëren de rijen in de bron **DataTable**, MyTable tabel de doeltabel in SQL Server.

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

Er zijn enkele gevallen waarbij bulksgewijs kopiëren heeft de voorkeur boven table-valued parameters. Zie de vergelijkingstabel van Table-Valued parameters versus BULK INSERT-bewerkingen in het onderwerp [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).

De volgende ad-hoc-testresultaten geven aan de prestaties van batchverwerking met **SqlBulkCopy** in milliseconden.

| Bewerkingen | On-Premises naar Azure (ms) | Azure hetzelfde datacenter (ms) |
| --- | --- | --- |
| 1 |433 |57 |
| 10 |441 |32 |
| 100 |636 |53 |
| 1000 |2535 |341 |
| 10.000 |21605 |2737 |

> [!NOTE]
> Geen zijn resultaten benchmarks. Zie de [opmerking over timing resultaten in dit onderwerp](#note-about-timing-results-in-this-topic).
> 
> 

In kleinere batch de gebruik table-valued parameters ver-loop van de **SqlBulkCopy** klasse. Echter, **SqlBulkCopy** 12-31% sneller dan table-valued parameters uitgevoerd voor de tests van 1000 en 10.000 rijen. Table-valued parameters, zoals **SqlBulkCopy** is een goede optie voor batch ingevoegd, vooral wanneer ten opzichte van de prestaties van bewerkingen niet batch verwerkt.

Zie voor meer informatie over bulksgewijs kopiëren in ADO.NET [Bulksgewijze kopieerbewerkingen in SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).

### <a name="multiple-row-parameterized-insert-statements"></a>Meerdere rij geparametriseerde INSERT-instructies
Een alternatief voor kleine batches is invoeginstructie te maken om grote geparameteriseerde die meerdere rijen worden ingevoegd. De volgende voorbeeldcode laat zien dat deze techniek.

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


In dit voorbeeld is bedoeld om het basisconcept weergeven. Een meer realistische scenario zou de vereiste entiteiten tegelijkertijd maken van de query-tekenreeks en de opdrachtparameters doorlopen. Bent u beperkt tot een totaal van 2100 queryparameters bevatten, zodat dit beperkt het totale aantal rijen die op deze manier kunnen worden verwerkt.

De volgende ad-hoc-testresultaten geven aan de prestaties van dit type insert-instructie in milliseconden.

| Bewerkingen | Table-valued parameters (ms) | Één instructie INSERT (ms) |
| --- | --- | --- |
| 1 |32 |20 |
| 10 |30 |25 |
| 100 |33 |51 |

> [!NOTE]
> Geen zijn resultaten benchmarks. Zie de [opmerking over timing resultaten in dit onderwerp](#note-about-timing-results-in-this-topic).
> 
> 

Deze methode kan worden iets snellere voor batches die minder dan 100 rijen zijn. Hoewel de verbetering klein is, is deze techniek een andere optie die goed in uw scenario specifieke toepassing werkt mogelijk.

### <a name="dataadapter"></a>DataAdapter
De **DataAdapter** klasse kunt u wijzigen een **gegevensset** object en vervolgens de wijzigingen Als INSERT, UPDATE en DELETE-bewerkingen te verzenden. Als u de **DataAdapter** op deze manier is het belangrijk te weten dat afzonderlijke worden aangeroepen voor elke afzonderlijke bewerking. Om prestaties te verbeteren, gebruiken de **UpdateBatchSize** eigenschap aan het aantal bewerkingen die op een tijdstip in batch moet worden opgenomen. Zie voor meer informatie [uitvoeren van Batch bewerkingen met behulp van DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).

### <a name="entity-framework"></a>Entity framework
Entity Framework ondersteunt momenteel geen batchverwerking. Andere ontwikkelaars in de community ter illustratie van tijdelijke oplossingen zoals onderdrukking hebt geprobeerd de **SaveChanges** methode. Maar de oplossingen zijn meestal complex en aangepaste met de toepassing en het gegevensmodel. De Entity Framework codeplex project heeft momenteel een bespreking van de pagina voor deze functie-aanvraag. Deze discussie Zie [ontwerp notulen - 2 augustus 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).

### <a name="xml"></a>XML
Voor de volledigheid denken we dat het is belangrijk om te communiceren over XML als een batchen strategie. Het gebruik van XML heeft echter geen voordelen ten opzichte van andere methoden en enkele nadelen. De methode is vergelijkbaar met tabelwaarden parameters, maar een XML-bestand of de tekenreeks wordt doorgegeven aan een opgeslagen procedure in plaats van een gebruiker gedefinieerde tabel. De opgeslagen procedure parseert de opdrachten in de opgeslagen procedure.

Er zijn enkele nadelen deze benadering:

* Werken met XML kan lastig zijn en foutgevoelig.
* Bij het parseren van het XML-bestand op de database kan CPU-intensief zijn.
* Deze methode is langzamer dan table-valued parameters in de meeste gevallen.

Daarom is het gebruik van XML voor batch-query's niet aanbevolen.

## <a name="batching-considerations"></a>Batchverwerking overwegingen
De volgende secties bevatten meer richtlijnen voor het gebruik van batchverwerking in toepassingen met SQL-Database.

### <a name="tradeoffs"></a>Voor-en nadelen
Afhankelijk van uw architectuur batchverwerking kan betrekking hebben op een afweging tussen de prestaties en tolerantie. Neem bijvoorbeeld het scenario waar uw rol onverwacht uitvalt. Als u één rij met gegevens verliest, is de impact kleiner dan de gevolgen van het verlies van een grote batch van niet-verzonden rijen. Er is een groter risico wanneer u rijen worden gebufferd voordat ze worden verzonden naar de database in een opgegeven periode.

Vanwege deze afweging evalueren het type van bewerkingen die u batch. Batch-agressiever (grotere batches en langer tijdvensters) met gegevens die minder kritiek is.

### <a name="batch-size"></a>Batchgrootte
In onze tests is er meestal geen voordeel gebruik te grote batches in kleinere reeksen op te splitsen. Deze onderverdeling vaak resulteerde in feite in trager dan één grote batch wordt verzonden. Neem bijvoorbeeld een scenario waar u 1000 rijen invoegen. De volgende tabel ziet hoe lang het duurt om het gebruik van parameters tabelwaarden 1000 rijen wanneer onderverdeeld in kleinere batches invoegen.

| Batchgrootte | Herhalingen | Table-valued parameters (ms) |
| --- | --- | --- |
| 1000 |1 |347 |
| 500 |2 |355 |
| 100 |10 |465 |
| 50 |20 |630 |

> [!NOTE]
> Geen zijn resultaten benchmarks. Zie de [opmerking over timing resultaten in dit onderwerp](#note-about-timing-results-in-this-topic).
> 
> 

U ziet dat de beste prestaties voor 1000 rijen ze allemaal tegelijk verzenden. In andere tests (hier niet weergegeven) is er een kleine prestatieverbetering te bereiken om een batch 10000 rij in twee batches van 5000. Maar het tabelschema voor deze tests is relatief eenvoudig, zodat u tests moet uitvoeren op uw specifieke gegevens en de batch-grootten om te controleren of deze bevindingen.

Een andere factor in overweging moet nemen is dat als de totale batch te groot wordt, SQL Database mogelijk beperken en weigeren doorvoeren van de batch. Test uw specifieke scenario om te bepalen of er een ideaal batchgrootte voor de beste resultaten. De batchgrootte configureerbaar maken tijdens runtime snel aanpassingen op basis van prestaties of fouten inschakelen.

Ten slotte wordt de grootte van de batch worden verdeeld met de risico's van batchverwerking. Als er tijdelijke fouten of de rol is mislukt, kunt u de gevolgen van het opnieuw te proberen of verlies van de gegevens in de batch.

### <a name="parallel-processing"></a>Parallelle verwerking
Wat gebeurt er als u de aanpak van batch verkleinen duurde maar meerdere threads gebruikt voor het uitvoeren van het werk? Onze tests bleek opnieuw, of meerdere kleinere multithreaded batches doorgaans uitgevoerd slechter dan één groter batch. De volgende test probeert 1000 rijen invoegen in een of meer parallelle batches. Deze test toont hoe meer gelijktijdige batches daadwerkelijk prestaties verlaagd.

| Batchgrootte [iteraties] | Twee threads (ms) | Vier threads (ms) | Zes threads (ms) |
| --- | --- | --- | --- |
| 1000 [1] |277 |315 |266 |
| 500 [2] |548 |278 |256 |
| 250 [4] |405 |329 |265 |
| 100 [10] |488 |439 |391 |

> [!NOTE]
> Geen zijn resultaten benchmarks. Zie de [opmerking over timing resultaten in dit onderwerp](#note-about-timing-results-in-this-topic).
> 
> 

Er zijn enkele mogelijke redenen voor de vermindering in prestaties vanwege parallelle uitvoering:

* Er zijn meerdere gelijktijdige netwerk aanroepen in plaats van een.
* Meerdere bewerkingen op één tabel kunnen leiden tot conflicten en blokkeren.
* Er zijn overhead die is gekoppeld aan multithreading.
* De kosten van meerdere verbindingen belangrijker is dan het voordeel van parallelle verwerking.

Als u verschillende tabellen of databases als doel, is het mogelijk om te zien krijgen met deze strategie prestaties. Database-sharding of federaties zou een scenario voor deze benadering zijn. Sharding maakt gebruik van meerdere databases en andere gegevens van routes in elke database. Als er elke kleine batch met een andere database, kan vervolgens de bewerkingen uit te voeren parallel efficiënter zijn. De prestatieverbetering is echter geen aanzienlijke te gebruiken als basis voor een beslissing database sharding gebruiken in uw oplossing.

Parallelle uitvoering van kleinere batches kan in sommige ontwerpen resulteren in betere doorvoer van aanvragen in een systeem wordt belast. In dit geval, zelfs als het sneller grotere één batch te verwerken, verwerking van meerdere batches parallel mogelijk efficiënter.

Als u parallelle uitvoering gebruikt, kunt u het maximum aantal werkthreads beheren. Een kleiner getal kan leiden tot minder conflicten en een sneller worden uitgevoerd. Houd ook rekening met de extra belasting die door dit op de doeldatabase in verbindingen en transacties geplaatst.

### <a name="related-performance-factors"></a>Prestaties gerelateerd aan factoren
Typische richtlijnen op de prestaties van de database is ook van invloed op de batchverwerking. Voeg bijvoorbeeld presteert voor tabellen die een primaire sleutel voor grote of veel niet-geclusterde indexen.

Als het table-valued parameters een opgeslagen procedure gebruikt, kunt u de opdracht **SET NOCOUNT ON** aan het begin van de procedure. Deze instructie onderdrukt het retourtype van de telling van de rijen in de procedure. Echter, in onze tests, het gebruik van **SET NOCOUNT ON** geen invloed heeft ofwel nemen de prestaties af. De opgeslagen testprocedure is heel eenvoudig met één **invoegen** opdracht van de parameter tabelwaarden. Het is mogelijk dat de complexere opgeslagen procedures van deze instructie profiteren wilt. Maar niet wordt ervan uitgegaan dat toe te voegen **SET NOCOUNT ON** toe aan de opgeslagen procedure automatisch zorgt voor betere prestaties. Om het effect te begrijpen, test u de opgeslagen procedure met en zonder de **SET NOCOUNT ON** instructie.

## <a name="batching-scenarios"></a>Batchverwerking scenario 's
De volgende secties wordt beschreven hoe table-valued parameters gebruiken in drie scenario's van toepassing. Het eerste scenario ziet u hoe buffer en batches kunnen samenwerken. Het tweede scenario worden de prestaties verbeterd door het uitvoeren van de operations master-details in een aanroep van één opgeslagen procedure. Het laatste scenario laat zien hoe table-valued parameters gebruiken in een "UPSERT"-bewerking.

### <a name="buffering"></a>Buffer
Er zijn enkele scenario's die voor de hand liggende kandidaat voor batchverwerking, zijn er veel scenario's die gebruikmaken van batchverwerking door vertraagde verwerking kunnen uitvoeren. Vertraagde verwerking wordt echter ook een groter risico dat de gegevens niet verloren zijn in het geval van een onverwachte fout. Het is belangrijk om te begrijpen dit risico en ook consequenties.

Neem bijvoorbeeld een webtoepassing waarin de navigatiegeschiedenis van elke gebruiker worden bijgehouden. De toepassing kan een database aanroepen om vast te leggen van de gebruiker paginaweergave maken op elke pagina-aanvraag. Maar hogere prestaties en schaalbaarheid kunnen worden bereikt door het bufferen van de gebruikers navigatie activiteiten en vervolgens deze gegevens worden verzonden naar de database in batches. U kunt de update van de database door de verstreken tijd en/of buffergrootte activeren. Een regel kan bijvoorbeeld opgeven dat de batch moet worden verwerkt na 20 seconden of wanneer de buffer 1000 objecten bereikt.

Het volgende codevoorbeeld wordt [reactieve extensies - Rx](https://msdn.microsoft.com/data/gg577609) buffer opgeslagen gebeurtenissen worden gegenereerd door een controleprogramma klasse verwerken. Wanneer u de buffer of een time-out is bereikt, de batch van gebruikersgegevens naar de database met een tabelwaardeparameter is verzonden.

De volgende NavHistoryData klasse modellen de gebruikersgegevens voor navigatie. Het bevat algemene informatie zoals de gebruikers-id, de URL die is geopend en de tijd toegang.

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

De klasse NavHistoryDataMonitor is verantwoordelijk voor het bufferen van de gebruikersgegevens voor navigatie naar de database. Het bevat een methode, RecordUserNavigationEntry die antwoordt door een **OnAdded** gebeurtenis. De volgende code toont de constructor-logica die Rx gebruikt voor het maken van een waarneembare verzameling op basis van de gebeurtenis. Deze zich abonneert op deze waarneembare verzameling met de methode Buffer. De overload geeft aan dat de buffer elke 20 seconden of 1000 vermeldingen moet worden verzonden.

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

De handler alle gebufferde items converteert naar een type tabelwaarden en vervolgens dit type doorgegeven aan een opgeslagen procedure die de batch verwerkt. De volgende code toont de volledige definitie voor zowel de NavHistoryDataEventArgs en de klassen NavHistoryDataMonitor.

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

De toepassing maakt voor het gebruik van deze buffer klasse, een statisch NavHistoryDataMonitor-object. Telkens wanneer die een gebruiker toegang krijgt een pagina tot wordt de toepassing de NavHistoryDataMonitor.RecordUserNavigationEntry-methode aanroept. De buffer logica verloopt zorgt voor het verzenden van deze items naar de database in batches.

### <a name="master-detail"></a>Details van basispagina
Table-valued parameters zijn handig voor eenvoudige INSERT-scenario's. Echter kan zijn meer moeilijker om batch voegt die betrekking hebben op meer dan één tabel. De ' master/detail'-scenario is een goed voorbeeld. De hoofdtabel identificeert de primaire entiteit. Een of meer detailtabellen opslaan meer gegevens over de entiteit. Refererende-sleutelrelaties afdwingen in dit scenario wordt de relatie van gegevens naar een unieke master entiteit. U kunt een vereenvoudigde versie van een tabel PurchaseOrder en de bijbehorende OrderDetail-tabel. De volgende Transact-SQL maakt de tabel PurchaseOrder met vier kolommen: OrderID, OrderDate CustomerID en Status.

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

Elke order bevat een of meer kopen van een product. Deze informatie wordt opgenomen in de tabel PurchaseOrderDetail. De volgende Transact-SQL maakt de tabel PurchaseOrderDetail met vijf kolommen: OrderID, OrderDetailID ProductID, UnitPrice en OrderQty.

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

De kolom OrderID in de tabel PurchaseOrderDetail moet verwijzen naar een order uit de tabel PurchaseOrder. Deze beperking wordt afgedwongen door de volgende definitie van een refererende sleutel.

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

Om het table-valued parameters gebruikt, moet u een gebruiker gedefinieerde tabeltype voor elke doeltabel hebben.

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

Vervolgens definieert u een opgeslagen procedure die tabellen van de volgende typen accepteert. Deze procedure kunt een toepassing in een batch moet lokaal een reeks orders en orderdetails in één aanroep. De volgende Transact-SQL biedt de volledige opgeslagen procedure-declaratie voor dit voorbeeld van de volgorde aankoop.

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

In dit voorbeeld wordt het lokaal gedefinieerde @IdentityLink tabel bevat de werkelijke waarden van de order-id van de ingevoegde rijen. Deze volgorde-id's zijn anders dan de tijdelijke OrderID waarden in de @orders en @details table-valued parameters. Daarom moet de @IdentityLink tabel maakt vervolgens verbinding met de order-id-waarden uit de @orders -parameter voor de echte OrderID waarden voor de nieuwe rijen in de tabel PurchaseOrder. Na deze stap wordt de @IdentityLink tabel invoegen van de details van de order met de werkelijke OrderID die voldoet aan de foreign key-beperking kunt vergemakkelijken.

Deze opgeslagen procedure kan worden gebruikt vanuit code of van andere Transact-SQL-aanroepen. Zie de sectie table-valued parameters van dit artikel voor een codevoorbeeld. De volgende Transact-SQL laat zien hoe de sp_InsertOrdersBatch niet aanroepen.

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

Deze oplossing kunt elke batch een set van OrderID waarden die bij 1 beginnen te gebruiken. Deze tijdelijke OrderID waarden beschrijven de relaties in de batch, maar de werkelijke waarden van OrderID zijn vastgesteld op het moment van de bewerking insert. U kunt meerdere keren dezelfde instructies in het vorige voorbeeld worden uitgevoerd en unieke orders genereren in de database. Overweeg meer code of database logica waarmee wordt voorkomen dubbele orders dat bij het gebruik van deze techniek batchverwerking om deze reden.

In dit voorbeeld laat zien dat zelfs complexere databasebewerkingen, zoals operations master-details, kunnen in batch worden opgenomen met tabelwaarden parameters.

### <a name="upsert"></a>UPSERT
Een ander batchen scenario omvat het gelijktijdig bijwerken van bestaande rijen en nieuwe rijen invoegen. Deze bewerking wordt soms aangeduid als een "UPSERT" (update + insert)-bewerking. In plaats van afzonderlijke oproepen invoegen en bijwerken, de instructie MERGE meest geschikt is voor deze taak. De instructie MERGE kunt uitvoeren van beide invoegen en updatebewerkingen in één aanroep.

Table-valued parameters kunnen worden gebruikt met de instructie MERGE updates en toevoegingen uit te voeren. Neem bijvoorbeeld een vereenvoudigde werknemerstabel met de volgende kolommen: werknemer-id, FirstName, LastName, SocialSecurityNumber:

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))

In dit voorbeeld kunt u het feit dat de SocialSecurityNumber uniek is voor het samenvoegen van meerdere werknemers is. Maak eerst de gebruiker gedefinieerde tabeltype:

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

Vervolgens maakt u een opgeslagen procedure of code schrijven waarmee de instructie MERGE gebruikt voor het uitvoeren van de update en invoegen. Het volgende voorbeeld wordt de instructie MERGE op een tabelwaardeparameter @employees, van het type EmployeeTableType. De inhoud van de @employees tabel worden hier niet weergegeven.

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

Zie voor meer informatie de documentatie en voorbeelden voor de instructie MERGE. Hoewel hetzelfde werk kan worden uitgevoerd in een opgeslagen meerdere stappen procedureaanroep met afzonderlijke INSERT en UPDATE-bewerkingen, de instructie MERGE is efficiënter. Databasecode kunt Transact-SQL-aanroepen die gebruikmaken van de instructie MERGE rechtstreeks zonder twee databaseaanroepen voor INSERT en UPDATE ook opstellen.

## <a name="recommendation-summary"></a>Aanbeveling samenvatting
De volgende lijst bevat een samenvatting van de batchen aanbevelingen in dit onderwerp wordt beschreven:

* Buffer en batches gebruiken om de prestaties en schaalbaarheid van SQL Database-toepassingen.
* Inzicht in de wisselwerking tussen batchverwerking/buffer en tolerantie. Tijdens een mislukte functie kan het risico dat een niet-verwerkte batch van essentiële gegevens verloren gaan opwegen tegen de prestatievoordelen van batchverwerking.
* Poging tot het behouden van alle aanroepen naar de database in een enkel datacenter naar de latentie te verminderen.
* Als u een bepaalde batchen techniek kiest, wordt table-valued parameters bieden de beste prestaties en flexibiliteit.
* Volg deze algemene richtlijnen voor de snelste insert-prestaties, maar testen van uw scenario:
  * Voor < 100 rijen geparametriseerde gebruik één opdracht invoegen.
  * Voor < 1000 rijen, table-valued parameters te gebruiken.
  * Voor > = 1000 rijen, SqlBulkCopy gebruiken.
* Voor bijwerken en verwijderen van bewerkingen, table-valued parameters gebruiken met opgeslagen procedure logica die de juiste werking op elke rij in de tabel-parameter bepaalt.
* Richtlijnen voor batch grootte:
  * Gebruik de grootste batch grootten die geschikt zijn voor uw toepassing en de zakelijke vereisten.
  * De prestatieverbetering van grote batches met de risico's van tijdelijke of onherstelbare fouten worden verdeeld. Wat is het gevolg van nieuwe pogingen of verlies van de gegevens in de batch? 
  * Test de grootste batchgrootte om te controleren of SQL-Database komt niet afwijzen.
  * Configuratie-instellingen maken dat beheer batches, zoals de batchgrootte van de of de buffer tijdvenster. Deze instellingen bieden flexibiliteit. U kunt het batchen gedrag in productie wijzigen zonder dat de cloudservice.
* Vermijd parallelle uitvoering van batches die worden uitgevoerd op één tabel in een database. Als u één batch verdelen over meerdere werkthreads kiest, voert u de tests uit om te bepalen hoeveel threads ideaal. Na een niet-opgegeven drempelwaarde meer threads worden de prestaties verminderen, in plaats van verhogen.
* Overweeg het bufferen van grootte en de tijd bij wijze van het implementeren van batchverwerking voor scenario's met meer.

## <a name="next-steps"></a>Volgende stappen
In dit artikel gericht op hoe ontwerpen van databases en technieken die betrekking hebben op batchverwerking coderen de prestaties en schaalbaarheid verbeteren kunnen. Maar dit is slechts één factor in uw algehele beveiligingsstrategie. Zie voor meer manieren om prestaties en schaalbaarheid te verbeteren, [richtlijnen voor Azure SQL Database-prestaties voor individuele databases](sql-database-performance-guidance.md) en [prijs- en Prestatieoverwegingen voor een elastische pool](sql-database-elastic-pool-guidance.md).

