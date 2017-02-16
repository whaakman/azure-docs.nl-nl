---
title: Azure SQL Data Warehouse - zelfstudie | Microsoft Docs
description: Zelfstudie voor Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: barbkess
ms.assetid: 52DFC191-E094-4B04-893F-B64D5828A900
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 12/21/2016
ms.author: elbutter
translationtype: Human Translation
ms.sourcegitcommit: fe9de0ffad3fe5d4acbf3caf2f08101f6a13daaf
ms.openlocfilehash: 12f500c01671799612b0d1988e0d07bbfda600da


---
# <a name="get-started-with-sql-data-warehouse"></a>Aan de slag met SQL Data Warehouse

Zelfstudie voor Azure SQL Data Warehouse. In deze zelfstudie leert u de basisbeginselen van het inrichten en laden van gegevens in een SQL Data Warehouse, en een aantal basisbeginselen over schalen, pauzeren en afstemmen. 

**Geschatte duur:** 75 minuten

## <a name="prerequisites"></a>Vereisten


### <a name="sign-up-for-microsoft-azure"></a>Registreren voor Microsoft Azure
Als u nog geen Microsoft Azure-account hebt, dient u zich te registreren voordat u deze service kunt gebruiken. Deze stap kunt u overslaan als u al een account hebt. 

1. Ga naar de accountpagina [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/)
2. Maak een gratis Azure-account of koop een account.
3. Volg de instructies

### <a name="install-appropriate-sql-client-driver-and-tools"></a>Toepasselijke SQL-clientstuurprogramma en SQL-clienthulpprogramma’s installeren

De meeste SQL-clienthulpprogramma’s kunnen verbinding maken met Azure SQL Data Warehouse met behulp van JDBC, ODBC of ADO.net. Vanwege de complexiteit van het product en het grote aantal T-SQL-functies dat SQL Data Warehouse ondersteunt, is mogelijk niet elke clienttoepassing volledig compatibel met SQL Data Warehouse.

Als u een Windows-besturingssysteem gebruikt, raden we u aan [Visual Studio] of [SQL Server Management Studio] te gebruiken.


[!INCLUDE [Create a new logical server](../../includes/sql-data-warehouse-create-logical-server.md)] 

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="create-an-azure-sql-data-warehouse"></a>Een Azure SQL Data Warehouse maken

> [!NOTE]
> Het maken van een SQL Data Warehouse kan een nieuwe factureerbare service tot gevolg hebben.  Zie [Prijzen van SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/) voor meer informatie.
>


### <a name="create-a-sql-data-warehouse"></a>Een SQL Data Warehouse maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **Nieuw** > **Databases** > **SQL Data Warehouse**.

    ![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png)
    ![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)

3. Vul de implementatiegegevens in

    **Databasenaam**: kies een naam. Als u meerdere SQL DW-exemplaren hebt, raden we u aan details zoals de regio, omgeving enz. in de namen op te nemen, bijvoorbeeld *mydw-westus-1-test*

    **Abonnement:** uw Azure-abonnement

    **Resourcegroep**: maak een nieuwe (of gebruik een bestaande als u van plan bent uw Azure SQL Data Warehouse met andere services te gebruiken)
    > [!NOTE]
    > Services binnen een resourcegroep horen dezelfde levenscyclus te hebben. Resourcegroepen zijn handig voor het beheren van resources, waaronder het bereik van toegangsbeheer controleren en het implementeren met sjablonen. [Hier](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) vindt u meer informatie over resourcegroepen en aanbevolen procedures voor Azure
    >

    **Bron**: lege database

    **Server**: selecteer de server die u hebt gemaakt in [Vereisten].

    **Sortering**: laat de standaardsortering SQL_Latin1_General_CP1_CI_AS staan

    **Prestaties selecteren**: we raden aan de standaard 400 DWU te gebruiken

4. Kies **Vastmaken aan dashboard**
    ![Vastmaken aan dashboard](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png)

5. Wacht tot uw Azure SQL Data Warehouse begint met implementeren. Dit proces duurt normaal gesproken enkele minuten. De portal geeft een melding wanneer uw exemplaar is geïmplementeerd. 

## <a name="connect-to-azure-sql-data-warehouse-through-sql-server-logical-server"></a>Verbinding maken met Azure SQL Data Warehouse via SQL Server (logische server)

In deze zelfstudie leert u SQL Server Management Studio te gebruiken om verbinding te maken met SQL Data Warehouse. Er kunnen andere hulpprogramma's worden gebruikt om verbinding te maken met SQL Data Warehouse via onze ondersteunde connectors: ADO.NET, JDBC, ODBC en PHP. De functionaliteit is mogelijk beperkt bij hulpprogramma's die niet worden ondersteund door Microsoft.


### <a name="get-connection-information"></a>Verbindingsgegevens ophalen

Om verbinding met uw SQL Data Warehouse te maken, moet u eerst verbinding maken via de SQL Server (logische server) die u hebt gemaakt in [Vereisten].

1. Selecteer uw SQL Data Warehouse op het dashboard of zoek het in uw resources.

    ![Dashboard van SQL Data Warehouse](./media/sql-data-warehouse-get-started-tutorial/sql-dw-dashboard.png)

2. Vind de volledige naam voor de logische server.

    ![Servernaam selecteren](./media/sql-data-warehouse-get-started-tutorial/select-server.png)

3. Open SSMS en gebruik de objectverkenner om verbinding te maken met deze server middels de referenties die u hebt gemaakt in [Vereisten]

    ![Verbinden met SSMS](./media/sql-data-warehouse-get-started-tutorial/ssms-connect.png)

Als alles goed gaat, hebt u nu verbinding met uw SQL Server-exemplaar (logische server). Met deze serverreferenties kunt u zich bij elke database op de server als de database-eigenaar verifiëren. De aanbevolen procedure is echter om afzonderlijke aanmeldingen en gebruikers voor elke database te maken. In [Een gebruiker voor SQL Data Warehouse maken](./sql-data-warehouse-get-started-tutorial.md#create-a-user-for-sql-data-warehouse) behandelen we het maken van gebruikers. 

## <a name="create-a-user-for-sql-data-warehouse"></a>Een gebruiker voor SQL Data Warehouse maken

### <a name="why-create-a-separate-user"></a>Waarom is het handig een afzonderlijke gebruiker te maken?

We gebruiken de verbinding met de SQL Server (logische server), met de serverreferenties van de vorige stap, voor het maken van een nieuwe gebruiker voor de SQL Data Warehouse. Er zijn twee primaire redenen om een afzonderlijke gebruiker/aanmelding te maken voor uw SQL DW.

1.  Gebruikers van uw organisatie moeten een ander account gebruiken om te verifiëren. Op die manier kunt u het aantal machtigingen voor de toepassing beperken en de kans op schadelijke activiteiten verkleinen.

2. Standaard gebruikt de aanmelding van de serverbeheerder waarmee u momenteel verbonden bent een kleinere resourceklasse. Resourceklassen helpen u de toewijzing van geheugen en CPU-cycli die worden toegewezen aan een query te beheren. Gebruikers in **smallrc** krijgen een kleiner deel van het geheugen toegewezen en kunnen profiteren van hogere gelijktijdigheid. Daarentegen krijgen gebruikers in **xlargerc** een groter deel van het geheugen toegewezen, waardoor minder van hun query's gelijktijdig kunnen worden uitgevoerd. Voor het laden van gegevens op een manier die compressie optimaliseert, moet de gebruiker die gegevens laadt, deel uitmaken van een grotere resourceklasse. Meer informatie over resourceklassen vindt u [hier](./sql-data-warehouse-develop-concurrency.md#resource-classes):

### <a name="creating-a-user-of-a-larger-resource-class"></a>Een gebruiker voor een grotere resourceklasse maken

1. Query's uitvoeren op de **hoofddatabase** van uw server

    ![Nieuwe query in Hoofd](./media/sql-data-warehouse-get-started-tutorial/query-on-server.png)

    ![Nieuwe query in Hoofd1](./media/sql-data-warehouse-get-started-tutorial/query-on-master.png)

2. Een serveraanmelding en -gebruiker maken

    ```sql
    CREATE LOGIN XLRCLOGIN WITH PASSWORD = 'a123reallySTRONGpassword!';
    CREATE USER LoadingUser FOR LOGIN XLRCLOGIN;
    ```

3. Een query uitvoeren op de SQL Data Warehouse-database, een nieuwe databasegebruiker op basis van de serveraanmelding maken 
    ```sql
    CREATE USER LoadingUser FOR LOGIN XLRCLOGIN;
    ```

4. Gebruiker toegang tot de database geven
    ```sql
    GRANT CONTROL ON DATABASE::[NYT] to LoadingUser;
    ```
    > [!NOTE]
    > Als de databasenaam afbreekstreepjes bevat, omsluit u deze met haken. 
    >

5. Uw databasegebruiker toevoegen aan de resourceklasserol **xlargerc**
    ```sql
    EXEC sp_addrolemember 'xlargerc', 'LoadingUser';
    ```

6. Aanmelden bij de database met uw nieuwe referenties

    ![Aanmelden met nieuwe aanmeldgegevens](./media/sql-data-warehouse-get-started-tutorial/new-login.png)


## <a name="loading-data"></a>Gegevens laden

### <a name="defining-external-data"></a>Externe gegevens definiëren
1. Een hoofdsleutel maken en een externe gegevensbron definiëren

    ```sql
    CREATE MASTER KEY;

    CREATE EXTERNAL DATA SOURCE NYTPublic
    WITH
    (
    TYPE = Hadoop
    , LOCATION = 'wasbs://2013@nytpublic.blob.core.windows.net/'
    );
    ```


2. De externe bestandsindelingen definiëren

    De opdracht ```CREATE EXTERNAL FILE FORMAT``` wordt gebruikt om de indeling op te geven van de externe gegevens waaruit wordt geladen. Voor de gegevens van de openbare taxi’s van New York, hebben we de twee indelingen gebruikt voor het opslaan van gegevens in Azure Blob Storage

    ```sql
    CREATE EXTERNAL FILE FORMAT uncompressedcsv
    WITH
    ( FORMAT_TYPE = DELIMITEDTEXT
    , FORMAT_OPTIONS ( FIELD_TERMINATOR = ','
    , STRING_DELIMITER = ''
    , DATE_FORMAT = ''
    , USE_TYPE_DEFAULT = False
    )
    );

    CREATE EXTERNAL FILE FORMAT compressedcsv
    WITH
    ( FORMAT_TYPE = DELIMITEDTEXT
    , FORMAT_OPTIONS ( FIELD_TERMINATOR = '|'
    , STRING_DELIMITER = ''
    , DATE_FORMAT = ''
    , USE_TYPE_DEFAULT = False
    )
    , DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
    );
    ```

3.  Een schema voor de externe bestandsindeling maken

    ```sql
    CREATE SCHEMA ext;
    GO
    ```

4. Maak de externe tabellen. Deze tabellen verwijzen naar gegevens die zijn opgeslagen in HDFS of Azure Blob Storage. 

    ```sql
    CREATE EXTERNAL TABLE [ext].[Date] 
    (
    [DateID] int NOT NULL,
    [Date] datetime NULL,
    [DateBKey] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [DayOfMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [DaySuffix] varchar(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [DayName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [DayOfWeek] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [DayOfWeekInMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [DayOfWeekInYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [DayOfQuarter] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [DayOfYear] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [WeekOfMonth] varchar(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [WeekOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [WeekOfYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [Month] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [MonthName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [MonthOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [Quarter] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [QuarterName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [Year] char(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [YearName] char(7) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [MonthYear] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [MMYYYY] char(6) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [FirstDayOfMonth] date NULL,
    [LastDayOfMonth] date NULL,
    [FirstDayOfQuarter] date NULL,
    [LastDayOfQuarter] date NULL,
    [FirstDayOfYear] date NULL,
    [LastDayOfYear] date NULL,
    [IsHolidayUSA] bit NULL,
    [IsWeekday] bit NULL,
    [HolidayUSA] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
    LOCATION = 'Date'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
    )
    CREATE EXTERNAL TABLE [ext].[Geography]
    (
    [GeographyID] int NOT NULL,
    [ZipCodeBKey] varchar(10) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
    [County] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [City] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [State] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [Country] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [ZipCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
    LOCATION = 'Geography'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0 
    )
    ;
    CREATE EXTERNAL TABLE [ext].[HackneyLicense]
    (
    [HackneyLicenseID] int NOT NULL,
    [HackneyLicenseBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
    [HackneyLicenseCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
    LOCATION = 'HackneyLicense'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
    )
    ;
    CREATE EXTERNAL TABLE [ext].[Medallion]
    (
    [MedallionID] int NOT NULL,
    [MedallionBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
    [MedallionCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
    LOCATION = 'Medallion'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
    )
    ;
    CREATE EXTERNAL TABLE [ext].[Time]
    (
    [TimeID] int NOT NULL,
    [TimeBKey] varchar(8) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
    [HourNumber] tinyint NOT NULL,
    [MinuteNumber] tinyint NOT NULL,
    [SecondNumber] tinyint NOT NULL,
    [TimeInSecond] int NOT NULL,
    [HourlyBucket] varchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
    [DayTimeBucketGroupKey] int NOT NULL,
    [DayTimeBucket] varchar(100) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL
    )
    WITH
    (
    LOCATION = 'Time'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
    )
    ;
    CREATE EXTERNAL TABLE [ext].[Trip]
    (
    [DateID] int NOT NULL,
    [MedallionID] int NOT NULL,
    [HackneyLicenseID] int NOT NULL,
    [PickupTimeID] int NOT NULL,
    [DropoffTimeID] int NOT NULL,
    [PickupGeographyID] int NULL,
    [DropoffGeographyID] int NULL,
    [PickupLatitude] float NULL,
    [PickupLongitude] float NULL,
    [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [DropoffLatitude] float NULL,
    [DropoffLongitude] float NULL,
    [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [PassengerCount] int NULL,
    [TripDurationSeconds] int NULL,
    [TripDistanceMiles] float NULL,
    [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [FareAmount] money NULL,
    [SurchargeAmount] money NULL,
    [TaxAmount] money NULL,
    [TipAmount] money NULL,
    [TollsAmount] money NULL,
    [TotalAmount] money NULL
    )
    WITH
    (
    LOCATION = 'Trip2013'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = compressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
    )
    ;
    CREATE EXTERNAL TABLE [ext].[Weather]
    (
    [DateID] int NOT NULL,
    [GeographyID] int NOT NULL,
    [PrecipitationInches] float NOT NULL,
    [AvgTemperatureFahrenheit] float NOT NULL
    )
    WITH
    (
    LOCATION = 'Weather2013'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
    )
    ;
    ```

### <a name="create-table-as-select-ctas"></a>Create Table as Select (CTAS)

5. Laad uw gegevens uit externe tabellen in uw exemplaar van SQL Data Warehouse. 
    ```sql
    CREATE TABLE [dbo].[Date]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Date]
    OPTION (LABEL = 'CTAS : Load [dbo].[Date]')
    ;
    CREATE TABLE [dbo].[Geography]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Geography]
    OPTION (LABEL = 'CTAS : Load [dbo].[Geography]')
    ;
    CREATE TABLE [dbo].[HackneyLicense]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[HackneyLicense]
    OPTION (LABEL = 'CTAS : Load [dbo].[HackneyLicense]')
    ;
    CREATE TABLE [dbo].[Medallion]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Medallion]
    OPTION (LABEL = 'CTAS : Load [dbo].[Medallion]')
    ;
    CREATE TABLE [dbo].[Time]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Time]
    OPTION (LABEL = 'CTAS : Load [dbo].[Time]')
    ;
    CREATE TABLE [dbo].[Weather]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Weather]
    OPTION (LABEL = 'CTAS : Load [dbo].[Weather]')
    ;
    CREATE TABLE [dbo].[Trip]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Trip]
    OPTION (LABEL = 'CTAS : Load [dbo].[Trip]')
    ;
    ```

    > [!NOTE]
    > U laadt meerdere GB's aan gegevens en comprimeert die tot hoogwaardige geclusterde columnstore-indexen. Voer de volgende DMV-query uit en zet een kopje koffie terwijl Azure SQL Data Warehouse even flink aan de slag gaat.
    >

6. Maak een nieuwe query en zie hoe de gegevens binnenkomen met deze dynamische beheerweergave (DMV)

    ```sql
    SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files,
    sum(s.bytes_processed)/1024/1024 as gb_processed
    FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
    on r.request_id = s.request_id
    WHERE
    r.[label] = 'CTAS : Load [dbo].[Date]' OR
    r.[label] = 'CTAS : Load [dbo].[Geography]' OR
    r.[label] = 'CTAS : Load [dbo].[HackneyLicense]' OR
    r.[label] = 'CTAS : Load [dbo].[Medallion]' OR
    r.[label] = 'CTAS : Load [dbo].[Time]' OR
    r.[label] = 'CTAS : Load [dbo].[Weather]' OR
    r.[label] = 'CTAS : Load [dbo].[Trip]'
    GROUP BY
    r.command,
    s.request_id,
    r.status
    ORDER BY
    nbr_files desc, gb_processed desc;
    ```

7. Alle systeemquery's weergeven

    ```sql
    SELECT * FROM sys.dm_pdw_exec_requests;
    ```

8. Al uw gegevens zijn netjes geladen in uw Azure SQL Data Warehouse

    ![Geladen gegevens bekijken](./media/sql-data-warehouse-get-started-tutorial/see-data-loaded.png)



## <a name="querying-data"></a>Query’s op gegevens uitvoeren 

### <a name="scan-query-with-scaling"></a>Scanquery met schalen

Laten we bekijken hoe schalen de snelheid van uw query's beïnvloedt.

Voordat we beginnen, gaan we de bewerking omlaag schalen naar 100 DWU, zodat we een idee krijgen van hoe een rekenknooppunt afzonderlijk presteert.

1. Ga naar de portal en selecteer uw exemplaar van SQL Data Warehouse.

2. Selecteer de schaal in de blade SQL Data Warehouse. 

    ![DW schalen vanuit de portal](./media/sql-data-warehouse-get-started-tutorial/scale-dw.png)

3. Schaal de prestatiebalk omlaag naar 100 DWU en klik op opslaan.

    ![Schalen en opslaan](./media/sql-data-warehouse-get-started-tutorial/scale-and-save.png)

4. Wacht totdat de schaalbewerking is voltooid.

    > [!NOTE]
    > Let op: schaalbewerkingen **beëindigen** de query’s die momenteel worden uitgevoerd en voorkomen dat nieuwe query’s kunnen worden uitgevoerd.
    >
    
5. Voer een scanbewerking uit op de reisgegevens, waarbij u het eerste miljoen vermeldingen voor alle kolommen selecteert. Als u snel door wilt gaan, kunt u gerust minder rijen selecteren.

    ```sql
    SELECT TOP(1000000) * FROM dbo.[Trip]
    ```

Noteer de tijd die nodig was voor het uitvoeren van deze bewerking.

6. Schaal uw exemplaar omhoog naar 400 DWU. Met elke 100 DWU wordt een extra rekenknooppunt aan uw Azure SQL Data Warehouse toegevoegd.

7. Voer de opnieuw query uit. U zou een duidelijk verschil moeten zien. 

> [!NOTE]
> Azure SQL Data Warehouse is een MPP-platform (Massively Parallel Processing). Bij query's en bewerkingen die taken tussen verschillende knooppunten parallel kunnen uitvoeren, laat Azure SQL Data Warehouse zijn ware kracht zien.
>

### <a name="join-query-with-statistics"></a>Joinquery met statistieken

1. Een query uitvoeren die de tabel Date met de tabel Trip samenvoegt

    ```sql
    SELECT TOP (1000000) dt.[DayOfWeek]
    ,tr.[MedallionID]
    ,tr.[HackneyLicenseID]
    ,tr.[PickupTimeID]
    ,tr.[DropoffTimeID]
    ,tr.[PickupGeographyID]
    ,tr.[DropoffGeographyID]
    ,tr.[PickupLatitude]
    ,tr.[PickupLongitude]
    ,tr.[PickupLatLong]
    ,tr.[DropoffLatitude]
    ,tr.[DropoffLongitude]
    ,tr.[DropoffLatLong]
    ,tr.[PassengerCount]
    ,tr.[TripDurationSeconds]
    ,tr.[TripDistanceMiles]
    ,tr.[PaymentType]
    ,tr.[FareAmount]
    ,tr.[SurchargeAmount]
    ,tr.[TaxAmount]
    ,tr.[TipAmount]
    ,tr.[TollsAmount]
    ,tr.[TotalAmount]
    FROM [dbo].[Trip] as tr
    join
    dbo.[Date] as dt
    on tr.DateID = dt.DateID
    ```

    Zoals te verwachten valt, neemt de query veel meer tijd in beslag wanneer u gegevens tussen de knooppunten verdeelt, met name bij een samenvoegingsbewerking zoals deze query.

2. We gaan nu kijken hoe deze query verloopt wanneer we statistieken voor de samen te voegen kolom maken door het volgende uit te voeren:

    ```sql
    CREATE STATISTICS [dbo.Date DateID stats] ON dbo.Date (DateID);
    CREATE STATISTICS [dbo.Trip DateID stats] ON dbo.Trip (DateID);
    ```

    > [!NOTE]
    > SQL DW beheert niet automatisch statistieken voor u. Statistieken zijn belangrijk voor de prestaties van query's en we raden u ten zeerste aan statistieken te maken en bij te werken.
    > 
    > **U haalt het meeste voordeel uit statistieken bij kolommen die onderdeel uitmaken van samenvoegingen, kolommen met het WHERE-component en kolommen in GROUP BY.**
    >

3. Voer de query in Vereisten opnieuw uit en kijk of er prestatieverschillen zijn. De verschillen in de prestaties van query's zijn niet zo ingrijpend als wanneer u omhoog schaalt, maar u zou wel een aanmerkelijke versnelling moeten zien. 

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om query’s uit te voeren en te verkennen. Bekijk onze aanbevolen procedures en tips.

Als u klaar bent voor vandaag, zorg dan dat u uw exemplaar onderbreekt. U kunt enorm besparen op de productie door te onderbreken en schalen op basis van uw zakelijke behoeften.

![Onderbreken](./media/sql-data-warehouse-get-started-tutorial/pause.png)

## <a name="useful-readings"></a>Handige documentatie

[Gelijktijdigheid en werklastbeheer]

[Aanbevolen procedures voor Azure SQL Data Warehouse]

[Querybewaking]

[Top 10 aanbevolen procedures voor het bouwen van een grootschalige relationele Data Warehouse]

[Gegevens migreren naar Azure SQL Data Warehouse]


[Gelijktijdigheid en werklastbeheer]: sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example
[Aanbevolen procedures voor Azure SQL Data Warehouse]: sql-data-warehouse-best-practices.md#hash-distribute-large-tables
[Querybewaking]: sql-data-warehouse-manage-monitor.md
[Top 10 aanbevolen procedures voor het bouwen van een grootschalige relationele Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2013/09/16/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse/
[Gegevens migreren naar Azure SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/



[!INCLUDE [Additional Resources](../../includes/sql-data-warehouse-article-footer.md)]

<!-- Internal Links -->
[Vereisten]: sql-data-warehouse-get-started-tutorial.md#prerequisites

<!--Other Web references-->
[Visual Studio]: https://www.visualstudio.com/
[SQL Server Management Studio]: https://msdn.microsoft.com/en-us/library/mt238290.aspx



<!--HONumber=Jan17_HO3-->


