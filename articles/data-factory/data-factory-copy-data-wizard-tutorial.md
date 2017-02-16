---
title: "Zelfstudie: een pijplijn maken met de wizard Kopiëren | Microsoft Docs"
description: "In deze zelfstudie maakt u een Azure Data Factory-pijplijn met een kopieeractiviteit. Hiervoor gebruikt u de wizard Kopiëren die wordt ondersteund door Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b87afb8e-53b7-4e1b-905b-0343dd096198
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/02/2017
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: fbf77e9848ce371fd8d02b83275eb553d950b0ff
ms.openlocfilehash: 5a50f583831b398ae22416e7ade23c33846de55c


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-data-factory-copy-wizard"></a>Zelfstudie: een pijplijn maken met de kopieeractiviteit middels de Data Factory-wizard Kopiëren
> [!div class="op_single_selector"]
> * [Overzicht en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [De wizard Kopiëren](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-sjabloon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

Met de **wizard Kopiëren** van Azure Data Factory kunt u snel en eenvoudig een pijplijn maken die het scenario voor gegevensopname/-verplaatsing implementeert. Daarom wordt u aangeraden de wizard te gebruiken als een eerste stap bij het maken van een voorbeeldpijplijn voor het scenario waarin gegevensverplaatsing plaatsvindt. Deze zelfstudie laat zien hoe u een Azure-gegevensfactory kunt maken, de wizard Kopiëren kunt starten en een reeks stappen kunt uitvoeren om informatie over uw scenario voor gegevensopname/-verplaatsing op te geven. Nadat u de stappen in de wizard hebt voltooid, maakt de wizard automatisch een pijplijn met een kopieeractiviteit om gegevens te kopiëren uit een Azure Blob-opslag naar Azure SQL-databases. Zie [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) voor meer informatie over de kopieeractiviteit. 

## <a name="prerequisites"></a>Vereisten
- Neem [Overzicht van de zelfstudie en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) door voor een overzicht van de zelfstudie en voer de **vereiste** stappen uit.


## <a name="create-data-factory"></a>Een gegevensfactory maken
In deze stap gebruikt u Azure Portal om een Azure Data Factory met de naam **ADFTutorialDataFactory** te maken.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) en klik op **+ Nieuw** in de linkerbovenhoek. Klik op **Intelligence en analytische gegevens** en klik op **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/data-factory-copy-data-wizard-tutorial/new-data-factory-menu.png)
2. In de blade **Nieuwe gegevensfactory**:
   
   1. Voer **ADFTutorialDataFactory** in als **naam**.
       De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als u de volgende fout ontvang: **De gegevensfactory ADFTutorialDataFactory is niet beschikbaar**, wijzigt u de naam van de gegevensfactory (naar bijvoorbeeld uwnaamADFTutorialDataFactory) en probeert u het opnieuw. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](data-factory-naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.  
      
       ![Naam van gegevensfactory niet beschikbaar](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-not-available.png)
      
      > [!NOTE]
      > De naam van de gegevensfactory wordt in de toekomst mogelijk geregistreerd als DNS-naam en wordt daarmee ook voor iedereen zichtbaar.
      > 
      > 
   2. Selecteer uw Azure-**abonnement**.
   3. Voer een van de volgende stappen uit voor de resourcegroep: 
      
      - Selecteer **Bestaande gebruiken** om een bestaande resourcegroep te selecteren.
      - Selecteer **Nieuwe maken** als u een naam voor een resourcegroep wilt typen.
         
          Voor sommige van de stappen in deze zelfstudie wordt ervan uitgegaan dat u voor de resourcegroep de naam **ADFTutorialResourceGroup** gebruikt. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.
   4. Selecteer een **locatie** voor de gegevensfactory.
   5. Selecteer het selectievakje **Vastmaken aan dashboard** onderaan de blade.  
   6. Klik op **Create**.
      
       ![Blade voor een nieuwe gegevensfactory](media/data-factory-copy-data-wizard-tutorial/new-data-factory-blade.png)            
3. Wanneer het aanmaken is voltooid, ziet u de blade **Gegevensfactory** zoals op de volgende afbeelding wordt weergegeven:
   
   ![Startpagina van de gegevensfactory](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-home-page.png)

## <a name="launch-copy-wizard"></a>De wizard Kopiëren starten
1. Op de startpagina van de gegevensfactory klikt u op de tegel **Gegevens kopiëren** om de **wizard Kopiëren** te openen. 
   
   > [!NOTE]
   > Als u ziet dat de webbrowser is vastgelopen bij Autoriseren... schakelt u **Cookies van derden en sitegegevens blokkeren** uit, of u laat dit ingeschakeld en u maakt een uitzondering voor **login.microsoftonline.com** en opent u de wizard opnieuw.
   > 
   > 
2. Op de pagina **Eigenschappen**:
   
   1. Voer **CopyFromBlobToAzureSql** in als **taaknaam**
   2. Voer de **beschrijving** in (optioneel).
   3. Wijzig de **Startdatum en -tijd** en de **Einddatum en -tijd** zodat de einddatum is ingesteld op vandaag en de startdatum op de datum vijf dagen vóór de huidige dag.  
   4. Klik op **Volgende**.  
      
      ![Hulpprogramma voor kopiëren - pagina Eigenschappen](./media/data-factory-copy-data-wizard-tutorial/copy-tool-properties-page.png) 
3. Op de pagina **Brongegevensarchief** klikt u op de tegel **Azure Blob Storage**. U gebruikt deze pagina om het brongegevensarchief op te geven voor de kopieertaak. U kunt een bestaande gekoppelde service van een gegevensarchief gebruiken of een nieuw gegevensarchief opgeven. Als u een bestaande gekoppelde service wilt gebruiken, klikt u op **VAN BESTAANDE GEKOPPELDE SERVICES** en selecteert u de juiste gekoppelde service. 
   
    ![Hulpprogramma voor kopiëren - pagina van brongegevensarchief](./media/data-factory-copy-data-wizard-tutorial/copy-tool-source-data-store-page.png)
4. Op de pagina **Het Azure Blob Storage-account opgeven**:
   
   1. Voer **AzureStorageLinkedService** in als **naam van de gekoppelde service**.
   2. Controleer of de optie **Van Azure-abonnementen** is geselecteerd als **accountselectiemethode**.
   3. Selecteer uw Azure-**abonnement**.  
   4. Selecteer een **Azure-opslagaccount** uit de lijst met Azure-opslagaccounts die beschikbaar is voor het abonnement dat u hebt geselecteerd. U kunt er ook voor kiezen om de opslagaccountinstellingen handmatig op te geven. Selecteer daarvoor de optie **Handmatig invoeren** als **accountselectiemethode** en klik vervolgens op **Volgende**. 
      
      ![Hulpprogramma voor kopiëren - Het Azure Blob Storage-account opgeven](./media/data-factory-copy-data-wizard-tutorial/copy-tool-specify-azure-blob-storage-account.png)
5. Op de pagina **Het invoerbestand of de invoermap kiezen**:
   
   1. Navigeer naar de map **adftutorial**.
   2. Selecteer **emp.txt** en klik op **Kiezen**
   3. Klik op **Volgende**. 
      
      ![Hulpprogramma voor kopiëren - Het invoerbestand of de invoermap kiezen](./media/data-factory-copy-data-wizard-tutorial/copy-tool-choose-input-file-or-folder.png)
6. Klik op de pagina **Het invoerbestand of de invoermap kiezen** op **Volgende** Selecteer niet **Binaire kopie**. 
   
    ![Hulpprogramma voor kopiëren - Het invoerbestand of de invoermap kiezen](./media/data-factory-copy-data-wizard-tutorial/chose-input-file-folder.png) 
7. Op de pagina **Bestandsinstellingen** ziet u de scheidingstekens en het schema dat automatisch is gedetecteerd door de wizard tijdens het parseren van het bestand. U kunt de scheidingstekens ook handmatig invoeren zodat de wizard Kopiëren stopt met automatisch detecteren, of als u wilt dat gegevens worden overschreven. Klik op **Volgende** nadat u de scheidingstekens hebt gecontroleerd en een voorbeeld van de gegevens hebt bekeken. 
   
    ![Hulpprogramma voor kopiëren - Bestandsindelingsinstellingen](./media/data-factory-copy-data-wizard-tutorial/copy-tool-file-format-settings.png)  
8. Op de pagina Doelgegevensarchief selecteert u **Azure SQL Database** en klikt u op **Volgende**.
   
    ![Hulpprogramma voor kopiëren - Doelarchief kiezen](./media/data-factory-copy-data-wizard-tutorial/choose-destination-store.png)
9. Op de pagina **De Azure SQL Database opgeven**:
   
   1. Typ **AzureSqlLinkedService** in het veld **Verbindingsnaam**.
   2. Controleer of de optie **Van Azure-abonnementen** is geselecteerd als **Server-/databaseselectiemethode**.
   3. Selecteer uw Azure-**abonnement**.  
   4. Selecteer de **servernaam** en **database**.
   5. Voer de **gebruikersnaam** en het **wachtwoord** in.
   6. Klik op **Volgende**.  
      
      ![Hulpprogramma voor kopiëren - Azure SQL-database opgeven](./media/data-factory-copy-data-wizard-tutorial/specify-azure-sql-database.png)
10. Op de pagina **Tabeltoewijzing** selecteert u uit de vervolgkeuzelijst **emp** in het veld **Bestemming** en klikt u op de **pijl naar beneden** (optioneel) om het schema en een voorbeeld van de gegevens te bekijken.
    
     ![Hulpprogramma voor kopiëren - Tabeltoewijzing](./media/data-factory-copy-data-wizard-tutorial/copy-tool-table-mapping-page.png) 
11. Op de pagina **Schematoewijzing** klikt u op **Volgende**.
    
    ![Hulpprogramma voor kopiëren - Schematoewijzing](./media/data-factory-copy-data-wizard-tutorial/schema-mapping-page.png)
12. Op de pagina **Prestatie-instellingen** klikt u op **Volgende**. 
    
    ![Hulpprogramma voor kopiëren - Prestatie-instellingen](./media/data-factory-copy-data-wizard-tutorial/performance-settings.png)
13. Lees de informatie op de pagina **Samenvatting** en klik op **Voltooien**. De wizard maakt twee gekoppelde services, twee gegevenssets (invoer en uitvoer) en één pijplijn in de gegevensfactory (van waaruit u de wizard Kopiëren hebt gestart). 
    
    ![Hulpprogramma voor kopiëren - Prestatie-instellingen](./media/data-factory-copy-data-wizard-tutorial/summary-page.png)

## <a name="launch-monitor-and-manage-application"></a>Monitor starten en toepassing beheren
1. Klik op de pagina **Implementatie** op de koppeling: `Click here to monitor copy pipeline`.
   
   ![Hulpprogramma voor kopiëren - Implementeren voltooid](./media/data-factory-copy-data-wizard-tutorial/copy-tool-deployment-succeeded.png)  
2. Volg de instructies in [Pijplijnen bewaken en beheren met de app voor bewaking](data-factory-monitor-manage-app.md) voor meer informatie over het bewaken van de pijplijn die u hebt gemaakt. Klik op het pictogram **Vernieuwen** in de lijst met **ACTIVITEITSVENSTERS** om het segment weer te geven. 
   
   ![App voor bewaking](./media/data-factory-copy-data-wizard-tutorial/monitoring-app.png) 
   
   
   Klik op de knop **Vernieuwen** in de lijst met **ACTIVITEITSVENSTERS** om de meest recente status te zien. Deze wordt niet automatisch vernieuwd. 

> [!NOTE]
> In de gegevenspijplijn in deze zelfstudie worden gegevens van een brongegevensarchief gekopieerd naar een doelgegevensarchief. Er worden geen invoergegevens mee getransformeerd in uitvoergegevens. Zie [Zelfstudie: uw eerste pijplijn maken om gegevens te transformeren met een Hadoop-cluster](data-factory-build-your-first-pipeline.md) voor meer informatie over het transformeren van gegevens met Azure Data Factory.
> 
> U kunt twee activiteiten koppelen (de ene activiteit na de andere laten uitvoeren) door de uitvoergegevensset van één activiteit in te stellen als invoergegevensset voor een andere activiteit. Zie [Planning en uitvoering in Data Factory](data-factory-scheduling-and-execution.md) voor gedetailleerde informatie.

## <a name="see-also"></a>Zie ook
| Onderwerp | Beschrijving |
|:--- |:--- |
| [Pijplijnen](data-factory-create-pipelines.md) |Met behulp van dit artikel krijgt u inzicht in de pijplijnen en activiteiten in Azure Data Factory en in de wijze waarop u deze kunt gebruiken om end-to-end gegevensgestuurde werkstromen te maken voor uw scenario of bedrijf. |
| [Gegevenssets](data-factory-create-datasets.md) |Op basis van dit artikel krijgt u inzicht in de gegevenssets in Azure Data Factory. |
| [Plannen en uitvoeren](data-factory-scheduling-and-execution.md) |In dit artikel wordt uitleg gegeven over de plannings- en uitvoeringsaspecten van het Azure Data Factory-toepassingsmodel. |




<!--HONumber=Feb17_HO1-->


