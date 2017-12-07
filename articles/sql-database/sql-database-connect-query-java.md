---
title: Java gebruiken om een query uit te voeren voor een Azure SQL-database | Microsoft Docs
description: In dit onderwerp ziet u hoe u Java gebruikt om een programma te maken dat is verbonden met een Azure SQL-database, en hoe u een query voor deze database uitvoert met behulp van Transact-SQL-instructies.
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: quickstart
ms.date: 07/11/2017
ms.author: andrela
ms.openlocfilehash: 994705b0a9c7ca850c357a5810f1edb1618098d6
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="use-java-to-query-an-azure-sql-database"></a>Java gebruiken om een query uit te voeren voor een Azure SQL-database

In deze Quick Start wordt gedemonstreerd hoe u [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) gebruikt om verbinding te maken met een Azure SQL-database, en hoe u vervolgens Transact-SQL-instructies gebruikt om een query uit te voeren voor gegevens.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u aan de volgende vereisten voldoet om deze beknopte zelfstudie uit te voeren:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Een [firewallregel op serverniveau](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) voor het openbare IP-adres van de computer die u gebruikt voor deze beknopte zelfstudie.

- U hebt Java en verwante software voor het besturingssysteem geïnstalleerd:

    - **Mac OS**: installeer Homebrew en Java, en installeer vervolgens Maven. Zie [Stap 1.2 en 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).
    - **Ubuntu**: installeer de Java Development Kit en Maven. Zie [Stap 1.2, 1.3 en 1.4](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).
    - **Windows**: installeer de Java Development Kit en Maven. Zie [Stap 1.2 en 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).    

## <a name="sql-server-connection-information"></a>SQL Server-verbindingsgegevens

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="create-maven-project-and-dependencies"></a>**Maven-project en -afhankelijkheden maken**
1. Maak een nieuw Maven-project vanaf de terminal met de naam **sqltest**. 

   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```

2. Voer **Y** in wanneer u daarom wordt gevraagd.
3. Wijzig de map naar **sqltest** en open ***pom.xml*** met uw favoriete teksteditor.  Voeg het **Microsoft JDBC-stuurprogramma voor SQL Server** toe aan de afhankelijkheden van uw project met de volgende code:

   ```xml
   <dependency>
       <groupId>com.microsoft.sqlserver</groupId>
       <artifactId>mssql-jdbc</artifactId>
       <version>6.2.1.jre8</version>
   </dependency>
   ```

4. Voeg, ook in ***pom.xml***, de volgende eigenschappen toe aan uw project.  Als u geen sectie met eigenschappen hebt, kunt u deze toevoegen na de afhankelijkheden.

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

5. Sla ***pom.xml*** op en sluit het af.

## <a name="insert-code-to-query-sql-database"></a>Code invoegen om een query uit te voeren voor een SQL-database

1. U hebt al een bestand met de naam ***App.java*** in uw Maven-project dat zich bevindt in:  ..\sqltest\src\main\java\com\sqlsamples\App.java

2. Open het bestand en vervang de inhoud ervan door de volgende code en voeg de juiste waarden toe voor de server, de database, de gebruiker en het wachtwoord.

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
           String hostName = "your_server.database.windows.net";
           String dbName = "your_database";
           String user = "your_username";
           String password = "your_password";
           String url = String.format("jdbc:sqlserver://%s:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
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
                    connection.close();
                   }                   
           }
           catch (Exception e) {
                   e.printStackTrace();
           }
       }
   }
   ```

## <a name="run-the-code"></a>De code uitvoeren

1. Voer bij de opdrachtprompt de volgende opdrachten uit:

   ```bash
   mvn package
   mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```

2. Controleer of de bovenste 20 rijen worden geretourneerd, en sluit vervolgens het toepassingsvenster.


## <a name="next-steps"></a>Volgende stappen
- [Uw eerste Azure SQL-database ontwerpen](sql-database-design-first-database.md)
- [Microsoft JDBC-stuurprogramma voor SQL Server](https://github.com/microsoft/mssql-jdbc)
- [Problemen melden/vragen stellen](https://github.com/microsoft/mssql-jdbc/issues)

