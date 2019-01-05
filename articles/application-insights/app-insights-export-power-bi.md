---
title: Exporteren naar Power BI van Azure Application Insights | Microsoft Docs
description: Analytics-query's kunnen worden weergegeven in Power BI.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 7f13ea66-09dc-450f-b8f9-f40fdad239f2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 08/10/2018
ms.author: mbullwin
ms.openlocfilehash: 1a5f756c4fac92260e77b24ca2cde6abccf0797a
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54042781"
---
# <a name="feed-power-bi-from-application-insights"></a>Power BI-kanaal van Application Insights
[Power BI](https://www.powerbi.com/) is een suite met hulpprogramma's voor business die helpt u bij het analyseren van gegevens en inzichten te delen. Uitgebreide dashboards zijn beschikbaar op elk apparaat. U kunt gegevens uit verschillende bronnen, met inbegrip van Analytics-query's uit combineren [Azure Application Insights](app-insights-overview.md).

Er zijn drie methoden voor het Application Insights-gegevens exporteren naar Power BI:

* [**Analytics-query's exporteren**](#export-analytics-queries). Dit is de aanbevolen methode. Schrijf een query die u wilt en exporteren naar Power BI. U kunt deze query plaatsen op een dashboard, samen met andere gegevens.
* [**Continue export en Azure Stream Analytics**](../azure-monitor/app/export-stream-analytics.md). Deze methode is handig als u wilt uw gegevens bewaren gedurende lange perioden. Als u een uitgebreide gegevens bewaren-vereiste niet hebt, gebruikt u de methode export analytics-query. Continue export en Stream Analytics omvat meer werk om in te stellen en de extra opslagruimte overhead.
* [**Power BI-adapter**](#power-pi-adapter). De set met grafieken is vooraf gedefinieerd, maar u kunt uw eigen query's uit andere bronnen toevoegen.

> [!NOTE]
> De Power BI-adapter is nu **afgeschaft**. De vooraf gedefinieerde grafieken voor deze oplossing worden ingevuld door statische vergrendelde query's. U hebt niet de mogelijkheid om te bewerken van deze query's en afhankelijk van bepaalde eigenschappen van uw gegevens is het mogelijk dat de verbinding met Power BI om succesvol te zijn, maar er zijn geen gegevens is gevuld. Dit komt door uitsluitingscriteria die zijn ingesteld in de query vastgelegd. Hoewel deze oplossing nog steeds voor bepaalde klanten werken kan, vanwege het ontbreken van de flexibiliteit van de adapter van de aanbevolen oplossing is met de [ **Analytics-query exporteren** ](#export-analytics-queries) functionaliteit.

## <a name="export-analytics-queries"></a>Analytics-query's exporteren
Deze route kunt u een Analytics-query u graag in of exporteren uit gebruik Trechters schrijven en die vervolgens exporteren naar Power BI-dashboard. (U kunt toevoegen aan het dashboard gemaakt door de adapter.)

### <a name="one-time-install-power-bi-desktop"></a>Één keer: Power BI Desktop installeren
Als u wilt importeren in uw Application Insights-query, moet u de bureaubladversie van Power BI gebruiken. U kunt deze vervolgens publiceren naar de web- of naar de werkruimte van uw Power BI-cloud. 

Installeer [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).

### <a name="export-an-analytics-query"></a>Exporteren van een Analytics-query
1. [Analytics openen en uw query schrijven](../azure-monitor/log-query/get-started-portal.md).
2. Test en de query te verfijnen totdat u tevreden met de resultaten bent. Zorg ervoor dat de query correct wordt uitgevoerd in Analytics voordat u het exporteren.
3. Op de **exporteren** menu, kiest u **Power BI (M)**. Bewaar het tekstbestand.
   
    ![Schermafbeelding van de analyses, met uitvoer menu gemarkeerd](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. Selecteer in Power BI Desktop, **gegevens ophalen** > **lege Query**. Klik in de query-editor onder **weergave**, selecteer **geavanceerde Editor**.

    Het geëxporteerde taal voor M-script in de geavanceerde Editor plakken.

    ![Schermafbeelding van de Power BI Desktop, met de geavanceerde Editor gemarkeerd](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

5. Mogelijk moet u referenties opgeven zodat Power BI toegang tot Azure. Gebruik **organisatieaccount** zich aanmelden met uw Microsoft-account.
   
    ![Dialoogvenster Screenshot van Power BI Query-instellingen](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

    Als u controleren of de referenties wilt, gebruikt u de **Gegevensbroninstellingen** opdracht in de query-editor. Zorg dat u opgeeft de referenties die u gebruiken voor Azure, dit van uw referenties voor Power BI afwijken kan.
6. Kiest u een visualisatie voor uw query en selecteer de velden voor de x-as, y-as en segmenteren dimensie.
   
    ![Schermafbeelding van de Power BI Desktop visualisatie-opties](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)
7. Uw rapport publiceren naar uw Power BI-cloud-werkruimte. Van daaruit kunt u een gesynchroniseerde versie insluiten in een andere webpagina's.
   
    ![Schermafbeelding van de Power BI Desktop, met de knop publiceren gemarkeerd](./media/app-insights-export-power-bi/publish-power-bi.png)
8. Het rapport handmatig vernieuwen met intervallen of een geplande vernieuwing op de optiepagina instellen.

### <a name="export-a-funnel"></a>Een trechter exporteren
1. [Controleer uw trechter](../azure-monitor/app/usage-funnels.md).
2. Selecteer **Power BI**.

   ![Schermafbeelding van de Power BI-knop](./media/app-insights-export-power-bi/button.png)

3. Selecteer in Power BI Desktop, **gegevens ophalen** > **lege Query**. Klik in de query-editor onder **weergave**, selecteer **geavanceerde Editor**.

   ![Schermafbeelding van de Power BI Desktop, met lege Query knop gemarkeerd](./media/app-insights-export-power-bi/blankquery.png)

   Het geëxporteerde taal voor M-script in de geavanceerde Editor plakken. 

   ![Schermafbeelding van de Power BI Desktop, met de geavanceerde Editor gemarkeerd](./media/app-insights-export-power-bi/advancedquery.png)

4. Items selecteren in de query en kiest u een visualisatie trechter.

   ![Schermafbeelding van de Power BI Desktop visualisatie-opties](./media/app-insights-export-power-bi/selectsequence.png)

5. Wijzig de titel om af te stemmen en uw rapport publiceren naar uw Power BI-cloud-werkruimte. 

   ![Schermafbeelding van de Power BI Desktop, met de titel wijzigen die zijn gemarkeerd](./media/app-insights-export-power-bi/changetitle.png)

## <a name="troubleshooting"></a>Problemen oplossen

U kunt tegenkomen fouten met betrekking tot referenties of de grootte van de gegevensset. Hier volgt informatie over wat te doen over deze fouten.

### <a name="unauthorized-401-or-403"></a>Niet-geautoriseerde (401 of 403)
Dit kan gebeuren als het vernieuwingstoken is niet bijgewerkt. Probeer de volgende stappen om te controleren of dat u nog altijd toegang:

1. Meld u aan bij de Azure-portal en controleer of dat u kunt toegang tot de resource.
2. Probeer te vernieuwen van de referenties voor het dashboard.

 Als u toegang hebt en vernieuwen van de referenties niet werkt, opent u een ondersteuningsticket.

### <a name="bad-gateway-502"></a>Ongeldige Gateway (502)
Dit wordt meestal veroorzaakt door een Analytics-query die te veel gegevens retourneert. Probeer een kleiner tijdsbereik voor de query te gebruiken. 

Als het verminderen van de gegevensset die afkomstig zijn van de Analytics-query niet voldoet aan uw vereisten, kunt u overwegen de [API](https://dev.applicationinsights.io/documentation/overview) voor het ophalen van een grotere gegevensset. Hier volgt het converteren van de uitvoer van de M-Query voor het gebruik van de API.

1. Maak een [API-sleutel](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).
2. Werk het Power BI-M-script dat u hebt geëxporteerd uit Analytics door de Azure Resource Manager-URL vervangen door de Application Insights-API.
   * Vervang  **https://management.azure.com/subscriptions/...**
   * met  **https://api.applicationinsights.io/beta/apps/...**
3. Ten slotte de referenties bijwerken naar een standaardschijf en gebruik van uw API-sleutel.

**Bestaand script**
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
**Bijgewerkte script**
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a>Over steekproeven
Afhankelijk van de hoeveelheid gegevens die door uw toepassing worden verzonden, is het raadzaam om te gebruiken van de functie voor adaptieve steekproeven, waarmee slechts een percentage van uw telemetrie wordt verzonden. Hetzelfde geldt als u de steekproeven handmatig hebt ingesteld in de SDK of bij het opnemen. [Meer informatie over steekproeven](../azure-monitor/app/sampling.md).

## <a name="power-bi-adapter-deprecated"></a>Power BI-adapter (afgeschaft)
Deze methode maakt u een voltooid dashboard van telemetrie voor u. De initiële gegevensset is vooraf gedefinieerd, maar u kunt meer gegevens toevoegen aan deze.

### <a name="get-the-adapter"></a>De adapter ophalen
1. Aanmelden bij [Power BI](https://app.powerbi.com/).
2. Open **gegevens ophalen** ![Screenshot van GetData pictogram in de linkerbenedenhoek](./media/app-insights-export-power-bi/001.png), **Services**.

    ![Schermafbeeldingen van ophalen uit Application Insights-gegevensbron](./media/app-insights-export-power-bi/002.png)

3. Selecteer **nu downloaden** onder Application Insights.

   ![Schermafbeeldingen van ophalen uit Application Insights-gegevensbron](./media/app-insights-export-power-bi/003.png)
4. Geef de details van uw Application Insights-resource, en vervolgens **aanmelden**.

    ![Schermafbeelding van ophalen uit Application Insights-gegevensbron](./media/app-insights-export-power-bi/005.png)

     Deze informatie kan worden gevonden in het deelvenster met Application Insights-overzicht:

     ![Schermafbeelding van ophalen uit Application Insights-gegevensbron](./media/app-insights-export-power-bi/004.png)

5. Open de zojuist gemaakte Application Insights Power BI-App.

6. Wacht een minuut of twee voor de gegevens worden geïmporteerd.

    ![Schermafbeelding van de Power BI-adapter](./media/app-insights-export-power-bi/010.png)

U kunt het dashboard bewerken door de Application Insights-grafieken met die van andere bronnen, en met Analytics-query's te combineren. Krijgt u meer diagrammen in de galerie met visualisatie, en elke grafiek heeft parameters die u kunt instellen.

Na de eerste import telkens het dashboard en de rapporten dagelijks bijgewerkt. U kunt het schema voor gegevensvernieuwing instellen op de gegevensset kunt beheren.

## <a name="next-steps"></a>Volgende stappen
* [Power BI - informatie](https://www.powerbi.com/learning/)
* [Analytics-zelfstudie](../azure-monitor/log-query/get-started-portal.md)

