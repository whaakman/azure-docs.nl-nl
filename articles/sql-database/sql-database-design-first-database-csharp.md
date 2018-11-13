---
title: Uw eerste Azure SQL database - C# ontwerpen | Microsoft Docs
description: Informatie over het ontwerpen van uw eerste Azure SQL database en het verbinding maken met een C#-programma dat ADO.NET gebruikt.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: tutorial
author: MightyPen
ms.author: genemi
ms.reviewer: carlrab
manager: craigg-msft
ms.date: 11/01/2018
ms.openlocfilehash: 82cf0303019d2cbb620c442fd6f750f733930f84
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50912336"
---
# <a name="tutorial-design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a>Zelfstudie: Een Azure SQL-database ontwerpen en verbinding maken met C# en ADO.NET

Azure SQL Database is een relationele DBaaS (database-as-a-service) in de Microsoft Cloud (Azure). In deze zelfstudie leert u hoe u Azure Portal en ADO.NET met Visual Studio gebruikt voor de volgende taken:

> [!div class="checklist"]
> * Een database maken in Azure Portal
> * Een serverfirewallregel instellen in Azure Portal
> * Verbinding maken met de database met ADO.NET en Visual Studio
> * Tabellen maken met ADO.NET
> * Gegevens invoegen, bijwerken en verwijderen met ADO.NET
> * Querygegevens ADO.NET

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Een installatie van [Visual Studio Community 2017, Visual Studio Professional 2017 of Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).

<!-- The following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]

<!-- The following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u kennisgemaakt met eenvoudige databasetaken, zoals het maken van een database en tabellen, het laden en opvragen van gegevens, en het herstellen van een database naar een eerder tijdstip. U hebt geleerd hoe u:
> [!div class="checklist"]
> * Een database maken
> * Een firewallregel instellen
> * Verbinding maken met de database met behulp van [Visual Studio en C#](sql-database-connect-query-dotnet-visual-studio.md)
> * Tabellen maken
> * Invoegen, bijwerken en verwijderen van gegevens
> * Querygegevens

Ga naar de volgende zelfstudie voor meer informatie over het migreren van uw gegevens.

> [!div class="nextstepaction"]
> [Uw SQL Server-database migreren naar Azure SQL Database](sql-database-migrate-your-sql-server-database.md)
