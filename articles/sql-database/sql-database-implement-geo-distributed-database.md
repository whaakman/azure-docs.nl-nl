---
title: Een Azure SQL Database geografisch verspreide oplossing implementeren | Microsoft Docs
description: Informatie over het configureren van uw Azure SQL Database en de toepassing voor failover voor een gerepliceerde database en failover testen.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,business continuity
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 05/26/2017
ms.author: carlrab
ms.openlocfilehash: 910be8ff5f9a882c7bb8ae875b8bf5fc74d1fb9a
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="implement-a-geo-distributed-database"></a>Een database geografisch verspreide implementeren

In deze zelfstudie kunt u configureren van een Azure SQL database en de toepassing voor een failover naar een externe regio en test u uw plan failover. Procedures voor: 

> [!div class="checklist"]
> * Maken van databasegebruikers en machtigingen verlenen
> * Een firewallregel op databaseniveau instellen
> * Maak een [geo-replicatie failover-groep](sql-database-geo-replication-overview.md)
> * Maken en het compileren van een Java-toepassing query uitvoeren op een Azure SQL database
> * Uitvoeren van een herstel na noodgevallen detailanalyse

Als u een Azure-abonnement geen [een gratis account maken](https://azure.microsoft.com/free/) voordat u begint.


## <a name="prerequisites"></a>Vereisten

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

- De meest recente geïnstalleerd [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs). 
- Een Azure SQL database geïnstalleerd. In deze zelfstudie wordt de voorbeelddatabase van AdventureWorksLT met de naam **mySampleDatabase** van een van deze snel aan de slag:

   - [Database maken - Portal](sql-database-get-started-portal.md)
   - [Database maken - CLI](sql-database-get-started-cli.md)
   - [Database maken - PowerShell](sql-database-get-started-powershell.md)

- Een methode voor het uitvoeren van SQL-scripts uitvoeren op uw database hebt geïdentificeerd kunt u een van de volgende query's:
   - De query-editor in de [Azure-portal](https://portal.azure.com). Zie voor meer informatie over het gebruik van de query-editor in Azure portal [Connect en query Query-Editor met](sql-database-get-started-portal.md#query-the-sql-database).
   - De nieuwste versie van [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), dit is een geïntegreerde omgeving voor het beheren van elke SQL-infrastructuur uit SQL Server met SQL-Database voor Microsoft Windows.
   - De nieuwste versie van [Visual Studio Code](https://code.visualstudio.com/docs), die een grafische code-editor voor Linux, Mac OS, en wanneer die extensies, met inbegrip van Windows ondersteunt de [mssql-extensie](https://aka.ms/mssql-marketplace) voor query's in Microsoft SQL Server Azure SQL Database en SQL datawarehouse. Zie voor meer informatie over het gebruik van dit hulpprogramma met Azure SQL Database [Connect en query met de Code van de VS](sql-database-connect-query-vscode.md). 

## <a name="create-database-users-and-grant-permissions"></a>Maken van databasegebruikers en machtigingen verlenen

Verbinding maken met uw database en Maak gebruikersaccounts met behulp van een van de volgende query's:

- De Query-editor in Azure portal
- SQL Server Management Studio
- Visual Studio Code

Deze gebruikersaccounts automatisch worden gerepliceerd naar de secundaire server (en gesynchroniseerd blijven). Voor het gebruik van SQL Server Management Studio of Visual Studio Code, moet u wellicht een firewallregel configureren als u verbinding maakt vanaf een client op een IP-adres waarvoor u nog niet hebt geconfigureerd een firewall. Zie voor gedetailleerde stappen [maken van een firewallregel op serverniveau](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).

- Uitvoeren in een query-venster de volgende query voor het maken van twee gebruikersaccounts in uw database. Dit script verleent **db_owner** machtigingen voor de **app_admin** -account en verleent **Selecteer** en **UPDATE** machtigingen voor de **app_user** account. 

   ```sql
   CREATE USER app_admin WITH PASSWORD = 'ChangeYourPassword1';
   --Add SQL user to db_owner role
   ALTER ROLE db_owner ADD MEMBER app_admin; 
   --Create additional SQL user
   CREATE USER app_user WITH PASSWORD = 'ChangeYourPassword1';
   --grant permission to SalesLT schema
   GRANT SELECT, INSERT, DELETE, UPDATE ON SalesLT.Product TO app_user;
   ```

## <a name="create-database-level-firewall"></a>Firewallregel op databaseniveau maken

Maak een [firewallregel op databaseniveau](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) voor uw SQL-database. Deze firewallregel op databaseniveau repliceert automatisch naar de secundaire server die u in deze zelfstudie maakt. Gebruik het openbare IP-adres van de computer waarop u de stappen in deze zelfstudie zijn uitvoert voor eenvoud (in deze zelfstudie). Om te bepalen van de IP-adres voor de firewallregel op serverniveau voor de huidige computer, Zie [maken van een firewall serverniveau](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).  

- In het venster openen query, kunt u de vorige query vervangen door de volgende query, het IP-adressen vervangen door de juiste IP-adressen voor uw omgeving.  

   ```sql
   -- Create database-level firewall setting for your public IP address
   EXECUTE sp_set_database_firewall_rule @name = N'myGeoReplicationFirewallRule',@start_ip_address = '0.0.0.0', @end_ip_address = '0.0.0.0';
   ```

## <a name="create-an-active-geo-replication-auto-failover-group"></a>Een actieve geo-replicatie automatische failover-groep maken 

Met Azure PowerShell maken een [actieve geo-replicatie automatische failover groep](sql-database-geo-replication-overview.md) tussen de bestaande Azure SQL-server en de nieuwe Azure SQL-server in een Azure-regio leeg en voeg vervolgens de voorbeelddatabase aan de groep failover.

> [!IMPORTANT]
> Deze cmdlets vereist Azure PowerShell 4.0. [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]
>

1. Variabelen voor de PowerShell-scripts met de waarden voor uw bestaande server en de voorbeelddatabase vullen en geef een globaal unieke waarde voor failover-groepsnaam op.

   ```powershell
   $adminlogin = "ServerAdmin"
   $password = "ChangeYourAdminPassword1"
   $myresourcegroupname = "<your resource group name>"
   $mylocation = "<your resource group location>"
   $myservername = "<your existing server name>"
   $mydatabasename = "mySampleDatabase"
   $mydrlocation = "<your disaster recovery location>"
   $mydrservername = "<your disaster recovery server name>"
   $myfailovergroupname = "<your unique failover group name>"
   ```

2. Een leeg back-server maken in uw regio failover.

   ```powershell
   $mydrserver = New-AzureRmSqlServer -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -Location $mydrlocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $mydrserver   
   ```

3. Maak een groep failover tussen de twee servers.

   ```powershell
   $myfailovergroup = New-AzureRMSqlDatabaseFailoverGroup `
      –ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -PartnerServerName $mydrservername  `
      –FailoverGroupName $myfailovergroupname `
      –FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   $myfailovergroup   
   ```

4. Uw database toevoegen aan de groep failover.

   ```powershell
   $myfailovergroup = Get-AzureRmSqlDatabase `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -DatabaseName $mydatabasename | `
    Add-AzureRmSqlDatabaseToFailoverGroup `
      -ResourceGroupName $myresourcegroupname ` `
      -ServerName $myservername `
      -FailoverGroupName $myfailovergroupname
   $myfailovergroup   
   ```

## <a name="install-java-software"></a>Java-software installeren

Voor de stappen in dit gedeelte wordt ervan uitgegaan dat u bekend bent met het ontwikkelen met Java, maar geen ervaring hebt met het werken met Azure SQL Database. 

### <a name="mac-os"></a>**Mac OS**
Open de terminal en navigeer naar een map waar u van plan bent een Java-project te maken. Installeer **brew** en **Maven** met de volgende opdrachten: 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

Ga voor gedetailleerde richtlijnen voor het installeren en configureren van Java en Maven-omgeving de [bouwen van een app met SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), selecteer **Java**, selecteer **Mac OS**, en volgt u de gedetailleerde instructies voor het configureren van Java en Maven in stap 1.2 en 1.3.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Open de terminal en navigeer naar een map waar u van plan bent een Java-project te maken. Installeer **Maven** met de volgende opdrachten:

```bash
sudo apt-get install maven
```

Ga voor gedetailleerde richtlijnen voor het installeren en configureren van Java en Maven-omgeving de [bouwen van een app met SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), selecteer **Java**, selecteer **Ubuntu**, en volg de gedetailleerde instructies voor het configureren van Java en Maven in stap 1.2, 1.3 en 1.4.

### <a name="windows"></a>**Windows**
Installeer [Maven](https://maven.apache.org/download.cgi) met behulp van het officiële installatieprogramma. Maven gebruiken voor het beheren van afhankelijkheden, bouwen, testen en uw Java-project uitvoeren. Ga voor gedetailleerde richtlijnen voor het installeren en configureren van Java en Maven-omgeving de [bouwen van een app met SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), selecteer **Java**, selecteer Windows en voer vervolgens de gedetailleerde instructies voor Java en de Maven geconfigureerd in stap 1.2 en 1.3.

## <a name="create-sqldbsample-project"></a>SqlDbSample-project maken

1. Maak een Maven-project in de opdrachtconsole (zoals Bash). 
   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```
2. Type **Y** en klik op **Enter**.
3. Wijzig de mappen in het nieuwe project.

   ```bash
   cd SqlDbSamples
   ```

4. Open uw favoriete editor met het bestand pom.xml in de projectmap. 

5. De Microsoft JDBC-stuurprogramma voor SQL Server-afhankelijkheid toevoegen aan uw Maven-project door uw favoriete teksteditor te openen en kopiëren en plakken van de volgende regels in uw pom.xml-bestand. De bestaande waarden vooraf ingevuld in het bestand niet overschrijven. De afhankelijkheid JDBC moet binnen de grotere 'afhankelijkheden' sectie () worden geplakt.   

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

6. Geef de versie van Java voor het compileren van het project op basis van door de volgende sectie van de 'Eigenschappen' in het bestand pom.xml na de sectie 'afhankelijkheden' toe te voegen. 

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```
7. De volgende sectie voor 'maken' in het bestand pom.xml toevoegen na de sectie 'Eigenschappen' ter ondersteuning van de manifest-bestanden in potten.       

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
8. Sla het bestand pom.xml op en sluit het af.
9. Open het bestand App.java (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) en vervang de inhoud door de volgende inhoud. Vervang de naam van de failover met de naam voor de failover-groep. Als u de waarden voor de databasenaam, de gebruiker of het wachtwoord hebt gewijzigd, moet u deze waarden ook wijzigen.

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

      private static final String FAILOVER_GROUP_NAME = "myfailovergroupname";
  
      private static final String DB_NAME = "mySampleDatabase";
      private static final String USER = "app_user";
      private static final String PASSWORD = "ChangeYourPassword1";

      private static final String READ_WRITE_URL = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);
      private static final String READ_ONLY_URL = String.format("jdbc:sqlserver://%s.secondary.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);

      public static void main(String[] args) {
         System.out.println("#######################################");
         System.out.println("## GEO DISTRIBUTED DATABASE TUTORIAL ##");
         System.out.println("#######################################");
         System.out.println(""); 

         int highWaterMark = getHighWaterMarkId();

         try {
            for(int i = 1; i < 1000; i++) {
                //  loop will run for about 1 hour
                System.out.print(i + ": insert on primary " + (insertData((highWaterMark + i))?"successful":"failed"));
                TimeUnit.SECONDS.sleep(1);
                System.out.print(", read from secondary " + (selectData((highWaterMark + i))?"successful":"failed") + "\n");
                TimeUnit.SECONDS.sleep(3);
            }
         } catch(Exception e) {
            e.printStackTrace();
      }
   }

   private static boolean insertData(int id) {
      // Insert data into the product table with a unique product name that we can use to find the product again later
      String sql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

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
      // Query the data that was previously inserted into the primary database from the geo replicated database
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
      // Query the high water mark id that is stored in the table to be able to make unique inserts 
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
6. Sla het bestand App.java op en sluit het af.

## <a name="compile-and-run-the-sqldbsample-project"></a>Compileren en het SqlDbSample-project uitvoeren

1. Uitvoeren op de volgende opdracht in de opdrachtconsole.

   ```bash
   mvn package
   ```
2. Als u klaar bent uitvoeren van de volgende opdracht om uit te voeren van de toepassing (deze wordt uitgevoerd voor ongeveer 1 uur tenzij u handmatig stopt):

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ```

## <a name="perform-disaster-recovery-drill"></a>Disaster recovery inzoomen uitvoeren

1. Handmatige failover van de groep failover-aanroep. 

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $mydrservername `
   -FailoverGroupName $myfailovergroupname
   ```

2. Bekijk het resultaat van de toepassing tijdens de failover. Sommige ingevoegd, mislukken als de DNS-server in de cache wordt vernieuwd.     

3. Ontdek welke rol u uw herstel na noodgevallen server wordt uitgevoerd.

   ```powershell
   $mydrserver.ReplicationRole
   ```

4. Failback.

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $myservername `
   -FailoverGroupName $myfailovergroupname
   ```

5. Bekijk het resultaat van de toepassing tijdens de failback. Sommige ingevoegd, mislukken als de DNS-server in de cache wordt vernieuwd.     

6. Ontdek welke rol u uw herstel na noodgevallen server wordt uitgevoerd.

   ```powershell
   $fileovergroup = Get-AzureRMSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername
   $fileovergroup.ReplicationRole
   ```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie [actieve geo-replicatie en failover groepen](sql-database-geo-replication-overview.md).
