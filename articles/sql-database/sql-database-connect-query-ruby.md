---
title: Verbinding maken met Azure SQL Database via Ruby | Microsoft Docs
description: "Is een Ruby-codevoorbeeld dat u kunt gebruiken om verbinding te maken met en query’s uit te voeren in Azure SQL Database."
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 94fec528-58ba-4352-ba0d-25ae4b273e90
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: hero-article
ms.date: 05/24/2017
ms.author: andrela
ms.translationtype: Human Translation
ms.sourcegitcommit: 6adaf7026d455210db4d7ce6e7111d13c2b75374
ms.openlocfilehash: c5d09cf03c87c8da1d8588be62fea3f0cc3eec4f
ms.contentlocale: nl-nl
ms.lasthandoff: 06/22/2017

---

<a id="azure-sql-database-use-ruby-to-connect-and-query-data" class="xliff"></a>

# Azure SQL Database: Ruby gebruiken om verbinding te maken en query's uit te voeren op gegevens

In deze Quick Start ziet u hoe u [Ruby](https://www.ruby-lang.org) gebruikt om verbinding te maken met een Azure SQL-database en vervolgens Transact-SQL-instructies gebruikt om gegevens in de database te zoeken, in te voegen, bij te werken en te verwijderen vanaf Mac OS- en Ubuntu Linux-platforms.

<a id="prerequisites" class="xliff"></a>

## Vereisten

In deze Quick Start wordt dit gebruikt als basis voor het maken van de resources die u hebt gemaakt in een van deze Quick Starts:

- [Database maken - Portal](sql-database-get-started-portal.md)
- [Database maken - CLI](sql-database-get-started-cli.md)
- [Database maken - PowerShell](sql-database-get-started-powershell.md)

<a id="install-ruby-and-database-communication-libraries" class="xliff"></a>

## Ruby en databasecommunicatiebibliotheken installeren

Voor de stappen in dit gedeelte wordt ervan uitgegaan dat u bekend bent met het ontwikkelen met Ruby, maar geen ervaring hebt met het werken met Azure SQL Database. Als u niet bekend bent met ontwikkelen met Ruby, gaat u naar [Een app bouwen met SQL Server](https://www.microsoft.com/en-us/sql-server/developer-get-started/) en selecteert u **Ruby**. Selecteer vervolgens uw besturingssysteem.

<a id="mac-os" class="xliff"></a>

### **Mac OS**
Open de terminal en ga naar een map waarin u het Ruby-script wilt maken. Voer de volgende opdrachten in voor het installeren van **Brew**, **FreeTDS** en **TinyTDS**.

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/msodbcsql https://github.com/Microsoft/homebrew-msodbcsql-preview
brew update
brew install FreeTDS
gem install tiny_tds
```

<a id="linux-ubuntu" class="xliff"></a>

### **Linux (Ubuntu)**
Open de terminal en ga naar een map waarin u het Ruby-script wilt maken. Voer de volgende opdrachten in voor het installeren van **FreeTDS** en **TinyTDS**.

```bash
wget ftp://ftp.freetds.org/pub/freetds/stable/freetds-1.00.27.tar.gz
tar -xzf freetds-1.00.27.tar.gz
cd freetds-1.00.27
./configure --prefix=/usr/local --with-tdsver=7.3
make
make install
gem install tiny_tds
```

<a id="get-connection-information" class="xliff"></a>

## Verbindingsgegevens ophalen

Haal de verbindingsgegevens op die nodig zijn om verbinding te maken met de Azure SQL-database. U hebt de volledig gekwalificeerde servernaam, databasenaam en aanmeldingsgegevens in de volgende procedures nodig.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **SQL-databases** in het menu links en klik op uw database op de pagina **SQL-databases**. 
3. Op de pagina **Overzicht** voor de database controleert u de volledig gekwalificeerde servernaam zoals in de onderstaande afbeelding wordt weergegeven. U kunt de cursor boven de servernaam houden om de optie **Klik om te kopiëren** naar boven te halen. 

   ![servernaam](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Als u de aanmeldingsgegevens voor de server bent vergeten, gaat u naar de SQL Database-serverpagina om de beheerdersnaam voor de server weer te geven en, indien nodig, het wachtwoord opnieuw in te stellen.
    

<a id="select-data" class="xliff"></a>

## Gegevens selecteren
Gebruik de volgende code om op categorie een query uit te voeren voor de 20 populairste producten. Gebruik de functie [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) met de Transact-SQL-instructie [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql). Met de functie TinyTDS::Client wordt een query geaccepteerd en een resultatenset geretourneerd. De resultatenset wordt herhaald met [result.each doen | rij |](https://github.com/rails-sqlserver/tiny_tds). Vervang de parameters voor server, database, gebruikersnaam en wachtwoord door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens.

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

<a id="insert-data" class="xliff"></a>

## Gegevens invoegen
Gebruik de volgende code om een nieuw product in te voegen in de tabel SalesLT.Product. Gebruik de functie [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) met de Transact-SQL-instructie [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql). Vervang de parameters voor server, database, gebruikersnaam en wachtwoord door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens.

In dit voorbeeld ziet u hoe u veilig een INSERT-instructie kunt uitvoeren, parameters kunt doorgeven waarmee de toepassing wordt beschermd tegen beveiligingsproblemen met [SQL-injectie](https://technet.microsoft.com/library/ms161953(v=sql.105).aspx) en de automatisch gegenereerde waarde van [Primaire sleutel](https://docs.microsoft.com/sql/relational-databases/tables/primary-and-foreign-key-constraints) kunt ophalen.    
  
Als u TinyTDS wilt gebruiken met Azure, is het raadzaam verschillende `SET`-instructies uit te voeren om te wijzigen hoe de huidige sessie specifieke informatie verwerkt. Aanbevolen `SET`-instructies vindt u in het codevoorbeeld. Met `SET ANSI_NULL_DFLT_ON` kunnen er bijvoorbeeld voor nieuwe kolommen null-waarden worden toegestaan, zelfs als de status voor het toestaan van null-waarden van de kolom niet expliciet is opgegeven.  
  
Als u de [datum-tijd](https://docs.microsoft.com/sql/t-sql/data-types/datetime-transact-sql)notatie van Microsoft SQL Server wilt gebruiken, kunt u met de functie [strftime](http://ruby-doc.org/core-2.2.0/Time.html#method-i-strftime) converteren naar de bijbehorende datum-tijdnotatie.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

# settings for Azure
result = client.execute("SET ANSI_NULLS ON")
result = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")
result = client.execute("SET ANSI_NULL_DFLT_ON ON")
result = client.execute("SET IMPLICIT_TRANSACTIONS OFF")
result = client.execute("SET ANSI_PADDING ON")
result = client.execute("SET QUOTED_IDENTIFIER ON")
result = client.execute("SET ANSI_WARNINGS ON")
result = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")

def insert(name, productnumber, color, standardcost, listprice, sellstartdate)
    tsql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) 
        VALUES (N'#{name}', N'#{productnumber}',N'#{color}',N'#{standardcost}',N'#{listprice}',N'#{sellstartdate}')"
    result = client.execute(tsql)
    result.each
    puts "#{result.affected_rows} row(s) affected"
end
insert('BrandNewProduct', '200989', 'Blue', 75, 80, '7/1/2016')
```

<a id="update-data" class="xliff"></a>

## Gegevens bijwerken
Gebruik de volgende code om het nieuwe product bij te werken dat u eerder hebt toegevoegd. Gebruik de functie [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) met de Transact-SQL-instructie [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql). Vervang de parameters voor server, database, gebruikersnaam en wachtwoord door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true
    
def update(name, listPrice, client)
    tsql = "UPDATE SalesLT.Product SET ListPrice = N'#{listPrice}' WHERE Name =N'#{name}'";
    result = client.execute(tsql)
    result.each
    puts "#{result.affected_rows} row(s) affected"
end
update('BrandNewProduct', 500, client)
```

<a id="delete-data" class="xliff"></a>

## Gegevens verwijderen
Gebruik de volgende code om het nieuwe product te verwijderen dat u eerder hebt toegevoegd. Gebruik de functie [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) met de Transact-SQL-instructie [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql). Vervang de parameters voor server, database, gebruikersnaam en wachtwoord door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

# settings for Azure
result = client.execute("SET ANSI_NULLS ON")
result = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")
result = client.execute("SET ANSI_NULL_DFLT_ON ON")
result = client.execute("SET IMPLICIT_TRANSACTIONS OFF")
result = client.execute("SET ANSI_PADDING ON")
result = client.execute("SET QUOTED_IDENTIFIER ON")
result = client.execute("SET ANSI_WARNINGS ON")
result = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")

def delete(name, client)
    tsql = "DELETE FROM SalesLT.Product WHERE Name = N'#{name}'"
    result = client.execute(tsql)
    result.each
    puts "#{result.affected_rows} row(s) affected"
end
delete('BrandNewProduct', client)
```

<a id="next-steps" class="xliff"></a>

## Volgende stappen
- [Uw eerste Azure SQL-database ontwerpen](sql-database-design-first-database.md)
- [GitHub-opslagplaats voor TinyTDS](https://github.com/rails-sqlserver/tiny_tds)
- [Problemen melden/vragen stellen](https://github.com/rails-sqlserver/tiny_tds/issues)
- [Ruby-stuurprogramma's voor SQL Server](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)


