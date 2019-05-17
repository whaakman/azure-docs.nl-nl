---
title: Gegevens kopiëren van Azure Data Lake Storage Gen1 naar Gen2 met Azure Data Factory
description: Gebruik Azure Data Factory om gegevens kopiëren van Azure Data Lake Storage Gen1 naar Gen2
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: jingwang
ms.openlocfilehash: d6e09ec1f070f9ee0f4162524e4bd80d1f81adc3
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65560639"
---
# <a name="copy-data-from-azure-data-lake-storage-gen1-to-gen2-with-azure-data-factory"></a>Gegevens kopiëren van Azure Data Lake Storage Gen1 naar Gen2 met Azure Data Factory

Azure Data Lake Storage Gen2 is een verscheidenheid aan functies die zijn toegewezen aan de analyse van big data, die is ingebouwd in [Azure Blob-opslag](../storage/blobs/storage-blobs-introduction.md). Hiermee kunt u samenwerken met uw gegevens met behulp van beide beschermingsparadigma in het systeem en de object-opslag.

Als u momenteel van Azure Data Lake Storage Gen1 gebruikmaakt, kunt u de nieuwe functie Gen2 evalueren door te kopiëren van gegevens van Data Lake Storage Gen1 naar Gen2 met Azure Data Factory.

Azure Data Factory is een volledig beheerde cloud-gebaseerde service voor gegevensintegratie. U kunt de service gebruiken voor het vullen van de lake met gegevens uit een grote verscheidenheid aan on-premises en cloud-gebaseerde gegevens worden opgeslagen en bespaar tijd bij het bouwen van uw analyseoplossingen. Voor een gedetailleerde lijst met ondersteunde connectors, Zie de tabel met [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory biedt een scale-out, beheerde oplossing voor gegevensverplaatsing. Vanwege de architectuur van de scale-out van ADF, kan deze gegevens met hoge doorvoer opnemen. Zie voor meer informatie, [prestaties van de Kopieeractiviteit](copy-activity-performance.md).

In dit artikel leest u hoe u het hulpprogramma Copy Data van Data Factory gegevens kopiëren naar _Azure Data Lake Storage Gen1_ in _Azure Data Lake Storage Gen2_. U kunt uitvoeren van gelijksoortige stappen als u wilt kopiëren van gegevens van andere soorten gegevensarchieven.

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.
* Account voor Azure Data Lake Storage Gen1 met gegevens.
* Azure Storage-account met Data Lake Storage Gen2 is ingeschakeld: Als u een Storage-account geen [maken van een account](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM).

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Selecteer in het menu links **een resource maken** > **gegevens en analyses** > **Data Factory**:
   
   ![Selectie van Data Factory in het deelvenster Nieuw](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. In de **nieuwe data factory** pagina, geef waarden op voor de velden die worden weergegeven in de volgende afbeelding: 
      
   ![De pagina Nieuwe data factory](./media/load-azure-data-lake-storage-gen2-from-gen1/new-azure-data-factory.png)
 
    * **Naam**: Voer een unieke naam op voor uw Azure-gegevensfactory. Als u de foutmelding ' naam Data factory \"LoadADLSDemo\" is niet beschikbaar, "Voer een andere naam voor de data factory. Bijvoorbeeld, kunt u de naam van de  _**uwnaam**_**ADFTutorialDataFactory**. Probeer het opnieuw maken van de data factory. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
    * **Abonnement**: Selecteer uw Azure-abonnement waarin u wilt maken van de data factory. 
    * **Resourcegroep**: Selecteer een bestaande resourcegroep in de vervolgkeuzelijst of Selecteer de **nieuw** optie en voer de naam van een resourcegroep. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
    * **Version**: Selecteer **V2**.
    * **Locatie**: Selecteer de locatie voor de data factory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevensarchieven die worden gebruikt door data factory, kunnen zich in andere locaties of regio's. 

3. Selecteer **Maken**.
4. Na het aanmaken is voltooid, gaat u naar uw data factory. U ziet de **Data Factory** startpagina zoals wordt weergegeven in de volgende afbeelding: 
   
   ![Startpagina van de gegevensfactory](./media/load-azure-data-lake-storage-gen2-from-gen1/data-factory-home-page.png)

   Selecteer de **Author & Monitor** tegel om te starten van de toepassing voor gegevensintegratie in een afzonderlijk tabblad.

## <a name="load-data-into-azure-data-lake-storage-gen2"></a>Gegevens laden in Azure Data Lake Storage Gen2

1. In de **aan de slag** weergeeft, schakelt de **Copy Data** tegel om het hulpprogramma Copy Data starten: 

   ![De tegel Copy Data-hulpprogramma](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-data-tool-tile.png)
2. In de **eigenschappen** pagina **CopyFromADLSGen1ToGen2** voor de **taaknaam** veld en selecteer **volgende**:

    ![De pagina Eigenschappen](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-data-tool-properties-page.png)
3. In de **brongegevensarchief** pagina, klikt u op **+ nieuwe verbinding maken**:

    ![De pagina Brongegevensarchief](./media/load-azure-data-lake-storage-gen2-from-gen1/source-data-store-page.png)
    
    Selecteer **Azure Data Lake Storage Gen1** uit de galerie met connector en selecteer **doorgaan**
    
    ![Azure Data Lake Storage Gen1 pagina van brongegevensarchief](./media/load-azure-data-lake-storage-gen2-from-gen1/source-data-store-page-adls-gen1.png)
    
4. In de **opgeven Azure Data Lake Storage Gen1 verbinding** pagina, de volgende stappen uit:
   1. Selecteer uw Data Lake Storage Gen1 voor de accountnaam, en geef aan of het valideren van de **Tenant**.
   2. Klik op **verbinding testen** voor het valideren van de instellingen, selecteer **voltooien**.
   3. U ziet dat een nieuwe verbinding wordt gemaakt. Selecteer **Next**.
   
   > [!IMPORTANT]
   > In dit scenario maakt u een beheerde identiteit voor Azure-resources gebruikt voor verificatie van uw Data Lake Storage Gen1. Zorg ervoor dat u het MSI-bestand de juiste machtigingen in Azure Data Lake Storage Gen1 verlenen door [deze instructies](connector-azure-data-lake-store.md#managed-identity).
   
   ![Gen1 van Azure Data Lake Storage-account opgeven](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen1-account.png)
      
5. In de **invoerbestand of invoermap kiezen** pagina, blader naar de map en elk bestand dat u wilt dat moeten worden gekopieerd. Selecteer de map/bestand, selecteer **kiezen**:

    ![Het invoerbestand of de invoermap kiezen](./media/load-azure-data-lake-storage-gen2-from-gen1/choose-input-folder.png)

6. Geef het gedrag op kopiëren door het controleren van de **bestanden kopiëren recursief** en **binaire kopie** opties. Selecteer **volgende**:

    ![Geef de map voor uitvoer](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-binary-copy.png)
    
7. In de **doelgegevensarchief** pagina, klikt u op **+ nieuwe verbinding maken**, en selecteer vervolgens **Azure Data Lake Storage Gen2**, en selecteer **doorgaan**:

    ![De pagina Doelgegevensarchief](./media/load-azure-data-lake-storage-gen2-from-gen1/destination-data-storage-page.png)

8. In de **opgeven Azure Data Lake Storage Gen2 verbinding** pagina, de volgende stappen uit:

   1. Selecteer uw Data Lake Storage Gen2 geschikt account van de "naam van het Opslagaccount" vervolgkeuzelijst.
   2. Selecteer **voltooien** om de verbinding te maken. Selecteer vervolgens **Volgende**.
   
   ![Azure Data Lake Storage Gen2 account opgeven](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen2-account.png)

9. In de **uitvoerbestand of uitvoermap kiezen** pagina **copyfromadlsgen1** als de naam van de uitvoer-map en selecteer **volgende**. ADF wordt het bijbehorende Gen2 ADLS-bestandssysteem en de submappen tijdens het kopiëren van maken als deze nog niet bestaat.

    ![Geef de map voor uitvoer](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen2-path.png)

10. In de **instellingen** weergeeft, schakelt **volgende** om de standaardinstellingen te gebruiken.

11. In de **samenvatting** pagina, Controleer de instellingen en selecteer **volgende**:

    ![Overzichtspagina](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-summary.png)
12. In de **implementatiepagina**, selecteer **Monitor** voor het bewaken van de pijplijn:

    ![De pagina Implementatie](./media/load-azure-data-lake-storage-gen2-from-gen1/deployment-page.png)
13. U ziet dat het tabblad **Controleren** aan de linkerkant automatisch wordt geselecteerd. De **acties** kolom bevat koppelingen om details uitvoering van activiteit weer te geven en de pijplijn opnieuw uitvoeren:

    ![Pijplijnuitvoeringen controleren](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-pipeline-runs.png)

14. Als u uitvoeringen van activiteit die gekoppeld aan de pijplijnuitvoering zijn, selecteer de **uitvoeringen van activiteit weergeven** herstelkoppeling in de **acties** kolom. Omdat er slechts één activiteit (kopieeractiviteit) in de pijplijn is, ziet u slechts één vermelding in de lijst. Overschakelen naar de vorige weergave, selecteert u de **pijplijnen** koppelen aan de bovenkant. Selecteer **Vernieuwen** om de lijst te vernieuwen. 

    ![Uitvoering van activiteiten controleren](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-activity-runs.png)

15. Voor het controleren van de uitvoering van details voor elke activiteit kopiëren, selecteert u de **Details** koppeling (van een bril) onder **acties** in de activiteit controle weergeven. U kunt details controleren zoals de hoeveelheid gegevens gekopieerd van de bron naar het sink, de doorvoer van gegevens, de stappen worden uitgevoerd met de bijbehorende tijdsduur en configuraties gebruikt:

    ![Details uitvoering van activiteit controleren](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-activity-run-details.png)

16. Controleer of dat de gegevens worden gekopieerd naar uw Data Lake Storage Gen2-account.

## <a name="best-practices"></a>Aanbevolen procedures

Raadpleeg voor het evalueren van het upgraden van Azure Data Lake Storage (ADLS) Gen1 naar Gen2 in het algemeen, [upgraden van uw big data analytics-oplossingen van Azure Data Lake Storage Gen1 naar Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-upgrade.md). De volgende secties vindt aanbevolen procedures van het gebruik van ADF voor gegevensupgrade van Gen1 naar Gen2.

### <a name="data-partition-for-historical-data-copy"></a>Partitie voor het kopiëren van historische gegevens

- Als de grootte van uw totale hoeveelheid gegevens in ADLS Gen1 is minder dan **30TB** en het aantal bestanden is minder dan **1 miljoen**, kunt u alle gegevens in één exemplaar activiteit die wordt uitgevoerd.
- Als er een grotere hoeveelheid gegevens te kopiëren, of u de flexibiliteit om te beheren van de gegevensmigratie in batches en elk van deze voltooid binnen een bepaald tijdstip windows kunt u voor het partitioneren van de gegevens worden voorgesteld, in welk geval het kan ook de verminderen het risico van een onverwachte iss UE.

Een PoC (Proof of Concept) wordt sterk aanbevolen om te controleren of de end-to-end-oplossing en de doorvoer van de kopie in uw omgeving te testen. Belangrijke stappen PoC te doen: 

1. Een ADF-pijplijn maken met één kopieeractiviteit verschillende TB aan gegevens van ADLS Gen1 naar ADLS Gen2 om op te halen van een kopie basislijn van prestaties, beginnend met kopieert [eenheden van de integratie van gegevens (DIUs)](copy-activity-performance.md#data-integration-units) als 128. 
2. Op basis van de kopie-doorvoer u in stap #1 krijgt, de geschatte tijd die nodig is voor de volledige gegevens-migratie berekenen. 
3. (Optioneel) Een besturingselement-tabel maken en definieer het bestand voor het partitioneren van de bestanden moeten worden gemigreerd. De manier voor het partitioneren van de volgende bestanden: 

    - Met de mapnaam- of mapnaam is gepartitioneerd met filteren op jokerteken (aanbevolen) 
    - Geregeld aan de hand van het bestand laatst gewijzigd 

### <a name="network-bandwidth-and-storage-io"></a>Netwerk bandbreedte en -opslag i/o 

U kunt de gelijktijdigheid van taken van de ADF-kopie die gegevens van ADLS Gen1 lezen en schrijven van gegevens naar ADLS Gen2, kunt beheren, zodat u het gebruik van opslag-i/o beheren kunt om te kunnen niet van invloed op het werk van het normale zakelijke op ADLS Gen1 tijdens de migratie.

### <a name="permissions"></a>Machtigingen 

In Data Factory, [ADLS Gen1 connector](connector-azure-data-lake-store.md) biedt ondersteuning voor Service-Principal en beheerde identiteit voor Azure-resource-verificaties; [ADLS Gen2 connector](connector-azure-data-lake-storage.md) ondersteunt accountsleutel, Service-Principal en beheerde identiteit voor Azure-resource-verificaties. Om Data Factory kunnen navigeren en kopieert u die alle de bestanden/ACL's als u nodig hebt, zorg ervoor dat u hoge voldoende machtigingen voor het account opgeven voor toegang tot/lezen/schrijven van alle bestanden en ACL's ingesteld als u wilt. Voorgesteld moet worden verleend als super-End-User/eigenaar rol tijdens de migratieperiode. 

### <a name="preserve-acls-from-data-lake-storage-gen1"></a>ACL's van Data Lake Storage Gen1 behouden

Als u repliceren van de ACL's samen met de bestanden wilt bij een upgrade van Data Lake Storage Gen1 naar Gen2, raadpleegt u [behouden ACL's van Data Lake Storage Gen1](connector-azure-data-lake-storage.md#preserve-acls-from-data-lake-storage-gen1). 

### <a name="incremental-copy"></a>Incrementele kopie 

Verschillende manieren kunnen worden gebruikt om alleen de nieuwe of bijgewerkte bestanden vanuit ADLS Gen1 laden:

- Nieuwe of bijgewerkte bestanden laden met de tijd gepartitioneerde bestand of map naam, bijvoorbeeld/2019/05/13 / *;
- Nieuwe of bijgewerkte bestanden door LastModifiedDate; laden
- Nieuwe of bijgewerkte bestanden identificeren door een 3e partij hulpprogramma/oplossing, en vervolgens het bestand of map naam doorgeven aan ADF-pijplijn via de parameter of een tabel per bestand.  

De juiste frequentie incrementeel laden doen, is afhankelijk van het totale aantal bestanden in ADLS Gen1 en de omvang van de nieuwe of bijgewerkte bestand worden geladen telkens.  

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Overzicht kopieeractiviteit](copy-activity-overview.md)
> [connector voor Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md)
> [Azure Data Lake Storage Gen2-connector](connector-azure-data-lake-storage.md)