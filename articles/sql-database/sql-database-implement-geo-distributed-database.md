---
title: Een geografisch gedistribueerde Azure SQL database-oplossing implementeren | Microsoft Docs
description: Informatie over het configureren van uw Azure SQL-database en de toepassing voor failover naar een gerepliceerde database, en failover testen.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 6022c016b83ffe1362db4d826a5ee4397afd4128
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57844140"
---
# <a name="tutorial-implement-a-geo-distributed-database"></a>Zelfstudie: Een geografisch gedistribueerde database implementeren

Een Azure SQL-database en -toepassing voor failover naar een externe regio configureren en testen van een failover-plan. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> - Maak een [failovergroep](sql-database-auto-failover-group.md)
> - Een Java-toepassing in een Azure SQL database query uitvoeren
> - Testfailover

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> De PowerShell Azure Resource Manager-module nog steeds wordt ondersteund door Azure SQL Database, maar alle toekomstige ontwikkeling is voor de module Az.Sql. Zie voor deze cmdlets [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). De argumenten voor de opdrachten in de Az-module en de AzureRm-modules zijn vrijwel identiek zijn.

Als u wilt de zelfstudie hebt voltooid, moet dat u de volgende items hebt geïnstalleerd:

- [Azure PowerShell](/powershell/azureps-cmdlets-docs)
- Een Azure SQL-database. Voor het maken een gebruiken
  - [Portal](sql-database-single-database-get-started.md)
  - [CLI](sql-database-cli-samples.md)
  - [PowerShell](sql-database-powershell-samples.md)

  > [!NOTE]
  > De zelfstudie wordt gebruikgemaakt van de *AdventureWorksLT* voorbeelddatabase.

- Java en Maven, Zie [een app bouwen met SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), markeer **Java** , selecteer uw omgeving en vervolgens de volgende stappen.

> [!IMPORTANT]
> Zorg ervoor dat firewall-regels ingesteld voor het gebruik van het openbare IP-adres van de computer waarop het uitvoeren van de stappen in deze zelfstudie. Database-level firewall-regels worden automatisch gerepliceerd naar de secundaire server.
>
> Zie voor meer informatie [een firewallregel op databaseniveau maken](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) of om te bepalen het IP-adres dat is gebruikt voor de firewallregel op serverniveau voor uw computer naar [maken van een firewall op serverniveau](sql-database-server-level-firewall-rule.md).  

## <a name="create-a-failover-group"></a>Maak een failovergroep

Met behulp van Azure PowerShell, [failovergroepen](sql-database-auto-failover-group.md) tussen een bestaande Azure SQL-server en een nieuwe Azure SQL-server in een andere regio. De database vervolgens toevoegen aan de failovergroep.

> [!IMPORTANT]
> [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]

Voer het volgende script voor het maken van een failovergroep:

   ```powershell
    # Set variables for your server and database
    $adminlogin = "<your admin>"
    $password = "<your password>"
    $myresourcegroupname = "<your resource group name>"
    $mylocation = "<your resource group location>"
    $myservername = "<your existing server name>"
    $mydatabasename = "<your database name>"
    $mydrlocation = "<your disaster recovery location>"
    $mydrservername = "<your disaster recovery server name>"
    $myfailovergroupname = "<your globally unique failover group name>"

    # Create a backup server in the failover region
    New-AzSqlServer -ResourceGroupName $myresourcegroupname `
       -ServerName $mydrservername `
       -Location $mydrlocation `
       -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
          -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

    # Create a failover group between the servers
    New-AzSqlDatabaseFailoverGroup `
       –ResourceGroupName $myresourcegroupname `
       -ServerName $myservername `
       -PartnerServerName $mydrservername  `
       –FailoverGroupName $myfailovergroupname `
       –FailoverPolicy Automatic `
       -GracePeriodWithDataLossHours 2

    # Add the database to the failover group
    Get-AzSqlDatabase `
       -ResourceGroupName $myresourcegroupname `
       -ServerName $myservername `
       -DatabaseName $mydatabasename | `
     Add-AzSqlDatabaseToFailoverGroup `
       -ResourceGroupName $myresourcegroupname `
       -ServerName $myservername `
       -FailoverGroupName $myfailovergroupname
   ```

Geo-replicatie-instellingen ook in de Azure-portal kunnen worden gewijzigd door het selecteren van de database, en vervolgens **instellingen** > **Geo-replicatie**.

![Geo-replicatie-instellingen](./media/sql-database-implement-geo-distributed-database/geo-replication.png)

## <a name="run-the-sample-project"></a>Het voorbeeldproject uitvoeren

1. Maak een Maven-project met de volgende opdracht in de console:

   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```

1. Type **Y** en druk op **Enter**.

1. Wijzig de mappen in het nieuwe project.

   ```bash
   cd SqlDbSample
   ```

1. Open uw favoriete editor met de *pom.xml* bestand in de projectmap.

1. De Microsoft JDBC-stuurprogramma voor SQL Server-afhankelijkheid toevoegen door toe te voegen van de volgende `dependency` sectie. De afhankelijkheid moet worden geplakt in de grotere `dependencies` sectie.

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

1. De Java-versie opgeven door toe te voegen de `properties` sectie na de `dependencies` sectie:

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

1. Ondersteuning van manifestbestanden door toe te voegen de `build` sectie na de `properties` sectie:

   ```xml
   <build>
     <plugins>
       <plugin>
         <groupId>org.apache.maven.plugins</groupId>
         <artifactId>maven-jar-plugin</artifactId>
         <version>3.0.0</version>
         <configuration>
           <archive>
             <manifest>
               <mainClass>com.sqldbsamples.App</mainClass>
             </manifest>
           </archive>
        </configuration>
       </plugin>
     </plugins>
   </build>
   ```

1. Opslaan en sluiten de *pom.xml* bestand.

1. Open de *App.java* bestand zich bevindt in... \SqlDbSample\src\main\java\com\sqldbsamples en vervang de inhoud door de volgende code:

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.Timestamp;
   import java.sql.DriverManager;
   import java.util.Date;
   import java.util.concurrent.TimeUnit;

   public class App {

      private static final String FAILOVER_GROUP_NAME = "<your failover group name>";  // add failover group name
  
      private static final String DB_NAME = "<your database>";  // add database name
      private static final String USER = "<your admin>";  // add database user
      private static final String PASSWORD = "<your password>";  // add database password

      private static final String READ_WRITE_URL = String.format("jdbc:" +
         "sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;" +
         "hostNameInCertificate=*.database.windows.net;loginTimeout=30;", +
         FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);
      private static final String READ_ONLY_URL = String.format("jdbc:" +
         "sqlserver://%s.secondary.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;" +
         "hostNameInCertificate=*.database.windows.net;loginTimeout=30;", +
         FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);

      public static void main(String[] args) {
         System.out.println("#######################################");
         System.out.println("## GEO DISTRIBUTED DATABASE TUTORIAL ##");
         System.out.println("#######################################");
         System.out.println("");

         int highWaterMark = getHighWaterMarkId();

         try {
            for(int i = 1; i < 1000; i++) {
                //  loop will run for about 1 hour
                System.out.print(i + ": insert on primary " +
                   (insertData((highWaterMark + i))?"successful":"failed"));
                TimeUnit.SECONDS.sleep(1);
                System.out.print(", read from secondary " +
                   (selectData((highWaterMark + i))?"successful":"failed") + "\n");
                TimeUnit.SECONDS.sleep(3);
            }
         } catch(Exception e) {
            e.printStackTrace();
      }
   }

   private static boolean insertData(int id) {
      // Insert data into the product table with a unique product name so we can find the product again
      String sql = "INSERT INTO SalesLT.Product " +
         "(Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL);
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         pstmt.setInt(2, 200989 + id + 10000);
         pstmt.setString(3, "Blue");
         pstmt.setDouble(4, 75.00);
         pstmt.setDouble(5, 89.99);
         pstmt.setTimestamp(6, new Timestamp(new Date().getTime()));
         return (1 == pstmt.executeUpdate());
      } catch (Exception e) {
         return false;
      }
   }

   private static boolean selectData(int id) {
      // Query the data previously inserted into the primary database from the geo replicated database
      String sql = "SELECT Name, Color, ListPrice FROM SalesLT.Product WHERE Name = ?";

      try (Connection connection = DriverManager.getConnection(READ_ONLY_URL);
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         try (ResultSet resultSet = pstmt.executeQuery()) {
            return resultSet.next();
         }
      } catch (Exception e) {
         return false;
      }
   }

   private static int getHighWaterMarkId() {
      // Query the high water mark id stored in the table to be able to make unique inserts
      String sql = "SELECT MAX(ProductId) FROM SalesLT.Product";
      int result = 1;
      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL);
              Statement stmt = connection.createStatement();
              ResultSet resultSet = stmt.executeQuery(sql)) {
         if (resultSet.next()) {
             result =  resultSet.getInt(1);
            }
         } catch (Exception e) {
          e.printStackTrace();
         }
         return result;
      }
   }
   ```

1. Opslaan en sluiten de *App.java* bestand.

1. Voer in de opdrachtconsole de volgende opdracht:

   ```bash
   mvn package
   ```

1. Start de toepassing die wordt uitgevoerd bij ongeveer 1 uur totdat deze handmatig wordt gestopt, zodat u tijd de failovertest wilt uitvoeren.

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```

   ```output
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ...
   ```

## <a name="test-failover"></a>Testfailover

Voer de volgende scripts voor het simuleren van een failover en bekijk de toepassingsresultaten. U ziet hoe sommige ingevoegd en selecteert u mislukken tijdens de migratie van de database.

U kunt ook de rol van de server voor noodherstelanalyse controleren tijdens de test met de volgende opdracht:

   ```powershell
   (Get-AzSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername).ReplicationRole
   ```

Een failover testen:

1. Start een handmatige failover van de failovergroep:

   ```powershell
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -FailoverGroupName $myfailovergroupname
   ```

1. Failovergroep naar de primaire server herstellen:

   ```powershell
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -FailoverGroupName $myfailovergroupname
   ```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie die u kunt een Azure SQL-database en -toepassing voor failover naar een externe regio geconfigureerd en getest een failover-plan. U hebt geleerd hoe u:

> [!div class="checklist"]
> - Een failover-groep met geo-replicatie maken
> - Een Java-toepassing in een Azure SQL database query uitvoeren
> - Testfailover

Ga naar de volgende zelfstudie voor het migreren met behulp van DMS.

> [!div class="nextstepaction"]
> [SQL Server migreren naar Azure SQL-database beheerd exemplaar met behulp van DMS](../dms/tutorial-sql-server-to-managed-instance.md)
