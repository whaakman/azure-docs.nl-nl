---
title: Gegevens kopiëren van Azure Data Factory naar Azure Data Explorer
description: In dit onderwerp leert u voor het opnemen van gegevens (laden) in Azure Data Explorer met behulp van Azure Data Factory-hulpprogramma voor kopiëren
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: c3c5484d865c73a6f478ffc9ad4c3fc86c2c8170
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59618998"
---
# <a name="copy-data-from-azure-data-factory-to-azure-data-explorer"></a>Gegevens kopiëren van Azure Data Factory naar Azure Data Explorer

Azure Data Explorer is een snelle, volledig beheerde data analytics-service voor realtime analyse van grote volumes aan gegevensstromen van verschillende bronnen, zoals toepassingen, websites en IoT-apparaten. Iteratief gegevens verkennen en identificeren van patronen en afwijkingen te verbeteren van producten en verbetert de ervaringen voor klanten, bewaken van apparaten en bewerkingen te vergroten. Ontdek nieuwe vragen en krijg antwoorden binnen enkele minuten. Azure Data Factory is een volledig beheerde cloud-gebaseerde service voor gegevensintegratie. U kunt de service aan uw Azure Data Explorer-database met gegevens uit uw bestaande systeem te vullen en bespaar tijd bij het bouwen van uw analyseoplossingen.

Azure Data Factory biedt de volgende voordelen voor het laden van gegevens in Azure Data Explorer:

* **Eenvoudig**: Een intuïtieve vijf stappen wizard met geen scripts vereist.
* **Uitgebreide gegevens store ondersteunen**: Ingebouwde ondersteuning voor een uitgebreide set on-premises en cloud-gebaseerde gegevensarchieven. Zie de tabel van voor een gedetailleerde lijst [ondersteunde gegevensarchieven](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats).
* **Beveiligd en compatibel**: Gegevens worden overgedragen via HTTPS of ExpressRoute. De aanwezigheid van de algemene service zorgt ervoor dat uw gegevens nooit de geografische grens verlaat.
* **Hoge prestaties**: Maximaal laadsnelheid 1-GB/s-gegevens in Azure Data Explorer. Zie voor meer informatie, [prestaties van de Kopieeractiviteit](/azure/data-factory/copy-activity-performance).

In dit artikel leest u hoe de gegevens kopiëren van Data Factory-hulpprogramma gebruiken om te laden van gegevens vanaf Amazon S3 in Azure Data Explorer. U kunt uitvoeren van gelijksoortige stappen als u wilt kopiëren van gegevens uit andere gegevensarchieven zoals [Azure Blob Storage](/azure/data-factory/connector-azure-blob-storage), [Azure SQL Database](/azure/data-factory/connector-azure-sql-database), [Azure SQL Data Warehouse](/azure/data-factory/connector-azure-sql-data-warehouse), [Google BigQuery](/azure/data-factory/connector-google-bigquery),[Oracle](/azure/data-factory/connector-oracle), en [bestandssysteem](/azure/data-factory/connector-file-system).

## <a name="prerequisites"></a>Vereisten

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis Azure-account](https://azure.microsoft.com/free/) aan voordat u begint.
* [Een cluster van Azure Data Explorer en -database](create-cluster-database-portal.md)
* Amazon S3.

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Selecteer de **een resource maken** knop (+) in de linkerbovenhoek van de portal > **Analytics** > **Data Factory**.

   ![Een nieuwe gegevensfactory maken](media/data-factory-load-data/create-adf.png)

1. In de **nieuwe data factory** pagina, geef waarden op voor de volgende velden en selecteer vervolgens **maken**.

    ![De pagina Nieuwe data factory](media/data-factory-load-data/my-new-data-factory.png)

    **Instelling**  | **Beschrijving van veld**
    |---|---|
    | **Naam** | Voer een unieke naam op voor uw data factory. Als u de foutmelding *' naam Data factory \"LoadADXDemo\" is niet beschikbaar '*, Geef een andere naam voor de data factory. Zie voor de naamgevingsregels van Data Factory-artefacten, [Data Factory-naamgevingsregels](/azure/data-factory/naming-rules).|
    | **Abonnement** | Selecteer uw Azure-abonnement waarin u wilt maken van de data factory. |
    | **Resourcegroep** | Selecteer **nieuw** en voer de naam van een nieuwe resourcegroep. Selecteer **gebruik bestaande**, hebt u een bestaande resourcegroep. |
    | **Versie** | Selecteer **V2** |
    | **Locatie** | Selecteer de locatie voor de data factory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevensarchieven die worden gebruikt door data factory, kunnen zich in andere locaties of regio's. |
    | | |

1. Selecteer meldingen op de werkbalk om het maakproces bewaken. Na het aanmaken is voltooid, gaat u naar de data factory die u hebt gemaakt. De **Data Factory** startpagina wordt geopend.

   ![Startpagina van de gegevensfactory](media/data-factory-load-data/data-factory-home-page.png)

1. Selecteer de **Author & Monitor** tegel om de toepassing in een afzonderlijk tabblad te starten.

## <a name="load-data-into-azure-data-explorer"></a>Gegevens laden in Azure Data Explorer

Gegevens kunnen worden geladen uit veel verschillende [gegevensarchieven](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats) in Azure Data Explorer. In dit onderwerp wordt uitgelegd het laden van gegevens vanaf Amazon S3.

Er zijn twee manieren om gegevens te laden in Azure Data Explorer met behulp van Azure Data Factory:

* Azure Data Factory-gebruikersinterface - [ **auteur** tabblad](/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory)
* [Azure Data Factory **Copy Data** hulpprogramma](/azure/data-factory/quickstart-create-data-factory-copy-data-tool) in dit artikel gebruikt.

### <a name="copy-data-from-amazon-s3-source"></a>Gegevens kopiëren van Amazon S3 (bron)

1. In de **aan de slag** weergeeft, schakelt de **Copy Data** tegel om het hulpprogramma Copy Data starten.

   ![Tegel voor hulpprogramma voor kopiëren](media/data-factory-load-data/copy-data-tool-tile.png)

1. In de **eigenschappen** pagina **taaknaam** en selecteer **volgende**.

    ![Kopiëren van de eigenschappen van gegevensbron](media/data-factory-load-data/copy-from-source.png)

1. In de **brongegevensarchief** pagina, klikt u op **+ nieuwe verbinding maken**.

    ![Brongegevens maken verbinding](media/data-factory-load-data/source-create-connection.png)

1. Selecteer **Amazon S3**, en selecteer vervolgens **doorgaan**

    ![Nieuwe gekoppelde service](media/data-factory-load-data/amazons3-select-new-linked-service.png)

1. In de **nieuwe gekoppelde Service (Amazon S3)** pagina, de volgende stappen uit:

    ![Amazon S3 gekoppelde service opgeven](media/data-factory-load-data/amazons3-new-linked-service-properties.png)

    * Geef **naam** van uw nieuwe gekoppelde service.
    * Selecteer **verbinding maken via integratieruntime** waarde in de vervolgkeuzelijst.
    * Geef de **toegangssleutel-ID** waarde.
    * Geef de **geheime toegangssleutel** waarde.
    * Selecteer **verbinding testen** voor het testen van de gekoppelde service-verbinding die u hebt gemaakt.
    * Selecteer **Voltooien**.

1. In de **brongegevensarchief** pagina, ziet u de nieuwe AmazonS31-verbinding. Selecteer **Volgende**.

   ![De verbinding gemaakt van brongegevensarchief](media/data-factory-load-data/source-data-store-created-connection.png)

1. In de **invoerbestand of invoermap kiezen** pagina:

    1. Blader naar de map/bestand dat u wilt kopiëren. Selecteer de map/bestand.
    1. Selecteer de flattenhierarchy zoals vereist. Houd **binaire kopie** uitgeschakeld.
    1. Selecteer **Volgende**.

    ![Het invoerbestand of de invoermap kiezen](media/data-factory-load-data/source-choose-input-file.png)

1. In de **instellingen bestandsindelingen** pagina selecteert u de relevante instellingen voor uw bestand en klik op **volgende**.

   ![Het invoerbestand of de invoermap kiezen](media/data-factory-load-data/source-file-format-settings.png)

### <a name="copy-data-into-azure-data-explorer-destination"></a>Gegevens kopiëren naar Azure Data Explorer (doel)

Azure Data Explorer nieuwe gekoppelde service is gemaakt om te kopiëren van de gegevens in de doeltabel van Azure Data Explorer (sink) die hieronder zijn opgegeven.

1. In de **doelgegevensarchief** pagina, kunt u gegevens in een bestaande verbinding opslaan of een nieuw gegevensarchief opgeven door te klikken op **+ nieuwe verbinding maken**.

    ![De pagina Doelgegevensarchief](media/data-factory-load-data/destination-create-connection.png)

1. In de **nieuwe gekoppelde Service** weergeeft, schakelt **Azure Data Explorer**, en selecteer vervolgens **doorgaan**

    ![Azure Data Explorer - nieuwe gekoppelde service selecteren](media/data-factory-load-data/adx-select-new-linked-service.png)

1. In de **nieuwe gekoppelde Service (Azure Data Explorer)** pagina, de volgende stappen uit:

    ![Nieuwe gekoppelde service ADX](media/data-factory-load-data/adx-new-linked-service.png)

   * Selecteer **naam** gekoppelde service voor Azure Data Explorer.
   * In **Accountselectiemethode**: 
        * Selecteer de **van Azure-abonnement** keuzerondje en selecteer uw **Azure-abonnement** account. Selecteer vervolgens uw **Cluster**. Houd er rekening mee de vervolgkeuzelijst worden alleen clusters groeperen die deel uitmaken van de gebruiker.
        * U kunt ook selecteren **handmatig invoeren** keuzerondje en voer uw **eindpunt**.
    * Geef de **Tenant**.
    * Voer **Service-principal-ID**.
    * Selecteer **sleutel van Service-principal** en voert u **Service-Principal sleutel**.
    * Selecteer uw **Database** in de vervolgkeuzelijst. U kunt ook selecteren **bewerken** selectievakje in en voer de databasenaam van uw.
    * Selecteer **verbinding testen** voor het testen van de gekoppelde service-verbinding die u hebt gemaakt. Als u verbinding met uw instelling, een groen vinkje maken kunt **verbinding tot stand gebracht** wordt weergegeven.
    * Selecteer **voltooien** om te maken van de gekoppelde service voltooien.

    > [!NOTE]
    > De service-principal wordt gebruikt door Azure Data Factory voor toegang tot de service Azure Data Explorer. Voor service-principal, [maken van een Azure Active Directory (Azure AD) service-principal](/azure/azure-stack/azure-stack-create-service-principals#manage-service-principal-for-azure-ad). Gebruik niet de **Azure Key Vault** methode.

1. De **doelgegevensarchief** wordt geopend. U hebt gemaakt verbinding voor de Azure Data Explorer is beschikbaar voor gebruik. Selecteer **volgende** om de verbinding te configureren.

    ![ADX doelgegevensarchief](media/data-factory-load-data/destination-data-store.png)

1. In **uitvoerbestand of uitvoermap kiezen**, voer de naam van de uitvoer instellingen vast en selecteer **volgende**.

    ![Geef de map voor uitvoer](media/data-factory-load-data/specify-path.png)

1. In **tabeltoewijzing**, stel de naam van de doel-tabel en selecteer **volgende**.

    ![Bestemming gegevensset tabeltoewijzing](media/data-factory-load-data/destination-dataset-table-mapping.png)

1. In de **kolomtoewijzing** pagina:
    * De eerste toewijzing wordt uitgevoerd door ADF volgens [ADF-schematoewijzing](/azure/data-factory/copy-activity-schema-and-type-mapping)
        * Stel de **kolomtoewijzingen** voor de doeltabel van Azure Data Factory. De standaardtoewijzing wordt weergegeven van bron naar de doeltabel ADF.
        * Hef de selectie van de kolommen die u niet nodig hebt voor het definiëren van de kolomtoewijzing.
    * De toewijzing van het tweede treedt op wanneer deze gegevens in tabelvorm in Azure Data Explorer wordt opgenomen. Toewijzing wordt uitgevoerd volgens [CSV toewijzingsregels](/azure/kusto/management/mappings#csv-mapping). Houd er rekening mee dat, zelfs als de brongegevens bevinden zich niet in CSV-indeling, ADF de gegevens in tabelvorm is geconverteerd, CSV-toewijzing is daarom alleen relevante de toewijzing van het in deze fase.
        * Onder **Azure Data Explorer (Kusto) sink eigenschappen** toevoegen de relevante **opname toewijzingsnaam** (optioneel) zodanig dat in de kolomtoewijzing kan worden gebruikt.
        * Als **opname toewijzingsnaam** is niet opgegeven, wordt "by-name" toewijzing volgorde gedefinieerd in **kolomtoewijzingen** sectie wordt uitgevoerd. Als "by-name" toewijzing is mislukt, probeert Azure Data Explorer voor opname van de gegevens in een volgorde 'kolom op positie' (maps door-positie als standaard).
    * Selecteer **volgende**

    ![Kolomtoewijzing bestemming-gegevensset](media/data-factory-load-data/destination-dataset-column-mapping.png)

1. Op de pagina **Instellingen** doet u het volgende:
    * Stel de relevante **fault tolerantie-instellingen**.
    * **Prestatie-instellingen**: Schakel fasering is niet van toepassing. **Geavanceerde instellingen** enkele overwegingen kosten. Laat de eigenschap omdat als er geen specifieke behoeften.
    * Selecteer **Volgende**.

    ![Instellingen van de gegevens kopiëren](media/data-factory-load-data/copy-data-settings.png)

1. In **samenvatting**, Controleer de instellingen en selecteer **volgende**.

    ![Gegevens beknopte kopiëren](media/data-factory-load-data/copy-data-summary.png)

1. In de **implementatiepagina**:
    * Selecteer **Monitor** overschakelen naar de **Monitor** tabblad en de status van de pijplijn (stroom wordt uitgevoerd, fouten en gegevens).
    * Selecteer **pijplijn bewerken** gekoppelde services, gegevenssets en pijplijnen te bewerken.
    * Selecteer **voltooien** volledige kopie gegevens taak

    ![De pagina Implementatie](media/data-factory-load-data/deployment.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het bewerken van gekoppelde services, gegevenssets en pijplijnen in de [Data Factory-UI](/azure/data-factory/quickstart-create-data-factory-portal)

* Meer informatie over [query's van Azure Data Explorer](/azure/data-explorer/web-query-data) voor het opvragen van gegevens.
