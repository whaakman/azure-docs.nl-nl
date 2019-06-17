---
title: Incrementeel kopiëren nieuwe en gewijzigde bestanden op basis van LastModifiedDate met behulp van het hulpprogramma Copy Data | Microsoft Docs
description: Maak een Azure data factory en gebruik vervolgens het hulpprogramma Copy Data incrementeel laden van nieuwe bestanden op basis van LastModifiedDate.
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
ms.openlocfilehash: 3865bb10346c4a55adbf94a7df225eacf2c11252
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65519133"
---
# <a name="incrementally-copy-new-and-changed-files-based-on-lastmodifieddate-by-using-the-copy-data-tool"></a>Nieuwe en gewijzigde bestanden op basis van LastModifiedDate met behulp van het hulpprogramma Copy Data incrementeel kopiëren

In deze zelfstudie gebruikt u de Azure portal om een gegevensfactory te maken. Vervolgens gebruikt u het hulpprogramma Copy Data om te maken van een pijplijn waarmee stapsgewijs worden gekopieerd, nieuwe en gewijzigde bestanden op basis van hun **LastModifiedDate** uit Azure Blob storage naar Azure Blob storage.

Op deze manier wordt ADF scant alle bestanden uit de brongegevensopslag, past u het bestandsfilter door hun LastModifiedDate en kopieer het bestand nieuwe en bijgewerkte alleen sinds de laatste keer dat naar het doelarchief.  Houd er rekening mee dat als u grote hoeveelheden van ADF scannen van bestanden, kunnen maar alleen enkele bestanden naar de bestemming kopiëren, u nog steeds verwacht dat de lange duur van de vanwege de bestandscontrole is ook tijd in beslag nemen.   

> [!NOTE]
> Zie [Inleiding tot Azure Data Factory](introduction.md) als u niet bekend bent met Azure Data Factory.

In deze zelfstudie wordt u de volgende taken uitvoeren:

> [!div class="checklist"]
> * Een data factory maken.
> * Het hulpprogramma Copy Data gebruiken om een pijplijn te maken.
> * De uitvoering van de pijplijn en van de activiteit controleren.

## <a name="prerequisites"></a>Vereisten

* **Azure-abonnement**: Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.
* **Azure-opslagaccount**: Gebruik van Blob-opslag als de _bron_ en _sink_ gegevensarchief. Als u geen Azure-opslagaccount hebt, raadpleegt u de instructies in [Een opslagaccount maken](../storage/common/storage-quickstart-create-account.md).

### <a name="create-two-containers-in-blob-storage"></a>Twee containers maken in Blob-opslag

De Blob-opslag voorbereiden voor de zelfstudie door deze stappen uit te voeren.

1. Maak een container met de naam **bron**. U kunt verschillende hulpprogramma's gebruiken voor het uitvoeren van deze taak, zoals [Azure Storage Explorer](https://storageexplorer.com/).

2. Maak een container met de naam **bestemming**. 

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Selecteer in het menu links **een resource maken** > **gegevens en analyses** > **Data Factory**: 
   
   ![Selectie van Data Factory in het deelvenster Nieuw](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. Voer op de pagina **Nieuwe data factory** **ADFTutorialDataFactory** in bij **Naam**. 
      
     ![Nieuwe data factory](./media/tutorial-copy-data-tool/new-azure-data-factory.png)
 
   De naam van de data factory moet _wereldwijd uniek_ zijn. Mogelijk wordt het volgende foutbericht weergegeven:
   
   ![Foutbericht nieuwe data factory](./media/tutorial-copy-data-tool/name-not-available-error.png)

   Als u een foutbericht ontvangt dat betrekking heeft op de waarde die bij de naam is ingevuld, voert u een andere naam in voor de data factory. Gebruik bijvoorbeeld de naam _**uwnaam**_ **ADFTutorialDataFactory**. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
3. Selecteer het Azure **abonnement** in die u de nieuwe data factory maakt. 
4. Voer een van de volgende stappen uit voor **Resourcegroep**:
     
    * Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst.

    * Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in. 
         
    Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.

5. Onder **versie**, selecteer **V2**.
6. Selecteer bij **Locatie** de locatie voor de data factory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevensarchieven (bijvoorbeeld Azure Storage en SQL Database) en berekeningen (bijvoorbeeld Azure HDInsight) die gebruikmaakt van uw data factory, kunnen zich in andere locaties of regio's.
7. Selecteer **Vastmaken aan dashboard**. 
8. Selecteer **Maken**.
9. Op het dashboard, verwijzen naar de **Data Factory implementeren** tegel om het proces weer te geven.

    ![Tegel Data Factory implementeren](media/tutorial-copy-data-tool/deploying-data-factory.png)
10. Nadat de data factory is gemaakt, wordt de startpagina **Data Factory** weergegeven.
   
    ![Startpagina van de gegevensfactory](./media/tutorial-copy-data-tool/data-factory-home-page.png)
11. Als u wilt openen in de Azure Data Factory-gebruikersinterface (UI) op een afzonderlijk tabblad, selecteert u de **Author & Monitor** tegel. 

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Het hulpprogramma Copy Data gebruiken om een pijplijn te maken

1. Op de **aan de slag** weergeeft, schakelt de **Copy Data** titel te openen van het hulpprogramma Copy Data. 

   ![De tegel Copy Data-hulpprogramma](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
   
2. Op de **eigenschappen** pagina, de volgende stappen uit:

    a. Onder **taaknaam**, voer **DeltaCopyFromBlobPipeline**.

    b. Onder **taak uitgebracht** of **taakschema**, selecteer **regelmatig worden uitgevoerd volgens schema**.

    c. Onder **triggertype**, selecteer **Tumblingvenster**.
    
    d. Onder **terugkeerpatroon**, voer **15 minuten**. 
    
    e. Selecteer **Next**. 
    
    De gebruikersinterface van Data Factory maakt een pijplijn met de opgegeven taaknaam. 

    ![De pagina Eigenschappen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/copy-data-tool-properties-page.png)
    
3. Voltooi op de pagina **Brongegevensarchief** de volgende stappen:

    a. Selecteer **+ nieuwe verbinding maken**om toe te voegen een verbinding.
    
    ![De pagina Brongegevensarchief](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page.png)

    b. Selecteer **Azure Blob Storage** uit de galerie, en selecteer vervolgens **doorgaan**.
    
    ![De pagina Brongegevensarchief](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-select-blob.png)

    c. Op de **nieuwe gekoppelde Service** pagina, selecteert u uw storage-account van de **opslagaccountnaam** lijst en selecteer vervolgens **voltooien**.
    
    ![De pagina Brongegevensarchief](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-linkedservice.png)
    
    d. Selecteer de zojuist gemaakte gekoppelde service en selecteer vervolgens **volgende**. 
    
   ![De pagina Brongegevensarchief](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-select-linkedservice.png)

4. Voltooi op de pagina **Invoerbestand of invoermap kiezen** de volgende stappen:
    
    a. Blader en selecteer de **bron** map en selecteer vervolgens **kiezen**.
    
    ![Kies het invoerbestand of invoermap.](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-input-file-folder.png)
    
    b. Onder **bestand laden gedrag**, selecteer **incrementeel laden: LastModifiedDate**.
    
    ![Kies het invoerbestand of invoermap.](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-loading-behavior.png)
    
    c. Controleer **binaire kopie** en selecteer **volgende**.
    
     ![Kies het invoerbestand of invoermap.](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/check-binary-copy.png)
     
5. Op de **doelgegevensarchief** weergeeft, schakelt **Azure BLOB Storage**. Dit is hetzelfde opslagaccount als de bron-gegevensopslag. Selecteer vervolgens **Volgende**.

    ![De pagina Doelgegevensarchief](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/destination-data-store-page-select-linkedservice.png)
    
6. Voltooi op de pagina **Uitvoerbestand of uitvoermap kiezen** de volgende stappen:
    
    a. Blader en selecteer de **bestemming** map en selecteer vervolgens **kiezen**.
    
    ![Het uitvoerbestand of de uitvoermap kiezen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-output-file-folder.png)
    
    b. Selecteer **Next**.
    
     ![Het uitvoerbestand of de uitvoermap kiezen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/click-next-after-output-folder.png)
    
7. Selecteer op de pagina **Instellingen** de optie **Volgende**. 

    ![De pagina Instellingen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/settings-page.png)
    
8. Op de **samenvatting** pagina, Controleer de instellingen en selecteer vervolgens **volgende**.

    ![Overzichtspagina](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/summary-page.png)
    
9. Selecteer op de pagina **Implementatie** de optie **Controleren** om de pijplijn of taak te controleren.

    ![De pagina Implementatie](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/deployment-page.png)
    
10. U ziet dat het tabblad **Controleren** aan de linkerkant automatisch wordt geselecteerd. De kolom **Acties** bevat onder andere koppelingen om details van de uitvoering van activiteiten te bekijken en de pijplijn opnieuw uit te voeren. Selecteer **vernieuwen** Vernieuw de lijst en selecteer de **uitvoeringen van activiteit weergeven** herstelkoppeling in de **acties** kolom. 

    ![Lijst vernieuwen en selecteert u uitvoeringen van activiteit weergeven](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs1.png)

11. Er is slechts één activiteit (kopieeractiviteit) in de pijplijn ziet u slechts één vermelding. Selecteer de koppeling **Details** (pictogram van een bril) in de kolom **Acties** om details over de kopieerbewerking te zien. 

    ![Copy-activiteit is in de pijplijn](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs2.png)
    
    Omdat er is geen bestand in de **bron** -container in uw Blob storage-account, niet ziet u een bestand gekopieerd naar de **bestemming** container in uw Blob storage-account.
    
    ![Er is geen bestand in Broncontainer of bestemmingscontainer](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs3.png)
    
12. Maak een leeg tekstbestand en noem dit **Bestand1.txt**. Upload dit tekstbestand naar de **bron** container in uw opslagaccount. U kunt verschillende hulpprogramma's gebruiken om deze taken uit te voeren, zoals [Azure Storage Explorer](https://storageexplorer.com/).   

    ![Bestand1.txt maken en uploaden naar Broncontainer](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs3-1.png)
    
13. Terugkeren naar de **Pijplijnuitvoeringen** weergave, selecteer **alle Pijplijnuitvoeringen**, en wacht tot de dezelfde pijplijn automatisch opnieuw wordt geactiveerd.  

    ![Selecteer alle Pijplijnuitvoeringen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs4.png)

14. Selecteer **weergave activiteit uitgevoerd** voor de tweede pijplijn-run wanneer u het te zien. Bekijk de details op dezelfde manier als die u dit hebt gedaan voor de eerste pijplijn-run.  

    ![Selecteer Weergave activiteit uitgevoerd en informatie](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs5.png)

    U gaat dat Zie een bestand (Bestand1.txt) zijn gekopieerd vanuit de **bron** container de **bestemming** container van uw Blob storage-account.
    
    ![Bestand1.txt zijn gekopieerd vanuit de Broncontainer naar doelcontainer](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs6.png)
    
15. Maak nog een leeg tekstbestand en noem dit **bestand2.txt samengevoegd**. Upload dit tekstbestand naar de **bron** container in uw Blob storage-account.   
    
16. Herhaal stap 13 en 14 voor dit tweede tekstbestand. U ziet dat alleen voor het nieuwe bestand (bestand2.txt samengevoegd) zijn gekopieerd vanuit de **bron** container te maken voor de **bestemming** container van uw opslagaccount in de volgende pijplijn-run.  
    
    ![Bestand2.txt samengevoegd is uit Broncontainer gekopieerd naar de doelcontainer](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs7.png)

    U kunt dit ook controleren met behulp van [Azure Storage Explorer](https://storageexplorer.com/) om de bestanden te scannen.
    
    ![Scannen van bestanden met behulp van Azure Storage Explorer](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs8.png)

    
## <a name="next-steps"></a>Volgende stappen
Ga naar de volgende zelfstudie voor meer informatie over het transformeren van gegevens met behulp van een Apache Spark-cluster op Azure:

> [!div class="nextstepaction"]
>[Gegevens in de cloud transformeren met behulp van een Apache Spark-cluster](tutorial-transform-data-spark-portal.md)