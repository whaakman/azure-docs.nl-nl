---
title: Java gebruiken om een query uit te voeren voor een Azure SQL-database | Microsoft Docs
description: Laat zien hoe u Java gebruikt om een programma te maken dat is verbonden met een Azure SQL-database, en hoe u deze database doorzoekt met behulp van T-SQL-instructies.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: java
ms.topic: quickstart
author: ajlam
ms.author: andrela
ms.reviewer: v-masebo
manager: craigg
ms.date: 11/20/2018
ms.openlocfilehash: afa975a593fd962050c9f894ec091d7f64579138
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52332609"
---
# <a name="quickstart-use-java-to-query-an-azure-sql-database"></a>Snelstart: Java gebruiken om een query uit te voeren voor een Azure SQL-database

In dit artikel ziet u hoe u [Java](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) gebruikt om verbinding te maken met een Azure SQL-database. Vervolgens kunt u T-SQL-instructies gebruiken om een query uit te voeren voor de gegevens.

## <a name="prerequisites"></a>Vereisten

Zorg dat u aan de volgende vereisten voldoet als u dit voorbeeld wilt uitvoeren:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Een [firewallregel op serverniveau](sql-database-get-started-portal-firewall.md) voor het openbare IP-adres van de computer die u gebruikt

- U moet aan Java verwante software hebben geïnstalleerd voor het besturingssysteem:

  - **MacOS**: installeer Homebrew en Java, en installeer vervolgens Maven. Zie [Stap 1.2 en 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).

  - **Ubuntu**: installeer Java, de Java Development Kit, en installeer vervolgens Maven. Zie [Stap 1.2, 1.3 en 1.4](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).

  - **Windows**: installeer Java, en installeer vervolgens Maven. Zie [Stap 1.2 en 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).

## <a name="get-database-connection"></a>Verbinding maken met de database

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="create-the-project"></a>Het project maken

1. Maak een nieuw Maven-project vanaf de terminal met de naam *sqltest*.

    ```bash
    mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0" --batch-mode
    ```

1. Wijzig de map naar *sqltest* en open *pom.xml* met uw favoriete teksteditor. Voeg het **Microsoft JDBC-stuurprogramma voor SQL Server** toe aan de afhankelijkheden van het project met behulp van de volgende code:

    ```xml
    <dependency>
        <groupId>com.microsoft.sqlserver</groupId>
        <artifactId>mssql-jdbc</artifactId>
        <version>7.0.0.jre8</version>
    </dependency>
    ```

1. Voeg, ook in *pom.xml*, de volgende eigenschappen toe aan uw project. Als u geen sectie met eigenschappen hebt, kunt u deze toevoegen na de afhankelijkheden.

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

1. Sla *pom.xml* op en sluit het af.

## <a name="add-code-to-query-database"></a>Code toevoegen om een query uit te voeren op de database

1. U hebt al een bestand met de naam *App.java* in uw Maven-project dat zich bevindt in:

   *..\sqltest\src\main\java\com\sqldbsamples\App.java*

1. Open het bestand en vervang de inhoud door de volgende code. Voeg vervolgens de juiste waarden toe voor uw server, database, gebruiker en wachtwoord.

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
            String url = String.format("jdbc:sqlserver://%s:1433;database=%s;user=%s;password=%s;encrypt=true;"
                + "hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
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

   > [!NOTE]
   > In het codevoorbeeld wordt de voorbeelddatabase **AdventureWorksLT** voor Azure SQL gebruikt.

## <a name="run-the-code"></a>De code uitvoeren

1. Voer het programma uit bij de opdrachtprompt.

    ```bash
    mvn package -DskipTests
    mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
    ```

1. Controleer de bovenste 20 rijen die worden geretourneerd en sluit het toepassingsvenster.

## <a name="next-steps"></a>Volgende stappen

- [Uw eerste Azure SQL-database ontwerpen](sql-database-design-first-database.md)  

- [Microsoft JDBC-stuurprogramma voor SQL Server](https://github.com/microsoft/mssql-jdbc)  

- [Problemen melden/vragen stellen](https://github.com/microsoft/mssql-jdbc/issues)  
