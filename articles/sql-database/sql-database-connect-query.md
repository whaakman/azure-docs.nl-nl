<properties
    pageTitle="Verbinding maken met SQL Database met een C#-query | Microsoft Azure"
    description="Schrijf een programma in C# om een SQL Database op te vragen en hiermee verbinding te maken. Informatie over IP-adressen, verbindingsreeksen, beveiligde aanmelding en gratis Visual Studio."
    services="sql-database"
    keywords="c#-databasequery, c#-query, verbinding maken met database, SQL C#"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="stevestein"/>




# Verbinding maken met een SQL Database met behulp van Visual Studio

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Lees hoe u verbinding kunt maken met een Azure SQL-database in Visual Studio. 

## Vereisten


Als u verbinding wilt maken met een SQL-database met behulp van Visual Studio, hebt u het volgende nodig: 


- Een SQL-database waarmee u verbinding kunt maken. In dit artikel wordt de voorbeelddatabase **AdventureWorks** gebruikt. Zie [De demodatabase maken](sql-database-get-started.md) voor informatie over hoe u aan de AdventureWorks-database komt.


- Visual Studio 2013 update 4 (of hoger). Microsoft biedt de Visual Studio Community nu *gratis* aan.
 - [Visual Studio Community downloaden](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [Meer opties voor gratis Visual Studio](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)




## Visual Studio openen via de Azure Portal


1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Klik op **Meer services** > **SQL-databases**
3. Open de blade van de **AdventureWorks**-database door de *AdventureWorks*-database te zoeken en erop te klikken.

6. Klik op de knop **Extra** boven aan de databaseblade:

    ![Nieuwe query. Verbinding maken met de SQL Database-server: SQL Server Management Studio](./media/sql-database-connect-query/tools.png)

7. Klik op **Openen in Visual Studio** (als u Visual Studio nodig hebt, klikt u op de downloadkoppeling):

    ![Nieuwe query. Verbinding maken met de SQL Database-server: SQL Server Management Studio](./media/sql-database-connect-query/open-in-vs.png)


8. Visual Studio wordt geopend, waarbij het venster **Verbinding maken met server** al zo is ingesteld dat verbinding wordt gemaakt met de server en de database die u in de portal hebt geselecteerd.  (Klik op **Opties** om te controleren of de verbinding is ingesteld op de juiste database.) Typ het beheerderswachtwoord van de server en klik op **Verbinden**.


    ![Nieuwe query. Verbinding maken met de SQL Database-server: SQL Server Management Studio](./media/sql-database-connect-query/connect.png)


8. Als u nog geen een firewallregel voor het IP-adres van uw computer hebt ingesteld, wordt het bericht *Kan geen verbinding maken* weergegeven. Zie [Een firewallregel op serverniveau voor Azure SQL Database configureren](sql-database-configure-firewall-settings.md) voor informatie over het maken van een firewallregel.


9. Nadat de verbinding tot stand is gebracht, wordt het venster **SQL Server-objectverkenner** geopend met een verbinding met uw database.

    ![Nieuwe query. Verbinding maken met de SQL Database-server: SQL Server Management Studio](./media/sql-database-connect-query/sql-server-object-explorer.png)


## Een voorbeeldquery uitvoeren

Nu u bent verbonden met de database, wordt in de volgende stappen uitgelegd hoe u een eenvoudige query uitvoert:

2. Klik met de rechtermuisknop op de database en selecteer **Nieuwe query**.

    ![Nieuwe query. Verbinding maken met de SQL Database-server: SQL Server Management Studio](./media/sql-database-connect-query/new-query.png)

3. Kopieer en plak de volgende code in het queryvenster.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. Klik op de knop **Uitvoeren** om de query uit te voeren:

    ![Geslaagd. Verbinding maken met de SQL Database-server: SVisual Studio](./media/sql-database-connect-query/run-query.png)

## Volgende stappen

- Bij het openen van SQL-databases in Visual Studio wordt gebruik gemaakt van SQL Server Data Tools. Zie [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686.aspx) voor meer informatie.
- Zie [Verbinding maken met SQL Database met behulp van .NET (C#)](sql-database-develop-dotnet-simple.md) als u via code verbinding wilt maken met een SQL-database.






<!--HONumber=Sep16_HO3-->


