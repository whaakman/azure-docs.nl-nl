---
title: Verbinding maken met Azure SQL Database via Python | Microsoft Docs
description: "Dit is een Python-codevoorbeeld dat u kunt gebruiken om verbinding te maken met en query’s uit te voeren in Azure SQL Database."
services: sql-database
documentationcenter: 
author: meet-bhagdev
manager: jhubbard
editor: 
ms.assetid: 452ad236-7a15-4f19-8ea7-df528052a3ad
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 05/24/2017
ms.author: meetb
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 99195b43a1577f978562864bac5fa12cdeb95d63
ms.contentlocale: nl-nl
ms.lasthandoff: 06/17/2017

---
<a id="azure-sql-database-use-python-to-connect-and-query-data" class="xliff"></a>

# Azure SQL Database: Python gebruiken om verbinding te maken en query's uit te voeren voor gegevens

 In deze Quick Start ziet u hoe u [Python](https://python.org) gebruikt om verbinding te maken met een Azure SQL-database en vervolgens Transact-SQL-instructies gebruikt om gegevens in de database te zoeken, in te voegen, bij te werken en te verwijderen vanaf het Mac OS-, Ubuntu Linux- en Windows-platform.

<a id="prerequisites" class="xliff"></a>

## Vereisten

In deze Quick Start wordt dit gebruikt als basis voor het maken van de resources die u hebt gemaakt in een van deze Quick Starts:

- [Database maken - Portal](sql-database-get-started-portal.md)
- [Database maken - CLI](sql-database-get-started-cli.md)
- [Database maken - PowerShell](sql-database-get-started-powershell.md)

<a id="install-the-python-and-database-communication-libraries" class="xliff"></a>

## De Python- en databasecommunicatiebibliotheken installeren

Voor de stappen in dit gedeelte wordt ervan uitgegaan dat u bekend bent met het ontwikkelen met Python, maar geen ervaring hebt met het werken met Azure SQL Database. Als u niet bekend bent met ontwikkelen met Python, gaat u naar [Een app bouwen met SQL Server](https://www.microsoft.com/sql-server/developer-get-started/) en selecteert u **Python**. Selecteer vervolgens uw besturingssysteem.

<a id="mac-os" class="xliff"></a>

### **Mac OS**
Open de terminal en ga naar een map waarin u het Python-script wilt maken. Voer de volgende opdrachten in om **brew**, **Microsoft ODBC-stuurprogramma voor Mac** en **pyodbc** te installeren. Pyodbc maakt gebruik van het Microsoft ODBC-stuurprogramma op Linux om verbinding te maken met SQL-databases.

``` bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/msodbcsql https://github.com/Microsoft/homebrew-msodbcsql-preview
brew update
brew install msodbcsql 
#for silent install ACCEPT_EULA=y brew install msodbcsql
sudo pip install pyodbc==3.1.1
```

<a id="linux-ubuntu" class="xliff"></a>

### **Linux (Ubuntu)**
Open de terminal en ga naar een map waarin u het Python-script wilt maken. Voer de volgende opdrachten in om het **Microsoft ODBC-stuurprogramma voor Linux** en **pyodbc** te installeren. Pyodbc maakt gebruik van het Microsoft ODBC-stuurprogramma op Linux om verbinding te maken met SQL-databases.

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql.list
exit
sudo apt-get update
sudo apt-get install msodbcsql mssql-tools unixodbc-dev
sudo pip install pyodbc==3.1.1
```

<a id="windows" class="xliff"></a>

### **Windows**
Installeer het [Microsoft ODBC-stuurprogramma 13.1](https://www.microsoft.com/download/details.aspx?id=53339) (voer zo nodig een upgrade uit op het stuurprogramma). Pyodbc maakt gebruik van het Microsoft ODBC-stuurprogramma op Linux om verbinding te maken met SQL-databases. 

Installeer **pyodbc** via pip.

```cmd
pip install pyodbc==3.1.1
```

Instructies voor het inschakelen van de use-pip vindt u [hier](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows)

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

Gebruik de volgende code om op categorie een query uit te voeren voor de 20 populairste producten. Gebruik de functie [pyodbc.connect](https://github.com/mkleehammer/pyodbc/wiki) met de Transact-SQL-instructie [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql). De functie [cursor.execute](https://github.com/mkleehammer/pyodbc/wiki/Cursor) wordt gebruikt om een resultatenset voor een query op te halen uit een SQL-database. Deze functie accepteert een query en retourneert een resultatenset die met behulp van **cursor.fetchone()** kan worden herhaald. Vervang de parameters voor server, database, gebruikersnaam en wachtwoord door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print str(row[0]) + " " + str(row[1])
    row = cursor.fetchone()
```

<a id="insert-data" class="xliff"></a>

## Gegevens invoegen
Gebruik de volgende code om een nieuw product in te voegen in de tabel SalesLT.Product. Gebruik hiervoor de functie [cursor.execute](https://github.com/mkleehammer/pyodbc/wiki/Cursor) met de Transact-SQL-instructie [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql). Vervang de parameters voor server, database, gebruikersnaam en wachtwoord door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
with cursor.execute("INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('BrandNewProduct', '200989', 'Blue', 75, 80, '7/1/2016')"): 
    print ('Successfuly Inserted!')
cnxn.commit()
```

<a id="update-data" class="xliff"></a>

## Gegevens bijwerken
Gebruik de volgende code om het nieuwe product bij te werken dat u eerder hebt toegevoegd. Gebruik de functie [cursor.execute](https://github.com/mkleehammer/pyodbc/wiki/Cursor) met de Transact-SQL-instructie [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql). Vervang de parameters voor server, database, gebruikersnaam en wachtwoord door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
tsql = "UPDATE SalesLT.Product SET ListPrice = ? WHERE Name = ?"
with cursor.execute(tsql,50,'BrandNewProduct'):
    print ('Successfuly Updated!')
cnxn.commit()

```

<a id="delete-data" class="xliff"></a>

## Gegevens verwijderen
Gebruik de volgende code om het nieuwe product te verwijderen dat u eerder hebt toegevoegd. Gebruik de functie [cursor.execute](https://github.com/mkleehammer/pyodbc/wiki/Cursor) met de Transact-SQL-instructie [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql). Vervang de parameters voor server, database, gebruikersnaam en wachtwoord door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
tsql = "DELETE FROM SalesLT.Product WHERE Name = ?"
with cursor.execute(tsql,'BrandNewProduct'):
    print ('Successfuly Deleted!')
cnxn.commit()
```

<a id="next-steps" class="xliff"></a>

## Volgende stappen

- [Uw eerste Azure SQL-database ontwerpen](sql-database-design-first-database.md)
- [Microsoft Python-stuurprogramma's voor SQL Server](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Python Developer Center](https://azure.microsoft.com/develop/python/?v=17.23h)


