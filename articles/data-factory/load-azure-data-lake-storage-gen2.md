---
title: Gegevens laden in Azure Data Lake Storage Gen2 (Preview) met Azure Data Factory
description: Azure Data Factory gebruiken om gegevens te kopiëren naar Azure Data Lake Storage Gen2 (Preview)
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: jingwang
ms.openlocfilehash: 961c8dea4dbb6b6600d10b75e84a9a84c34c329b
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036186"
---
# <a name="load-data-into-azure-data-lake-storage-gen2-preview-with-azure-data-factory"></a>Gegevens laden in Azure Data Lake Storage Gen2 Preview met Azure Data Factory

[Azure Data Lake Storage Gen2 Preview](../storage/data-lake-storage/introduction.md) wordt een protocol met hiërarchische naamruimte en beveiliging functies toegevoegd aan Azure Blob Storage zodat u eenvoudig verbinding maken met analytics frameworks een duurzame storage-laag. In Data Lake Storage Gen2 (Preview) alle kenmerken van de opslag van objecten blijven tijdens het toevoegen van de voordelen van een bestand system interface.

Azure Data Factory is een volledig beheerde gegevens cloud-gebaseerde integration-service. Kunt u de service voor het vullen van de lake met gegevens uit een groot aantal lokale en cloud-gebaseerde gegevens worden opgeslagen en tijd besparen bij het bouwen van uw analyseoplossingen. Voor een gedetailleerde lijst met ondersteunde connectors, Zie de tabel met [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory biedt een uitbreidbare beheerde oplossing voor gegevensverplaatsing in plaats van AzCopy, een opdrachtregel gegevens overbrengen hulpprogramma. Als gevolg van de architectuur van de scale-out van de ADF, kan deze gegevens met een hoge doorvoersnelheid opnemen. Zie voor meer informatie [kopiëren van de prestaties van de activiteit](copy-activity-performance.md).

Dit artikel laat zien hoe de gegevens kopiëren van Data Factory-hulpprogramma gebruiken om te laden van gegevens van _Amazon Web Services-S3 service_ in _Azure Data Lake Storage Gen2_. U kunt vergelijkbare stappen voor het kopiëren van gegevens van andere typen gegevensarchieven.

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* Azure Storage-account met Data Lake Storage Gen2 ingeschakeld: als u geen Storage-account hebt, klikt u op [hier](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) een maken.
* AWS-account met een S3-bucket die gegevens bevat: dit artikel laat zien hoe u gegevens vanaf Amazon S3 kopieert. U kunt andere gegevensarchieven door gelijksoortige stappen te volgen.

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Selecteer op het menu links **nieuw** > **gegevens en analyse** > **Data Factory**:
   
   ![Een nieuwe gegevensfactory maken](./media/load-azure-data-lake-storage-gen2/new-azure-data-factory-menu.png)
2. In de **nieuwe gegevensfactory** pagina, geef waarden op voor de velden die worden weergegeven in de volgende afbeelding: 
      
   ![De pagina Nieuwe data factory](./media/load-azure-data-lake-storage-gen2//new-azure-data-factory.png)
 
    * **Naam**: Voer een globaal unieke naam voor uw Azure data factory. Als u de foutmelding "naam gegevensfactory \"LoadADLSDemo\" is niet beschikbaar ' Voer een andere naam voor de data factory. Bijvoorbeeld, u kunt de naam van de  _**uwnaam**_**ADFTutorialDataFactory**. Probeer het opnieuw maken van de gegevensfactory. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
    * **Abonnement**: Selecteer uw Azure-abonnement in te maken van de gegevensfactory. 
    * **Resourcegroep**: Selecteer een bestaande resourcegroep in de vervolgkeuzelijst, of Selecteer de **nieuw** optie en typ de naam van een resourcegroep. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
    * **Versie**: Selecteer **V2(Preview)**.
    * **Locatie**: Selecteer de locatie voor de gegevensfactory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevens worden opgeslagen die worden gebruikt door data factory kunnen zich in andere locaties en regio's. 

3. Selecteer **Maken**.
4. Nadat het maken voltooid is, gaat u naar uw gegevensfactory. U ziet de **Data Factory** startpagina zoals weergegeven in de volgende afbeelding: 
   
   ![Startpagina van de gegevensfactory](./media/load-azure-data-lake-storage-gen2/data-factory-home-page.png)

   Selecteer de **auteur & Monitor** tegel om de toepassing van de integratie gegevens op een afzonderlijke tabblad te starten.

## <a name="load-data-into-azure-data-lake-storage-gen2"></a>Gegevens laden in Azure Data Lake Storage Gen2

1. In de **aan de slag** pagina de **gegevens kopiëren** tegel starten van het hulpprogramma voor kopiëren: 

   ![De tegel Copy Data-hulpprogramma](./media/load-azure-data-lake-storage-gen2/copy-data-tool-tile.png)
2. In de **eigenschappen** pagina **CopyFromAmazonS3ToADLS** voor de **taaknaam** veld en selecteert u een **volgende**:

    ![De pagina Eigenschappen](./media/load-azure-data-lake-storage-gen2/copy-data-tool-properties-page.png)
3. In de **brongegevensarchief** pagina, klikt u op **+ nieuwe verbinding maken**:

    ![De pagina Brongegevensarchief](./media/load-azure-data-lake-storage-gen2/source-data-store-page.png)
    
    Selecteer **Amazon S3** van de connector-galerie en selecteer **doorgaan**
    
    ![Bronpagina data store s3](./media/load-azure-data-lake-storage-gen2/source-data-store-page-s3.png)
    
4. In de **Geef Amazon S3 verbinding** pagina, voert u de volgende stappen uit:
   1. Geef de **toegangssleutel-ID** waarde.
   2. Geef de **geheime toegangssleutel** waarde.
   3. Klik op **verbinding testen** voor het valideren van de instellingen, selecteer **voltooien**.
   
   ![Amazon S3-account opgeven](./media/load-azure-data-lake-storage-gen2/specify-amazon-s3-account.png)
   
   4. U ziet dat een nieuwe verbinding wordt gemaakt. Selecteer **Volgende**.
   
5. In de **het invoerbestand of de invoermap kiezen** pagina, blader naar de map en het bestand dat u wilt worden overschreven. Selecteer de map/bestand, selecteer **Kies**:

    ![Het invoerbestand of de invoermap kiezen](./media/load-azure-data-lake-storage-gen2/choose-input-folder.png)

6. Geef het gedrag op kopiëren door het controleren van de **bestanden kopiëren recursief** en **binaire kopiëren** opties. Selecteer **volgende**:

    ![Geef de map voor uitvoer](./media/load-azure-data-lake-storage-gen2/specify-binary-copy.png)
    
7. In de **doelgegevensopslagplaats** pagina, klikt u op **+ nieuwe verbinding maken**, en selecteer vervolgens **Azure Data Lake Storage Gen2 (Preview)**, en selecteer **doorgaan** :

    ![De pagina Doelgegevensarchief](./media/load-azure-data-lake-storage-gen2/destination-data-storage-page.png)

8. In de **Geef Azure Data Lake Storage verbinding** pagina, voert u de volgende stappen uit:

   1. Selecteer uw Data Lake Storage Gen2 geschikt account van de 'opslagaccountnaam' vervolgkeuzelijst.
   2. Selecteer **Volgende**.
   
   ![Azure Data Lake Store-account opgeven](./media/load-azure-data-lake-storage-gen2/specify-adls.png)

9. In de **het uitvoerbestand of de invoermap kiezen** pagina **copyfroms3** als de naam van de map uitvoer en selecteer **volgende**: 

    ![Geef de map voor uitvoer](./media/load-azure-data-lake-storage-gen2/specify-adls-path.png)

10. In de **instellingen** pagina **volgende** om de standaardinstellingen te gebruiken:

    ![De pagina Instellingen](./media/load-azure-data-lake-storage-gen2/copy-settings.png)
11. In de **samenvatting** pagina, controleert u de instellingen en selecteer **volgende**:

    ![Overzichtspagina](./media/load-azure-data-lake-storage-gen2/copy-summary.png)
12. In de **implementatie pagina**, selecteer **Monitor** voor het bewaken van de pijplijn:

    ![De pagina Implementatie](./media/load-azure-data-lake-storage-gen2/deployment-page.png)
13. U ziet dat het tabblad **Controleren** aan de linkerkant automatisch wordt geselecteerd. De **acties** kolom bevat koppelingen naar details uitvoering van activiteiten bekijken en naar de pijplijn opnieuw uitvoeren:

    ![Pijplijnuitvoeringen controleren](./media/load-azure-data-lake-storage-gen2/monitor-pipeline-runs.png)

14. Als u wilt weergeven van activiteiten bij uitvoering die gekoppeld aan de pijplijn uitvoeren zijn, selecteer de **weergave activiteit wordt uitgevoerd** koppelen de **acties** kolom. Omdat er slechts één activiteit (kopieeractiviteit) in de pijplijn is, ziet u slechts één vermelding in de lijst. Overschakelen naar de pijplijn uitgevoerd weergeven, selecteert u de **pijplijnen** koppeling aan de bovenkant. Selecteer **Vernieuwen** om de lijst te vernieuwen. 

    ![Uitvoering van activiteiten controleren](./media/load-azure-data-lake-storage-gen2/monitor-activity-runs.png)

15. Voor het bewaken van de uitvoering details voor elke activiteit kopiëren, selecteert u de **Details** koppeling (bril installatiekopie) onder **acties** in de activiteit controle weergeven. Als de hoeveelheid gegevens gekopieerd van de bron naar het sink, de doorvoer van gegevens, de stappen worden uitgevoerd met een bijbehorende duur, en configuraties gebruikt, kunt u gegevens controleren:

    ![Monitoractiviteit details uitvoering](./media/load-azure-data-lake-storage-gen2/monitor-activity-run-details.png)

16. Controleer of dat de gegevens wordt gekopieerd naar uw Data Lake Storage Gen2-account.

## <a name="best-practice"></a>Best practices

Wanneer grote volumes kopiëren van gegevens van het gegevensarchief op basis van bestanden, aan u worden voorgesteld:

- Partitioneren van de bestanden in 10TB aan 20TB fileset.
- Te veel gelijktijdige kopie wordt uitgevoerd om te voorkomen dat de beperking van de bron- of sink gegevensarchieven niet activeren. U kunt beginnen met een kopie uitvoeren en bewaken van de doorvoer en vervolgens geleidelijk meer behoefte toevoegen.

## <a name="next-steps"></a>Volgende stappen

* [Activiteit-overzicht](copy-activity-overview.md)
* [Azure Data Lake Storage Gen2-connector](connector-azure-data-lake-storage.md)