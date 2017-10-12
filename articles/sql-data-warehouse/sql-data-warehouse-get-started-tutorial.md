---
title: 'Azure SQL Data Warehouse: aan de slag-zelfstudie | Microsoft Docs'
description: In deze zelfstudie leert u hoe u gegevens kunt inrichten en laden in Azure SQL Data Warehouse. U leert ook de basisbeginselen voor schalen, onderbreken en afstemmen.
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
ms.custom: quickstart
ms.date: 01/26/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: 39efa954fa1eb3d7d93dbeceac48b96d865349ab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-sql-data-warehouse"></a>Aan de slag met SQL Data Warehouse

In deze zelfstudie wordt beschreven hoe u gegevens kunt inrichten en laden in Azure SQL Data Warehouse. U leert ook de basisbeginselen voor schalen, onderbreken en afstemmen. Na deze zelfstudie bent u er klaar voor om uw datawarehouse te verkennen en een query voor dit datawarehouse uit te voeren.

**Geschatte tijd om dit te voltooien:** dit is een end-to-end-zelfstudie met voorbeeldcode. Als aan de vereisten wordt voldaan, duurt het ongeveer 30 minuten om deze te voltooien. 

## <a name="prerequisites"></a>Vereisten

Aangenomen wordt dat u bekend bent met de basisbeginselen van SQL Data Warehouse. Zie [Wat is SQL Data Warehouse?](sql-data-warehouse-overview-what-is.md) als u eerst wat meer informatie nodig hebt. 

### <a name="sign-up-for-microsoft-azure"></a>Registreren voor Microsoft Azure
Als u nog geen Microsoft Azure-account hebt, moet u zich registreren voordat u deze service kunt gebruiken. Deze stap kunt u overslaan als u al een account hebt. 

1. Ga naar de accountpagina [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/)
2. Maak een gratis Azure-account of koop een account.
3. Volg de instructies

### <a name="install-appropriate-sql-client-drivers-and-tools"></a>Toepasselijke stuurprogramma's en hulpprogramma's installeren voor SQL-client

De meeste SQL-clienthulpprogramma's kunnen verbinding maken met SQL Data Warehouse met behulp van JDBC, ODBC of ADO.NET. Vanwege het grote aantal T-SQL-functies waarvoor SQL Data Warehouse ondersteuning biedt, zijn sommige clienttoepassingen niet volledig compatibel met SQL Data Warehouse.

Als u een Windows-besturingssysteem gebruikt, raden we u aan te kiezen voor [Visual Studio] of [SQL Server Management Studio].

[!INCLUDE [Create a new logical server](../../includes/sql-data-warehouse-create-logical-server.md)] 

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="create-a-sql-data-warehouse"></a>Een SQL Data Warehouse maken

SQL Data Warehouse is een speciaal databasetype dat is ontworpen voor parallelle verwerking op zeer grote schaal. De database wordt gedistribueerd over meerdere knooppunten en query's worden parallel verwerkt. SQL Data Warehouse heeft een besturingselementknooppunt waarmee de activiteiten van alle knooppunten worden ingedeeld. De knooppunten zelf maken gebruik van SQL Database om uw gegevens te beheren.  

> [!NOTE]
> Het maken van een SQL Data Warehouse kan een nieuwe factureerbare service tot gevolg hebben.  Zie [Prijzen van SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/) voor meer informatie.
>

### <a name="create-a-data-warehouse"></a>Een datawarehouse maken

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).
2. Klik op **Nieuw** > **Databases** > **SQL Data Warehouse**.

    ![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png) ![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)

3. Vul de implementatiegegevens in

    **Databasenaam**: kies een naam. Als u meerdere datawarehouses hebt, raden we u aan details, zoals de regio en omgeving, op te nemen in de namen, bijvoorbeeld *mydw-westus-1-test*.

    **Abonnement:** uw Azure-abonnement

    **Resourcegroep**: maak een resourcegroep of gebruik een bestaande resourcegroep.
    > [!NOTE]
    > Resourcegroepen zijn handig voor het beheren van resources, waaronder het bereik van toegangsbeheer controleren en het implementeren met sjablonen. [Hier](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) vindt u meer informatie over resourcegroepen en aanbevolen procedures voor Azure

    **Bron**: lege database

    **Server**: selecteer de server die u hebt gemaakt in [Vereisten].

    **Sortering**: laat de standaardsortering SQL_Latin1_General_CP1_CI_AS staan.

    **Prestaties selecteren**: we raden aan te beginnen met de standaardinstelling 400 DWU.

4. Kies **Vastmaken aan dashboard** ![Vastmaken aan dashboard](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png)

5. U hoeft niets te doen. Het datawarehouse wordt automatisch geïmplementeerd. Dit proces duurt normaal gesproken enkele minuten. U ontvangt een waarschuwing via de portal als het datawarehouse klaar is voor gebruik. 

## <a name="connect-to-sql-data-warehouse"></a>Verbinding maken met SQL Data Warehouse

In deze zelfstudie leert u SSMS (SQL Server Management Studio) te gebruiken om verbinding te maken met het datawarehouse. U kunt verbinding maken met SQL Data Warehouse via deze ondersteunde connectoren: ADO.NET, JDBC, ODBC en PHP. Let op: de functionaliteit van hulpprogramma's die niet door Microsoft worden ondersteund, is mogelijk beperkt.


### <a name="get-connection-information"></a>Verbindingsgegevens ophalen

Als u verbinding wilt maken met het datawarehouse, moet u eerst verbinding maken via de logische SQL-server die u hebt gemaakt in [Vereisten].

1. Selecteer het datawarehouse op het dashboard of zoek het in de resources.

    ![Dashboard van SQL Data Warehouse](./media/sql-data-warehouse-get-started-tutorial/sql-dw-dashboard.png)

2. Ga naar de volledige naam voor de logische server.

    ![Servernaam selecteren](./media/sql-data-warehouse-get-started-tutorial/select-server.png)

3. Open SSMS en gebruik de objectverkenner om verbinding te maken met deze server met behulp van de referenties voor de serverbeheerder die u hebt gemaakt in [Vereisten]

    ![Verbinden met SSMS](./media/sql-data-warehouse-get-started-tutorial/ssms-connect.png)

Als alles goed gaat, hebt u nu verbinding met de logische SQL-server. U bent aangemeld als serverbeheerder en kunt daarom verbinding maken met elke database die wordt gehost op de server, inclusief de hoofddatabase. 

Er is slechts één serverbeheerdersaccount. Dit account heeft van alle gebruikers de meeste bevoegdheden. Zorg ervoor dat niet te veel gebruikers in de organisatie het beheerderswachtwoord kennen. 

U kunt ook een Azure Active Directory-beheerdersaccount hebben. Hier gaan we verder niet op in. Zie [Azure AD-verificatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) voor meer informatie over het gebruik van Azure Active Directory-verificatie.

Hierna bespreken we het maken van extra aanmeldingen en gebruikers.


## <a name="create-a-database-user"></a>Een databasegebruiker maken

In deze stap maakt u een gebruikersaccount voor toegang tot het datawarehouse. We laten ook zien hoe u deze gebruiker de mogelijkheid kunt geven om query's uit te voeren met zeer veel geheugen en CPU-resources.

### <a name="notes-about-resource-classes-for-allocating-resources-to-queries"></a>Opmerkingen over resourceklassen voor het toewijzen van resources aan query's

- Als u uw gegevens wilt beveiligen, kunt u beter niet het serverbeheerdersaccount gebruiken om query's uit te voeren voor de productiedatabases. Dit account heeft de meeste bevoegdheden van alle gebruikers. Als u het gebruikt om bewerkingen uit te voeren op gebruikersgegevens, lopen uw gegevens mogelijk risico. Aangezien het serverbeheerdersaccount is bedoeld om beheerbewerkingen uit te voeren, wordt voor de uitvoering van deze bewerkingen bovendien slechts een kleine hoeveelheid geheugen en CPU-resources toegewezen. 

- SQL Data Warehouse maakt gebruik van vooraf gedefinieerde databaserollen, 'resourceklassen' genaamd, om verschillende hoeveelheden geheugen, CPU-resources en gelijktijdigheidssleuven toe te wijzen aan gebruikers. Elke gebruiker kan deel uitmaken van een kleine, gemiddelde, grote of extra grote resourceklasse. De resourceklasse van de gebruiker bepaalt de resources die de gebruiker heeft om query's uit te voeren en bewerkingen te laden.

- Voor een optimale compressie van gegevens moet de gebruiker bewerkingen meestal laden met grote of extra grote resourcetoewijzingen. Meer informatie over resourceklassen vindt u [hier](./sql-data-warehouse-develop-concurrency.md#resource-classes):

### <a name="create-an-account-that-can-control-a-database"></a>Een account maken waarmee een database kan worden beheerd

Omdat u momenteel bent aangemeld als serverbeheerder, bent u gemachtigd om aanmeldingen en gebruikers te maken.

1. Open een nieuwe query voor **Hoofd** met behulp van SSMS of een andere queryclient.

    ![Nieuwe query in Hoofd](./media/sql-data-warehouse-get-started-tutorial/query-on-server.png)

    ![Nieuwe query in Hoofd1](./media/sql-data-warehouse-get-started-tutorial/query-on-master.png)

2. Voer in het queryvenster deze T-SQL-opdracht uit om een aanmelding te maken met de naam MedRCLogin en een gebruiker met de naam Gebruiker laden. Deze aanmelding kan verbinding maken met de logische SQL-server.

    ```sql
    CREATE LOGIN MedRCLogin WITH PASSWORD = 'a123reallySTRONGpassword!';
    ```

3. Tijdens het uitvoeren van een query voor de *SQL Data Warehouse-database* maakt u nu een databasegebruiker op basis van de aanmelding die u hebt gemaakt, om toegang te krijgen tot bewerkingen en deze uit te voeren voor de database.

    ```sql
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

4. Verleen de databasegebruiker machtigingen voor de database met de naam NYT. 

    ```sql
    GRANT CONTROL ON DATABASE::[NYT] to LoadingUser;
    ```
    > [!NOTE]
    > Als de databasenaam afbreekstreepjes bevat, omsluit u deze met haken. 
    >

### <a name="give-the-user-medium-resource-allocations"></a>De gebruiker middelgrote resourcetoewijzingen geven

1. Voer deze T-SQL-opdracht uit om de gebruiker lid te maken van de middelgrote resourceklasse met de naam mediumrc. 

    ```sql
    EXEC sp_addrolemember 'mediumrc', 'LoadingUser';
    ```
    > [!NOTE]
    > Klik [hier](sql-data-warehouse-develop-concurrency.md#resource-classes) voor meer informatie over gelijktijdigheid- en resourceklassen. 
    >

2. Via de nieuwe referenties verbinding maken met de logische server

    ![Aanmelden met nieuwe aanmeldgegevens](./media/sql-data-warehouse-get-started-tutorial/new-login.png)


## <a name="load-data-from-azure-blob-storage"></a>Gegevens laden vanuit Azure-blobopslag

U bent nu klaar om gegevens te laden in het datawarehouse. In deze stap wordt beschreven hoe u gegevens over taxi's in New York kunt laden uit een openbare Azure Storage-blob. 

- Een veelgebruikte manier om gegevens te laden in SQL Data Warehouse is om de gegevens eerst te verplaatsen naar Azure-blobopslag en ze vervolgens in het datawarehouse te laden. Er worden al taxigegevens over New York gehost in de openbare Azure Storage-blob, zodat u gemakkelijker kunt zien hoe u moet laden. 

- Raadpleeg het [laadoverzicht](sql-data-warehouse-overview-load.md) voor informatie over het overbrengen van gegevens naar Azure-blobopslag of het rechtstreeks vanuit de bron laden van gegevens in SQL Data Warehouse.


### <a name="define-external-data"></a>Externe gegevens definiëren

1. Maak een hoofdsleutel. U hoeft maar één hoofdsleutel per database te maken. 

    ```sql
    CREATE MASTER KEY;
    ```

2. Definieer de locatie van de Azure-blob die de taxigegevens bevat.  

    ```sql
    CREATE EXTERNAL DATA SOURCE NYTPublic
    WITH
    (
        TYPE = Hadoop,
        LOCATION = 'wasbs://2013@nytpublic.blob.core.windows.net/'
    );
    ```

3. De externe bestandsindelingen definiëren

    De ```CREATE EXTERNAL FILE FORMAT```-opdracht wordt gebruikt om de indeling op te geven van de bestanden die de externe gegevens bevatten. Ze bevatten tekst gescheiden door een of meer tekens, genaamd scheidingstekens. Ter illustratie worden de taxigegevens opgeslagen als niet-gecomprimeerde gegevens en als gecomprimeerde GZip-gegevens.

    Voer deze T-SQL-opdrachten uit om twee verschillende indelingen te definiëren: niet-gecomprimeerd en gecomprimeerd.

    ```sql
    CREATE EXTERNAL FILE FORMAT uncompressedcsv
    WITH (
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( 
            FIELD_TERMINATOR = ',',
            STRING_DELIMITER = '',
            DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        )
    );

    CREATE EXTERNAL FILE FORMAT compressedcsv
    WITH ( 
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( FIELD_TERMINATOR = '|',
            STRING_DELIMITER = '',
        DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        ),
        DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
    );
    ```

4.  Maak een schema voor de externe bestandsindeling. 

    ```sql
    CREATE SCHEMA ext;
    ```
5. Maak de externe tabellen. Deze tabellen verwijzen naar gegevens die zijn opgeslagen in Azure-blobopslag. Voer de volgende T-SQL-opdrachten uit om verschillende externe tabellen te maken die allemaal verwijzen naar de Azure-blob die eerder is gedefinieerd in de externe gegevensbron.

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
        LOCATION = 'Date',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    );
    
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
        LOCATION = 'Geography',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0 
    );
        
    
    CREATE EXTERNAL TABLE [ext].[HackneyLicense]
    (
        [HackneyLicenseID] int NOT NULL,
        [HackneyLicenseBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HackneyLicenseCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'HackneyLicense',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
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
        LOCATION = 'Medallion',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
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
        LOCATION = 'Time',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
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
        LOCATION = 'Trip2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = compressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
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
        LOCATION = 'Weather2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
```

### <a name="import-the-data-from-azure-blob-storage"></a>Importeer de gegevens vanuit Azure-blobopslag.

SQL Data Warehouse ondersteunt een sleutelinstructie met de naam CREATE TABLE AS SELECT (CTAS). Deze instructie maakt een nieuwe tabel op basis van de resultaten van een selecteerinstructie. De nieuwe tabel heeft dezelfde gegevenstypen en kolommen als de resultaten van de selecteerinstructie.  Dit is een elegante manier om gegevens te importeren uit Azure-blobopslag naar SQL Data Warehouse.

1. Voer dit script uit om uw gegevens te importeren.

    ```sql
    CREATE TABLE [dbo].[Date]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Date]
    OPTION (LABEL = 'CTAS : Load [dbo].[Date]')
    ;
    
    CREATE TABLE [dbo].[Geography]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT * FROM [ext].[Geography]
    OPTION (LABEL = 'CTAS : Load [dbo].[Geography]')
    ;
    
    CREATE TABLE [dbo].[HackneyLicense]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[HackneyLicense]
    OPTION (LABEL = 'CTAS : Load [dbo].[HackneyLicense]')
    ;
    
    CREATE TABLE [dbo].[Medallion]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Medallion]
    OPTION (LABEL = 'CTAS : Load [dbo].[Medallion]')
    ;
    
    CREATE TABLE [dbo].[Time]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Time]
    OPTION (LABEL = 'CTAS : Load [dbo].[Time]')
    ;
    
    CREATE TABLE [dbo].[Weather]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Weather]
    OPTION (LABEL = 'CTAS : Load [dbo].[Weather]')
    ;
    
    CREATE TABLE [dbo].[Trip]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Trip]
    OPTION (LABEL = 'CTAS : Load [dbo].[Trip]')
    ;
    ```

2. Bekijk uw gegevens tijdens het laden.

   U laadt meerdere GB's aan gegevens en comprimeert die tot hoogwaardige geclusterde columnstore-indexen. Voer de volgende query uit, die gebruikmaakt van dynamische beheerweergaven (DMV's) om de status van de belasting weer te geven. Pak er een kopje koffie bij nadat u de query hebt gestart. SQL Data Warehouse doet ondertussen het zware werk voor u.
    
    ```sql
    SELECT
        r.command,
        s.request_id,
        r.status,
        count(distinct input_name) as nbr_files,
        sum(s.bytes_processed)/1024/1024/1024 as gb_processed
    FROM 
        sys.dm_pdw_exec_requests r
        INNER JOIN sys.dm_pdw_dms_external_work s
        ON r.request_id = s.request_id
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
        nbr_files desc, 
        gb_processed desc;
    ```

3. Bekijk alle systeemquery's.

    ```sql
    SELECT * FROM sys.dm_pdw_exec_requests;
    ```

4. Al uw gegevens zijn netjes geladen in uw Azure SQL Data Warehouse.

    ![Geladen gegevens bekijken](./media/sql-data-warehouse-get-started-tutorial/see-data-loaded.png)


## <a name="improve-query-performance"></a>Queryprestaties verbeteren

Er zijn verschillende manieren om de queryprestaties te verbeteren en om de snelle prestaties te bereiken waarvoor SQL Data Warehouse is ontworpen.  

### <a name="see-the-effect-of-scaling-on-query-performance"></a>Het effect van schalen op de queryprestaties bekijken 

U kunt de queryprestaties verbeteren door resources te schalen. Dit doet u door het DWU-serviceniveau voor uw datawarehouse te wijzigen. Elk serviceniveau kost meer, maar u kunt op elk gewenst moment terugschalen of resources onderbreken. 

In deze stap vergelijkt u de prestaties bij twee verschillende DWU-instellingen.

Eerst gaan we de bewerking omlaag schalen naar 100 DWU, zodat we een idee krijgen van hoe een rekenknooppunt afzonderlijk presteert.

1. Ga naar de portal en selecteer uw SQL Data Warehouse.

2. Selecteer de schaal in de blade SQL Data Warehouse. 

    ![DW schalen vanuit de portal](./media/sql-data-warehouse-get-started-tutorial/scale-dw.png)

3. Schaal de prestatiebalk omlaag naar 100 DWU en klik op opslaan.

    ![Schalen en opslaan](./media/sql-data-warehouse-get-started-tutorial/scale-and-save.png)

4. Wacht totdat de schaalbewerking is voltooid.

    > [!NOTE]
    > Query's kunnen niet worden uitgevoerd tijdens het wijzigen van de schaal. Het schalen **beëindigt** uw huidige actieve query's. U kunt ze opnieuw starten wanneer de bewerking is voltooid.
    >
    
5. Voer een scanbewerking uit op de reisgegevens, waarbij u het eerste miljoen vermeldingen voor alle kolommen selecteert. Als u snel door wilt gaan, kunt u gerust minder rijen selecteren. Noteer de tijd die nodig is voor het uitvoeren van deze bewerking.

    ```sql
    SELECT TOP(1000000) * FROM dbo.[Trip]
    ```
6. Schaal uw datawarehouse terug naar 400 DWU. Met elke 100 DWU wordt een extra rekenknooppunt aan uw Azure SQL Data Warehouse toegevoegd.

7. Voer de opnieuw query uit. U zou een duidelijk verschil moeten zien. 

    > [!NOTE]
    > Omdat de query een grote hoeveelheid gegevens retourneert, kan de beschikbare bandbreedte van de computer waarop SSMS wordt uitgevoerd een knelpunt zijn. Dit kan tot gevolg hebben dat er helemaal geen sprake is van prestatieverbetering!

> [!NOTE]
> SQL Data Warehouse gebruikt MPP (Massively Parallel Processing). Query's die scannen of analytische functies uitvoeren op miljoenen rijen, ervaren de ware kracht van Azure SQL Data Warehouse.
>

### <a name="see-the-effect-of-statistics-on-query-performance"></a>Het effect van statistieken op de queryprestaties bekijken

1. Een query uitvoeren die de tabel Date met de tabel Trip samenvoegt

    ```sql
    SELECT TOP (1000000) 
        dt.[DayOfWeek],
        tr.[MedallionID],
        tr.[HackneyLicenseID],
        tr.[PickupTimeID],
        tr.[DropoffTimeID],
        tr.[PickupGeographyID],
        tr.[DropoffGeographyID],
        tr.[PickupLatitude],
        tr.[PickupLongitude],
        tr.[PickupLatLong],
        tr.[DropoffLatitude],
        tr.[DropoffLongitude],
        tr.[DropoffLatLong],
        tr.[PassengerCount],
        tr.[TripDurationSeconds],
        tr.[TripDistanceMiles],
        tr.[PaymentType],
        tr.[FareAmount],
        tr.[SurchargeAmount],
        tr.[TaxAmount],
        tr.[TipAmount],
        tr.[TollsAmount],
        tr.[TotalAmount]
    FROM [dbo].[Trip] as tr
        JOIN dbo.[Date] as dt
        ON  tr.DateID = dt.DateID
    ```

    Deze query kost extra tijd, omdat SQL Data Warehouse gegevens moet verdelen voordat er kan worden samengevoegd. Er hoeven geen gegevens te worden verdeeld bij samenvoegingen als de gegevens moeten worden samengevoegd op dezelfde manier als ze zijn gedistribueerd. Dat is een ingewikkelder onderwerp. 

2. Statistieken maken een groot verschil. 
3. Voer deze instructie uit om statistieken te maken voor de samenvoegingskolommen.

    ```sql
    CREATE STATISTICS [dbo.Date DateID stats] ON dbo.Date (DateID);
    CREATE STATISTICS [dbo.Trip DateID stats] ON dbo.Trip (DateID);
    ```

    > [!NOTE]
    > SQL DW beheert niet automatisch statistieken voor u. Statistieken zijn belangrijk voor de prestaties van query's en we raden u ten zeerste aan statistieken te maken en bij te werken.
    > 
    > **U haalt het meeste voordeel uit statistieken bij kolommen die onderdeel uitmaken van samenvoegingen, kolommen met het WHERE-component en kolommen in GROUP BY.**
    >

3. Voer de query in Vereisten opnieuw uit en kijk of er prestatieverschillen zijn. De verschillen in de prestaties van query's zijn niet zo ingrijpend als wanneer u omhoog schaalt, maar u zou wel een versnelling moeten zien. 

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om query’s uit te voeren en te verkennen. Bekijk onze aanbevolen procedures en tips.

Als u klaar bent voor vandaag, zorg dan dat u uw exemplaar onderbreekt. U kunt enorm besparen op de productie door te onderbreken en schalen op basis van uw zakelijke behoeften.

![Onderbreken](./media/sql-data-warehouse-get-started-tutorial/pause.png)

## <a name="useful-readings"></a>Handige documentatie

[Gelijktijdigheid en werklastbeheer][]

[Aanbevolen procedures voor Azure SQL Data Warehouse][]

[Querybewaking][]

[Top 10 aanbevolen procedures voor het bouwen van een grootschalige relationele Data Warehouse][]

[Gegevens migreren naar Azure SQL Data Warehouse][]

[Gelijktijdigheid en werklastbeheer]: sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example
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
