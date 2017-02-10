---
title: Migratie van de SQL Server-database naar Azure SQL Database | Microsoft Docs
description: "Lees meer over on-premises SQL Server-databasemigratie naar Azure SQL Database in de cloud. Hulpprogramma&quot;s voor databasemigratie gebruiken voor het testen van de compatibiliteit vóór de databasemigratie."
keywords: databasemigratie, sql server-databasemigratie, hulpprogramma&quot;s voor databasemigratie, database migreren, sql-database migreren
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 9cf09000-87fc-4589-8543-a89175151bc2
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 11/08/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 86bc7d89bb5725add8ba05b6f0978467147fd3ca
ms.openlocfilehash: 61fb027dfdd5830d87fe4fcfff57f685db71475e


---
# <a name="sql-server-database-migration-to-sql-database-in-the-cloud"></a>SQL Server-databasemigratie naar SQL Database in de cloud
In dit artikel leest u hoe u een on-premises SQL Server 2005- of latere database kunt migreren naar Azure SQL Database. In dit databasemigratieproces migreert u het schema en de gegevens van de SQL Server-database in uw huidige omgeving naar SQL Database. Dit lukt alleen als de bestaande database eerst een compatibiliteitstest doorstaat. Met SQL Database V12 passen we een benadering op basis van [functiepariteit](sql-database-features.md) toe, behalve voor problemen met betrekking tot bewerkingen op serverniveau en tussen databases. Bij databases en toepassingen die afhankelijk zijn van [slechts gedeeltelijk of niet ondersteunde functies](sql-database-transact-sql-information.md), is een zekere mate van herstructurering vereist om compatibiliteitsproblemen op te lossen voordat de SQL Server-database kan worden gemigreerd.

Als u uw database wilt migreren, doorloopt u de volgende stappen:

* **Compatibiliteit testen**: valideer de databasecompatibiliteit met SQL Database. 
* **Eventuele compatibiliteitsproblemen oplossen**: als de validatie is mislukt, moet u de validatiefouten oplossen.  
* **De migratie uitvoeren**: is uw database eenmaal compatibel, dan kunt u een of meer methoden gebruiken om de migratie uit te voeren. 

SQL Server biedt verschillende methoden om elk van deze taken uit te voeren. Dit artikel biedt een overzicht van de beschikbare methoden voor elke taak. Het volgende diagram illustreert de stappen en de methoden.

  ![Diagram van VSSSDT-migratie](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

> [!NOTE]
> Voor het migreren van een SQL Server-database, met inbegrip van Microsoft Access, Sybase, MySQL Oracle en DB2, naar Azure SQL Database raadpleegt u [SQL Server Migration Assistant](http://blogs.msdn.com/b/ssma/) (Migratieassistent voor SQL Server).
> 
> 

## <a name="database-migration-tools-test-sql-server-database-compatibility-with-sql-database"></a>Hulpprogramma's voor databasemigratie testen de databasecompatibiliteit met de SQL-database
Om te testen op problemen met de SQL-databasecompatibiliteit voordat u het databasemigratieproces start, gebruikt u een van de volgende methoden:

> [!div class="op_single_selector"]
> * [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
> * [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
> * [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
> * [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
> 
> 

* [SQL Server Data Tools voor Visual Studio (SSDT)](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): SSDT gebruikt de recentste compatibiliteitsregels om incompatibiliteit met SQL Database V12 te detecteren. Als er incompatibiliteit wordt gedetecteerd, kunt u de gedetecteerde problemen direct in dit hulpprogramma oplossen. Dit is de aanbevolen methode om te testen op SQL Database V12-compatibiliteitsproblemen en deze op te lossen. 
* [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md): SqlPackage is een opdrachtregelprogramma dat test op compatibiliteitsproblemen en een rapport met gedetecteerde compatibiliteitsproblemen genereert. Als u dit hulpprogramma gebruikt, moet u ervoor zorgen dat u de nieuwste versie installeert, zodat u kunt profiteren van de recentste compatibiliteitsregels. Worden er fouten gedetecteerd, dan moet u een ander hulpprogramma gebruiken om gedetecteerde compatibiliteitsproblemen op te lossen. SSDT wordt hiervoor aanbevolen.  
* [De toepassingswizard Data-Tier exporteren in SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md): deze wizard detecteert fouten en rapporteert deze op het scherm. Als er geen fouten zijn gedetecteerd, kunt u doorgaan en de migratie naar SQL Database voltooien. Worden er fouten gedetecteerd, dan moet u een ander hulpprogramma gebruiken om gedetecteerde compatibiliteitsproblemen op te lossen. SSDT wordt hiervoor aanbevolen.
* [SQL Azure-migratiewizard (SAMW)](sql-database-cloud-migrate-fix-compatibility-issues.md): SAMW is een codeplex-hulpprogramma dat de compatibiliteitsregels van Azure SQL Database V11 gebruikt voor het detecteren van Azure SQL Database V12-compatibiliteitsproblemen. Als er incompatibiliteit wordt gedetecteerd, kunt u sommige problemen direct in dit hulpprogramma oplossen. Dit hulpprogramma kan vormen van incompatibiliteit detecteren die u niet hoeft op te lossen. Het was het eerste beschikbare hulpprogramma voor hulp bij Azure SQL Database-migratie en wordt actief ondersteund door de SQL Server-community. Met dit hulpprogramma kunt u ook de migratie voltooien vanuit het programma zelf. 

## <a name="fix-database-migration-compatibility-issues"></a>Compatibiliteitsproblemen met databasemigratie oplossen
Als er compatibiliteitsproblemen zijn gedetecteerd, moet u deze oplossen voordat u doorgaat met de migratie van uw SQL Server-database. Er zijn veel verschillende compatibiliteitsproblemen die zich kunnen voordoen. Dit is afhankelijk van de versie van SQL Server in de brondatabase en de complexiteit van de database die u migreert. Oudere versies van SQL Server hebben vaker te maken met compatibiliteitsproblemen. Gebruik de volgende resources en voer ook op internet een gerichte zoekopdracht uit (met uw zoekmachine):

* [SQL Server-databasefuncties die niet worden ondersteund in Azure SQL Database](sql-database-transact-sql-information.md)
* [Database Engine-functionaliteit in SQL Server 2016 stopgezet](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
* [Database Engine-functionaliteit in SQL Server 2014 stopgezet](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
* [Database Engine-functionaliteit in SQL Server 2012 stopgezet](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
* [Database Engine-functionaliteit in SQL Server 2008 R2 stopgezet](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
* [Database Engine-functionaliteit in SQL Server 2005 stopgezet](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

In aanvulling op deze resources en uw zoekopdrachten op internet kunt u ook de [MSDN SQL Server-communityforums](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) of [StackOverflow](http://stackoverflow.com/) raadplegen.

Gebruik een van de volgende hulpprogramma's voor databasemigratie om de gedetecteerde problemen op te lossen:

> [!div class="op_single_selector"]
> * [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
> * [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
> * [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
> 
> 

* [SQL Server Data Tools voor Visual Studio (SSDT)](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) gebruiken: voor het gebruik van SSDT importeert u uw databaseschema in SQL Server Data Tools voor Visual Studio (SSDT) en bouwt u het project voor een implementatie van SQL Database V12. U kunt dan alle gedetecteerde compatibiliteitsproblemen oplossen in SSDT. Als u klaar bent, synchroniseert u de wijzigingen naar de brondatabase (of naar een kopie van de brondatabase. SSDT is momenteel de aanbevolen methode om SQL Database V12-compatibiliteitsproblemen te testen en op te lossen. Volg de koppeling voor een [overzicht van het gebruik van SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md).
* [SQL Server Management Studio (SSMS)](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) gebruiken: als u SSMS wilt gebruiken, voert u de Transact-SQL-opdrachten uit om de gedetecteerde fouten te herstellen met een ander hulpprogramma. Deze methode is vooral bedoeld voor gevorderde gebruikers die het databaseschema direct in de brondatabase willen wijzigen. 
* [SQL Azure migratiewizard (SAMW)](sql-database-cloud-migrate-fix-compatibility-issues.md) gebruiken: als u SAMW wilt gebruiken, genereert u een Transact-SQL-script vanuit de brondatabase. De wizard transformeert het script, indien mogelijk, om het schema compatibel te maken met de SQL Database V12. Als u klaar bent, kan SAMW verbinding maken met SQL Database V12 voor het uitvoeren van het script. Dit hulpprogramma analyseert ook traceringsbestanden om problemen met de compatibiliteit vast te stellen. Het script kan worden gegenereerd met alleen het schema of met gegevens in BCP-indeling.

## <a name="migrate-a-compatible-sql-server-database-to-sql-database"></a>Een compatibele SQL Server-database migreren naar SQL Database
Voor de migratie van een compatibele SQL Server-database levert Microsoft meerdere migratiemethoden voor verschillende scenario's. Welke methode u kiest, is afhankelijk van de tolerantie voor uitvaltijd, de grootte en de complexiteit van de SQL Server-database, en de verbinding met de Microsoft Azure-cloud.  

> [!div class="op_single_selector"]
> * [SSMS-migratiewizard](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
> * [Een BACPAC-bestand exporteren](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
> * [Importeren vanuit BACPAC-bestand](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
> * [Transactionele replicatie](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
> 
> 

Bij het kiezen van de meest geschikte migratiemethode moet u zich eerst afvragen of u het zich kunt permitteren om de database tijdens de migratie uit productie te halen. Migratie van een database terwijl er actieve transacties plaatsvinden, kan leiden tot database-inconsistenties en tot mogelijke beschadiging van de database. Er zijn veel methoden om een database stil te leggen, van het uitschakelen van de clientconnectiviteit tot het maken van een [databasemomentopname](https://msdn.microsoft.com/library/ms175876.aspx).

Als u de migratie wilt uitvoeren met minimale uitvaltijd, gebruikt u [SQL Server-transactiereplicatie](sql-database-cloud-migrate-compatible-using-transactional-replication.md). Uw database moet dan wel voldoen aan de vereisten voor transactionele replicatie. Als u zich enige uitvaltijd kunt permitteren of als u met het oog op een latere migratie een testmigratie van een productiedatabase uitvoert, kunt u een van de volgende drie methoden overwegen:

* [SSMS-migratiewizard](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md): voor kleine tot middelgrote databases is het migreren van een compatibele SQL Server 2005- of latere database net zo eenvoudig als het uitvoeren van de [wizard Deploy Database to Microsoft Azure](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) in SQL Server Management Studio.
* [Exporteren naar BACPAC-bestand](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) en vervolgens [importeren vanuit PACPAC-bestand](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md): als u verbindingsproblemen hebt (geen verbinding, lage bandbreedte of time-outproblemen), gebruikt u voor middelgrote tot grote databases een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4)-bestand. Met deze methode exporteert u het SQL Server-schema en de gegevens naar een BACPAC-bestand. Vervolgens importeert u het BACPAC-bestand in SQL Database met de wizard Export Data Tier Application in SQL Server Management Studio of met het opdrachtprompthulpprogramma [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx).
* BACPAC en BCP samen gebruiken: gebruik een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4)-bestand en [BCP](https://msdn.microsoft.com/library/ms162802.aspx) voor veel grotere databases om een grotere parallellisering te bereiken voor betere prestaties. Dit leidt echter ook tot een hogere mate van complexiteit. Met deze methode worden het schema en de gegevens afzonderlijk gemigreerd.
  
  * [Alleen het schema exporteren naar een BACPAC-bestand](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md).
  * [Alleen het schema vanuit het BACPAC-bestand importeren](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) in SQL Database.
  * Gebruik [BCP](https://msdn.microsoft.com/library/ms162802.aspx) om gegevens op te halen in platte bestanden en laad deze bestanden vervolgens [parallel](https://technet.microsoft.com/library/dd425070.aspx) in Azure SQL Database.
    
     ![SQL Server-databasemigratie - SQL-database migreren naar de cloud.](./media/sql-database-cloud-migrate/01SSMSDiagram_new.png)

## <a name="next-steps"></a>Volgende stappen
* [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
* [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Aanvullende bronnen
* [SQL Database-functies](sql-database-features.md)
  [Transact-SQL gedeeltelijk of niet ondersteunde functies](sql-database-transact-sql-information.md)
* [Niet-SQL Server-databases migreren met de SQL Server Migration Assistant](http://blogs.msdn.com/b/ssma/)




<!--HONumber=Jan17_HO1-->


