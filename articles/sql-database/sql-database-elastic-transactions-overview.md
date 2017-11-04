---
title: Over clouddatabases gedistribueerde transacties
description: Overzicht van transacties met Azure SQL Database elastische Database
services: sql-database
documentationcenter: 
author: torsteng
manager: jhubbard
editor: torsteng
ms.assetid: e14df7a3-7788-4cfb-bcd1-7ad6433ef1f9
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: 012fc38075285b898599517f3e6ed5a3c9eb854d
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="distributed-transactions-across-cloud-databases"></a>Over clouddatabases gedistribueerde transacties
Elastische databasetransacties voor Azure SQL Database (SQL DB) kunnen u transacties op meerdere verschillende databases in SQL-database worden uitgevoerd. Elastische databasetransacties voor SQL-database zijn beschikbaar voor .NET-toepassingen die gebruikmaken van ADO .NET en integreren met de bekende programmering ervaring met de [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) klassen. Als u de bibliotheek, Zie [.NET Framework 4.6.1 (webinstallatie)](https://www.microsoft.com/download/details.aspx?id=49981).

On-premises vereist dergelijk scenario doorgaans Microsoft Distributed Transaction Coordinator (MSDTC) wordt uitgevoerd. Aangezien MSDTC niet beschikbaar voor Platform-as-a-Service-toepassing in Azure is, is de mogelijkheid voor het coördineren van gedistribueerde transacties nu rechtstreeks geïntegreerd in SQL-database. Toepassingen kunnen verbinding maken met elke SQL-Database gedistribueerde transacties starten en een van de databases wordt transparant coördinatie van de gedistribueerde transactie, zoals wordt weergegeven in de volgende afbeelding. 

  ![Gedistribueerde transacties met Azure SQL Database met behulp van transacties voor elastische database ][1]

## <a name="common-scenarios"></a>Algemene scenario's
Elastische databasetransacties voor SQL-database kunnen toepassingen atomic wijzigingen aanbrengen in de gegevens die zijn opgeslagen in een aantal verschillende SQL-Databases. De evaluatieversie is gericht op de ervaringen van de client-side '-ontwikkeling in C# en .NET. Een server-side '-ervaring met T-SQL is gepland voor een later tijdstip.  
De volgende scenario's zijn bedoeld voor elastische databasetransacties:

* Toepassingen in Azure meerdere databases: met dit scenario gegevens is verticaal gepartitioneerd in verschillende databases in SQL-database zodanig dat verschillende soorten gegevens zich op andere databases bevinden. Bepaalde bewerkingen vereist wijzigingen in gegevens die in twee of meer databases wordt bewaard. De toepassing gebruikt een elastische databasetransacties coördineren van de wijzigingen in de databases en ervoor zorgen dat atomisch.
* Databasetoepassingen van de shard-in Azure: met dit scenario wordt de gegevenslaag maakt gebruik van de [clientbibliotheek voor elastische Database](sql-database-elastic-database-client-library.md) of self-sharding voor het partitioneren van de gegevens horizontaal tussen meerdere databases in SQL-database. Een opvallende gebruiksvoorbeeld is de noodzaak atomic wijzigingen voor een shard multitenant-toepassing uitvoeren wanneer wijzigingen span tenants. Beschouw bijvoorbeeld een overdracht van een tenant naar de andere beide die zich op verschillende databases. Een tweede geval is fijnmazig sharding voor capaciteitsbehoeften voor een grote tenant die op zijn beurt doorgaans impliceert dat een aantal atomische bewerkingen moet uitrekken verschillende databases gebruikt voor dezelfde tenant. Een derde aanvraag is atomic-updates om te verwijzen naar gegevens die worden gerepliceerd tussen databases. Atomic, transactionele, bewerkingen langs deze regels kunnen nu worden gecoördineerd tussen verschillende databases met behulp van de preview.
  Doorvoertransactie transacties elastische database gebruiken om ervoor te zorgen atomisch transactie tussen databases. Het is geschikt voor transacties met minder dan 100 databases op een tijdstip binnen een transactie. Deze limieten worden niet afgedwongen, maar een verwachte prestaties en het succespercentage voor elastische databasetransacties lijden wanneer deze limiet wordt overschreden.

## <a name="installation-and-migration"></a>Installatie en migratie
De mogelijkheden voor elastische databasetransacties in SQL-database zijn opgenomen in updates voor de .NET-bibliotheken System.Data.dll en System.Transactions.dll. De DLL's ervoor te zorgen dat doorvoertransactie wordt gebruikt, indien nodig om ervoor te zorgen atomisch. Als u wilt beginnen met het ontwikkelen van toepassingen met behulp van de elastische databasetransacties, installeren [.NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) of een latere versie. Wanneer op een eerdere versie van .NET framework wordt uitgevoerd, geen transacties promoveren tot een gedistribueerde transactie en wordt een uitzondering wordt gegenereerd.

Na de installatie, kunt u de gedistribueerde transactie API's in System.Transactions met verbindingen met SQL-database. Als er bestaande MSDTC-toepassingen met behulp van deze API's, gewoon uw bestaande .NET 4.6-toepassingen opnieuw na de installatie van de 4.6.1 Framework. Als uw projecten .NET 4.6 richten, gebruiken ze automatisch de bijgewerkte dll-bestanden van de nieuwe versie van Framework en gedistribueerde transactie die API-in combinatie met verbindingen met SQL-database aanroepen nu slaagt.

Houd er rekening mee dat transacties voor elastische database vereisen geen MSDTC installeren. In plaats daarvan worden elastische databasetransacties rechtstreeks beheerd door en binnen SQL-database. Dit vereenvoudigt scenario's voor de aanzienlijk omdat een implementatie van MSDTC niet nodig is om het gebruik van gedistribueerde transacties met SQL-database. Sectie 4 wordt in meer detail uitgelegd hoe elastische databasetransacties en vereist .NET framework samen met uw cloud-toepassingen naar Azure te implementeren.

## <a name="development-experience"></a>Ontwikkeling biedt
### <a name="multi-database-applications"></a>Meerdere databasetoepassingen
De volgende voorbeeldcode maakt gebruik van de vertrouwde programmeerervaring met .NET System.Transactions. De TransactionScope-klasse stelt een ambient transactie in .NET. (Een 'ambient transactie' is een die zich in de huidige thread.) Alle verbindingen in de TransactionScope geopend deelnemen aan de transactie. Als verschillende databases deel uitmaakt, wordt de transactie is automatisch uitgebreid naar een gedistribueerde transactie. Het resultaat van de transactie wordt bepaald door het instellen van het bereik voltooien om af te wijzen op een doorvoer.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Shard databasetoepassingen
Elastische databasetransacties voor SQL-database bieden ook ondersteuning voor coördineren van gedistribueerde transacties waar u de methode OpenConnectionForKey van de clientbibliotheek voor elastische database gebruiken om te openen verbindingen voor een uitgebreid uit gegevens die laag. Houd rekening met gevallen waarin u wilt garanderen transactionele consistentie voor wijzigingen in verschillende andere sharding-sleutelwaarden. Verbindingen met de shards die als host fungeert voor de verschillende sharding sleutelwaarden worden geleverd met OpenConnectionForKey. In het algemeen zijn mag de verbindingen naar verschillende shards zodat gezorgd transactionele garanties een gedistribueerde transactie vereist. Het volgende codevoorbeeld ziet u deze methode. Er wordt ervan uitgegaan dat een variabele met de naam shardmap vertegenwoordigt een shard-toewijzing van de clientbibliotheek voor elastische database wordt gebruikt:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }


## <a name="net-installation-for-azure-cloud-services"></a>.NET-installatie voor Azure Cloud Services
Azure biedt verschillende aanbiedingen om als host .NET-toepassingen. Er is een vergelijking van de andere aanbiedingen beschikbaar in [vergelijking van Azure App Service, Cloud Services en virtuele Machines](../app-service/choose-web-site-cloud-service-vm.md). Als het gastbesturingssysteem van de aanbieding kleiner dan .NET 4.6.1 vereist voor de elastische transacties is, moet u de upgrade van het gastbesturingssysteem naar 4.6.1. 

Voor Azure App Services worden upgrades met het gastbesturingssysteem momenteel niet ondersteund. Voor Azure Virtual Machines gewoon aanmelden op de virtuele machine en voer het installatieprogramma voor de meest recente .NET framework. Voor Azure Cloud Services moet u bijvoorbeeld de installatie van een nieuwere versie van .NET bij het starten van de taken van uw implementatie. De concepten en de stappen worden beschreven in [.NET installeren op een Cloud Service-functie](../cloud-services/cloud-services-dotnet-install-dotnet.md).  

Houd er rekening mee dat het installatieprogramma voor .NET 4.6.1 meer tijdelijke opslag kan nodig zijn tijdens de bootstrapping op Azure-cloudservices dan het installatieprogramma voor .NET 4.6. Om te zorgen voor een geslaagde installatie moet u verhogen tijdelijke opslag voor uw Azure-cloud-service in uw bestand ServiceDefinition.csdef in de sectie LocalResources en de omgevingsinstellingen van uw taak starten, zoals wordt weergegeven in het volgende voorbeeld:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>

## <a name="transactions-across-multiple-servers"></a>Transacties op meerdere servers
Elastische databasetransacties worden ondersteund tussen de verschillende logische Azure SQL Database-servers. Meerdere logische server grenzen, moeten de deelnemende servers eerst worden ingevoerd in een relatie wederzijdse communicatie. Zodra de relatie communicatie tot stand is gebracht, deel elke database in een van de twee servers uitmaken van elastische transacties van databases uit de andere server. Een communicatie-relatie moet aanwezig zijn voor elk paar logische servers met meer dan twee logische servers spanning transacties.

Gebruik de volgende PowerShell-cmdlets voor het beheren van relaties voor elastische databasetransacties communicatie tussen servers:

* **Nieuwe AzureRmSqlServerCommunicationLink**: Gebruik deze cmdlet voor het maken van een nieuwe relatie van de communicatie tussen twee logische Azure SQL DB-servers. De relatie is symmetrische dat beide servers transacties met de andere server tot stand kunnen brengen.
* **Get-AzureRmSqlServerCommunicationLink**: Gebruik deze cmdlet voor het ophalen van de bestaande communicatie relaties en hun eigenschappen.
* **Verwijder AzureRmSqlServerCommunicationLink**: Gebruik deze cmdlet om te verwijderen van een bestaande relatie voor de communicatie. 

## <a name="monitoring-transaction-status"></a>Controle van transactiestatus
Gebruik dynamische beheerweergaven (DMV's) in SQL-database naar de monitor status en voortgang van uw transacties lopende elastische database. Alle DMV's die zijn gerelateerd aan transacties zijn relevant voor gedistribueerde transacties die in SQL-database. U vindt hier de bijbehorende lijst van de DMV's: [transactie gerelateerde dynamische Management weergaven en -functies (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).

Deze DMV's zijn vooral handig:

* **sys.DM\_tran\_active\_transacties**: een lijst met huidige actieve transacties en hun status. De kolom UOW (werkeenheid) kunt identificeren van de verschillende onderliggende transacties die deel uitmaken van dezelfde gedistribueerde transactie. Alle transacties in de dezelfde gedistribueerde transactie voeren dezelfde UOW-waarde. Zie de [DMV documentatie](https://msdn.microsoft.com/library/ms174302.aspx) voor meer informatie.
* **sys.DM\_tran\_database\_transacties**: biedt aanvullende informatie over transacties, zoals de plaatsing van de transactie in het logboek. Zie de [DMV documentatie](https://msdn.microsoft.com/library/ms186957.aspx) voor meer informatie.
* **sys.DM\_tran\_vergrendelingen**: bevat informatie over de vergrendelingen die momenteel zijn ingesteld door actieve transacties. Zie de [DMV documentatie](https://msdn.microsoft.com/library/ms190345.aspx) voor meer informatie.

## <a name="limitations"></a>Beperkingen
De volgende beperkingen zijn momenteel van toepassing op de transacties elastische database in SQL-database:

* Alleen de transacties voor databases in SQL-database worden ondersteund. Andere [X / Open XA](https://en.wikipedia.org/wiki/X/Open_XA) resourceproviders en databases buiten SQL-database kunnen niet deelnemen aan elastische databasetransacties. Dit betekent dat transacties voor elastische database kunnen niet uitrekken on-premises SQL Server en Azure SQL-Databases. Blijven gebruiken MSDTC voor gedistribueerde transacties on-premises. 
* Alleen client gecoördineerd transacties van een .NET-toepassing worden ondersteund. Server-side-ondersteuning voor T-SQL zoals BEGIN DISTRIBUTED TRANSACTION is gepland, maar nog niet beschikbaar. 
* Transacties die in de hele WCF-services worden niet ondersteund. U hebt bijvoorbeeld een WCF-service-methode die een transactie wordt uitgevoerd. De aanroep vanuit een transactiebereik insluitende mislukt als een [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception).

## <a name="next-steps"></a>Volgende stappen
Voor vragen kunt contact met ons op de [SQL Database-forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) en voor functieaanvragen, deze toe te voegen aan de [forum met feedback van SQL-Database](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png



