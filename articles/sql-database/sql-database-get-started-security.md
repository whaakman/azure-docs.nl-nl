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
    ms.date="07/19/2016"
    ms.author="carlrab"/>

# SQL Database-zelfstudie: SQL Database-gebruikersaccounts maken voor toegang tot en beheer van een database met de Azure-portal


> [AZURE.SELECTOR]
- [Zelfstudie Aan de slag](sql-database-get-started-security.md)
- [Toegang verlenen](sql-database-manage-logins.md)

In deze zelfstudie leert u hoe u de Azure-portal gebruikt voor het volgende:

- Aanmelden bij SQL Database met een principal-aanmelding op serverniveau
- Een SQL Database-gebruikersaccount maken
- Een SQL Database-gebruikersaccount dbo-machtigingen geven binnen een gebruikersdatabase
- Een SQL-database koppelen aan een gebruikersaccount dat geen principal op serverniveau is 

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]


[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]


## Volgende stappen
Nu u deze zelfstudie voor SQL Database hebt voltooid, een gebruikersaccount hebt gemaakt en dit account dbo-machtigingen hebt verleend, kunt u meer gaan lezen over [beveiliging van SQL Database](sql-database-manage-logins.md).





<!--HONumber=ago16_HO4-->


