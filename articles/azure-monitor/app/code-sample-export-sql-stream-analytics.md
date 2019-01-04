---
title: Exporteren naar SQL van Azure Application Insights | Microsoft Docs
description: Continu Application Insights-gegevens exporteren naar SQL met Stream Analytics.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 48903032-2c99-4987-9948-d6e4559b4a63
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: mbullwin
ms.openlocfilehash: 8ed645066eef93ae02722858381af77854d7d1eb
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "54001875"
---
# <a name="walkthrough-export-to-sql-from-application-insights-using-stream-analytics"></a>Overzicht: Exporteren naar SQL van Application Insights met behulp van Stream Analytics
Dit artikel wordt beschreven hoe u uw telemetriegegevens van verplaatst [Azure Application Insights] [ start] in een Azure SQL-database met behulp van [continue Export] [ export] en [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). 

Continue export worden de telemetriegegevens verplaatst naar Azure Storage in JSON-indeling. We parseren van de JSON-objecten met behulp van Azure Stream Analytics en rijen in een databasetabel maken.

(Meer over het algemeen continue Export is de manier om uw eigen analyse van de telemetrie die uw apps naar Application Insights verzenden. U kunt dit codevoorbeeld hiervoor andere dingen met de geëxporteerde telemetriegegevens, zoals aggregatie van gegevens worden aanpassen.)

We beginnen met de veronderstelling dat u al hebt de app die u wilt bewaken.

In dit voorbeeld gebruiken we de gegevens van de pagina weergeven, maar hetzelfde patroon kan eenvoudig worden uitgebreid naar andere gegevenstypen, zoals aangepaste gebeurtenissen en uitzonderingen. 

## <a name="add-application-insights-to-your-application"></a>Application Insights toevoegen aan uw toepassing
Aan de slag gaan:

1. [Application Insights instellen voor uw webpagina's](../../azure-monitor/app/javascript.md). 
   
    (In dit voorbeeld, ligt de focus op het verwerken van gegevens van de pagina weergeven in de clientbrowsers, maar u kunt Application Insights instellen voor de serverkant van uw [Java](../../azure-monitor/app/java-get-started.md) of [ASP.NET](../../azure-monitor/app/asp-net.md) app en de proces-aanvraag afhankelijkheid en andere servertelemetrie.)
2. Uw app publiceren en bekijk de telemetrische gegevens verschijnen in uw Application Insights-resource.

## <a name="create-storage-in-azure"></a>Opslag maken in Azure
Continue export voert altijd gegevens uit naar een Azure Storage-account, dus moet u eerst de opslag maken.

1. Een storage-account maken in uw abonnement in de [Azure-portal][portal].
   
    ![Kies nieuw, gegevens, opslag in Azure portal. Selecteer klassiek, kiest u maken. Geef een opslagnaam.](./media/code-sample-export-sql-stream-analytics/040-store.png)
2. Een container maken
   
    ![In de nieuwe opslag Containers selecteren, klikt u op de Containers tegel en klik vervolgens op toevoegen](./media/code-sample-export-sql-stream-analytics/050-container.png)
3. De toegangssleutel voor opslag kopiëren
   
    U moet deze beschikbaar voor het instellen van de invoer voor de stream analytics-service.
   
    ![In de opslag, open instellingen, sleutels, en een kopie van de primaire toegangssleutel](./media/code-sample-export-sql-stream-analytics/21-storage-key.png)

## <a name="start-continuous-export-to-azure-storage"></a>Start de continue export naar Azure storage
1. In de Azure-portal, blader naar de Application Insights-resource die u hebt gemaakt voor uw toepassing.
   
    ![Kies Bladeren, Application Insights, uw toepassing](./media/code-sample-export-sql-stream-analytics/060-browse.png)
2. Maak een continue export.
   
    ![Kies instellingen, continue Export toevoegen](./media/code-sample-export-sql-stream-analytics/070-export.png)

    Selecteer het opslagaccount dat u eerder hebt gemaakt:

    ![Stel de doelserver exporteren](./media/code-sample-export-sql-stream-analytics/080-add.png)

    Stel de typen gebeurtenissen die u wilt zien:

    ![Kies gebeurtenistypen](./media/code-sample-export-sql-stream-analytics/085-types.png)


1. Sommige gegevens worden verzameld, kunnen. Begint en laat mensen uw toepassing een tijdje gebruiken. Telemetrie komt en ziet u statistische grafieken in [metric explorer](../../azure-monitor/app/metrics-explorer.md) en afzonderlijke gebeurtenissen in [diagnostische gegevens doorzoeken](../../azure-monitor/app/diagnostic-search.md). 
   
    En ook de gegevens worden geëxporteerd naar uw opslag. 
2. Inspecteer de geëxporteerde gegevens in de portal - kiezen **Bladeren**, selecteer uw opslagaccount, en vervolgens **Containers** - of in Visual Studio. Kies in Visual Studio **weergeven / Cloud Explorer**, en open Azure / Storage. (Als u deze optie niet hebt, moet u de Azure SDK installeren: Open het dialoogvenster Nieuw Project en Visual C# / Cloud / Microsoft Azure SDK voor .NET ophalen.)
   
    ![Open in Visual Studio Server Browser, Azure, opslag](./media/code-sample-export-sql-stream-analytics/087-explorer.png)
   
    Maak een notitie van het algemene gedeelte van de naam van het pad, die is afgeleid van de sleutel van de naam en -instrumentatie in de toepassing. 

De gebeurtenissen worden geschreven naar de blob-bestanden in de JSON-indeling. Elk bestand kan een of meer gebeurtenissen bevatten. Dus graag we lezen van gegevens van de gebeurtenis en de velden die we willen filteren. Er zijn allerlei dingen die we met de gegevens kan doen, maar ons abonnement is vandaag nog aan Stream Analytics gebruiken voor het verplaatsen van de gegevens naar een SQL-database. Dit maakt het eenvoudig uitvoeren van tal van interessante query's.

## <a name="create-an-azure-sql-database"></a>Maak een Azure SQL Database
Opnieuw starten van uw abonnement in [Azure-portal][portal], de database maken (en een nieuwe server, tenzij u al een hebt) op die u de gegevens schrijft.

![Nieuw, gegevens, SQL](./media/code-sample-export-sql-stream-analytics/090-sql.png)

Zorg ervoor dat de database-server toegang tot Azure-services geeft:

![Bladeren, Servers, uw server, instellingen, Firewall, toegang geven tot Azure](./media/code-sample-export-sql-stream-analytics/100-sqlaccess.png)

## <a name="create-a-table-in-azure-sql-db"></a>Een tabel maken in Azure SQL DB
Verbinding maken met de database hebt gemaakt in de vorige sectie met uw favoriete beheerprogramma. In dit scenario gebruiken we [SQL Server Management Tools](https://msdn.microsoft.com/ms174173.aspx) (SSMS).

![](./media/code-sample-export-sql-stream-analytics/31-sql-table.png)

Een nieuwe query maken en uitvoeren van de volgende T-SQL:

```SQL

CREATE TABLE [dbo].[PageViewsTable](
    [pageName] [nvarchar](max) NOT NULL,
    [viewCount] [int] NOT NULL,
    [url] [nvarchar](max) NULL,
    [urlDataPort] [int] NULL,
    [urlDataprotocol] [nvarchar](50) NULL,
    [urlDataHost] [nvarchar](50) NULL,
    [urlDataBase] [nvarchar](50) NULL,
    [urlDataHashTag] [nvarchar](max) NULL,
    [eventTime] [datetime] NOT NULL,
    [isSynthetic] [nvarchar](50) NULL,
    [deviceId] [nvarchar](50) NULL,
    [deviceType] [nvarchar](50) NULL,
    [os] [nvarchar](50) NULL,
    [osVersion] [nvarchar](50) NULL,
    [locale] [nvarchar](50) NULL,
    [userAgent] [nvarchar](max) NULL,
    [browser] [nvarchar](50) NULL,
    [browserVersion] [nvarchar](50) NULL,
    [screenResolution] [nvarchar](50) NULL,
    [sessionId] [nvarchar](max) NULL,
    [sessionIsFirst] [nvarchar](50) NULL,
    [clientIp] [nvarchar](50) NULL,
    [continent] [nvarchar](50) NULL,
    [country] [nvarchar](50) NULL,
    [province] [nvarchar](50) NULL,
    [city] [nvarchar](50) NULL
)

CREATE CLUSTERED INDEX [pvTblIdx] ON [dbo].[PageViewsTable]
(
    [eventTime] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON)

```

![](./media/code-sample-export-sql-stream-analytics/34-create-table.png)

In dit voorbeeld gebruiken we gegevens uit de paginaweergaven. Als u wilt zien van de andere beschikbare gegevens, uw JSON-uitvoer controleren en zien het [gegevensmodel exporteren](../../azure-monitor/app/export-data-model.md).

## <a name="create-an-azure-stream-analytics-instance"></a>Azure Stream Analytics-exemplaar maken
Uit de [Azure-portal](https://portal.azure.com/), selecteert u de Azure Stream Analytics-service en een nieuwe Stream Analytics-taak te maken:

![Instellingen voor Stream analytics](./media/code-sample-export-sql-stream-analytics/SA001.png)

![](./media/code-sample-export-sql-stream-analytics/SA002.png)

Wanneer de nieuwe taak is gemaakt, selecteert u **naar de resource gaan**.

![Instellingen voor Stream analytics](./media/code-sample-export-sql-stream-analytics/SA003.png)

#### <a name="add-a-new-input"></a>Een nieuwe invoer toevoegen

![Instellingen voor Stream analytics](./media/code-sample-export-sql-stream-analytics/SA004.png)

Instellen om te ondernemen invoer van de continue Export-blob:

![Instellingen voor Stream analytics](./media/code-sample-export-sql-stream-analytics/SA0005.png)

Nu moet u de primaire toegangssleutel van uw Opslagaccount, die u eerder hebt genoteerd. Stel dit in als de Opslagaccountsleutel.

#### <a name="set-path-prefix-pattern"></a>Set-voorvoegsel padpatroon

**Zorg ervoor dat de datumnotatie ingesteld op DD-MM-jjjj (met streepjes).**

Het pad naar het voorvoegsel patroon geeft aan hoe de invoerbestanden in Stream Analytics worden gevonden in de opslag. U moet deze overeenkomen met het continue Export hoe de gegevens opslaat in te stellen. Doet u het als volgt:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

In dit voorbeeld:

* `webapplication27` de naam van de Application Insights-resource **alles in kleine letters**. 
* `1234...` de instrumentatiesleutel van de Application Insights-resource is **met streepjes verwijderd**. 
* `PageViews` is het type gegevens wilt analyseren. De beschikbare typen, is afhankelijk van het filter dat u in de continue Export instellen. Bekijk de geëxporteerde gegevens om te zien van de beschikbare typen en Zie de [gegevensmodel exporteren](../../azure-monitor/app/export-data-model.md).
* `/{date}/{time}` een patroon is letterlijk geschreven.

Als u de naam en sleutel van uw Application Insights-resource, open Essentials op de overzichtspagina, of instellingen.

> [!TIP]
> De voorbeeld-functie gebruiken om te controleren of u het invoerpad juist zijn ingesteld. Als dit mislukt: Controleer of er gegevens in de opslag voor de voorbeeld-tijdsbereik dat u hebt gekozen. De definitie van de invoer bewerken en controleer u de storage-account, het padvoorvoegsel instellen en datumnotatie voor de juiste manier.
> 
> 
## <a name="set-query"></a>Set-query
Open de querysectie:

Vervang de standaardquery met:

```SQL

    SELECT flat.ArrayValue.name as pageName
    , flat.ArrayValue.count as viewCount
    , flat.ArrayValue.url as url
    , flat.ArrayValue.urlData.port as urlDataPort
    , flat.ArrayValue.urlData.protocol as urlDataprotocol
    , flat.ArrayValue.urlData.host as urlDataHost
    , flat.ArrayValue.urlData.base as urlDataBase
    , flat.ArrayValue.urlData.hashTag as urlDataHashTag
      ,A.context.data.eventTime as eventTime
      ,A.context.data.isSynthetic as isSynthetic
      ,A.context.device.id as deviceId
      ,A.context.device.type as deviceType
      ,A.context.device.os as os
      ,A.context.device.osVersion as osVersion
      ,A.context.device.locale as locale
      ,A.context.device.userAgent as userAgent
      ,A.context.device.browser as browser
      ,A.context.device.browserVersion as browserVersion
      ,A.context.device.screenResolution.value as screenResolution
      ,A.context.session.id as sessionId
      ,A.context.session.isFirst as sessionIsFirst
      ,A.context.location.clientip as clientIp
      ,A.context.location.continent as continent
      ,A.context.location.country as country
      ,A.context.location.province as province
      ,A.context.location.city as city
    INTO
      AIOutput
    FROM AIinput A
    CROSS APPLY GetElements(A.[view]) as flat


```

U ziet dat de eerste paar eigenschappen specifiek voor paginaweergavegegevens zijn. Uitvoer van de andere telemetrietypen hebben verschillende eigenschappen. Zie de [gedetailleerde naslaginformatie over gegevensmodellen voor de typen eigenschappen en waarden.](../../azure-monitor/app/export-data-model.md)

## <a name="set-up-output-to-database"></a>Uitvoer naar de database instellen
Selecteer SQL als de uitvoer.

![Selecteer in de stream analytics, uitvoer](./media/code-sample-export-sql-stream-analytics/SA006.png)

Geef de SQL-database.

![Vul de gegevens van uw database](./media/code-sample-export-sql-stream-analytics/SA007.png)

De wizard te sluiten en wachten op een melding dat de uitvoer is ingesteld.

## <a name="start-processing"></a>Verwerking starten
Start de taak in de actiebalk:

![In stream analytics, klik op Start](./media/code-sample-export-sql-stream-analytics/SA008.png)

U kunt kiezen of u wilt beginnen met het verwerken van de gegevens die vanaf nu of beginnen met oudere gegevens. De laatste is handig als u de continue Export is al uitgevoerd voor een tijdje hebt gehad.

Na een paar minuten, gaat u terug naar SQL Server-beheerhulpprogramma's en bekijk de gegevens die. Bijvoorbeeld, gebruikt u een query als volgt:

    SELECT TOP 100 *
    FROM [dbo].[PageViewsTable]


## <a name="related-articles"></a>Verwante artikelen:
* [Exporteren naar Power BI met behulp van Stream Analytics](../../application-insights/app-insights-export-power-bi.md)
* [Gedetailleerde gegevens modelleren verwijzing voor de typen eigenschappen en waarden.](../../azure-monitor/app/export-data-model.md)
* [Continue Export in Application Insights](../../azure-monitor/app/export-telemetry.md)
* [Application Insights](https://azure.microsoft.com/services/application-insights/)

<!--Link references-->

[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[export]: ../../azure-monitor/app/export-telemetry.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
[portal]: https://portal.azure.com/
[start]: ../../application-insights/app-insights-overview.md

