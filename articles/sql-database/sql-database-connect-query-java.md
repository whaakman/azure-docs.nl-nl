---
title: Verbinding maken met Azure SQL Database via Java | Microsoft Docs
description: Is een Java-codevoorbeeld dat u kunt gebruiken om verbinding te maken met en gegevens op te vragen uit Azure SQL Database.
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 05/23/2017
ms.author: andrela
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 0df7bd56d3aac462a86e31b5512e48371918bbf2
ms.contentlocale: nl-nl
ms.lasthandoff: 07/04/2017


---
<a id="azure-sql-database-use-java-to-connect-and-query-data" class="xliff"></a>

# Azure SQL Database: Java gebruiken om verbinding te maken en query's uit te voeren voor gegevens

In deze Quick Start ziet u hoe u [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) gebruikt om verbinding te maken met een Azure SQL-database en vervolgens Transact-SQL-instructies gebruikt om gegevens in de database te zoeken, in te voegen, bij te werken en te verwijderen vanaf het Mac OS-, Ubuntu Linux- en Windows-platform.

<a id="prerequisites" class="xliff"></a>

## Vereisten

In deze Quick Start wordt dit gebruikt als basis voor het maken van de resources die u hebt gemaakt in een van deze Quick Starts:

- [Database maken - Portal](sql-database-get-started-portal.md)
- [Database maken - CLI](sql-database-get-started-cli.md)
- [Database maken - PowerShell](sql-database-get-started-powershell.md)

<a id="install-java-software" class="xliff"></a>

## Java-software installeren

Voor de stappen in dit gedeelte wordt ervan uitgegaan dat u bekend bent met het ontwikkelen met Java, maar geen ervaring hebt met het werken met Azure SQL Database. Als u niet bekend bent met ontwikkelen met Java, gaat u naar [Een app bouwen met SQL Server](https://www.microsoft.com/en-us/sql-server/developer-get-started/) en selecteert u **Java**. Selecteer vervolgens uw besturingssysteem.

<a id="mac-os" class="xliff"></a>

### **Mac OS**
Open de terminal en navigeer naar een map waar u van plan bent een Java-project te maken. Installeer **brew** en **Maven** met de volgende opdrachten: 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

<a id="linux-ubuntu" class="xliff"></a>

### **Linux (Ubuntu)**
Open de terminal en navigeer naar een map waar u van plan bent een Java-project te maken. Installeer **Maven** met de volgende opdrachten:

```bash
sudo apt-get install maven
```

<a id="windows" class="xliff"></a>

### **Windows**
Installeer [Maven](https://maven.apache.org/download.cgi) met behulp van het officiële installatieprogramma. Gebruik Maven om afhankelijkheden te beheren en uw Java-project te bouwen, te testen en uit te voeren. 

<a id="get-connection-information" class="xliff"></a>

## Verbindingsgegevens ophalen

Haal de verbindingsgegevens op die nodig zijn om verbinding te maken met de Azure SQL-database. U hebt de volledig gekwalificeerde servernaam, databasenaam en aanmeldingsgegevens in de volgende procedures nodig.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **SQL-databases** in het menu links en klik op uw database op de pagina **SQL-databases**. 
3. Op de pagina **Overzicht** voor de database controleert u de volledig gekwalificeerde servernaam zoals in de onderstaande afbeelding wordt weergegeven. U kunt de cursor boven de servernaam houden om de optie **Klik om te kopiëren** naar boven te halen. 

   ![servernaam](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Als u de aanmeldingsgegevens voor uw server bent vergeten, gaat u naar de SQL Database-serverpagina om de beheerdersnaam voor de server weer te geven en, indien nodig, het wachtwoord opnieuw in te stellen.
5. Klik op **Databaseverbindingsreeksen tonen**.

6. Bekijk de volledige**JDBC**-verbindingsreeks.

    ![JDBC-verbindingsreeks](./media/sql-database-connect-query-jdbc/jdbc-connection-string.png)   

<a id="create-maven-project" class="xliff"></a>

### **Maven-project maken**
Maak een nieuw Maven-project vanaf de terminal. 
```bash
mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
```

Voeg het **Microsoft JDBC-stuurprogramma voor SQL Server** toe aan de afhankelijkheden in ***pom.xml***. 

```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.0.jre8</version>
</dependency>
```

<a id="select-data" class="xliff"></a>

## Gegevens selecteren

Gebruik de volgende code om op categorie een query uit te voeren voor de 20 populairste producten. Gebruik de [verbindings](https://docs.microsoft.com/sql/connect/jdbc/working-with-a-connection)klasse met de Transact-SQL-instructie [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql). Vervang de hostName-, dbName-, gebruiker- en wachtwoordparameters door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.Statement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Query data example:");
                System.out.println("=========================================");

                // Create and execute a SELECT SQL statement.
                String selectSql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName " 
                    + "FROM [SalesLT].[ProductCategory] pc "  
                    + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid";
                
                try (Statement statement = connection.createStatement();
                    ResultSet resultSet = statement.executeQuery(selectSql)) {

                        // Print results from select statement
                        System.out.println("Top 20 categories:");
                        while (resultSet.next())
                        {
                            System.out.println(resultSet.getString(1) + " "
                                + resultSet.getString(2));
                        }
                }
        }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```

<a id="insert-data" class="xliff"></a>

## Gegevens invoegen

Gebruik de volgende code om een nieuw product in te voegen in de tabel SalesLT.Product. Gebruik de [Voorbereide instructie](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql)klasse met de Transact-SQL-instructie [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql). Vervang de hostName-, dbName-, gebruiker- en wachtwoordparameters door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Insert data example:");
                System.out.println("=========================================");

                // Prepared statement to insert data
                String insertSql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, " 
                    + " StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

                java.util.Date date = new java.util.Date();
                java.sql.Timestamp sqlTimeStamp = new java.sql.Timestamp(date.getTime());

                try (PreparedStatement prep = connection.prepareStatement(insertSql)) {
                        prep.setString(1, "BrandNewProduct");
                        prep.setInt(2, 200989);
                        prep.setString(3, "Blue");
                        prep.setDouble(4, 75);
                        prep.setDouble(5, 80);
                        prep.setTimestamp(6, sqlTimeStamp);

                        int count = prep.executeUpdate();
                        System.out.println("Inserted: " + count + " row(s)");
                }
        }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```
<a id="update-data" class="xliff"></a>

## Gegevens bijwerken

Gebruik de volgende code om het nieuwe product bij te werken dat u eerder hebt toegevoegd. Gebruik de [Voorbereide instructie](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql)klasse met de Transact-SQL-instructie [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql) om gegevens in uw Azure SQL-database bij te werken. Vervang de hostName-, dbName-, gebruiker- en wachtwoordparameters door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Update data example:");
                System.out.println("=========================================");

                // Prepared statement to update data
                String updateSql = "UPDATE SalesLT.Product SET ListPrice = ? WHERE Name = ?";

                try (PreparedStatement prep = connection.prepareStatement(updateSql)) {
                        prep.setString(1, "500");
                        prep.setString(2, "BrandNewProduct");

                        int count = prep.executeUpdate();
                        System.out.println("Updated: " + count + " row(s)")
                }
        }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```



<a id="delete-data" class="xliff"></a>

## Gegevens verwijderen

Gebruik de volgende code om het nieuwe product te verwijderen dat u eerder hebt toegevoegd. Gebruik de [Voorbereide instructies](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql) met de Transact-SQL-instructie [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql). Vervang de hostName-, dbName-, gebruiker- en wachtwoordparameters door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Delete data example:");
                System.out.println("=========================================");

                // Prepared statement to delete data
                String deleteSql = "DELETE SalesLT.Product WHERE Name = ?";

                try (PreparedStatement prep = connection.prepareStatement(deleteSql)) {
                        prep.setString(1, "BrandNewProduct");

                        int count = prep.executeUpdate();
                        System.out.println("Deleted: " + count + " row(s)");
                }
        }       
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```

<a id="next-steps" class="xliff"></a>

## Volgende stappen
- [Uw eerste Azure SQL-database ontwerpen](sql-database-design-first-database.md)
- [Microsoft JDBC-stuurprogramma voor SQL Server](https://github.com/microsoft/mssql-jdbc)
- [Problemen melden/vragen stellen](https://github.com/microsoft/mssql-jdbc/issues)


