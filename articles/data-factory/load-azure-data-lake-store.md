---
title: Gegevens laden in Azure Data Lake Storage Gen1 met behulp van Azure Data Factory | Microsoft Docs
description: Azure Data Factory gebruiken om gegevens te kopiëren in Azure Data Lake Storage Gen1
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/17/2018
ms.author: jingwang
ms.openlocfilehash: e401508fc5ffc1de666f727ffbb7790005384fc1
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39003794"
---
# <a name="load-data-into-azure-data-lake-storage-gen1-by-using-azure-data-factory"></a>Gegevens laden in Azure Data Lake Storage Gen1 met behulp van Azure Data Factory

[Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-overview.md) (voorheen bekend als Azure Data Lake Store) is een bedrijfsbrede zeer grote opslagruimte voor big data-analysewerklasten. Azure Data Lake kunt u gegevens van elke grootte, het type en de snelheid van de opname opnemen. De gegevens worden vastgelegd in een enkele locatie voor operationele en verkennende analyse.

Azure Data Factory is een volledig beheerde cloud-gebaseerde service voor gegevensintegratie. U kunt de service gebruiken voor het vullen van de lake met gegevens uit uw bestaande systeem en bespaar tijd bij het bouwen van uw analyseoplossingen.

Azure Data Factory biedt de volgende voordelen voor het laden van gegevens in Azure Data Lake Store:

* **Eenvoudig**: een intuïtieve wizard 5-stap geen scripts vereist.
* **Uitgebreide ondersteuning voor gegevensopslag**: ingebouwde ondersteuning voor een uitgebreide set on-premises en cloud-gebaseerde gegevensarchieven. Zie de tabel van voor een gedetailleerde lijst [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
* **Beveiligd en compatibel**: gegevens worden overgedragen via HTTPS of ExpressRoute. De aanwezigheid van de algemene service zorgt ervoor dat uw gegevens nooit de geografische grens verlaat.
* **Hoge prestaties**: maximaal 1-GB/s-gegevens sneller laden in Azure Data Lake Store. Zie voor meer informatie, [prestaties van de Kopieeractiviteit](copy-activity-performance.md).

Dit artikel laat u het gebruik van het hulpprogramma Data Factory kopiëren van gegevens naar _laden van gegevens vanaf Amazon S3 in Azure Data Lake Store_. U kunt uitvoeren van gelijksoortige stappen als u wilt kopiëren van gegevens van andere soorten gegevensarchieven.

> [!NOTE]
> Zie voor meer informatie, [gegevens kopiëren naar of van Azure Data Lake Store met behulp van Azure Data Factory](connector-azure-data-lake-store.md).

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* Azure Data Lake Store: Als u een Data Lake Store-account niet hebt, raadpleegt u de instructies in [maken van een Data Lake Store-account](../data-lake-store/data-lake-store-get-started-portal.md#create-an-azure-data-lake-store-account).
* Amazon S3: In dit artikel ziet u hoe het kopiëren van gegevens vanaf Amazon S3. U kunt andere gegevensarchieven gebruiken door gelijksoortige stappen te volgen.

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Selecteer in het menu links **nieuw** > **gegevens en analyses** > **Data Factory**:
   
   ![Een nieuwe gegevensfactory maken](./media/load-data-into-azure-data-lake-store/new-azure-data-factory-menu.png)
2. In de **nieuwe data factory** pagina, geef waarden op voor de velden die worden weergegeven in de volgende afbeelding: 
      
   ![De pagina Nieuwe data factory](./media/load-data-into-azure-data-lake-store//new-azure-data-factory.png)
 
    * **Naam**: Voer een unieke naam op voor uw Azure-gegevensfactory. Als u de foutmelding ' naam Data factory \"LoadADLSDemo\" is niet beschikbaar, "Voer een andere naam voor de data factory. Bijvoorbeeld, kunt u de naam van de  _**uwnaam**_**ADFTutorialDataFactory**. Probeer het opnieuw maken van de data factory. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
    * **Abonnement**: Selecteer uw Azure-abonnement waarin u wilt maken van de data factory. 
    * **Resourcegroep**: Selecteer een bestaande resourcegroep in de vervolgkeuzelijst of Selecteer de **nieuw** optie en voer de naam van een resourcegroep. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
    * **Versie**: Selecteer **V2**.
    * **Locatie**: Selecteer de locatie voor de data factory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevensarchieven die worden gebruikt door data factory, kunnen zich in andere locaties of regio's. Deze gegevensarchieven bevatten Azure Data Lake Store, Azure Storage en Azure SQL Database.

3. Selecteer **Maken**.
4. Na het aanmaken is voltooid, gaat u naar uw data factory. U ziet de **Data Factory** startpagina zoals wordt weergegeven in de volgende afbeelding: 
   
   ![Startpagina van de gegevensfactory](./media/load-data-into-azure-data-lake-store/data-factory-home-page.png)

   Selecteer de **Author & Monitor** tegel om te starten van de toepassing voor gegevensintegratie in een afzonderlijk tabblad.

## <a name="load-data-into-azure-data-lake-store"></a>Gegevens laden in Azure Data Lake Store

1. In de **aan de slag** weergeeft, schakelt de **Copy Data** tegel om het hulpprogramma Copy Data starten: 

   ![De tegel Copy Data-hulpprogramma](./media/load-data-into-azure-data-lake-store/copy-data-tool-tile.png)
2. In de **eigenschappen** pagina **CopyFromAmazonS3ToADLS** voor de **taaknaam** veld en selecteer **volgende**:

    ![De pagina Eigenschappen](./media/load-data-into-azure-data-lake-store/copy-data-tool-properties-page.png)
3. In de **brongegevensarchief** pagina, klikt u op **+ nieuwe verbinding maken**:

    ![De pagina Brongegevensarchief](./media/load-data-into-azure-data-lake-store/source-data-store-page.png)
    
    Selecteer **Amazon S3**, en selecteer **doorgaan**
    
    ![Pagina brongegevensarchief s3](./media/load-data-into-azure-data-lake-store/source-data-store-page-s3.png)
    
4. In de **opgeven Amazon S3 verbinding** pagina, de volgende stappen uit: 
   1. Geef de **toegangssleutel-ID** waarde.
   2. Geef de **geheime toegangssleutel** waarde.
   3. Selecteer **Voltooien**.
   
   ![Amazon S3-account opgeven](./media/load-data-into-azure-data-lake-store/specify-amazon-s3-account.png)
   
   4. Hier ziet u een nieuwe verbinding. Selecteer **Volgende**.
   
   ![Amazon S3-account opgeven](./media/load-data-into-azure-data-lake-store/specify-amazon-s3-account-created.png)
   
5. In de **invoerbestand of invoermap kiezen** pagina, blader naar de map en elk bestand dat u wilt dat moeten worden gekopieerd. Selecteer de map/bestand, selecteer **kiezen**, en selecteer vervolgens **volgende**:

    ![Het invoerbestand of de invoermap kiezen](./media/load-data-into-azure-data-lake-store/choose-input-folder.png)

6. Flattenhierarchy kiezen door het selecteren van de **bestanden kopiëren recursief** en **binaire kopie** (bestanden als kopiëren-is) opties. Selecteer **volgende**:

    ![Geef de map voor uitvoer](./media/load-data-into-azure-data-lake-store/specify-binary-copy.png)
    
7. In de **doelgegevensarchief** pagina, klikt u op **+ nieuwe verbinding maken**, en selecteer vervolgens **Azure Data Lake Store**, en selecteer **doorgaan**:

    ![De pagina Doelgegevensarchief](./media/load-data-into-azure-data-lake-store/destination-data-storage-page.png)

8. In de **Geef Data Lake Store verbinding** pagina, de volgende stappen uit: 

   1. Selecteer uw Data Lake Store voor de **Data Lake Store-accountnaam**.
   2. Geef de **Tenant**, en selecteer voltooien.
   3. Selecteer **Volgende**.
   
   > [!IMPORTANT]
   > In dit scenario maakt u een _beheerde service-identiteit_ om te verifiëren van uw Data Lake Store. Zorg ervoor dat u het MSI-bestand van de juiste machtigingen in Azure Data Lake Store door verlenen [deze instructies](connector-azure-data-lake-store.md#using-managed-service-identity-authentication).
   
   ![Azure Data Lake Store-account opgeven](./media/load-data-into-azure-data-lake-store/specify-adls.png)
9. In de **uitvoerbestand of uitvoermap kiezen** pagina **copyfroms3** als de naam van de uitvoer-map en selecteer **volgende**: 

    ![Geef de map voor uitvoer](./media/load-data-into-azure-data-lake-store/specify-adls-path.png)

10. In de **instellingen** weergeeft, schakelt **volgende**:

    ![De pagina Instellingen](./media/load-data-into-azure-data-lake-store/copy-settings.png)
11. In de **samenvatting** pagina, Controleer de instellingen en selecteer **volgende**:

    ![Overzichtspagina](./media/load-data-into-azure-data-lake-store/copy-summary.png)
12. In de **implementatiepagina**, selecteer **Monitor** voor het bewaken van de pijplijn of taak:

    ![De pagina Implementatie](./media/load-data-into-azure-data-lake-store/deployment-page.png)
13. U ziet dat het tabblad **Controleren** aan de linkerkant automatisch wordt geselecteerd. De **acties** kolom bevat koppelingen om details uitvoering van activiteit weer te geven en de pijplijn opnieuw uitvoeren:

    ![Pijplijnuitvoeringen controleren](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)
14. Als u uitvoeringen van activiteit die gekoppeld aan de pijplijnuitvoering zijn, selecteer de **uitvoeringen van activiteit weergeven** herstelkoppeling in de **acties** kolom. Omdat er slechts één activiteit (kopieeractiviteit) in de pijplijn is, ziet u slechts één vermelding in de lijst. Overschakelen naar de vorige weergave, selecteert u de **pijplijnen** koppelen aan de bovenkant. Selecteer **Vernieuwen** om de lijst te vernieuwen. 

    ![Uitvoering van activiteiten controleren](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

15. Voor het controleren van de uitvoering van details voor elke activiteit kopiëren, selecteert u de **Details** koppeling onder **acties** in de activiteit controle weergeven. U kunt details controleren zoals de hoeveelheid gegevens gekopieerd van de bron naar het sink, de doorvoer van gegevens, de stappen worden uitgevoerd met de bijbehorende tijdsduur en configuraties gebruikt:

    ![Details uitvoering van activiteit controleren](./media/load-data-into-azure-data-lake-store/monitor-activity-run-details.png)

16. Controleer of dat de gegevens worden gekopieerd naar uw Azure Data Lake Store: 

    ![Data Lake Store-uitvoer controleren](./media/load-data-into-azure-data-lake-store/adls-copy-result.png)

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over de ondersteuning van Azure Data Lake Store: 

> [!div class="nextstepaction"]
>[Azure Data Lake Store-connector](connector-azure-data-lake-store.md)
