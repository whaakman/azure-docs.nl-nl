---
title: Apache Spark gebruiken om te lezen en schrijven van gegevens naar Azure SQL-database
description: Meer informatie over het instellen van een verbinding tussen HDInsight Spark-cluster en een Azure SQL-database gegevens lezen, schrijven van gegevens en streaminggegevens naar een SQL-database
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2018
ms.openlocfilehash: 8e651f516254d408b15ab4aeae718861dfc2b3e6
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53608286"
---
# <a name="use-hdinsight-spark-cluster-to-read-and-write-data-to-azure-sql-database"></a>HDInsight Spark-cluster gebruiken om te lezen en schrijven van gegevens naar Azure SQL-database

Leer hoe u een Apache Spark-cluster in Azure HDInsight verbinden met een Azure SQL database en vervolgens lezen, schrijven en streamen van gegevens naar de SQL-database. De instructies in dit artikel gebruiken een [Jupyter-Notebook](https://jupyter.org/) om uit te voeren van de Scala-codefragmenten. U kunt echter een zelfstandige toepassing maken in Scala of Python en de dezelfde taken uitvoeren. 

## <a name="prerequisites"></a>Vereisten

* **Azure HDInsight Spark-cluster**.  Volg de instructies op [maken van een Apache Spark-cluster in HDInsight](apache-spark-jupyter-spark-sql.md).

* **Azure SQL-database**. Volg de instructies op [maken van een Azure SQL database](../../sql-database/sql-database-get-started-portal.md). Zorg ervoor dat u een database maken met het voorbeeld **AdventureWorksLT** schema en gegevens. Zorg ervoor dat u maakt een firewall op serverniveau-regel voor het toestaan van uw client-IP-adres voor toegang tot de SQL-database op de server. De instructies voor het toevoegen van de firewallregel is beschikbaar in hetzelfde artikel. Nadat u uw Azure SQL database hebt gemaakt, zorg ervoor dat u de volgende waarden bij de hand houden. U moet ze verbinding kunnen maken met de database van een Spark-cluster.

    * Naam van de server die als host fungeert voor de Azure SQL-database.
    * Azure SQL-databasenaam.
    * Azure SQL database-beheerder-gebruikersnaam / wachtwoord.

* **SQL Server Management Studio**. Volg de instructies op [gebruik SSMS verbinding maken en gegevens op te vragen](../../sql-database/sql-database-connect-query-ssms.md).

## <a name="create-a-jupyter-notebook"></a>Een Jupyter-Notebook maken 

Maak eerst een [Jupyter-Notebook](https://jupyter.org/) die zijn gekoppeld aan het Spark-cluster. Dit notitieblok kunt u de codefragmenten die wordt gebruikt in dit artikel worden uitgevoerd. 

1. Uit de [Azure-portal](https://portal.azure.com/), opent u het cluster.
1. Selecteer **Jupyter-notebook** onder **Clusterdashboards** aan de rechterkant.  Als er geen **Clusterdashboards**, klikt u op **overzicht** in het menu links op de blade. Voer de beheerdersreferenties voor het cluster in als u daarom wordt gevraagd.

    ![Jupyter-notebook in Spark](./media/apache-spark-connect-to-sql-database/hdinsight-spark-cluster-dashboard-jupyter-notebook.png "Jupyter-notebook in Spark")
   
   > [!NOTE]  
   > U kunt de Jupyter-notebook in Spark-cluster ook openen door de volgende URL in uw browser te openen. Vervang **CLUSTERNAME** door de naam van uw cluster.
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

1. Klik in de Jupyter-notebook in de rechterbovenhoek op **nieuw**, en klik vervolgens op **Spark** om een Scala-notitieblok te maken. Jupyter-notebooks in HDInsight Spark-cluster bieden ook de **PySpark** kernel voor Python2-toepassingen, en de **PySpark3** kernel voor Python3-toepassingen. In dit artikel maken wordt een Scala-notebook.
   
    ![Kernels voor Jupyter-notebook in Spark](./media/apache-spark-connect-to-sql-database/kernel-jupyter-notebook-on-spark.png "Kernels voor Jupyter-notebook in Spark")

    Zie [Use Jupyter notebook kernels with Apache Spark clusters in HDInsight](apache-spark-jupyter-notebook-kernels.md) (Jupyter-notebookkernels gebruiken met Apache Spark-clusters in HDInsight) voor meer informatie over de kernels.

   > [!NOTE]  
   > In dit artikel gebruiken we een kernel Spark (Scala) omdat het streaming-gegevens van Spark in SQL-database wordt alleen ondersteund in Scala en Java op dit moment. Hoewel het lezen van en schrijven in SQL kunnen worden gedaan met behulp van Python, voor consistentie in dit artikel, gebruiken we Scala voor alle drie de bewerkingen.

1. Hiermee opent u een nieuwe notebook met een standaardnaam **naamloos**. Klik op de naam van het notitieblok en voer een naam in van uw keuze.

    ![Een naam opgeven voor de notebook](./media/apache-spark-connect-to-sql-database/hdinsight-spark-jupyter-notebook-name.png "Een naam opgeven voor de notebook")

U kunt nu beginnen met het maken van uw toepassing.
    
## <a name="read-data-from-azure-sql-database"></a>Lezen van gegevens van Azure SQL-database

In deze sectie maakt u gegevens lezen uit een tabel (bijvoorbeeld **SalesLT.Address**) die zich in de AdventureWorks-database.

1. Plak het volgende codefragment in een nieuwe Jupyter-notebook in een codecel en vervang de tijdelijke aanduiding door de waarden voor uw Azure SQL-database.

       // Declare the values for your Azure SQL database

       val jdbcUsername = "<SQL DB ADMIN USER>"
       val jdbcPassword = "<SQL DB ADMIN PWD>"
       val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
       val jdbcPort = 1433
       val jdbcDatabase ="<AZURE SQL DB NAME>"

    Druk op **SHIFT+ENTER** om de codecel uit te voeren.  

1. Gebruik het onderstaande fragment een JDBC-URL die u kunt doorgeven aan de API's maakt Spark-gegevensframe maken een `Properties` object voor het opslaan van de parameters. Plak het codefragment in een codecel en druk op **SHIFT + ENTER** om uit te voeren.

       import java.util.Properties

       val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=60;"
       val connectionProperties = new Properties()
       connectionProperties.put("user", s"${jdbcUsername}")
       connectionProperties.put("password", s"${jdbcPassword}")         

1. Gebruik het onderstaande codefragment aan een gegevensframe maken met de gegevens uit een tabel in uw Azure SQL database. In dit fragment gebruiken we een **SalesLT.Address** tabel die is beschikbaar als onderdeel van de **AdventureWorksLT** database. Plak het codefragment in een codecel en druk op **SHIFT + ENTER** om uit te voeren.

       val sqlTableDF = spark.read.jdbc(jdbc_url, "SalesLT.Address", connectionProperties)

1. U kunt nu bewerkingen van het gegevensframe, zoals het ophalen van het schema van de uitvoeren:

       sqlTableDF.printSchema
   
    U ziet een uitvoer die vergelijkbaar is met het volgende:

    ![Een naam opgeven voor de notebook](./media/apache-spark-connect-to-sql-database/read-from-sql-schema-output.png "Een naam opgeven voor de notebook")

1. U kunt ook bewerkingen uitvoeren zoals ophalen de bovenste 10 rijen.

       sqlTableDF.show(10)

1. Of, op te halen van specifieke kolommen uit de gegevensset.

       sqlTableDF.select("AddressLine1", "City").show(10)

## <a name="write-data-into-azure-sql-database"></a>Gegevens schrijven naar Azure SQL-database

In deze sectie gebruiken we een voorbeeld van CSV-bestand beschikbaar op het cluster een tabel in Azure SQL-database maken en deze vullen met gegevens. Het CSV-voorbeeldbestand (**HVAC.csv**) is beschikbaar in alle HDInsight-clusters op `HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv`.

1. Plak het volgende codefragment in een nieuwe Jupyter-notebook in een codecel en vervang de tijdelijke aanduiding door de waarden voor uw Azure SQL-database.

       // Declare the values for your Azure SQL database

       val jdbcUsername = "<SQL DB ADMIN USER>"
       val jdbcPassword = "<SQL DB ADMIN PWD>"
       val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
       val jdbcPort = 1433
       val jdbcDatabase ="<AZURE SQL DB NAME>"

    Druk op **SHIFT+ENTER** om de codecel uit te voeren.  

1. Het volgende fragment maakt een JDBC-URL die u kunt doorgeven aan het Spark-gegevensframe API's maakt een `Properties` object voor het opslaan van de parameters. Plak het codefragment in een codecel en druk op **SHIFT + ENTER** om uit te voeren.

       import java.util.Properties

       val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=60;"
       val connectionProperties = new Properties()
       connectionProperties.put("user", s"${jdbcUsername}")
       connectionProperties.put("password", s"${jdbcPassword}")

1. Gebruikt u het volgende fragment uitpakken van het schema van de gegevens in HVAC.csv en gebruiken van het schema voor het laden van de gegevens uit het CSV-bestand in een dataframe `readDf`. Plak het codefragment in een codecel en druk op **SHIFT + ENTER** om uit te voeren.

       val userSchema = spark.read.option("header", "true").csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv").schema
       val readDf = spark.read.format("csv").schema(userSchema).load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

1. Gebruik de `readDf` dataframe te maken van een tijdelijke tabel `temphvactable`. De tijdelijke tabel vervolgens gebruiken om een hive-tabel `hvactable_hive`.

       readDf.createOrReplaceTempView("temphvactable")
       spark.sql("create table hvactable_hive as select * from temphvactable")

1. Gebruik tot slot de hive-tabel naar een tabel maken in Azure SQL-database. Hiermee maakt u het volgende codefragment `hvactable` in Azure SQL-database.

       spark.table("hvactable_hive").write.jdbc(jdbc_url, "hvactable", connectionProperties)

1. Verbinding maken met de Azure SQL-database met behulp van SSMS en controleer of dat u ziet een `dbo.hvactable` er.

    a. Start SSMS en maak verbinding met de Azure SQL database door te geven Verbindingsdetails zoals wordt weergegeven in de onderstaande schermafbeelding.

    ![Verbinding maken met SQL database met behulp van SSMS](./media/apache-spark-connect-to-sql-database/connect-to-sql-db-ssms.png "verbinding maken met SQL database met behulp van SSMS")

    b. Vouw in Object Explorer de Azure SQL database en het knooppunt van de tabel om te zien de **dbo.hvactable** gemaakt.

    ![Verbinding maken met SQL database met behulp van SSMS](./media/apache-spark-connect-to-sql-database/connect-to-sql-db-ssms-locate-table.png "verbinding maken met SQL database met behulp van SSMS")

1. Een query uitvoeren in SSMS om te zien van de kolommen in de tabel.

        SELECT * from hvactable

## <a name="stream-data-into-azure-sql-database"></a>Stream-gegevens in Azure SQL-database

In deze sectie we streamen van gegevens in de **hvactable** dat u al hebt gemaakt in Azure SQL-database in de vorige sectie.

1. Als eerste stap, zorg ervoor dat er zijn geen records in de **hvactable**. Met behulp van SSMS, de volgende query uitvoeren op de tabel.

       DELETE FROM [dbo].[hvactable]

1. Maak een nieuwe Jupyter-notebook op de HDInsight Spark-cluster. Plak het volgende codefragment in een codecel en druk vervolgens op **SHIFT + ENTER**:

       import org.apache.spark.sql._
       import org.apache.spark.sql.types._
       import org.apache.spark.sql.functions._
       import org.apache.spark.sql.streaming._
       import java.sql.{Connection,DriverManager,ResultSet}

1. We streamen van gegevens uit de **HVAC.csv** in de hvactable. HVAC.csv bestand is beschikbaar op het cluster op */HdiSamples/HdiSamples/SensorSampleData/HVAC/*. In het volgende codefragment krijgen we eerst het schema van de gegevens kunnen worden gestreamd. Vervolgens maakt u een streaming dataframe met behulp van dit schema. Plak het codefragment in een codecel en druk op **SHIFT + ENTER** om uit te voeren.

       val userSchema = spark.read.option("header", "true").csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv").schema
       val readStreamDf = spark.readStream.schema(userSchema).csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/") 
       readStreamDf.printSchema

1. De uitvoer ziet u het schema van **HVAC.csv**. De **hvactable** hetzelfde schema heeft. De uitvoer bevat de kolommen in de tabel.

    ![Schema van tabel](./media/apache-spark-connect-to-sql-database/schema-of-table.png "Schema van tabel")

1. Gebruik ten slotte het volgende codefragment om te lezen van gegevens van de HVAC.csv en stream het naar de **hvactable** in Azure SQL-database. Plak het codefragment in een codecel, vervang de tijdelijke aanduiding door de waarden voor uw Azure SQL database en druk vervolgens op **SHIFT + ENTER** om uit te voeren.

       val WriteToSQLQuery  = readStreamDf.writeStream.foreach(new ForeachWriter[Row] {
          var connection:java.sql.Connection = _
          var statement:java.sql.Statement = _
          
          val jdbcUsername = "<SQL DB ADMIN USER>"
          val jdbcPassword = "<SQL DB ADMIN PWD>"
          val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
          val jdbcPort = 1433
          val jdbcDatabase ="<AZURE SQL DB NAME>"
          val driver = "com.microsoft.sqlserver.jdbc.SQLServerDriver"
          val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
  
         def open(partitionId: Long, version: Long):Boolean = {
           Class.forName(driver)
           connection = DriverManager.getConnection(jdbc_url, jdbcUsername, jdbcPassword)
           statement = connection.createStatement
           true
         }
  
         def process(value: Row): Unit = {
           val Date  = value(0)
           val Time = value(1)
           val TargetTemp = value(2)
           val ActualTemp = value(3)
           val System = value(4)
           val SystemAge = value(5)
           val BuildingID = value(6)  
    
           val valueStr = "'" + Date + "'," + "'" + Time + "'," + "'" + TargetTemp + "'," + "'" + ActualTemp + "'," + "'" + System + "'," + "'" + SystemAge + "'," + "'" + BuildingID + "'"
           statement.execute("INSERT INTO " + "dbo.hvactable" + " VALUES (" + valueStr + ")")   
           }

         def close(errorOrNull: Throwable): Unit = {
            connection.close
          }
         })
        
         var streamingQuery = WriteToSQLQuery.start()

1. Controleer of dat de gegevens worden gestreamd naar de **hvactable** door het uitvoeren van de volgende query in SQL Server Management Studio (SSMS). Telkens wanneer u de query uitvoert, ziet deze het aantal rijen in de tabel verhogen.

        SELECT COUNT(*) FROM hvactable

## <a name="next-steps"></a>Volgende stappen

* [HDInsight Spark-cluster gebruiken voor het analyseren van gegevens in Data Lake-opslag](apache-spark-use-with-data-lake-store.md)
* [Structured streaming gebeurtenissen verwerken met behulp van Event hub](apache-spark-eventhub-structured-streaming.md)
* [Apache Spark Structured Streaming met Apache Kafka in HDInsight gebruiken](../hdinsight-apache-kafka-spark-structured-streaming.md)
