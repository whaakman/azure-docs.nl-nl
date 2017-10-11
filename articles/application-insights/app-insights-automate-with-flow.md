---
title: Azure Application Insights-processen met Microsoft-Flow automatiseren
description: Meer informatie over hoe u Microsoft Flow snel herhaalbare processen automatiseren met behulp van de Application Insights-connector kunt gebruiken.
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/25/2017
ms.author: bwren
ms.openlocfilehash: 510f4f284bbd0dbe4171896899f7ade7dee19e39
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="automate-azure-application-insights-processes-with-the-connector-for-microsoft-flow"></a>Azure Application Insights-processen met de connector voor Microsoft-Flow automatiseren

Vindt u uzelf herhaaldelijk de dezelfde query's uitvoeren op de telemetriegegevens om te controleren of uw service goed werkt? Zoekt u voor het automatiseren van deze query's voor het vinden van trends en afwijkingen en vervolgens uw eigen werkstromen omheen te bouwen? De Azure Application Insights-connector (preview) voor Microsoft-Flow is de juiste hulpprogramma voor deze doeleinden.

Met deze integratie kunt u nu meerdere processen automatiseren zonder een één regel code te schrijven. Nadat u een stroom met behulp van een actie Application Insights hebt gemaakt, wordt uw Application Insights Analytics query automatisch uitgevoerd door de stroom. 

U kunt ook aanvullende acties toevoegen. Microsoft-Flow maakt honderden acties beschikbaar. U kunt bijvoorbeeld Microsoft Flow automatisch een e-mailmelding verzenden of maken van een bug in Visual Studio Team Services. U kunt ook een van de vele gebruiken [sjablonen](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) die beschikbaar zijn voor de connector voor Microsoft-Flow. Deze sjablonen wordt het proces van het maken van een stroom versnellen. 

<!--The Application Insights connector also works with [Azure Power Apps](https://powerapps.microsoft.com/en-us/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/?v=17.23h). --> 

## <a name="create-a-flow-for-application-insights"></a>Maken van een stroom voor Application Insights

In deze zelfstudie leert u hoe u een stroom die gebruikmaakt van het algoritme Analytics auto-cluster een Groepkenmerken in de gegevens voor een webtoepassing maakt. De stroom verzendt de resultaten automatisch via e-mail, slechts één voorbeeld van hoe u kunt Microsoft Flow en Application Insights Analytics samen gebruiken. 

### <a name="step-1-create-a-flow"></a>Stap 1: Een stroom maken
1. Aanmelden bij [Microsoft Flow](http://flow.microsoft.com), en selecteer vervolgens **mijn loopt**.
2. Klik op **maken van een stroom van leeg**.

### <a name="step-2-create-a-trigger-for-your-flow"></a>Stap 2: Maak een trigger voor uw flow
1. Selecteer **planning**, en selecteer vervolgens **schema - terugkeerpatroon**.
2. In de **frequentie** de optie **dag**, en in de **Interval** Voer **1**.

    ![Dialoogvenster voor Microsoft-Flow-trigger](./media/app-insights-automate-with-flow/flow1.png)


### <a name="step-3-add-an-application-insights-action"></a>Stap 3: Een Application Insights-actie toevoegen
1. Klik op **nieuwe stap**, en klik vervolgens op **een actie toevoegen**.
2. Zoeken naar **Azure Application Insights**.
3. Klik op **Azure Application Insights – visualiseren Analytics query Preview**.

    ![Analytics query-venster uitvoeren](./media/app-insights-automate-with-flow/flow2.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a>Stap 4: Verbinding maken met een Application Insights-resource

Deze stap voltooien, moet u een toepassings-ID en een API-sleutel voor uw resource. U kunt ze ophalen uit de Azure-portal, zoals wordt weergegeven in het volgende diagram:

![Toepassings-ID in de Azure portal](./media/app-insights-automate-with-flow/appid.png) 

- Geef een naam voor de verbinding, samen met de toepassing-ID en API-sleutel.

    ![Venster van de verbinding met Microsoft-Flow](./media/app-insights-automate-with-flow/flow3.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>Stap 5: Geef het Analytics-query en grafiek type
Deze voorbeeldquery selecteert de mislukte aanvragen binnen de laatste dag en ze correleert met uitzonderingen die als onderdeel van de bewerking opgetreden. Analytics correleert toe op basis van de operation_Id-id. De query vervolgens de resultaten met behulp van de algoritme autocluster segmenten. 

Wanneer u uw eigen query's maakt, moet u controleren of ze correct in Analytics werken voordat u deze aan uw stroom toevoegen.

- De volgende Analytics query toevoegen en selecteer vervolgens het grafiektype voor HTML-tabel. 

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
    
    ![De configuratie queryvenster Analytics](./media/app-insights-automate-with-flow/flow4.png)

### <a name="step-6-configure-the-flow-to-send-email"></a>Stap 6: De stroom voor het verzenden van e-mail configureren

1. Klik op **nieuwe stap**, en klik vervolgens op **een actie toevoegen**.
2. Zoeken naar **OfficeOutlook 365**.
3. Klik op **Outlook in Office 365 – stuurt u een e-mailadres**.

    ![Selectievenster voor Office 365 Outlook](./media/app-insights-automate-with-flow/flow2b.png)

4. In de **e-mailbericht verzenden** venster de volgende handelingen uit:

   a. Typ het e-mailadres van de ontvanger.

   b. Typ een onderwerp in voor het e-mailbericht.

   c. Klik ergens in de **hoofdtekst** vak en selecteer vervolgens in het dynamische inhoud menu dat op het recht verschijnt **hoofdtekst**.

   d. Klik op **geavanceerde opties weergeven**.

    ![Outlook-configuratie voor Office 365](./media/app-insights-automate-with-flow/flow5.png)

5. In het menu van dynamische inhoud het volgende doen:

    a. Selecteer **Bijlagenaam**.

    b. Selecteer **bijlage inhoud**.
    
    c. In de **Is HTML** de optie **Ja**.

    ![Venster van Office 365 e-configuratie](./media/app-insights-automate-with-flow/flow7.png)

### <a name="step-7-save-and-test-your-flow"></a>Stap 7: Opslaan en testen van uw flow
- In de **stroom naam** vak en klik vervolgens op toevoegen van een naam voor uw flow **stroom maken**.

    ![Venster van de stroom maken](./media/app-insights-automate-with-flow/flow8.png)

U kunt wachten op de trigger wordt met deze actie worden uitgevoerd, of u kunt de stroom onmiddellijk via uitvoeren [de trigger op verzoek uitgevoerd](https://flow.microsoft.com/blog/run-now-and-six-more-services/).

Wanneer de stroom wordt uitgevoerd, wordt de ontvangers die u hebt opgegeven in de lijst met e-mailadres een e-mailbericht dat op het volgende lijkt:

![Voorbeeldbericht](./media/app-insights-automate-with-flow/flow9.png)


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het maken van [analysequery's](app-insights-analytics-using.md).
- Meer informatie over [Microsoft Flow](https://ms.flow.microsoft.com).



<!--Link references-->





