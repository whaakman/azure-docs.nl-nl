---
title: Logische apps met Log Analytics - Azure Logic Apps bewaken | Microsoft Docs
description: Verzamel inzichten en foutopsporing van gegevens voor het oplossen en diagnosticeren van uw logische app wordt uitgevoerd met Azure Log Analytics
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 10/19/2018
ms.openlocfilehash: c65cc24f9b0083e9c873465008490bf00ea83852
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50232373"
---
# <a name="monitor-logic-apps-with-azure-log-analytics"></a>Logische apps controleren met Azure Log Analytics

Inschakelen om te controleren en uitgebreidere foutopsporing meer informatie over uw logische apps, [Azure Log Analytics](../log-analytics/log-analytics-overview.md) wanneer u uw logische app maakt. Log Analytics biedt Diagnostische logboekregistratie en bewaking voor uw logische apps wanneer u de oplossing voor Logic Apps-beheer in Azure portal installeren. Deze oplossing biedt ook de verzamelde informatie voor uw logische app wordt uitgevoerd met specifieke details zoals status, uitvoeringstijd, de status opnieuw indienen en correlatie-id's. In dit artikel laat zien hoe inschakelen op Log Analytics, zodat u de runtime-gebeurtenissen weergeven kunt en gegevens voor uw logische app wordt uitgevoerd.

Als u wilt inschakelen op Azure Log Analytics voor bestaande logische apps, volgt u deze stappen voor het [Diagnostische logboekregistratie inschakelen en de logische app runtime-gegevens verzenden naar Log Analytics](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

> [!NOTE]
> Eerder beschreven stappen voor het uitvoeren van deze taken met de Microsoft Operations Management Suite (OMS), is deze pagina [buiten gebruik stellen in januari 2019](../log-analytics/log-analytics-oms-portal-transition.md), vervangt u deze stappen met Azure Log Analytics op in plaats daarvan. 

## <a name="prerequisites"></a>Vereisten

Voordat u begint, moet u een Log Analytics-werkruimte. Informatie over [over het maken van een Log Analytics-werkruimte](../log-analytics/log-analytics-quick-create-workspace.md). 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a>Diagnostische logboekregistratie inschakelen bij het maken van logische apps

1. In [Azure-portal](https://portal.azure.com), een logische app maken. Kies **een resource maken** > **integratie** > **logische App**.

   ![Een logische app maken](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

1. Onder **logische app maken**, taken uitvoeren zoals wordt weergegeven:

   1. Geef een naam voor uw logische app en selecteer uw Azure-abonnement. 

   1. Maak of Selecteer een Azure-resourcegroep.

   1. Stel **Log Analytics** naar **op**. 

   1. In de lijst met Log Analytics-werkruimtelijst, selecteer de werkruimte waar u voor het verzenden van gegevens voor uw logische app wordt uitgevoerd. 

      ![Logische app maken](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      Nadat u deze stap hebt voltooid, maakt Azure uw logische app, die nu is gekoppeld aan uw Log Analytics-werkruimte. 
      Deze stap installeert de Logic Apps-beheeroplossing bovendien ook automatisch in uw werkruimte.

   1. Wanneer u klaar bent, kiest u **Maken**.

1. Wordt uitgevoerd om uw logische app weer te geven, [gaat u verder met deze stappen](#view-logic-app-runs-oms).

## <a name="install-logic-apps-management-solution"></a>Oplossing voor de Logic Apps-beheer installeren

Als u al ingesteld op Log Analytics tijdens het maken van uw logische app, moet u deze stap overslaan. U hebt al de Logic Apps-beheeroplossing geïnstalleerd.

1. Selecteer in [Azure Portal](https://portal.azure.com) de optie **Alle services**. In het zoekvak zoeken "log analytics" en selecteer **Log Analytics**.

   ![Selecteer "Log Analytics"](./media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

1. Onder **Log Analytics**, zoek en selecteer uw Log Analytics-werkruimte. 

   ![Selecteer uw Log Analytics-werkruimte](./media/logic-apps-monitor-your-logic-apps-oms/select-log-analytics-workspace.png)

1. Onder **aan de slag met Log Analytics** > **bewakingsoplossingen configureren**, kiest u **oplossingen weergeven**.

   ![Kies "Oplossingen weergeven"](media/logic-apps-monitor-your-logic-apps-oms/log-analytics-workspace.png)

1. Kies op de pagina overzicht **toevoegen**, wordt geopend de **beheeroplossingen** lijst. Uit de lijst, selecteer **Logic Apps-beheer**. 

   ![Selecteer "Logic Apps-beheer"](./media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

   Als u de oplossing, klikt u onder aan de lijst niet vinden Kies **meer laden** totdat de oplossing wordt weergegeven.

1. Kies **maken**, Controleer of de Log Analytics-werkruimte waar u wilt installeren van de oplossing en kies vervolgens **maken** opnieuw.   

   ![Kies 'Maken' voor "Logic Apps-beheer"](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-apps-management-solution.png)

   Als u niet wilt gebruiken van een bestaande werkruimte, kunt u ook een nieuwe werkruimte maken op dit moment.

   Wanneer u klaar bent, is de oplossing Logic Apps-beheer wordt weergegeven op de pagina overzicht. 

<a name="view-logic-app-runs-oms"></a>

## <a name="view-logic-app-run-information"></a>Voer informatie de logische app weergeven

Nadat uw logische app wordt uitgevoerd, kunt u de status en het aantal voor deze uitvoeringen weergeven op de **Logic Apps-beheer** tegel. 

1. Ga naar uw Log Analytics-werkruimte en open de pagina overzicht. Kies **Logic Apps-beheer**. 

   ![Logische app de status en het aantal](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

   Uw runs voor logic Apps worden hier, gegroepeerd op naam of uitvoeringsstatus. 
   Deze pagina bevat ook informatie over fouten in acties of triggers voor de logische app wordt uitgevoerd.

   ![Statusoverzicht voor uw logische app wordt uitgevoerd](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
1. Als u wilt weergeven van alle uitvoeringen voor een specifieke logische app of status, selecteer de rij voor een logische app of status.

   Hier volgt een voorbeeld waarin de uitgevoerd voor een specifieke logische app:

   ![Uitvoeringen weergeven voor een logische app of status](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   Deze pagina bevat deze geavanceerde opties:

   * **Bijgehouden eigenschappen:**

     Deze kolom wordt bijgehouden eigenschappen die zijn gegroepeerd op acties voor de logische app. U kunt de bijgehouden eigenschappen bekijken **weergave**. 
     Als u wilt zoeken naar de bijgehouden eigenschappen, gebruikt u het kolomfilter.
   
     ![Bijgehouden eigenschappen voor een logische app weergeven](media/logic-apps-monitor-your-logic-apps-oms/logic-app-tracked-properties.png)

     Alle toegevoegde bijgehouden eigenschappen kunnen 10-15 minuten voordat ze worden weergegeven eerst duren. Informatie over [bijgehouden eigenschappen toevoegen aan uw logische app](logic-apps-monitor-your-logic-apps.md#azure-diagnostics-event-settings-and-details).

   * **Verzend:** kunt opnieuw indienen voor een of meer uitvoeringen van logische app die is mislukt, is geslaagd, of u nog steeds uitgevoerd. Schakel de selectievakjes in voor de wordt uitgevoerd die u wilt verzenden, en kies **opnieuw indienen**. 

     ![Uitvoeringen van logische app opnieuw indienen](media/logic-apps-monitor-your-logic-apps-oms/logic-app-resubmit.png)

1. Als u wilt deze resultaten filteren, kunt u filteren zowel client-zijde en serverzijde uitvoeren.

   * **Client-side '-filter**: voor elke kolom, kiest u de filters die u, bijvoorbeeld wilt:

     ![Voorbeeld van de kolomfilters](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * **Server-side '-filter**: kiezen van een specifiek tijdvenster of om te beperken van het aantal uitvoeringen die worden weergegeven, gebruikt u de bereik-besturingselement aan de bovenkant van de pagina. Standaard worden alleen 1000 records weergegeven op een tijdstip.
   
     ![Het tijdvenster wijzigen](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
1. Als u wilt weergeven van alle acties en de bijbehorende details voor een specifieke uitvoering, selecteert u een rij voor een logische app uitvoeren.

   Hier volgt een voorbeeld waarin alle acties voor een specifieke logische app:

   ![Acties voor een logische app weergeven](media/logic-apps-monitor-your-logic-apps-oms/logic-app-action-details.png)
   
1. Op een willekeurige resultatenpagina om de query achter de resultaten weer te geven of om alle resultaten te bekijken, kiest u **Zie alle**, die de zoeken in Logboeken-pagina wordt geopend.
   
   ![Zie alles op de pagina 's](media/logic-apps-monitor-your-logic-apps-oms/logic-app-seeall.png)
   
   Op de pagina zoeken in Logboeken

   * U kunt bekijken van resultaten van de query in een tabel **tabel**.

   * Als u wilt wijzigen van de query, kunt u de gebruikte queryreeks in de zoekbalk. 
   Kies voor een betere ervaring **Advanced Analytics**.

     ![Acties en de details voor een logische app weergeven](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)
     
     U kunt op de pagina Azure Log Analytics bijwerken, query's en bekijk de resultaten uit de tabel. Maakt gebruik van deze query [Kusto-querytaal](https://aka.ms/LogAnalyticsLanguageReference), die u kunt bewerken als u wilt om verschillende resultaten weer te geven. 

     ![Azure Log Analytics - query weergeven](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Volgende stappen

* [B2B-berichten bewaken](../logic-apps/logic-apps-monitor-b2b-message.md)