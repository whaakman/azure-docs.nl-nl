---
title: Aangepaste rapporten met gegevens van Azure Application Insights automatiseren
description: Aangepaste wekelijks-dagelijks/maandelijkse rapporten met gegevens van Azure Application Insights automatiseren
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/25/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: c8cff54c67ab2c9c3d09f9261617b6312cc4434a
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37025741"
---
# <a name="automate-custom-reports-with-azure-application-insights-data"></a>Aangepaste rapporten met gegevens van Azure Application Insights automatiseren

Periodieke rapporten kunt u ervoor zorgen dat een team op de hoogte hoe hun zakelijke essentiële services doen. Ontwikkelaars, DevOps/SRE teams en hun managers kunnen productief zijn met geautomatiseerde rapporten betrouwbaar inzicht leveren zonder iedereen aan te melden in de portal. Deze rapporten kunnen ook helpen identificeren geleidelijke toename in latentie, waarschuwt laden of fout tarieven die niet voor alle activeren mogelijk regels.

Elke enterprise heeft de unieke rapportagevereisten, zoals: 

* Specifieke percentiel aggregaties van metrische gegevens of aangepaste metrische gegevens in een rapport.
* Andere rapporten voor dagelijkse, wekelijkse en maandelijkse roll-ups van gegevens voor verschillende doelgroepen hebben.
* Segmentering door aangepaste kenmerken, zoals regio, of de omgeving. 
* Sommige AI resources groeperen samen in één rapport, zelfs als ze mogelijk in verschillende abonnementen of de resource groepen enzovoort.
* Afzonderlijke rapporten met gevoelige metrische gegevens die worden verzonden naar selectief doelgroep.
* Rapporten voor belanghebbenden die geen toegang tot de portal resources.

> [!NOTE] 
> Wekelijks Application Insights overzicht via e-mail is niet toegestaan voor elke aanpassing en door de volgende aangepaste opties worden afgebroken. Het laatste wekelijkse digest e-mailbericht wordt verzonden op 11 juni 2018. Configureer een van de volgende opties om op te halen vergelijkbare aangepaste rapporten (Gebruik de query voorgestelde hieronder).

## <a name="to-automate-custom-report-emails"></a>Aangepast rapport e-mailberichten automatiseren

U kunt [programmatisch query Application Insights](https://dev.applicationinsights.io/) gegevens om aangepaste rapporten op basis van een planning te genereren. Aan de hand van de volgende opties kunnen u snel aan de slag:

* [Rapporten voor Microsoft-Flow automatiseren](app-insights-automate-with-flow.md)
* [Rapporten met Logic Apps automatiseren](automate-with-logic-apps.md)
* Gebruik het 'Application Insights geplande digest' [Azure functie](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) sjabloon in het scenario voor bewaking. Hierbij wordt gebruikgemaakt van SendGrid naar de e-mail te bezorgen. 

    ![De functie Azure-sjabloon](./media/automate-custom-reports/azure-function-template.png)

## <a name="sample-query-for-a-weekly-digest-email"></a>Voorbeeldquery voor een wekelijks overzicht via e-mail
De volgende query ziet over meerdere gegevenssets voor een wekelijks overzicht via e-mail zoals rapport toevoegen. Pas deze zo nodig en deze gebruiken met een van de opties hierboven vermelde een wekelijks rapport automatiseren.   

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

1. Selecteer in de Azure-portal **maken van een Resource** > **Compute** > **functie-App**.

   ![Maken van een Azure Resource functie-App-schermafbeelding](./media/automate-custom-reports/function-app-01.png)

2. Voer de juiste info voor uw app en selecteer _maken_. (Application Insights _op_ is alleen vereist als u wilt bewaken van uw nieuwe functie-App met Application Insights)

   ![Een schermafbeelding van de functie App-instellingen van Azure Resource maken](./media/automate-custom-reports/function-app-02.png)

3. Als uw nieuwe functie-App implementatie is voltooid, selecteert u **gaat u naar de resource**.

4. Selecteer **nieuwe functie**.

   ![Maak een nieuwe functie-schermafbeelding](./media/automate-custom-reports/function-app-03.png)

5. Selecteer de  **_Application Insights geplande digest sjabloon_**.

   ![Schermafbeelding van de nieuwe functie Application Insights-sjabloon](./media/automate-custom-reports/function-app-04.png)

6. Voer een juiste geadresseerde e-mailadres voor uw rapport op en selecteer **maken**.

   ![Schermafbeelding van de functie-instellingen](./media/automate-custom-reports/function-app-05.png)

7. Selecteer uw **functie-App** > **platformfuncties** > **toepassingsinstellingen**.

    ![Schermopname van Azure toepassing van de functie-instellingen](./media/automate-custom-reports/function-app-07.png)

8. Drie nieuwe toepassingsinstellingen maken met de juiste overeenkomende waarden ``AI_APP_ID``, ``AI_APP_KEY``, en ``SendGridAPI``. Selecteer **Opslaan**.

     ![Schermafbeelding van de functie integratie-interface](./media/automate-custom-reports/function-app-08.png)
    
    (De waarden AI_ vindt u onder de API-toegang voor de Application Insights-Resource die u rapporteren wilt op. Als u een Application Insights-API-sleutel hebt, is de optie voor **API-sleutel maken**.)
    
    * AI_APP_ID toepassings-ID =
    * AI_APP_KEY = API-sleutel
    * SendGridAPI = SendGrid-API-sleutel

    > [!NOTE]
    > Als u geen SendGrid-account hebt, kunt u een kunt maken. De SendGrid-documentatie voor Azure Functions is [hier](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-sendgrid). Als alleen kunt u een minimale uitleg van de instelling SendGrid en genereren van een API-sleutel een aan het einde van dit artikel is opgegeven. 

9. Selecteer **integreren** en onder uitvoer **SendGrid ($return)**.

     ![Schermafbeelding van de uitvoer](./media/automate-custom-reports/function-app-09.png)

10. Onder de **SendGridAPI sleutel App-instelling**, selecteer uw nieuwe App-instelling voor **SendGridAPI**.

     ![Schermafbeelding van de functie-App uitvoeren](./media/automate-custom-reports/function-app-010.png)

11. Voer en testen van uw App in de functie.

     ![Test-schermafbeelding](./media/automate-custom-reports/function-app-11.png)

12. Controleer uw e-mailadres om te bevestigen dat het bericht verzonden/ontvangen is.

     ![Schermafbeelding voor e-onderwerp regel](./media/automate-custom-reports/function-app-12.png)

## <a name="sendgrid-with-azure"></a>SendGrid met Azure

Deze stappen zijn alleen van toepassing als u al een SendGrid-account geconfigureerd hebt.

1. Van de Azure portal selecteren **maken van een resource** zoekt **de levering via E-mail SendGrid** > klikt u op **maken** > en vul de SendGrid-specifieke instructies maken. 

     ![Schermafbeelding van de SendGrid-Resource maken](./media/automate-custom-reports/function-app-13.png)

2. Eenmaal gemaakt onder SendGrid Accounts selecteert **beheren**.

     ![Instellingen voor API-sleutel-schermafbeelding](./media/automate-custom-reports/function-app-14.png)

3. Hierdoor wordt de SendGrid-site gestart. Selecteer **instellingen** > **API-sleutels**.

     ![Maken en weergeven van de API-sleutel App-schermafbeelding](./media/automate-custom-reports/function-app-15.png)

4. Maak een API-sleutel > Kies **maken & weergave** (Raadpleeg de SendGrid-documentatie over beperkte toegang om te bepalen welk niveau van machtigingen geschikt is voor uw API-sleutel. Volledige toegang is ingeschakeld hier alleen ter voorbeeld.)

   ![Schermafbeelding van de volledige toegang](./media/automate-custom-reports/function-app-16.png)

5. Kopieer de gehele sleutel, deze waarde is wat u nodig hebt in de functie-App-instellingen als de waarde voor SendGridAPI

   ![Kopiëren van de belangrijkste API-schermafbeelding](./media/automate-custom-reports/function-app-17.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het maken van [analysequery's](app-insights-analytics-using.md).
* Meer informatie over [programmatisch opvragen van gegevens van Application Insights](https://dev.applicationinsights.io/)
* Meer informatie over [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).
* Meer informatie over [Microsoft Flow](https://ms.flow.microsoft.com).
