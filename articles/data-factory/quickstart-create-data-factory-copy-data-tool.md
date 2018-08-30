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
ms.topic: quickstart
ms.date: 06/20/2018
ms.author: jingwang
ms.openlocfilehash: d314c04a40155fccc99660bacdb9f646ce77b22f
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43107585"
---
# <a name="use-the-copy-data-tool-to-copy-data"></a>Het hulpprogramma Copy Data gebruiken om gegevens te kopiëren 
> [!div class="op_single_selector" title1="Select the version of Data Factory service that you are using:"]
> * [Versie 1:](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Huidige versie](quickstart-create-data-factory-copy-data-tool.md)

In deze snelstart gebruikt u Azure Portal voor het maken van een data factory. Gebruik vervolgens het hulpprogramma Copy Data om een pijplijn te maken waarmee gegevens uit een map in Azure Blob Storage worden gekopieerd naar een andere map. 

> [!NOTE]
> Als u niet bekend bent met Azure Data Factory, raadpleegt u eerst de [Inleiding voor Azure Data Factory](data-factory-introduction.md) voordat u deze quickstart uitvoert. 

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Selecteer **Nieuw** in het menu links, selecteer **Gegevens en analyses** en selecteer vervolgens **Data Factory**. 
   
   ![Selectie van Data Factory in het deelvenster Nieuw](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory-menu.png)
1. Voer op de pagina **Nieuwe data factory** **ADFTutorialDataFactory** in als **naam**. 
      
   ![Pagina Nieuwe data factory](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet *wereldwijd uniek* zijn. Als u het volgende foutbericht ziet, wijzigt u de naam van de data factory (bijvoorbeeld **&lt;uwnaam&gt;ADFTutorialDataFactory**) en probeert u het opnieuw. Zie het artikel [Data factory - Naamgevingsregels](naming-rules.md) voor naamgevingsregels voor Data Factory-artefacten.
  
   ![Fout als de naam niet beschikbaar is](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
1. Selecteer voor **Abonnement** het Azure-abonnement waarin u de gegevensfactory wilt maken. 
1. Voer een van de volgende stappen uit voor **Resourcegroep**:
     
   - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de lijst. 
   - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
   Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
1. Selecteer **V2** als **Versie**.
1. Selecteer voor **Locatie** een locatie voor de data factory. 

   In de lijst worden alleen ondersteunde locaties getoond. De gegevensarchieven (zoals Azure Storage en Azure SQL Database) en -berekeningen (zoals Azure HDInsight) die Data Factory gebruikt, kunnen zich in andere locaties of regio's bevinden.

1. Selecteer **Vastmaken aan dashboard**.     
1. Selecteer **Maken**.
1. Op het dashboard ziet u de volgende tegel met de status: **Data Factory implementeren**: 

    ![Tegel Data Factory implementeren](media/quickstart-create-data-factory-copy-data-tool/deploying-data-factory.png)
1. Wanneer het maken is voltooid, ziet u de pagina **Data Factory**. Selecteer de tegel **Maken en controleren** om de gebruikersinterface (UI) van Azure Data Factory te openen op een afzonderlijk tabblad.
   
   ![Startpagina van Data Factory, met de tegel Maken en controleren](./media/quickstart-create-data-factory-copy-data-tool/data-factory-home-page.png)

## <a name="start-the-copy-data-tool"></a>Het hulpprogramma Copy Data starten

1. Selecteer op de pagina **Aan de slag** de tegel **Copy Data** om het hulpprogramma Copy Data te starten. 

   ![Tegel Copy Data](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-tile.png)

1. Op de pagina **Eigenschappen** van het hulpprogramma Copy Data, kunt u een naam en beschrijving opgeven voor de pijplijn. Selecteer vervolgens **Volgende**. 

   ![Pagina Eigenschappen](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-properties-page.png)
1. Voltooi op de pagina **Brongegevensarchief** de volgende stappen:

    a. Klik op **+ Nieuwe verbinding maken** om een verbinding toe te voegen.

    ![Pagina Brongegevensarchief](./media/quickstart-create-data-factory-copy-data-tool/new-source-linked-service.png)

    b. Selecteer in de galerie de optie **Azure Blob Storage**. Selecteer vervolgens **Volgende**.

    ![Blob-opslag selecteren in de galerie](./media/quickstart-create-data-factory-copy-data-tool/select-blob-source.png)

    c. Selecteer op de pagina **Azure Blob Storage-account opgeven** uw opslagaccount in de lijst **Naam van opslagaccount** en selecteer **Voltooien**. 

   ![Het nieuwe Azure Blob Storage-account configureren](./media/quickstart-create-data-factory-copy-data-tool/configure-blob-storage.png)

   d. Selecteer de zojuist gemaakte gekoppelde service als bron. Klik vervolgens op **Volgende**.

   ![Aan de bron gekoppelde service selecteren](./media/quickstart-create-data-factory-copy-data-tool/select-source-linked-service.png)


1. Voltooi op de pagina **Invoerbestand of invoermap kiezen** de volgende stappen:

   a. Klik op **Bladeren** om naar de map **adftutorial/input** te gaan en selecteer het bestand **emp.txt**. Klik vervolgens op **Kiezen**. 

   ![Pagina Invoerbestand of invoermap kiezen](./media/quickstart-create-data-factory-copy-data-tool/configure-source-path.png)

   d. Schakel de optie **Binair exemplaar** in om het bestand as-is te kopiëren. Selecteer vervolgens **Volgende**. 

   ![Pagina Invoerbestand of invoermap kiezen](./media/quickstart-create-data-factory-copy-data-tool/select-binary-copy.png)


1. Selecteer op de pagina **Doelgegevensarchief** de gekoppelde **Azure Blob Storage**-service die u zojuist hebt gemaakt. Selecteer vervolgens **Volgende**. 

   ![Pagina Doelgegevensarchief](./media/quickstart-create-data-factory-copy-data-tool/select-sink-linked-service.png)

1. Voer op de pagina **Uitvoerbestand of uitvoermap kiezen** voor het mappad in: **adftutorial/output**. Selecteer vervolgens **Volgende**. 

   ![Pagina Uitvoerbestand of uitvoermap kiezen](./media/quickstart-create-data-factory-copy-data-tool/configure-sink-path.png) 

1. Selecteer op de pagina **Instellingen** de optie **Volgende** om de standaardconfiguraties te gebruiken. 

1. Controleer op de pagina **Overzicht** alle instellingen. Selecteer vervolgens **Volgende**. 

    ![Pagina Overzicht](./media/quickstart-create-data-factory-copy-data-tool/summary-page.png)

1. Selecteer op de pagina **Implementatie voltooid** de optie **Controleren** om de pijplijn of taak die u hebt gemaakt, te controleren. 

    ![Pagina Implementatie voltooid](./media/quickstart-create-data-factory-copy-data-tool/deployment-page.png)

1. De toepassing wordt overgeschakeld naar het **Monitor**-tabblad. U ziet de status van de pijplijn op dit tabblad. Selecteer **Vernieuwen** om de lijst te vernieuwen. 
    
    ![De uitvoering van de pijplijn controleren](./media/quickstart-create-data-factory-copy-data-tool/pipeline-monitoring.png)

1. Selecteer de koppeling **Uitvoeringen van activiteit weergeven** in de kolom **Acties**. De pijplijn heeft slechts één activiteit van het type **Kopiëren**. 

    ![Uitvoering van activiteiten controleren](./media/quickstart-create-data-factory-copy-data-tool/activity-monitoring.png)
    
1. Selecteer de koppeling **Details** (afbeelding van een bril) in de kolom **Acties** om details over de kopieerbewerking weer te geven. Zie [Overzicht van kopieeractiviteit](copy-activity-overview.md) voor meer informatie over de eigenschappen.

    ![Details van de kopieerbewerking](./media/quickstart-create-data-factory-copy-data-tool/activity-execution-details.png)

1. Controleer of er een **emp.txt**- bestand is gemaakt in de **uitvoermap** van de container **adftutorial**. Als de uitvoermap niet bestaat, wordt deze automatisch aangemaakt in de Data Factory-service. 

1. Schakel over naar het tabblad **Auteur** boven het tabblad **Monitor** op het linkerpaneel, zodat u de gekoppelde services, gegevenssets en pijplijnen kunt bewerken. Zie [Een gegevensfactory maken in Azure Portal](quickstart-create-data-factory-portal.md) voor meer informatie over het bewerken hiervan in de gebruikersinterface van Data Factory.

## <a name="next-steps"></a>Volgende stappen
Met de pijplijn in dit voorbeeld worden gegevens gekopieerd van de ene locatie naar een andere locatie in Azure Blob Storage. Doorloop de [zelfstudies](tutorial-copy-data-portal.md) voor meer informatie over het gebruiken van Data Factory in andere scenario's. 