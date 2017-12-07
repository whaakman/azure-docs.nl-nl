---
title: Ruby gebruiken om een query uit te voeren voor een Azure SQL-database | Microsoft Docs
description: In dit onderwerp ziet u hoe u Ruby gebruikt om een programma te maken dat is verbonden met een Azure SQL-database, en hoe u een query voor deze database uitvoert met behulp van Transact-SQL-instructies.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 94fec528-58ba-4352-ba0d-25ae4b273e90
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: quickstart
ms.date: 07/15/2017
ms.author: carlrab
ms.openlocfilehash: 3427d216540451bc10b968f866d0fce0f6df3c54
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="use-ruby-to-query-an-azure-sql-database"></a>Ruby gebruiken om een query uit te voeren voor een Azure SQL-database

In deze beknopte zelfstudie wordt gedemonstreerd hoe u [Ruby](https://www.ruby-lang.org) gebruikt om een programma te maken dat verbinding maakt met een Azure SQL-database, en hoe u Transact-SQL-instructies gebruikt om een query uit te voeren voor gegevens.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u aan de volgende vereisten voldoet om deze beknopte zelfstudie uit te voeren:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Een [firewallregel op serverniveau](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) voor het openbare IP-adres van de computer die u gebruikt voor deze beknopte zelfstudie.

- U hebt Ruby en verwante software voor uw besturingssysteem geïnstalleerd:
    - **Mac OS**: installeer Homebrew, installeer rbenv en ruby-build, installeer Ruby en installeer vervolgens FreeTDS. Zie [Stap 1.2, 1.3, 1.4 en 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).
    - **Ubuntu**: installeer de vereisten voor Ruby, installeer rbenv en ruby-build, installeer Ruby en installeer vervolgens FreeTDS. Zie [Stap 1.2, 1.3, 1.4 en 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).

## <a name="sql-server-connection-information"></a>SQL Server-verbindingsgegevens

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

> [!IMPORTANT]
> U moet een firewallregel hebben ingesteld voor het openbare IP-adres van de computer waarop u deze zelfstudie uitvoert. Als u een andere computer gebruikt of een ander openbaar IP-adres hebt, maakt u een [firewallregel op serverniveau met behulp van Azure Portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 

## <a name="insert-code-to-query-sql-database"></a>Code invoegen om een query uit te voeren voor een SQL-database

1. Maak een nieuw bestand in uw favoriete teksteditor **sqltest.rb**

2. Vervang de inhoud door de volgende code en voeg de juiste waarden toe voor de server, de database, de gebruiker en het wachtwoord.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
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

1. Voer bij de opdrachtprompt de volgende opdrachten uit:

   ```bash
   ruby sqltest.rb
   ```

2. Controleer of de bovenste 20 rijen worden geretourneerd, en sluit vervolgens het toepassingsvenster.


## <a name="next-steps"></a>Volgende stappen
- [Uw eerste Azure SQL-database ontwerpen](sql-database-design-first-database.md)
- [GitHub-opslagplaats voor TinyTDS](https://github.com/rails-sqlserver/tiny_tds)
- [Problemen melden of vragen over TinyTDS stellen](https://github.com/rails-sqlserver/tiny_tds/issues)
- [Ruby-stuurprogramma's voor SQL Server](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)
