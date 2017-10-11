---
title: Exporteren naar Power BI in Application Insights | Microsoft Docs
description: Analytics-query's kunnen worden weergegeven in Power BI.
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 7f13ea66-09dc-450f-b8f9-f40fdad239f2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: bwren
ms.openlocfilehash: 350a65b1c6432baf258e014c9e63133d2b29e34f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="feed-power-bi-from-application-insights"></a>Power BI feed vanuit Application Insights
[Power BI](http://www.powerbi.com/) is een suite met business analytics hulpmiddelen waarmee u gegevens analyseren en inzichten te delen. Uitgebreide dashboards zijn beschikbaar op elk apparaat. U kunt gegevens uit diverse bronnen, met inbegrip van analysequery's van combineren [Azure Application Insights](app-insights-overview.md).

Er zijn drie aanbevolen methoden voor het exporteren van gegevens van de Application Insights naar Power BI. U kunt deze afzonderlijk of samen gebruiken.

* [**Power BI adapter** ](#power-pi-adapter) -instellen van een volledig dashboard telemetrie van uw app. De reeks grafieken vooraf wordt gedefinieerd, maar u kunt uw eigen query's uit andere bronnen toevoegen.
* [**Exporteren van analysequery's** ](#export-analytics-queries) -schrijven van een query die u wilt met Analytics en exporteren naar Power BI. U kunt deze query op een dashboard samen met andere gegevens plaatsen.
* [**Continue export en Stream Analytics** ](app-insights-export-stream-analytics.md) -hierbij meer werk om in te stellen. Dit is handig als u wilt uw gegevens behouden gedurende langere perioden. Anders wordt worden de andere methoden aanbevolen.

## <a name="power-bi-adapter"></a>Power BI-adapter
Deze methode maakt u een volledig dashboard telemetrie voor u. De initiële gegevensset is vooraf gedefinieerd, maar u kunt meer gegevens toevoegen aan deze.

### <a name="get-the-adapter"></a>De adapter ophalen
1. Aanmelden bij [Power BI](https://app.powerbi.com/).
2. Open **gegevens ophalen**, **Services**, **Application Insights**
   
    ![Ophalen van Application Insights-gegevensbron](./media/app-insights-export-power-bi/power-bi-adapter.png)
3. Geef de details van uw Application Insights-resource.
   
    ![Ophalen van Application Insights-gegevensbron](./media/app-insights-export-power-bi/azure-subscription-resource-group-name.png)
4. Wacht enkele minuten duren om de gegevens te worden geïmporteerd.
   
    ![Power BI-adapter](./media/app-insights-export-power-bi/010.png)

U kunt het dashboard bewerken de Application Insights-grafieken combineren met die van andere bronnen en met Analytics query's. Er is een visualisatie galerie krijgt u meer grafieken, waarbij elke grafiek heeft een parameters die u kunt instellen.

Na de initiële import blijven het dashboard en de rapporten dagelijks bijgewerkt. U kunt het vernieuwingsschema op de gegevensset beheren.

## <a name="export-analytics-queries"></a>Analysequery's exporteren
Deze route kunt u elke gewenste Analytics-query schrijven en die vervolgens exporteren naar een Power BI-dashboard. (U kunt toevoegen aan het dashboard is gemaakt door de adapter.)

### <a name="one-time-install-power-bi-desktop"></a>Eén keer: Installeer Power BI Desktop
Als u wilt uw Application Insights-query importeren, moet u de bureaubladversie van Power BI gebruiken. Maar vervolgens kunt u deze publiceren naar het web of naar uw Power BI-cloud-werkruimte. 

Installeer [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).

### <a name="export-an-analytics-query"></a>Exporteren van een query Analytics
1. [Open Analytics en uw query schrijven](app-insights-analytics-tour.md).
2. Testen en de query te verfijnen totdat u tevreden met de resultaten bent.

   **Zorg ervoor dat de query correct wordt uitgevoerd in Analytics voordat u deze exporteert.**
3. Op de **exporteren** menu kiezen **Power BI (M)**. Bewaar het tekstbestand.
   
    ![Power BI query exporteren](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. In Power BI Desktop selecteer **gegevens ophalen, lege Query** en klik in de query-editor, onder **weergave** Selecteer **geavanceerde Query-Editor**.

    Plak het geëxporteerde M taal script in de geavanceerde Query-Editor.

    ![Geavanceerde query-editor](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

1. Mogelijk moet referenties opgeven voor het toestaan van Power BI voor toegang tot Azure. Gebruik 'organisatieaccount' zich kunnen aanmelden met je Microsoft-account.
   
    ![Azure-referenties om in te schakelen van Power BI voor uw Application Insights-query opgeven](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

    (Als u controleren of de referenties wilt, gebruiken de menuopdracht instellingen voor de gegevensbron in de Query-Editor. Wees voorzichtig om op te geven de referenties die u gebruikt voor Azure, dit van uw referenties voor Power BI afwijken kan.)
2. Kies een visualisatie in voor uw query en selecteer de velden voor x-as, y-as en dimensie segmenteren.
   
    ![Visualisatie selecteren](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)
3. Uw rapport publiceren op uw Power BI-cloud-werkruimte. Daar kunt u een gesynchroniseerde versie insluiten in andere webpagina's.
   
    ![Visualisatie selecteren](./media/app-insights-export-power-bi/publish-power-bi.png)
4. Het rapport handmatig vernieuwen met tussenpozen of een geplande vernieuwing op de optiepagina instellen.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="401-or-403-unauthorized"></a>401- of 403 niet geautoriseerd 
Dit kan gebeuren als uw refesh-token is niet bijgewerkt. Probeer de volgende stappen om te controleren of dat u nog steeds toegang hebben. Als u hebt toegang tot en refershing de referenties niet werkt, opent u een ondersteuningsticket.

1. Meld u aan bij de Azure-Portal en controleer of dat u toegang hebt tot de resource
2. Probeer de referenties voor het Dashboard vernieuwen

### <a name="502-bad-gateway"></a>502 Slechte Gateway
Dit wordt meestal veroorzaakt door een Analytics-query die te veel gegevens retourneert. U moet proberen met behulp van een kleinere tijdsbereik of met behulp van de [geleden](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) of [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek) werkt alleen [project](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator) de velden die u nodig.

Als de gegevensset die afkomstig zijn van de query Analytics verminderen niet voldoet aan uw vereisten moet u overwegen de [API](https://dev.applicationinsights.io/documentation/overview) voor het ophalen van een grotere gegevensset. Hier vindt u instructies voor het converteren van de export M-Query voor het gebruik van de API.

1. Maak een [API-sleutel](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)
2. Bijwerken van het Power BI-M-script dat u hebt geëxporteerd van analytische gegevens door de ARM-URL vervangen door een AI-API (Zie onderstaand voorbeeld)
   * Vervang **https://management.azure.com/subscriptions/...**
   * met **https://api.applicationinsights.io/beta/apps/...**
3. Ten slotte referenties bijwerken op basis en gebruik van uw API-sleutel
  

**Bestaand Script**
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
**Bijgewerkte Script**
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a>Over steekproeven
Als uw toepassing grote hoeveelheden gegevens verzendt, wordt de functie voor adaptieve steekproeven werkt en wordt slechts een percentage van uw telemetrie verzenden. Hetzelfde geldt als u de samplingfrequentie handmatig hebt ingesteld in de SDK of op de opname. [Meer informatie over steekproeven.](app-insights-sampling.md)


## <a name="next-steps"></a>Volgende stappen
* [Power BI - informatie](http://www.powerbi.com/learning/)
* [Analytics-zelfstudie](app-insights-analytics-tour.md)

