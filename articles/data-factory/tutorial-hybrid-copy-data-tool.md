---
title: On-premises gegevens kopiëren met behulp van het hulpprogramma Azure Copy Data | Microsoft Docs
description: Maak een Azure-gegevensfactory en gebruik vervolgens het hulpprogramma Copy Data om gegevens uit een on-premises SQL Server-database te kopiëren naar Azure Blob-opslag.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/04/2018
ms.author: jingwang
ms.openlocfilehash: 85b721df1e666903c4966ca240c433ded01c06b7
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage-by-using-the-copy-data-tool"></a>Gegevens uit een on-premises SQL-serverdatabase naar Azure Blob-opslag kopiëren met behulp van het hulpprogramma Copy Data.
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1: algemeen verkrijgbaar](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versie 2 - Preview](tutorial-hybrid-copy-data-tool.md)

In deze zelfstudie gebruikt u Azure Portal om een gegevensfactory te maken. Vervolgens gebruikt u het hulpprogramma Copy Data om een pijplijn te maken waarmee gegevens uit een on-premises SQL Server-database worden gekopieerd naar Azure Blob Storage.

> [!NOTE]
> - Zie [Inleiding tot Data Factory](introduction.md) als u niet bekend bent met Azure Data Factory.
>
> - Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van Data Factory gebruikt, die algemeen beschikbaar is, raadpleegt u [Aan de slag met versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een data factory maken.
> * Het hulpprogramma Copy Data gebruiken om een pijplijn te maken.
> * De uitvoering van de pijplijn en van de activiteit controleren.

## <a name="prerequisites"></a>Vereisten
### <a name="azure-subscription"></a>Azure-abonnement
Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

### <a name="azure-roles"></a>Azure-rollen
Als u Data Factory-exemplaren wilt maken, moet het gebruikersaccount waarmee u zich bij Azure aanmeldt, zijn toegewezen aan de rollen *Inzender* of *Eigenaar*, of moet dit een *beheerder* van het Azure-abonnement zijn. 

Ga naar Azure Portal als u de machtigingen wilt weergeven die u hebt in het abonnement. Selecteer uw gebruikersnaam in de rechterbovenhoek en selecteer vervolgens **Machtigingen**. Als u toegang tot meerdere abonnementen hebt, moet u het juiste abonnement selecteren. Zie [Rollen toevoegen](../billing/billing-add-change-azure-subscription-administrator.md) voor voorbeelden van instructies voor het toevoegen van een gebruiker aan een rol.

### <a name="sql-server-2014-2016-and-2017"></a>SQL Server 2014, 2016 en 2017
In deze zelfstudie gebruikt u een on-premises SQL Server-database als een *brongegevensopslag*. De pijplijn in de data factory die u in deze zelfstudie gaat maken, kopieert gegevens van deze on-premises SQL Server-database (bron) naar Blob-opslag (sink). Maak een tabel met de naam **emp** in uw SQL Server-database en voeg een aantal voorbeeldgegevens toe aan de tabel. 

1. Start SQL Server Management Studio. Als dit niet al is geïnstalleerd op uw computer, gaat u naar [SQL Server Management Studio downloaden](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). 

2. Maak verbinding met SQL Server-exemplaar met behulp van uw referenties. 

3. Maak een voorbeelddatabase. Klik in de structuurweergave met de rechtermuisknop op **Databases** en selecteer **Nieuwe database**. 
 
4. Voer in het venster **Nieuwe database** een naam in voor de database en selecteer **OK**. 

5. Voer het volgende queryscript uit voor de database. Hiermee wordt de **emp**-tabel gemaakt en worden enkele voorbeeldgegevens ingevoegd in deze tabel. In de structuurweergave klikt u met de rechtermuisknop op de database die u hebt gemaakt en selecteert u **Nieuwe query**.

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO
    
    INSERT INTO emp (FirstName, LastName) VALUES ('John', 'Doe')
    INSERT INTO emp (FirstName, LastName) VALUES ('Jane', 'Doe')
    GO
    ```

### <a name="azure-storage-account"></a>Azure Storage-account
In deze zelfstudie gaat u een algemeen Azure Storage-account (en dan met name Blob Storage) gebruiken als een doel/sink-gegevensopslag. Zie het artikel [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) als u geen opslagaccount voor algemeen gebruikt hebt en er een wilt maken. De pijplijn in de data factory die u in deze zelfstudie gaat maken, kopieert gegevens van de on-premises SQL Server-database (bron) naar deze Blob-opslag (sink). 

#### <a name="get-the-storage-account-name-and-account-key"></a>De naam en sleutel van een opslagaccount ophalen
In deze zelfstudie gaat u de naam en sleutel van uw opslagaccount gebruiken. Voer de volgende stappen uit om de naam en sleutel van uw opslagaccount op te halen: 

1. Meld u aan bij [Azure Portal](https://portal.azure.com) met uw Azure-gebruikersnaam en -wachtwoord. 

2. Selecteer in het linkerdeelvenster **Meer services**. Filter met behulp van het sleutelwoord **Opslag** en selecteer vervolgens **Opslagaccounts**.

    ![Zoeken naar Storage-account](media/tutorial-hybrid-copy-powershell/search-storage-account.png)

3. Filter indien nodig in de lijst met opslagaccounts op uw opslagaccount. Selecteer vervolgens uw opslagaccount. 

4. Selecteer in het venster **Opslagaccount** de optie **Toegangssleutels**.

    ![Toegangssleutels](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)

5. Kopieer de waarden in de vakken **opslagaccountnaam** en **key1** en plak deze in Kladblok of een andere editor voor later gebruik in de zelfstudie. 

#### <a name="create-the-adftutorial-container"></a>De container adftutorial maken 
In deze sectie maakt u in uw Blob Storage een blobcontainer met de naam **adftutorial**. 

1. Schakel in het venster **Opslagaccount** over naar **Overzicht** en klik vervolgens op **Blobs**. 

    ![De optie Blobs selecteren](media/tutorial-hybrid-copy-powershell/select-blobs.png)

2. Selecteer in het venster **Blob-service** **Container**. 

    ![Knop Container](media/tutorial-hybrid-copy-powershell/add-container-button.png)

3. Voer in het venster **Nieuwe container** in het vak **Naam** **adftutorial** in en selecteer **OK**. 

    ![Nieuwe container](media/tutorial-hybrid-copy-powershell/new-container-dialog.png)

4. Selecteer **adftutorial** in de lijst met containers.

    ![Container selecteren](media/tutorial-hybrid-copy-powershell/seelct-adftutorial-container.png)

5. Houd het venster **Container** voor **adftutorial** geopend. U gaat hiermee aan het einde van deze zelfstudie de uitvoer controleren. In Data Factory wordt automatisch in deze container de uitvoermap gemaakt, zodat u er zelf geen hoeft te maken.

    ![Containervenster](media/tutorial-hybrid-copy-powershell/container-page.png)


## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Selecteer in het linkermenu **Nieuw** > **Gegevens en analyses** > **Data Factory**. 
   
   ![Nieuwe data factory maken](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory-menu.png)
2. Voer op de pagina **Nieuwe data factory** **ADFTutorialDataFactory** in bij **Naam**. 
      
     ![Nieuwe data factory](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory.png)
 
   De naam van de data factory moet *wereldwijd uniek* zijn. Als het volgende foutbericht wordt weergegeven voor het naamveld, wijzigt u de naam van de data factory (bijvoorbeeld uwnaamADFTutorialDataFactory). Zie [Data Factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
  
   ![Naam nieuwe data factory](./media/tutorial-hybrid-copy-data-tool/name-not-available-error.png)
3. Selecteer het Azure-**abonnement** waarin u de data factory wilt maken. 
4. Voer een van de volgende stappen uit voor **Resourcegroep**:
     
      - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst.

      - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in. 
         
      Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.
5. Selecteer **V2 (preview-versie)** bij **Versie**.
6. Selecteer bij **Locatie** de locatie voor de data factory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevensarchieven (bijvoorbeeld Azure Storage en SQL Database) en -berekeningen (bijvoorbeeld Azure HDInsight) die door Data Factory worden gebruikt, kunnen zich in andere locaties/regio's bevinden.
7. Selecteer **Vastmaken aan dashboard**. 
8. Selecteer **Maken**.
9. Op het dashboard ziet u de volgende tegel met de status: **Data Factory implementeren**:

    ![Tegel Data Factory implementeren](media/tutorial-hybrid-copy-data-tool/deploying-data-factory.png)
10. Na het aanmaken ziet u de pagina **Data Factory** zoals weergegeven in de afbeelding.
   
    ![Startpagina van de gegevensfactory](./media/tutorial-hybrid-copy-data-tool/data-factory-home-page.png)
11. Selecteer **Maken en controleren** om de Data Factory-gebruikersinterface te openen op een afzonderlijk tabblad. 

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Het hulpprogramma Copy Data gebruiken om een pijplijn te maken

1. Selecteer op de pagina **Aan de slag** **Copy Data** om het hulpprogramma Copy Data te starten. 

   ![De tegel Copy Data-hulpprogramma](./media/tutorial-hybrid-copy-data-tool/copy-data-tool-tile.png)
2. Typ op de pagina **Eigenschappen** van het hulpprogramma Copy Data **CopyFromOnPremSqlToAzureBlobPipeline** bij **Taaknaam**. Selecteer vervolgens **Volgende**. Met het hulpprogramma Copy Data wordt een pijplijn gemaakt met de naam die u opgeeft in dit veld. 
    
   ![Taaknaam](./media/tutorial-hybrid-copy-data-tool/properties-page.png)
3. Selecteer op de pagina **Brongegevensarchief** de optie **SQL Server** en selecteer vervolgens **Volgende**. Mogelijk moet u omlaag schuiven om **SQL Server** te zien in de lijst. 

   ![SQL Server-selecteren](./media/tutorial-hybrid-copy-data-tool/select-source-data-store.png)
4. Voer **SqlServerLinkedService** in bij **Verbindingsnaam**. Selecteer de koppeling **Integratieruntime maken**. U moet een zelf-hostende integratieruntime maken, deze downloaden op de computer en registreren bij Data Factory. Met de zelf-hostende integratieruntime worden gegevens gekopieerd tussen uw on-premises omgeving en de cloud.

   ![Een zelf-hostende integratieruntime maken](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-link.png)
5. Voer in het dialoogvenster **Integratieruntime maken** **TutorialIntegration Runtime** bij **Naam**. Selecteer vervolgens **Maken**. 

   ![Naam integratieruntime](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-dialog.png)
6. Selecteer **Snelle installatie op deze computer starten**. Met deze actie wordt de integratieruntime op de computer geïnstalleerd en geregistreerd bij Data Factory. U kunt er ook voor kiezen om handmatig te configureren door het installatiebestand te downloaden, uit te voeren, en de sleutel te gebruiken om de integratieruntime te registreren. 

    ![Koppeling Snelle installatie op deze computer starten](./media/tutorial-hybrid-copy-data-tool/launch-express-setup-link.png)
7. Voer de gedownloade toepassing uit. U ziet de status van de snelle installatie in het venster. 

    ![Status van snelle installatie](./media/tutorial-hybrid-copy-data-tool/express-setup-status.png)
8. Controleer of **TutorialIntegrationRuntime** is geselecteerd voor het veld **Integratieruntime**.

    ![Integratieruntime geselecteerd](./media/tutorial-hybrid-copy-data-tool/integration-runtime-selected.png)
9. Voer bij **De on-premises SQL Server-database opgeven** de volgende stappen uit: 

    a. Voer **OnPremSqlLinkedService** in bij **Verbindingsnaam**.

    b. Voer de naam van uw on-premises SQL Server-exemplaar in bij **Servernaam**.

    c. Voer de naam van uw on-premises database in bij **Databasenaam**.

    d. Selecteer de juiste verificatie bij **Verificatietype**.

    e. Voer bij **Gebruikersnaam** de naam in van de gebruiker die toegang heeft tot de on-premises SQL Server.

    f. Voer het **wachtwoord** voor de gebruiker in. 
10. Selecteer op de pagina **Tabellen selecteren waaruit de gegevens moeten worden gekopieerd of een aangepaste query gebruiken** de tabel **[dbo].[emp]** in de lijst en selecteer **Volgende**. 

    ![De selectie van de emp-tabel](./media/tutorial-hybrid-copy-data-tool/select-emp-table.png)
11. Selecteer op de pagina **Doelgegevensarchief** de optie **Azure Blob Storage** en selecteer **Volgende**.

    ![Selectie Blob-opslag](./media/tutorial-hybrid-copy-data-tool/select-destination-data-store.png)
12. Voer op de pagina **Azure Blob-opslagaccount opgeven** de volgende stappen uit: 

    a. Voer bij **Verbindingsnaam** **AzureStorageLinkedService** in.

    b. Selecteer bij **Opslagaccountnaam** uw opslagaccount in de vervolgkeuzelijst. 

    c. Selecteer **Volgende**.

    ![Geef het opslagaccount op.](./media/tutorial-hybrid-copy-data-tool/specify-azure-blob-storage-account.png)
13. Typ bij **Mappad** **adftutorial/fromonprem** op de pagina **Het uitvoerbestand of de uitvoermap kiezen**. U hebt de container **adftutorial** gemaakt als onderdeel van de vereisten. Als de uitvoermap niet bestaat, wordt deze automatisch aangemaakt in Data Factory. U kunt ook de knop **Bladeren** gebruiken om te navigeren in de blob-opslag en bijbehorende containers/mappen. U ziet dat de naam van het uitvoerbestand standaard is ingesteld op **dbo.emp**.
        
    ![Het uitvoerbestand of de uitvoermap kiezen](./media/tutorial-hybrid-copy-data-tool/choose-output-file-folder.png)
14. Selecteer op de pagina **Instellingen Bestandsindelingen** de optie **Volgende**. 

    ![De pagina Instellingen bestandsindelingen](./media/tutorial-hybrid-copy-data-tool/file-format-settings-page.png)
15. Selecteer op de pagina **Instellingen** de optie **Volgende**. 

    ![De pagina Instellingen](./media/tutorial-hybrid-copy-data-tool/settings-page.png)
16. Bekijk op de **Overzichtspagina** de waarden voor alle instellingen en selecteer **Volgende**. 

    ![Overzichtspagina](./media/tutorial-hybrid-copy-data-tool/summary-page.png)
17. Selecteer op de pagina **Implementatie** de optie **Controleren** om de pijplijn of taak die u hebt gemaakt, te controleren.

    ![De pagina Implementatie](./media/tutorial-hybrid-copy-data-tool/deployment-page.png)
18. Op het tabblad **Controleren** kunt u de status bekijken van de pijplijn die u hebt gemaakt. Met de koppelingen in de kolom **Acties** kunt u de uitvoeringen van activiteit weergeven die zijn gekoppeld aan de pijplijnuitvoering, en de pijplijn opnieuw uitvoeren. 

    ![Pijplijnuitvoeringen controleren](./media/tutorial-hybrid-copy-data-tool/monitor-pipeline-runs.png)
19. Selecteer de koppeling **Uitvoeringen van activiteit weergeven** in de kolom **Acties** om de activiteituitvoeringen te zien die zijn gekoppeld aan de pijplijnuitvoering. Selecteer de koppeling **Details** (pictogram van een bril) in de kolom **Acties** om details over de kopieerbewerking te zien. Als u wilt terugkeren naar de weergave**Pijplijnuitvoeringen**, selecteert u **Pijplijnen** bovenaan.

    ![Uitvoering van activiteiten controleren](./media/tutorial-hybrid-copy-data-tool/monitor-activity-runs.png)
20. Controleer of een uitvoerbestand wordt weergegeven in de map **fromonprem** van de container **adftutorial**. 
 
    ![Uitvoer-blob](./media/tutorial-hybrid-copy-data-tool/output-blob.png)
21. Selecteer het tabblad **Bewerken** aan de linkerkant om over te schakelen naar de bewerkingsmodus. U kunt de gekoppelde services, gegevenssets en pijplijnen die zijn gemaakt met het hulpprogramma, bijwerken met behulp van de editor. Selecteer **Code** om de JSON-code weer te geven die is gekoppeld aan de entiteit die in de editor is geopend. Bekijk [de Azure Portal-versie van deze tutorial](tutorial-copy-data-portal.md) voor details over hoe u entiteiten in de Data Factory-gebruikersinterface kunt bewerken.

    ![Tabblad Bewerken](./media/tutorial-hybrid-copy-data-tool/edit-tab.png)


## <a name="next-steps"></a>Volgende stappen
Met de pijplijn in dit voorbeeld worden gegevens uit een on-premises SQL Server-database gekopieerd naar Blob Storage. U hebt geleerd hoe u: 

> [!div class="checklist"]
> * Een data factory maken.
> * Het hulpprogramma Copy Data gebruiken om een pijplijn te maken.
> * De uitvoering van de pijplijn en van de activiteit controleren.

Zie [Ondersteunde gegevensopslagexemplaren](copy-activity-overview.md#supported-data-stores-and-formats) voor een lijst met gegevensopslagexemplaren die worden ondersteund door Data Factory.

Ga door naar de volgende zelfstudie voor informatie over het bulksgewijs kopiëren van gegevens uit een bron naar een bestemming:

> [!div class="nextstepaction"]
>[Gegevens bulksgewijs kopiëren](tutorial-bulk-copy-portal.md)
