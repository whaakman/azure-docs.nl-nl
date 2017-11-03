---
title: Ontwerp van uw eerste Azure SQL database - C# | Microsoft Docs
description: Informatie over het ontwerpen van uw eerste Azure SQL database en verbinding maken met een C#-programma ADO.NET gebruiken.
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg-msft
editor: CarlRabeler
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: develop databases, mvc, devcenter
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 08/25/2017
ms.author: genemi
ms.openlocfilehash: 7d6c1ca0f48974f8a7d839b23f71d43bfeb400ba
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a>Ontwerp van een Azure SQL database en verbinding maken met C & #x23; en ADO.NET

Azure SQL-Database is een relationele database als een service (DBaaS) in de Microsoft-Cloud ('Azure'). In deze zelfstudie leert u hoe de Azure-portal en ADO.NET met Visual Studio te gebruiken: 

> [!div class="checklist"]
> * Maak een database in de Azure-portal
> * Een firewallregel op serverniveau in de Azure portal instellen
> * Verbinding maken met de database met ADO.NET en Visual Studio
> * Tabellen maken met ADO.NET
> * Invoegen, bijwerken en verwijderen van gegevens met ADO.NET 
> * Querygegevens ADO.NET

Als u een Azure-abonnement geen [een gratis account maken](https://azure.microsoft.com/free/) voordat u begint.

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

In deze zelfstudie hebt u geleerd basic databasetaken, zoals een database en tabellen maken, laden en een query over gegevens en de database naar een eerder tijdstip herstellen. U hebt geleerd hoe u:
> [!div class="checklist"]
> * Een database maken
> * Een firewallregel instellen
> * Verbinding maken met de database met [Visual Studio en C#](sql-database-connect-query-dotnet-visual-studio.md)
> * Tabellen maken
> * Invoegen, bijwerken en verwijderen van gegevens
> * Querygegevens

Ga naar de volgende zelfstudie voor meer informatie over het migreren van uw gegevens.

> [!div class="nextstepaction"]
>[De SQL Server-database migreren naar Azure SQL Database](sql-database-migrate-your-sql-server-database.md)

