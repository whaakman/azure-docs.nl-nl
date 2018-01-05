---
title: Exporteren met behulp van de Stream Analytics uit Azure Application Insights | Microsoft Docs
description: Stream Analytics kunt continu transformeren, filteren en doorsturen van de gegevens die u uit de Application Insights exporteren.
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 31594221-17bd-4e5e-9534-950f3b022209
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 01/04/2018
ms.author: mbullwin
ms.openlocfilehash: ddaf7bf12854aa5f80c1d292613c3049850ca3ff
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="use-stream-analytics-to-process-exported-data-from-application-insights"></a>Gebruik van Stream Analytics voor het verwerken van de geëxporteerde gegevens van Application Insights
[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) is het ideaal hulpprogramma voor het verwerken van gegevens [geëxporteerd uit de Application Insights](app-insights-export-telemetry.md). Stream Analytics kunt ophalen van gegevens uit verschillende bronnen. U kunt transformeren en filter de gegevens en vervolgens doorsturen naar een groot aantal Put.

In dit voorbeeld maakt u een netwerkadapter die worden gegevens uit de Application Insights, naam en het doorgesluisd naar Power BI sommige velden worden verwerkt.

> [!WARNING]
> Er zijn veel beter en eenvoudiger [aanbevolen manieren Application Insights-gegevens weergeven in Power BI](app-insights-export-power-bi.md). Het pad hier geïllustreerd is slechts een voorbeeld ter illustratie van de geëxporteerde gegevens verwerken.
> 
> 

![Diagram voor exporteren via SA naar PBI blokkeren](./media/app-insights-export-stream-analytics/020.png)

## <a name="create-storage-in-azure"></a>Maken van opslag in Azure
Continue export levert altijd gegevens aan een Azure Storage-account, dus u moet de opslag om eerst te maken.

1. Een 'klassiek' storage-account maken in uw abonnement in de [Azure-portal](https://portal.azure.com).
   
   ![Kies nieuw, gegevens, opslag in Azure-portal](./media/app-insights-export-stream-analytics/030.png)
2. Een container maken
   
    ![In de nieuwe opslag Containers selecteren, klikt u op de tegel Containers en toevoegen](./media/app-insights-export-stream-analytics/040.png)
3. De toegangssleutel voor opslag kopiëren
   
    U moet deze snel voor het instellen van de invoer van de stream analytics-service.
   
    ![Instellingen, sleutels, open in de opslag, en een kopie van de primaire toegangssleutel](./media/app-insights-export-stream-analytics/045.png)

## <a name="start-continuous-export-to-azure-storage"></a>Start de continue export naar Azure-opslag
[Continue export](app-insights-export-telemetry.md) worden gegevens uit de Application Insights in Azure-opslag verplaatst.

1. Blader naar de Application Insights-resource die u hebt gemaakt voor uw toepassing in de Azure-portal.
   
    ![Kies Bladeren, Application Insights uw toepassing](./media/app-insights-export-stream-analytics/050.png)
2. Maak een continue export.
   
    ![Instellingen voor continue Export toevoegen](./media/app-insights-export-stream-analytics/060.png)

    Selecteer het opslagaccount dat u eerder hebt gemaakt:

    ![Stel de doelserver exporteren](./media/app-insights-export-stream-analytics/070.png)

    Stel de typen gebeurtenissen die u wilt zien:

    ![Gebeurtenistypen kiezen](./media/app-insights-export-stream-analytics/080.png)

1. Laat een aantal gegevens worden verzameld. Terug zitten en toestaan dat uw toepassing een tijdje gebruiken. Telemetrie wordt geleverd in en ziet u statistische grafieken in [metrische explorer](app-insights-metrics-explorer.md) en afzonderlijke gebeurtenissen in [diagnostische gegevens doorzoeken](app-insights-diagnostic-search.md). 
   
    En ook de gegevens worden geëxporteerd naar uw opslag. 
2. Inspecteer de geëxporteerde gegevens. Kies in Visual Studio **weergeven / Cloud Explorer**, en open Azure / opslag. (Als u deze optie niet hebt, moet u de Azure SDK installeren: Open het dialoogvenster New Project en Visual C# / Cloud / ophalen van Microsoft Azure SDK voor .NET.)
   
    ![](./media/app-insights-export-stream-analytics/04-data.png)
   
    Noteer het algemene gedeelte van de padnaam die is afgeleid van de sleutel voor naam en instrumentatie van toepassing. 

De gebeurtenissen worden geschreven naar de blob-bestanden in de JSON-indeling. Elk bestand kan een of meer gebeurtenissen bevatten. Dus willen we gelezen gegevens van de gebeurtenis en de velden die we wilt filteren. Er zijn alle soorten wat die we met de gegevens doen kan, maar onze plan is vandaag de dag Stream Analytics gebruiken naar de gegevens met Power BI pipe.

## <a name="create-an-azure-stream-analytics-instance"></a>Azure Stream Analytics-exemplaar maken
Van de [Azure-portal](https://portal.azure.com/), selecteer de Azure Stream Analytics-service en een nieuwe Stream Analytics-taak maken:

![](./media/app-insights-export-stream-analytics/SA001.png)

![](./media/app-insights-export-stream-analytics/SA002.png)

Wanneer de nieuwe taak is gemaakt, selecteert u **gaat u naar de resource**.

![](./media/app-insights-export-stream-analytics/SA003.png)

### <a name="add-a-new-input"></a>Een nieuwe invoer toevoegen

![](./media/app-insights-export-stream-analytics/SA004.png)

Stel deze in op invoer van uw blob continue Export nemen:

![](./media/app-insights-export-stream-analytics/SA005.png)

Nu moet u de primaire toegangssleutel van uw Opslagaccount die u eerder hebt genoteerd. Stel dit in als de sleutel van het Opslagaccount.

### <a name="set-path-prefix-pattern"></a>Set pad voorvoegselpatroon

**Zorg ervoor dat de datumnotatie ingesteld op jjjj-MM-DD (met streepjes).**

Het pad naar het voorvoegsel patroon geeft aan waar de invoerbestanden in Stream Analytics worden gevonden in de opslag. U moet worden ingesteld in overeenstemming met continue Export hoe de gegevens opslaat. Stel deze als volgt:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

In dit voorbeeld:

* `webapplication27`de naam van de Application Insights-resource **alle kleine letters**.
* `1234...`de instrumentatiesleutel van de Application Insights-resource is **weglaten streepjes**. 
* `PageViews`is het type gegevens dat u wilt analyseren. De beschikbare typen, is afhankelijk van het filter dat u in de continue Export instellen. De geëxporteerde gegevens om de beschikbare typen Zie en bekijk de [exporteren gegevensmodel](app-insights-export-data-model.md).
* `/{date}/{time}`een patroon er wordt letterlijk geschreven.

> [!NOTE]
> Inspecteer de opslag om ervoor te zorgen dat u het pad naar rechts.
> 

## <a name="add-new-output"></a>Nieuwe uitvoer toevoegen
Nu uw taak selecteren > **uitvoer** > **toevoegen**.

![](./media/app-insights-export-stream-analytics/SA006.png)


![Selecteer het nieuwe kanaal, klikt u op de uitvoer, toevoegen, Power BI](./media/app-insights-export-stream-analytics/SA010.png)

Geef uw **werk- of schoolaccount** voor het autoriseren van Stream Analytics voor toegang tot uw Power BI-bron. Vervolgens tot een naam voor de uitvoer en voor de Power BI-gegevensset doel en de tabel.

## <a name="set-the-query"></a>Instellen van de query
De query bepaalt de vertaling van invoer om uit te voeren.

Gebruik de functie Test om te controleren dat u de uitvoer van de juiste krijgt. Geef deze de voorbeeldgegevens die u hebt gemaakt van de invoer-pagina. 

### <a name="query-to-display-counts-of-events"></a>Query voor het aantal gebeurtenissen weergeven
Plak deze query:

```SQL

    SELECT
      flat.ArrayValue.name,
      count(*)
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.[event]) as flat
    GROUP BY TumblingWindow(minute, 1), flat.ArrayValue.name
```

* export-invoer is de alias die wordt gestuurd naar de stroom invoer
* pbi-uitvoer is de uitvoeralias die is gedefinieerd
* We gebruiken [buitenste toepassen GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx) omdat de gebeurtenisnaam van de in een geneste JSON-matrix is. Selecteer hervat vervolgens de naam van de gebeurtenis, samen met een telling van het aantal exemplaren met die naam aanwezig in de periode. De [Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx) component elementen gegroepeerd in perioden van één minuut.

### <a name="query-to-display-metric-values"></a>Query voor metrische waarden weergeven
```SQL

    SELECT
      A.context.data.eventtime,
      avg(CASE WHEN flat.arrayvalue.myMetric.value IS NULL THEN 0 ELSE  flat.arrayvalue.myMetric.value END) as myValue
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.context.custom.metrics) as flat
    GROUP BY TumblingWindow(minute, 1), A.context.data.eventtime

``` 

* Deze query ingezoomd in de telemetrie van de metrische gegevens ophalen van de tijd van de gebeurtenis en de metrische waarde. De metrische waarden zijn binnen een matrix, zodat we het buitenste toepassen GetElements-patroon gebruiken om op te halen van de rijen. 'myMetric' is de naam van de metrische gegevens in dit geval. 

### <a name="query-to-include-values-of-dimension-properties"></a>Query voor het opnemen van waarden van de dimensie-eigenschappen
```SQL

    WITH flat AS (
    SELECT
      MySource.context.data.eventTime as eventTime,
      InstanceId = MyDimension.ArrayValue.InstanceId.value,
      BusinessUnitId = MyDimension.ArrayValue.BusinessUnitId.value
    FROM MySource
    OUTER APPLY GetArrayElements(MySource.context.custom.dimensions) MyDimension
    )
    SELECT
     eventTime,
     InstanceId,
     BusinessUnitId
    INTO AIOutput
    FROM flat

```

* Deze query bevat de waarden van de dimensie-eigenschappen zonder afhankelijk van een bepaalde dimensie wordt op een vaste index in de dimensiematrix.

## <a name="run-the-job"></a>Voer de taak
U kunt een datum selecteren in het verleden om de taak uit te starten. 

![De taak te selecteren en klik op de Query. Plak het onderstaande voorbeeld.](./media/app-insights-export-stream-analytics/SA008.png)

Wacht totdat de taak wordt uitgevoerd.

## <a name="see-results-in-power-bi"></a>Weergeven van resultaten in Power BI
> [!WARNING]
> Er zijn veel beter en eenvoudiger [aanbevolen manieren Application Insights-gegevens weergeven in Power BI](app-insights-export-power-bi.md). Het pad hier geïllustreerd is slechts een voorbeeld ter illustratie van de geëxporteerde gegevens verwerken.
> 
> 

Power BI openen met uw werk of school-account en selecteer de gegevensset en de tabel die u hebt gedefinieerd als de uitvoer van de Stream Analytics-taak.

![Selecteer uw gegevensset en de velden in Power BI.](./media/app-insights-export-stream-analytics/200.png)

Nu kunt u deze gegevensset in rapporten en dashboards in [Power BI](https://powerbi.microsoft.com).

![Selecteer uw gegevensset en de velden in Power BI.](./media/app-insights-export-stream-analytics/210.png)

## <a name="no-data"></a>Zijn er geen gegevens?
* Controleer dat u [datumnotatie instellen](#set-path-prefix-pattern) correct naar jjjj-MM-DD (met streepjes).

## <a name="video"></a>Video
Noam Ben Zeev laat zien hoe geëxporteerde gegevens met behulp van de Stream Analytics verwerken.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Export-to-Power-BI-from-Application-Insights/player]
> 
> 

## <a name="next-steps"></a>Volgende stappen
* [Continue export](app-insights-export-telemetry.md)
* [Gedetailleerde gegevens model verwijzing voor de eigenschaptypen en waarden.](app-insights-export-data-model.md)
* [Application Insights](app-insights-overview.md)

