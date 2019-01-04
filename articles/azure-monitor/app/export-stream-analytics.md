---
title: Exporteren met behulp van Stream Analytics van Azure Application Insights | Microsoft Docs
description: Stream Analytics kunt continu transformeren, filteren en de gegevens die u vanuit Application Insights exporteren te routeren.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 31594221-17bd-4e5e-9534-950f3b022209
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/04/2018
ms.author: mbullwin
ms.openlocfilehash: 8afe994f2441e736d660a306746508e9c6d13691
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "54001835"
---
# <a name="use-stream-analytics-to-process-exported-data-from-application-insights"></a>Stream Analytics gebruiken voor het verwerken van de geëxporteerde gegevens van Application Insights
[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) is het ideaal hulpprogramma voor het verwerken van gegevens [geëxporteerd uit Application Insights](export-telemetry.md). Stream Analytics kunt u gegevens ophalen uit een groot aantal bronnen. Deze kunt transformeren en de gegevens filteren en vervolgens te routeren naar een verscheidenheid aan Put.

In dit voorbeeld maken we een adapter die neemt gegevens uit Application Insights, naam en sommige velden worden verwerkt en geeft het in Power BI.

> [!WARNING]
> Er zijn veel beter en eenvoudiger [aanbevolen manier om weer te geven van Application Insights-gegevens in Power BI](../../application-insights/app-insights-export-power-bi.md). Het pad dat hier wordt geïllustreerd is slechts een voorbeeld ter illustratie van de geëxporteerde gegevens te verwerken.
> 
> 

![Diagram voor exporteren via Software Assurance voor aan PBI blokkeren](./media/export-stream-analytics/020.png)

## <a name="create-storage-in-azure"></a>Opslag maken in Azure
Continue export voert altijd gegevens uit naar een Azure Storage-account, dus moet u eerst de opslag maken.

1. Een 'klassieke' storage-account maken in uw abonnement in de [Azure-portal](https://portal.azure.com).
   
   ![Kies in Azure portal, nieuw, gegevens, opslag](./media/export-stream-analytics/030.png)
2. Een container maken
   
    ![In de nieuwe opslag Containers selecteren, klikt u op de Containers tegel en klik vervolgens op toevoegen](./media/export-stream-analytics/040.png)
3. De toegangssleutel voor opslag kopiëren
   
    U moet deze beschikbaar voor het instellen van de invoer voor de stream analytics-service.
   
    ![In de opslag, open instellingen, sleutels, en een kopie van de primaire toegangssleutel](./media/export-stream-analytics/045.png)

## <a name="start-continuous-export-to-azure-storage"></a>Start de continue export naar Azure storage
[Continue export](export-telemetry.md) verplaatst gegevens van Application Insights in Azure storage.

1. In de Azure-portal, blader naar de Application Insights-resource die u hebt gemaakt voor uw toepassing.
   
    ![Kies Bladeren, Application Insights, uw toepassing](./media/export-stream-analytics/050.png)
2. Maak een continue export.
   
    ![Kies instellingen, continue Export toevoegen](./media/export-stream-analytics/060.png)

    Selecteer het opslagaccount dat u eerder hebt gemaakt:

    ![Stel de doelserver exporteren](./media/export-stream-analytics/070.png)

    Stel de typen gebeurtenissen die u wilt zien:

    ![Kies gebeurtenistypen](./media/export-stream-analytics/080.png)

1. Sommige gegevens worden verzameld, kunnen. Begint en laat mensen uw toepassing een tijdje gebruiken. Telemetrie komt en ziet u statistische grafieken in [metric explorer](../../azure-monitor/app/metrics-explorer.md) en afzonderlijke gebeurtenissen in [diagnostische gegevens doorzoeken](../../azure-monitor/app/diagnostic-search.md). 
   
    En ook de gegevens worden geëxporteerd naar uw opslag. 
2. Inspecteer de geëxporteerde gegevens. Kies in Visual Studio **weergeven / Cloud Explorer**, en open Azure / Storage. (Als u deze optie niet hebt, moet u de Azure SDK installeren: Open het dialoogvenster Nieuw Project en Visual C# / Cloud / Microsoft Azure SDK voor .NET ophalen.)
   
    ![](./media/export-stream-analytics/04-data.png)
   
    Maak een notitie van het algemene gedeelte van de naam van het pad, die is afgeleid van de sleutel van de naam en -instrumentatie in de toepassing. 

De gebeurtenissen worden geschreven naar de blob-bestanden in de JSON-indeling. Elk bestand kan een of meer gebeurtenissen bevatten. Dus graag we lezen van gegevens van de gebeurtenis en de velden die we willen filteren. Er zijn allerlei dingen die we met de gegevens kan doen, maar ons abonnement is vandaag nog aan Stream Analytics gebruiken voor het doorgeven van de gegevens naar Power BI.

## <a name="create-an-azure-stream-analytics-instance"></a>Azure Stream Analytics-exemplaar maken
Uit de [Azure-portal](https://portal.azure.com/), selecteert u de Azure Stream Analytics-service en een nieuwe Stream Analytics-taak te maken:

![](./media/export-stream-analytics/SA001.png)

![](./media/export-stream-analytics/SA002.png)

Wanneer de nieuwe taak is gemaakt, selecteert u **naar de resource gaan**.

![](./media/export-stream-analytics/SA003.png)

### <a name="add-a-new-input"></a>Een nieuwe invoer toevoegen

![](./media/export-stream-analytics/SA004.png)

Instellen om te ondernemen invoer van de continue Export-blob:

![](./media/export-stream-analytics/SA0005.png)

Nu moet u de primaire toegangssleutel van uw Opslagaccount, die u eerder hebt genoteerd. Stel dit in als de Opslagaccountsleutel.

### <a name="set-path-prefix-pattern"></a>Set-voorvoegsel padpatroon

**Zorg ervoor dat de datumnotatie ingesteld op DD-MM-jjjj (met streepjes).**

Het pad naar het voorvoegsel patroon geeft aan waar de invoerbestanden in Stream Analytics worden gevonden in de opslag. U moet deze overeenkomen met het continue Export hoe de gegevens opslaat in te stellen. Doet u het als volgt:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

In dit voorbeeld:

* `webapplication27` de naam van de Application Insights-resource **alleen kleine letters**.
* `1234...` de instrumentatiesleutel van de Application Insights-resource is **weglaten streepjes**. 
* `PageViews` is het type gegevens dat u wilt analyseren. De beschikbare typen, is afhankelijk van het filter dat u in de continue Export instellen. Bekijk de geëxporteerde gegevens om te zien van de beschikbare typen en Zie de [gegevensmodel exporteren](export-data-model.md).
* `/{date}/{time}` een patroon is letterlijk geschreven.

> [!NOTE]
> Inspecteer de opslag om ervoor te zorgen dat u het pad naar rechts.
> 

## <a name="add-new-output"></a>Nieuwe uitvoer toevoegen
Selecteer nu uw taak > **uitvoer** > **toevoegen**.

![](./media/export-stream-analytics/SA006.png)


![Selecteer het nieuwe kanaal, uitvoer, toevoegen, Power BI](./media/export-stream-analytics/SA010.png)

Geef uw **werk- of schoolaccount** te machtigen van Stream Analytics voor toegang tot uw Power BI-resource. Vervolgens tot een naam op voor de uitvoer en voor de doel-Power BI-gegevensset en de tabel.

## <a name="set-the-query"></a>Instellen van de query
De query bepaalt de vertaling van invoer om uit te voeren.

Gebruik de functie testen om te controleren dat u de juiste uitvoer krijgen. Wijs hieraan de voorbeeldgegevens die u hebt gemaakt op de pagina invoer. 

### <a name="query-to-display-counts-of-events"></a>Query om aantallen gebeurtenissen weer te geven
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

* exporteren-invoer is de alias die we naar de stroom invoer heeft gegeven
* pbi-uitvoer is de uitvoeralias die we hebben gedefinieerd
* We gebruiken [buitenste toepassen GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx) omdat de naam van de gebeurtenis in een geneste JSON-matrix. Selecteer hervat vervolgens de naam van de gebeurtenis, samen met een telling van het aantal instanties met die naam bestaat in de periode. De [Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx) component groepen de elementen in perioden van één minuut.

### <a name="query-to-display-metric-values"></a>Query voor het weergeven van metrische waarden
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

* Deze query ingezoomd in de telemetrie van metrische gegevens om de tijd van de gebeurtenis en de metrische waarde te verkrijgen. De metrische waarden zijn in een matrix, zodat we het buitenste toepassen GetElements-patroon gebruiken om op te halen van de rijen. "myMetric" is in dit geval de naam van de metrische gegevens. 

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

* Deze query bevat waarden van de dimensie-eigenschappen zonder afhankelijk van een bepaalde dimensie wordt bij een vaste index in de dimensiematrix.

## <a name="run-the-job"></a>De taak uitvoeren
U kunt een datum selecteren in het verleden om de taak uit te starten. 

![Selecteer de taak en klik op de Query. Plak het onderstaande voorbeeld.](./media/export-stream-analytics/SA008.png)

Wacht totdat de taak wordt uitgevoerd.

## <a name="see-results-in-power-bi"></a>Resultaten worden weergegeven in Power BI
> [!WARNING]
> Er zijn veel beter en eenvoudiger [aanbevolen manier om weer te geven van Application Insights-gegevens in Power BI](../../application-insights/app-insights-export-power-bi.md). Het pad dat hier wordt geïllustreerd is slechts een voorbeeld ter illustratie van de geëxporteerde gegevens te verwerken.
> 
> 

Open Power BI met uw werk of school-account en selecteer de gegevensset en de tabel die u hebt gedefinieerd als de uitvoer van de Stream Analytics-taak.

![Selecteer uw gegevensset en de velden in Power BI.](./media/export-stream-analytics/200.png)

Nu kunt u deze gegevensset in rapporten en dashboards in [Power BI](https://powerbi.microsoft.com).

![Selecteer uw gegevensset en de velden in Power BI.](./media/export-stream-analytics/210.png)

## <a name="no-data"></a>Zijn er geen gegevens?
* Controleer dat u [datumnotatie instellen](#set-path-prefix-pattern) correct aan jjjj-MM-jjjj (met streepjes).

## <a name="video"></a>Video
Noam Ben Zeev laat zien hoe voor het verwerken van de geëxporteerde gegevens met behulp van Stream Analytics.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Export-to-Power-BI-from-Application-Insights/player]
> 
> 

## <a name="next-steps"></a>Volgende stappen
* [Continue export](export-telemetry.md)
* [Gedetailleerde gegevens modelleren verwijzing voor de typen eigenschappen en waarden.](export-data-model.md)
* [Application Insights](../../application-insights/app-insights-overview.md)

