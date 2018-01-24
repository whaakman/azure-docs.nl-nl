---
title: Terabytes aan gegevens laden in SQL Data Warehouse | Microsoft Docs
description: Laat zien hoe 1 TB aan gegevens kunnen worden geladen in Azure SQL Data Warehouse onder 15 minuten met Azure Data Factory
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: a6c133c0-ced2-463c-86f0-a07b00c9e37f
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 3350645d4f173a6d0d007ff9095bb3115600a13b
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-data-factory"></a>Laden van 1 TB in Azure SQL Data Warehouse onder 15 minuten met Data Factory
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [kopiëren van gegevens of naar Azure SQL Data Warehouse met behulp van de Data Factory versie 2](../connector-azure-sql-data-warehouse.md).


[Azure SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) is een cloud-gebaseerde, uitbreidbare database geschikt is voor het verwerken van grote hoeveelheden relationele en niet-relationele gegevens.  Gebaseerd op de uitgebreide parallelle verwerkingsarchitectuur (MPP) en is SQL Data Warehouse geoptimaliseerd voor enterprise datawarehouse-werkbelastingen.  Elasticiteit cloud biedt de flexibiliteit om te schalen van opslag en onafhankelijk berekenen.

Aan de slag met Azure SQL Data Warehouse is nu eenvoudiger dan ooit met **Azure Data Factory**.  Azure Data Factory is een volledig beheerde cloud-gebaseerde integration-gegevensservice kan worden gebruikt voor het vullen van een SQL Data Warehouse met de gegevens uit uw bestaande systeem en bespaart u kostbare tijd bij het evalueren van SQL Data Warehouse en het bouwen van uw analyses oplossingen. Hier volgen de belangrijkste voordelen van het laden van gegevens in Azure SQL Data Warehouse met behulp van Azure Data Factory:

* **Eenvoudig instellen van**: 5 stap intuïtieve wizard geen scripts nodig.
* **Uitgebreide ondersteuning voor gegevensopslag**: ingebouwde ondersteuning voor een groot aantal on-premises en cloud-gebaseerde gegevensarchieven.
* **Beveiligd en compatibel**: gegevens worden overgedragen via HTTPS of ExpressRoute en global service aanwezigheid zorgt ervoor dat uw gegevens de geografische grens nooit verlaten
* **Ongeëvenaarde prestaties door middel van PolyBase** – met behulp van Polybase is de meest efficiënte manier om gegevens te verplaatsen naar Azure SQL Data Warehouse. De fasering blob-functie kunt u hoge belasting snelheden van alle typen gegevensarchieven naast Azure Blob storage, die ondersteuning biedt voor de Polybase standaard bereiken.

In dit artikel leest u hoe Data Factory-Wizard kopiëren gebruiken om u te 1 TB gegevens uit Azure Blob-opslag laden in Azure SQL Data Warehouse in onder 15 minuten op meer dan 1,2 GBps doorvoer.

In dit artikel bevat stapsgewijze instructies voor het verplaatsen van gegevens naar Azure SQL Data Warehouse met behulp van de Wizard kopiëren.

> [!NOTE]
>  Raadpleeg voor algemene informatie over de mogelijkheden van Data Factory bij het verplaatsen van gegevens naar/van Azure SQL Data Warehouse [gegevens verplaatsen en naar Azure SQL Data Warehouse met behulp van Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) artikel.
>
> U kunt ook samenstellen pijplijnen met Azure portal, Visual Studio, PowerShell, enzovoort. Zie [zelfstudie: gegevens kopiëren van Azure-Blob naar Azure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor een snel overzicht met stapsgewijze instructies voor het gebruik van de Kopieeractiviteit in Azure Data Factory.  
>
>

## <a name="prerequisites"></a>Vereisten
* Azure Blob Storage: dit experiment maakt gebruik van Azure Blob-opslag (GRS) voor het opslaan van TPC-H testen gegevensset.  Als u een Azure storage-account niet hebt, ontdek [het maken van een opslagaccount](../../storage/common/storage-create-storage-account.md#create-a-storage-account).
* [TPC-H](http://www.tpc.org/tpch/) gegevens: we gaan TPC-H gebruiken als de tests gegevensset.  Hiervoor moet u gebruiken `dbgen` uit de werkset TPC-H, die u helpt bij het genereren van de gegevensset.  Kunt u de broncode voor downloaden `dbgen` van [TPC extra](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) zelf compileren en downloaden van de gecompileerde binaire van [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).  Dbgen.exe uitvoeren met de volgende opdrachten voor het genereren van 1 TB plat bestand voor `lineitem` tabel verspreiding in 10 bestanden:

  * `Dbgen -s 1000 -S **1** -C 10 -T L -v`
  * `Dbgen -s 1000 -S **2** -C 10 -T L -v`
  * …
  * `Dbgen -s 1000 -S **10** -C 10 -T L -v`

    Nu de gegenereerde bestanden kopiëren naar Azure-Blob.  Raadpleeg [gegevens verplaatsen naar en van een on-premises bestandssysteem met behulp van Azure Data Factory](data-factory-onprem-file-system-connector.md) voor hoe dat met ADF-exemplaar.    
* Azure SQL datawarehouse: dit experiment laadt gegevens in Azure SQL Data Warehouse is gemaakt met 6000 dwu 's

    Raadpleeg [maken van een Azure SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) voor gedetailleerde instructies voor het maken van een SQL Data Warehouse-database.  Als u de beste prestaties van de mogelijke laden in SQL Data Warehouse met Polybase, kiest u maximale aantal Data Warehouse Units (dwu's) zijn toegestaan in de prestatie-instelling, 6000 dwu's.

  > [!NOTE]
  > Bij het laden van een Azure Blob, is het laden van de prestaties van gegevens rechtstreeks evenredig met het aantal dwu's die u op de SQL Data Warehouse configureert:
  >
  > Laden van 1 TB in 1.000 duurt DWU SQL Data Warehouse 87 minuten (~ 200 MBps doorvoer) bij het laden van 1 TB in 2.000 DWU SQL Data Warehouse vergt 46 minuten (~ 380 MBps doorvoer) bij het laden van 1 TB in 6000 DWU SQL Data Warehouse duurt 14 minuten (~1.2 GBps doorvoer)
  >
  >

    Als een SQL Data Warehouse maken met 6000 dwu's, de schuifregelaar prestaties helemaal naar rechts:

    ![Schuifregelaar voor prestaties](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    Voor een bestaande database die niet is geconfigureerd met 6000 dwu's, kunt u het schalen met behulp van Azure-portal.  Navigeer naar de database in Azure-portal en er is een **Scale** knop in de **overzicht** deelvenster wordt weergegeven in de volgende afbeelding:

    ![Knop schaal](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    Klik op de **Scale** knop opent u het volgende deelvenster en klik op de schuifregelaar aan de maximumwaarde **opslaan** knop.

    ![Dialoogvenster schaal](media/data-factory-load-sql-data-warehouse/scale-dialog.png)

    Dit experiment gegevens laadt in met behulp van Azure SQL Data Warehouse `xlargerc` bronklasse.

    Als u wilt bereiken best mogelijke doorvoer, kopie moet worden uitgevoerd met behulp van een SQL Data Warehouse-gebruiker die horen bij `xlargerc` bronklasse.  Meer informatie over hoe dat door de volgende [wijzigen van een voorbeeld van een gebruiker resource klasse](../../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md).  
* Tabel doelschema in Azure SQL Data Warehouse-database maken door het uitvoeren van de volgende DDL-instructie:

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
Met de vereiste stappen voltooid, zijn er nu klaar om te configureren met de kopieerbewerking met de Wizard kopiëren.

## <a name="launch-copy-wizard"></a>De wizard Kopiëren starten
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **+ nieuw** vanuit de linkerbovenhoek op **Intelligence en analyse**, en klik op **Data Factory**.
3. In de blade **Nieuwe gegevensfactory**:

   1. Voer **LoadIntoSQLDWDataFactory** voor de **naam**.
       De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als u de foutmelding: **Data factory-naam 'LoadIntoSQLDWDataFactory' is niet beschikbaar**, wijzig de naam van de gegevensfactory (bijvoorbeeld yournameLoadIntoSQLDWDataFactory) en probeert u het opnieuw. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](data-factory-naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.  
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

## <a name="step-1-configure-data-loading-schedule"></a>Stap 1: Planning laden van gegevens configureren
De eerste stap is om de gegevens bij het laden van schema te configureren.  

Op de pagina **Eigenschappen**:

1. Voer **CopyFromBlobToAzureSqlDataWarehouse** voor **taaknaam**
2. Selecteer **eenmaal nu uitvoeren** optie.   
3. Klik op **Volgende**.  

    ![Wizard voor kopiëren - pagina eigenschappen](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>Stap 2: Een bron configureren
Deze sectie leest u de stappen voor het configureren van de bron: Azure Blob met de TPC 1 TB-H regelitem bestanden.

1. Selecteer de **Azure Blob Storage** de gegevens opslaan en klik op **volgende**.

    ![Wizard voor kopiëren - pagina van de bron selecteren](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

2. Vul de verbindingsgegevens voor de Azure Blob storage-account in en klik op **volgende**.

    ![Wizard - informatie van de gegevensbronverbinding kopiëren](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

3. Kies de **map** met de TPC-H item bestanden regel en klik op **volgende**.

    ![Wizard kopiëren - invoermap selecteren](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

4. Wanneer u op klikt **volgende**, de instellingen van het bestand-indeling worden automatisch gedetecteerd.  Controleer of het kolomscheidingsteken voor die is ' | 'in plaats van de standaard-komma','.  Klik op **volgende** nadat u de gegevens hebt bekeken.

    ![Wizard voor kopiëren - bestandsindelingsinstellingen](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>Stap 3: Doel configureren
Deze sectie leest u over het configureren van de bestemming: `lineitem` tabel in de Azure SQL Data Warehouse-database.

1. Kies **Azure SQL Data Warehouse** opslaan als het doel en klik op **volgende**.

    ![Wizard voor kopiëren - gegevensarchief van de doelserver selecteren](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

2. Vul in de verbindingsgegevens voor Azure SQL Data Warehouse.  Zorg ervoor dat u opgeeft dat de gebruiker die lid is van de rol `xlargerc` (Zie de **vereisten** gedeelte voor meer informatie), en klik op **volgende**.

    ![Wizard - verbindingsgegevens bestemming kopiëren](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

3. Kies de doeltabel en klik op **volgende**.

    ![Wizard - tabel toewijzing van pagina kopiëren](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

4. In de pagina Schema-toewijzing, laat u 'Toepassen kolomtoewijzing' optie uitgeschakeld en klik op **volgende**.

## <a name="step-4-performance-settings"></a>Stap 4: Prestatie-instellingen

**Toestaan van polybase** is standaard ingeschakeld.  Klik op **Volgende**.

![Wizard - schema toewijzing van pagina kopiëren](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>Stap 5: Implementeren en controleren van de load-resultaten
1. Klik op **voltooien** knop om te implementeren.

    ![Wizard voor kopiëren - pagina overzicht](media/data-factory-load-sql-data-warehouse/summary-page.png)

2. Nadat de implementatie voltooid is, klikt u op `Click here to monitor copy pipeline` voor het bewaken van de kopie voortgang uitvoeren. Selecteer de kopieerpijplijn die u hebt gemaakt in de **Activiteitsvensters** lijst.

    ![Wizard voor kopiëren - pagina overzicht](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

    Vindt u de kopie die details uitvoering van de **activiteit venster Explorer** in het rechterpaneel inclusief gegevensvolume van bron gelezen en geschreven naar de bestemming, duur en de gemiddelde doorvoer voor de verwerking.

    Zoals u in de volgende schermafdruk ziet, duurde 1 TB uit Azure Blob Storage kopiëren naar SQL Data Warehouse 14 minuten, effectief 1,22 GBps doorvoer kan bereiken!

    ![Wizard - geslaagd dialoogvenster kopiëren](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>Aanbevolen procedures
Hier volgen enkele aanbevolen procedures voor het uitvoeren van uw Azure SQL Data Warehouse-database:

* Gebruik een grotere bronklasse bij het laden in een GECLUSTERDE COLUMNSTORE-INDEX.
* Voor efficiënter joins, kunt u overwegen hash distributie door een kolom selecteren in plaats van standaard round robin distributie.
* Voor snellere load snelheden, kunt u overwegen heap voor tijdelijke gegevens.
* Statistieken maken als u klaar bent met het laden van Azure SQL Data Warehouse.

Zie [aanbevolen procedures voor Azure SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-best-practices.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
* [Data Factory-Wizard kopiëren](data-factory-copy-wizard.md) -in dit artikel bevat informatie over de Wizard kopiëren.
* [Prestaties van de activiteit en prestatieafstemming handleiding kopiëren](data-factory-copy-activity-performance.md) -in dit artikel bevat de verwijzing prestatiemetingen en prestatieafstemming handleiding.
