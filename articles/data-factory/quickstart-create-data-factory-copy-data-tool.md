---
title: "Gegevens kopiëren met behulp van het hulpprogramma Azure Copy Data | Microsoft Docs"
description: "Maak een Azure data factory aan en gebruik vervolgens het hulpprogramma Copy Data om gegevens vanuit een map in een Azure Blob-opslag te kopiëren naar een andere map."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/16/2018
ms.author: jingwang
ms.openlocfilehash: c17e738e3db18503f7cdda24a5f01ceb78e4e6a3
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="use-copy-data-tool-to-copy-data"></a>Het hulpprogramma Copy Data gebruiken om gegevens te kopiëren 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versie 2 - Preview](quickstart-create-data-factory-copy-data-tool.md)

In deze quickstart gebruikt u Azure Portal voor het maken van een data factory. Gebruik vervolgens het hulpprogramma Copy Data om een pijplijn te maken waarmee gegevens uit een map in een Azure Blob-opslag worden gekopieerd naar een andere map. 

> [!NOTE]
> Als u niet bekend bent met Azure Data Factory, raadpleegt u eerst de [Inleiding voor Azure Data Factory](data-factory-introduction.md) voordat u deze quickstart uitvoert. 
>
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Aan de slag met versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory-menu.png)
2. Voer op de pagina **Nieuwe data factory** **ADFTutorialDataFactory** in als **naam**. 
      
     ![Pagina nieuwe data factory](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als het volgende foutbericht wordt weergegeven voor het naamveld, wijzigt u de naam van de data factory (bijvoorbeeld uwnaamADFTutorialDataFactory) en probeert u deze opnieuw aan te maken. Zie het artikel [Data Factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
  
     ![Naam niet beschikbaar - fout](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
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

    ![tegel met de status 'gegevensfactory implementeren'](media/quickstart-create-data-factory-copy-data-tool/deploying-data-factory.png)
9. Na het aanmaken ziet u de pagina **Data Factory** zoals weergegeven in de afbeelding.
   
   ![Startpagina van de gegevensfactory](./media/quickstart-create-data-factory-copy-data-tool/data-factory-home-page.png)
10. Klik op de tegel **Author & Monitor** om de gebruikersinterface (UI) van Azure Data Factory te openen in een afzonderlijk tabblad. 

## <a name="launch-copy-data-tool"></a>Het hulpprogramma Copy Data opstarten

1. Klik op de pagina Aan de slag op de tegel **Copy Data** om het hulpprogramma Copy Data op te starten. 

   ![De tegel Copy Data-hulpprogramma](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-tile.png)
2. Op de **Eigenschappen**-pagina van het hupprogramma Copy Data, klikt u op **Volgende**. Op deze pagina kunt u een naam voor de pijplijn met een beschrijving opgeven. 

    ![Copy Data-hulpprogramma - pagina Eigenschappen](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-properties-page.png)
3. Selecteer op de pagina **Brongegevensarchief** de optie **Azure Blob Storage** en klik op **Volgende**.

    ![De pagina Brongegevensarchief](./media/quickstart-create-data-factory-copy-data-tool/source-data-store-page.png)
4. Selecteer op de pagina **Azure Blob-opslagaccount opgeven** uw **opslagaccountnaam** in de vervolgkeuzelijst lijst en klik op Volgende. 

    ![Blob-opslagaccount opgeven](./media/quickstart-create-data-factory-copy-data-tool/specify-blob-storage-account.png)
5. Voer op de pagina **Het invoerbestand of de invoermap kiezen** de volgende stappen uit:

    1. Navigeer naar de map **adftutorial/input**. 
    2. Selecteer het bestand **emp.txt**.
    3. Klik op **Kiezen**. U kunt dubbelklikken op **emp.txt** om deze stap over te slaan. 
    4. Klik op **Volgende**. 

    ![Het invoerbestand of de invoermap kiezen](./media/quickstart-create-data-factory-copy-data-tool/choose-input-file-folder.png)
6. Op de pagina **Instellingen bestandsindelingen**, ziet u ziet dat het hulpprogramma automatisch de scheidingstekens voor kolommen en rijen detecteert. Hier klikt u op **Volgende**. U kunt ook een voorbeeld van gegevens en het schema van de ingevoerde gegevens op deze pagina bekijken. 

    ![De pagina Instellingen bestandsindelingen](./media/quickstart-create-data-factory-copy-data-tool/file-format-settings-page.png)
7. Selecteer op de pagina **Doelgegevensarchief** de optie **Azure Blob Storage** en klik op **Volgende**. 

    ![De pagina Doelgegevensarchief](./media/quickstart-create-data-factory-copy-data-tool/destination-data-store-page.png)    
8. Selecteer op de pagina **Azure Blob -opslagaccount opgeven** uw Azure Blob-opslagaccount en klik op **Volgende**. 

    ![De pagina Sink van gegevensarchief opgeven](./media/quickstart-create-data-factory-copy-data-tool/specify-sink-blob-storage-account.png)
9. Voer op de pagina **Het uitvoerbestand of de uitvoermap kiezen** de volgende stappen uit: 

    1. Voer **adftutorial/output** in voor het **pad van de map**.
    2. Voer **emp.txt** voor de **bestandsnaam**. 
    3. Klik op **Volgende**. 

    ![Het uitvoerbestand of de uitvoermap kiezen](./media/quickstart-create-data-factory-copy-data-tool/choose-output-file-folder.png) 
10. Klik op de pagina **Instellingen Bestandsindelingen** op **Volgende**. 

    ![De pagina Instellingen bestandsindelingen](./media/quickstart-create-data-factory-copy-data-tool/file-format-settings-output-page.png)
11. Klik op de pagina **Instellingen** op **Volgende**. 

    ![De pagina Geavanceerde instellingen](./media/quickstart-create-data-factory-copy-data-tool/advanced-settings-page.png)
12. Controleer de instellingen op de pagina **Samenvatting** en klik op Volgende. 

    ![Overzichtspagina](./media/quickstart-create-data-factory-copy-data-tool/summary-page.png)
13. Klik op de pagina **Implementatie voltooid** op **Controleren** om de pijplijn of taak die u hebt gemaakt, te controleren. 

    ![De pagina Implementatie](./media/quickstart-create-data-factory-copy-data-tool/deployment-page.png)
14. De toepassing wordt overgeschakeld naar het **Monitor**-tabblad. U ziet de status van de pijplijn op deze pagina. Klik op **Vernieuwen** om de lijst te vernieuwen. 
    
    ![Pijplijnuitvoeringen controleren](./media/quickstart-create-data-factory-copy-data-tool/monitor-pipeline-runs-page.png)
15. Klik op de koppeling **Uitvoeringen van activiteit weergeven** onder Acties. De pijplijn heeft slechts één activiteit van het type **Kopiëren**. 

    ![De pagina Uitvoeringen van activiteit weergeven](./media/quickstart-create-data-factory-copy-data-tool/activity-runs.png)
16. Klik op de koppeling **Details** (afbeelding van een bril) in de kolom **Acties** om details over de kopieerbewerking weer te geven. Zie [Overzicht van kopieeractiviteit](copy-activity-overview.md) voor meer informatie over de eigenschappen. 

    ![Details van de kopieerbewerking](./media/quickstart-create-data-factory-copy-data-tool/copy-operation-details.png)
17. Controleer of er een **emp.txt**- bestand is gemaakt in de **uitvoermap** van de container **adftutorial**. Als de uitvoermap niet bestaat, wordt deze automatisch aangemaakt in de Data Factory-service. 
18. Schakel over naar het tabblad **Bewerken** om gekoppelde services, gegevenssets en pijplijnen te kunnen bewerken. Zie voor meer informatie over het bewerken hiervan in de gebruikersinterface van Data Factory: [Een gegevensfactory maken met Azure portal](quickstart-create-data-factory-portal.md).

    ![Tabblad Bewerken](./media/quickstart-create-data-factory-copy-data-tool/edit-tab.png)

## <a name="next-steps"></a>Volgende stappen
Met de pijplijn in dit voorbeeld worden gegevens gekopieerd van de ene locatie naar een andere locatie in een Azure Blob-opslag. Doorloop de [zelfstudies](tutorial-copy-data-portal.md) voor meer informatie over het gebruiken van Data Factory in andere scenario's. 