---
title: Ruby gebruiken om een query uit te voeren voor een Azure SQL-database | Microsoft Docs
description: In dit onderwerp ziet u hoe u Ruby gebruikt om een programma te maken dat is verbonden met een Azure SQL-database, en hoe u een query voor deze database uitvoert met behulp van Transact-SQL-instructies.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ruby
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 12/20/2018
ms.openlocfilehash: 66819cbd65f6f044d0dac68326eb5890476964b6
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53993906"
---
# <a name="quickstart-use-ruby-to-query-an-azure-sql-database"></a>Quickstart: Ruby gebruiken om een query uit te voeren voor een Azure SQL-database

In deze snelstart wordt gedemonstreerd hoe u [Ruby](https://www.ruby-lang.org) gebruikt om verbinding te maken met een Azure SQL Database en hoe u met Transact-SQL-instructies een query uitvoert voor de gegevens.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze snelstart moet aan de volgende vereisten worden voldaan:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]
  
- Een [firewallregel op serverniveau](sql-database-get-started-portal-firewall.md) voor het openbare IP-adres van de computer die u gebruikt voor deze snelstart.
  
- Ruby en verwante software voor uw besturingssysteem:
  
  - **MacOS**: Installeer Homebrew, rbenv en ruby-build, Ruby, FreeTDS en TinyTDS. Zie stappen 1.2, 1.3, 1.4, 1.5 en 2.1 in [Ruby-apps maken met behulp van SQL Server in macOS](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).
  
  - **Ubuntu**: Installatievoorwaarden voor Ruby, rbenv en ruby-build, Ruby, FreeTDS en TinyTDS. Zie stappen 1.2, 1.3, 1.4, 1.5 en 2.1 in [Ruby-apps maken met behulp van SQL Server in Ubuntu](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).
  
  - **Windows**: Installeer Ruby, Ruby Devkit en TinyTDS. Zie [Uw ontwikkelomgeving configureren voor ontwikkeling in Ruby](/sql/connect/ruby/step-1-configure-development-environment-for-ruby-development).

## <a name="get-sql-server-connection-information"></a>SQL Server-verbindingsgegevens ophalen

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="create-code-to-query-your-sql-database"></a>Code maken om query's uit te voeren op uw SQL-database

1. Maak in een tekst- of code-editor een nieuw bestand met de naam *sqltest.rb*.
   
1. Voeg de volgende code toe. Vervang de waarden van uw Azure SQL Database door `<server>`, `<database>`, `<username>` en `<password>`.
   
   >[!IMPORTANT]
   >Voor de code in dit voorbeeld worden de voorbeeldgegevens gebruikt van AdventureWorksLT, die u als bron kunt kiezen bij het maken van uw database. Als in uw database andere gegevens staan, kunt u tabellen uit uw eigen database gebruiken in de SELECT-query. 
   
   ```ruby
   require 'tiny_tds'
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'
   client = TinyTds::Client.new username: username, password: password, 
       host: server, port: 1433, database: database, azure: true
   
   puts "Reading data from table"
   tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
           FROM [SalesLT].[ProductCategory] pc
           JOIN [SalesLT].[Product] p
           ON pc.productcategoryid = p.productcategoryid"
   result = client.execute(tsql)
   result.each do |row|
       puts row
   end
   ```

## <a name="run-the-code"></a>De code uitvoeren

1. Voer de volgende opdracht uit op een opdrachtprompt:

   ```bash
   ruby sqltest.rb
   ```
   
1. Controleer of de bovenste twintig rijen voor Categorie/Product in uw database worden geretourneerd. 

## <a name="next-steps"></a>Volgende stappen
- [Uw eerste Azure SQL Database ontwerpen](sql-database-design-first-database.md).
- [GitHub-opslagplaats voor TinyTDS](https://github.com/rails-sqlserver/tiny_tds).
- [Problemen melden of vragen over TinyTDS stellen](https://github.com/rails-sqlserver/tiny_tds/issues).
- [Ruby-stuurprogramma voor SQL Server](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/).
