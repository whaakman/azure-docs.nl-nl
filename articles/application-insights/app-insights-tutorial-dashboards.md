---
title: Aangepaste dashboards maken in Azure Application Insights | Microsoft Docs
description: Zelfstudie voor het maken van aangepaste KPI dashboards met behulp van Azure Application Insights.
keywords: 
services: application-insights
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/20/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 0d2f98ca2fb39289b2916ddd24590924856507d6
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="create-custom-kpi-dashboards-using-azure-application-insights"></a>Aangepaste KPI dashboards maken met Azure Application Insights

U kunt meerdere dashboards maken in de Azure portal dat elke opnemen tegels gegevens uit meerdere Azure-resources te visualiseren via verschillende resourcegroepen en abonnementen.  U kunt vastmaken verschillende grafieken en weergaven van Azure Application Insights om aangepaste dashboards die u met een volledig overzicht van de status en prestaties van uw toepassing te maken.  Deze zelfstudie helpt u bij het maken van een aangepast dashboard met meerdere typen gegevens en visualisaties van Azure Application Insights.  Procedures voor:

> [!div class="checklist"]
> * Een aangepast dashboard maken in Azure
> * Een tegel in de tegel galerie toevoegen
> * Standaard metrische gegevens in Application Insights toevoegen aan het dashboard 
> * Een aangepaste metrische grafiek Application Insights toevoegen aan het dashboard
> * De resultaten van een Analytics query toevoegen aan het dashboard 



## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- Een .NET-toepassing in Azure implementeert en [inschakelen van de Application Insights-SDK](app-insights-asp-net.md). 

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Aanmelden bij de Azure portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-new-dashboard"></a>Een nieuw dashboard maken
Een dashboard kan resources van meerdere toepassingen, resourcegroepen en abonnementen bevatten.  De zelfstudie begint met het maken van een nieuw dashboard voor uw toepassing.  

2.  Selecteer in het hoofdvenster van de portal **nieuwe dashboard**.

    ![Nieuwe dashboard](media/app-insights-tutorial-dashboards/new-dashboard.png)

3. Typ een naam voor het dashboard.
4. Bekijk de **tegel galerie** voor een verscheidenheid aan tegels die u aan uw dashboard toevoegen kunt.  U kunt naast het toevoegen van tegels uit de galerie grafieken en andere weergaven rechtstreeks vanuit Application Insights naar het dashboard vastmaken.
5. Zoek de **Markdown** tegel en sleep deze naar uw dashboard.  Deze tegel kunt u tekst opgemaakt in markdown dit ideaal is voor de beschrijvende tekst toe te voegen aan uw dashboard toe te voegen.
6. Tekst toevoegen aan de tegel eigenschappen en deze op het canvas dashboard vergroten of verkleinen.
    
    ![Markdown-tegel bewerken](media/app-insights-tutorial-dashboards/edit-markdown.png)

6. Klik op **gedaan aanpassen** aan de bovenkant van het scherm om af te sluiten van de tegel aanpassing modus en vervolgens **wijzigingen publiceren** uw wijzigingen op te slaan.

    ![Dashboard met markdown-tegel](media/app-insights-tutorial-dashboards/dashboard-01.png)


## <a name="add-health-overview"></a>Overzicht van status toevoegen
Een dashboard met alleen statische tekst is niet erg interessante, dus een tegel van Application Insights om weer te geven informatie over uw toepassing toevoegen.  U kunt Application Insights tegels toevoegen uit de galerie tegel of u ze rechtstreeks vanuit Application Insights schermen kunt vastmaken.  Hiermee kunt u voor het configureren van grafieken en weergaven die u al bekend met bent voordat ze aan uw dashboard vastmaken.  Starten door het overzicht van de standaard status voor uw toepassing toe te voegen.  Dit is geen configuratie vereist en minimale aanpassing kan in het dashboard.


1. Selecteer **Application Insights** in het menu van Azure en selecteer vervolgens uw toepassing.
2. In de **overzicht tijdlijn**, selecteer het snelmenu en klikt u op **vastmaken aan dashboard**.  Hiermee wordt de tegel toegevoegd aan het laatste dashboard die u bekeek.  

    ![Tijdlijn voor pincode-overzicht](media/app-insights-tutorial-dashboards/pin-overview-timeline.png)
 
3. Klik boven aan het scherm op **weergave dashboard** om terug te keren naar het dashboard.
4. De tijdlijn overzicht wordt nu toegevoegd aan uw dashboard.  Klik en sleep deze naar de gewenste positie en klik vervolgens op **gedaan aanpassen** en **wijzigingen publiceren**.  Uw dashboard heeft nu een tegel met enkele nuttige informatie.

    ![Dashboard met overzicht tijdlijn](media/app-insights-tutorial-dashboards/dashboard-02.png)



## <a name="add-custom-metric-chart"></a>Aangepaste metrische grafiek toevoegen
De **metrische gegevens** deelvenster kunt u een waarde die is verzameld door Application Insights gedurende een periode met optionele filters en groeperen in een grafiek.  Als alles in Application Insights, kunt u deze grafiek toevoegen aan het dashboard.  Dit vereist dat u enige aanpassing eerst doen.

1. Selecteer **Application Insights** in het menu van Azure en selecteer vervolgens uw toepassing.
1. Selecteer **metrische gegevens**.  
2. Een lege grafiek al zijn gemaakt en u wordt gevraagd een metriek toevoegen.  Een metriek toevoegen aan de grafiek en optioneel een filter en een groep toevoegen.  Het volgende voorbeeld toont het aantal serververzoeken, gegroepeerd per geslaagd.  Hiermee geeft u een actieve weergave van geslaagde en mislukte aanvragen.

    ![Metrische gegevens toevoegen](media/app-insights-tutorial-dashboards/metrics-chart.png)

4. Selecteer het snelmenu voor de grafiek en selecteer **vastmaken aan dashboard**.  De weergave wordt toegevoegd aan het laatste dashboard waarmee u werkt.

    ![Pincode metrische grafiek](media/app-insights-tutorial-dashboards/pin-metrics-chart.png)

3. Klik boven aan het scherm op **weergave dashboard** om terug te keren naar het dashboard.

4. De tijdlijn overzicht wordt nu toegevoegd aan uw dashboard.  Klik en sleep deze naar de gewenste positie en klik vervolgens op **gedaan aanpassen** en vervolgens **wijzigingen publiceren**. 

    ![dashboard met metrische gegevens](media/app-insights-tutorial-dashboards/dashboard-03.png)


## <a name="metrics-explorer"></a>Metrics Explorer
**Metrics Explorer** is vergelijkbaar met metrische gegevens Hoewel kan aanzienlijk meer aanpassing wanneer toegevoegd aan het dashboard.  Waarmee u kunt metrische gegevens van uw grafiek is afhankelijk van uw eigen voorkeuren en vereisten.

1. Selecteer **Application Insights** in het menu van Azure en selecteer vervolgens uw toepassing.
1. Selecteer **Metrics Explorer**. 
2. Klik op de grafiek bewerken en selecteer een of meer waarden en eventueel een gedetailleerde configuratie.  Het voorbeeld wordt een lijndiagram gemiddelde pagina reactietijden bijhouden.
3. Klik op het Punaisepictogram in de rechterbovenhoek rechts van de grafiek toevoegen aan uw dashboard en sleep deze naar de gewenste positie.

    ![Metrics Explorer](media/app-insights-tutorial-dashboards/metrics-explorer.png)

4. De tegel Metrics Explorer kunt meer aanpassing zodra deze toegevoegd aan het dashboard.  Met de rechtermuisknop op de tegel en selecteer **titel bewerken** een aangepaste titel toevoegen.  Opwekken en andere aanpassingen maken als u wilt.

    ![Dashboard met metrics explorer](media/app-insights-tutorial-dashboards/dashboard-04a.png)

5. U hebt nu de Metrics Explorer-grafiek toegevoegd aan uw dashboard.

    ![Dashboard met metrics explorer](media/app-insights-tutorial-dashboards/dashboard-04.png)

## <a name="add-analytics-query"></a>Analytics-query toevoegen
Azure Application Insights Analytics biedt een uitgebreide querytaal waarmee u voor het analyseren van alle gegevens verzameld Application Insights.  Net als andere weergaven en grafieken kunt u de uitvoer van een Analytics query toevoegen aan uw dashboard.   

Aangezien Azure toepassingen Insights Analytics een afzonderlijke service is, moet u uw dashboard voor het opnemen van een query Analytics te delen.  Wanneer u een Azure-dashboard deelt, kunt u deze publiceren als een Azure resource die u het beschikbaar voor andere gebruikers en bronnen maken kunt.  

1. Aan de bovenkant van het scherm dashboard, klikt u op **Share**.

    ![dashboard publiceren](media/app-insights-tutorial-dashboards/publish-dashboard.png)

2. Houd de **dashboardnaam** dezelfde en selecteer de **abonnementsnaam** om het dashboard te delen.  Klik op **Publish**.  Het dashboard is nu beschikbaar voor andere services en abonnementen.  Desgewenst kunt u specifieke gebruikers die toegang tot het dashboard hebben moeten definiëren.
1. Selecteer **Application Insights** in het menu van Azure en selecteer vervolgens uw toepassing.
2. Klik op **Analytics** aan de bovenkant van het scherm de Analytics-portal openen.

    ![Analytics start](media/app-insights-tutorial-dashboards/start-analytics.png)

3. Typ de volgende query de bovenste 10 meest aangevraagde pagina's en het aantal aanvragen retourneert:

    ```
    requests
    | summarize count() by name
    | sort by count_ desc
    | take 10 
    ```

4. Klik op **gaat** valideren van de resultaten van de query.
5. Klik op het Punaisepictogram en selecteer de naam van uw dashboard.  De reden dat deze optie u een dashboard in tegenstelling tot de vorige stappen heeft, waarbij de laatste dashboard gebruikt selecteren is omdat de console Analytics een afzonderlijke service is en moet in alle beschikbare gedeelde dashboards te selecteren.

    ![Pincode Analytics-query](media/app-insights-tutorial-dashboards/analytics-pin.png)

5. Voordat u teruggaan naar het dashboard, toevoegen van een andere query, maar deze tijd weergeven als een diagram zodat u de verschillende manieren voor het visualiseren van een Analytics-query in een dashboard zien.  Beginnen met de volgende query overzicht van de top 10 bewerkingen met de meeste uitzonderingen.

    ```
    exceptions
    | summarize count() by operation_Name
    | sort by count_ desc
    | take 10 
    ```

6. Selecteer **grafiek** wijzigen en vervolgens een **Doughnut** voor het visualiseren van de uitvoer.

    ![Grafiek](media/app-insights-tutorial-dashboards/analytics-chart.png)

6. Klik op het pictogram pincode om de grafiek aan uw dashboard vast en deze tijd, selecteer de koppeling om te retourneren aan uw dashboard.
4. De resultaten van de query's worden nu toegevoegd aan uw dashboard in de indeling die u hebt geselecteerd.  Klik en sleep naar de gewenste positie en klik vervolgens op **bewerken voltooid**.
5. Met de rechtermuisknop op elk van de tegels en selecteer **titel bewerken** om hem een beschrijvende titel.

    ![Dashboard met Analytics](media/app-insights-tutorial-dashboards/dashboard-05.png)

5. Klik op **wijzigingen publiceren** opslaan van wijzigingen aan uw dashboard die nu tal van grafieken en visualisaties van Application Insights bevat.


## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt geleerd hoe u aangepaste dashboards maken, hebt u een overzicht van de rest van de Application Insights-documentatie met inbegrip van een casestudy.

> [!div class="nextstepaction"]
> [Diepe diagnostische gegevens](app-insights-devops.md)