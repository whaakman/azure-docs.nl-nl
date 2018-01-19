---
title: Gegevens laden in Azure Data Lake Store met Azure Data Factory | Microsoft Docs
description: "Gebruik Azure Data Factory om gegevens te kopiëren naar Azure Data Lake Store"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/17/2018
ms.author: jingwang
ms.openlocfilehash: 3f73cd65b0ceb3148ce8ceb83d7b4e1be1280077
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="load-data-into-azure-data-lake-store-using-azure-data-factory"></a>Gegevens laden in Azure Data Lake Store met Azure Data Factory

[Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) is een opslagplaats van de hele onderneming hyperschaal voor analytische workloads van big data. Met Azure Data Lake kunt u gegevens van elke grootte, type en opnamesnelheid vastleggen op één enkele locatie voor operationele en experimentele analyses.

Azure Data Factory is een volledig beheerde gegevens cloud-gebaseerde integration-service, die kan worden gebruikt voor het vullen van de lake met de gegevens uit uw bestaande systeem en bespaart u kostbare tijd tijdens het bouwen van uw analyseoplossingen. Hier volgen de belangrijkste voordelen van het laden van gegevens in Azure Data Lake Store met Azure Data Factory:

* **Eenvoudig instellen van**: 5 stap intuïtieve wizard geen scripts nodig.
* **Uitgebreide ondersteuning voor gegevensopslag**: ingebouwde ondersteuning voor een groot aantal on-premises en cloud-gebaseerde gegevensarchieven gedetailleerd overzicht in [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.
* **Beveiligd en compatibel**: gegevens worden overgedragen via HTTPS of ExpressRoute en global service aanwezigheid zorgt ervoor dat uw gegevens de geografische grens nooit verlaten
* **Hoge prestaties**: maximaal 1 Gbps gegevens snelheid laden in Azure Data Lake Store. Meer informatie over de details van [kopiëren van de prestaties van de activiteit](copy-activity-performance.md).

Dit artikel laat zien hoe u gegevens kopiëren van Data Factory-hulpprogramma voor **gegevens vanaf Amazon S3 laden in Azure Data Lake Store**. U kunt de stappen vergelijkbaar voor het kopiëren van gegevens van andere typen gegevensarchieven.

> [!NOTE]
>  Raadpleeg voor algemene informatie over de mogelijkheden van Data Factory bij het kopiëren van gegevens naar/van Azure Data Lake Store [kopiëren van gegevens of naar Azure Data Lake Store met behulp van Azure Data Factory](connector-azure-data-lake-store.md) artikel.
>
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Kopieeractiviteit in V1](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Vereisten

* **Azure-abonnement**. Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* **Azure Data Lake Store**. Als u geen een Data Lake Store-account hebt, raadpleegt u de [een Data Lake Store-account maken](../data-lake-store/data-lake-store-get-started-portal.md#create-an-azure-data-lake-store-account) voor stappen voor het maken van een artikel.
* **Amazon S3**. Dit artikel laat zien hoe u gegevens vanaf Amazon S3 kopieert. U kunt andere gegevensarchieven door gelijksoortige stappen te volgen.

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Klik op **NIEUW** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/load-data-into-azure-data-lake-store/new-azure-data-factory-menu.png)
2. In de **nieuwe gegevensfactory** pagina, de waarden opgeven, zoals wordt weergegeven in de volgende schermafbeelding: 
      
     ![Nieuwe data factory-pagina](./media/load-data-into-azure-data-lake-store//new-azure-data-factory.png)
 
   * **Name.** Voer een globale unieke naam voor de data factory. Als u het volgende foutbericht krijgt, wijzigt u de naam van de gegevensfactory (bijvoorbeeld uwnaamADFTutorialDataFactory) en probeert u het opnieuw. Zie [Data Factory - naamgevingsregels](naming-rules.md) artikel voor naamgevingsregels voor Data Factory-artefacten.
  
            `Data factory name "LoadADLSDemo" is not available`

    * **Abonnement.** Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
    * **Resourcegroep.** Selecteer een bestaande resourcegroep in de vervolgkeuzelijst, of selecteer **nieuw** optie en typ de naam van een resourcegroep. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
    * **Versie.** Selecteer **V2 (Preview)**.
    * **Locatie.** Selecteer de locatie voor de data factory. Alleen ondersteunde locaties worden weergegeven in de vervolgkeuzelijst. De gegevens worden opgeslagen die door data factory wordt gebruikt (Azure Data Lake Store, Azure Storage, Azure SQL Database, etc.) kunnen zich in andere locaties/regio's.

3. Klik op **Create**.
4. Nadat het maken voltooid, gaat u naar uw gegevensfactory is en u ziet de **Data Factory** pagina zoals in de afbeelding. Klik op **auteur & Monitor** tegel om de toepassing van de integratie gegevens op een afzonderlijke tabblad te starten. 
   
   ![Startpagina van de gegevensfactory](./media/load-data-into-azure-data-lake-store/data-factory-home-page.png)

## <a name="load-data-into-azure-data-lake-store"></a>Gegevens laden in Azure Data Lake Store

1. Klik in de pagina aan de slag, **gegevens kopiëren** tegel starten van het hulpprogramma voor kopiëren. 

   ![Kopiëren van gegevens hulpprogramma tegel](./media/load-data-into-azure-data-lake-store/copy-data-tool-tile.png)
2. In de **eigenschappen** pagina opgeven van het hulpprogramma gegevens kopiëren **CopyFromAmazonS3ToADLS** voor de **taaknaam**, en klik op **volgende**. 

    ![Pagina met hulpprogramma gegevenseigenschappen kopiëren](./media/load-data-into-azure-data-lake-store/copy-data-tool-properties-page.png)
3. In de **brongegevensarchief** pagina **Amazon S3**, en klik op **volgende**.

    ![Bronpagina data store](./media/load-data-into-azure-data-lake-store/source-data-store-page.png)
4. In de **Geef Amazon S3 verbinding** pagina, voert u de volgende stappen uit: 
    1. Geef de **toegang tot de sleutel-ID**.
    2. Geef de **geheime toegangssleutel**.
    3. Klik op **Volgende**. 

        ![Amazon S3-account opgeven](./media/load-data-into-azure-data-lake-store/specify-amazon-s3-account.png)
5. In de **het invoerbestand of de invoermap kiezen** pagina, gaat u naar de map/bestand dat u wilt kopiëren via, selecteren en op **Kies**, klikt u vervolgens op **volgende**. 

    ![Invoerbestand of de invoermap kiezen](./media/load-data-into-azure-data-lake-store/choose-input-folder.png)

6. In de **doelgegevensopslagplaats** pagina **Azure Data Lake Store**, en klik op **volgende**. 

    ![Doelpagina data store](./media/load-data-into-azure-data-lake-store/destination-data-storage-page.png)

7. In deze pagina kiest u het gedrag kopiëren door te controleren **bestanden kopiëren recursief** en **binaire kopiëren** (kopiëren van bestanden als-is) opties. Klik op **Volgende**.

    ![Geef de map voor uitvoer](./media/load-data-into-azure-data-lake-store/specify-binary-copy.png)

8. In de **Geef Data Lake Store verbinding** pagina, voert u de volgende stappen uit: 

    1. Selecteer uw Data Lake Store voor de **Data Lake Store-accountnaam**.
    2. Geef de service principal informatie zoals **Tenant**, **Service-principal-ID**, en **principal servicesleutel**.
    3. Klik op **Volgende**. 

    > [!IMPORTANT]
    > In dit scenario maakt u een **Service-principal** om te verifiëren van data lake store. Volg de aanwijzingen [hier](connector-azure-data-lake-store.md#using-service-principal-authentication) en zorg ervoor dat u service principal juiste toestemming geven in Azure Data Lake Store.

    ![Azure Data Lake Store-account opgeven](./media/load-data-into-azure-data-lake-store/specify-adls.png)

9. In de **het uitvoerbestand of de invoermap kiezen** pagina **copyfroms3**, klikt u vervolgens op **volgende**. 

    ![Geef de map voor uitvoer](./media/load-data-into-azure-data-lake-store/specify-adls-path.png)


10. In de **instellingen** pagina, klikt u op **volgende**. 

    ![Pagina met instellingen](./media/load-data-into-azure-data-lake-store/copy-settings.png)
11. In de **samenvatting** pagina, controleert u de instellingen en klik op **volgende**.

    ![Overzichtspagina](./media/load-data-into-azure-data-lake-store/copy-summary.png)
12. In de **implementatie pagina**, klikt u op **Monitor** voor het bewaken van de pijplijn (taak).

    ![Implementatie-pagina](./media/load-data-into-azure-data-lake-store/deployment-page.png)
13. U ziet dat de **Monitor** tabblad aan de linkerkant wordt automatisch geselecteerd. U ziet de koppelingen naar details uitvoering van activiteiten bekijken en opnieuw uit te voeren van de pijplijn in de **acties** kolom. 

    ![De pijplijn bewaken wordt uitgevoerd](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)
14. Als u wilt weergeven die zijn gekoppeld aan de pijplijn uitvoeren activiteiten bij uitvoering **weergave activiteit wordt uitgevoerd** koppelen de **acties** kolom. Er is slechts één activiteit (kopieeractiviteit) in de pijplijn, zodat u slechts één vermelding ziet. Overschakelen naar de pijplijn uitgevoerd weergeven, klikt u op **pijplijnen** koppeling aan de bovenkant. Klik op **vernieuwen** de lijst te vernieuwen. 

    ![Monitor activiteit wordt uitgevoerd](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

15. U kunt meer details van de uitvoering van de kopieeractiviteit van elke, controleren door te klikken op de **Details** koppeling onder **acties** in activiteit controle weergeven. Hier ziet u informatie zoals de hoeveelheid gegevens gekopieerd van bron naar sink, doorvoer, stappen het doorloopt met bijbehorende duur en configuraties gebruikt.

    ![Monitoractiviteit details uitvoering](./media/load-data-into-azure-data-lake-store/monitor-activity-run-details.png)

16. Controleer of dat de gegevens wordt gekopieerd naar uw Azure Data Lake Store. 

    ![Controleer of de uitvoer van de Data Lake Store](./media/load-data-into-azure-data-lake-store/adls-copy-result.png)

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over ondersteuning van Azure Data Lake Store: 

> [!div class="nextstepaction"]
>[Azure Data Lake Store-connector](connector-azure-data-lake-store.md)