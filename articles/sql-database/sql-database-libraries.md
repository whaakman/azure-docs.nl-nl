---
title: Verbindingsbibliotheken voor SQL-Database | Microsoft Docs
description: Koppelingen voor het downloaden van modules die verbinding maken met SQL Server en SQL-Database van tal van client programmeertalen inschakelen. De modules worden vrijgegeven door de community of door Microsoft.
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: genemi
ms.assetid: 13d899d3-cf46-4e4d-8919-cf4b41ca836d
ms.service: sql-database
ms.custom: develop apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: genemi
ms.openlocfilehash: 082abf57b139b9f7d44774dce3a80e20b97f0e3b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="connectivity-libraries-and-frameworks-for-microsoft-sql-server"></a>Connectiviteit bibliotheken en frameworks voor Microsoft SQL Server

Bekijk onze [gestart zelfstudies ophalen](http://aka.ms/sqldev) snel aan de slag met programmeertalen zoals C#, Java, Node.js, PHP en Python en bouwen van een app met behulp van SQL Server voor Linux- of Windows- of Docker op Mac OS.

De volgende tabel bevat de bibliotheken voor databaseconnectiviteit of *stuurprogramma's* die clienttoepassingen van tal van talen verbinding maken met en gebruik van Microsoft SQL Server lokaal wordt uitgevoerd of in de cloud, op Linux-, Windows- of Docker en ook voor kunnen gebruiken Azure SQL Database en Azure SQL datawarehouse. 

| Taal | Platform | Aanvullende bronnen | Downloaden | Aan de slag |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows, Linux, Mac OS | [Microsoft ADO.NET voor SQL Server](https://docs.microsoft.com/sql/connect/ado-net/microsoft-ado-net-for-sql-server) | [Downloaden](https://www.microsoft.com/net/download/) | [Aan de slag](https://www.microsoft.com/en-us/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows, Linux, Mac OS | [Microsoft JDBC-stuurprogramma voor SQL Server](http://msdn.microsoft.com/library/mt484311.aspx) | [Downloaden](https://go.microsoft.com/fwlink/?linkid=852460) |  [Aan de slag](https://www.microsoft.com/en-us/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows, Linux, Mac OS| [PHP-SQL-stuurprogramma voor SQL Server](http://msdn.microsoft.com/library/dn865013.aspx) | Besturingssysteem: <br/> \*[Windows](https://www.microsoft.com/download/details.aspx?id=20098) <br/> \*[Linux](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) <br/> \*[Mac OS](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) |  [Aan de slag](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/ubuntu)
| Node.js | Windows, Linux, Mac OS | [Node.js-stuurprogramma voor SQL Server](http://msdn.microsoft.com/library/mt652093.aspx) | [Installeren](https://msdn.microsoft.com/library/mt652094.aspx) |  [Aan de slag](https://www.microsoft.com/en-us/sql-server/developer-get-started/node/ubuntu)
| Python | Windows, Linux, Mac OS | [Python-SQL-stuurprogramma](http://msdn.microsoft.com/library/mt652092.aspx) | Installeer keuzes: <br/> \*[pymssql](https://msdn.microsoft.com/library/mt694094.aspx) <br/> \*[pyodbc](http://msdn.microsoft.com/library/mt763257.aspx) |  [Aan de slag](https://www.microsoft.com/en-us/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows, Linux, Mac OS | [Ruby stuurprogramma voor SQL Server](http://msdn.microsoft.com/library/mt691981.aspx) | [Installeren](https://msdn.microsoft.com/library/mt711041.aspx) | [Aan de slag](https://www.microsoft.com/en-us/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows, Linux, Mac OS | [Microsoft ODBC-stuurprogramma voor SQL Server](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) | [Downloaden](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) |  

De volgende tabel bevat enkele voorbeelden van Object relationele toewijzing (ORM) frameworks en web-frameworks die clienttoepassingen gebruiken kunnen met Microsoft SQL Server lokaal wordt uitgevoerd of in de cloud, op Linux-, Windows- of Docker en naar Azure SQL Database en Azure SQL datawarehouse. 

| Taal | Platform | ORM(s) |
| :-- | :-- | :-- |
| C# | Windows, Linux, Mac OS | [Entity Framework](https://docs.microsoft.com/en-us/ef)<br>[Entity Framework Core](https://docs.microsoft.com/en-us/ef/core/index) |
| Java | Windows, Linux, Mac OS |[Sluimerstand ORM](http://hibernate.org/orm)|
| PHP | Windows, Linux | [Laravel (Eloquent)](https://laravel.com/docs/5.0/eloquent) |
| Node.js | Windows, Linux, Mac OS | [ORM sequelize](http://docs.sequelizejs.com) |
| Python | Windows, Linux, Mac OS |[Django](https://www.djangoproject.com/) |
| Ruby | Windows, Linux, Mac OS | [Ruby op Rails](http://rubyonrails.org/) |

## <a name="related-links"></a>Verwante koppelingen
- [SQL Server-stuurprogramma's](http://msdn.microsoft.com/library/mt654049.aspx) om verbinding te maken van clienttoepassingen
- [Verbinding maken met SQL Database met behulp van .NET (C#)](sql-database-connect-query-dotnet.md)
- [Verbinding maken met SQL Database met behulp van PHP](sql-database-connect-query-php.md)
- [Verbinding maken met SQL Database met behulp van Node.js](sql-database-connect-query-nodejs.md)
- [Verbinding maken met SQL Database met behulp van Java](sql-database-connect-query-java.md)
- [Verbinding maken met SQL Database met behulp van Python](sql-database-connect-query-python.md)
- [Verbinding maken met SQL Database met behulp van Ruby](sql-database-connect-query-ruby.md)
