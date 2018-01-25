---
title: "Gegevens van SQL Server naar Blob Storage kopiëren met behulp van Azure Data Factory | Microsoft Docs"
description: "Meer informatie over het kopiëren van gegevens uit een on-premises gegevensopslag naar Azure-cloud met behulp van de zelf-hostende Integration Runtime in Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/11/2018
ms.author: jingwang
ms.openlocfilehash: 64cd758e2f40ff2b18abbff1194a7e57389d8a54
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="tutorial-copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage"></a>Zelfstudie: gegevens van een on-premises SQL-serverdatabase naar Azure Blob Storage kopiëren
In deze zelfstudie gebruikt u de gebruikersinterface (UI) van Azure Data Factory om een pijplijn voor een gegevensfactory te maken waarmee gegevens worden gekopieerd van een on-premises SQL Server-database naar Azure Blob-opslag. U gaat een zelf-hostende Integration Runtime maken en gebruiken. Deze verplaatst gegevens van on-premises gegevensarchieven en gegevensarchieven in de cloud en omgekeerd. 

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Azure Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Dit artikel is geen gedetailleerde introductie tot de Data Factory-service. Zie voor meer informatie [Inleiding tot Azure Data Factory](introduction.md). 

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een data factory maken.
> * Een zelf-hostende Integration Runtime maken.
> * Gekoppelde services maken voor SQL Server en Azure Storage. 
> * Gegevenssets maken voor SQL Server en Azure Blob.
> * Een pijplijn maakt met een kopieeractiviteit om de gegevens te verplaatsen.
> * Een pijplijnuitvoering starten.
> * De pijplijnuitvoering controleert.

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

5. Voer het volgende queryscript uit voor de database. Hiermee wordt de **emp**-tabel gemaakt en worden enkele voorbeeldgegevens ingevoegd in deze tabel:

   ```
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

6. In de structuurweergave klikt u met de rechtermuisknop op de database die u hebt gemaakt en selecteert u **Nieuwe query**.

### <a name="azure-storage-account"></a>Azure-opslagaccount
In deze zelfstudie gaat u een algemeen Azure Storage-account (en dan met name Azure Blob Storage) gebruiken als een doel/sink-gegevensopslag. Zie het artikel [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) als u geen Azure Storage-account hebt voor algemene doeleinden. De pijplijn in de data factory die u in deze zelfstudie gaat maken, kopieert gegevens van de on-premises SQL Server-database (bron) naar deze Azure Blob-opslag (sink). 

#### <a name="get-storage-account-name-and-account-key"></a>De naam en sleutel van een opslagaccount ophalen
In deze QuickStart gaat u de naam en sleutel van uw Azure Storage-account gebruiken. Als u de naam en sleutel van uw opslagaccount wilt ophalen, doet u het volgende: 

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
In deze stap maakt u een data factory en start u de Azure Data Factory-gebruikersinterface om een pijplijn te maken in de data factory. 

1. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/tutorial-hybrid-copy-portal/new-azure-data-factory-menu.png)
2. Voer op de pagina **Nieuwe data factory** **ADFTutorialDataFactory** in als **naam**. 
      
     ![De pagina Nieuwe data factory](./media/tutorial-hybrid-copy-portal/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als het volgende foutbericht wordt weergegeven voor het naamveld, wijzigt u de naam van de data factory (bijvoorbeeld uwnaamADFTutorialDataFactory). Zie het artikel [Data factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
  
     ![De pagina Nieuwe data factory](./media/tutorial-hybrid-copy-portal/name-not-available-error.png)
3. Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
4. Voer een van de volgende stappen uit voor de **Resourcegroep**:
     
      - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
      - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
        Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
4. Selecteer **V2 (Preview)** als de **versie**.
5. Selecteer de **locatie** voor de gegevensfactory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich in andere regio's bevinden.
6. Selecteer **Vastmaken aan dashboard**.     
7. Klik op **Create**.      
8. Op het dashboard ziet u de volgende tegel met de status: **Gegevensfactory implementeren**. 

    ![tegel met de status 'gegevensfactory implementeren'](media/tutorial-hybrid-copy-portal/deploying-data-factory.png)
9. Na het aanmaken ziet u de pagina **Data Factory** zoals weergegeven in de afbeelding.
   
   ![Startpagina van de gegevensfactory](./media/tutorial-hybrid-copy-portal/data-factory-home-page.png)
10. Klik op de tegel **Maken en controleren** om de Azure Data Factory-UI te openen op een afzonderlijk tabblad. 


## <a name="create-a-pipeline"></a>Een pijplijn maken

1. Klik op de pagina **Aan de slag** op **Pijplijn maken**. Er wordt automatisch een pijplijn voor u gemaakt. U ziet de pijplijn in de structuurweergave en de bijbehorende editor is geopend. 

   ![Pagina Aan de slag](./media/tutorial-hybrid-copy-portal/get-started-page.png)
2. Ga in het venster **Eigenschappen** onderaan naar het tabblad **Algemeen** en voer **SQLServerToBlobPipeline** in als **Naam**.

   ![Naam pijplijn](./media/tutorial-hybrid-copy-portal/pipeline-name.png)
2. Vouw in de **Activiteiten**-werkset de optie **Gegevensstroom** uit. Gebruik vervolgens slepen-en-neerzetten om de **kopieeractiviteit** te verplaatsen naar het ontwerpoppervlak voor pijplijnen. Stel de naam van de activiteit in op **CopySqlServerToAzureBlobActivity**.

   ![Naam van de activiteit](./media/tutorial-hybrid-copy-portal/copy-activity-name.png)
3. Ga in het venster Eigenschappen naar het tabblad **Bron** en klik op **+ Nieuw**.

   ![Nieuwe brongegevensset - knop](./media/tutorial-hybrid-copy-portal/source-dataset-new-button.png)
4. Ga in het venster **Nieuwe gegevensset** naar **SQL-server**, selecteer **SQL Server** en klik op **Voltooien**. U ziet een nieuw tabblad met de titel **SqlServerTable1**. U ziet ook de gegevensset **SqlServerTable1** in de structuurweergave aan de linkerkant. 

   ![SQL-server selecteren](./media/tutorial-hybrid-copy-portal/select-sql-server.png)
5. Voer op het tabblad **Algemeen** in het venster Eigenschappen **SqlServerDataset** in als **Naam**.
    
   ![Brongegevensset - naam](./media/tutorial-hybrid-copy-portal/source-dataset-name.png)
6. Ga naar het tabblad **Verbindingen** en klik op **+ Nieuw**. In deze stap maakt u een verbinding met het brongegevensexemplaar (SQL Server-database). 

   ![Brongegevensset - knop voor nieuwe verbinding](./media/tutorial-hybrid-copy-portal/source-connection-new-button.png)
7. Klik in het venster **Nieuwe gekoppelde service** op **Nieuwe integratieruntime**. In deze sectie kunt u een zelf-hostende Integration Runtime maken en deze koppelen aan een on-premises computer met de SQL Server-database. De zelf-hostende Integration Runtime is het onderdeel waarmee gegevens worden gekopieerd van SQL Server-database op uw computer naar Azure Blob Storage. 

   ![Nieuwe integratieruntime](./media/tutorial-hybrid-copy-portal/new-integration-runtime-button.png)
8. Selecteer in het venster **Integratieruntime instellen** de optie **Privénetwerk** en klik op **Volgende**. 

   ![Privénetwerk selecteren](./media/tutorial-hybrid-copy-portal/select-private-network.png)
9. Voer een naam in voor de integratieruntime en klik op **Volgende**.  
    
   ![Integratieruntime - naam](./media/tutorial-hybrid-copy-portal/integration-runtime-name.png)
10. Klik in de sectie **Optie 1: snelle installatie** op **Klik hier om de snelle installatie voor deze computer te starten**. 

   ![Klik op de koppeling Snelle installatie](./media/tutorial-hybrid-copy-portal/click-exress-setup.png)
11. Klik in het venster **Snelle installatie van integratieruntime (zelf-hostend)** op **Sluiten**. 

   ![Installatie van integratieruntime - geslaagd](./media/tutorial-hybrid-copy-portal/integration-runtime-setup-successful.png)
12. Klik in de webbrowser in het venster **Installatie van integratieruntime** op **Voltooien**. 

   ![Installatie van integratieruntime - voltooien](./media/tutorial-hybrid-copy-portal/click-finish-integration-runtime-setup.png)
13. Voer in het venster **Nieuwe gekoppelde service** de volgende stappen uit:

    1. Voer **SqlServerLinkedService** in als **Naam**.
    2. Controleer of de zelf-hostende integratieruntime die u eerder hebt gemaakt, wordt weergegeven voor **Verbinding maken via integratieruntime**.
    3. Geef de naam van uw SQL-server op bij **Servernaam**. 
    4. Geef de naam van de database met de **emp**-tabel op in het veld **Databasenaam**. 
    5. Selecteer het geschikte **verificatietype** dat in Data Factory moet worden gebruikt om verbinding te maken met uw SQL Server-database. 
    6. Voer de **gebruikersnaam** en het **wachtwoord** in. Als u een backslash wilt gebruiken (\\) in de naam van uw gebruikersaccount of server, moet u er voor het escapeteken (\\) gebruiken. Gebruik bijvoorbeeld *mydomain\\\\myuser*. 
    7. Klik op **Verbinding testen**. Voer deze stap uit om te bevestigen dat de Data Factory-service verbinding mag maken met de SQL Server-database met behulp van de zelf-hostende integratieruntime die u hebt gemaakt. 
    8. Klik op **Opslaan** om de gekoppelde service op te slaan.

       ![Gekoppelde SQL Server-service - instellingen](./media/tutorial-hybrid-copy-portal/sql-server-linked-service-settings.png)
14. Als het goed is, ziet u nu het venster met de geopende brongegevensset weer. Voer bij **Verbinding** in het venster **Eigenschappen** de volgende stappen uit: 

    1. Controleer of **SqlServerLinkedService** wordt weergegeven als **Gekoppelde service**. 
    2. Selecteer **[dbo].[emp]** als **Tabel**.

        ![Brongegevensset - verbindingsgegevens](./media/tutorial-hybrid-copy-portal/source-dataset-connection.png)
15. Ga naar het tabblad met de SQLServerToBlobPipeline (of) klik in de structuurweergave op **SQLServerToBlobPipeline**. 

    ![Tabblad Pijplijn](./media/tutorial-hybrid-copy-portal/pipeliene-tab.png)
16. Ga in het venster **Eigenschappen** naar het tabblad **Sink** en klik op + **Nieuw**. 

    ![Sink-gegevensset - knop Nieuw](./media/tutorial-hybrid-copy-portal/sink-dataset-new-button.png)
17. Selecteer in het venster **Nieuwe gegevensset** de optie **Azure Blob-opslag** en klik op **Voltooien**. Er wordt nu een nieuw tabblad geopend voor de gegevensset. Ook ziet u de gegevensset in de structuurweergave. 

    ![Azure Blob-opslag selecteren](./media/tutorial-hybrid-copy-portal/select-azure-blob-storage.png)
18. Voer **AzureBlobDataset** in als **Naam**.

    ![Sink-gegevensset - naam](./media/tutorial-hybrid-copy-portal/sink-dataset-name.png)
19. Ga in het venster **Eigenschappen** naar het tabblad **Verbinding** en klik op **+Nieuw** voor **Gekoppelde service**. 

    ![Nieuwe gekoppelde service - knop](./media/tutorial-hybrid-copy-portal/new-storage-linked-service-button.png)
20. Voer in het venster **Nieuwe gekoppelde service** de volgende stappen uit:

    1. Voer **AzureStorageLinkedService** in als **Naam**.
    2. Selecteer uw Azure Storage-account als **naam van het opslagaccount**. 
    3. Test de verbinding met uw Azure Storage-account door te klikken op **Verbinding testen**.
    4. Klik op **Opslaan**.

        ![Gekoppelde Azure Storage-service - instellingen](./media/tutorial-hybrid-copy-portal/azure-storage-linked-service-settings.png) 
21.  Als het goed is, ziet u nu het venster met de geopende sink-gegevensset weer. Voer op het tabblad **Verbinding** de volgende stappen uit: 

        1. Controleer of **AzureStorageLinkedService** is geselecteerd als **Gekoppelde service**.
        2. Voer **adftutorial/fromonprem** in als het **mapgedeelte** van het **bestandspad**. Als de uitvoermap niet bestaat in de container adftutorial, wordt de uitvoermap automatisch gemaakt in de Data Factory-service.
        3. Voer `@CONCAT(pipeline().RunId, '.txt')` in als het **bestandsnaamgedeelte** van het **bestandspad**.

            ![Sink-gegevensset - verbinding](./media/tutorial-hybrid-copy-portal/sink-dataset-connection.png)
22. Ga naar het tabblad met de pijplijn geopend (of) klik op de **pijplijn** in de **structuurweergave**. Controleer of **AzureBlobDataset** is geselecteerd als **Sink-gegevensset**. 

    ![Sink-gegevensset geselecteerd ](./media/tutorial-hybrid-copy-portal/sink-dataset-selected.png)
23. Klik in de werkbalk op Valideren om de instellingen voor de pijplijn te valideren. Sluit het **Validatierapport voor de pijplijn** door in de rechterhoek op **X** te klikken. 

    ![Pijplijn valideren](./media/tutorial-hybrid-copy-portal/validate-pipeline.png)
1. U kunt entiteiten die u hebt gemaakt, publiceren in de Azure Data Factory-service door op **Publiceren** te klikken.

    ![De knop Publiceren](./media/tutorial-hybrid-copy-portal/publish-button.png)
24. Wacht tot u de pop-up **Het publiceren is voltooid** ziet. U kunt ook de status van de publicatie controleren door aan de linkerkant op **Meldingen weergeven** te klikken. Sluit het meldingenvenster door op **X** te klikken. 

    ![Het publiceren is voltooid](./media/tutorial-hybrid-copy-portal/publishing-succeeded.png)
    

## <a name="trigger-a-pipeline-run"></a>Een pijplijnuitvoering activeren
Klik in de werkbalk voor de pijplijn op **Activeren** en klik vervolgens op **Nu activeren**.

![Nu activeren](./media/tutorial-hybrid-copy-portal/trigger-now.png)

## <a name="monitor-the-pipeline-run"></a>De pijplijnuitvoering controleren.

1. Ga naar het tabblad **Controleren**. U ziet de pijplijn die u handmatig hebt geactiveerd in de vorige stap. 

    ![Pijplijnuitvoeringen](./media/tutorial-hybrid-copy-portal/pipeline-runs.png)
2. Klik op de koppeling **Uitvoeringen van activiteit weergeven** in de kolom **Acties** om de uitvoeringen van activiteit weer te geven die zijn gekoppeld aan de pijplijnuitvoering. U ziet maar één uitvoering van activiteit omdat de pijplijn maar één activiteit bevat. Klik op de koppeling **Details** (pictogram van een bril) in de kolom **Acties** om details over de kopieerbewerking te zien. Als u wilt terugkeren naar de weergave voor pijplijnuitvoeringen, klikt u bovenaan op **Pijplijnen**.

    ![Uitvoering van activiteiten](./media/tutorial-hybrid-copy-portal/activity-runs.png)

## <a name="verify-the-output"></a>De uitvoer controleren
De uitvoermap *fromonprem* wordt automatisch door de pijplijn gemaakt in de `adftutorial` blobcontainer. Controleer of u het bestand *dbo.emp.txt* in de uitvoermap ziet. 

1. Klik in Azure Portal op het venster met de **adftutorial**-container op **Vernieuwen** om de uitvoermap weer te geven.

    ![Uitvoermap gemaakt](media/tutorial-hybrid-copy-portal/fromonprem-folder.png)
2. Selecteer `fromonprem` in de lijst met mappen. 
3. Controleer of u een bestand met de naam `dbo.emp.txt` ziet.

    ![Uitvoerbestand](media/tutorial-hybrid-copy-portal/fromonprem-file.png)


## <a name="next-steps"></a>Volgende stappen
Met de pijplijn in dit voorbeeld worden gegevens gekopieerd van de ene locatie naar een andere locatie in een Azure Blob-opslag. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een data factory maken.
> * Een zelf-hostende Integration Runtime maken.
> * Gekoppelde services maken voor SQL Server en Azure Storage. 
> * Gegevenssets maken voor SQL Server en Azure Blob.
> * Een pijplijn maakt met een kopieeractiviteit om de gegevens te verplaatsen.
> * Een pijplijnuitvoering starten.
> * De pijplijnuitvoering controleert.

Zie [Ondersteunde gegevensopslagexemplaren](copy-activity-overview.md#supported-data-stores-and-formats) voor een lijst met gegevensopslagexemplaren die worden ondersteund door Data Factory.

Ga door naar de volgende zelfstudie voor informatie over het bulksgewijs kopiëren van gegevens uit een bron naar een bestemming:

> [!div class="nextstepaction"]
>[Gegevens bulksgewijs kopiëren](tutorial-bulk-copy-portal.md)
