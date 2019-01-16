---
title: 'Zelfstudie: Meer informatie over extraheren, laden en overdragen door gebruik te maken van Azure Databricks'
description: In deze zelfstudie leert u hoe u gegevens uit Azure Data Lake Storage Gen2 Preview extraheert en overdraagt naar Azure Databricks, de gegevens transformeert en vervolgens laadt in Azure SQL Data Warehouse.
services: storage
author: jamesbak
ms.service: storage
ms.author: jamesbak
ms.topic: tutorial
ms.date: 12/06/2018
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 6b2812e31174c4e5d61ae9941563e39357de9522
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54107086"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-azure-databricks"></a>Zelfstudie: Gegevens extraheren, transformeren en laden met Azure Databricks

In deze zelfstudie gebruikt u Azure Databricks om een ETL-bewerking uit te voeren (gegevens extraheren, transformeren en laden). U verplaatst gegevens vanuit een Azure Storage-account met Azure Data Lake Storage Gen2 dat voor Azure SQL Data Warehouse is ingeschakeld.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Azure Databricks-werkruimte maken.
> * Een Apache Spark-cluster in Azure Databricks maken.
> * Een voor Azure Data Lake Storage Gen2 geschikt account maken.
> * Gegevens uploaden naar Azure Data Lake Storage Gen2.
> * Een notitieblok maken in Azure Databricks.
> * Gegevens extraheren uit Data Lake Store Gen2.
> * Gegevens transformeren in Azure Databricks.
> * Gegevens laden in Azure SQL Data Warehouse.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

* Maak een Azure SQL-datawarehouse, maak een firewallregel op serverniveau en maak verbinding met de server als serverbeheerder. Volg de instructies in het artikel [Quick Start: een Azure SQL Data Warehouse maken](../../sql-data-warehouse/create-data-warehouse-portal.md).
* Maak een databasehoofdsleutel voor de Azure SQL-datawarehouse. Volg de instructies in het artikel [Create a database master key](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key) (Een databasehoofdsleutel maken).
* [Een Azure Data Lake Storage Gen2-account maken](data-lake-storage-quickstart-create-account.md).
* Download (**small_radio_json.json**) van de opslagplaatsen [U-SQL-voorbeelden en problemen bijhouden](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json) en noteer het pad waar u het bestand opslaat.
* Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-the-workspace"></a>De werkruimte maken

In deze sectie gaat u een Azure Databricks-werkruimte maken met behulp van de Azure-portal.

1. Selecteer in Azure Portal **Een resource maken** > **Analyse** > **Azure Databricks**.

    ![Databricks in Azure Portal](./media/data-lake-storage-handle-data-using-databricks/azure-databricks-on-portal.png "Databricks in Azure Portal")

1. Geef bij **Azure Databricks Service** de volgende waarden op voor het maken van een Databricks-werkruimte:

    |Eigenschap  |Beschrijving  |
    |---------|---------|
    |**Werkruimtenaam**     | Geef een naam op voor de Databricks-werkruimte.        |
    |**Abonnement**     | Selecteer uw Azure-abonnement in de vervolgkeuzelijst.        |
    |**Resourcegroep**     | Geef aan of u een nieuwe resourcegroep wilt maken of een bestaande groep wilt gebruiken. Een resourcegroep is een container met gerelateerde resources voor een Azure-oplossing. Zie [Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) voor meer informatie. |
    |**Locatie**     | Selecteer **US - west 2**.        |
    |**Prijscategorie**     |  selecteer **Standaard**.     |

    ![Een Azure Databricks-werkruimte maken](./media/data-lake-storage-handle-data-using-databricks/create-databricks-workspace.png "Een Azure Databricks-werkruimte maken")

1. Selecteer **Vastmaken aan dashboard** en selecteer **Maken**.

1. Het duurt enkele minuten om het account te maken. Tijdens het maken van het account wordt rechts in de portal de tegel **Implementatie verzenden voor Azure Databricks** weergegeven. Bekijk de voortgangsbalk bovenaan om de bewerkingsstatus te volgen.

    ![Tegel Databricks-implementatie](./media/data-lake-storage-handle-data-using-databricks/databricks-deployment-tile.png "Tegel Databricks-implementatie")

## <a name="create-the-spark-cluster"></a>Het Apache Spark-cluster maken

U hebt een Apache Spark-cluster nodig als u de bewerkingen in deze zelfstudie wilt uitvoeren. Volg deze stappen om het Apache Spark-cluster te maken.

1. Ga in de Azure-portal naar de Databricks-werkruimte die u hebt gemaakt en selecteer **Werkruimte starten**.

1. U wordt omgeleid naar de Azure Databricks-portal. Klik in de portal op **Cluster**.

    ![Databricks in Azure](./media/data-lake-storage-handle-data-using-databricks/databricks-on-azure.png "Databricks in Azure")

1. Op de pagina **Nieuw cluster** geeft u de waarden op waarmee een nieuw cluster wordt gemaakt.

    ![Een Databricks Spark-cluster maken in Azure](./media/data-lake-storage-handle-data-using-databricks/create-databricks-spark-cluster.png "Een Databricks Spark-cluster maken in Azure")

1. Vul de waarden voor de volgende velden in (en laat bij de overige velden de standaardwaarden staan):

    * Voer een naam in voor het cluster.
    * Maak voor dit artikel een cluster met de **5.1**-runtime.
    * Zorg ervoor dat u het selectievakje **Beëindigen na \_\_ minuten van inactiviteit** inschakelt. Geef een duur (in minuten) op waarna het cluster moet worden beëindigd als het niet wordt gebruikt.

1. Selecteer **Cluster maken**.

Als het cluster wordt uitgevoerd, kunt u notitieblokken koppelen aan het cluster en Apache Spark-taken uitvoeren.

## <a name="create-a-file-system"></a>Een bestandssysteem maken

Als u gegevens wilt opslaan in uw Data Lake Storage Gen2-opslagaccount, dient u een bestandssysteem te maken.

Eerst maakt u een notitieblok in de Azure Databricks-werkruimte en vervolgens voert u codefragmenten uit om het bestandssysteem in uw opslagaccount te maken.

1. Ga in de [Azure-portal](https://portal.azure.com) naar de Azure Databricks-werkruimte die u hebt gemaakt en selecteer **Werkruimte starten**.

1. Selecteer aan de linkerkant **Werkruimte**. Selecteer in de **Werkruimte**-vervolgkeuzelijst, **Notitieblok** > **maken**.

    ![Een notitieblok maken in Databricks](./media/data-lake-storage-handle-data-using-databricks/databricks-create-notebook.png "Een notitieblok maken in Databricks")

1. Voer in het dialoogvenster **Notitieblok maken** een naam voor het notitieblok in. Selecteer **Scala** als taal en selecteer het Spark-cluster dat u eerder hebt gemaakt.

    ![Details opgeven voor een notebook in Databricks](./media/data-lake-storage-handle-data-using-databricks/databricks-notebook-details.png "Details opgeven voor een notebook in Databricks")

    Selecteer **Maken**.

1. Voer de volgende code in de eerste cel van het notebook in en voer de code uit. Vervang de tijdelijke aanduidingen tussen haken in het voorbeeld door uw eigen waarden:

    ```scala
    %python%
    configs = {"fs.azure.account.auth.type": "OAuth",
        "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
        "fs.azure.account.oauth2.client.id": "<service-client-id>",
        "fs.azure.account.oauth2.client.secret": "<service-credentials>",
        "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<tenant-id>/oauth2/token"}
     
    dbutils.fs.mount(
        source = "abfss://<file-system-name>@<account-name>.dfs.core.windows.net/[<directory-name>]",
        mount_point = "/mnt/<mount-name>",
        extra_configs = configs)
    ```

1. Selecteer de toetsen Shift+Enter om de code uit te voeren.

Nu is het bestandssysteem voor het opslagaccount gemaakt.

## <a name="upload-the-sample-data"></a>De voorbeeldgegevens uploaden

De volgende stap bestaat uit het uploaden van een voorbeeldgegevensbestand naar het opslagaccount om later te transformeren in Azure Databricks.

Upload de voorbeeldgegevens die u hebt gedownload naar het opslagaccount. De methode die u gebruikt om de gegevens te uploaden naar het opslagaccount is afhankelijk van of u de hiërarchische naamruimte hebt ingeschakeld.

Voor het uploaden kunt u Azure Data Factory, distp of AzCopy (versie 10) gebruiken. AzCopy-versie 10 is momenteel alleen als preview beschikbaar. Plak om AzCopy te gebruiken de volgende code in een opdrachtvenster:

```bash
set ACCOUNT_NAME=<ACCOUNT_NAME>
set ACCOUNT_KEY=<ACCOUNT_KEY>
azcopy cp "<DOWNLOAD_PATH>\small_radio_json.json" https://<ACCOUNT_NAME>.dfs.core.windows.net/data --recursive 
```

## <a name="extract-the-data"></a>De gegevens extraheren

Als u met de voorbeeldgegevens in Databricks wilt werken, moet u de gegevens uit uw opslagaccount extraheren.

Ga terug naar uw Databricks-notebook en voer de volgende code in een nieuwe cel in het notebook in.

Voeg het volgende codefragment in een lege codecel in. Vervang de tijdelijke aanduidingen tussen de punthaken door de waarden die u eerder hebt opgeslagen in het opslagaccount.

```scala
dbutils.widgets.text("storage_account_name", "STORAGE_ACCOUNT_NAME", "<YOUR_STORAGE_ACCOUNT_NAME>")
dbutils.widgets.text("storage_account_access_key", "YOUR_ACCESS_KEY", "<YOUR_STORAGE_ACCOUNT_SHARED_KEY>")
```

Selecteer de toetsen Shift+Enter om de code uit te voeren.

U kunt nu het JSON-voorbeeldbestand laden als een dataframe in Azure Databricks. Plak de volgende code in de nieuwe cel. Vervang de tijdelijke aanduidingen tussen haken door uw eigen waarden.

```scala
val df = spark.read.json("abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/data/small_radio_json.json")
```

Selecteer de toetsen Shift+Enter om de code uit te voeren.

Voer de volgende code uit om de inhoud van het dataframe weer te geven:

```scala
df.show()
```

Het volgende (of een vergelijkbaar) codefragment wordt weergegeven:

```bash
+---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
|               artist|     auth|firstName|gender|itemInSession|  lastName|   length|  level|            location|method|    page| registration|sessionId|                song|status|           ts|userId|
+---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
| El Arrebato         |Logged In| Annalyse|     F|            2|Montgomery|234.57914| free  |  Killeen-Temple, TX|   PUT|NextSong|1384448062332|     1879|Quiero Quererte Q...|   200|1409318650332|   309|
| Creedence Clearwa...|Logged In|   Dylann|     M|            9|    Thomas|340.87138| paid  |       Anchorage, AK|   PUT|NextSong|1400723739332|       10|        Born To Move|   200|1409318653332|    11|
| Gorillaz            |Logged In|     Liam|     M|           11|     Watts|246.17751| paid  |New York-Newark-J...|   PUT|NextSong|1406279422332|     2047|                DARE|   200|1409318685332|   201|
...
...
```

U hebt de gegevens nu geëxtraheerd uit Azure Data Lake Store Gen2 en geladen in Azure Databricks.

## <a name="transform-the-data"></a>De gegevens transformeren

Het bestand **small_radio_json.json** met de onbewerkte voorbeeldgegevensset legt de doelgroep voor een radiozender vast en bestaat uit een aantal kolommen. In deze sectie gaat u de gegevens zodanig transformeren dat alleen bepaalde kolommen uit de gegevensset worden opgehaald.

Haal eerst alleen de kolommen **firstName**, **lastName**, **gender**, **location** en **level** op uit het dataframe dat u hebt gemaakt.

```scala
val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
```

De uitvoer die u ontvangt, wordt weergegeven in het volgende codefragment:

```bash
+---------+----------+------+--------------------+-----+
|firstname|  lastname|gender|            location|level|
+---------+----------+------+--------------------+-----+
| Annalyse|Montgomery|     F|  Killeen-Temple, TX| free|
|   Dylann|    Thomas|     M|       Anchorage, AK| paid|
|     Liam|     Watts|     M|New York-Newark-J...| paid|
|     Tess|  Townsend|     F|Nashville-Davidso...| free|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
|     Alan|     Morse|     M|Chicago-Napervill...| paid|
|Gabriella|   Shelton|     F|San Jose-Sunnyval...| free|
|   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
|     Tess|  Townsend|     F|Nashville-Davidso...| free|
|     Alan|     Morse|     M|Chicago-Napervill...| paid|
|     Liam|     Watts|     M|New York-Newark-J...| paid|
|     Liam|     Watts|     M|New York-Newark-J...| paid|
|   Dylann|    Thomas|     M|       Anchorage, AK| paid|
|     Alan|     Morse|     M|Chicago-Napervill...| paid|
|   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
|     Alan|     Morse|     M|Chicago-Napervill...| paid|
|   Dylann|    Thomas|     M|       Anchorage, AK| paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
+---------+----------+------+--------------------+-----+
```

U kunt deze gegevens nog verder transformeren door de naam van de kolom **level** te wijzigen in **subscription_type**.

```scala
val renamedColumnsDF = specificColumnsDf.withColumnRenamed("level", "subscription_type")
renamedColumnsDF.show()
```

De uitvoer die u ontvangt, wordt weergegeven in het volgende codefragment.

```bash
+---------+----------+------+--------------------+-----------------+
|firstname|  lastname|gender|            location|subscription_type|
+---------+----------+------+--------------------+-----------------+
| Annalyse|Montgomery|     F|  Killeen-Temple, TX|             free|
|   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
|     Liam|     Watts|     M|New York-Newark-J...|             paid|
|     Tess|  Townsend|     F|Nashville-Davidso...|             free|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
|     Alan|     Morse|     M|Chicago-Napervill...|             paid|
|Gabriella|   Shelton|     F|San Jose-Sunnyval...|             free|
|   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
|     Tess|  Townsend|     F|Nashville-Davidso...|             free|
|     Alan|     Morse|     M|Chicago-Napervill...|             paid|
|     Liam|     Watts|     M|New York-Newark-J...|             paid|
|     Liam|     Watts|     M|New York-Newark-J...|             paid|
|   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
|     Alan|     Morse|     M|Chicago-Napervill...|             paid|
|   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
|     Alan|     Morse|     M|Chicago-Napervill...|             paid|
|   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
+---------+----------+------+--------------------+-----------------+
```

## <a name="load-the-data"></a>De gegevens laden

In deze sectie uploadt u de getransformeerde gegevens naar Azure SQL Data Warehouse. Gebruik de Azure SQL Data Warehouse-connector voor Azure Databricks om een dataframe rechtstreeks als een tabel in een SQL-datawarehouse te uploaden.

De SQL Data Warehouse-connector maakt gebruik van Azure Blob Storage als tijdelijke opslag voor het uploaden van gegevens tussen Azure Databricks en Azure SQL Data Warehouse. U begint met het opgeven van de configuratie om verbinding te maken met het opslagaccount. U moet het account al hebben gemaakt als onderdeel van de vereisten voor dit artikel.

Geef de configuratie op voor toegang tot het Azure Storage-account vanuit Azure Databricks.

```scala
val storageURI = "<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net"
val fileSystemName = "<FILE_SYSTEM_NAME>"
val accessKey =  "<ACCESS_KEY>"
```

Geef een tijdelijke map op die wordt gebruikt tijdens het verplaatsen van gegevens tussen Azure Databricks en Azure SQL Data Warehouse.

```scala
val tempDir = "abfs://" + fileSystemName + "@" + storageURI +"/tempDirs"
```

Voer het volgende codefragment uit om toegangssleutels voor Azure Blob-opslag op te slaan in de configuratie. Deze actie zorgt ervoor dat u de toegangssleutel niet als gewone tekst in het notebook hoeft te bewaren.

```scala
val acntInfo = "fs.azure.account.key."+ storageURI
sc.hadoopConfiguration.set(acntInfo, accessKey)
```

Geef de waarden op om verbinding te maken met de Azure SQL Data Warehouse-instantie. U moet als vereiste een SQL-datawarehouse hebben gemaakt.

```scala
//SQL Data Warehouse related settings
val dwDatabase = "<DATABASE NAME>"
val dwServer = "<DATABASE SERVER NAME>" 
val dwUser = "<USER NAME>"
val dwPass = "<PASSWORD>"
val dwJdbcPort =  "1433"
val dwJdbcExtraOptions = "encrypt=true;trustServerCertificate=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
val sqlDwUrl = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass + ";$dwJdbcExtraOptions"
val sqlDwUrlSmall = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass
```

Voer het volgende codefragment uit om het getransformeerde dataframe, **renamedColumnsDF**, als een tabel te laden in een SQL-datawarehouse. Met dit fragment wordt een tabel met de naam **SampleTable** gemaakt in de SQL-database.

```scala
spark.conf.set(
    "spark.sql.parquet.writeLegacyFormat",
    "true")
    
renamedColumnsDF.write
    .format("com.databricks.spark.sqldw")
    .option("url", sqlDwUrlSmall) 
    .option("dbtable", "SampleTable")
    .option( "forward_spark_azure_storage_credentials","True")
    .option("tempdir", tempDir)
    .mode("overwrite")
    .save()
```

Maak verbinding met de SQL-database en controleer of u de database **SampleTable** ziet.

![De voorbeeldtabel verifiëren](./media/data-lake-storage-handle-data-using-databricks/verify-sample-table.png "Voorbeeldtabel verifiëren")

Voer een Select-query uit om de inhoud van de tabel te controleren. De tabel moet dezelfde gegevens bevatten als het dataframe **renamedColumnsDF**.

![De inhoud van de voorbeeldtabel controleren](./media/data-lake-storage-handle-data-using-databricks/verify-sample-table-content.png "De inhoud van de voorbeeldtabel controleren")

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u de zelfstudie hebt voltooid, kunt u het cluster beëindigen. Selecteer links **Clusters** vanuit de Azure Databricks-werkruimte. Als u het cluster wilt beëindigen, wijst u onder **Acties** het beletselteken (...) aan en selecteert u het pictogram **Beëindigen**.

![Een Databricks-cluster stopzetten](./media/data-lake-storage-handle-data-using-databricks/terminate-databricks-cluster.png "Een Databricks-cluster stopzetten")

Als u het cluster niet handmatig beëindigt, stopt het cluster automatisch, op voorwaarde dat het selectievakje **Beëindigen na \_\_ minuten van inactiviteit** is ingeschakeld tijdens het maken van het cluster. In dat geval stopt het cluster automatisch als het gedurende de opgegeven tijd inactief is geweest.

## <a name="next-steps"></a>Volgende stappen

Ga naar de volgende zelfstudie voor informatie over het streamen van realtime gegevens naar Azure Databricks met behulp van Azure Event Hubs.

> [!div class="nextstepaction"]
>[Gegevens streamen naar Azure Databricks met behulp van Event Hubs](../../azure-databricks/databricks-stream-from-eventhubs.md)
