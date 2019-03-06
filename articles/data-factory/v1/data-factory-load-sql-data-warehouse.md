---
title: Terabytes aan gegevens laden in SQL Data Warehouse | Microsoft Docs
description: Laat zien hoe 1 TB aan gegevens kunnen worden geladen in Azure SQL Data Warehouse minder dan 15 minuten met Azure Data Factory
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: a6c133c0-ced2-463c-86f0-a07b00c9e37f
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 727a503a2fd942e6fbef9009c760a39f90828d2f
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57433124"
---
# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-data-factory"></a>1 TB laden in Azure SQL Data Warehouse minder dan 15 minuten met Data Factory
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [gegevens kopiëren naar of van Azure SQL Data Warehouse met behulp van Data Factory](../connector-azure-sql-data-warehouse.md).


[Azure SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) is een cloud-gebaseerde, scale-out database staat van de verwerking van grote hoeveelheden relationele en niet-relationele gegevens.  Gebouwd op een aanzienlijke parallelle verwerkingsarchitectuur (MPP) en is SQL Data Warehouse geoptimaliseerd voor werkbelastingen van datawarehouses enterprise.  Cloudflexibiliteit biedt de flexibiliteit om te schalen storage en compute-onafhankelijk van elkaar.

Aan de slag met Azure SQL Data Warehouse is nu eenvoudiger dan ooit met behulp van **Azure Data Factory**.  Azure Data Factory is een volledig beheerde cloud-gebaseerde service voor gegevensintegratie, die kan worden gebruikt voor het vullen van een SQL Data Warehouse met de gegevens van uw bestaande systeem en bespaart u kostbare tijd bij het evalueren van SQL Data Warehouse en het bouwen van uw analyses oplossingen. Hier volgen de belangrijkste voordelen van het laden van gegevens in Azure SQL Data Warehouse met behulp van Azure Data Factory:

* **Eenvoudig**: 5-stap intuïtieve wizard met geen scripts vereist.
* **Uitgebreide ondersteuning voor gegevensopslag**: ingebouwde ondersteuning voor een uitgebreide set on-premises en cloud-gebaseerde gegevensarchieven.
* **Beveiligd en compatibel**: gegevens worden overgedragen via HTTPS of ExpressRoute en global service aanwezigheid zorgt ervoor dat uw gegevens nooit de geografische grens verlaat
* **Ongeëvenaarde prestaties met behulp van PolyBase** – met behulp van Polybase is de meest efficiënte manier om gegevens te verplaatsen naar Azure SQL Data Warehouse. Met behulp van de staging blob-functie, kunt u hoge belasting snelheden vanuit allerlei typen gegevensarchieven naast Azure Blob-opslag, waarmee de Polybase biedt standaard ondersteuning voor bereiken.

Dit artikel ziet u hoe u de Kopieerwizard van Data Factory met 1 TB gegevens uit Azure Blob-opslag laden in Azure SQL Data Warehouse in minder dan 15 minuten, met meer dan 1,2 GBps-doorvoer.

In dit artikel bevat stapsgewijze instructies voor het verplaatsen van gegevens in Azure SQL Data Warehouse met behulp van de Wizard kopiëren.

> [!NOTE]
>  Raadpleeg voor algemene informatie over de mogelijkheden van Data Factory in om gegevens te verplaatsen naar/van Azure SQL Data Warehouse [verplaatsen van gegevens en naar Azure SQL Data Warehouse met behulp van Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) artikel.
>
> U kunt ook bouwen met behulp van Azure portal, Visual Studio, PowerShell, pijplijnen enzovoort. Zie [zelfstudie: Gegevens kopiëren van Azure-Blob naar Azure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor een snel overzicht met stapsgewijze instructies voor het gebruik van de Kopieeractiviteit in Azure Data Factory.  
>
>

## <a name="prerequisites"></a>Vereisten
* Azure Blob Storage: dit experiment maakt gebruik van Azure Blob-opslag (GRS) voor het opslaan van TPC-H testen gegevensset.  Als u een Azure storage-account hebt, krijgt u informatie [over het maken van een storage-account](../../storage/common/storage-quickstart-create-account.md).
* [TPC-H](http://www.tpc.org/tpch/) gegevens: we gaan TPC-H gebruiken als de tests gegevensset.  Om dit te doen, moet u gebruiken `dbgen` van TPC-H toolkit, waarmee u de gegevensset te genereren.  U kunt de broncode voor downloaden `dbgen` van [TPC-hulpprogramma's](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) en zelf samenstellen, of downloaden van het gecompileerde binaire bestand van [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).  Dbgen.exe uitvoeren met de volgende opdrachten voor het genereren van plat bestand 1 TB voor `lineitem` tabel verspreid over 10 bestanden:

  * `Dbgen -s 1000 -S **1** -C 10 -T L -v`
  * `Dbgen -s 1000 -S **2** -C 10 -T L -v`
  * …
  * `Dbgen -s 1000 -S **10** -C 10 -T L -v`

    Nu de gegenereerde bestanden kopiëren naar Azure-Blob.  Raadpleeg [verplaatsen van gegevens en naar een on-premises bestandssysteem met behulp van Azure Data Factory](data-factory-onprem-file-system-connector.md) voor hoe u dat doet met ADF-kopie.    
* Azure SQL datawarehouse: dit experiment laadt gegevens in Azure SQL Data Warehouse gemaakt met 6000 dwu 's

    Raadpleeg [maken van een Azure SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) voor gedetailleerde instructies voor het maken van een SQL Data Warehouse-database.  Als u de best mogelijke load-prestaties in SQL Data Warehouse met Polybase, kiezen wij maximumaantal Data Warehouse Units (dwu's) zijn toegestaan in de instelling van de prestaties, die 6000 dwu's.

  > [!NOTE]
  > Bij het laden van Azure-Blob, is het laden van prestaties van de gegevens rechtstreeks in verhouding met het aantal dwu's die u op de SQL Data Warehouse configureert:
  >
  > Het laden van 1 TB in 1000 duurt DWU SQL Data Warehouse 87 minuten (~ 200 MBps doorvoer) het laden van 1 TB in 2000 DWU SQL Data Warehouse wordt 46 minuten (~ 380 MBps doorvoer) het laden van 1 TB in 6000 DWU SQL Data Warehouse duurt 14 minuten (doorvoer ~1.2 GBps)
  >
  >

    Voor het maken van een SQL Data Warehouse met 6000 dwu's, de schuifregelaar prestaties helemaal naar rechts:

    ![Schuifregelaar voor prestaties](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    Voor een bestaande database die niet is geconfigureerd met 6000 dwu's, kunt u de schaal met behulp van Azure portal.  Ga naar de database in Azure portal en er is een **schaal** knop in de **overzicht** deelvenster wordt weergegeven in de volgende afbeelding:

    ![Knop voor schaal](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    Klik op de **schaal** knop opent u het volgende deelvenster en klikt u op de schuifregelaar naar de maximale waarde **opslaan** knop.

    ![Dialoogvenster schaal](media/data-factory-load-sql-data-warehouse/scale-dialog.png)

    Dit experiment gegevens in met behulp van Azure SQL Data Warehouse laadt `xlargerc` resourceklasse.

    Voor het bereiken van de best mogelijke doorvoer up moet worden uitgevoerd met behulp van een SQL Data Warehouse-gebruiker die behoort tot `xlargerc` resourceklasse.  Leer hoe u dit doen door het volgende [wijzigen van een voorbeeld van een gebruiker resource klasse](../../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md).  
* Doelschema tabel maken in Azure SQL Data Warehouse-database door het uitvoeren van de volgende DDL-instructie:

    ```SQL  
    CREATE TABLE [dbo].[lineitem]
    (
        [L_ORDERKEY] [bigint] NOT NULL,
        [L_PARTKEY] [bigint] NOT NULL,
        [L_SUPPKEY] [bigint] NOT NULL,
        [L_LINENUMBER] [int] NOT NULL,
        [L_QUANTITY] [decimal](15, 2) NULL,
        [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
        [L_DISCOUNT] [decimal](15, 2) NULL,
        [L_TAX] [decimal](15, 2) NULL,
        [L_RETURNFLAG] [char](1) NULL,
        [L_LINESTATUS] [char](1) NULL,
        [L_SHIPDATE] [date] NULL,
        [L_COMMITDATE] [date] NULL,
        [L_RECEIPTDATE] [date] NULL,
        [L_SHIPINSTRUCT] [char](25) NULL,
        [L_SHIPMODE] [char](10) NULL,
        [L_COMMENT] [varchar](44) NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    ```
Met de vereiste stappen voltooid, zijn er nu klaar om te configureren van de kopieeractiviteit in de Wizard kopiëren.

## <a name="launch-copy-wizard"></a>De wizard Kopiëren starten
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **een resource maken** Klik in de linkerbovenhoek op **Intelligence en analyse**, en klikt u op **Data Factory**.
3. In de **nieuwe data factory** deelvenster:

   1. Voer **LoadIntoSQLDWDataFactory** voor de **naam**.
       De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als de volgende foutmelding wordt weergegeven: **Naam gegevensfactory 'LoadIntoSQLDWDataFactory' is niet beschikbaar**, wijzigt u de naam van de data factory (bijvoorbeeld yournameLoadIntoSQLDWDataFactory) en probeert u het opnieuw. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](data-factory-naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.  
   2. Selecteer uw Azure-**abonnement**.
   3. Voer een van de volgende stappen uit voor de resourcegroep:
      1. Selecteer **Bestaande gebruiken** om een bestaande resourcegroep te selecteren.
      2. Selecteer **Nieuwe maken** als u een naam voor een resourcegroep wilt typen.
   4. Selecteer een **locatie** voor de gegevensfactory.
   5. Selecteer het selectievakje **Vastmaken aan dashboard** onderaan de blade.  
   6. Klik op **Create**.
4. Wanneer het aanmaken is voltooid, ziet u de blade **Gegevensfactory** zoals op de volgende afbeelding wordt weergegeven:

   ![Startpagina van de gegevensfactory](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
5. Op de startpagina van de gegevensfactory klikt u op de tegel **Gegevens kopiëren** om de **wizard Kopiëren** te openen.

   > [!NOTE]
   > Als u ziet dat de webbrowser is vastgelopen bij Autoriseren... schakelt u **Cookies van derden en sitegegevens blokkeren** uit, of u laat dit ingeschakeld en u maakt een uitzondering voor **login.microsoftonline.com** en opent u de wizard opnieuw.
   >
   >

## <a name="step-1-configure-data-loading-schedule"></a>Stap 1: Schema laden van gegevens configureren
De eerste stap is het configureren van het schema laden van gegevens.  

Op de pagina **Eigenschappen**:

1. Voer **CopyFromBlobToAzureSqlDataWarehouse** voor **taaknaam**
2. Selecteer **nu één keer uitvoeren** optie.   
3. Klik op **volgende**.  

    ![De Wizard kopiëren - pagina eigenschappen](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>Stap 2: Bron configureren
Deze sectie leest u de stappen voor het configureren van de bron: Azure-Blob met de 1 TB TPC-H regelitem bestanden.

1. Selecteer de **Azure Blob Storage** als de gegevens opslaan en klik op **volgende**.

    ![De Wizard kopiëren - pagina van de bron selecteren](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

2. Vul de verbindingsgegevens voor de Azure Blob storage-account en klik op **volgende**.

    ![Wizard - informatie van de gegevensbronverbinding kopiëren](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

3. Kies de **map** met de TPC-H item bestanden regel en klikt u op **volgende**.

    ![De Wizard kopiëren - invoermap selecteren](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

4. Wanneer u op klikt **volgende**, de bestandsindelingsinstellingen automatisch worden gedetecteerd.  Controleer of het kolomscheidingsteken voor die is ' | 'in plaats van de standaard-komma','.  Klik op **volgende** nadat u de gegevens hebt bekeken.

    ![De Wizard kopiëren - bestandsindelingsinstellingen](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>Stap 3: Doel configureren
Deze sectie leest u hoe u het configureren van de bestemming: `lineitem` tabel in de Azure SQL Data Warehouse-database.

1. Kies **Azure SQL Data Warehouse** als doel opslaan en klik op **volgende**.

    ![De Wizard kopiëren - gegevensarchief van de doelserver selecteren](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

2. Vul de verbindingsgegevens voor Azure SQL Data Warehouse.  Zorg ervoor dat u opgeeft dat de gebruiker die lid is van de rol `xlargerc` (Zie de **vereisten** sectie voor gedetailleerde instructies), en klikt u op **volgende**.

    ![Wizard - doel-verbindingsgegevens kopiëren](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

3. Kies de doeltabel en klik op **volgende**.

    ![Wizard voor kopiëren - pagina tabeltoewijzing](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

4. In de pagina schematoewijzing, laat u optie 'Toepassen in de kolomtoewijzing' is niet ingeschakeld en klik op **volgende**.

## <a name="step-4-performance-settings"></a>Stap 4: Prestatie-instellingen

**Toestaan van polybase** is standaard ingeschakeld.  Klik op **volgende**.

![Wizard voor kopiëren - pagina schematoewijzing](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>Stap 5: Implementeren en bewaken van de load-resultaten
1. Klik op **voltooien** knop om te implementeren.

    ![De Wizard kopiëren - overzichtspagina](media/data-factory-load-sql-data-warehouse/summary-page.png)

2. Nadat de implementatie voltooid is, klikt u op `Click here to monitor copy pipeline` voor het bewaken van het exemplaar wordt uitgevoerd worden uitgevoerd. Selecteer de kopieerpijplijn die u hebt gemaakt in de **activiteit Windows** lijst.

    ![De Wizard kopiëren - overzichtspagina](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

    Vindt u de kopie details van de uitvoering de **Activiteitsvensterverkenner** in het rechter deelvenster, met inbegrip van het gegevensvolume bron gelezen en geschreven naar de bestemming, de duur en de gemiddelde doorvoer voor de uitvoering.

    Zoals u in de volgende schermafbeelding ziet, duurde 1 TB kopiëren van Azure Blob Storage naar SQL Data Warehouse 14 minuten, effectief bereiken 1,22 GBps doorvoer!

    ![Wizard - geslaagd dialoogvenster kopiëren](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>Aanbevolen procedures
Hier volgen enkele aanbevolen procedures voor het uitvoeren van uw Azure SQL Data Warehouse-database:

* Een grotere resourceklasse gebruiken bij het laden in een GECLUSTERDE COLUMNSTORE-INDEX.
* Voor een efficiëntere joins, kunt u overwegen hash-distributie met een kolom selecteren in plaats van standaard round robin-distributie.
* Voor hogere load snelheden worden bereikt, kunt u overwegen heap voor tijdelijke gegevens.
* Statistieken maakt nadat u klaar bent met het laden van Azure SQL Data Warehouse.

Zie [aanbevolen procedures voor Azure SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-best-practices.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
* [Kopieerwizard van Data Factory](data-factory-copy-wizard.md) -in dit artikel bevat informatie over de Wizard kopiëren.
* [Kopiëren en afstemmingshandleiding activiteit](data-factory-copy-activity-performance.md) -in dit artikel bevat de metingen van naslaginformatie over de prestaties en afstemmingshandleiding.
