---
title: Azure Application Insights-processen automatiseren met behulp van Logic Apps.
description: Meer informatie over hoe u snel herhaalbare processen kunt automatiseren door de Application Insights-connector toe te voegen aan uw logische app.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/29/2017
ms.author: mbullwin
ms.openlocfilehash: 44e654202cdaebedab82f40f154c1483b7e979ae
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52725689"
---
# <a name="automate-application-insights-processes-by-using-logic-apps"></a>Application Insights-processen automatiseren met behulp van Logic Apps

Vind u uzelf herhaaldelijk uitvoeren van de dezelfde query's op de telemetriegegevens om te controleren of uw service goed functioneert? Bent u op zoek naar deze query's voor het vinden van trends en afwijkingen automatiseren en bouw vervolgens uw eigen werkstromen omheen? De Azure Application Insights-connector (preview) voor Logic Apps is de juiste hulpprogramma voor dit doel.

Met deze integratie kunt kunt u veel processen automatiseren zonder één regel code te schrijven. U kunt een logische app maken met de Application Insights-connector voor het snel een Application Insights-proces te automatiseren. 

U kunt ook aanvullende acties toevoegen. De functie logische Apps van Azure App Service maakt honderden acties beschikbaar. Bijvoorbeeld, met behulp van een logische app, kunt u automatisch een e-mailmelding verzenden of een bug in Azure DevOps maken. U kunt ook een van de vele beschikbare gebruiken [sjablonen](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) om u te helpen versnellen van het proces voor het maken van uw logische app. 

## <a name="create-a-logic-app-for-application-insights"></a>Een logische app maken voor Application Insights

In deze zelfstudie leert u hoe u een logische app die gebruikmaakt van de algoritme Analytics autocluster groepskenmerken in de gegevens voor een webtoepassing maken. De stroom worden de resultaten automatisch verzonden via e-mail, slechts één voorbeeld van hoe u Application Insights Analytics en Logic Apps samen kunt. 

### <a name="step-1-create-a-logic-app"></a>Stap 1: Een logische app maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Klik op **een resource maken**, selecteer **Web en mobiel**, en selecteer vervolgens **logische App**.

    ![Nieuw venster van de logische app](./media/automate-with-logic-apps/logicapp1.png)

### <a name="step-2-create-a-trigger-for-your-logic-app"></a>Stap 2: Een trigger voor uw logische app maken
1. In de **Logic App Designer** venster onder **beginnen met een algemene trigger**, selecteer **terugkeerpatroon**.

    ![Logic App Designer-venster](./media/automate-with-logic-apps/logicapp2.png)

1. In de **frequentie** Schakel **dag** en klikt u op de **Interval** in het vak **1**.

    ![Logic App Designer "Terugkeerpatroon" venster](./media/automate-with-logic-apps/step2b.png)

### <a name="step-3-add-an-application-insights-action"></a>Stap 3: Een Application Insights-actie toevoegen
1. Klik op **nieuwe stap**, en klik vervolgens op **een actie toevoegen**.

1. In de **een actie kiezen** zoekvak, type **Azure Application Insights**.

1. Onder **acties**, klikt u op **Preview query uitvoeren op Azure Application Insights - Analytics visualiseren**.

    ![Logic App Designer 'Kies een actie' venster](./media/automate-with-logic-apps/flow2.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a>Stap 4: Verbinding maken met een Application Insights-resource

Als u wilt deze stap hebt voltooid, moet u een toepassings-ID en een API-sleutel voor uw resource. U kunt ze ophalen vanuit de Azure-portal, zoals wordt weergegeven in het volgende diagram:

![Toepassings-ID in de Azure-portal](./media/automate-with-logic-apps/appid.png) 

Geef een naam voor de verbinding, de toepassings-ID en de API-sleutel.

![Logic App Designer stroom verbindingsvenster](./media/automate-with-logic-apps/flow3.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>Stap 5: Geef het Analytics-query's en grafiek-type
In het volgende voorbeeld wordt de query selecteert de mislukte aanvragen in de laatste dag en verbindt deze met uitzonderingen die als onderdeel van de bewerking opgetreden. Analytics overeenkomt met de mislukte aanvragen, op basis van de operation_Id-id. De query vervolgens de resultaten met behulp van het algoritme autocluster segmenten. 

Wanneer u uw eigen query's maakt, controleert u of dat ze goed in Analytics werken voordat u deze aan uw stroom toevoegt.

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

1. In de **grafiektype** Schakel **Html-tabel**.

    ![Analytics-queryvenster configuratie](./media/automate-with-logic-apps/flow4.png)

### <a name="step-6-configure-the-logic-app-to-send-email"></a>Stap 6: De logische app configureren voor e-mailbericht verzenden

1. Klik op **nieuwe stap**, en selecteer vervolgens **een actie toevoegen**.

1. Typ in het zoekvak **Office 365 Outlook**.

1. Klik op **Office 365 Outlook - een e-mail verzenden**.

    ![Office 365 Outlook-selectie](./media/automate-with-logic-apps/flow2b.png)

1. In de **een e-mailbericht verzenden** venster de volgende handelingen uit:

   a. Typ het e-mailadres van de ontvanger.

   b. Typ een onderwerp in voor het e-mailbericht.

   c. Klik ergens in de **hoofdtekst** vak en selecteer vervolgens in het menu voor dynamische inhoud die wordt geopend aan de rechterkant, **hoofdtekst**.

   d. Klik op **geavanceerde opties weergeven**.

      ![Office 365 Outlook-configuratie](./media/automate-with-logic-apps/flow5.png)

1. Op het menu voor dynamische inhoud het volgende doen:

    a. Selecteer **Bijlagenaam**.

    b. Selecteer **inhoud van de bijlage**.
    
    c. In de **Is HTML** Schakel **Ja**.

      ![Configuratiescherm voor Office 365-e-mailadres](./media/automate-with-logic-apps/flow7.png)

### <a name="step-7-save-and-test-your-logic-app"></a>Stap 7: Opslaan en testen van uw logische app
* Klik op **opslaan** uw wijzigingen op te slaan.

U kunt wachten op de trigger om uit te voeren van de logische app of kunt u de logische app direct uitvoeren door het selecteren van **uitvoeren**.

![Scherm voor logische app maken](./media/automate-with-logic-apps/step7.png)

Wanneer uw logische app wordt uitgevoerd, is de ontvangers die u hebt opgegeven in de lijst met e-mailbericht ontvangt een e-mailbericht dat lijkt op het volgende:

![Logische app e-mailbericht](./media/automate-with-logic-apps/flow9.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het maken van [Analytics-query's](../log-analytics/query-language/get-started-queries.md).
- Meer informatie over [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).



<!--Link references-->





