---
title: Gegevens kopiëren met behulp van het hulpprogramma Azure Copy Data | Microsoft Docs
description: Maak een Azure data factory aan en gebruik vervolgens het hulpprogramma Copy Data om gegevens vanuit de ene locatie in Azure Blob Storage te kopiëren naar een andere locatie.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/16/2018
ms.author: jingwang
ms.openlocfilehash: aea76544f244adba8368a1d8fbe268746060b5a6
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="use-the-copy-data-tool-to-copy-data"></a>Het hulpprogramma Copy Data gebruiken om gegevens te kopiëren 
> [!div class="op_single_selector" title1="Select the version of Data Factory service that you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versie 2 - Preview](quickstart-create-data-factory-copy-data-tool.md)

In deze snelstart gebruikt u Azure Portal voor het maken van een data factory. Gebruik vervolgens het hulpprogramma Copy Data om een pijplijn te maken waarmee gegevens uit een map in Azure Blob Storage worden gekopieerd naar een andere map. 

> [!NOTE]
> Als u niet bekend bent met Azure Data Factory, raadpleegt u eerst de [Inleiding voor Azure Data Factory](data-factory-introduction.md) voordat u deze quickstart uitvoert. 
>
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de service gebruikt, die algemeen beschikbaar is, raadpleegt u [Aan de slag met versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Selecteer **Nieuw** in het menu links, selecteer **Gegevens en analyses** en selecteer vervolgens **Data Factory**. 
   
   ![Selectie van Data Factory in het deelvenster Nieuw](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory-menu.png)
2. Voer op de pagina **Nieuwe data factory** **ADFTutorialDataFactory** in als **naam**. 
      
   ![Pagina Nieuwe data factory](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet *wereldwijd uniek* zijn. Als u het volgende foutbericht ziet, wijzigt u de naam van de data factory (bijvoorbeeld **&lt;uwnaam&gt;ADFTutorialDataFactory**) en probeert u het opnieuw. Zie het artikel [Data factory - Naamgevingsregels](naming-rules.md) voor naamgevingsregels voor Data Factory-artefacten.
  
   ![Fout als de naam niet beschikbaar is](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
3. Selecteer voor **Abonnement** het Azure-abonnement waarin u de gegevensfactory wilt maken. 
4. Voer een van de volgende stappen uit voor **Resourcegroep**:
     
   - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de lijst. 
   - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
   Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
4. Selecteer **V2 (Preview)** voor **Versie**.
5. Selecteer voor **Locatie** een locatie voor de data factory. 

   In de lijst worden alleen ondersteunde locaties getoond. De gegevensarchieven (zoals Azure Storage en Azure SQL Database) en -berekeningen (zoals Azure HDInsight) die Data Factory gebruikt, kunnen zich in andere locaties of regio's bevinden.

6. Selecteer **Vastmaken aan dashboard**.     
7. Selecteer **Maken**.
8. Op het dashboard ziet u de volgende tegel met de status: **Data Factory implementeren**: 

    ![Tegel Data Factory implementeren](media/quickstart-create-data-factory-copy-data-tool/deploying-data-factory.png)
9. Wanneer het maken is voltooid, ziet u de pagina **Data Factory**. Selecteer de tegel **Maken en controleren** om de gebruikersinterface (UI) van Azure Data Factory te openen op een afzonderlijk tabblad.
   
   ![Startpagina van Data Factory, met de tegel Maken en controleren](./media/quickstart-create-data-factory-copy-data-tool/data-factory-home-page.png)

## <a name="start-the-copy-data-tool"></a>Het hulpprogramma Copy Data starten

1. Selecteer op de pagina **Aan de slag** de tegel **Copy Data** om het hulpprogramma Copy Data te starten. 

   ![Tegel Copy Data](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-tile.png)
2. Op de pagina **Eigenschappen** van het hulpprogramma Copy Data selecteert u **Volgende**. Op deze pagina kunt u een naam voor de pijplijn met een beschrijving opgeven. 

   ![Pagina Eigenschappen](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-properties-page.png)
3. Selecteer op de pagina **Brongegevensarchief** de optie **Azure Blob Storage** en selecteer vervolgens **Volgende**.

   ![Pagina Brongegevensarchief](./media/quickstart-create-data-factory-copy-data-tool/source-data-store-page.png)
4. Selecteer op de pagina **Azure Blob Storage-account opgeven** uw opslagaccount in de lijst **Naam van opslagaccount** en selecteer **Volgende**. 

   ![Pagina Azure Blob Storage-account opgeven](./media/quickstart-create-data-factory-copy-data-tool/specify-blob-storage-account.png)
5. Voltooi op de pagina **Invoerbestand of invoermap kiezen** de volgende stappen:

   a. Blader naar de map **adftutorial/input**.

   b. Selecteer het bestand **emp.txt**.

   c. Selecteer **Kiezen**. U kunt dubbelklikken op **emp.txt** om deze stap over te slaan.

   d. Selecteer **Volgende**. 

   ![Pagina Invoerbestand of invoermap kiezen](./media/quickstart-create-data-factory-copy-data-tool/choose-input-file-folder.png)
6. Op de pagina **Instellingen bestandsindelingen**, ziet u dat het hulpprogramma automatisch de scheidingstekens voor kolommen en rijen detecteert. Selecteer **Volgende**. U kunt ook een voorbeeld van gegevens en schema's van de ingevoerde gegevens op deze pagina bekijken. 

   ![Pagina Instellingen bestandsindelingen](./media/quickstart-create-data-factory-copy-data-tool/file-format-settings-page.png)
7. Selecteer op de pagina **Doelgegevensarchief** de optie **Azure Blob Storage** en selecteer **Volgende**. 

   ![Pagina Doelgegevensarchief](./media/quickstart-create-data-factory-copy-data-tool/destination-data-store-page.png)    
8. Selecteer op de pagina **Azure Blob Storage-account opgeven** uw Azure Blob Storage-account en selecteer **Volgende**. 

   ![Pagina Azure Blob Storage-account opgeven](./media/quickstart-create-data-factory-copy-data-tool/specify-sink-blob-storage-account.png)
9. Voltooi op de pagina **Uitvoerbestand of uitvoermap kiezen** de volgende stappen: 

   a. Voer **adftutorial/output** in voor het mappad.

   b. Voer **emp.txt** in voor de bestandsnaam.

   c. Selecteer **Volgende**. 

   ![Pagina Uitvoerbestand of uitvoermap kiezen](./media/quickstart-create-data-factory-copy-data-tool/choose-output-file-folder.png) 
10. Selecteer op de pagina **Instellingen Bestandsindelingen** de optie **Volgende**. 

    ![Pagina Instellingen bestandsindelingen](./media/quickstart-create-data-factory-copy-data-tool/file-format-settings-output-page.png)
11. Selecteer op de pagina **Instellingen** de optie **Volgende**. 

    ![Pagina Instellingen](./media/quickstart-create-data-factory-copy-data-tool/advanced-settings-page.png)
12. Controleer de instellingen op de pagina **Samenvatting** en selecteer **Volgende**. 

    ![Pagina Overzicht](./media/quickstart-create-data-factory-copy-data-tool/summary-page.png)
13. Selecteer op de pagina **Implementatie voltooid** de optie **Controleren** om de pijplijn of taak die u hebt gemaakt, te controleren. 

    ![Pagina Implementatie voltooid](./media/quickstart-create-data-factory-copy-data-tool/deployment-page.png)
14. De toepassing wordt overgeschakeld naar het **Monitor**-tabblad. U ziet de status van de pijplijn op dit tabblad. Selecteer **Vernieuwen** om de lijst te vernieuwen. 
    
    ![Tabblad voor het controleren van pijplijnuitvoeringen met knop Vernieuwen](./media/quickstart-create-data-factory-copy-data-tool/monitor-pipeline-runs-page.png)
15. Selecteer de koppeling **Uitvoeringen van activiteit weergeven** in de kolom **Acties**. De pijplijn heeft slechts één activiteit van het type **Kopiëren**. 

    ![Lijst met uitvoeringen van activiteit](./media/quickstart-create-data-factory-copy-data-tool/activity-runs.png)
16. Selecteer de koppeling **Details** (afbeelding van een bril) in de kolom **Acties** om details over de kopieerbewerking weer te geven. Zie [Overzicht van kopieeractiviteit](copy-activity-overview.md) voor meer informatie over de eigenschappen. 

    ![Details van de kopieerbewerking](./media/quickstart-create-data-factory-copy-data-tool/copy-operation-details.png)
17. Controleer of er een **emp.txt**- bestand is gemaakt in de **uitvoermap** van de container **adftutorial**. Als de uitvoermap niet bestaat, wordt deze automatisch aangemaakt in de Data Factory-service. 
18. Schakel over naar het tabblad **Bewerken** zodat u gekoppelde services, gegevenssets en pijplijnen kunt bewerken. Zie [Een gegevensfactory maken in Azure Portal](quickstart-create-data-factory-portal.md) voor meer informatie over het bewerken hiervan in de gebruikersinterface van Data Factory.

    ![Tabblad Bewerken](./media/quickstart-create-data-factory-copy-data-tool/edit-tab.png)

## <a name="next-steps"></a>Volgende stappen
Met de pijplijn in dit voorbeeld worden gegevens gekopieerd van de ene locatie naar een andere locatie in Azure Blob Storage. Doorloop de [zelfstudies](tutorial-copy-data-portal.md) voor meer informatie over het gebruiken van Data Factory in andere scenario's. 