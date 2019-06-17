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
ms.openlocfilehash: d13dad6e87bd6c821b497138ad75d7ae2b9a052d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67068976"
---
# <a name="copy-data-from-azure-data-lake-storage-gen1-to-gen2-with-azure-data-factory"></a>Gegevens kopiëren van Azure Data Lake Storage Gen1 naar Gen2 met Azure Data Factory

Azure Data Lake Storage Gen2 is een verscheidenheid aan functies die zijn toegewezen aan de analyse van big data die ingebouwd in [Azure Blob-opslag](../storage/blobs/storage-blobs-introduction.md). U kunt deze gebruiken om met uw gegevens met behulp van beide beschermingsparadigma in het systeem en de object-opslag.

Als u momenteel gebruikmaakt van Azure Data Lake Storage Gen1, kunt u Azure Data Lake Storage Gen2 evalueren door het kopiëren van gegevens uit Data Lake Storage Gen1 naar Gen2 met behulp van Azure Data Factory.

Azure Data Factory is een volledig beheerde cloud-gebaseerde service voor gegevensintegratie. U kunt de service gebruiken voor het vullen van de lake met gegevens uit een grote verscheidenheid aan on-premises en cloud-gebaseerde gegevens worden opgeslagen en bespaar tijd wanneer u uw analyseoplossingen bouwen. Voor een lijst van ondersteunde connectors, Zie de tabel met [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory biedt een scale-out, beheerde oplossing voor gegevensverplaatsing. Vanwege de architectuur van de scale-out van Data Factory, kan deze gegevens met hoge doorvoer opnemen. Zie voor meer informatie, [prestaties van de Kopieeractiviteit](copy-activity-performance.md).

Dit artikel ziet u hoe u kunt het hulpprogramma copy Data Factory gebruiken om gegevens te kopiëren van Azure Data Lake Storage Gen1 in Azure Data Lake Storage Gen2. U kunt uitvoeren van gelijksoortige stappen als u wilt kopiëren van gegevens van andere soorten gegevensarchieven.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.
* Account voor Azure Data Lake Storage Gen1 met gegevens.
* Azure Storage-account met Data Lake Storage Gen2 is ingeschakeld. Als u een Storage-account geen [maken van een account](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM).

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Selecteer in het menu links **een resource maken** > **gegevens en analyses** > **Data Factory**.
   
   ![Selectie van Data Factory in het nieuwe deelvenster](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. Op de **nieuwe data factory** pagina, geef waarden op voor de velden die worden weergegeven in de volgende afbeelding: 
      
   ![Pagina nieuwe Data factory](./media/load-azure-data-lake-storage-gen2-from-gen1/new-azure-data-factory.png)
 
    * **Naam**: Voer een unieke naam op voor uw Azure-gegevensfactory. Als u de foutmelding ' naam Data factory \"LoadADLSDemo\" is niet beschikbaar, "Voer een andere naam voor de data factory. Gebruik bijvoorbeeld de naam _**uwnaam**_ **ADFTutorialDataFactory**. De data factory opnieuw maken. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
    * **Abonnement**: Selecteer uw Azure-abonnement waarin u wilt maken van de data factory. 
    * **Resourcegroep**: Selecteer een bestaande resourcegroep in de vervolgkeuzelijst. U kunt ook selecteren de **nieuw** optie en voer de naam van een resourcegroep. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie. 
    * **Version**: Selecteer **V2**.
    * **Locatie**: Selecteer de locatie voor de data factory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevensarchieven die worden gebruikt door de data factory, kunnen zich in andere locaties of regio's. 

3. Selecteer **Maken**.
4. Nadat het maken is voltooid, gaat u naar uw data factory. U ziet de **Data Factory** startpagina zoals wordt weergegeven in de volgende afbeelding: 
   
   ![Startpagina van de gegevensfactory](./media/load-azure-data-lake-storage-gen2-from-gen1/data-factory-home-page.png)

5. Selecteer de **Author & Monitor** tegel om de toepassing van de integratie van gegevens in een afzonderlijk tabblad te starten.

## <a name="load-data-into-azure-data-lake-storage-gen2"></a>Gegevens laden in Azure Data Lake Storage Gen2

1. Op de **aan de slag** weergeeft, schakelt de **Copy Data** tegel om het hulpprogramma copy data starten. 

   ![Tegel voor hulpprogramma voor kopiëren](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-data-tool-tile.png)
2. Op de **eigenschappen** pagina **CopyFromADLSGen1ToGen2** voor de **taaknaam** veld. Selecteer **Next**.

    ![De pagina Eigenschappen](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-data-tool-properties-page.png)
3. Op de **brongegevensarchief** weergeeft, schakelt **+ nieuwe verbinding maken**.

    ![De pagina Brongegevensarchief](./media/load-azure-data-lake-storage-gen2-from-gen1/source-data-store-page.png)
    
4. Selecteer **Azure Data Lake Storage Gen1** uit de galerie met connector en selecteer **doorgaan**.
    
    ![Azure Data Lake Storage Gen1 pagina van brongegevensarchief](./media/load-azure-data-lake-storage-gen2-from-gen1/source-data-store-page-adls-gen1.png)
    
5. Op de **opgeven Azure Data Lake Storage Gen1 verbinding** pagina, als volgt te werk:

   a. Selecteer uw Data Lake Storage Gen1 voor de accountnaam, en geef aan of het valideren van de **Tenant**.
  
   b. Selecteer **verbinding testen** om de instellingen te valideren. Selecteer vervolgens **Voltooien**.
  
   c. U ziet dat een nieuwe verbinding is gemaakt. Selecteer **Next**.
   
   > [!IMPORTANT]
   > In dit voorbeeld gebruikt u een beheerde identiteit voor Azure-resources voor het verifiëren van uw Azure Data Lake Storage Gen1. Als u wilt de beheerde identiteit de juiste machtigingen in Azure Data Lake Storage Gen1 verlenen, gaat u als volgt [deze instructies](connector-azure-data-lake-store.md#managed-identity).
   
   ![Gen1 van Azure Data Lake Storage-account opgeven](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen1-account.png)
      
6. Op de **invoerbestand of invoermap kiezen** pagina, blader naar de map en elk bestand dat u wilt dat moeten worden gekopieerd. Selecteer de map of het bestand en selecteer **kiezen**.

    ![Het invoerbestand of de invoermap kiezen](./media/load-azure-data-lake-storage-gen2-from-gen1/choose-input-folder.png)

7. Geef het gedrag op kopiëren door te selecteren de **bestanden kopiëren recursief** en **binaire kopie** opties. Selecteer **Next**.

    ![Geef de map voor uitvoer](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-binary-copy.png)
    
8. Op de **doelgegevensarchief** weergeeft, schakelt **+ nieuwe verbinding maken** > **Azure Data Lake Storage Gen2**  >   **Doorgaan met**.

    ![De pagina Doelgegevensarchief](./media/load-azure-data-lake-storage-gen2-from-gen1/destination-data-storage-page.png)

9. Op de **opgeven Azure Data Lake Storage Gen2 verbinding** pagina, als volgt te werk:

   a. Selecteer uw Data Lake Storage Gen2 geschikt account van de **opslagaccountnaam** vervolgkeuzelijst.
   
   b. Selecteer **voltooien** om de verbinding te maken. Selecteer vervolgens **Volgende**.
   
   ![Azure Data Lake Storage Gen2 account opgeven](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen2-account.png)

10. Op de **uitvoerbestand of uitvoermap kiezen** pagina **copyfromadlsgen1** als de naam van de uitvoer-map en selecteer **volgende**. Data Factory wordt de bijbehorende Azure Data Lake Storage Gen2-bestandssysteem en de submappen tijdens het kopiëren van gemaakt als deze nog niet bestaan.

    ![Geef de map voor uitvoer](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen2-path.png)

11. Op de **instellingen** weergeeft, schakelt **volgende** om de standaardinstellingen te gebruiken.

12. Op de **samenvatting** pagina, Controleer de instellingen en selecteer **volgende**.

    ![Overzichtspagina](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-summary.png)
13. Op de **implementatiepagina**, selecteer **Monitor** voor het bewaken van de pijplijn.

    ![De pagina Implementatie](./media/load-azure-data-lake-storage-gen2-from-gen1/deployment-page.png)
14. U ziet dat het tabblad **Controleren** aan de linkerkant automatisch wordt geselecteerd. De kolom **Acties** bevat onder andere koppelingen om details van de uitvoering van activiteiten te bekijken en de pijplijn opnieuw uit te voeren.

    ![Pijplijnuitvoeringen controleren](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-pipeline-runs.png)

15. Als u uitvoeringen van activiteit die gekoppeld aan de pijplijnuitvoering zijn, selecteer de **uitvoeringen van activiteit weergeven** herstelkoppeling in de **acties** kolom. Omdat er slechts één activiteit (kopieeractiviteit) in de pijplijn is, ziet u slechts één vermelding in de lijst. Overschakelen naar de vorige weergave, selecteert u de **pijplijnen** koppelen aan de bovenkant. Selecteer **Vernieuwen** om de lijst te vernieuwen. 

    ![Uitvoering van activiteiten controleren](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-activity-runs.png)

16. Voor het controleren van de uitvoering van details voor elke activiteit kopiëren, selecteert u de **Details** koppeling (van een bril) onder **acties** in de activiteit controle weergeven. U kunt details controleren zoals de hoeveelheid gegevens gekopieerd van de bron naar het sink, de doorvoer van gegevens, de stappen worden uitgevoerd met de bijbehorende tijdsduur en configuraties gebruikt.

    ![Details uitvoering van activiteit controleren](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-activity-run-details.png)

17. Controleer of dat de gegevens worden gekopieerd naar uw Azure Data Lake Storage Gen2-account.

## <a name="best-practices"></a>Aanbevolen procedures

Zie voor het evalueren van het upgraden van Azure Data Lake Storage Gen1 naar Azure Data Lake Storage Gen2 in het algemeen, [upgraden van uw big data analytics-oplossingen van Azure Data Lake Storage Gen1 naar Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-upgrade.md). De volgende secties vindt aanbevolen procedures voor het gebruik van Data Factory voor een gegevensupgrade van een Data Lake Storage Gen1 naar Data Lake Storage Gen2.

### <a name="data-partition-for-historical-data-copy"></a>Partitie voor het kopiëren van historische gegevens

- Als de grootte van uw totale hoeveelheid gegevens in Data Lake Storage Gen1 minder dan 30 TB is en het aantal bestanden minder dan 1 miljoen is, kunt u alle gegevens kopiëren in een activiteit die één exemplaar wordt uitgevoerd.
- Als er een grotere hoeveelheid gegevens te kopiëren, of u de flexibiliteit om te beheren van de gegevensmigratie in batches en elk van deze voltooid binnen een bepaalde periode wilt, moet u de gegevens partitioneren. Partitioneren, verlaagt u ook het risico van een onverwacht probleem.

Gebruik een bewijs van concept om te controleren van de end-to-end-oplossing en de doorvoer van de kopie in uw omgeving te testen. Proof-of-concept hoofdstappen: 

1. Maak een Data Factory-pijplijn met een enkele copy activity in verschillende TB aan gegevens van Data Lake Storage Gen1 naar Data Lake Storage Gen2 om op te halen van een basislijn voor de prestaties kopiëren kopieert. Beginnen met [eenheden van de integratie van gegevens (DIUs)](copy-activity-performance.md#data-integration-units) als 128. 
2. Op basis van de kopie-doorvoer die u in stap 1, bereken de geschatte duur op voor de migratie van de volledige gegevens zijn vereist. 
3. (Optioneel) Een besturingselement-tabel maken en definieer het bestand voor het partitioneren van de bestanden moeten worden gemigreerd. De manier voor het partitioneren van de bestanden is: 

    - De partitie met de mapnaam of de naam van de map met een wildcard-filter. U wordt aangeraden deze methode.
    - Partitie van een bestand het tijdstip laatst gewijzigd.

### <a name="network-bandwidth-and-storage-io"></a>Netwerk bandbreedte en -opslag i/o 

U kunt de waarde voor concurrency van Data Factory kopiëren taken die gegevens van Data Lake Storage Gen1 lezen en schrijven van gegevens naar Data Lake Storage Gen2 beheren. Op deze manier kunt u het gebruik in dat opslagsysteem i/o om te voorkomen die betrekking hebben op het normale zakelijke werk in Data Lake Storage Gen1 tijdens de migratie kunt beheren.

### <a name="permissions"></a>Machtigingen 

In Data Factory, de [Data Lake Storage Gen1 connector](connector-azure-data-lake-store.md) ondersteunt service-principal en beheerde identiteit voor Azure-resource-verificaties. De [Data Lake Storage Gen2 connector](connector-azure-data-lake-storage.md) ondersteunt account sleutel, service-principal en beheerde identiteit voor Azure-resource-verificaties. Data Factory kunt navigeren en kopieert u alle bestanden of toegangsbeheerlijsten (ACL's) u nodig hebt, hoog voldoende machtigingen voor het account dat u opgeeft als u wilt openen verlenen, lezen of schrijven van alle bestanden en ACL's ingesteld als u wilt maken. Het geven een functie van supergebruiker of eigenaar tijdens de migratieperiode. 

### <a name="preserve-acls-from-data-lake-storage-gen1"></a>ACL's van Data Lake Storage Gen1 behouden

Als u repliceren van de ACL's samen met bestanden wilt wanneer u een upgrade van Data Lake Storage Gen1 naar Data Lake Storage Gen2 uitvoeren, Zie [behouden ACL's van Data Lake Storage Gen1](connector-azure-data-lake-storage.md#preserve-acls-from-data-lake-storage-gen1). 

### <a name="incremental-copy"></a>Incrementele kopie 

Verschillende manieren kunt u alleen de nieuwe of bijgewerkte bestanden vanuit Data Lake Storage Gen1 laden:

- Nieuwe of bijgewerkte bestanden met de tijd gepartitioneerde bestand of map naam laden. Er is een voorbeeld/2019/05/13 / *.
- Nieuwe of bijgewerkte bestanden door LastModifiedDate laden.
- Nieuwe of bijgewerkte bestanden door een hulpprogramma van derden of oplossing identificeren. Geeft de naam van het bestand of map aan de Data Factory-pijplijn via de parameter of een tabel of bestand. 

De juiste frequentie incrementeel laden doen, is afhankelijk van het totale aantal bestanden in Azure Data Lake Storage Gen1 en het volume van de nieuwe of bijgewerkte bestanden moeten worden geladen telkens. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Overzicht kopieeractiviteit](copy-activity-overview.md)
> [connector voor Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md)
> [Azure Data Lake Storage Gen2-connector](connector-azure-data-lake-storage.md)