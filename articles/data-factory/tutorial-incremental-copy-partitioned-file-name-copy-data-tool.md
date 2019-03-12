---
title: Met Azure Data Factory incrementeel kopiëren van nieuwe bestanden alleen op basis van tijd gepartitioneerde bestandsnaam | Microsoft Docs
description: Maak een Azure data factory en vervolgens het hulpprogramma Copy Data gebruiken voor het incrementeel laden van nieuwe bestanden alleen op basis van tijd gepartitioneerde bestandsnaam.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 1/24/2019
ms.openlocfilehash: df1542d6d20120a9b1e087fadf3743479ecebf07
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57533830"
---
# <a name="incrementally-copy-new-files-based-on-time-partitioned-file-name-by-using-the-copy-data-tool"></a>Nieuwe bestanden op basis van tijd gepartitioneerde bestandsnaam met behulp van het hulpprogramma Copy Data incrementeel kopiëren

In deze zelfstudie gebruikt u Azure Portal om een gegevensfactory te maken. Vervolgens kunt u het hulpprogramma Copy Data gebruiken om een pijplijn waarmee nieuwe bestanden op basis van tijd gepartitioneerde bestandsnaam uit Azure Blob storage naar Azure Blob-opslag stapsgewijs worden gekopieerd te maken. 

> [!NOTE]
> Zie [Inleiding tot Azure Data Factory](introduction.md) als u niet bekend bent met Azure Data Factory.

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een data factory maken.
> * Het hulpprogramma Copy Data gebruiken om een pijplijn te maken.
> * De uitvoering van de pijplijn en van de activiteit controleren.

## <a name="prerequisites"></a>Vereisten

* **Azure-abonnement**: Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.
* **Azure-opslagaccount**: Gebruik van Blob-opslag als de _bron_ en _sink_ gegevensarchief. Als u geen Azure-opslagaccount hebt, raadpleegt u de instructies in [Een opslagaccount maken](../storage/common/storage-quickstart-create-account.md).

### <a name="create-two-containers-in-blob-storage"></a>Twee containers maken in Blob-opslag

De Blob-opslag voorbereiden voor de zelfstudie door deze stappen uit te voeren.

1. Maak een container met de naam **bron**.  Maken van een mappad als **2019/02/26/14** in de container. Maak een leeg tekstbestand en als de naam **Bestand1.txt**. De Bestand1.txt uploaden naar het pad naar de **bron/2019/02/26/14** in uw opslagaccount.  U kunt verschillende hulpprogramma's gebruiken om deze taken uit te voeren, zoals [Azure Storage Explorer](https://storageexplorer.com/).
    
    ![Bestanden uploaden](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/upload-file.png)
    
    > [!NOTE]
    > Wijzig de naam van de map met de UTC-tijd.  Bijvoorbeeld, als de huidige UTC-tijd 2:03 PM 26 februari 2019 is, kunt u het pad naar de map als **bron/2019/02/26/14/** door de regel van **bron / {Year} / {Month} / {Day} / {Hour} /**.

2. Maak een container met de naam **bestemming**. U kunt verschillende hulpprogramma's gebruiken om deze taken uit te voeren, zoals [Azure Storage Explorer](https://storageexplorer.com/).

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Selecteer in het linkermenu **+ Nieuw** > **Gegevens en analyses** > **Data Factory**: 
   
   ![Nieuwe data factory maken](./media/tutorial-copy-data-tool/new-azure-data-factory-menu.png)
2. Voer op de pagina **Nieuwe data factory** **ADFTutorialDataFactory** in bij **Naam**. 
      
    ![Nieuwe data factory](./media/tutorial-copy-data-tool/new-azure-data-factory.png)
    
    De naam van de data factory moet _wereldwijd uniek_ zijn. Mogelijk wordt het volgende foutbericht weergegeven:
   
   ![Foutbericht nieuwe data factory](./media/tutorial-copy-data-tool/name-not-available-error.png)
   
   Als u een foutbericht ontvangt dat betrekking heeft op de waarde die bij de naam is ingevuld, voert u een andere naam in voor de data factory. Gebruik bijvoorbeeld de naam _**uwnaam**_**ADFTutorialDataFactory**. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
3. Selecteer het Azure-**abonnement** waarin u de nieuwe data factory wilt maken. 
4. Voer een van de volgende stappen uit voor **Resourcegroep**:
     
    a. Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst.

    b. Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in. 
         
    Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.

5. Selecteer bij **Versie** de optie **V2** als de versie.
6. Selecteer bij **Locatie** de locatie voor de data factory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevensarchieven (bijvoorbeeld Azure Storage en SQL Database) en -berekeningen (bijvoorbeeld Azure HDInsight) die door uw data factory worden gebruikt, kunnen zich in andere locaties of regio's bevinden.
7. Selecteer **Vastmaken aan dashboard**. 
8. Selecteer **Maken**.
9. Op het dashboard wordt op de tegel **Data Factory implementeren** de processtatus weergegeven.

    ![Tegel Data Factory implementeren](media/tutorial-copy-data-tool/deploying-data-factory.png)
10. Nadat de data factory is gemaakt, wordt de startpagina **Data Factory** weergegeven.
   
    ![Startpagina van de gegevensfactory](./media/tutorial-copy-data-tool/data-factory-home-page.png)
11. Selecteer de tegel **Author & Monitor** om de gebruikersinterface (UI) van Azure Data Factory te openen in een afzonderlijk tabblad. 

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Het hulpprogramma Copy Data gebruiken om een pijplijn te maken

1. Op de **aan de slag** weergeeft, schakelt de **Copy Data** titel van het hulpprogramma Copy Data starten. 

   ![De tegel Copy Data-hulpprogramma](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
   
2. Op de **eigenschappen** pagina, de volgende stappen uit:

    a. Onder **taaknaam**, voer **DeltaCopyFromBlobPipeline**.

    b. Onder **uitgebracht van de taak of de planning van taak**, selecteer **regelmatig worden uitgevoerd volgens schema**.

    c. Onder **van het type activeert**, selecteer **Tumblingvenster**.
    
    d. Onder **terugkeerpatroon**, voer **1 uur**. 
    
    e. Selecteer **Volgende**. 
    
    De gebruikersinterface van Data Factory maakt een pijplijn met de opgegeven taaknaam. 

    ![De pagina Eigenschappen](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/copy-data-tool-properties-page.png)
3. Voltooi op de pagina **Brongegevensarchief** de volgende stappen:

    a. Klik op **+ nieuwe verbinding maken**om toe te voegen een verbinding.

    ![De pagina Brongegevensarchief](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page.png)
    
    b. Selecteer **Azure Blob Storage** uit de galerie en klik vervolgens op **doorgaan**.

    ![De pagina Brongegevensarchief](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page-select-blob.png)
    
    c. Op de **nieuwe gekoppelde Service** pagina, selecteert u uw storage-account van de **opslagaccountnaam** lijst en klik vervolgens op **voltooien**.
    
    ![De pagina Brongegevensarchief](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page-linkedservice.png)
    
    d. Selecteer de zojuist gemaakte gekoppelde service en klik vervolgens op **volgende**. 
    
   ![De pagina Brongegevensarchief](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page-select-linkedservice.png)
4. Voer op de pagina **Het invoerbestand of de invoermap kiezen** de volgende stappen uit:
    
    a. Blader en selecteer de **bron** container, selecteert u vervolgens **kiezen**.
    
    ![Kies het invoerbestand of invoermap.](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/choose-input-file-folder.png)
    
    b. Onder **bestand laden gedrag**, selecteer **incrementeel laden: map/bestand tijd gepartitioneerd namen**.
    
    ![Kies het invoerbestand of invoermap.](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/choose-loading-behavior.png)
    
    c. Het dynamische pad als schrijven **bron / {year} / {month} / {day} / {hour} /**, en de indeling wijzigen als het volgende:
    
    ![Kies het invoerbestand of invoermap.](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/input-file-name.png)
    
    d. Controleer **binaire kopie** en klikt u op **volgende**.
    
    ![Kies het invoerbestand of invoermap.](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/check-binary-copy.png)     
5. Op de **doelgegevensarchief** weergeeft, schakelt de **Azure BLOB Storage**, dit is dezelfde opslag als gegevensopslag voor de bron-account en klik vervolgens op **volgende**.

    ![De pagina Doelgegevensarchief](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/destination-data-store-page-select-linkedservice.png) 
6. Op de **uitvoerbestand of uitvoermap kiezen** pagina, de volgende stappen uit:
    
    a. Blader en selecteer de **bestemming** map, klikt u vervolgens op **kiezen**.
    
    ![Het uitvoerbestand of de uitvoermap kiezen](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/choose-output-file-folder.png)   
    
    b. Het dynamische pad als schrijven **bron / {year} / {month} / {day} / {hour} /**, en de indeling wijzigen als het volgende:
    
    ![Het uitvoerbestand of de uitvoermap kiezen](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/input-file-name2.png)    
    
    c. Klik op **volgende**.
    
    ![Het uitvoerbestand of de uitvoermap kiezen](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/click-next-after-output-folder.png)  
7. Selecteer op de pagina **Instellingen** de optie **Volgende**. 

    ![De pagina Instellingen](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/settings-page.png)  
8. Bekijk op de **Overzichtspagina** de waarden voor alle instellingen en selecteer vervolgens **Volgende**.

    ![Overzichtspagina](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/summary-page.png)
    
9. Selecteer op de pagina **Implementatie** de optie **Controleren** om de pijplijn of taak te controleren.
    ![Implementatiepagina](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/deployment-page.png)
    
10. U ziet dat het tabblad **Controleren** aan de linkerkant automatisch wordt geselecteerd.  U moet wachten op de pijplijn die wordt uitgevoerd wanneer deze wordt automatisch geactiveerd (over na een uur).  Wanneer deze wordt uitgevoerd, de **acties** kolom bevat koppelingen om details uitvoering van activiteit weer te geven en de pijplijn opnieuw uitvoeren. Selecteer **vernieuwen** Vernieuw de lijst en selecteer de **uitvoeringen van activiteit weergeven** herstelkoppeling in de **acties** kolom. 

    ![Pijplijnuitvoeringen controleren](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs1.png)
11. Omdat er slechts één activiteit (kopieeractiviteit) in de pijplijn is, ziet u slechts één vermelding in de lijst. U ziet dat het bronbestand (Bestand1.txt) zijn gekopieerd vanuit **bron/2019/02/26/14/** naar **bestemming/2019/02/26/14/** met dezelfde naam.  

    ![Pijplijnuitvoeringen controleren](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs2.png)
    
    U kunt dezelfde ook controleren met behulp van Azure Storage Explorer (https://storageexplorer.com/) om de bestanden te scannen.
    
    ![Pijplijnuitvoeringen controleren](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs3.png)
12. Nog een leeg tekstbestand maken met de nieuwe naam als **bestand2.txt samengevoegd**. Upload het bestand Bestand2.txt samengevoegd naar het pad naar de **bron/2019/02/26/15** in uw opslagaccount.   U kunt verschillende hulpprogramma's gebruiken om deze taken uit te voeren, zoals [Azure Storage Explorer](https://storageexplorer.com/).   
    
    ![Pijplijnuitvoeringen controleren](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs4.png)
    
    > [!NOTE]
    > Het is mogelijk dat u op de hoogte dat een nieuw pad is vereist om te worden gemaakt. Wijzig de naam van de map met de UTC-tijd.  Bijvoorbeeld, als de huidige UTC-tijd 3:20 uur op 26 februari 2019 is, kunt u het pad naar de map als **bron/2019/02/26/15/** door de regel van **{Year} / {Month} / {Day} / {Hour} /**.
    
13. Terugkeren naar de **Pijplijnuitvoeringen** weergave, selecteer **alle pijplijnen uitvoeringen**, en wacht tot de dezelfde pijplijn opnieuw wordt geactiveerd na een andere één uur automatisch.  

    ![Pijplijnuitvoeringen controleren](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs5.png)

14. Selecteer **weergave activiteit uitgevoerd** voor de tweede pijplijn uitvoeren wanneer deze wordt geleverd, en doe hetzelfde voor meer informatie.  

    ![Pijplijnuitvoeringen controleren](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs6.png)
    
    U ziet dat het bronbestand (bestand2.txt samengevoegd) zijn gekopieerd vanuit **bron/2019/02/26/15/** naar **bestemming/2019/02/26/15/** met dezelfde bestandsnaam.
    
    ![Pijplijnuitvoeringen controleren](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs7.png) 
    
    U kunt dezelfde ook controleren met behulp van Azure Storage Explorer (https://storageexplorer.com/) voor het scannen van de bestanden in **bestemming** container
    
    ![Pijplijnuitvoeringen controleren](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs8.png)

    
## <a name="next-steps"></a>Volgende stappen
Ga naar de volgende zelfstudie voor meer informatie over het transformeren van gegevens met behulp van een Spark-cluster in Azure:

> [!div class="nextstepaction"]
>[Gegevens transformeren met behulp van een Spark-cluster in de cloud](tutorial-transform-data-spark-portal.md)