<properties
    pageTitle="SQL Database tutorial: een SQL database maken | Microsoft Azure"
    description="Informatie over het instellen van een logische SQL Database-server, een serverfirewallregel, een SQL Database en voorbeeldgegevens. Lees ook hoe u verbinding met clienthulpprogramma's maakt, gebruikers configureert en een databasefirewallregel instelt."
    keywords="zelfstudie over sql-database, een sql-database maken"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/05/2016"
    ms.author="carlrab"/>


# SQL Database tutorial: maak in slechts enkele minuten een SQL Database met behulp van Azure Portal

**Individuele database**

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

In deze zelfstudie leert u hoe u de Azure-portal kunt gebruiken om:

- Een logische Azure SQL Databaseserver voor SQL Databases te maken.
- Een SQL Database zonder gegevens te maken, met voorbeeldgegevens of met gegevens uit een back-up van een SQL Database.
- Een firewallregel op serverniveau kunt maken voor een individueel IP-adres of voor een aantal IP-adressen.

U kunt dezelfde taken uitvoeren met [C#](sql-database-get-started-csharp.md) of [PowerShell](sql-database-get-started-powershell.md).

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

<a name="create-logical-server-bk"></a>

[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-create-new-server-portal.md)]

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-portal.md)]

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-server-firewall-portal.md)]

## Volgende stappen
Nu u deze SQL Database-tutorial hebt afgerond en een database hebt gemaakt met wat voorbeeldgegevens, bent u klaar om verder te gaan met uw favoriete tools.

- Als u bekend bent met Transact-SQL en SQL Server Management Studio (SSMS), ontdek dan hoe u [verbinding kunt maken met een SQL Database en een query op de database kunt uitvoeren met SSMS](sql-database-connect-query-ssms.md).

- Bent u bekend met Excel? Ontdek dan hoe u [verbinding kunt maken met een SQL Database in Azure via Excel](sql-database-connect-excel.md).

- Als u wilt gaan coderen, kiest u uw programmeertaal in [Verbindingsbibliotheken voor SQL Database en SQL Server](sql-database-libraries.md).

- Als u uw on-premises SQL Server-database wilt verplaatsen naar Azure, raadpleegt u [Een database migreren naar SQL Database](sql-database-cloud-migrate.md) voor meer informatie.

- Als u met het opdrachtregelprogramma BCP enkele gegevens vanuit een csv-bestand in een nieuwe tabel wilt laden, leest u [Met BCP gegevens vanuit een csv-bestand in SQL Database laden](sql-database-load-from-csv-with-bcp.md).


## Aanvullende resources

[Wat is SQL Database?](sql-database-technical-overview.md)



<!--HONumber=sep16_HO1-->


