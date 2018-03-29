---
title: Apache Spark gebruiken om te lezen en schrijven van gegevens naar Azure SQL database | Microsoft Docs
description: Meer informatie over het instellen van een verbinding tussen HDInsight Spark-cluster en een Azure SQL database om te lezen van gegevens, gegevens en stroomgegevens schrijven naar een SQL-database
services: hdinsight
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/28/2018
ms.author: nitinme
ms.openlocfilehash: 011471224d79ae326ccc33fc23fd6fada347497b
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2018
---
# <a name="use-hdinsight-spark-cluster-to-read-and-write-data-to-azure-sql-database"></a>HDInsight Spark-cluster gebruiken om te lezen en schrijven van gegevens naar Azure SQL-database

Ontdek hoe u een Apache Spark-cluster in Azure HDInsight verbinden met een Azure SQL database en vervolgens lezen, schrijven en het streamen van gegevens in de SQL-database. De instructies in dit artikel gebruiken een Jupyter-notebook om uit te voeren de codefragmenten Scala. U kunt echter een zelfstandige toepassing maken in Scala of Python en dezelfde taken uitvoeren. 

## <a name="prerequisites"></a>Vereisten

* **Azure HDInsight Spark-cluster**.  Volg de instructies voor [maken van een Apache Spark-cluster in HDInsight](apache-spark-jupyter-spark-sql.md).

* **Azure SQL-database**. Volg de instructies voor [maken van een Azure SQL database](../../sql-database/sql-database-get-started-portal.md). Zorg ervoor dat u een database maken met het voorbeeld **AdventureWorksLT** schema en de gegevens. Controleer ook of dat u een firewallregel op serverniveau zodat IP-adres van de client toegang tot de SQL-database op de server maken. De instructies voor het toevoegen van de firewallregel is beschikbaar in hetzelfde artikel. Nadat u uw Azure SQL database hebt gemaakt, controleert u of dat u de volgende waarden bij de hand houden. U moet ze verbinding maken met de database van een Spark-cluster.

    * De naam van de server die als host fungeert voor de Azure SQL database
    * Naam van een Azure SQL-database
    * Azure SQL database admin-gebruikersnaam / wachtwoord

* **SQL Server Management Studio**. Volg de instructies voor [gebruik SSMS verbinding maakt en gegevens opvragen](../../sql-database/sql-database-connect-query-ssms.md).

## <a name="create-a-jupyter-notebook"></a>Een Jupyter-notebook maken

Begint met het maken van een Jupyter-notebook die zijn gekoppeld aan het Spark-cluster. Met deze notebook kunt u de codefragmenten gebruikt in dit artikel worden uitgevoerd. 

1. Van de [Azure-portal](https://portal.azure.com/), opent u het cluster. 

2. Van de **snelle koppelingen** sectie, klikt u op **Cluster dashboards** openen de **Cluster dashboards** weergeven.  Als er geen **snelkoppelingen**, klikt u op **overzicht** in het menu links op de blade.

    ![Cluster-dashboard op Spark](./media/apache-spark-connect-to-sql-database/hdinsight-cluster-dashboard-on-spark.png "Cluster-dashboard op Spark") 

3. Klik op **Jupyter-Notebook**. Voer de beheerdersreferenties voor het cluster in als u daarom wordt gevraagd.

    ![Jupyter-notebook in Spark](./media/apache-spark-connect-to-sql-database/hdinsight-jupyter-notebook-on-spark.png "Jupyter-notebook in Spark")
   
   > [!NOTE]
   > U kunt de Jupyter-notebook in Spark-cluster ook openen door de volgende URL in uw browser te openen. Vervang **CLUSTERNAME** door de naam van uw cluster.
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 

4. Klik in de Jupyter-notebook in de rechterbovenhoek op **nieuw**, en klik vervolgens op **Spark** een Scala-notebook maken. Jupyter-notebooks op HDInsight Spark-cluster bieden ook de **PySpark** kernel voor Python2 toepassingen, en de **PySpark3** kernel voor Python3 toepassingen. Voor dit artikel maken we een laptop Scala.
   
    ![Kernels voor Jupyter-notebook in Spark](./media/apache-spark-connect-to-sql-database/kernel-jupyter-notebook-on-spark.png "Kernels voor Jupyter-notebook in Spark")

    Zie [Use Jupyter notebook kernels with Apache Spark clusters in HDInsight](apache-spark-jupyter-notebook-kernels.md) (Jupyter-notebookkernels gebruiken met Apache Spark-clusters in HDInsight) voor meer informatie over de kernels.

   > [!NOTE]
   > In dit artikel gebruiken we een kernel Spark (Scala) omdat streaminggegevens van Spark in SQL-database wordt alleen ondersteund in Scala en Java momenteel. Hoewel het lezen van en schrijven naar SQL kunnen worden gedaan met behulp van Python, voor consistentie in dit artikel, we Scala gebruiken voor alle drie de bewerkingen.
   >

5. Hiermee opent u een nieuwe notebook met een standaardnaam **naamloos**. Klik op de naam van de notebook en voer een naam van uw keuze.

    ![Een naam opgeven voor de notebook](./media/apache-spark-connect-to-sql-database/hdinsight-spark-jupyter-notebook-name.png "Een naam opgeven voor de notebook")

U kunt nu beginnen met het maken van uw toepassing.
    
## <a name="read-data-from-azure-sql-database"></a>Gegevens lezen in Azure SQL database

In deze sectie kunt u gegevens lezen van een tabel (bijvoorbeeld **SalesLT.Address**) die zich in de database AdventureWorks.

1. Plak het volgende codefragment in een nieuwe Jupyter-notebook in een codecel en vervang de tijdelijke aanduiding voor waarden door de waarden voor uw Azure SQL database.

       // Declare the values for your Azure SQL database

       val jdbcUsername = "<SQL DB ADMIN USER>"
       val jdbcPassword = "<SQL DB ADMIN PWD>"
       val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
       val jdbcPort = 1433
       val jdbcDatabase ="<AZURE SQL DB NAME>"

    Druk op **SHIFT + ENTER** om uit te voeren van de codecel.  

2. Het volgende fragment maakt een JDBC-URL die u kunt doorgeven aan de Spark dataframe API's maakt een `Properties` object voor het opslaan van de parameters. Plak het codefragment in een codecel en druk op **SHIFT + ENTER** om uit te voeren.

       import java.util.Properties

       val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=60;"
       val connectionProperties = new Properties()
       connectionProperties.put("user", s"${jdbcUsername}")
       connectionProperties.put("password", s"${jdbcPassword}")         

3. Het volgende codefragment maakt een dataframe met de gegevens uit een tabel in uw Azure SQL database. In dit fragment gebruiken we een **SalesLT.Address** tabel die beschikbaar is als onderdeel van de **AdventureWorksLT** database. Plak het codefragment in een codecel en druk op **SHIFT + ENTER** om uit te voeren.

       val sqlTableDF = spark.read.jdbc(jdbc_url, "SalesLT.Address", connectionProperties)

4. Nu kunt u bewerkingen op de dataframe, zoals het ophalen van het schema van de uitvoeren:

       sqlTableDF.printSchema
   
    U ziet de uitvoer ziet er als volgt:

    ![Een naam opgeven voor de notebook](./media/apache-spark-connect-to-sql-database/read-from-sql-schema-output.png "Een naam opgeven voor de notebook")

5. U kunt bewerkingen zoals ophalen ook de bovenste 10 rijen uitvoeren.

       sqlTableDF.show(10)

6. Of het ophalen van specifieke kolommen uit de gegevensset.

       sqlTableDF.select("AddressLine1", "City").show(10)

## <a name="write-data-into-azure-sql-database"></a>Schrijven van gegevens naar Azure SQL-database

In deze sectie gebruiken we een CSV-voorbeeldbestand beschikbaar op het cluster een tabel maken in Azure SQL-database en deze vullen met gegevens. De CSV-voorbeeldbestand (**HVAC.csv**) beschikbaar is op alle HDInsight-clusters op `HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv`.

1. Plak het volgende codefragment in een nieuwe Jupyter-notebook in een codecel en vervang de tijdelijke aanduiding voor waarden door de waarden voor uw Azure SQL database.

       // Declare the values for your Azure SQL database

       val jdbcUsername = "<SQL DB ADMIN USER>"
       val jdbcPassword = "<SQL DB ADMIN PWD>"
       val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
       val jdbcPort = 1433
       val jdbcDatabase ="<AZURE SQL DB NAME>"

    Druk op **SHIFT + ENTER** om uit te voeren van de codecel.  

2. Het volgende fragment maakt een JDBC-URL die u kunt doorgeven aan de Spark dataframe API's maakt een `Properties` object voor het opslaan van de parameters. Plak het codefragment in een codecel en druk op **SHIFT + ENTER** om uit te voeren.

       import java.util.Properties

       val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=60;"
       val connectionProperties = new Properties()
       connectionProperties.put("user", s"${jdbcUsername}")
       connectionProperties.put("password", s"${jdbcPassword}")

3. Het volgende fragment pakt u het schema van de gegevens in HVAC.csv en het schema gebruikt voor het laden van de gegevens van de CSV in een dataframe `readDf`. Plak het codefragment in een codecel en druk op **SHIFT + ENTER** om uit te voeren.

       val userSchema = spark.read.option("header", "true").csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv").schema
       val readDf = spark.read.format("csv").schema(userSchema).load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

4. Gebruik de `readDf` dataframe voor het maken van een tijdelijke tabel `temphvactable`. Gebruik vervolgens de tijdelijke tabel te maken van een hive-tabel `hvactable_hive`.

       readDf.createOrReplaceTempView("temphvactable")
       spark.sql("create table hvactable_hive as select * from temphvactable")

5. Gebruik tot slot de hive-tabel maken van een tabel in Azure SQL-database. Het volgende codefragment maakt `hvactable` in Azure SQL-database.

       spark.table("hvactable_hive").write.jdbc(jdbc_url, "hvactable", connectionProperties)

6. Verbinding maken met de Azure SQL database met behulp van SSMS en controleer of u ziet een `dbo.hvactable` er.

    a. SSMS te starten en verbinding maken met de Azure SQL database Verbindingsdetails verstrekken die wordt weergegeven in de onderstaande schermafbeelding.

    ![Verbinding maken met SQL database met behulp van SSMS](./media/apache-spark-connect-to-sql-database/connect-to-sql-db-ssms.png "verbinding maken met SQL database met behulp van SSMS")

    b. Vouw in Object Explorer, de Azure SQL database en het knooppunt tabel om te zien de **dbo.hvactable** gemaakt.

    ![Verbinding maken met SQL database met behulp van SSMS](./media/apache-spark-connect-to-sql-database/connect-to-sql-db-ssms-locate-table.png "verbinding maken met SQL database met behulp van SSMS")

## <a name="stream-data-into-azure-sql-database"></a>Van stroomgegevens in Azure SQL database

In deze sectie stream we gegevens in de **hvactable** dat u al in Azure SQL-database in de vorige sectie gemaakt.

1. Als eerste stap, zorg ervoor dat er zijn geen records in de **hvactable**. Met behulp van SSMS, de volgende query uitvoeren op de tabel.

       DELETE FROM [dbo].[hvactable]

2. Maak een nieuwe Jupyter-notebook op HDInsight Spark-cluster. Plak het volgende codefragment in een codecel en druk vervolgens op **SHIFT + ENTER**:

       import org.apache.spark.sql._
       import org.apache.spark.sql.types._
       import org.apache.spark.sql.functions._
       import org.apache.spark.sql.streaming._
       import java.sql.{Connection,DriverManager,ResultSet}

3. We stream met gegevens uit de **HVAC.csv** in de hvactable. HVAC.csv bestand is beschikbaar op het cluster op */HdiSamples/HdiSamples/SensorSampleData/HVAC/*. In het volgende fragment krijgen we eerst het schema van de gegevens kunnen worden gestreamd. Vervolgens maakt u een streaming dataframe met behulp van dat het schema. Plak het codefragment in een codecel en druk op **SHIFT + ENTER** om uit te voeren.

       val userSchema = spark.read.option("header", "true").csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv").schema
       val readStreamDf = spark.readStream.schema(userSchema1).csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/") 
       readStreamDf.printSchema

4. De uitvoer ziet u het schema van **HVAC.csv**. De **hvactable** heeft van hetzelfde schema. De uitvoer bevat de kolommen in de tabel.

    ![Schema van tabel](./media/apache-spark-connect-to-sql-database/schema-of-table.png "Schema van tabel")

5. Ten slotte het volgende fragment gebruiken voor het lezen van gegevens van de HVAC.csv en stream in de **hvactable** in Azure SQL-database. Het codefragment in een codecel plakken, vervang de tijdelijke aanduiding voor waarden door de waarden voor uw Azure SQL database en druk vervolgens op **SHIFT + ENTER** om uit te voeren.

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

6. Controleer of de gegevens wordt gestreamd naar de **hvactable** door het uitvoeren van de volgende query in SQL Server Management Studio (SSMS). Telkens wanneer u de query uitvoert, wordt het aantal rijen in de tabel verhogen.

        SELECT COUNT(*) FROM hvactable

## <a name="next-steps"></a>Volgende stappen

* [HDInsight Spark-cluster gebruiken voor het analyseren van gegevens in Data Lake Store](apache-spark-use-with-data-lake-store.md)
* [Proces gestructureerd streaming met behulp van EventHub-gebeurtenissen](apache-spark-eventhub-structured-streaming.md)
* [Spark gestructureerd streamen met Kafka in HDInsight gebruiken](../hdinsight-apache-kafka-spark-structured-streaming.md)
