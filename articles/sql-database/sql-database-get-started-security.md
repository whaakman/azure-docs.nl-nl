---
title: 'SQL Database-zelfstudie: Aan de slag met beveiliging'
description: Lees hoe u gebruikersaccounts maakt voor toegang tot en beheer van een database.
keywords: 
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 67797b09-f5c3-4ec2-8494-fe18883edf7f
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/17/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 09c2332589b1170b411c6f45f4109fb8048887e2
ms.openlocfilehash: e846654b659c1240451e3ac26973fbfce206936d


---
# <a name="sql-database-tutorial-create-sql-database-user-accounts-to-access-and-manage-a-database"></a>SQL Database-zelfstudie: SQL Database-gebruikersaccounts maken om toegang tot een database te krijgen en deze te beheren
> [!div class="op_single_selector"]
> * [Zelfstudie Aan de slag](sql-database-get-started-security.md)
> * [Toegang verlenen](sql-database-manage-logins.md)
> 
> 

In deze zelfstudie leert u hoe u met SQL Server Management Studio (SSMS) de volgende handelingen verricht:

* Aanmelden bij SQL Database met een principal-aanmelding op serverniveau.
* Een SQL Database-gebruikersaccount maken.
* Een SQL Database-gebruiker [db_owner-machtigingen](https://msdn.microsoft.com/library/ms189121.aspx#Anchor_0) toekennen.
* Met een SQL-database verbinding maken met een gebruikersaccount dat geen principal op serverniveau is.

[!INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

[!INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]

[!INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]

[!INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]

[!INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]

## <a name="next-steps"></a>Volgende stappen
Nu u deze zelfstudie voor SQL Database hebt voltooid, een gebruikersaccount hebt gemaakt en dit account dbo-machtigingen hebt verleend, kunt u meer gaan lezen over [beveiliging van SQL Database](sql-database-manage-logins.md).




<!--HONumber=Dec16_HO1-->


