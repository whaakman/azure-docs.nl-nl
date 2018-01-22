---
title: "On-premises gegevens kopiëren met behulp van het hulpprogramma Azure Copy Data | Microsoft Docs"
description: "Maak een Azure-gegevensfactory en gebruik vervolgens het hulpprogramma Copy Data om gegevens uit een on-premises SQL Server-database te kopiëren naar Azure Blob-opslag."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/04/2018
ms.author: jingwang
ms.openlocfilehash: aba8b13d47b2b9236a0d5cc868e53780e894c406
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage-by-using-copy-data-tool"></a>Gegevens uit een on-premises SQL-serverdatabase naar Azure Blob-opslag kopiëren met behulp van het hulpprogramma Copy Data.
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versie 2 - Preview](tutorial-hybrid-copy-data-tool.md)

In deze zelfstudie gebruikt u Azure Portal om een gegevensfactory te maken. Vervolgens gebruikt u het hulpprogramma Copy Data om een pijplijn te maken waarmee gegevens uit een on-premises SQL Server-database worden gekopieerd naar een Azure Blob-opslag.

> [!NOTE]
> - Zie [Inleiding tot Azure Data Factory](introduction.md) als u niet bekend bent met Azure Data Factory.
>
> - Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Aan de slag met versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een data factory maken.
> * Het hulpprogramma Copy Data gebruiken om een pijplijn te maken
> * De uitvoeringen van de pijplijn en van de activiteit controleren.

## <a name="prerequisites"></a>Vereisten
### <a name="azure-subscription"></a>Azure-abonnement
Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

### <a name="azure-roles"></a>Azure-rollen
Als u Data Factory-exemplaren wilt maken, moet het gebruikersaccount waarmee u zich bij Azure aanmeldt, zijn toegewezen aan de rollen *Inzender* of *Eigenaar*, of moet dit een *beheerder* van het Azure-abonnement zijn. 

Als u de machtigingen die u hebt in het abonnement wilt bekijken, gaat u naar Azure Portal, selecteert u rechtsboven in de hoek uw gebruikersnaam en selecteert u vervolgens **Machtigingen**. Als u toegang tot meerdere abonnementen hebt, moet u het juiste abonnement selecteren. Zie het artikel [Rollen toevoegen](../billing/billing-add-change-azure-subscription-administrator.md) voor voorbeelden van instructies voor het toevoegen van een gebruiker aan een rol.

### <a name="sql-server-2014-2016-and-2017"></a>SQL Server 2014, 2016 en 2017
In deze zelfstudie gebruikt u een on-premises SQL Server-database als een *brongegevensopslag*. De pijplijn in de data factory die u in deze zelfstudie gaat maken, kopieert gegevens van deze on-premises SQL Server-database (bron) naar een Azure Blob-opslag (sink). Maak een tabel met de naam **emp** in uw SQL Server-database en voeg een aantal voorbeeldgegevens toe aan de tabel. 

1. Start SQL Server Management Studio. Als dit niet al is geïnstalleerd op uw computer, gaat u naar [SQL Server Management Studio downloaden](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). 

2. Maak verbinding met SQL Server-exemplaar met behulp van uw referenties. 

3. Maak een voorbeelddatabase. Klik in de structuurweergave met de rechtermuisknop op **Databases** en selecteer **Nieuwe database**. 
 
4. Voer in het venster **Nieuwe database** een naam in voor de database en selecteer **OK**. 

5. Als u de tabel **emp** wilt maken en enkele voorbeeldgegevens in de tabel wilt invoegen, voert u het volgende queryscript uit voor de nieuwe database: klik in de structuurweergave met de rechtermuisknop op de database die u hebt gemaakt, en selecteer vervolgens **Nieuwe query**.

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

### <a name="azure-storage-account"></a>Azure-opslagaccount
In deze zelfstudie gaat u een algemeen Azure Storage-account (en dan met name Azure Blob Storage) gebruiken als een doel/sink-gegevensopslag. Zie het artikel [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) als u geen Azure Storage-account voor algemeen gebruikt hebt en er een wilt maken. De pijplijn in de data factory die u in deze zelfstudie gaat maken, kopieert gegevens van de on-premises SQL Server-database (bron) naar deze Azure Blob-opslag (sink). 

#### <a name="get-storage-account-name-and-account-key"></a>De naam en sleutel van een opslagaccount ophalen
In deze QuickStart gaat u de naam en sleutel van uw Azure Storage-account gebruiken. Voer de volgende stappen uit om de naam en sleutel van uw opslagaccount op te halen: 

1. Meld u aan bij [Azure Portal](https://portal.azure.com) met uw Azure gebruikersnaam en wachtwoord. 

2. Selecteer in het linkerdeelvenster **Meer services**, filter met behulp van het sleutelwoord **Opslag** en selecteer vervolgens **Opslagaccounts**.

    ![Zoeken naar een opslagaccount](media/tutorial-hybrid-copy-powershell/search-storage-account.png)

3. Filter in de lijst met opslagaccounts op uw opslagaccount (indien nodig) en selecteer vervolgens uw opslagaccount. 

4. Selecteer in het venster **Opslagaccount** de optie **Toegangssleutels**.

    ![De naam en sleutel van het opslagaccount ophalen](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)

5. Kopieer de waarden in de vakken **opslagaccountnaam** en **key1** en plak deze in Kladblok of een andere editor voor later gebruik in de zelfstudie. 

#### <a name="create-the-adftutorial-container"></a>De container adftutorial maken 
In deze sectie maakt u in uw Azure Blob Storage een blobcontainer met de naam **adftutorial**. 

1. Schakel in het venster **Opslagaccount** over naar **Overzicht** en klik vervolgens op **Blobs**. 

    ![De optie Blobs selecteren](media/tutorial-hybrid-copy-powershell/select-blobs.png)

2. Selecteer in het venster **Blob-service** **Container**. 

    ![Knop Container toevoegen](media/tutorial-hybrid-copy-powershell/add-container-button.png)

3. Voer in het venster **Nieuwe container** in het vak **Naam** **adftutorial** in en selecteer **OK**. 

    ![Naam van container invoeren](media/tutorial-hybrid-copy-powershell/new-container-dialog.png)

4. Selecteer **adftutorial** in de lijst met containers.  

    ![De container selecteren](media/tutorial-hybrid-copy-powershell/seelct-adftutorial-container.png)

5. Houd het venster **Container** voor **adftutorial** geopend. U gaat hiermee aan het einde van deze zelfstudie de uitvoer controleren. In Data Factory wordt automatisch in deze container de uitvoermap gemaakt, zodat u er zelf geen hoeft te maken.

    ![Containervenster](media/tutorial-hybrid-copy-powershell/container-page.png)


## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory-menu.png)
2. Voer op de pagina **Nieuwe gegevensfactory** **ADFTutorialDataFactory** in als de **naam**. 
      
     ![Pagina Nieuwe gegevensfactory](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als het volgende foutbericht wordt weergegeven voor het naamveld, wijzigt u de naam van de gegevensfactory (bijvoorbeeld uwnaamADFTutorialDataFactory). Zie het artikel [Data Factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
  
     ![Pagina Nieuwe gegevensfactory](./media/tutorial-hybrid-copy-data-tool/name-not-available-error.png)
3. Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
4. Voer een van de volgende stappen uit voor de **Resourcegroep**:
     
      - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
      - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
      Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
4. Selecteer **V2 (Preview)** als de **versie**.
5. Selecteer de **locatie** voor de gegevensfactory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevensopslag (Azure Storage, Azure SQL Database, enz.) en berekeningen (HDInsight, enz.) die worden gebruikt in Data Factory, kunnen zich op andere locaties/regio’s bevinden.
6. Selecteer **Vastmaken aan dashboard**.     
7. Klik op **Create**.
8. Op het dashboard ziet u de volgende tegel met de status: **Gegevensfactory implementeren**. 

    ![tegel met de status 'gegevensfactory implementeren'](media/tutorial-hybrid-copy-data-tool/deploying-data-factory.png)
9. Wanneer de gegevensfactory is gemaakt, ziet u de pagina **Gegevensfactory** zoals wordt weergegeven in de afbeelding.
   
   ![Startpagina van de gegevensfactory](./media/tutorial-hybrid-copy-data-tool/data-factory-home-page.png)
10. Klik op de tegel **Maken en controleren** om de gebruikersinterface (UI) van Azure Data Factory te openen op een afzonderlijk tabblad. 

## <a name="use-copy-data-tool-to-create-a-pipeline"></a>Het hulpprogramma Copy Data gebruiken om een pijplijn te maken

1. Klik op de pagina Aan de slag op de tegel **Copy Data** om het hulpprogramma Copy Data op te starten. 

   ![De tegel Copy Data-hulpprogramma](./media/tutorial-hybrid-copy-data-tool/copy-data-tool-tile.png)
2. Geef op de pagina **Eigenschappen** van het hulpprogramma Copy Data **CopyFromOnPremSqlToAzureBlobPipeline** op als de **Taaknaam** en klik op **Volgende**. Met het hulpprogramma Copy Data wordt een pijplijn gemaakt met de naam die u opgeeft in dit veld. 
    
   ![De pagina Eigenschappen](./media/tutorial-hybrid-copy-data-tool/properties-page.png)
3. Selecteer op de pagina **Brongegevensexemplaar** de optie **SQL-server** en klik op **Volgende**. Mogelijk moet u omlaag schuiven om **SQL-server** te zien in de lijst. 

   ![De pagina Brongegevensarchief](./media/tutorial-hybrid-copy-data-tool/select-source-data-store.png)
4. Voer **SqlServerLinkedService** in als **Verbindingsnaam** en klik op de koppeling **Integratieruntime maken**. U moet een zelf-hostende integratieruntime maken, deze downloaden op de computer en registreren bij de Data Factory-service. Met de zelf-hostende integratieruntime worden gegevens gekopieerd tussen uw on-premises omgeving en de Azure-cloud.

   ![Koppeling integratieruntime maken](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-link.png)
5. Voer in het dialoogvenster **Integratieruntime maken** **TutorialIntegration Runtime** in het veld **Naam** in en klik op **Maken**. 

   ![Dialoogvenster integratieruntime maken](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-dialog.png)
6. Klik op **Snelle installatie op deze computer starten**. Met deze actie wordt de integratieruntime op de computer geïnstalleerd en geregistreerd bij de Data Factory-service. U kunt er ook voor kiezen om handmatig te configureren door het installatiebestand te downloaden, uit te voeren, en de sleutel te gebruiken om de integratieruntime te registreren. 

    ![Koppeling integratieruntime maken](./media/tutorial-hybrid-copy-data-tool/launch-express-setup-link.png)
7. Voer de gedownloade toepassing uit. U ziet de **status** van de snelle installatie in het venster. 

    ![Status van snelle installatie](./media/tutorial-hybrid-copy-data-tool/express-setup-status.png)
8. Controleer of de **TutorialIntegrationRuntime** is geselecteerd voor het veld **Integratieruntime**.

    ![Integratieruntime geselecteerd](./media/tutorial-hybrid-copy-data-tool/integration-runtime-selected.png)
9. Voer bij **De on-premises SQL Server-database opgeven** de volgende stappen uit: 

    1. Voer **OnPremSqlLinkedService** in als **Verbindingsnaam**.
    2. Voer de naam van uw SQL-server in als **Servernaam**.
    3. Voer de naam van uw on-premises database in als **Databasenaam**. 
    4. Selecteer de juiste verificatie bij **Verificatietype**.
    5. Voer de naam van de gebruiker die toegang heeft tot de on-premises SQL-server, in als **Gebruikersnaam**.
    6. Voer het **wachtwoord** van de gebruiker in. 
10. Selecteer op de pagina **Select tables from which to copy the data or use a custom query** (Tabellen selecteren waaruit de gegevens moeten worden gekopieerd of een aangepaste query gebruiken), selecteer de tabel **[dbo].[emp]** in de lijst en klik op **Volgende**. 

    ![Selecteer de tabel emp](./media/tutorial-hybrid-copy-data-tool/select-emp-table.png)
11. Selecteer op de pagina **Doelgegevensarchief** de optie **Azure Blob Storage** en klik op **Volgende**.

    ![Azure Blob-opslag selecteren](./media/tutorial-hybrid-copy-data-tool/select-destination-data-store.png)
12. Voer op de pagina **Azure Blob-opslagaccount opgeven** de volgende stappen uit: 

    1. Voer **AzureStorageLinkedService** in als **Verbindingsnaam**.
    2. Selecteer uw Azure Storage-account in de vervolgkeuzelijst als **naam van het opslagaccount**. 
    3. Klik op **Volgende**.

        ![Azure Blob-opslag selecteren](./media/tutorial-hybrid-copy-data-tool/specify-azure-blob-storage-account.png)
13. Voer op de pagina **Het uitvoerbestand of de uitvoermap kiezen** de volgende stappen uit: 
    
    1. Voer **adftutorial/fromonprem** in als **Mappad**. U hebt de container **adftutorial** gemaakt als onderdeel van de vereisten. Als de uitvoermap niet bestaat, wordt deze automatisch gemaakt in de Data Factory-service. U kunt ook de knop **Bladeren** gebruiken om te navigeren in de blob-opslag en bijbehorende containers/mappen. U ziet dat de naam van het uitvoerbestand standaard is ingesteld op **dbo.emp**.
        
        ![Het uitvoerbestand of de uitvoermap kiezen](./media/tutorial-hybrid-copy-data-tool/choose-output-file-folder.png)
14. Klik op de pagina **Instellingen Bestandsindelingen** op **Volgende**. 

    ![De pagina Instellingen bestandsindelingen](./media/tutorial-hybrid-copy-data-tool/file-format-settings-page.png)
15. Klik op de pagina **Instellingen** op **Volgende**. 

    ![De pagina Instellingen](./media/tutorial-hybrid-copy-data-tool/settings-page.png)
16. Bekijk op de **Overzichtspagina** de waarden voor alle instellingen en klik op **Volgende**. 

    ![Overzichtspagina](./media/tutorial-hybrid-copy-data-tool/summary-page.png)
17. Klik op de pagina **Implementatie** op **Controleren** om de pijplijn of taak die u hebt gemaakt, te controleren.

    ![De pagina Implementatie](./media/tutorial-hybrid-copy-data-tool/deployment-page.png)
18. Op het tabblad **Controleren** kunt u de status bekijken van de pijplijn die u hebt gemaakt. Met de koppelingen in de kolom **Acties** kunt u de uitvoeringen van activiteit weergeven die zijn gekoppeld aan de pijplijnuitvoering, en de pijplijn opnieuw uitvoeren. 

    ![Pijplijnuitvoeringen](./media/tutorial-hybrid-copy-data-tool/monitor-pipeline-runs.png)
19. Klik op de koppeling **Uitvoeringen van activiteit weergeven** in de kolom **Acties** om de uitvoeringen van activiteit te zien die zijn gekoppeld aan de pijplijnuitvoering. Klik op de koppeling **Details** (pictogram van een bril) in de kolom **Acties** om details over de kopieerbewerking te zien. Als u wilt terugkeren naar de weergave voor pijplijnuitvoeringen, klikt u bovenaan op **Pijplijnen**.

    ![Uitvoeringen van activiteit](./media/tutorial-hybrid-copy-data-tool/monitor-activity-runs.png)
20. Controleer of een uitvoerbestand wordt weergegeven in de map **fromonprem** van de container **adftutorial**.   
 
    ![Uitvoer-blob](./media/tutorial-hybrid-copy-data-tool/output-blob.png)
21. Klik op het tabblad **Bewerken** aan de linkerkant om over te schakelen naar de bewerkingsmodus. U kunt de gekoppelde services, gegevenssets en pijplijnen die zijn gemaakt met het hulpprogramma, bijwerken met behulp van de editor. Klik op **Code** om de JSON-code weer te geven die is gekoppeld aan de entiteit die in de editor is geopend. Bekijk [de Azure Portal-versie van deze zelfstudie](tutorial-copy-data-portal.md) voor details over het bewerken van entiteiten in de Data Factory-UI.

    ![Tabblad Bewerken](./media/tutorial-hybrid-copy-data-tool/edit-tab.png)


## <a name="next-steps"></a>Volgende stappen
Met de pijplijn in dit voorbeeld worden gegevens uit een on-premises SQL Server-database gekopieerd naar een Azure Blob-opslag. U hebt geleerd hoe u: 

> [!div class="checklist"]
> * Een data factory maken.
> * Het hulpprogramma Copy Data gebruiken om een pijplijn te maken
> * De uitvoeringen van de pijplijn en van de activiteit controleren.

Zie [Ondersteunde gegevensopslagexemplaren](copy-activity-overview.md#supported-data-stores-and-formats) voor een lijst met gegevensopslagexemplaren die worden ondersteund door Data Factory.

Ga door naar de volgende zelfstudie voor informatie over het bulksgewijs kopiëren van gegevens uit een bron naar een bestemming:

> [!div class="nextstepaction"]
>[Gegevens bulksgewijs kopiëren](tutorial-bulk-copy-portal.md)