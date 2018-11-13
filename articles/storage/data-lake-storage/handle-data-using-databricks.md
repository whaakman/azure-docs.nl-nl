---
title: Meer informatie over extraheren, laden en overdragen met Azure Databricks
description: Leer hoe u gegevens uit Data Lake Store Storage Gen2 Preview extraheert en overdraagt naar Azure Databricks, de gegevens transformeert en vervolgens laadt in Azure SQL Data Warehouse.
services: azure-databricks
ms.service: azure-databricks
author: jamesbak
ms.author: jamesbak
ms.reviewer: jasonwhowell
ms.custom: mvc
ms.topic: tutorial
ms.date: 06/27/2018
ms.openlocfilehash: 5c1684f7367aec2d283cd6ad310657def49dd3cb
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282557"
---
# <a name="tutorial-extract-transform-and-load-data-using-azure-databricks"></a>Zelfstudie: Gegevens extraheren, transformeren en laden met behulp van Azure Databricks

In deze zelfstudie voert u een ETL-bewerking uit (gegevens extraheren, transformeren en laden) om gegevens uit een Azure-opslagaccount waarvoor Azure Data Lake Storage Gen2 is ingeschakeld te verplaatsen naar Azure SQL Data Warehouse, met behulp van Azure Databricks.

In de volgende afbeelding wordt de stroom van de toepassing weergegeven:

![Azure Databricks met Data Lake Store Gen2 en SQL Data Warehouse](./media/handle-data-using-databricks/databricks-extract-transform-load-sql-datawarehouse.png "Azure Databricks met Data Lake Store Gen2 en SQL Data Warehouse")

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een Azure Databricks-werkruimte maken
> * Een Apache Spark-cluster in Azure Databricks maken
> * Een voor Azure Data Lake Storage Gen2 geschikt account maken
> * Gegevens uploaden naar Azure Data Lake Storage Gen2
> * Een notitieblok maken in Azure Databricks
> * Gegevens extraheren uit Data Lake Store Gen2
> * Gegevens transformeren in Azure Databricks
> * Gegevens laden in Azure SQL Data Warehouse

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

* Maak een Azure SQL Data Warehouse, maak een firewallregel op serverniveau en maak verbinding met de server als serverbeheerder. Volg de instructies bij [Snelstart: Een Azure SQL-datawarehouse maken](../../sql-data-warehouse/create-data-warehouse-portal.md)
* Maak een databasehoofdsleutel voor Azure SQL Data Warehouse. Volg de instructies bij [Een databasehoofdsleutel maken](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key).
* [Een Azure Data Lake Storage Gen2-account maken](quickstart-create-account.md)

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-an-azure-databricks-workspace"></a>Een Azure Databricks-werkruimte maken

In deze sectie gaat u een Azure Databricks-werkruimte maken met behulp van Azure Portal.

1. Selecteer in Azure Portal **Een resource maken** > **Analyse** > **Azure Databricks**.

    ![Databricks in Azure Portal](./media/handle-data-using-databricks/azure-databricks-on-portal.png "Databricks in Azure Portal")

2. Geef bij **Azure Databricks Service** de waarden op voor het maken van een Databricks-werkruimte.

    ![Een Azure Databricks-werkruimte maken](./media/handle-data-using-databricks/create-databricks-workspace.png "Een Azure Databricks-werkruimte maken")

    Geef de volgende waarden op:

    |Eigenschap  |Beschrijving  |
    |---------|---------|
    |**Werkruimtenaam**     | Geef een naam op voor de Databricks-werkruimte.        |
    |**Abonnement**     | Selecteer uw Azure-abonnement in de vervolgkeuzelijst.        |
    |**Resourcegroep**     | Geef aan of u een nieuwe resourcegroep wilt maken of een bestaande groep wilt gebruiken. Een resourcegroep is een container met gerelateerde resources voor een Azure-oplossing. Zie [Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) voor meer informatie. |
    |**Locatie**     | Selecteer **US - west 2**. Zie [Producten beschikbaar per regio](https://azure.microsoft.com/regions/services/) voor andere beschikbare regio's.        |
    |**Prijscategorie**     |  U kunt kiezen tussen **Standard** en **Premium**. Bekijk de pagina [Prijzen voor Databricks](https://azure.microsoft.com/pricing/details/databricks/) voor meer informatie over deze categorieën.       |

    Selecteer **Vastmaken aan dashboard** en selecteer **Maken**.

3. Het duurt enkele minuten om het account te maken. Tijdens het maken van het account wordt rechts in de portal de tegel **Implementatie verzenden voor Azure Databricks** weergegeven. Mogelijk moet u op uw dashboard naar rechts scrollen om de tegel te zien. Bovenaan het scherm wordt ook een voortgangsbalk weergegeven. U kunt beide gebieden bekijken voor de voortgang.

    ![Tegel Databricks-implementatie](./media/handle-data-using-databricks/databricks-deployment-tile.png "Tegel Databricks-implementatie")

## <a name="create-a-spark-cluster-in-databricks"></a>Een Spark-cluster maken in Databricks

1. Ga in Azure Portal naar de Databricks-werkruimte die u hebt gemaakt en selecteer **Werkruimte starten**.

2. U wordt omgeleid naar de Azure Databricks-portal. Klik in de portal op **Cluster**.

    ![Databricks in Azure](./media/handle-data-using-databricks/databricks-on-azure.png "Databricks in Azure")

3. Op de pagina **Nieuw cluster** geeft u de waarden op waarmee een nieuw cluster wordt gemaakt.

    ![Een Databricks Spark-cluster maken in Azure](./media/handle-data-using-databricks/create-databricks-spark-cluster.png "Een Databricks Spark-cluster maken in Azure")

    Vul de waarden voor de volgende velden in (en laat bij de overige velden de standaardwaarden staan):

    * Voer een naam in voor het cluster.
    * Maak voor dit artikel een cluster met een **4.2**-runtime.
    * Zorg ervoor dat u het selectievakje **Beëindigen na\_\_ minuten van inactiviteit** inschakelt. Geef een duur (in minuten) op waarna het cluster moet worden beëindigd als het niet wordt gebruikt.

    Selecteer **Cluster maken**. Zodra het cluster wordt uitgevoerd, kunt u notitieblokken koppelen aan het cluster en Spark-taken uitvoeren.

## <a name="create-storage-account-file-system"></a>Bestandssysteem voor opslagaccount maken

In deze sectie maakt u een notitieblok in de Azure Databricks-werkruimte en voert u vervolgens codefragmenten uit om het opslagaccount te configureren.

1. Ga in [Azure Portal](https://portal.azure.com) naar de Azure Databricks-werkruimte die u hebt gemaakt en selecteer **Werkruimte starten**.

2. Selecteer **Werkruimte** in het linkerdeelvenster. Selecteer in de **Werkruimte**-vervolgkeuzelijst, **Notitieblok** > **maken**.

    ![Een notitieblok maken in Databricks](./media/handle-data-using-databricks/databricks-create-notebook.png "Een notitieblok maken in Databricks")

3. Voer in het dialoogvenster **Notitieblok maken** een naam voor het notitieblok in. Selecteer **Scala** als taal en selecteer het Spark-cluster dat u eerder hebt gemaakt.

    ![Een notitieblok maken in Databricks](./media/handle-data-using-databricks/databricks-notebook-details.png "Een notitieblok maken in Databricks")

    Selecteer **Maken**.

4. Voer de volgende code in de eerste cel in en voer de code uit:

    ```scala
    spark.conf.set("fs.azure.account.key.<ACCOUNT_NAME>.dfs.core.windows.net", "<ACCOUNT_KEY>") 
    spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "true")
    dbutils.fs.ls("abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/")
    spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "false") 
    ```

    Druk op **SHIFT+ENTER** om de codecel uit te voeren.

    Nu is het bestandssysteem voor het opslagaccount gemaakt.

## <a name="upload-data-to-the-storage-account"></a>Gegevens uploaden in het opslagaccount

De volgende stap is het uploaden van een voorbeeldbestand naar het opslagaccount om later te transformeren in Azure Databricks. 

> [!NOTE]
> Als u nog geen account hebt dat geschikt is voor Azure Data Lake Storage Gen2, volgt u de [snelstart om er een te maken](./quickstart-create-account.md).

1. Download (**small_radio_json.json**) van de opslagplaatsen [U-SQL-voorbeelden en problemen bijhouden](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json) en noteer het pad waar u het bestand opslaat.

2. Upload de voorbeeldgegevens vervolgens naar het opslagaccount. De methode die u gebruikt om gegevens te uploaden naar het opslagaccount, verschilt afhankelijk van of u de hiërarchische naamruimte hebt ingeschakeld.

    Als de hiërarchische naamruimte is ingeschakeld in uw Azure Storage-account, kunt u Azure Data Factory, distp of AzCopy (versie 10) gebruiken om de upload te verwerken. AzCopy-versie 10 is momenteel alleen beschikbaar via de preview. Plak om AzCopy te gebruiken de volgende code in een opdrachtvenster:

    ```bash
    set ACCOUNT_NAME=<ACCOUNT_NAME>
    set ACCOUNT_KEY=<ACCOUNT_KEY>
    azcopy cp "<DOWNLOAD_PATH>\small_radio_json.json" https://<ACCOUNT_NAME>.dfs.core.windows.net/data --recursive 
    ```
    
## <a name="extract-data-from-azure-storage"></a>Gegevens extraheren uit Azure Storage

Ga terug naar DataBricks Notebook en voer de volgende code in een nieuwe cel in:

1. Voeg het volgende fragment toe aan een lege codecel en vervang de tijdelijke aanduidingen door de waarden uit het opslagaccount die u eerder hebt opgeslagen.

    ```scala
    dbutils.widgets.text("storage_account_name", "STORAGE_ACCOUNT_NAME", "<YOUR_STORAGE_ACCOUNT_NAME>")
    dbutils.widgets.text("storage_account_access_key", "YOUR_ACCESS_KEY", "<YOUR_STORAGE_ACCOUNT_SHARED_KEY>")
    ```

    Druk op **SHIFT+ENTER** om de codecel uit te voeren.

2. U kunt nu het JSON-voorbeeldbestand laden als een dataframe in Azure Databricks. Plak de volgende code in een nieuwe cel en druk vervolgens op **SHIFT+ENTER** (zorg ervoor dat u de tijdelijke aanduidingen hebt vervangen):

    ```scala
    val df = spark.read.json("abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/data/small_radio_json.json")
    ```

3. Voer de volgende code uit om de inhoud van het dataframe weer te geven.

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

## <a name="transform-data-in-azure-databricks"></a>Gegevens transformeren in Azure Databricks

De onbewerkte voorbeeldgegevensset **small_radio_json.json** legt de doelgroep voor een radiozender vast en bestaat uit een aantal kolommen. In deze sectie gaat u de gegevens zodanig transformeren dat alleen bepaalde kolommen uit de gegevensset worden opgehaald.

1. Begin met het ophalen van alleen de kolommen *firstName*, *lastName*, *gender*, *location* en *level* uit het dataframe dat u al hebt gemaakt.

    ```scala
    val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
    ```

    De volgende uitvoer wordt weergegeven:

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

2.  U kunt deze gegevens nog verder transformeren door de naam van de kolom **level** te wijzigen in **subscription_type**.

    ```scala
    val renamedColumnsDF = specificColumnsDf.withColumnRenamed("level", "subscription_type")
    renamedColumnsDF.show()
    ```

    De volgende uitvoer wordt weergegeven.

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

## <a name="load-data-into-azure-sql-data-warehouse"></a>Gegevens laden in Azure SQL Data Warehouse

In deze sectie uploadt u de getransformeerde gegevens naar Azure SQL Data Warehouse. Met de Azure SQL Data Warehouse-connector voor Azure Databricks kunt u een dataframe rechtstreeks uploaden als tabel in SQL Data Warehouse.

Zoals eerder vermeld, maakt de SQL Data Warehouse-connector gebruik van Azure Blob Storage als tijdelijke opslag voor het uploaden van gegevens tussen Azure Databricks en Azure SQL Data Warehouse. U begint met het opgeven van de configuratie om verbinding te maken met het opslagaccount. U moet het account al hebben gemaakt als onderdeel van de vereisten voor dit artikel.

1. Geef de configuratie op voor toegang tot het Azure Storage-account vanuit Azure Databricks.

    ```scala
    val storageURI = "<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net"
    val fileSystemName = "<FILE_SYSTEM_NAME>"
    val accessKey =  "<ACCESS_KEY>"
    ```

2. Geef een tijdelijke map op die wordt gebruikt tijdens het verplaatsen van gegevens tussen Azure Databricks en Azure SQL Data Warehouse.

    ```scala
    val tempDir = "abfs://" + fileSystemName + "@" + storageURI +"/tempDirs"
    ```

3. Voer het volgende codefragment uit om toegangssleutels voor Azure Blob-opslag op te slaan in de configuratie. Daardoor hoeft u de toegangssleutel niet als gewone tekst in het notitieblok te bewaren.

    ```scala
    val acntInfo = "fs.azure.account.key."+ storageURI
    sc.hadoopConfiguration.set(acntInfo, accessKey)
    ```

4. Geef de waarden op om verbinding te maken met de Azure SQL Data Warehouse-instantie. U moet een SQL-datawarehouse hebben gemaakt als onderdeel van de vereisten.

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

5. Voer het volgende codefragment uit om het getransformeerde dataframe, **renamedColumnsDF**, als tabel te laden in SQL Data Warehouse. Met dit fragment wordt een tabel met de naam **SampleTable** gemaakt in de SQL-database.

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

6. Maak verbinding met de SQL-database en controleer of u de **SampleTable** ziet.

    ![Voorbeeldtabel verifiëren](./media/handle-data-using-databricks/verify-sample-table.png "Voorbeeldtabel verifiëren")

7. Voer een Select-query uit om de inhoud van de tabel te controleren. Deze moet dezelfde gegevens bevatten als het **renamedColumnsDF**-dataframe.

    ![Inhoud van voorbeeldtabel controleren](./media/handle-data-using-databricks/verify-sample-table-content.png "Inhoud van voorbeeldtabel controleren")

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u de zelfstudie hebt voltooid, kunt u het cluster beëindigen. Dit doet u door vanuit de Azure Databricks-werkruimte in het linkerdeelvenster **Clusters** te selecteren. Voor het cluster dat u wilt beëindigen, plaatst u de cursor op het weglatingsteken onder de kolom **Acties** en selecteert u het **beëindigingspictogram**.

![Een Databricks-cluster stopzetten](./media/handle-data-using-databricks/terminate-databricks-cluster.png "Een Databricks-cluster stopzetten")

Als u het cluster niet handmatig beëindigt, stopt het cluster automatisch, op voorwaarde dat het selectievakje **Beëindigen na __ minuten inactiviteit** is ingeschakeld tijdens het maken van het cluster. In dat geval stopt het cluster automatisch als het gedurende de opgegeven tijd inactief is geweest.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een Azure Databricks-werkruimte maken
> * Een Apache Spark-cluster in Azure Databricks maken
> * Een voor Azure Data Lake Storage Gen2 geschikt account maken
> * Gegevens uploaden naar Azure Data Lake Storage Gen2
> * Een notitieblok maken in Azure Databricks
> * Gegevens extraheren uit Data Lake Store Gen2
> * Gegevens transformeren in Azure Databricks
> * Gegevens laden in Azure SQL Data Warehouse

Ga naar de volgende zelfstudie voor informatie over het streamen van realtime gegevens naar Azure Databricks met behulp van Azure Event Hubs.

> [!div class="nextstepaction"]
>[Gegevens streamen naar Azure Databricks met behulp van Event Hubs](../../azure-databricks/databricks-stream-from-eventhubs.md)
