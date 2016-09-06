<properties
    pageTitle="SQL Database-zelfstudie: Aan de slag met beveiliging"
    description="Lees hoe u gebruikersaccounts maakt voor toegang tot en beheer van een database."
    keywords=""
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
    ms.date="08/17/2016"
    ms.author="carlrab"/>

# SQL Database-zelfstudie: SQL Database-gebruikersaccounts maken om toegang tot een database te krijgen en deze te beheren


> [AZURE.SELECTOR]
- [Zelfstudie Aan de slag](sql-database-get-started-security.md)
- [Toegang verlenen](sql-database-manage-logins.md)

In deze zelfstudie leert u hoe u met SQL Server Management Studio (SSMS) de volgende handelingen verricht:

- Aanmelden bij SQL Database met een principal-aanmelding op serverniveau.
- Een SQL Database-gebruikersaccount maken.
- Een SQL Database-gebruiker [db_owner-machtigingen](https://msdn.microsoft.com/library/ms189121.aspx#Anchor_0) toekennen.
- Met een SQL-database verbinding maken met een gebruikersaccount dat geen principal op serverniveau is.

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]


[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]


## Volgende stappen
Nu u deze zelfstudie voor SQL Database hebt voltooid, een gebruikersaccount hebt gemaakt en dit account dbo-machtigingen hebt verleend, kunt u meer gaan lezen over [beveiliging van SQL Database](sql-database-manage-logins.md).





<!--HONumber=ago16_HO5-->


