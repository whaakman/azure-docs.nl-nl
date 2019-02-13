---
title: 'Zelfstudie: ETL-bewerkingen uitvoeren met behulp van Azure Databricks'
description: Leer gegevens op te halen uit Data Lake Storage Gen2 en over te dragen naar Azure Databricks, de gegevens te transformeren en ze vervolgens in Azure SQL Data Warehouse te laden.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.custom: mvc
ms.topic: tutorial
ms.workload: Active
ms.date: 01/24/2019
ms.openlocfilehash: c8b97ed5a1e1131e073783300a233ef72ef34110
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55818648"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-azure-databricks"></a>Zelfstudie: Gegevens extraheren, transformeren en laden met Azure Databricks

In deze zelfstudie voert u een ETL-bewerking (Extraction, Transformation, and Loading) uit met behulp van Azure Databricks. U haalt gegevens op uit Azure Data Lake Storage Gen2 en draagt deze over naar Azure Databricks, voert transformaties uit op de gegevens in Azure Databricks, waarna u de getransformeerde gegevens in Azure SQL Data Warehouse laadt.

Voor de stappen in deze zelfstudie wordt gebruik gemaakt van de SQL Data Warehouse-connector voor Azure Databricks om gegevens over te dragen naar Azure Databricks. Op zijn beurt gebruikt deze connector Azure Blob Storage als tijdelijke opslag voor de gegevens die worden overgebracht tussen een Azure Databricks-cluster en Azure SQL Data Warehouse.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een Azure Databricks-service maken.
> * Een Apache Spark-cluster in Azure Databricks maken.
> * Een bestandssysteem maken en gegevens uploaden naar Azure Data Lake Storage Gen2.
> * Een service-principal maken.
> * Gegevens extraheren uit de Data Lake Store.
> * Gegevens transformeren in Azure Databricks.
> * Gegevens laden in Azure SQL Data Warehouse.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voltooi deze taken voordat u aan deze zelfstudie begint:

* Maak een Azure SQL-datawarehouse, maak een firewallregel op serverniveau en maak verbinding met de server als serverbeheerder. Zie [Quickstart: Een Azure SQL Data Warehouse maken](../sql-data-warehouse/create-data-warehouse-portal.md).

* Maak een databasehoofdsleutel voor de Azure SQL-datawarehouse. Zie [Een databasehoofdsleutel maken](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key).

* Een Azure Data Lake Storage Gen2-account maken. Zie [Een Azure Data Lake Storage Gen2-account maken](../storage/blobs/data-lake-storage-quickstart-create-account.md).

* Maak een Azure Blob-opslagaccount met daarin een container. Haal ook de toegangssleutel op voor toegang tot het opslagaccount. Zie [Quickstart: Een Azure Blob Storage-account maken](../storage/blobs/storage-quickstart-blobs-portal.md).

* Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-an-azure-databricks-service"></a>Een Azure Databricks-service maken

In dit gedeelte gaat u een Azure Databricks-service maken met behulp van de Azure-portal.

1. Selecteer in Azure Portal **Een resource maken** > **Analyse** > **Azure Databricks**.

    ![Databricks in Azure Portal](./media/databricks-extract-load-sql-data-warehouse/azure-databricks-on-portal.png "Databricks in Azure Portal")

2. Geef bij **Azure Databricks Service** de volgende waarden op voor het maken van een Databricks-service:

    |Eigenschap  |Beschrijving  |
    |---------|---------|
    |**Werkruimtenaam**     | Geef een naam op voor de Databricks-werkruimte.        |
    |**Abonnement**     | Selecteer uw Azure-abonnement in de vervolgkeuzelijst.        |
    |**Resourcegroep**     | Geef aan of u een nieuwe resourcegroep wilt maken of een bestaande groep wilt gebruiken. Een resourcegroep is een container met gerelateerde resources voor een Azure-oplossing. Zie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie. |
    |**Locatie**     | Selecteer **US - west 2**.  Zie [Producten beschikbaar per regio](https://azure.microsoft.com/regions/services/) voor andere beschikbare regio's.      |
    |**Prijscategorie**     |  selecteer **Standaard**.     |

3. Selecteer **Vastmaken aan dashboard** en selecteer **Maken**.

4. Het duurt enkele minuten om het account te maken. Tijdens het maken van het account wordt rechts in de portal de tegel **Implementatie verzenden voor Azure Databricks** weergegeven. Bekijk de voortgangsbalk bovenaan om de bewerkingsstatus te volgen.

    ![Tegel Databricks-implementatie](./media/databricks-extract-load-sql-data-warehouse/databricks-deployment-tile.png "Tegel Databricks-implementatie")

## <a name="create-a-spark-cluster-in-azure-databricks"></a>Een Apache Spark-cluster in Azure Databricks maken

1. Ga in de Azure-portal naar de Databricks-service die u hebt gemaakt en selecteer **Werkruimte starten**.

2. U wordt omgeleid naar de Azure Databricks-portal. Klik in de portal op **Cluster**.

    ![Databricks in Azure](./media/databricks-extract-load-sql-data-warehouse/databricks-on-azure.png "Databricks in Azure")

3. Op de pagina **Nieuw cluster** geeft u de waarden op waarmee een nieuw cluster wordt gemaakt.

    ![Een Databricks Spark-cluster maken in Azure](./media/databricks-extract-load-sql-data-warehouse/create-databricks-spark-cluster.png "Een Databricks Spark-cluster maken in Azure")

4. Vul de waarden voor de volgende velden in (en laat bij de overige velden de standaardwaarden staan):

    * Voer een naam in voor het cluster.

    * Maak voor dit artikel een cluster met de **5.1**-runtime.

    * Zorg ervoor dat u het selectievakje **Beëindigen na \_\_ minuten van inactiviteit** inschakelt. Geef een duur (in minuten) op waarna het cluster moet worden beëindigd als het niet wordt gebruikt.

    * Selecteer **Cluster maken**. Als het cluster wordt uitgevoerd, kunt u notitieblokken koppelen aan het cluster en Apache Spark-taken uitvoeren.

## <a name="create-a-file-system-and-upload-sample-data"></a>Een bestandssysteem maken en voorbeeldgegevens uploaden

Eerst maakt u een bestandssysteem in uw Data Lake Storage Gen2-account. Vervolgens uploadt u een voorbeeldgegevensbestand naar Data Lake Store. Dit bestand gebruikt u later in Azure Databricks om enkele transformaties uit te voeren.

1. Download het voorbeeldgegevensbestand [small_radio_json.json](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json) naar uw lokale bestandssysteem.

2. Vanuit de [Microsoft Azure-portal](https://portal.azure.com/) navigeert u naar het Data Lake Storage Gen2-account dat u als een vereiste voor deze zelfstudie hebt gemaakt.

3. Selecteer vanaf de pagina **Overzicht** van het opslagaccount de optie **Openen in Explorer**.

   ![Storage Explorer openen](./media/databricks-extract-load-sql-data-warehouse/data-lake-storage-open-storage-explorer.png "Storage Explorer openen")

4. Selecteer **Azure Storage Explorer openen** om Storage Explorer te openen.

   ![Tweede prompt van Storage Explorer openen](./media/databricks-extract-load-sql-data-warehouse/data-lake-storage-open-storage-explorer-2.png "Tweede prompt van Storage Explorer openen")

   Storage Explorer wordt geopend. U kunt een bestandssysteem maken en de voorbeeldgegevens uploaden met behulp van de richtlijnen in dit onderwerp: [Snelstart: Azure Storage Explorer gebruiken voor het beheren van gegevens in een Azure Data Lake Storage Gen2-account](../storage/blobs/data-lake-storage-explorer.md).

<a id="service-principal"/>

## <a name="create-a-service-principal"></a>Een service-principal maken

Maak een service-principal aan de hand van de instructies in dit onderwerp: [Procedure: Gebruik de portal voor het maken van een Azure AD-toepassing en service-principal die toegang hebben tot resources](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

Er zijn een paar dingen die u moet doen terwijl u de stappen in het artikel uitvoert.

:heavy_check_mark: Als u de stappen gaat uitvoeren in de sectie [De toepassing aan een rol toewijzen](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role) van het artikel, moet u er voor zorgen dat u de toepassing toewijst aan de **rol van inzender voor Blob Storage**.

:heavy_check_mark: Als u de stappen gaat uitvoeren in de sectie [Waarden ophalen voor het aanmelden](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) van het artikel, plakt u de waarden van de tenant-id, de toepassings-id en de verificatiesleutel in een tekstbestand. U hebt deze binnenkort nodig.
Eerst maakt u een notitieblok in de Azure Databricks-werkruimte en vervolgens voert u codefragmenten uit om het bestandssysteem in uw opslagaccount te maken.

## <a name="extract-data-from-the-data-lake-store"></a>Gegevens extraheren uit de Data Lake Store

In deze sectie maakt u een notitieblok in de Azure Databricks-werkruimte en voert u codefragmenten uit om gegevens op te halen uit de Data Lake Store en deze vervolgens te laden in Azure Databricks.

1. Ga in de [Azure-portal](https://portal.azure.com) naar de Azure Databricks-service die u hebt gemaakt en selecteer **Werkruimte starten**.

2. Selecteer aan de linkerkant **Werkruimte**. Selecteer in de **Werkruimte**-vervolgkeuzelijst, **Notitieblok** > **maken**.

    ![Een notitieblok maken in Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-create-notebook.png "Een notitieblok maken in Databricks")

3. Voer in het dialoogvenster **Notitieblok maken** een naam voor het notitieblok in. Selecteer **Scala** als taal en selecteer het Spark-cluster dat u eerder hebt gemaakt.

    ![Details opgeven voor een notebook in Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-notebook-details.png "Details opgeven voor een notebook in Databricks")

4. Selecteer **Maken**.

5. Kopieer en plak het volgende codeblok in de eerste cel.

   ```scala
   spark.conf.set("fs.azure.account.auth.type.<storage-account-name>.dfs.core.windows.net", "OAuth")
   spark.conf.set("fs.azure.account.oauth.provider.type.<storage-account-name>.dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
   spark.conf.set("fs.azure.account.oauth2.client.id.<storage-account-name>.dfs.core.windows.net", "<application-id>")
   spark.conf.set("fs.azure.account.oauth2.client.secret.<storage-account-name>.dfs.core.windows.net", "<authentication-key>")
   spark.conf.set("fs.azure.account.oauth2.client.endpoint.<storage-account-name>.dfs.core.windows.net", "https://login.microsoftonline.com/<tenant-id>/oauth2/token")
   ```

6. In dit codeblok vervangt u de tijdelijke aanduidingen `application-id`, `authentication-id` en `tenant-id` door de waarden die u hebt verkregen bij het uitvoeren van de stappen in [Configuratie opslagaccount instellen](#config). Vervang de waarde van de tijdelijke plaatsaanduiding `storage-account-name` door de naam van uw opslagaccount.

7. Druk op de toetsen **Shift + Enter** om de code in dit blok uit te voeren.

8. U kunt nu het JSON-voorbeeldbestand laden als een dataframe in Azure Databricks. Plak de volgende code in de nieuwe cel. Vervang de tijdelijke aanduidingen tussen haken door uw eigen waarden.

   ```scala
   val df = spark.read.json("abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/small_radio_json.json")
   ```

   * Vervang de tijdelijke-aanduidingswaarde `file-system-name` door de naam die u uw bestandssysteem in Storage Explorer hebt gegeven.

   * Vervang de tijdelijke plaatsaanduiding `storage-account-name` door de naam van uw opslagaccount.

9. Druk op de toetsen **Shift + Enter** om de code in dit blok uit te voeren.

10. Voer de volgende code uit om de inhoud van het dataframe weer te geven:

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

Het bestand **small_radio_json.json** met de onbewerkte voorbeeldgegevensset legt de doelgroep voor een radiozender vast en bestaat uit een aantal kolommen. In deze sectie gaat u de gegevens zodanig transformeren dat alleen bepaalde kolommen uit de gegevensset worden opgehaald.

1. Haal eerst alleen de kolommen **firstName**, **lastName**, **gender**, **location** en **level** op uit het dataframe dat u hebt gemaakt.

   ```scala
   val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
   specificColumnsDf.show()
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

2. U kunt deze gegevens nog verder transformeren door de naam van de kolom **level** te wijzigen in **subscription_type**.

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

## <a name="load-data-into-azure-sql-data-warehouse"></a>Gegevens laden in Azure SQL Data Warehouse

In deze sectie uploadt u de getransformeerde gegevens naar Azure SQL Data Warehouse. Gebruik de Azure SQL Data Warehouse-connector voor Azure Databricks om een dataframe rechtstreeks als een tabel in een SQL-datawarehouse te uploaden.

Zoals eerder vermeld, maakt de SQL Data Warehouse-connector gebruik van Azure Blob Storage als tijdelijke opslag voor het uploaden van gegevens tussen Azure Databricks en Azure SQL Data Warehouse. U begint met het opgeven van de configuratie om verbinding te maken met het opslagaccount. U moet het account al hebben gemaakt als onderdeel van de vereisten voor dit artikel.

1. Geef de configuratie op voor toegang tot het Azure Storage-account vanuit Azure Databricks.

   ```scala
   val blobStorage = "<blob-storage-account-name>.blob.core.windows.net"
   val blobContainer = "<blob-container-name>"
   val blobAccessKey =  "<access-key>"
   ```

2. Geef een tijdelijke map op die wordt gebruikt tijdens het verplaatsen van gegevens tussen Azure Databricks en Azure SQL Data Warehouse.

   ```scala
   val tempDir = "wasbs://" + blobContainer + "@" + blobStorage +"/tempDirs"
   ```

3. Voer het volgende codefragment uit om toegangssleutels voor Azure Blob-opslag op te slaan in de configuratie. Deze actie zorgt ervoor dat u de toegangssleutel niet als gewone tekst in het notebook hoeft te bewaren.

   ```scala
   val acntInfo = "fs.azure.account.key."+ blobStorage
   sc.hadoopConfiguration.set(acntInfo, blobAccessKey)
   ```

4. Geef de waarden op om verbinding te maken met de Azure SQL Data Warehouse-instantie. U moet als vereiste een SQL-datawarehouse hebben gemaakt.

   ```scala
   //SQL Data Warehouse related settings
   val dwDatabase = "<database-name>"
   val dwServer = "<database-server-name>"
   val dwUser = "<user-name>"
   val dwPass = "<password>"
   val dwJdbcPort =  "1433"
   val dwJdbcExtraOptions = "encrypt=true;trustServerCertificate=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
   val sqlDwUrl = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass + ";$dwJdbcExtraOptions"
   val sqlDwUrlSmall = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass
   ```

5. Voer het volgende codefragment uit om het getransformeerde dataframe, **renamedColumnsDF**, als een tabel te laden in een SQL-datawarehouse. Met dit fragment wordt een tabel met de naam **SampleTable** gemaakt in de SQL-database.

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

6. Maak verbinding met de SQL-database en controleer of u de database **SampleTable** ziet.

   ![De voorbeeldtabel verifiëren](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table.png "Voorbeeldtabel verifiëren")

7. Voer een Select-query uit om de inhoud van de tabel te controleren. De tabel moet dezelfde gegevens bevatten als het dataframe **renamedColumnsDF**.

    ![De inhoud van de voorbeeldtabel controleren](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table-content.png "De inhoud van de voorbeeldtabel controleren")

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u de zelfstudie hebt voltooid, kunt u het cluster beëindigen. Selecteer links **Clusters** vanuit de Azure Databricks-werkruimte. Als u het cluster wilt beëindigen, wijst u onder **Acties** het beletselteken (...) aan en selecteert u het pictogram **Beëindigen**.

![Een Databricks-cluster stopzetten](./media/databricks-extract-load-sql-data-warehouse/terminate-databricks-cluster.png "Een Databricks-cluster stopzetten")

Als u het cluster niet handmatig beëindigt, stopt het cluster automatisch, op voorwaarde dat het selectievakje **Beëindigen na \_\_ minuten van inactiviteit** is ingeschakeld tijdens het maken van het cluster. In dat geval stopt het cluster automatisch als het gedurende de opgegeven tijd inactief is geweest.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een Azure Databricks-service maken
> * Een Apache Spark-cluster in Azure Databricks maken
> * Een notitieblok maken in Azure Databricks
> * Gegevens extraheren uit een Data Lake Storage Gen2-account
> * Gegevens transformeren in Azure Databricks
> * Gegevens laden in Azure SQL Data Warehouse

Ga naar de volgende zelfstudie voor informatie over het streamen van realtime gegevens naar Azure Databricks met behulp van Azure Event Hubs.

> [!div class="nextstepaction"]
>[Gegevens streamen naar Azure Databricks met behulp van Event Hubs](databricks-stream-from-eventhubs.md)
