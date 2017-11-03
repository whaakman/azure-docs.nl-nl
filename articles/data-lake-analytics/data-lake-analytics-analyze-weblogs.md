---
title: Websitelogboeken analyseren met Azure Data Lake Analytics | Microsoft Docs
description: 'Informatie over het websitelogboeken analyseren met Data Lake Analytics. '
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 3a196735-d0d9-4deb-ba68-c4b3f3be8403
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: saveenr
ms.openlocfilehash: 52d19297ae5c34f9daf5e42250a53a78e0168192
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="analyze-website-logs-using-azure-data-lake-analytics"></a>Websitelogboeken analyseren met Azure Data Lake Analytics
Informatie over het websitelogboeken analyseren met Data Lake Analytics, met name op fouten die verwijzingen is gedetecteerd tijdens het naar de website te controleren.

## <a name="prerequisites"></a>Vereisten
* **Visual Studio 2015 of Visual Studio 2013**.
* **[Data Lake Tools voor Visual Studio](http://aka.ms/adltoolsvs)**.

    Wanneer Data Lake Tools voor Visual Studio is geïnstalleerd, ziet u een **Data Lake** item in de **extra** menu in Visual Studio:

    ![U-SQL Visual Studio-menu](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)
* **Basiskennis van Data Lake Analytics en Data Lake Tools voor Visual Studio**. Zie het volgende om te beginnen:

  * [U-SQL-script met Data Lake tools voor Visual Studio ontwikkelen](data-lake-analytics-data-lake-tools-get-started.md).
* **Een Data Lake Analytics-account.**  Zie [een Azure Data Lake Analytics-account maken](data-lake-analytics-get-started-portal.md).
* **Installeer de voorbeeldgegevens.** In de Azure-Portal, opent u Data Lake Analytics-account en klikt u op **voorbeeldscripts** Klik in het menu links **Copy Sample Data**. 

## <a name="connect-to-azure"></a>Verbinding maken met Azure
Voordat u kunt bouwen en testen van de U-SQL-scripts, moet u eerst verbinding met Azure.

**Verbinding maken met Data Lake Analytics**

1. Open Visual Studio.
2. Klik op **Data Lake > Opties en instellingen**.
3. Klik op **aanmelden**, of **gebruiker wijzigen** als iemand anders heeft aangemeld en volg de instructies.
4. Klik op **OK** om het dialoogvenster Opties en instellingen te sluiten.

**Om te bladeren van uw Data Lake Analytics-accounts**

1. Open in Visual Studio **Server Explorer** door press **CTRL + ALT + S**.
2. Vouw in **Server Explorer** achtereenvolgens **Azure** en **Data Lake Analytics** uit. U ziet een lijst met uw Data Lake Analytics-accounts, als u die hebt. U kunt vanuit de studio Data Lake Analytics-accounts maken. Zie [Aan de slag met Azure Data Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md) of [Aan de slag met Azure Data Lake Analytics met Azure PowerShell](data-lake-analytics-get-started-powershell.md) voor meer informatie over het maken van een account.

## <a name="develop-u-sql-application"></a>U-SQL-toepassing ontwikkelen
Een U-SQL-toepassing is meestal een U-SQL-script. Zie voor meer informatie over U-SQL, [aan de slag met U-SQL](data-lake-analytics-u-sql-get-started.md).

U kunt de gebruiker gedefinieerde operators toevoeging toevoegen aan de toepassing.  Zie voor meer informatie [ontwikkelen van U-SQL door de gebruiker gedefinieerde operators voor Data Lake Analytics-taken](data-lake-analytics-u-sql-develop-user-defined-operators.md).

**Een Data Lake Analytics-taak maken en verzenden**

1. Klik op de **bestand > Nieuw > Project**.
2. Selecteer het type Project U-SQL.

    ![nieuw U-SQL Visual Studio-project](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. Klik op **OK**. Visual studio maakt een oplossing met een bestand Script.usql.
4. Voer het volgende script in het bestand Script.usql:

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            DISTRIBUTED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    Zie voor informatie over de U-SQL, [aan de slag met Data Lake Analytics U-SQL-taal](data-lake-analytics-u-sql-get-started.md).    
5. Een nieuw U-SQL-script toevoegen aan uw project en voer de volgende gegevens:

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();
6. Schakel terug naar het eerste U-SQL-script en vervolgens naar de **indienen** knop, geeft u uw Analytics-account.
7. Ga naar **Solution Explorer**, klik met de rechtermuisknop op **Script.usql** en klik vervolgens op **Build Script**. Controleer de resultaten in het deelvenster Uitvoer.
8. Ga naar **Solution Explorer**, klik met de rechtermuisknop op **Script.usql** en klik vervolgens op **Submit Script**.
9. Controleer of de **Analytics-Account** is waar u de taak uitvoeren en klik vervolgens op **indienen**. Het resultaat van het verzenden en de koppeling naar de taak zijn beschikbaar in het resultaatvenster van Data Lake Tools voor Visual Studio wanneer het verzenden is voltooid.
10. Wacht totdat de taak is voltooid.  Als de taak is mislukt, waarschijnlijk ontbreken het bronbestand.  Zie de sectie vereisten van deze zelfstudie. Zie voor aanvullende informatie voor probleemoplossing [Monitor en Azure Data Lake Analytics-taken oplossen](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    Als de taak is voltooid, ziet u het volgende scherm:

    ![Data lake analytics analyseren weblogboeken websitelogboeken](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)
11. Herhaal stap 7 - 10 voor **Script1.usql**.

**De taakuitvoer weergeven**

1. Ga naar **Server Explorer**, vouw **Azure** uit, vouw **Data Lake Analytics** uit, vouw uw Data Lake Analytics-account uit, vouw **Storage Accounts** uit, klik met de rechtermuisknop op het Data Lake Storage-standaardaccount en klik vervolgens op **Explorer**.
2. Dubbelklik op **voorbeelden** open de map en dubbelklikt u vervolgens op **uitvoer**.
3. Dubbelklik op **UnsuccessfulResponsees.log**.
4. U kunt ook het uitvoerbestand binnen de grafiekweergave van de taak dubbelklikken om gaat u rechtstreeks naar de uitvoer.

## <a name="see-also"></a>Zie ook
Om aan de slag te gaan met Data Lake Analytics met verschillende hulpprogramma's, zie:

* [Aan de slag met Data Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md)
* [Aan de slag met Data Lake Analytics met Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Aan de slag met Data Lake Analytics met .NET SDK](data-lake-analytics-get-started-net-sdk.md)
