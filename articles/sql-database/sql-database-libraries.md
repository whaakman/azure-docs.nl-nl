---
title: Verbindingsbibliotheken voor SQL-Database | Microsoft Docs
description: Koppelingen voor het downloaden van modules die verbinding maken met SQL Server en SQL-Database van tal van client programmeertalen inschakelen.
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: genemi
ms.assetid: 13d899d3-cf46-4e4d-8919-cf4b41ca836d
ms.service: sql-database
ms.custom: develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: genemi
ms.openlocfilehash: a01ccd8b0e7c4759b3731adba9d37b7ce5afd764
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="connectivity-libraries-and-frameworks-for-sql-server"></a>Connectiviteit bibliotheken en frameworks voor SQL Server

Bekijk onze [ophalen zelfstudies gestart](http://aka.ms/sqldev) snel aan de slag met programmeertalen zoals C#, Java, Node.js, PHP en Python. Vervolgens maken een app met behulp van SQL Server voor Linux- of Windows- of Docker op Mac OS.

De volgende tabel bevat de bibliotheken voor databaseconnectiviteit of *stuurprogramma's* die clienttoepassingen van tal van talen verbinding maken met en gebruik van SQL Server lokaal wordt uitgevoerd of in de cloud kunnen gebruiken. U kunt deze gebruiken op Linux-, Windows- of Docker en gebruiken ze verbinding maken met Azure SQL Database en Azure SQL Data Warehouse. 

| Taal | Platform | Aanvullende resources | Downloaden | Aan de slag |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Microsoft ADO.NET voor SQL Server](https://docs.microsoft.com/sql/connect/ado-net/microsoft-ado-net-for-sql-server) | [Downloaden](https://www.microsoft.com/net/download/) | [Aan de slag](https://www.microsoft.com/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows, Linux, macOS | [Microsoft JDBC-stuurprogramma voor SQL Server](http://msdn.microsoft.com/library/mt484311.aspx) | [Downloaden](https://go.microsoft.com/fwlink/?linkid=852460) |  [Aan de slag](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows, Linux, macOS| [PHP-SQL-stuurprogramma voor SQL Server](http://msdn.microsoft.com/library/dn865013.aspx) | Besturingssysteem: <br/> \* [Windows](https://www.microsoft.com/download/details.aspx?id=55642) <br/> \* [Linux](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) <br/> \* [macOS](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) |  [Aan de slag](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu)
| Node.js | Windows, Linux, macOS | [Node.js-stuurprogramma voor SQL Server](http://msdn.microsoft.com/library/mt652093.aspx) | [Installeren](https://msdn.microsoft.com/library/mt652094.aspx) |  [Aan de slag](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu)
| Python | Windows, Linux, macOS | [Python-SQL-stuurprogramma](http://msdn.microsoft.com/library/mt652092.aspx) | Installeer keuzes: <br/> \* [pymssql](https://msdn.microsoft.com/library/mt694094.aspx) <br/> \* [pyodbc](http://msdn.microsoft.com/library/mt763257.aspx) |  [Aan de slag](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows, Linux, macOS | [Ruby stuurprogramma voor SQL Server](http://msdn.microsoft.com/library/mt691981.aspx) | [Installeren](https://msdn.microsoft.com/library/mt711041.aspx) | [Aan de slag](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows, Linux, macOS | [ODBC-stuurprogramma voor SQL Server](https://msdn.microsoft.com/library/mt654048(v=sql.1).aspx) | [Downloaden](https://msdn.microsoft.com/library/mt654048(v=sql.1).aspx) |  

De volgende tabel bevat voorbeelden van object-relationele gegevens (ORM) frameworks en web-frameworks die clienttoepassingen gebruiken kunnen met SQL Server lokaal wordt uitgevoerd of in de cloud. U kunt de frameworks gebruiken op Linux-, Windows- of Docker en ze verbinding maken met SQL-Database en SQL Data Warehouse te gebruiken. 

| Taal | Platform | ORM(s) |
| :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Entity Framework](https://docs.microsoft.com/ef)<br>[Entity Framework Core](https://docs.microsoft.com/ef/core/index) |
| Java | Windows, Linux, macOS |[Sluimerstand ORM](http://hibernate.org/orm)|
| PHP | Windows, Linux | [Laravel (Eloquent)](https://laravel.com/docs/5.0/eloquent) |
| Node.js | Windows, Linux, macOS | [ORM sequelize](http://docs.sequelizejs.com) |
| Python | Windows, Linux, macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Windows, Linux, macOS | [Ruby op Rails](http://rubyonrails.org/) |
||||

## <a name="related-links"></a>Verwante koppelingen
- [SQL Server-stuurprogramma](http://msdn.microsoft.com/library/mt654049.aspx) die worden gebruikt voor het verbinding maken vanaf de client-toepassingen
- Verbinding maken met SQL-Database:
    - [Verbinding maken met SQL Database met behulp van .NET (C#)](sql-database-connect-query-dotnet.md)
    - [Verbinding maken met SQL Database met behulp van PHP](sql-database-connect-query-php.md)
    - [Verbinding maken met SQL Database met behulp van Node.js](sql-database-connect-query-nodejs.md)
    - [Verbinding maken met SQL Database met behulp van Java](sql-database-connect-query-java.md)
    - [Verbinding maken met SQL Database met behulp van Python](sql-database-connect-query-python.md)
    - [Verbinding maken met SQL Database met behulp van Ruby](sql-database-connect-query-ruby.md)
- Probeer de codevoorbeelden logica:
    - [Resiliently verbinding te maken met SQL met ADO.NET][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
    - [Resiliently verbinding te maken met SQL met PHP][step-4-connect-resiliently-to-sql-with-php-p42h]


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php

