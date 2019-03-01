---
title: Tracerings-query's voor B2B-berichten maken in Azure Monitor-logs - Azure Logic Apps | Microsoft Docs
description: Query's die AS2, X 12 en EDIFACT-berichten in Azure Log Analytics voor Azure Logic Apps bijhouden maken
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 10/19/2018
ms.openlocfilehash: d4a94e75de34bbafd3bc8f1c1a0d1a6817245e5f
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57194908"
---
# <a name="create-tracking-queries-for-b2b-messages-in-azure-monitor-logs-for-azure-logic-apps"></a>Tracerings-query's voor B2B-berichten maken in Azure Monitor-logboeken voor Azure Logic Apps

Als u wilt zoeken AS2, X12 of EDIFACT-berichten dat u met bijhoudt [logboeken van Azure Monitor](../log-analytics/log-analytics-overview.md), kunt u query's die filteracties op basis van specifieke criteria. U kunt bijvoorbeeld berichten op basis van een specifieke uitwisseling controlenummer vinden.

> [!NOTE]
> Eerder beschreven stappen voor het uitvoeren van deze taken met de Microsoft Operations Management Suite (OMS), is deze pagina [buiten gebruik stellen in januari 2019](../azure-monitor/platform/oms-portal-transition.md), vervangt u deze stappen met Azure Log Analytics op in plaats daarvan. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Vereisten

* Een logische app die ingesteld met de logboekregistratie van diagnostische gegevens. Informatie over [over het maken van een logische app](quickstart-create-first-logic-app-workflow.md) en [over het instellen van logboekregistratie voor deze logische app](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Integratie-account ingesteld met bewaking en logboekregistratie. Informatie over [over het maken van een integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) en [over het instellen van controle en logboekregistratie voor dat account](../logic-apps/logic-apps-monitor-b2b-message.md).

* Als u dat nog niet gedaan hebt, [diagnostische gegevens publiceren naar Azure Monitor logboeken](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) en [bericht bijhouden in Logboeken van Azure Monitor instellen](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

## <a name="create-queries-with-filters"></a>Query's met filters maken

Berichten op basis van specifieke eigenschappen of waarden zoekt, kunt u query's die gebruikmaken van filters maken. 

1. Selecteer in [Azure Portal](https://portal.azure.com) de optie **Alle services**. In het zoekvak zoeken "log analytics" en selecteer **Log Analytics**.

   ![Selecteer de Log Analytics](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/find-log-analytics.png)

1. Onder **Log Analytics**, zoek en selecteer uw Log Analytics-werkruimte. 

   ![Log Analytics-werkruimte selecteren](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/select-log-analytics-workspace.png)

1. In het werkruimtemenu onder **algemene**, selecteert u **Logboeken (klassiek)** of **logboeken**. 

   Dit voorbeeld laat zien hoe u de klassieke weergave van de logboeken. 
   Als u ervoor kiest **logboeken bekijken** in de **maximaliseren van uw Log Analytics-ervaring** sectie onder **zoeken in Logboeken en analyseren**, krijgt u de **Logboeken (klassieke weergave)** . 

   ![Klassieke logboeken weergeven](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/view-classic-logs.png)

1. In de query-invoervak, typ de naam van het veld dat u wilt zoeken. Wanneer u te typen begint, wordt de query-editor toont de mogelijke overeenkomsten en de bewerkingen die u kunt gebruiken. Nadat u uw query maakt, kiest u **uitvoeren** of druk op de Enter-toets.

   In dit voorbeeld wordt gezocht naar overeenkomsten op **LogicAppB2B**. 
   Meer informatie over [gegevens zoeken in Logboeken van Azure Monitor](../log-analytics/log-analytics-log-searches.md).

   ![Begin met het typen van queryreeks](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/create-query.png)

1. Als u wilt wijzigen van de periode die u wilt weergeven in het linkerdeelvenster in de duur van de lijst selecteren of sleep de schuifregelaar. 

   ![Wijziging tijdsbestek](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/change-timeframe.png)

1. Als u wilt een filter toevoegen aan uw query, kies **toevoegen**. 

   ![Filter toevoegen aan query](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/add-filter.png)

1. Onder **Filters toevoegen**, voer filternaam die u wilt zoeken. Als u het filter hebt gevonden, selecteert u met het filter. Kies in het linkerdeelvenster **toevoegen** opnieuw.

   Dit is bijvoorbeeld een andere query die u zoekt op **Type == "AzureDiagnostics"** gebeurtenissen en vindt u de resultaten op basis van het controlenummer uitwisseling door het selecteren van de **event_record_messageProperties_ interchangeControlNumber_s** filter.

   ![De filterwaarde selecteren](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/filter-example.png)

   Nadat u hebt gekozen **toevoegen**, uw query wordt bijgewerkt met de gebeurtenis geselecteerde filter en de waarde. 
   De resultaten van uw vorige worden nu te gefilterd. 

   Bijvoorbeeld: deze query zoekt **Type == "AzureDiagnostics"** en vindt u resultaten op basis van een controlenummer uitwisseling met behulp van de **event_record_messageProperties_interchangeControlNumber_s**filter.

   ![Gefilterde resultaten](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-query"></a>Query opslaan

Uw query in op te slaan **Logboeken (klassiek)** weergeven, als volgt te werk:

1. Uit uw query op de **Logboeken (klassiek)** pagina, kies **Analytics**. 

   ![Kies 'Analytics'](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/choose-analytics.png)

1. Kies op de werkbalk query **opslaan**.

   ![Kies 'Opslaan'](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/save-query.png)

1. Geef de details over uw query, bijvoorbeeld, Geef uw query een naam, selecteer **Query**, en geef een categorienaam op. Als u bent klaar, kiest u **Opslaan**.

   ![Kies 'Opslaan'](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query-details.png)

1. Opgeslagen query's, Ga terug naar de querypagina om weer te geven. Kies op de werkbalk query **opgeslagen zoekopdrachten**.

   ![Kies "Opgeslagen zoekopdrachten"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/choose-saved-searches.png)

1. Onder **opgeslagen zoekopdrachten**, selecteert u uw query, zodat u kunt de resultaten bekijken. 

   ![Selecteer uw query](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/saved-query-results.png)

   Voor het bijwerken van de query, zodat u verschillende resultaten kunt vinden, kunt u de query bewerken.

## <a name="find-and-run-saved-queries"></a>Zoeken en opgeslagen query's uitvoeren

1. Selecteer in [Azure Portal](https://portal.azure.com) de optie **Alle services**. In het zoekvak zoeken "log analytics" en selecteer **Log Analytics**.

   ![Selecteer de Log Analytics](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/find-log-analytics.png)

1. Onder **Log Analytics**, zoek en selecteer uw Log Analytics-werkruimte. 

   ![Log Analytics-werkruimte selecteren](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/select-log-analytics-workspace.png)

1. In het werkruimtemenu onder **algemene**, selecteert u **Logboeken (klassiek)** of **logboeken**. 

   Dit voorbeeld laat zien hoe u de klassieke weergave van de logboeken. 

1. Nadat de querypagina wordt geopend, op de werkbalk query kiest **opgeslagen zoekopdrachten**.

   ![Kies "Opgeslagen zoekopdrachten"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/choose-saved-searches.png)

1. Onder **opgeslagen zoekopdrachten**, selecteert u uw query, zodat u kunt de resultaten bekijken. 

   ![Selecteer uw query](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/saved-query-results.png) 

   De query wordt automatisch uitgevoerd, maar als de query kan niet worden uitgevoerd voor een bepaalde reden, in de query-editor kiest **uitvoeren**.

## <a name="next-steps"></a>Volgende stappen

* [Volgschema’s voor AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Volgschema’s voor X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Aangepaste volgschema 's](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)