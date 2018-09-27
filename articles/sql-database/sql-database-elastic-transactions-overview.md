---
title: Over clouddatabases gedistribueerde transacties
description: Overzicht van transacties met Azure SQL Database Elastic Database
services: sql-database
ms.service: sql-database
ms.subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 3147061f527621ba98dee84f4d347a6e883d61c0
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166466"
---
# <a name="distributed-transactions-across-cloud-databases"></a>Over clouddatabases gedistribueerde transacties
Elastische transacties voor Azure SQL Database (SQL database) kunnen u uitvoeren van transacties met betrekking meerdere databases in SQL-database tot. Elastische transacties voor SQL-database zijn beschikbaar voor .NET-toepassingen met behulp van ADO .NET en integreren met de vertrouwde programmering ervaring met de [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) klassen. Als u de bibliotheek, Zie [.NET Framework 4.6.1 (webinstallatieprogramma)](https://www.microsoft.com/download/details.aspx?id=49981).

On-premises, in dit scenario normaal gesproken nodig waarop Microsoft Distributed Transaction Coordinator (MSDTC) wordt uitgevoerd. Aangezien MSDTC niet beschikbaar voor Platform-as-a-Service-toepassing in Azure is, is de mogelijkheid voor de coördinatie van gedistribueerde transacties nu rechtstreeks geïntegreerd in SQL DB. Toepassingen kunnen verbinding maken met elke SQL-Database om te starten van gedistribueerde transacties en een van de databases wordt transparant coördinatie van de gedistribueerde transactie, zoals wordt weergegeven in de volgende afbeelding. 

  ![Gedistribueerde transacties met Azure SQL Database met behulp van transacties met elastic databases ][1]

## <a name="common-scenarios"></a>Algemene scenario's
Transacties voor SQL-database elastische databases kunnen toepassingen atomische wijzigingen aanbrengen in de gegevens die zijn opgeslagen in diverse verschillende SQL-Databases. De Preview-versie is gericht op de ervaringen van de client-side-ontwikkeling in C# en .NET. Een met T-SQL server-side '-ervaring is gepland voor een later tijdstip.  
Elastische transacties is gericht op de volgende scenario's:

* Meerdere databasetoepassingen in Azure: met dit scenario de gegevens zijn verticaal gepartitioneerd voor meerdere databases in SQL DB dat verschillende soorten gegevens bevinden zich op verschillende databases. Sommige bewerkingen vereist wijzigingen in gegevens die wordt bewaard in twee of meer databases. De toepassing maakt gebruik van elastische databasetransacties coördineren van de wijzigingen voor databases en ervoor zorgen dat atomisch.
* Shard-database-toepassingen in Azure: met dit scenario, de gegevenslaag maakt gebruik van de [Elastic Database-clientbibliotheek](sql-database-elastic-database-client-library.md) of self-sharding voor het horizontaal partitioneren van de gegevens voor verschillende databases in SQL-database. Een prominente use-case is de noodzaak voor het uitvoeren van atomische wijzigingen voor een shard-toepassing met meerdere tenants wanneer wijzigingen tenants omvatten. Denk bijvoorbeeld aan een overdracht van één tenant naar een andere, zowel die zich bevinden op verschillende databases. Een tweede geval is fijnmazige sharding voor behoeften aan capaciteit voor een grote tenant die op zijn beurt doorgaans impliceert dat sommige atomische bewerkingen moet zich uitstrekt over meerdere databases die worden gebruikt voor de dezelfde tenant. Een derde case is atomic updates om te verwijzen naar gegevens die worden gerepliceerd tussen databases. Atomische, transactionele, bewerkingen langs deze regels kunnen nu worden gecoördineerd voor verschillende databases met behulp van de Preview-versie.
  Elastische transacties gebruiken doorvoeren in twee fasen om ervoor te zorgen transactie atomisch voor databases. Het is geschikt voor transacties die betrekking hebben op minder dan 100 databases op een tijdstip binnen een transactie. Deze limieten worden niet afgedwongen, maar een goed is, prestaties en succespercentages voor elastische databasetransacties afnemen wanneer deze limieten overschrijden.

## <a name="installation-and-migration"></a>Installatie en migratie
De mogelijkheden voor transacties met elastische databases in SQL-database worden geleverd via updates van de .NET-bibliotheken System.Data.dll en System.Transactions.dll. De dll-bestanden te garanderen dat doorvoertransactie wordt gebruikt wanneer het noodzakelijk is om ervoor te zorgen atomisch. Installeren als u wilt beginnen met het ontwikkelen van toepassingen die gebruikmaken van transacties met elastische databases, [.NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) of een latere versie. Bij het uitvoeren op een eerdere versie van .NET framework, transacties om te promoveren tot een gedistribueerde transactie mislukt en een uitzondering is, wordt weergegeven.

Na de installatie, kunt u de gedistribueerde transactie API's in System.Transactions via verbindingen met SQL-database. Als u bestaande MSDTC-toepassingen met behulp van deze API's hebt, moet u gewoon uw bestaande toepassingen voor .NET 4.6 opnieuw na de installatie van de 4.6.1 Framework. Als uw projecten gericht op .NET 4.6, gebruiken ze automatisch de bijgewerkte dll-bestanden van de nieuwe Framework-versie en gedistribueerde transactie die API-in combinatie met verbindingen met SQL DB aanroepen wordt nu slagen.

Houd er rekening mee dat transacties voor elastische database vereisen geen MSDTC installeren. Elastische transacties worden in plaats daarvan rechtstreeks beheerd door en in SQL-database. Dit vereenvoudigt aanzienlijk cloudscenario's omdat een implementatie van MSDTC niet nodig is om het gebruik van gedistribueerde transacties met SQL-database. Sectie 4 wordt in meer detail uitgelegd hoe u transacties met elastic databases en vereist .NET framework, samen met uw cloudtoepassingen naar Azure te implementeren.

## <a name="development-experience"></a>Ontwikkelervaring
### <a name="multi-database-applications"></a>Meerdere database-toepassingen
De volgende voorbeeldcode maakt gebruik van de vertrouwde programmeren met .NET System.Transactions. De klasse TransactionScope stelt een ambient transactie in .NET. (Een "ambient transactie' is een die zich in de huidige thread.) Alle verbindingen geopend in de TransactionScope deelnemen aan de transactie. Als verschillende databases deelnemen, wordt de transactie is automatisch uitgebreid naar een gedistribueerde transactie. Het resultaat van de transactie wordt bepaald door het instellen van het bereik om aan te geven van een wijziging is voltooid.

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

### <a name="sharded-database-applications"></a>Shard-database-toepassingen
Elastische transacties voor SQL-database worden ook ondersteund coördineren van gedistribueerde transacties waar u de methode OpenConnectionForKey van de clientbibliotheek van elastische database gebruiken om te openen verbindingen voor een uitgeschaalde gegevens laag. Na te gaan waar u nodig hebt om te waarborgen van transactionele consistentie voor wijzigingen in verschillende waarden voor verschillende sharding-sleutel. Verbindingen met de shards die als host fungeert voor de verschillende sharding-sleutelwaarden zijn brokered OpenConnectionForKey gebruiken. In het algemeen zijn mag de verbindingen naar verschillende shards dat ervoor te zorgen dat transactionele garanties een gedistribueerde transactie vereist. Het volgende codevoorbeeld ziet u deze aanpak. Hierbij wordt ervan uitgegaan dat een variabele met de naam shardmap wordt gebruikt om weer te geven van een shard-toewijzing van de clientbibliotheek van elastische database:

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


## <a name="net-installation-for-azure-cloud-services"></a>Installatie van .NET voor Azure Cloud Services
Azure biedt verschillende aanbiedingen bij host .NET-toepassingen. Een vergelijking van de verschillende aanbiedingen is beschikbaar in [vergelijking van Azure App Service, Cloud Services en virtuele Machines](../app-service/choose-web-site-cloud-service-vm.md). Als het gastbesturingssysteem van de aanbieding kleiner dan .NET 4.6.1 vereist voor de elastische transacties is, moet u het gastbesturingssysteem upgraden naar 4.6.1 worden geïnstalleerd. 

Voor Azure App Services, worden upgrades voor het gastbesturingssysteem momenteel niet ondersteund. Voor Azure Virtual Machines, meld u aan bij de VM en voer het installatieprogramma voor de nieuwste .NET framework. Voor Azure Cloud Services moet u bijvoorbeeld de installatie van een nieuwere versie van .NET in de opstarttaken van uw implementatie. De concepten en stappen worden beschreven in [.NET installeren op een Cloudservicerol](../cloud-services/cloud-services-dotnet-install-dotnet.md).  

Houd er rekening mee dat het installatieprogramma voor .NET 4.6.1 meer tijdelijke opslag mogelijk tijdens de bootstrapping op Azure cloudservices dan het installatieprogramma voor .NET 4.6. Om te controleren of een geslaagde installatie, moet u verhogen van de tijdelijke opslag voor uw Azure-cloud-service in het bestand ServiceDefinition.csdef in de sectie LocalResources en de omgevingsinstellingen van uw taak starten, zoals wordt weergegeven in het volgende voorbeeld:

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
Elastische transacties worden ondersteund over verschillende logische servers in Azure SQL Database. Wanneer de grenzen van de logische server meerdere, moeten de deelnemende servers eerst worden ingevoerd in een relatie wederzijdse communicatie. Wanneer is de relatie communicatie tot stand is gebracht, deel elke database in een van de twee servers uitmaken van elastische transacties met databases van de andere server. Een communicatie-relatie moet worden voldaan voor een paar van logische servers met transacties die meer dan twee logische servers omvatten.

Gebruik de volgende PowerShell-cmdlets voor het beheren van relaties voor elastische databasetransacties communicatie tussen servers:

* **Nieuwe AzureRmSqlServerCommunicationLink**: Gebruik deze cmdlet voor het maken van een nieuwe relatie van de communicatie tussen twee logische servers in Azure SQL-database. De relatie is symmetrische dat beide servers transacties met de andere server tot stand kunnen brengen.
* **Get-AzureRmSqlServerCommunicationLink**: Gebruik deze cmdlet voor het ophalen van de bestaande communicatie relaties en hun eigenschappen.
* **Remove-AzureRmSqlServerCommunicationLink**: Gebruik deze cmdlet voor het verwijderen van een bestaande relatie voor de communicatie. 

## <a name="monitoring-transaction-status"></a>Controlestatus van transactie
Gebruik dynamische beheerweergaven (DMV's) in SQL-database naar de monitor status en voortgang van uw transacties lopende elastische databases. Alle DMV's met betrekking tot transacties zijn relevant voor gedistribueerde transacties in SQL-database. U vindt hier de bijbehorende lijst met DMV's: [transactie gerelateerde Dynamic Management Views en -functies (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).

Deze DMV's zijn met name handig:

* **sys.DM\_tran\_active\_transacties**: een lijst met actieve transacties en hun status. De kolom UOW (werkeenheid) kunt identificeren de verschillende onderliggende transacties die deel uitmaken van dezelfde gedistribueerde transactie. Alle transacties binnen de dezelfde gedistribueerde transactie voeren dezelfde UOW-waarde. Zie de [DMV documentatie](https://msdn.microsoft.com/library/ms174302.aspx) voor meer informatie.
* **sys.DM\_tran\_database\_transacties**: biedt aanvullende informatie over transacties, zoals de plaatsing van de transactie in het logboek. Zie de [DMV documentatie](https://msdn.microsoft.com/library/ms186957.aspx) voor meer informatie.
* **sys.DM\_tran\_vergrendelingen**: bevat informatie over de vergrendelingen die momenteel worden vastgehouden door actieve transacties. Zie de [DMV documentatie](https://msdn.microsoft.com/library/ms190345.aspx) voor meer informatie.

## <a name="limitations"></a>Beperkingen
De volgende beperkingen zijn momenteel van toepassing op transacties met elastische databases in SQL-database:

* Alleen de transacties in meerdere databases in SQL-database worden ondersteund. Andere [X / Open XA](https://en.wikipedia.org/wiki/X/Open_XA) resourceproviders en -databases buiten SQL-database kunnen niet deelnemen aan transacties met elastische databases. Dit betekent dat transacties voor elastische database niet uitbreiden voor on-premises SQL Server en Azure SQL Database. MSDTC gebruiken voor gedistribueerde transacties on-premises, blijven. 
* Alleen client gecoördineerd transacties via een .NET-toepassing worden ondersteund. Server-side '-ondersteuning voor T-SQL, zoals starten DISTRIBUTED TRANSACTION is gepland, maar nog niet beschikbaar. 
* Transacties voor WCF-services worden niet ondersteund. U hebt bijvoorbeeld een WCF-service-methode die door een transactie worden uitgevoerd. De aanroep in een transactiebereik insluitende mislukt als een [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception).

## <a name="next-steps"></a>Volgende stappen
U vragen hebt, neemt u contact met ons op de [forum van SQL-Database](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) en voor functieaanvragen, deze toe te voegen aan de [forum met feedback van SQL-Database](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png



