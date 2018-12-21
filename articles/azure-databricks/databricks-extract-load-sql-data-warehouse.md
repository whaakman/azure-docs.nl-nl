---
title: 'Zelfstudie: ETL-bewerkingen uitvoeren met behulp van Azure Databricks'
description: Leer gegevens op te halen uit Data Lake Store en over te dragen naar Azure Databricks, de gegevens te transformeren en ze vervolgens in Azure SQL Data Warehouse te laden.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.custom: mvc
ms.topic: tutorial
ms.workload: Active
ms.date: 11/19/2018
ms.openlocfilehash: 48b2cdb26994d01dfced8216bb70493802f672a7
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53413673"
---
# <a name="tutorial-extract-transform-and-load-data-using-azure-databricks"></a>Zelfstudie: Gegevens uitpakken, transformeren en laden met Azure Databricks

In deze zelfstudie voert u een ETL-bewerking (Extraction, Transformation, and Loading) uit met behulp van Azure Databricks. U haalt gegevens op uit Azure Data Lake Store en draagt deze over naar Azure Databricks, voert transformaties uit op de gegevens in Azure Databricks, waarna u de getransformeerde gegevens in Azure SQL Data Warehouse laadt.

Voor de stappen in deze zelfstudie wordt gebruik gemaakt van de SQL Data Warehouse-connector voor Azure Databricks om gegevens over te dragen naar Azure Databricks. Op zijn beurt gebruikt deze connector Azure Blob Storage als tijdelijke opslag voor de gegevens die worden overgebracht tussen een Azure Databricks-cluster en Azure SQL Data Warehouse.

In de volgende afbeelding wordt de stroom van de toepassing weergegeven:

![Azure Databricks met Data Lake Store en SQL Data Warehouse](./media/databricks-extract-load-sql-data-warehouse/databricks-extract-transform-load-sql-datawarehouse.png "Azure Databricks met Data Lake Store en SQL Data Warehouse")

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een Azure Databricks-werkruimte maken
> * Een Apache Spark-cluster in Azure Databricks maken
> * Een Azure Data Lake Store-account maken
> * Gegevens uploaden naar Azure Data Lake Store
> * Een notitieblok maken in Azure Databricks
> * Gegevens ophalen uit Data Lake Store
> * Gegevens transformeren in Azure Databricks
> * Gegevens laden in Azure SQL Data Warehouse

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u aan de volgende vereisten voldoet voordat u met deze zelfstudie begint:
- Maak een Azure SQL Data Warehouse, maak een firewallregel op serverniveau en maak verbinding met de server als serverbeheerder. Volg de instructies in [Quick Start: Een Azure SQL Data Warehouse maken](../sql-data-warehouse/create-data-warehouse-portal.md)
- Maak een databasehoofdsleutel voor Azure SQL Data Warehouse. Volg de instructies bij [Een databasehoofdsleutel maken](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key).
- Maak een Azure Blob-opslagaccount met daarin een container. Haal ook de toegangssleutel op voor toegang tot het opslagaccount. Volg de instructies in [Quick Start: Een Azure Blob Storage-account maken](../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-an-azure-databricks-workspace"></a>Een Azure Databricks-werkruimte maken

In deze sectie gaat u een Azure Databricks-werkruimte maken met behulp van Azure Portal.

1. Selecteer in Azure Portal **Een resource maken** > **Gegevens en analyses** > **Azure Databricks**.

    ![Databricks in Azure Portal](./media/databricks-extract-load-sql-data-warehouse/azure-databricks-on-portal.png "Databricks in Azure Portal")

3. Geef bij **Azure Databricks Service** de waarden op voor het maken van een Databricks-werkruimte.

    ![Een Azure Databricks-werkruimte maken](./media/databricks-extract-load-sql-data-warehouse/create-databricks-workspace.png "Een Azure Databricks-werkruimte maken")

    Geef de volgende waarden op:
    
    |Eigenschap  |Beschrijving  |
    |---------|---------|
    |**Werkruimtenaam**     | Geef een naam op voor uw Databricks-werkruimte.        |
    |**Abonnement**     | Selecteer uw Azure-abonnement in de vervolgkeuzelijst.        |
    |**Resourcegroep**     | Geef aan of u een nieuwe resourcegroep wilt maken of een bestaande groep wilt gebruiken. Een resourcegroep is een container met gerelateerde resources voor een Azure-oplossing. Zie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie. |
    |**Locatie**     | Selecteer **US - oost 2**. Zie [Producten beschikbaar per regio](https://azure.microsoft.com/regions/services/) voor andere beschikbare regio's.        |
    |**Prijscategorie**     |  U kunt kiezen tussen **Standard** en **Premium**. Bekijk de pagina [Prijzen voor Databricks](https://azure.microsoft.com/pricing/details/databricks/) voor meer informatie over deze categorieën.       |

    Selecteer **Vastmaken aan dashboard** en selecteer **Maken**.

4. Het duurt enkele minuten om het account te maken. Tijdens het maken van het account wordt rechts in de portal de tegel **Implementatie verzenden voor Azure Databricks** weergegeven. Mogelijk moet u op uw dashboard naar rechts scrollen om de tegel te zien. Bovenaan het scherm wordt ook een voortgangsbalk weergegeven. U kunt beide gebieden bekijken voor de voortgang.

    ![Tegel Databricks-implementatie](./media/databricks-extract-load-sql-data-warehouse/databricks-deployment-tile.png "Tegel Databricks-implementatie")

## <a name="create-a-spark-cluster-in-databricks"></a>Een Spark-cluster maken in Databricks

1. Ga in Azure Portal naar de Databricks-werkruimte die u hebt gemaakt en selecteer **Werkruimte starten**.

2. U wordt omgeleid naar de Azure Databricks-portal. Klik in de portal op **Cluster**.

    ![Databricks in Azure](./media/databricks-extract-load-sql-data-warehouse/databricks-on-azure.png "Databricks in Azure")

3. Op de pagina **Nieuw cluster** geeft u de waarden op waarmee een nieuw cluster wordt gemaakt.

    ![Een Databricks Spark-cluster maken in Azure](./media/databricks-extract-load-sql-data-warehouse/create-databricks-spark-cluster.png "Een Databricks Spark-cluster maken in Azure")

    Accepteer alle andere standaardwaarden die anders zijn dan de volgende waarden:

    * Voer een naam in voor het cluster.
    * Voor dit artikel maakt u een cluster met een **4.0**-runtime.
    * Zorg ervoor dat u het selectievakje **Beëindigen na\_\_ minuten van inactiviteit** inschakelt. Geef een duur (in minuten) op waarna het cluster moet worden beëindigd als het niet wordt gebruikt.
    
    Selecteer **Cluster maken**. Zodra het cluster wordt uitgevoerd, kunt u notitieblokken koppelen aan het cluster en Spark-taken uitvoeren.

## <a name="create-an-azure-data-lake-store-account"></a>Een Azure Data Lake Store-account maken

In deze sectie maakt u een Azure Data Lake Store-account waaraan u een service-principal voor Azure Active Directory koppelt. Verderop in deze zelfstudie gebruikt u deze service-principal in Azure Databricks voor toegang tot Azure Data Lake Store.

1. Selecteer in [Azure Portal](https://portal.azure.com) de optie **Een resource maken** > **Opslag** > **Data Lake Store**.
3. In de blade **Nieuwe Data Lake Store** geeft u de waarden op zoals u ze in de volgende schermafbeelding ziet:

    ![Een nieuw Azure Data Lake Store-account maken](./media/databricks-extract-load-sql-data-warehouse/create-new-datalake-store.png "Een nieuw Azure Data Lake Store-account maken")

    Geef de volgende waarden op:
    
    |Eigenschap  |Beschrijving  |
    |---------|---------|
    |**Naam**     | Voer een unieke naam in voor het Data Lake Store-account.        |
    |**Abonnement**     | Selecteer uw Azure-abonnement in de vervolgkeuzelijst.        |
    |**Resourcegroep**     | Selecteer voor deze zelfstudie dezelfde resourcegroep die u hebt gebruikt bij het maken van de Azure Databricks-werkruimte.  |
    |**Locatie**     | Selecteer **US - oost 2**.  |
    |**Prijspakket**     |  Selecteer **Betalen per gebruik**. |
    | **Versleutelingsinstellingen** | Behoud de standaardinstellingen. |

    Selecteer **Vastmaken aan dashboard** en selecteer **Maken**.

U gaat nu een service-principal voor Azure Active Directory maken en deze koppelen aan het Azure Data Lake Store-account dat u hebt gemaakt.

### <a name="create-an-azure-active-directory-service-principal"></a>Een service-principal voor Azure Active Directory maken

1. Selecteer in [Azure Portal](https://portal.azure.com) de optie **Alle services** en zoek naar **Azure Active Directory**.

2. Selecteer **App-registraties**.

   ![app-registraties selecteren](./media/databricks-extract-load-sql-data-warehouse/select-app-registrations.png)

3. Selecteer **Nieuwe toepassing registreren**.

   ![toepassing toevoegen](./media/databricks-extract-load-sql-data-warehouse/select-add-app.png)

4. Geef een naam en URL op voor de toepassing. Selecteer **Web-app/API** voor het type toepassing dat u wilt maken. Geef een aanmeldings-URL op en selecteer **Maken**.

   ![toepassing een naam geven](./media/databricks-extract-load-sql-data-warehouse/create-app.png)

Voor toegang tot het Data Lake Store-account vanuit Azure Databricks hebt u de volgende waarden nodig voor de service-principal voor Azure Active Directory die u hebt gemaakt:
- Toepassings-id
- Verificatiesleutel
- Tenant-id

In de volgende secties haalt u deze waarden op voor de service-principal voor Azure Active Directory die u eerder hebt gemaakt.

### <a name="get-application-id-and-authentication-key-for-the-service-principal"></a>Toepassings-id en verificatiesleutel voor de service-principal ophalen

Wanneer u zich programmatisch aanmeldt, hebt u de id voor uw toepassing en een verificatiesleutel nodig. U kunt deze waarden als volgt ophalen:

1. Selecteer uw toepassing bij **App-registraties** in Azure Active Directory.

   ![toepassing selecteren](./media/databricks-extract-load-sql-data-warehouse/select-app.png)

2. Kopieer de **Toepassings-id** en sla deze op in uw toepassingscode. Sommige [voorbeeldtoepassingen](#log-in-as-the-application) verwijzen naar deze waarde als de client-id.

   ![client-id](./media/databricks-extract-load-sql-data-warehouse/copy-app-id.png)

3. Selecteer **Instellingen** om een verificatiesleutel te genereren.

   ![instellingen selecteren](./media/databricks-extract-load-sql-data-warehouse/select-settings.png)

4. Selecteer **Sleutels** om een verificatiesleutel te genereren.

   ![sleutels selecteren](./media/databricks-extract-load-sql-data-warehouse/select-keys.png)

5. Geef een beschrijving op van de sleutel en geef de duur van de sleutel op. Selecteer **Opslaan** wanneer u klaar bent.

   ![sleutel opslaan](./media/databricks-extract-load-sql-data-warehouse/save-key.png)

   Na het opslaan van de sleutel wordt de waarde van de sleutel weergegeven. Kopieer deze waarde, want u kunt de sleutel later niet meer ophalen. U geeft de sleutelwaarde samen met de toepassings-id op om u aan te melden met de toepassing. Bewaar de sleutelwaarde op een locatie waar de toepassing deze kan ophalen.

   ![opgeslagen sleutel](./media/databricks-extract-load-sql-data-warehouse/copy-key.png)

### <a name="get-tenant-id"></a>Tenant-id ophalen

Wanneer u zich programmatisch aanmeldt, moet u de tenant-id samen met uw verificatieaanvraag doorgeven.

1. Selecteer **Azure Active Directory**.

   ![azure active directory selecteren](./media/databricks-extract-load-sql-data-warehouse/select-active-directory.png)

1. Haal de tenant-id op door **Eigenschappen** voor uw Azure AD-tenant te selecteren.

   ![Azure AD-eigenschappen selecteren](./media/databricks-extract-load-sql-data-warehouse/select-ad-properties.png)

1. Kopieer de **Map-id**. Deze waarde is uw tenant-id.

   ![tenant-id](./media/databricks-extract-load-sql-data-warehouse/copy-directory-id.png)

## <a name="upload-data-to-data-lake-store"></a>Gegevens uploaden naar Data Lake Store

In deze sectie uploadt u een voorbeeldgegevensbestand naar Data Lake Store. Dit bestand gebruikt u later in Azure Databricks om enkele transformaties uit te voeren. De voorbeeldgegevens (**small_radio_json.json**) die u in deze zelfstudie gebruikt, zijn beschikbaar in deze [GitHub-opslagplaats](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json).

1. Selecteer in [Azure Portal](https://portal.azure.com) het Data Lake Store-account dat u hebt gemaakt.

2. Open het tabblad **Overzicht** en klik op **Data Explorer**.

    ![Data Explorer openen](./media/databricks-extract-load-sql-data-warehouse/open-data-explorer.png "Data Explorer openen")

3. Klik in Data Explorer op **Uploaden**.

    ![Optie Uploaden](./media/databricks-extract-load-sql-data-warehouse/upload-to-data-lake-store.png "Optie Uploaden")

4. Blader in **Bestanden uploaden** naar de locatie van uw voorbeeldgegevensbestand en selecteer **Geselecteerde bestanden toevoegen**.

    ![Optie Uploaden](./media/databricks-extract-load-sql-data-warehouse/upload-data.png "Optie Uploaden")

5. In deze zelfstudie hebt u het gegevensbestand geüpload naar de hoofdmap van de Data Lake Store. Het bestand is daarom nu beschikbaar op `adl://<YOUR_DATA_LAKE_STORE_ACCOUNT_NAME>.azuredatalakestore.net/small_radio_json.json`.

## <a name="associate-service-principal-with-azure-data-lake-store"></a>Service-principal koppelen aan Azure Data Lake Store

In deze sectie koppelt u de gegevens in het Azure Data Lake Store-account aan de service-principal voor de Azure Active Directory die u hebt gemaakt. Daardoor kunt u het Data Lake Store-account vanuit Azure Databricks openen. Voor het scenario in dit artikel leest u de gegevens in Data Lake Store om een tabel in SQL Data Warehouse te vullen. Volgens het [Overzicht van Access Control in Data Lake Store](../data-lake-store/data-lake-store-access-control.md#common-scenarios-related-to-permissions) moet u voor leestoegang voor een bestand in Data Lake Store in het bezit zijn van het volgende:

- Machtigingen voor **uitvoeren** in alle mappen in de mapstructuur die naar het bestand leiden.
- Machtigingen voor **lezen** in het bestand zelf.

Voer de volgende stappen uit om deze machtigingen te verlenen.

1. Selecteer in [Azure Portal](https://portal.azure.com) het Data Lake Store-account dat u hebt gemaakt en selecteer vervolgens **Data Explorer**.

    ![Data Explorer starten](./media/databricks-extract-load-sql-data-warehouse/azure-databricks-data-explorer.png "Data Explorer starten")

2. Omdat het bestand met voorbeeldgegevens zich in de hoofdmap van de mapstructuur bevindt, hoeft u alleen de machtigingen voor **Uitvoeren** toe te wijzen aan de hoofdmap. Selecteer hiertoe in de hoofdmap van Data Explorer de optie **Toegang**.

    ![Toegangsbeheerlijsten toevoegen voor de map](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-folder-1.png "Toegangsbeheerlijsten toevoegen voor de map")

3. Selecteer onder **Toegang** de optie **Toevoegen**.

    ![Toegangsbeheerlijsten toevoegen voor de map](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-folder-2.png "Toegangsbeheerlijsten toevoegen voor de map")

4. Klik onder **Machtigingen toewijzen** op **Gebruiker of groep selecteren** en zoek naar de Azure Active Directory-service-principal die u eerder hebt gemaakt.

    ![Toegang tot Data Lake Store toevoegen](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-folder-3.png "Toegang tot Data Lake Store toevoegen")

    Selecteer de AAD-service-principal die u wilt toewijzen en klik op **Selecteren**.

5. Klik onder **Machtigingen toewijzen** op **Machtigingen selecteren** > **Uitvoeren**. Behoud de andere standaardwaarden en selecteer **OK** onder **Machtigingen selecteren** en vervolgens onder **Machtigingen toewijzen**.

    ![Toegang tot Data Lake Store toevoegen](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-folder-4.png "Toegang tot Data Lake Store toevoegen")

6. Ga terug naar Data Explorer en klik nu op het bestand waaraan u de leesmachtiging wilt toewijzen. Selecteer onder **Bestandsvoorbeeld** de optie **Toegang**.

    ![Toegang tot Data Lake Store toevoegen](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-file-1.png "Toegang tot Data Lake Store toevoegen")

7. Selecteer onder **Toegang** de optie **Toevoegen**. Klik onder **Machtigingen toewijzen** op **Gebruiker of groep selecteren** en zoek naar de Azure Active Directory-service-principal die u eerder hebt gemaakt.

    ![Toegang tot Data Lake Store toevoegen](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-folder-3.png "Toegang tot Data Lake Store toevoegen")

    Selecteer de AAD-service-principal die u wilt toewijzen en klik op **Selecteren**.

8. Klik onder **Machtigingen toewijzen** op **Machtigingen selecteren** > **Lezen**. Selecteer **OK** onder **Machtigingen selecteren** en vervolgens onder **Machtigingen toewijzen**.

    ![Toegang tot Data Lake Store toevoegen](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-file-2.png "Toegang tot Data Lake Store toevoegen")

    De service-principal heeft nu voldoende machtigingen om het bestand met voorbeeldgegevens uit Azure Data Lake Store te lezen.

## <a name="extract-data-from-data-lake-store"></a>Gegevens ophalen uit Data Lake Store

In deze sectie maakt u een notitieblok in de Azure Databricks-werkruimte en voert u codefragmenten uit om gegevens op te halen uit Data Lake Store en deze vervolgens te laden in Azure Databricks.

1. Ga in [Azure Portal](https://portal.azure.com) naar de Azure Databricks-werkruimte die u hebt gemaakt en selecteer **Werkruimte starten**.

2. Selecteer **Werkruimte** in het linkerdeelvenster. Selecteer in de **Werkruimte**-vervolgkeuzelijst, **Notitieblok** > **maken**.

    ![Een notitieblok maken in Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-create-notebook.png "Een notitieblok maken in Databricks")

2. Voer in het dialoogvenster **Notitieblok maken** een naam voor het notitieblok in. Selecteer **Scala** als taal en selecteer het Spark-cluster dat u eerder hebt gemaakt.

    ![Een notitieblok maken in Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-notebook-details.png "Een notitieblok maken in Databricks")

    Selecteer **Maken**.

3. Voeg het volgende fragment toe aan een lege codecel en vervang de tijdelijke waarden door de waarden die u eerder hebt opgeslagen voor de service-principal voor Azure Active Directory.

        spark.conf.set("dfs.adls.oauth2.access.token.provider.type", "ClientCredential")
        spark.conf.set("dfs.adls.oauth2.client.id", "<APPLICATION-ID>")
        spark.conf.set("dfs.adls.oauth2.credential", "<AUTHENTICATION-KEY>")
        spark.conf.set("dfs.adls.oauth2.refresh.url", "https://login.microsoftonline.com/<TENANT-ID>/oauth2/token")

    Druk op **SHIFT + ENTER** om de codecel uit te voeren.

4. U kunt nu het JSON-voorbeeldbestand in Data Lake Store laden als een dataframe in Azure Databricks. Plak het volgende fragment in een nieuwe codecel, vervang de tijdelijke waarde en druk op **SHIFT + ENTER**.

        val df = spark.read.json("adl://<DATA LAKE STORE NAME>.azuredatalakestore.net/small_radio_json.json")

5. Voer het volgende codefragment uit om de inhoud van het dataframe weer te geven.

        df.show()

    Het volgende (of een vergelijkbaar) codefragment wordt weergegeven:

        +---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
        |               artist|     auth|firstName|gender|itemInSession|  lastName|   length|  level|            location|method|    page| registration|sessionId|                song|status|           ts|userId|
        +---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
        | El Arrebato         |Logged In| Annalyse|     F|            2|Montgomery|234.57914| free  |  Killeen-Temple, TX|   PUT|NextSong|1384448062332|     1879|Quiero Quererte Q...|   200|1409318650332|   309|
        | Creedence Clearwa...|Logged In|   Dylann|     M|            9|    Thomas|340.87138| paid  |       Anchorage, AK|   PUT|NextSong|1400723739332|       10|        Born To Move|   200|1409318653332|    11|
        | Gorillaz            |Logged In|     Liam|     M|           11|     Watts|246.17751| paid  |New York-Newark-J...|   PUT|NextSong|1406279422332|     2047|                DARE|   200|1409318685332|   201|
        ...
        ...

U hebt nu de gegevens opgehaald uit Azure Data Lake Store en geladen in Azure Databricks.

## <a name="transform-data-in-azure-databricks"></a>Gegevens transformeren in Azure Databricks

De onbewerkte voorbeeldgegevensset **small_radio_json.json** legt de doelgroep voor een radiozender vast en bestaat uit een aantal kolommen. In deze sectie gaat u de gegevens zodanig transformeren dat alleen bepaalde kolommen uit de gegevensset worden opgehaald.

1. Begin met het ophalen van alleen de kolommen *firstName*, *lastName*, *gender*, *location* en *level* uit het dataframe dat u al hebt gemaakt.

        val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
        specificColumnsDf.show()

    De volgende uitvoer wordt weergegeven:

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

2. U kunt deze gegevens nog verder transformeren door de naam van de kolom **level** te wijzigen in **subscription_type**.

        val renamedColumnsDf = specificColumnsDf.withColumnRenamed("level", "subscription_type")
        renamedColumnsDf.show()

    De volgende uitvoer wordt weergegeven.

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

## <a name="load-data-into-azure-sql-data-warehouse"></a>Gegevens laden in Azure SQL Data Warehouse

In deze sectie uploadt u de getransformeerde gegevens naar Azure SQL Data Warehouse. Met de Azure SQL Data Warehouse-connector voor Azure Databricks kunt u een dataframe rechtstreeks uploaden als tabel in SQL Data Warehouse.

Zoals eerder vermeld, maakt de SQL Data Warehouse-connector gebruik van Azure Blob Storage als een tijdelijke opslaglocatie voor het uploaden van gegevens tussen Azure Databricks en Azure SQL Data Warehouse. U begint met het opgeven van de configuratie om verbinding te maken met het opslagaccount. U moet het account al hebben gemaakt als onderdeel van de vereisten voor dit artikel.

1. Geef de configuratie op voor toegang tot het Azure Storage-account vanuit Azure Databricks. Als u de URL voor de blob-opslag vanuit de portal kopiëren, moet u *https://* van het begin verwijderen.

        val blobStorage = "<STORAGE ACCOUNT NAME>.blob.core.windows.net"
        val blobContainer = "<CONTAINER NAME>"
        val blobAccessKey = "<ACCESS KEY>"

2. Geef een tijdelijke map op die wordt gebruikt tijdens het verplaatsen van gegevens tussen Azure Databricks en Azure SQL Data Warehouse.

        val tempDir = "wasbs://" + blobContainer + "@" + blobStorage +"/tempDirs"

3. Voer het volgende codefragment uit om toegangssleutels voor Azure Blob-opslag op te slaan in de configuratie. Daardoor hoeft u de toegangssleutel niet als gewone tekst in het notitieblok te bewaren.

        val acntInfo = "fs.azure.account.key."+ blobStorage
        sc.hadoopConfiguration.set(acntInfo, blobAccessKey)

4. Geef de waarden op om verbinding te maken met de Azure SQL Data Warehouse-instantie. U moet een SQL-datawarehouse hebben gemaakt als onderdeel van de vereisten.

        //SQL Data Warehouse related settings
        val dwDatabase = "<DATABASE NAME>"
        val dwServer = "<DATABASE SERVER NAME>"
        val dwUser = "<USER NAME>"
        val dwPass = "<PASSWORD>"
        val dwJdbcPort = "1433"
        val dwJdbcExtraOptions = "encrypt=true;trustServerCertificate=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
        val sqlDwUrl = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass + ";$dwJdbcExtraOptions"
        val sqlDwUrlSmall = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass

5. Voer het volgende codefragment uit om het getransformeerde dataframe, **renamedColumnsDf**, als tabel te laden in SQL Data Warehouse. Met dit fragment wordt een tabel met de naam **SampleTable** gemaakt in de SQL-database. Houd er rekening mee dat Azure SQL DW een hoofdsleutel vereist. U kunt een hoofdsleutel maken door de opdracht "CREATE MASTER KEY;" in SQL Server Management Studio uit te voeren.

        spark.conf.set(
          "spark.sql.parquet.writeLegacyFormat",
          "true")
    
        renamedColumnsDf.write
            .format("com.databricks.spark.sqldw")
            .option("url", sqlDwUrlSmall)
            .option("dbtable", "SampleTable")
            .option( "forward_spark_azure_storage_credentials","True")
            .option("tempdir", tempDir)
            .mode("overwrite")
            .save()

6. Maak verbinding met de SQL-database en controleer of u de **SampleTable** ziet.

    ![Voorbeeldtabel verifiëren](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table.png "Voorbeeldtabel verifiëren")

7. Voer een Select-query uit om de inhoud van de tabel te controleren. Deze moet dezelfde gegevens bevatten als het **renamedColumnsDf**-dataframe.

    ![Inhoud van voorbeeldtabel controleren](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table-content.png "Inhoud van voorbeeldtabel controleren")

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u de zelfstudie hebt voltooid, kunt u het cluster beëindigen. Dit doet u door vanuit de Azure Databricks-werkruimte in het linkerdeelvenster **Clusters** te selecteren. Voor het cluster dat u wilt beëindigen, plaatst u de cursor op het weglatingsteken onder de kolom **Acties** en selecteert u het **beëindigingspictogram**.

![Een Databricks-cluster stopzetten](./media/databricks-extract-load-sql-data-warehouse/terminate-databricks-cluster.png "Een Databricks-cluster stopzetten")

Als u het cluster niet handmatig beëindigt, stopt het cluster automatisch, op voorwaarde dat het selectievakje **Beëindigen na \_\_ minuten inactiviteit** is ingeschakeld tijdens het maken van het cluster. In dat geval stopt het cluster automatisch als het gedurende de opgegeven tijd inactief is geweest.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een Azure Databricks-werkruimte maken
> * Een Apache Spark-cluster in Azure Databricks maken
> * Een Azure Data Lake Store-account maken
> * Gegevens uploaden naar Azure Data Lake Store
> * Een notitieblok maken in Azure Databricks
> * Gegevens ophalen uit Data Lake Store
> * Gegevens transformeren in Azure Databricks
> * Gegevens laden in Azure SQL Data Warehouse

Ga naar de volgende zelfstudie voor informatie over het streamen van realtime gegevens naar Azure Databricks met behulp van Azure Event Hubs.

> [!div class="nextstepaction"]
>[Gegevens streamen naar Azure Databricks met behulp van Event Hubs](databricks-stream-from-eventhubs.md)
