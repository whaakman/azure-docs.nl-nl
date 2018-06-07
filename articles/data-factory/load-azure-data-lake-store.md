---
title: Gegevens laden in Azure Data Lake Store met behulp van Azure Data Factory | Microsoft Docs
description: Gebruik Azure Data Factory om gegevens te kopiëren naar Azure Data Lake Store
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
ms.openlocfilehash: fdfb35b0e1c52ad2aad164a38ae308f9142880a6
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34619623"
---
# <a name="load-data-into-azure-data-lake-store-by-using-azure-data-factory"></a>Gegevens laden in Azure Data Lake Store met behulp van Azure Data Factory

[Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) is een opslagplaats van de hele onderneming hyperschaal voor analytische workloads van big data. Azure Data Lake kunt u het vastleggen van gegevens van elke grootte, het type en de opname-snelheid. De gegevens is vastgelegd in een enkele locatie voor operationele en experimentele analyses.

Azure Data Factory is een volledig beheerde gegevens cloud-gebaseerde integration-service. U kunt de service de lake met gegevens van uw bestaande systeem vullen en tijd besparen bij het bouwen van uw analyseoplossingen.

Azure Data Factory biedt de volgende voordelen voor het laden van gegevens in Azure Data Lake Store:

* **Eenvoudig instellen van**: een intuïtieve wizard 5 stap geen scripts nodig.
* **Uitgebreide ondersteuning voor gegevensopslag**: ingebouwde ondersteuning voor een groot aantal on-premises en cloud-gebaseerde gegevensarchieven. Zie voor een gedetailleerde lijst, de tabel van [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
* **Beveiligd en compatibel**: gegevens worden overgedragen via HTTPS of ExpressRoute. De aanwezigheid van global service zorgt ervoor dat uw gegevens nooit de geografische grens verlaten.
* **Hoge prestaties**: maximaal 1-GB/s gegevens snelheid laden in Azure Data Lake Store. Zie voor meer informatie [kopiëren van de prestaties van de activiteit](copy-activity-performance.md).

Dit artikel laat zien hoe u de gegevens kopiëren van Data Factory-hulpprogramma te _gegevens vanaf Amazon S3 laden in Azure Data Lake Store_. U kunt vergelijkbare stappen voor het kopiëren van gegevens van andere typen gegevensarchieven.

> [!NOTE]
> Zie voor meer informatie [kopiëren van gegevens of naar Azure Data Lake Store met behulp van Azure Data Factory](connector-azure-data-lake-store.md).
>
> Dit artikel is van toepassing op versie 2 van Azure Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service is algemeen beschikbaar (GA), Zie [Kopieeractiviteit in Azure Data Factory versie 1](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* Azure Data Lake Store: Als u een Data Lake Store-account niet hebt, raadpleegt u de instructies in [maken van een Data Lake Store-account](../data-lake-store/data-lake-store-get-started-portal.md#create-an-azure-data-lake-store-account).
* Amazon S3: Dit artikel laat zien hoe u gegevens vanaf Amazon S3 kopieert. U kunt andere gegevensarchieven door gelijksoortige stappen te volgen.

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Selecteer op het menu links **nieuw** > **gegevens en analyse** > **Data Factory**:
   
   ![Een nieuwe gegevensfactory maken](./media/load-data-into-azure-data-lake-store/new-azure-data-factory-menu.png)
2. In de **nieuwe gegevensfactory** pagina, geef waarden op voor de velden die worden weergegeven in de volgende afbeelding: 
      
   ![De pagina Nieuwe data factory](./media/load-data-into-azure-data-lake-store//new-azure-data-factory.png)
 
    * **Naam**: Voer een globaal unieke naam voor uw Azure data factory. Als u de foutmelding "naam gegevensfactory \"LoadADLSDemo\" is niet beschikbaar ' Voer een andere naam voor de data factory. Bijvoorbeeld, u kunt de naam van de  _**uwnaam**_**ADFTutorialDataFactory**. Probeer het opnieuw maken van de gegevensfactory. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
    * **Abonnement**: Selecteer uw Azure-abonnement in te maken van de gegevensfactory. 
    * **Resourcegroep**: Selecteer een bestaande resourcegroep in de vervolgkeuzelijst, of Selecteer de **nieuw** optie en typ de naam van een resourcegroep. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
    * **Versie**: Selecteer **V2 (Preview)**.
    * **Locatie**: Selecteer de locatie voor de gegevensfactory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevens worden opgeslagen die worden gebruikt door data factory kunnen zich in andere locaties en regio's. Deze gegevensarchieven bevatten Azure Data Lake Store, Azure Storage en Azure SQL Database.

3. Selecteer **Maken**.
4. Nadat het maken voltooid is, gaat u naar uw gegevensfactory. U ziet de **Data Factory** startpagina zoals weergegeven in de volgende afbeelding: 
   
   ![Startpagina van de gegevensfactory](./media/load-data-into-azure-data-lake-store/data-factory-home-page.png)

   Selecteer de **auteur & Monitor** tegel om de toepassing van de integratie gegevens op een afzonderlijke tabblad te starten.

## <a name="load-data-into-azure-data-lake-store"></a>Gegevens laden in Azure Data Lake Store

1. In de **aan de slag** pagina de **gegevens kopiëren** tegel starten van het hulpprogramma voor kopiëren: 

   ![De tegel Copy Data-hulpprogramma](./media/load-data-into-azure-data-lake-store/copy-data-tool-tile.png)
2. In de **eigenschappen** pagina **CopyFromAmazonS3ToADLS** voor de **taaknaam** veld en selecteert u een **volgende**:

    ![De pagina Eigenschappen](./media/load-data-into-azure-data-lake-store/copy-data-tool-properties-page.png)
3. In de **brongegevensarchief** pagina **Amazon S3**, en selecteer **volgende**:

    ![De pagina Brongegevensarchief](./media/load-data-into-azure-data-lake-store/source-data-store-page.png)
4. In de **Geef Amazon S3 verbinding** pagina, voert u de volgende stappen uit: 
   1. Geef de **toegangssleutel-ID** waarde.
   2. Geef de **geheime toegangssleutel** waarde.
   3. Selecteer **Volgende**.
   
   ![Amazon S3-account opgeven](./media/load-data-into-azure-data-lake-store/specify-amazon-s3-account.png)
5. In de **het invoerbestand of de invoermap kiezen** pagina, blader naar de map en het bestand dat u wilt worden overschreven. Selecteer de map/bestand, selecteer **Kies**, en selecteer vervolgens **volgende**:

    ![Het invoerbestand of de invoermap kiezen](./media/load-data-into-azure-data-lake-store/choose-input-folder.png)

6. In de **doelgegevensopslagplaats** pagina **Azure Data Lake Store**, en selecteer **volgende**:

    ![De pagina Doelgegevensarchief](./media/load-data-into-azure-data-lake-store/destination-data-storage-page.png)

7. Kies soorten gedrag kopiëren door het selecteren van de **bestanden kopiëren recursief** en **binaire kopiëren** (kopiëren van bestanden als-is) opties. Selecteer **volgende**:

    ![Geef de map voor uitvoer](./media/load-data-into-azure-data-lake-store/specify-binary-copy.png)

8. In de **Geef Data Lake Store verbinding** pagina, voert u de volgende stappen uit: 

   1. Selecteer uw Data Lake Store voor de **Data Lake Store-accountnaam**.
   2. Geef de gegevens van de service-principal: **Tenant**, **Service-principal-ID**, en **principal servicesleutel**.
   3. Selecteer **Volgende**.
   
   > [!IMPORTANT]
   > In dit scenario maakt u een _service-principal_ voor verificatie van uw Data Lake Store. Zorg ervoor dat de service-principal de juiste machtigingen in Azure Data Lake Store verlenen, gevolgd door [deze instructies](connector-azure-data-lake-store.md#using-service-principal-authentication).
   
   ![Azure Data Lake Store-account opgeven](./media/load-data-into-azure-data-lake-store/specify-adls.png)
9. In de **het uitvoerbestand of de invoermap kiezen** pagina **copyfroms3** als de naam van de map uitvoer en selecteer **volgende**: 

    ![Geef de map voor uitvoer](./media/load-data-into-azure-data-lake-store/specify-adls-path.png)

10. In de **instellingen** pagina **volgende**:

    ![De pagina Instellingen](./media/load-data-into-azure-data-lake-store/copy-settings.png)
11. In de **samenvatting** pagina, controleert u de instellingen en selecteer **volgende**:

    ![Overzichtspagina](./media/load-data-into-azure-data-lake-store/copy-summary.png)
12. In de **implementatie pagina**, selecteer **Monitor** voor het bewaken van de pijplijn (taak):

    ![De pagina Implementatie](./media/load-data-into-azure-data-lake-store/deployment-page.png)
13. U ziet dat het tabblad **Controleren** aan de linkerkant automatisch wordt geselecteerd. De **acties** kolom bevat koppelingen naar details uitvoering van activiteiten bekijken en naar de pijplijn opnieuw uitvoeren:

    ![Pijplijnuitvoeringen controleren](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)
14. Als u wilt weergeven van activiteiten bij uitvoering die gekoppeld aan de pijplijn uitvoeren zijn, selecteer de **weergave activiteit wordt uitgevoerd** koppelen de **acties** kolom. Omdat er slechts één activiteit (kopieeractiviteit) in de pijplijn is, ziet u slechts één vermelding in de lijst. Overschakelen naar de pijplijn uitgevoerd weergeven, selecteert u de **pijplijnen** koppeling aan de bovenkant. Selecteer **Vernieuwen** om de lijst te vernieuwen. 

    ![Uitvoering van activiteiten controleren](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

15. Voor het bewaken van de uitvoering details voor elke activiteit kopiëren, selecteert u de **Details** koppeling onder **acties** in de activiteit controle weergeven. Als de hoeveelheid gegevens gekopieerd van de bron naar het sink, de doorvoer van gegevens, de stappen worden uitgevoerd met een bijbehorende duur, en configuraties gebruikt, kunt u gegevens controleren:

    ![Monitoractiviteit details uitvoering](./media/load-data-into-azure-data-lake-store/monitor-activity-run-details.png)

16. Controleer of dat de gegevens wordt gekopieerd naar uw Azure Data Lake Store: 

    ![Controleer of de uitvoer van de Data Lake Store](./media/load-data-into-azure-data-lake-store/adls-copy-result.png)

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over ondersteuning van Azure Data Lake Store: 

> [!div class="nextstepaction"]
>[Azure Data Lake Store-connector](connector-azure-data-lake-store.md)
