---
title: Aangepaste rapporten met Azure Application Insights-gegevens automatiseren
description: Aangepaste dagelijkse/wekelijkse/maandelijkse rapporten met Azure Application Insights-gegevens automatiseren
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/25/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: 51482aa460826d4b364ba6b034129be9b7211ff3
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52872439"
---
# <a name="automate-custom-reports-with-azure-application-insights-data"></a>Aangepaste rapporten met Azure Application Insights-gegevens automatiseren

Periodieke rapporten zorgen ervoor dat een team in kennis gesteld op hoe hun zakelijke essentiële services doen. Ontwikkelaars, DevOps/SRE teams en hun managers kunnen productief zijn met geautomatiseerde rapporten op betrouwbare wijze inzichten leveren zonder iedereen aan te melden bij de portal. Deze rapporten kunnen ook helpen identificeren geleidelijke verhogingen in van minder dan een belasting of een fout tarieven die niet voor een activeren mogelijk waarschuwingsregels.

Elke onderneming heeft de unieke rapportagebehoeften, zoals: 

* Specifieke percentiel aggregaties van metrische gegevens of aangepaste metrische gegevens in een rapport.
* Hebben verschillende rapporten voor dagelijkse, wekelijkse en maandelijkse implementatie-ups van gegevens voor verschillende doelgroepen.
* Segmentering per aangepaste kenmerken, zoals regio, of de omgeving. 
* Sommige AI resources groeperen samen in een rapport, zelfs als ze mogelijk in verschillende abonnementen of resource groepen enzovoort.
* Afzonderlijke rapporten met gevoelige metrische gegevens die zijn verzonden naar selectief doelgroep.
* Rapporten naar belanghebbenden die met u mogelijk geen toegang tot de portal resources.

> [!NOTE] 
> De wekelijkse e-mail met de Application Insights samenvatting staat niet toe dat een aanpassing en en vervangen door de onderstaande aangepaste opties worden afgebroken. Het laatste wekelijkse samenvatting e-mailbericht ontvangt op 11 juni 2018. Configureer een van de volgende opties om op te halen van vergelijkbare aangepaste rapporten (Gebruik de query die wordt voorgesteld hieronder).

## <a name="to-automate-custom-report-emails"></a>Voor het automatiseren van aangepast rapport maken e-mailberichten

U kunt [programmatisch opvragen Application Insights](https://dev.applicationinsights.io/) gegevens om aangepaste rapporten volgens een schema te genereren. De volgende opties kunt u snel aan de slag:

* [Rapporten met Microsoft Flow te automatiseren](app-insights-automate-with-flow.md)
* [Rapporten met Logic Apps automatiseren](automate-with-logic-apps.md)
* Gebruik van de 'Application Insights geplande Samenvatting" [Azure-functie](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) sjabloon in het scenario voor bewaking. Deze functie maakt gebruik van SendGrid te leveren van het e-mailbericht. 

    ![Azure-functie-sjabloon](./media/automate-custom-reports/azure-function-template.png)

## <a name="sample-query-for-a-weekly-digest-email"></a>Voorbeeldquery voor een wekelijkse e-mail met de samenvatting
De volgende query geeft samenvoegen over meerdere gegevenssets voor een wekelijkse e-mail met de samenvatting, zoals het rapport. Aanpassen zoals vereist en deze gebruiken met een van de opties voor het automatiseren van een wekelijks rapport genoemde.   

```AIQL
let period=7d;
requests
| where timestamp > ago(period)
| summarize Row = 1, TotalRequests = sum(itemCount), FailedRequests = sum(toint(success == 'False')),
    RequestsDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
| join (
dependencies
| where timestamp > ago(period)
| summarize Row = 1, TotalDependencies = sum(itemCount), FailedDependencies = sum(success == 'False'),
    DependenciesDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row | join (
pageViews
| where timestamp > ago(period)
| summarize Row = 1, TotalViews = sum(itemCount)
) on Row | join (
exceptions
| where timestamp > ago(period)
| summarize Row = 1, TotalExceptions = sum(itemCount)
) on Row | join (
availabilityResults
| where timestamp > ago(period)
| summarize Row = 1, OverallAvailability = iff(isnan(avg(toint(success))), '------', tostring(toint(avg(toint(success)) * 10000) / 100.0)),
    AvailabilityDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row
| project TotalRequests, FailedRequests, RequestsDuration, TotalDependencies, FailedDependencies, DependenciesDuration, TotalViews, TotalExceptions, OverallAvailability, AvailabilityDuration
```

## <a name="application-insights-scheduled-digest-report"></a>Application Insights geplande digest-rapport

1. Selecteer in de Azure-portal **een Resource maken** > **Compute** > **functie-App**.

   ![Een schermafbeelding van de functie-App van Azure-Resource maken](./media/automate-custom-reports/function-app-01.png)

2. Voer de juiste gegevens voor uw app en selecteer _maken_. (Application Insights _op_ is alleen vereist als u wilt bewaken van uw nieuwe functie-App met Application Insights)

   ![Een schermopname van Azure Resource-instellingen voor functie-App maken](./media/automate-custom-reports/function-app-02.png)

3. Als uw nieuwe functie-App de implementatie is voltooid, selecteert u **naar de resource gaan**.

4. Selecteer **nieuwe functie**.

   ![Maak een nieuwe functie-schermafbeelding](./media/automate-custom-reports/function-app-03.png)

5. Selecteer de  **_Application Insights geplande samenvatting sjabloon_**.

   ![Nieuwe functiesjabloon Application Insights-schermafbeelding](./media/automate-custom-reports/function-app-04.png)

6. Voer een juiste geadresseerde e-mailadres voor uw rapport en selecteer **maken**.

   ![Schermafbeelding van de functie-instellingen](./media/automate-custom-reports/function-app-05.png)

7. Selecteer uw **functie-App** > **platformfuncties** > **toepassingsinstellingen**.

    ![Schermafbeelding van de instellingen voor Azure Functions-toepassing](./media/automate-custom-reports/function-app-07.png)

8. Drie nieuwe toepassingsinstellingen maken met de juiste overeenkomende waarden ``AI_APP_ID``, ``AI_APP_KEY``, en ``SendGridAPI``. Selecteer **Opslaan**.

     ![Schermafbeelding van functie-integratie-interface](./media/automate-custom-reports/function-app-08.png)
    
    (De waarden AI_ vindt u onder de API-toegang voor de Application Insights-Resource die u wilt rapporteren. Als u een Application Insights-API-sleutel niet hebt, is de optie voor **API-sleutel maken**.)
    
    * AI_APP_ID toepassings-ID =
    * AI_APP_KEY = API-sleutel
    * SendGridAPI = SendGrid-API-sleutel

    > [!NOTE]
    > Als u geen een SendGrid-account hebt, kunt u een kunt maken. Van SendGrid-documentatie voor Azure Functions is [hier](https://docs.microsoft.com/azure/azure-functions/functions-bindings-sendgrid). Als alleen kunt u een minimale uitleg over hoe u SendGrid instellen en genereren van een API-sleutel een aan het einde van dit artikel is opgegeven. 

9. Selecteer **integreren** en onder uitvoer **SendGrid ($return)**.

     ![Schermopname van uitvoer](./media/automate-custom-reports/function-app-09.png)

10. Onder de **SendGridAPI sleutel App-instelling**, selecteert u de zojuist gemaakte App-instelling voor **SendGridAPI**.

     ![Schermafbeelding van de functie-App uitvoeren](./media/automate-custom-reports/function-app-010.png)

11. Uitvoeren en testen van uw functie-App.

     ![Schermafbeelding van de test](./media/automate-custom-reports/function-app-11.png)

12. Controleer uw e-mail om te bevestigen dat het bericht verzonden/ontvangen is.

     ![Schermafbeelding van de regel e-onderwerp](./media/automate-custom-reports/function-app-12.png)

## <a name="sendgrid-with-azure"></a>SendGrid met Azure

Deze stappen zijn alleen van toepassing als u al een SendGrid-account geconfigureerd hebt.

1. Vanuit de Azure portal selecteert **een resource maken** zoeken naar **SendGrid-e-Maillevering** > klikt u op **maken** > en vul de SendGrid-specifieke instructies maken. 

     ![Schermafbeelding van SendGrid-Resource maken](./media/automate-custom-reports/function-app-13.png)

2. Zodra de gemaakt op basis van SendGrid-Accounts selecteert **beheren**.

     ![Instellingen voor API-sleutel-schermafbeelding](./media/automate-custom-reports/function-app-14.png)

3. Van SendGrid-site wordt geopend. Selecteer **instellingen** > **API-sleutels**.

     ![Maken en weergeven van de API-sleutel App-schermafbeelding](./media/automate-custom-reports/function-app-15.png)

4. Maak een API-sleutel > Kies **maken & weergave** (Raadpleeg de documentatie van SendGrid op beperkte toegang om te bepalen welk niveau van de machtigingen die geschikt is voor uw API-sleutel is. Volledige toegang is hier geselecteerde voorbeeld alleen voor testdoeleinden.)

   ![Schermafbeelding van de volledige toegang](./media/automate-custom-reports/function-app-16.png)

5. Kopieer de gehele sleutel, wordt deze waarde is wat u nodig hebt in uw functie-App-instellingen als de waarde voor SendGridAPI

   ![Schermafbeelding van API-sleutel kopiëren](./media/automate-custom-reports/function-app-17.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het maken van [Analytics-query's](../azure-monitor/log-query/get-started-queries.md).
* Meer informatie over [Application Insights-gegevens via een programma uitvoeren van query's](https://dev.applicationinsights.io/)
* Meer informatie over [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).
* Meer informatie over [Microsoft Flow](https://ms.flow.microsoft.com).
