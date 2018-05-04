---
title: Een geografisch gedistribueerde Azure SQL Database-oplossing implementeren| Microsoft Docs
description: Informatie over het configureren van de Azure SQL-database en toepassing voor failover naar een gerepliceerde database, en failover testen.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: mvc,business continuity
ms.topic: tutorial
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: fbd239c3c8c11b1907a6d28eb95d2c0ad26cfe61
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="implement-a-geo-distributed-database"></a>Een geografisch gedistribueerde database implementeren

In deze zelfstudie configureert u een Azure SQL-database en -toepassing voor failover naar een externe regio, waarna u het failoverschema gaat testen. In deze zelfstudie leert u procedures om het volgende te doen: 

> [!div class="checklist"]
> * Databasegebruikers maken en ze machtigingen verlenen
> * Firewallregel op databaseniveau instellen
> * [Failover-groep met geo-replicatie](sql-database-geo-replication-overview.md) maken
> * Java-toepassing maken en compileren om een query uit te voeren van een Azure SL-database
> * Noodherstelanalyse uitvoeren

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.


## <a name="prerequisites"></a>Vereisten

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

- Meest recente [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs) is geïnstalleerd. 
- Azure SQL-database is geïnstalleerd. In deze zelfstudie wordt de voorbeelddatabase AdventureWorksLT gebruikt met de naam **mySampleDatabase** uit een van deze snelstartgidsen:

   - [Database maken - Portal](sql-database-get-started-portal.md)
   - [Database maken - CLI](sql-database-get-started-cli.md)
   - [Database maken - PowerShell](sql-database-get-started-powershell.md)

- Als u een methode hebt geïdentificeerd voor het uitvoeren van SQL-scripts tegen de database, kunt u een van de volgende query-hulpprogramma's gebruiken:
   - Query-editor in [Azure Portal](https://portal.azure.com). Zie [Verbinding maken en query's uitvoeren met Query-editor](sql-database-get-started-portal.md#query-the-sql-database) voor meer informatie over het gebruik van de query-editor in Azure Portal.
   - De nieuwste versie van [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), een geïntegreerde omgeving voor het beheren van een SQL-infrastructuur, van SQL Server tot SQL Database voor Microsoft Windows.
   - De nieuwste versie van [Visual Studio Code](https://code.visualstudio.com/docs), een grafische code-editor voor Linux, Mac OS en Windows die ondersteuning biedt voor extensies, waaronder de [mssql-extensie](https://aka.ms/mssql-marketplace) voor het uitvoeren van query's in Microsoft SQL Server, Azure SQL Database en SQL Data Warehouse. Zie [Verbinding maken en query's uitvoeren met VS Code](sql-database-connect-query-vscode.md) voor meer informatie over het gebruik van dit hulpprogramma met Azure SQL Database. 

## <a name="create-database-users-and-grant-permissions"></a>Databasegebruikers maken en machtigingen verlenen

Maak verbinding met uw database en maak gebruikersaccounts met behulp van een van de volgende query-hulpprogramma's:

- Query-editor in Azure Portal
- SQL Server Management Studio
- Visual Studio Code

Deze gebruikersaccounts worden automatisch gerepliceerd met de secundaire server (en gesynchroniseerd gehouden). Als u SQL Server Management Studio of Visual Studio Code wilt gebruiken, dient u een firewallregel te configureren als u verbinding maakt vanaf een client op een IP-adres waarvoor nog geen firewall is geconfigureerd. Zie [Een serverfirewallregel maken](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) voor uitgebreide stappen.

- Voer in een queryvenster de volgende query uit om twee gebruikersaccounts te maken in uw database. Dit script verleent **db_owner**-machtigingen aan het **app_admin**-account en verleent de machtigingen **SELECT** en **UPDATE** aan het **app_user**-account. 

   ```sql
   CREATE USER app_admin WITH PASSWORD = 'ChangeYourPassword1';
   --Add SQL user to db_owner role
   ALTER ROLE db_owner ADD MEMBER app_admin; 
   --Create additional SQL user
   CREATE USER app_user WITH PASSWORD = 'ChangeYourPassword1';
   --grant permission to SalesLT schema
   GRANT SELECT, INSERT, DELETE, UPDATE ON SalesLT.Product TO app_user;
   ```

## <a name="create-database-level-firewall"></a>Firewall op databaseniveau maken

Maak een [firewallregel op databaseniveau](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) voor uw SQL-database. Deze firewallregel op databaseniveau wordt automatisch gerepliceerd met de secundaire server die u in deze zelfstudie hebt gemaakt. Gebruik voor het gemak (in deze zelfstudie) het openbare IP-adres van de computer waarop u de stappen in deze zelfstudie uitvoert. Zie [Een serverfirewallregel maken](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) om het IP-adres vast te stellen dat wordt gebruikt voor de firewall op serverniveau voor uw huidige computer.  

- Vervang in het geopende queryvenster de vorige query door de volgende, waarbij u het IP-adres vervangt door de bijbehorende IP-adressen voor uw omgeving.  

   ```sql
   -- Create database-level firewall setting for your public IP address
   EXECUTE sp_set_database_firewall_rule @name = N'myGeoReplicationFirewallRule',@start_ip_address = '0.0.0.0', @end_ip_address = '0.0.0.0';
   ```

## <a name="create-an-active-geo-replication-auto-failover-group"></a>Actieve automatische-failovergroep met actieve geo-replicatie maken 

Maak met behulp van Azure PowerShell een [actieve automatische-failovergroep met actieve geo-replicatie](sql-database-geo-replication-overview.md) tussen uw bestaande Azure SQL-server en de nieuwe, lege Azure SQL-server in een Azure-regio. Voeg vervolgens uw voorbeelddatabase aan de failovergroep toe.

> [!IMPORTANT]
> Voor deze cmdlets is Azure PowerShell 4.0 vereist. [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]
>

1. Vul variabelen in voor uw PowerShell-scripts met behulp van de waarden voor uw bestaande server en voorbeelddatabase, en geef een globaal unieke waarde op voor de naam van de failovergroep.

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

2. Maak een lege back-upserver in uw failoverregio.

   ```powershell
   $mydrserver = New-AzureRmSqlServer -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -Location $mydrlocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $mydrserver   
   ```

3. Maak een failovergroep tussen de twee servers.

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

4. Voeg uw database toe aan de failovergroep.

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

Voor uitgebreide begeleiding voor het installeren en configureren van een Java- en Maven-omgeving, gaat u naar [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/) (Apps bouwen met SQL Server). Selecteer **Java**, selecteer **MacOS** en volg de uitgebreide instructies voor het configureren van Java en Maven in stap 1.2 en 1.3.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Open de terminal en navigeer naar een map waar u van plan bent een Java-project te maken. Installeer **Maven** met de volgende opdrachten:

```bash
sudo apt-get install maven
```

Voor uitgebreide begeleiding voor het installeren en configureren van een Java- en Maven-omgeving, gaat u naar [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/) (Apps bouwen met SQL Server). Selecteer **Java**, selecteer **Ubuntu** en volg de uitgebreide instructies voor het configureren van Java en Maven in stap 1.2, 1.3 en 1.4.

### <a name="windows"></a>**Windows**
Installeer [Maven](https://maven.apache.org/download.cgi) met behulp van het officiële installatieprogramma. Gebruik Maven om afhankelijkheden te beheren en uw Java-project te bouwen, te testen en uit te voeren. Voor uitgebreide begeleiding voor het installeren en configureren van een Java- en Maven-omgeving, gaat u naar [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/) (Apps bouwen met SQL Server). Selecteer **Java**, selecteer Windows en volg de uitgebreide instructies voor het configureren van Java en Maven in stap 1.2 en 1.3.

## <a name="create-sqldbsample-project"></a>SqlDbSample-project maken

1. Maak een Maven-project in de opdrachtconsole (bijvoorbeeld Bash). 
   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```
2. Typ **Y** en klik op **Enter**.
3. Wijzig mappen in het nieuwe project.

   ```bash
   cd SqlDbSamples
   ```

4. Open in een editor het POM.XML-bestand in de projectmap. 

5. Voeg de afhankelijkheid Microsoft JDBC-stuurprogramma voor SQL Server toe aan het Maven-project door de editor te openen en de volgende regels te kopiëren en in het POM.XML-bestand te plakken. Overschrijf de bestaande waarden die vooraf in het bestand zijn ingevuld niet. De JDBC-afhankelijkheid moet tussen de grotere sectie 'dependencies' ( ) worden geplakt.   

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

6. Geef de Java-versie op waarvoor het project moet worden gecompileerd door na de sectie 'dependencies' de volgende sectie 'properties' aan het POM.XML-bestand toe te voegen. 

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```
7. Voeg na de sectie 'dependencies' de volgende sectie 'build' toe aan het POM.XML-bestand ter ondersteuning van manifestbestanden in JAR-bestanden.       

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
9. Open het APP.JAVA-bestand (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) en vervang de inhoud door de volgende inhoud. Vervang de naam van de failovergroep door de naam van uw failovergroep. Als u de waarden voor de databasenaam, -gebruiker of het databasewachtwoord hebt gewijzigd, wijzigt u ook die waarden.

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

## <a name="compile-and-run-the-sqldbsample-project"></a>SqlDbSample-project compileren en uitvoeren

1. Voer in de opdrachtconsole de volgende opdracht uit.

   ```bash
   mvn package
   ```
2. Als u klaar bent, voert u de volgende opdracht uit om de toepassing uit te voeren (deze wordt circa een uur uitgevoerd, tenzij u deze handmatig stopt):

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ```

## <a name="perform-disaster-recovery-drill"></a>Noodherstelanalyse uitvoeren

1. Roep de handmatige failover van de failovergroep aan. 

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $mydrservername `
   -FailoverGroupName $myfailovergroupname
   ```

2. Bekijk de toepassingsresultaten tijdens failover. Sommige invoegingen mislukken tijdens het vernieuwen van de DNS-cache.     

3. Zoek uit welke rol de server voor noodherstelanalyse uitvoert.

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

5. Bekijk de toepassingsresultaten tijdens failback. Sommige invoegingen mislukken tijdens het vernieuwen van de DNS-cache.     

6. Zoek uit welke rol de server voor noodherstelanalyse uitvoert.

   ```powershell
   $fileovergroup = Get-AzureRMSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername
   $fileovergroup.ReplicationRole
   ```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een Azure SQL-database en -toepassing configureert voor failover naar een externe regio, en hoe u het failoverschema kunt testen.  U hebt geleerd hoe u: 

> [!div class="checklist"]
> * Databasegebruikers maken en ze machtigingen verlenen
> * Firewallregel op databaseniveau instellen
> * Een failover-groep met geo-replicatie maken
> * Java-toepassing maken en compileren om een query uit te voeren van een Azure SL-database
> * Noodherstelanalyse uitvoeren

Ga door naar de volgende zelfstudie voor informatie over het maken van een beheerd exemplaar.

> [!div class="nextstepaction"]
>[Een beheerd exemplaar maken](sql-database-managed-instance-create-tutorial-portal.md)

