---
title: Exporteren naar Power BI in Azure Application Insights | Microsoft Docs
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
ms.author: mbullwin
ms.openlocfilehash: 19595983ba49a88d9139c85afbf38d3106d4a81d
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="feed-power-bi-from-application-insights"></a>Power BI feed vanuit Application Insights
[Power BI](http://www.powerbi.com/) is een suite met hulpprogramma's voor business waarmee u gegevens analyseren en inzichten te delen. Uitgebreide dashboards zijn beschikbaar op elk apparaat. U kunt gegevens uit diverse bronnen, met inbegrip van analysequery's van combineren [Azure Application Insights](app-insights-overview.md).

Er zijn drie aanbevolen methoden voor het exporteren van gegevens van de Application Insights naar Power BI. U kunt deze afzonderlijk of samen gebruiken.

* [**Power BI adapter**](#power-pi-adapter). Instellen van een volledig dashboard telemetrie van uw app. De reeks grafieken vooraf wordt gedefinieerd, maar u kunt uw eigen query's uit andere bronnen toevoegen.
* [**Exporteren van analysequery's**](#export-analytics-queries). Schrijven van een query die u wilt en exporteren naar Power BI. U kunt uw query schrijven met behulp van Analytics of schrijf deze uit de schoorstenen gebruik. U kunt deze query op een dashboard, samen met andere gegevens plaatsen.
* [**Continue export en Azure Stream Analytics**](app-insights-export-stream-analytics.md). Deze methode is handig als u wilt uw gegevens behouden gedurende langere perioden. Als u dat niet doet, gebruikt u een van de andere methoden, omdat deze meer werk voor het instellen van omvat.

## <a name="power-bi-adapter"></a>Power BI-adapter
Deze methode maakt u een volledig dashboard telemetrie voor u. De initiële gegevensset is vooraf gedefinieerd, maar u kunt meer gegevens toevoegen aan deze.

### <a name="get-the-adapter"></a>De adapter ophalen
1. Aanmelden bij [Power BI](https://app.powerbi.com/).
2. Open **gegevens ophalen**, **Services**, en vervolgens **Application Insights**.
   
    ![Schermafbeeldingen van ophalen uit de Application Insights-gegevensbron](./media/app-insights-export-power-bi/power-bi-adapter.png)
3. Geef de details van uw Application Insights-resource.
   
    ![Schermopname van ophalen uit de Application Insights-gegevensbron](./media/app-insights-export-power-bi/azure-subscription-resource-group-name.png)
4. Wacht enkele minuten duren om de gegevens te worden geïmporteerd.
   
    ![Schermopname van Power BI-adapter](./media/app-insights-export-power-bi/010.png)

U kunt het dashboard bewerken de Application Insights-grafieken combineren met die van andere bronnen en met Analytics query's. U krijgt meer grafieken in de galerie visualisatie, en elke grafiek heeft parameters die u kunt instellen.

Na de initiële import blijven het dashboard en de rapporten dagelijks bijgewerkt. U kunt het vernieuwingsschema op de gegevensset beheren.

## <a name="export-analytics-queries"></a>Analysequery's exporteren
Deze route kunt u een query Analytics zoals, of exporteren uit gebruik schoorstenen schrijven en die vervolgens exporteren naar een Power BI-dashboard. (U kunt toevoegen aan het dashboard is gemaakt door de adapter.)

### <a name="one-time-install-power-bi-desktop"></a>Eén keer: Installeer Power BI Desktop
Als u wilt uw Application Insights-query importeren, moet u de bureaubladversie van Power BI gebruiken. U kunt deze vervolgens publiceren naar het web of naar uw Power BI-cloud-werkruimte. 

Installeer [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).

### <a name="export-an-analytics-query"></a>Exporteren van een query Analytics
1. [Open Analytics en uw query schrijven](app-insights-analytics-tour.md).
2. Testen en de query te verfijnen totdat u tevreden met de resultaten bent. Zorg ervoor dat de query correct wordt uitgevoerd in Analytics voordat u deze exporteert.
3. Op de **exporteren** menu kiezen **Power BI (M)**. Bewaar het tekstbestand.
   
    ![Schermopname van Analytics, met Export menu gemarkeerd](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. Selecteer in Power BI Desktop **gegevens ophalen** > **leeg Query**. Klik in de query-editor, onder **weergave**, selecteer **geavanceerde Editor**.

    Het geëxporteerde M taal script in de geavanceerde Editor plakken.

    ![Schermopname van Power BI Desktop, met geavanceerde Editor gemarkeerd](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

1. Mogelijk moet u referenties opgeven zodat Power BI voor toegang tot Azure. Gebruik **organisatieaccount** zich kunnen aanmelden met je Microsoft-account.
   
    ![Schermopname van Power BI Query het dialoogvenster Instellingen voor](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

    Als u controleren of de referenties wilt, gebruikt u de **Gegevensbroninstellingen** menuopdracht in de query-editor. Zorg ervoor dat de referenties die u gebruikt voor Azure, dit van uw referenties voor Power BI afwijken kan opgeven.
2. Kies een visualisatie in voor uw query en selecteer de velden voor x-as, y-as en dimensie segmenteren.
   
    ![Schermopname van Power BI Desktop vizualization opties](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)
3. Uw rapport publiceren op uw Power BI-cloud-werkruimte. Daar kunt u een gesynchroniseerde versie insluiten in andere webpagina's.
   
    ![Schermopname van Power BI Desktop, met de knop publiceren gemarkeerd](./media/app-insights-export-power-bi/publish-power-bi.png)
4. Het rapport handmatig vernieuwen met tussenpozen of een geplande vernieuwing op de optiepagina instellen.

### <a name="export-a-funnel"></a>Exporteren van een trechter
1. [Maken van de trechter](usage-funnels.md).
2. Selecteer **Power BI**. 

   ![Schermopname van Power BI-knop](./media/app-insights-export-power-bi/button.png)
   
3. Selecteer in Power BI Desktop **gegevens ophalen** > **leeg Query**. Klik in de query-editor, onder **weergave**, selecteer **geavanceerde Editor**.

   ![Schermopname van Power BI Desktop, met lege Query knop gemarkeerd](./media/app-insights-export-power-bi/blankquery.png)

   Het geëxporteerde M taal script in de geavanceerde Editor plakken. 

   ![Schermopname van Power BI Desktop, met geavanceerde Editor gemarkeerd](./media/app-insights-export-power-bi/advancedquery.png)

4. Selecteren van items uit de query en kies een visualisatie van de trechter.

   ![Schermopname van Power BI Desktop vizualization opties](./media/app-insights-export-power-bi/selectsequence.png)

5. Wijzig de titel om af te stemmen en publiceren van uw rapport naar uw Power BI-cloud-werkruimte. 

   ![Schermopname van Power BI Desktop, met titelwijziging gemarkeerd](./media/app-insights-export-power-bi/changetitle.png)

## <a name="troubleshooting"></a>Problemen oplossen

U kunt tegenkomen fouten met betrekking tot de referenties of de grootte van de gegevensset. Hier volgt enige informatie over wat te doen over deze fouten.

### <a name="unauthorized-401-or-403"></a>Niet-geautoriseerde (401 of 403)
Dit kan gebeuren als het vernieuwingstoken dat is niet bijgewerkt. Voer deze stappen om te controleren of dat u nog steeds toegang hebben:

1. Meld u aan bij de Azure-portal en controleer of dat u toegang hebt tot de resource.
2. Probeer te vernieuwen van de referenties voor het dashboard.

 Als u toegang hebt en vernieuwen van de referenties niet werkt, opent u een ondersteuningsticket.

### <a name="bad-gateway-502"></a>Ongeldige Gateway (502)
Dit wordt meestal veroorzaakt door een Analytics-query die te veel gegevens retourneert. Gebruik een kleinere tijdsbereik voor de query. 

Als het verminderen van de gegevensset die afkomstig zijn van de query Analytics niet voldoet aan uw vereisten, kunt u overwegen de [API](https://dev.applicationinsights.io/documentation/overview) voor het ophalen van een grotere gegevensset. Hier volgt het converteren van de export M-Query voor het gebruik van de API.

1. Maak een [API-sleutel](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).
2. Werk het Power BI-M-script dat u hebt geëxporteerd van analytische gegevens door de Azure Resource Manager-URL vervangen door de Application Insights-API.
   * Vervang **https://management.azure.com/subscriptions/...**
   * met **https://api.applicationinsights.io/beta/apps/...**
3. Ten slotte de referenties bijwerken op basis en gebruik van uw API-sleutel.
  

**Bestaand script**
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
**Bijgewerkte script**
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a>Over steekproeven
Als uw toepassing grote hoeveelheden gegevens verzendt, wilt u mogelijk gebruik van de functie adaptieve steekproeven, waardoor slechts een percentage van uw telemetrie verzendt. Hetzelfde geldt als u de samplingfrequentie handmatig hebt ingesteld in de SDK of op de opname. [Meer informatie over steekproeven](app-insights-sampling.md).


## <a name="next-steps"></a>Volgende stappen
* [Power BI - informatie](http://www.powerbi.com/learning/)
* [Analytics-zelfstudie](app-insights-analytics-tour.md)

