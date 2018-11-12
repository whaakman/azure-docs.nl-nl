---
title: Azure Application Insights processen automatiseren met Microsoft Flow
description: Lees hoe u Microsoft Flow snel herhaalbare om processen te automatiseren met behulp van de Application Insights-connector kunt gebruiken.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/25/2017
ms.author: mbullwin
ms.openlocfilehash: 65909e13c75ae4d2577ea29f562b841a1eb20477
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51256422"
---
# <a name="automate-azure-application-insights-processes-with-the-connector-for-microsoft-flow"></a>Azure Application Insights processen automatiseren met de connector voor Microsoft Flow

Vind u uzelf herhaaldelijk uitvoeren van de dezelfde query's op de telemetriegegevens om te controleren of uw service goed functioneert? Bent u op zoek naar deze query's voor het vinden van trends en afwijkingen automatiseren en bouw vervolgens uw eigen werkstromen omheen? De Azure Application Insights-connector (preview) voor Microsoft Flow is het juiste hulpprogramma voor deze doeleinden.

Met deze integratie kunt kunt u veel processen automatiseren zonder één regel code te schrijven. Nadat u een stroom maken met behulp van een Application Insights-actie, wordt de stroom automatisch uw Application Insights Analytics-query uitgevoerd. 

U kunt ook aanvullende acties toevoegen. Microsoft Flow kunt u honderden acties beschikbaar. Bijvoorbeeld, kunt u Microsoft Flow automatisch een e-mailmelding verzenden of een bug in Azure DevOps maken. U kunt ook een van de vele gebruiken [sjablonen](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) die beschikbaar zijn voor de connector voor Microsoft Flow. Deze sjablonen versnellen kunt het proces voor het maken van een stroom. 

<!--The Application Insights connector also works with [Azure Power Apps](https://powerapps.microsoft.com/en-us/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/?v=17.23h). --> 

## <a name="create-a-flow-for-application-insights"></a>Een stroom maken voor Application Insights

In deze zelfstudie leert u hoe u een stroom die gebruikmaakt van het algoritme Analytics automatisch-cluster op groepskenmerken in de gegevens voor een webtoepassing maakt. De stroom worden de resultaten automatisch verzonden via e-mail, slechts één voorbeeld van hoe u kunt Microsoft Flow en Application Insights Analytics samen gebruiken. 

### <a name="step-1-create-a-flow"></a>Stap 1: Een stroom maken
1. Aanmelden bij [Microsoft Flow](https://flow.microsoft.com), en selecteer vervolgens **mijn stromen**.
1. Klik op **een stroom maken met een lege App**.

### <a name="step-2-create-a-trigger-for-your-flow"></a>Stap 2: Een trigger voor uw stroom maken
1. Selecteer **planning**, en selecteer vervolgens **planning - terugkeerpatroon**.
1. In de **frequentie** Schakel **dag**, en klik in de **Interval** Voer **1**.

    ![In het dialoogvenster van Microsoft Flow-trigger](./media/app-insights-automate-with-flow/flow1.png)


### <a name="step-3-add-an-application-insights-action"></a>Stap 3: Een Application Insights-actie toevoegen
1. Klik op **nieuwe stap**, en klik vervolgens op **een actie toevoegen**.
1. Zoeken naar **Azure Application Insights**.
1. Klik op **Preview query uitvoeren op Azure Application Insights - Analytics visualiseren**.

    ![Analytics-query-venster worden uitgevoerd](./media/app-insights-automate-with-flow/flow2.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a>Stap 4: Verbinding maken met een Application Insights-resource

Als u wilt deze stap hebt voltooid, moet u een toepassings-ID en een API-sleutel voor uw resource. U kunt ze ophalen vanuit de Azure-portal, zoals wordt weergegeven in het volgende diagram:

![Toepassings-ID in de Azure-portal](./media/app-insights-automate-with-flow/appid.png) 

- Geef een naam voor de verbinding, samen met de toepassing-ID en API-sleutel.

    ![Venster van de verbinding met Microsoft Flow](./media/app-insights-automate-with-flow/flow3.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>Stap 5: Geef het Analytics-query's en grafiek-type
In dit voorbeeld van een query selecteert de mislukte aanvragen in de laatste dag en verbindt deze met uitzonderingen die als onderdeel van de bewerking opgetreden. Analytics correleert toe op basis van de operation_Id-id. De query vervolgens de resultaten met behulp van het algoritme autocluster segmenten. 

Wanneer u uw eigen query's maakt, controleert u of dat ze goed in Analytics werken voordat u deze aan uw stroom toevoegt.

- De volgende analytische query toevoegen en selecteer vervolgens het grafiektype voor HTML-tabel. 

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
    
    ![Analytics-queryvenster configuratie](./media/app-insights-automate-with-flow/flow4.png)

### <a name="step-6-configure-the-flow-to-send-email"></a>Stap 6: Configureer de stroom om e-mail te verzenden

1. Klik op **nieuwe stap**, en klik vervolgens op **een actie toevoegen**.
1. Zoeken naar **Office 365 Outlook**.
1. Klik op **Office 365 Outlook - een e-mail verzenden**.

    ![Selectievenster voor Office 365 Outlook](./media/app-insights-automate-with-flow/flow2b.png)

1. In de **een e-mailbericht verzenden** venster de volgende handelingen uit:

   a. Typ het e-mailadres van de ontvanger.

   b. Typ een onderwerp in voor het e-mailbericht.

   c. Klik ergens in de **hoofdtekst** vak en selecteer vervolgens in het menu voor dynamische inhoud die wordt geopend aan de rechterkant, **hoofdtekst**.

   d. Klik op **geavanceerde opties weergeven**.

    ![Office 365 Outlook-configuratie](./media/app-insights-automate-with-flow/flow5.png)

1. Op het menu voor dynamische inhoud het volgende doen:

    a. Selecteer **Bijlagenaam**.

    b. Selecteer **inhoud van de bijlage**.
    
    c. In de **Is HTML** Schakel **Ja**.

    ![Venster voor Office 365 e-configuratie](./media/app-insights-automate-with-flow/flow7.png)

### <a name="step-7-save-and-test-your-flow"></a>Stap 7: Opslaan en uw stroom testen
- In de **Stroomnaam** vak, een naam voor uw stroom toevoegen en klik vervolgens op **stroom maken**.

    ![Venster stromen maken](./media/app-insights-automate-with-flow/flow8.png)

U kunt wachten op de trigger voor deze actie niet uitvoeren of kunt u direct door de stroom uitvoeren [waarop de trigger wordt uitgevoerd op aanvraag](https://flow.microsoft.com/blog/run-now-and-six-more-services/).

Wanneer de stroom wordt uitgevoerd, wordt de ontvangers die u hebt opgegeven in de lijst met e-mailbericht een e-mailbericht dat lijkt op het volgende:

![Voorbeeldbericht](./media/app-insights-automate-with-flow/flow9.png)


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het maken van [Analytics-query's](../log-analytics/query-language/get-started-queries.md).
- Meer informatie over [Microsoft Flow](https://ms.flow.microsoft.com).



<!--Link references-->





