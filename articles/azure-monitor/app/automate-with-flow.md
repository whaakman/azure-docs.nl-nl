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
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: mbullwin
ms.openlocfilehash: cbb9cceef68febec0af991c3d8d5df3e3404f1c6
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57834047"
---
# <a name="automate-azure-application-insights-processes-with-the-connector-for-microsoft-flow"></a>Azure Application Insights processen automatiseren met de connector voor Microsoft Flow

Vind u uzelf herhaaldelijk uitvoeren van de dezelfde query's op de telemetriegegevens om te controleren of uw service goed functioneert? Bent u op zoek naar deze query's voor het vinden van trends en afwijkingen automatiseren en bouw vervolgens uw eigen werkstromen omheen? De Azure Application Insights-connector voor Microsoft Flow is het juiste hulpprogramma voor deze doeleinden.

Met deze integratie kunt kunt u veel processen automatiseren zonder één regel code te schrijven. Nadat u een stroom maken met behulp van een Application Insights-actie, wordt de stroom automatisch uw Application Insights Analytics-query uitgevoerd. 

U kunt ook aanvullende acties toevoegen. Microsoft Flow kunt u honderden acties beschikbaar. Bijvoorbeeld, kunt u Microsoft Flow automatisch een e-mailmelding verzenden of een bug in Azure DevOps maken. U kunt ook een van de vele gebruiken [sjablonen](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) die beschikbaar zijn voor de connector voor Microsoft Flow. Deze sjablonen versnellen kunt het proces voor het maken van een stroom. 

<!--The Application Insights connector also works with [Azure Power Apps](https://powerapps.microsoft.com/en-us/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/?v=17.23h). --> 

## <a name="create-a-flow-for-application-insights"></a>Een stroom maken voor Application Insights

In deze zelfstudie leert u hoe u een stroom die gebruikmaakt van het algoritme Analytics automatisch-cluster op groepskenmerken in de gegevens voor een webtoepassing maakt. De stroom worden de resultaten automatisch verzonden via e-mail, slechts één voorbeeld van hoe u kunt Microsoft Flow en Application Insights Analytics samen gebruiken. 

### <a name="step-1-create-a-flow"></a>Stap 1: Stroom maken
1. Aanmelden bij [Microsoft Flow](https://flow.microsoft.com), en selecteer vervolgens **mijn stromen**.
2. Klik op **nieuw** vervolgens **leeg item maken**.

    ![Nieuwe stroom maken met een lege App](./media/automate-with-flow/1createflow.png)

### <a name="step-2-create-a-trigger-for-your-flow"></a>Stap 2: Een trigger voor uw stroom maken
1. Selecteer in de Build-tabblad **planning**, en selecteer vervolgens **planning - terugkeerpatroon**.

    ![Selecteer plannen onder build](./media/automate-with-flow/2schedule.png)

1. In de de **Interval** Voer **1**, en klik in de **frequentie** Schakel **dag**.
2. Klik op **nieuwe stap**

    ![Het terugkeerpatroon van de planning bij het invoeren van de frequentie en interval instellen](./media/automate-with-flow/3schedulerecurrence.png)


### <a name="step-3-add-an-application-insights-action"></a>Stap 3: Een Application Insights-actie toevoegen
1. Zoeken naar **Azure Application Insights**.
2. Klik op **Azure Application Insights - visualiseren analysequery**.
 
    ![Kies een actie: Azure Application Insights visualiseren Analytics-query](./media/automate-with-flow/4visualize.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a>Stap 4: Verbinding maken met een Application Insights-resource

Als u wilt deze stap hebt voltooid, moet u een toepassings-ID en een API-sleutel voor uw resource. U kunt ze ophalen vanuit de Azure-portal, zoals wordt weergegeven in het volgende diagram:

![Toepassings-ID in de Azure-portal](./media/automate-with-flow/5apiaccess.png)

![API-sleutel in Azure portal](./media/automate-with-flow/6apikey.png)

- Geef een naam voor de verbinding, samen met de toepassing-ID en API-sleutel.

    ![Venster van de verbinding met Microsoft Flow](./media/automate-with-flow/7connection.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>Stap 5: Geef het Analytics-query en grafiek
In dit voorbeeld van een query selecteert de mislukte aanvragen in de laatste dag en verbindt deze met uitzonderingen die als onderdeel van de bewerking opgetreden. Analytics correleert toe op basis van de operation_Id-id. De query vervolgens de resultaten met behulp van het algoritme autocluster segmenten. 

Wanneer u uw eigen query's maakt, controleert u of dat ze goed in Analytics werken voordat u deze aan uw stroom toevoegt.

- De volgende analytische query toevoegen en selecteer het grafiektype voor HTML-tabel. Selecteer vervolgens **nieuwe stap**.

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
    
    ![Analytics-queryvenster configuratie](./media/automate-with-flow/8query.png)

### <a name="step-6-configure-the-flow-to-send-email"></a>Stap 6: Configureren van de stroom om e-mail te verzenden

1. Zoeken naar **Office 365 Outlook**.
2. Klik op **Office 365 Outlook - een e-mail verzenden**.

    ![Selectievenster voor Office 365 Outlook](./media/automate-with-flow/9outlookaction.png)

1. In de **een e-mailbericht verzenden** venster de volgende handelingen uit:

   a. Typ het e-mailadres van de ontvanger.

   b. Typ een onderwerp in voor het e-mailbericht.

   c. Klik ergens in de **hoofdtekst** vak en selecteer vervolgens in het menu voor dynamische inhoud die wordt geopend aan de rechterkant, **hoofdtekst**.

   d. Klik op **geavanceerde opties weergeven**.

    ![Office 365 Outlook-configuratie](./media/automate-with-flow/10sendemailbody.png)

1. Op het menu voor dynamische inhoud het volgende doen:

    a. Selecteer **Bijlagenaam**.

    b. Selecteer **inhoud van de bijlage**.
    
    c. In de **Is HTML** Schakel **Ja**.

    ![Venster voor Office 365 e-configuratie](./media/automate-with-flow/11emailattachment.png)

### <a name="step-7-save-and-test-your-flow"></a>Stap 7: Opslaan en uw stroom testen
- In de **Stroomnaam** vak, een naam voor uw stroom toevoegen en klik vervolgens op **opslaan**.

    ![Geef stroom een naam en sla](./media/automate-with-flow/12nameflow.png)

U kunt wachten op de trigger voor deze actie niet uitvoeren of kunt u direct door de stroom uitvoeren [waarop de trigger wordt uitgevoerd op aanvraag](https://flow.microsoft.com/blog/run-now-and-six-more-services/).

Wanneer de stroom wordt uitgevoerd, wordt de ontvangers die u hebt opgegeven in de lijst met e-mailbericht een e-mailbericht dat lijkt op het volgende:

![Voorbeeldbericht](./media/automate-with-flow/flow9.png)


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het maken van [Analytics-query's](../../azure-monitor/log-query/get-started-queries.md).
- Meer informatie over [Microsoft Flow](https://ms.flow.microsoft.com).



<!--Link references-->





