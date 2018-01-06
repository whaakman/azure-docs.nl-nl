---
title: Exporteren naar SQL van Azure Application Insights | Microsoft Docs
description: Continu Application Insights gegevens exporteren naar SQL met Stream Analytics.
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
editor: mrbullwinkle
ms.assetid: 48903032-2c99-4987-9948-d6e4559b4a63
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/06/2015
ms.author: mbullwin
ms.openlocfilehash: 8d008727d964df56d128265b632dafa4ab776f98
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="walkthrough-export-to-sql-from-application-insights-using-stream-analytics"></a>Overzicht: Exporteren naar SQL van Application Insights met Stream Analytics
Dit artikel laat zien hoe u verplaatst uw telemetriegegevens van [Azure Application Insights] [ start] in een Azure SQL database met behulp van [continue Export] [ export] en [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). 

Continue export verplaatst de telemetriegegevens naar Azure Storage in JSON-indeling. We parseren van de JSON-objecten met behulp van Azure Stream Analytics en rijen in een databasetabel te maken.

(Meer in het algemeen continue Export is de manier om uw eigen analyse van de telemetrie die uw apps naar Application Insights verzenden. U kunt dit codevoorbeeld hiervoor andere mogelijkheden met de geëxporteerde telemetriegegevens, zoals aggregatie van gegevens worden aanpassen.)

We beginnen met de veronderstelling dat u al hebt de app die u wilt bewaken.

We gebruiken de gegevens van de pagina in dit voorbeeld, maar dit patroon kan gemakkelijk worden uitgebreid naar andere gegevenstypen zoals aangepaste gebeurtenissen en uitzonderingen. 

## <a name="add-application-insights-to-your-application"></a>Application Insights toevoegen aan uw toepassing
Aan de slag:

1. [Application Insights instellen voor uw webpagina's](app-insights-javascript.md). 
   
    (In dit voorbeeld we ligt de nadruk op gegevens van de pagina weergeven in de clientbrowsers verwerking, maar u kunt Application Insights instellen voor de serverkant van uw [Java](app-insights-java-get-started.md) of [ASP.NET](app-insights-asp-net.md) -app en verwerking van aanvragen, afhankelijkheden en andere servertelemetrie.)
2. Uw app publiceren en bekijk de telemetrische gegevens verschijnen in uw Application Insights-resource.

## <a name="create-storage-in-azure"></a>Maken van opslag in Azure
Continue export levert altijd gegevens aan een Azure Storage-account, dus u moet de opslag om eerst te maken.

1. Een opslagaccount maken in uw abonnement in de [Azure-portal][portal].
   
    ![Kies nieuw, gegevens, opslag in Azure-portal. Klassieke selecteert, kiest u maken. Geef een opslagnaam.](./media/app-insights-code-sample-export-sql-stream-analytics/040-store.png)
2. Een container maken
   
    ![In de nieuwe opslag Containers selecteren, klikt u op de tegel Containers en toevoegen](./media/app-insights-code-sample-export-sql-stream-analytics/050-container.png)
3. De toegangssleutel voor opslag kopiëren
   
    U moet deze snel voor het instellen van de invoer van de stream analytics-service.
   
    ![Instellingen, sleutels, open in de opslag, en een kopie van de primaire toegangssleutel](./media/app-insights-code-sample-export-sql-stream-analytics/21-storage-key.png)

## <a name="start-continuous-export-to-azure-storage"></a>Start de continue export naar Azure-opslag
1. Blader naar de Application Insights-resource die u hebt gemaakt voor uw toepassing in de Azure-portal.
   
    ![Kies Bladeren, Application Insights uw toepassing](./media/app-insights-code-sample-export-sql-stream-analytics/060-browse.png)
2. Maak een continue export.
   
    ![Instellingen voor continue Export toevoegen](./media/app-insights-code-sample-export-sql-stream-analytics/070-export.png)

    Selecteer het opslagaccount dat u eerder hebt gemaakt:

    ![Stel de doelserver exporteren](./media/app-insights-code-sample-export-sql-stream-analytics/080-add.png)

    Stel de typen gebeurtenissen die u wilt zien:

    ![Gebeurtenistypen kiezen](./media/app-insights-code-sample-export-sql-stream-analytics/085-types.png)


1. Laat een aantal gegevens worden verzameld. Terug zitten en toestaan dat uw toepassing een tijdje gebruiken. Telemetrie wordt geleverd in en ziet u statistische grafieken in [metrische explorer](app-insights-metrics-explorer.md) en afzonderlijke gebeurtenissen in [diagnostische gegevens doorzoeken](app-insights-diagnostic-search.md). 
   
    En ook de gegevens worden geëxporteerd naar uw opslag. 
2. Inspecteer de geëxporteerde gegevens in de portal - kiezen **Bladeren**, selecteer uw storage-account en vervolgens **Containers** - of in Visual Studio. Kies in Visual Studio **weergeven / Cloud Explorer**, en open Azure / opslag. (Als u deze optie niet hebt, moet u de Azure SDK installeren: Open het dialoogvenster New Project en Visual C# / Cloud / ophalen van Microsoft Azure SDK voor .NET.)
   
    ![Open in Visual Studio Server Browser, Azure, opslag](./media/app-insights-code-sample-export-sql-stream-analytics/087-explorer.png)
   
    Noteer het algemene gedeelte van de padnaam die is afgeleid van de sleutel voor naam en instrumentatie van toepassing. 

De gebeurtenissen worden geschreven naar de blob-bestanden in de JSON-indeling. Elk bestand kan een of meer gebeurtenissen bevatten. Dus willen we gelezen gegevens van de gebeurtenis en de velden die we wilt filteren. Er zijn alle soorten wat die we met de gegevens doen kan, maar onze plan is vandaag de dag Stream Analytics gebruiken de gegevens worden verplaatst naar een SQL-database. Die wordt maakt het eenvoudig om uit te voeren veel interessante query's.

## <a name="create-an-azure-sql-database"></a>Een Azure SQL-Database maken
Opnieuw starten vanuit uw abonnement in [Azure-portal][portal], de database maken (en een nieuwe server, tenzij u er al hebt verkregen) die u gaat het om gegevens te schrijven.

![Nieuw, gegevens, SQL](./media/app-insights-code-sample-export-sql-stream-analytics/090-sql.png)

Zorg ervoor dat de database-server staat de toegang tot Azure-services:

![Bladeren, Servers, uw server, instellingen, Firewall, toegang tot Azure toestaan](./media/app-insights-code-sample-export-sql-stream-analytics/100-sqlaccess.png)

## <a name="create-a-table-in-azure-sql-db"></a>Een tabel maken in Azure SQL DB
Verbinding maken met de database gemaakt in de vorige sectie met het beheerprogramma van uw voorkeur. In dit overzicht we gebruiken [SQL Server Management Tools](https://msdn.microsoft.com/ms174173.aspx) (SSMS).

![](./media/app-insights-code-sample-export-sql-stream-analytics/31-sql-table.png)

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

![](./media/app-insights-code-sample-export-sql-stream-analytics/34-create-table.png)

In dit voorbeeld gebruiken we gegevens van paginaweergaven. Overzicht van de gegevens beschikbaar Inspecteer de JSON-uitvoer en Zie de [exporteren gegevensmodel](app-insights-export-data-model.md).

## <a name="create-an-azure-stream-analytics-instance"></a>Azure Stream Analytics-exemplaar maken
Van de [Azure-portal](https://portal.azure.com/), selecteer de Azure Stream Analytics-service en een nieuwe Stream Analytics-taak maken:

![](./media/app-insights-code-sample-export-sql-stream-analytics/SA001.png)

![](./media/app-insights-code-sample-export-sql-stream-analytics/SA002.png)

Wanneer de nieuwe taak is gemaakt, selecteert u **gaat u naar de resource**.

![](./media/app-insights-code-sample-export-sql-stream-analytics/SA003.png)

#### <a name="add-a-new-input"></a>Een nieuwe invoer toevoegen

![](./media/app-insights-code-sample-export-sql-stream-analytics/SA004.png)

Stel deze in op invoer van uw blob continue Export nemen:

![](./media/app-insights-code-sample-export-sql-stream-analytics/SA005.png)

Nu moet u de primaire toegangssleutel van uw Opslagaccount die u eerder hebt genoteerd. Stel dit in als de sleutel van het Opslagaccount.

#### <a name="set-path-prefix-pattern"></a>Set pad voorvoegselpatroon

**Zorg ervoor dat de datumnotatie ingesteld op jjjj-MM-DD (met streepjes).**

Het pad naar het voorvoegsel patroon geeft aan hoe de invoerbestanden in Stream Analytics worden gevonden in de opslag. U moet worden ingesteld in overeenstemming met continue Export hoe de gegevens opslaat. Stel deze als volgt:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

In dit voorbeeld:

* `webapplication27`de naam van de Application Insights-resource **allemaal in kleine letters**. 
* `1234...`de instrumentatiesleutel van de Application Insights-resource is **met streepjes verwijderd**. 
* `PageViews`is het type gegevens wilt analyseren. De beschikbare typen, is afhankelijk van het filter dat u in de continue Export instellen. De geëxporteerde gegevens om de beschikbare typen Zie en bekijk de [exporteren gegevensmodel](app-insights-export-data-model.md).
* `/{date}/{time}`een patroon er wordt letterlijk geschreven.

Als u de naam en sleutel van uw Application Insights-resource, Essentials open op de overzichtspagina, of instellingen.

> [!TIP]
> De voorbeeld-functie gebruiken om te controleren of u het ingevoerde pad correct hebt ingesteld. Als dit mislukt: Controleer of er gegevens in de opslag voor de voorbeeld-periode die u hebt gekozen. Bewerk de definitie van de invoer en controleer u de storage-account, het pad voorvoegsel ingesteld en datumnotatie correct.
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

U ziet dat de eerste enkele eigenschappen specifiek voor paginaweergavegegevens zijn. Uitvoer van andere typen telemetrie heeft andere eigenschappen. Zie de [gedetailleerde gegevens modelverwijzing voor de eigenschaptypen en waarden.](app-insights-export-data-model.md)

## <a name="set-up-output-to-database"></a>Instellen van uitvoer naar de database
Selecteer SQL als uitvoer.

![Selecteer in de stream analytics, uitvoer](./media/app-insights-code-sample-export-sql-stream-analytics/SA006.png)

Geef de SQL-database.

![Vul de details van de database](./media/app-insights-code-sample-export-sql-stream-analytics/SA007.png)

De wizard te sluiten en wachten op een melding dat de uitvoer is ingesteld.

## <a name="start-processing"></a>Verwerking starten
Start de taak van de actiebalk:

![Klik op Start in de stream analytics,](./media/app-insights-code-sample-export-sql-stream-analytics/SA008.png)

U kunt kiezen of verwerken van de gegevens vanaf nu of beginnen met oudere gegevens wordt gestart. De laatste is handig als u hebt continue Export is al een tijdje wordt uitgevoerd.

Ga terug naar SQL Server-beheerprogramma's na een paar minuten en bekijk de gegevens die. Gebruik bijvoorbeeld een query als volgt:

    SELECT TOP 100 *
    FROM [dbo].[PageViewsTable]


## <a name="related-articles"></a>Verwante artikelen:
* [Exporteren naar Power BI met Stream Analytics](app-insights-export-power-bi.md)
* [Gedetailleerde gegevens model verwijzing voor de eigenschaptypen en waarden.](app-insights-export-data-model.md)
* [Continue Export in de Application Insights](app-insights-export-telemetry.md)
* [Application Insights](https://azure.microsoft.com/services/application-insights/)

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[export]: app-insights-export-telemetry.md
[metrics]: app-insights-metrics-explorer.md
[portal]: http://portal.azure.com/
[start]: app-insights-overview.md

