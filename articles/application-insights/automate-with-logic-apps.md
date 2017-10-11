---
title: Azure Application Insights-processen automatiseren met behulp van Logic Apps.
description: Meer informatie over hoe u snel herhaalbare processen kunt automatiseren door de Application Insights-connector toe te voegen aan uw logische app.
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: bwren
ms.openlocfilehash: 36df5bc0a019f4197d40fd6fa5a2a9957820c8b4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="automate-application-insights-processes-by-using-logic-apps"></a>Application Insights-processen automatiseren met behulp van Logic Apps

Vindt u uzelf herhaaldelijk de dezelfde query's uitvoeren op de telemetriegegevens om te controleren of uw service goed werkt? Zoekt u voor het automatiseren van deze query's voor het vinden van trends en afwijkingen en vervolgens uw eigen werkstromen omheen te bouwen? De Azure Application Insights-connector (preview) voor Logic Apps is het juiste programma voor dit doel.

Met deze integratie kunt u meerdere processen automatiseren zonder een één regel code te schrijven. U kunt een logische app maken met de Application Insights-connector voor het snel een Application Insights-proces te automatiseren. 

U kunt ook aanvullende acties toevoegen. De functie Logic Apps van Azure App Service maakt honderden acties beschikbaar. Bijvoorbeeld, met behulp van een logische app, kunt u automatisch een e-mailmelding verzenden of maken van een bug in Visual Studio Team Services. U kunt ook een van de vele beschikbaar gebruiken [sjablonen](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) om u te helpen bij het maken van uw logische app versnellen. 

## <a name="create-a-logic-app-for-application-insights"></a>Een logische app maken voor Application Insights

In deze zelfstudie leert u hoe u een logische app die gebruikmaakt van het algoritme Analytics autocluster kenmerken in de gegevens voor een webtoepassing maken. De stroom verzendt de resultaten automatisch via e-mail, slechts één voorbeeld van hoe u kunt Application Insights Analytics en Logic Apps samen gebruiken. 

### <a name="step-1-create-a-logic-app"></a>Stap 1: Een logische app maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. In de **nieuw** deelvenster **Web en mobiel**, en selecteer vervolgens **logische App**.

    ![Nieuwe logische app venster](./media/automate-with-logic-apps/logicapp1.png)

### <a name="step-2-create-a-trigger-for-your-logic-app"></a>Stap 2: Een trigger voor uw logische app maken
1. In de **Logic App-ontwerper** venster onder **beginnen met een algemene trigger**, selecteer **terugkeerpatroon**.

    ![Logic App Designer-venster](./media/automate-with-logic-apps/logicapp2.png)

2. In de **frequentie** de optie **dag** en klikt u op de **Interval** in het vak **1**.

    ![Logic App-ontwerper "Recurrence" venster](./media/automate-with-logic-apps/step2b.png)

### <a name="step-3-add-an-application-insights-action"></a>Stap 3: Een Application Insights-actie toevoegen
1. Klik op **nieuwe stap**, en klik vervolgens op **een actie toevoegen**.

2. In de **kiest u een actie** zoekvak, type **Azure Application Insights**.

3. Onder **acties**, klikt u op **Azure Application Insights – visualiseren Analytics query Preview**.

    ![Logic App-ontwerper 'Kies een actie' venster](./media/automate-with-logic-apps/flow2.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a>Stap 4: Verbinding maken met een Application Insights-resource

Deze stap voltooien, moet u een toepassings-ID en een API-sleutel voor uw resource. U kunt ze ophalen uit de Azure-portal, zoals wordt weergegeven in het volgende diagram:

![Toepassings-ID in de Azure portal](./media/automate-with-logic-apps/appid.png) 

Geef een naam voor de verbinding, de toepassings-ID en de API-sleutel.

![Venster Logic App-ontwerper stroom-verbinding](./media/automate-with-logic-apps/flow3.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>Stap 5: Geef het Analytics-query en grafiek type
In het volgende voorbeeld wordt de query selecteert de mislukte aanvragen binnen de laatste dag en verbindt deze met uitzonderingen die als onderdeel van de bewerking opgetreden. Analytics correleert de mislukte aanvragen, op basis van de operation_Id-id. De query vervolgens de resultaten met behulp van de algoritme autocluster segmenten. 

Wanneer u uw eigen query's maakt, moet u controleren of ze correct in Analytics werken voordat u deze aan uw stroom toevoegen.

1. In de **Query** Voeg de volgende Analytics-query: 

    ```
    requests
    | where timestamp > ago(1d)
    | where success == "False"
    | project name, operation_Id
    | join ( exceptions
        | project problemId, outerMessage, operation_Id
    ) on operation_Id
    | evaluate autocluster()
    ```

2. In de **grafiektype** de optie **HTML-tabel**.

    ![De configuratie queryvenster Analytics](./media/automate-with-logic-apps/flow4.png)

### <a name="step-6-configure-the-logic-app-to-send-email"></a>Stap 6: Configureer de logische app voor het verzenden van e-mail

1. Klik op **nieuwe stap**, en selecteer vervolgens **een actie toevoegen**.

2. Typ in het zoekvak **Outlook van Office 365**.

3. Klik op **Outlook in Office 365 – stuurt u een e-mailadres**.

    ![Selectie van Office 365 Outlook](./media/automate-with-logic-apps/flow2b.png)

4. In de **e-mailbericht verzenden** venster de volgende handelingen uit:

   a. Typ het e-mailadres van de ontvanger.

   b. Typ een onderwerp in voor het e-mailbericht.

   c. Klik ergens in de **hoofdtekst** vak en selecteer vervolgens in het dynamische inhoud menu dat op het recht verschijnt **hoofdtekst**.

   d. Klik op **geavanceerde opties weergeven**.

      ![Outlook-configuratie voor Office 365](./media/automate-with-logic-apps/flow5.png)

5. In het menu van dynamische inhoud het volgende doen:

    a. Selecteer **Bijlagenaam**.

    b. Selecteer **bijlage inhoud**.
    
    c. In de **Is HTML** de optie **Ja**.

      ![Configuratiescherm voor Office 365-e-mailadres](./media/automate-with-logic-apps/flow7.png)

### <a name="step-7-save-and-test-your-logic-app"></a>Stap 7: Opslaan en uw logische app testen
* Klik op **opslaan** uw wijzigingen op te slaan.

U kunt wachten op de trigger uit te voeren van de logische app, of u kunt de logische app onmiddellijk uitvoeren door te selecteren **uitvoeren**.

![Scherm van logische app maken](./media/automate-with-logic-apps/step7.png)

Wanneer uw logische app wordt uitgevoerd, is de ontvangers die u hebt opgegeven in de lijst met e-mailbericht ontvangt een e-mailbericht dat op het volgende lijkt:

![Logic app e-mailbericht](./media/automate-with-logic-apps/flow9.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het maken van [analysequery's](app-insights-analytics-using.md).
- Meer informatie over [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).



<!--Link references-->





