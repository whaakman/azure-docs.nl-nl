---
title: Verbindingsbibliotheken voor SQL Database | Microsoft Docs
description: Koppelingen voor het downloaden van de modules waarmee verbinding met SQL Server en SQL-Database vanuit een brede verscheidenheid client programmeertalen.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: ''
manager: craigg
ms.date: 01/15/2019
ms.openlocfilehash: 088ebd37a4dfcd2b0d68d2519a8b70f7064244b2
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55561050"
---
# <a name="connectivity-libraries-and-frameworks-for-sql-server"></a>Connectiviteitsbibliotheken en frameworks voor SQL Server

Bekijk onze [ophalen gestart zelfstudies](https://aka.ms/sqldev) snel aan de slag met programmeertalen, zoals C#, Java, Node.js, PHP en Python. Vervolgens moet u een app bouwen met behulp van SQL Server op Linux of Windows- of Docker op macOS.

De volgende tabel bevat connectiviteitsbibliotheken of *stuurprogramma's* die clienttoepassingen uit een groot aantal talen verbinding maken gebruiken kunnen met en gebruiken van SQL Server on-premises uitgevoerd of in de cloud. U kunt ze op Linux-, Windows- of Docker gebruiken en deze gebruiken om te verbinden met Azure SQL Database en Azure SQL Data Warehouse. 

| Taal | Platform | Aanvullende resources | Downloaden | Aan de slag |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Microsoft ADO.NET voor SQL Server](https://docs.microsoft.com/sql/connect/ado-net/microsoft-ado-net-for-sql-server) | [Downloaden](https://www.microsoft.com/net/download/) | [Aan de slag](https://www.microsoft.com/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows, Linux, macOS | [Microsoft JDBC-stuurprogramma voor SQL Server](https://msdn.microsoft.com/library/mt484311.aspx) | [Downloaden](https://go.microsoft.com/fwlink/?linkid=852460) |  [Aan de slag](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows, Linux, macOS| [PHP-SQL-stuurprogramma voor SQL Server](https://docs.microsoft.com/sql/connect/php/microsoft-php-driver-for-sql-server) | [Downloaden](https://docs.microsoft.com/sql/connect/php/download-drivers-php-sql-server) | [Aan de slag](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/)
| Node.js | Windows, Linux, macOS | [Node.js-stuurprogramma voor SQL Server](https://msdn.microsoft.com/library/mt652093.aspx) | [Installeren](https://msdn.microsoft.com/library/mt652094.aspx) |  [Aan de slag](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu)
| Python | Windows, Linux, macOS | [SQL Python-stuurprogramma](https://msdn.microsoft.com/library/mt652092.aspx) | Opties installeren: <br/> \* [pymssql](https://msdn.microsoft.com/library/mt694094.aspx) <br/> \* [pyodbc](https://msdn.microsoft.com/library/mt763257.aspx) |  [Aan de slag](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows, Linux, macOS | [Ruby-stuurprogramma voor SQL Server](https://msdn.microsoft.com/library/mt691981.aspx) | [Installeren](https://msdn.microsoft.com/library/mt711041.aspx) | [Aan de slag](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows, Linux, macOS | [Microsoft ODBC-stuurprogramma voor SQL Server](https://msdn.microsoft.com/library/mt654048(v=sql.1).aspx) | [Downloaden](https://msdn.microsoft.com/library/mt654048(v=sql.1).aspx) |  

De volgende tabel bevat voorbeelden van object-relationele gegevens (ORM)-frameworks en web-frameworks die clienttoepassingen gebruiken kunnen met SQL Server on-premises uitgevoerd of in de cloud. U kunt de frameworks gebruiken op Linux-, Windows- of Docker en deze gebruiken om te verbinden met SQL-Database en SQL Data Warehouse. 

| Taal | Platform | ORM(s) |
| :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Entity Framework](https://docs.microsoft.com/ef)<br>[Entity Framework Core](https://docs.microsoft.com/ef/core/index) |
| Java | Windows, Linux, macOS |[Sluimerstand ORM](https://hibernate.org/orm)|
| PHP | Windows, Linux, macOS | [Laravel (Eloquent)](https://laravel.com/docs/5.0/eloquent) |
| Node.js | Windows, Linux, macOS | [ORM sequelize](https://docs.sequelizejs.com) |
| Python | Windows, Linux, macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Windows, Linux, macOS | [Ruby on Rails](https://rubyonrails.org/) |
||||

## <a name="related-links"></a>Verwante koppelingen
- [Drivers voor SQL Server](https://msdn.microsoft.com/library/mt654049.aspx) die verbinding maken vanuit clienttoepassingen worden gebruikt
- Verbinding maken met SQL Database:
    - [Verbinding maken met SQL Database met behulp van .NET (C#)](sql-database-connect-query-dotnet.md)
    - [Verbinding maken met SQL Database met behulp van PHP](sql-database-connect-query-php.md)
    - [Verbinding maken met SQL Database met behulp van Node.js](sql-database-connect-query-nodejs.md)
    - [Verbinding maken met SQL Database met behulp van Java](sql-database-connect-query-java.md)
    - [Verbinding maken met SQL Database met behulp van Python](sql-database-connect-query-python.md)
    - [Verbinding maken met SQL Database met behulp van Ruby](sql-database-connect-query-ruby.md)
- Voorbeelden van code logica voor opnieuw proberen:
    - [Flexibel verbinden met SQL via ADO.NET][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
    - [Flexibel verbinden met SQL via PHP][step-4-connect-resiliently-to-sql-with-php-p42h]


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php

