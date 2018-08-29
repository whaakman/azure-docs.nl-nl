---
title: Monitor voor logische app wordt uitgevoerd met Log Analytics - Azure Logic Apps | Microsoft Docs
description: Verkrijgen van inzichten en gegevens over uw logische app wordt uitgevoerd met Log Analytics voor foutopsporing voor het opsporen en oplossen
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/19/2018
ms.openlocfilehash: 1aa55728b222c2838026cf5b06175736c5c84194
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43123287"
---
# <a name="monitor-and-get-insights-about-logic-app-runs-with-log-analytics"></a>Controleren op en Verkrijg inzichten over runs voor logic Apps met Log Analytics

Voor controle en uitgebreidere foutopsporing informatie, kunt u Log Analytics inschakelen op hetzelfde moment als u een logische app maakt. Log Analytics biedt Diagnostische logboekregistratie en bewaking voor uw logische app wordt uitgevoerd via Azure portal. Wanneer u de Logic Apps-beheeroplossing toevoegt, krijgt u de cumulatieve status van de runs voor logic Apps en -specifieke details, zoals status, uitvoeringstijd, de status opnieuw indienen en correlatie-id's.

Dit artikel laat het inschakelen van Log Analytics, zodat u de runtime-gebeurtenissen weergeven kunt en gegevens voor uw logische app uitvoeren.

 > [!TIP]
 > Voor het bewaken van uw bestaande logische apps, volgt u deze stappen voor het [Diagnostische logboekregistratie inschakelen en de logische app runtime-gegevens verzenden naar Log Analytics](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

## <a name="requirements"></a>Vereisten

Voordat u begint, moet u een Log Analytics-werkruimte. Informatie over [over het maken van een Log Analytics-werkruimte](../log-analytics/log-analytics-quick-create-workspace.md). 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a>Diagnostische logboekregistratie inschakelen bij het maken van logische apps

1. In [Azure-portal](https://portal.azure.com), een logische app maken. Kies **een resource maken** > **bedrijfsintegratie** > **logische App**.

   ![Een logische app maken](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

2. In de **logische app maken** pagina, taken uitvoeren zoals wordt weergegeven:

   1. Geef een naam voor uw logische app en selecteer uw Azure-abonnement. 
   2. Maak of Selecteer een Azure-resourcegroep.
   3. Stel **Log Analytics** naar **op**. 
   Selecteer de Log Analytics-werkruimte waar u voor het verzenden van gegevens voor uw logische app wordt uitgevoerd. 
   4. Wanneer u klaar bent, kiest u **vastmaken aan dashboard** > **maken**.

      ![Logische app maken](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      Nadat u deze stap hebt voltooid, maakt Azure uw logische app, die nu is gekoppeld aan uw Log Analytics-werkruimte. 
      Deze stap installeert de Logic Apps-beheeroplossing bovendien ook automatisch in uw werkruimte.

3. Wordt uitgevoerd om uw logische app weer te geven, [gaat u verder met deze stappen](#view-logic-app-runs-oms).

## <a name="install-the-logic-apps-management-solution"></a>De oplossing voor Logic Apps-beheer installeren

Als u al ingesteld op Log Analytics tijdens het maken van uw logische app, moet u deze stap overslaan. U hebt al de Logic Apps-beheeroplossing geïnstalleerd.

1. In de [Azure-portal](https://portal.azure.com), kiest u **meer Services**. Zoeken naar "log analytics" als filter en kies **Log Analytics** zoals wordt weergegeven:

   ![Kies "Log Analytics"](media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

2. Onder **Log Analytics**, zoek en selecteer uw Log Analytics-werkruimte. 

   ![Selecteer uw Log Analytics-werkruimte](media/logic-apps-monitor-your-logic-apps-oms/select-logic-app.png)

3. Onder **Management**, kiest u **overzicht**.

   ![Kies 'OMS-Portal'](media/logic-apps-monitor-your-logic-apps-oms/ibiza-portal-page.png)

4. Kies op de pagina overzicht **toevoegen** om de tegel beheeroplossingen. 

   ![Kies "Logic Apps-beheer"](media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

5. Blader door de lijst met **beheeroplossingen**, kiest u **Logic Apps-beheer** oplossing, en kies **maken** ze deze willen installeren op de pagina overzicht.

   ![Kies 'Toevoegen' voor "Logic Apps-beheer"](media/logic-apps-monitor-your-logic-apps-oms/create-logic-apps-management-solution.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-your-logic-app-runs-in-your-log-analytics-workspace"></a>Uw logische app wordt uitgevoerd in uw Log Analytics-werkruimte weergeven

1. Als u wilt weergeven van het aantal en de status van uw logische app wordt uitgevoerd, gaat u naar de overzichtspagina voor uw Log Analytics-werkruimte. Lees de informatie op de **Logic Apps-beheer** tegel.

   ![Overzichtstegel met logische app uitvoeren aantal en de status](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

2. Als u een overzicht met meer informatie over uw logische app wordt uitgevoerd, kiest u de **Logic Apps-beheer** tegel.

   Uw runs voor logic Apps worden hier, gegroepeerd op naam of uitvoeringsstatus. U ziet ook details over de fouten in acties of triggers voor de logische app wordt uitgevoerd.

   ![Statusoverzicht voor uw logische app wordt uitgevoerd](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
3. Als u wilt weergeven van alle uitvoeringen voor een specifieke logische app of status, selecteer de rij voor een logische app of status.

   Hier volgt een voorbeeld waarin de uitgevoerd voor een specifieke logische app:

   ![Uitvoeringen weergeven voor een logische app of status](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   Er zijn twee geavanceerde opties op deze pagina:
   * **Bijgehouden eigenschappen:** in deze kolom wordt bijgehouden eigenschappen die zijn gegroepeerd op acties voor de logische app. U kunt de bijgehouden eigenschappen bekijken **weergave**. U kunt de bijgehouden eigenschappen zoeken met behulp van de kolomfilter.
   
     ![Bijgehouden eigenschappen voor een logische app weergeven](media/logic-apps-monitor-your-logic-apps-oms/logic-app-tracked-properties.png)

     Alle toegevoegde bijgehouden eigenschappen kunnen 10-15 minuten voordat ze worden weergegeven eerst duren. Informatie over [bijgehouden eigenschappen toevoegen aan uw logische app](logic-apps-monitor-your-logic-apps.md#azure-diagnostics-event-settings-and-details).

   * **Verzend:** kunt opnieuw indienen voor een of meer uitvoeringen van logische app die is mislukt, is geslaagd, of u nog steeds uitgevoerd. Schakel de selectievakjes in voor de wordt uitgevoerd die u wilt verzenden, en kies **opnieuw indienen**. 

     ![Uitvoeringen van logische app opnieuw indienen](media/logic-apps-monitor-your-logic-apps-oms/logic-app-resubmit.png)

4. Als u wilt deze resultaten filteren, kunt u filteren zowel client-zijde en serverzijde uitvoeren.

   * Client-side '-filter: voor elke kolom, kiest u de filters die u wilt. 
   Hier volgen enkele voorbeelden:

     ![Voorbeeld van de kolomfilters](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * Server-side '-filter: kiezen van een specifiek tijdvenster of om te beperken van het aantal uitvoeringen die worden weergegeven, gebruikt u de bereik-besturingselement aan de bovenkant van de pagina. 
   Standaard worden alleen 1000 records weergegeven op een tijdstip. 
   
     ![Het tijdvenster wijzigen](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
5. Als u wilt weergeven van alle acties en de bijbehorende details voor een specifieke uitvoering, selecteert u een rij voor een logische app uitvoeren.

   Hier volgt een voorbeeld waarin alle acties voor een specifieke logische app:

   ![Acties voor een logische app weergeven](media/logic-apps-monitor-your-logic-apps-oms/logic-app-action-details.png)
   
6. Op een willekeurige resultatenpagina om de query achter de resultaten weer te geven of om alle resultaten te bekijken, kiest u **Zie alle**, die de zoeken in Logboeken-pagina wordt geopend.
   
   ![Zie alles op de pagina 's](media/logic-apps-monitor-your-logic-apps-oms/logic-app-seeall.png)
   
   Op de pagina zoeken in Logboeken
   * U kunt bekijken van resultaten van de query in een tabel **tabel**.
   * Als u wilt wijzigen van de query, kunt u de gebruikte queryreeks in de zoekbalk. 
   Kies voor een betere ervaring **Advanced Analytics**.

     ![Acties en de details voor een logische app weergeven](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)
     
     Op de pagina Azure Log Analytics kunt u hier query's bijwerken en de resultaten van de tabel weer te geven. 
     Maakt gebruik van deze query [Kusto-querytaal](https://docs.loganalytics.io/docs/Language-Reference), die u kunt bewerken als u wilt om verschillende resultaten weer te geven. 

     ![Azure Log Analytics - query weergeven](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Volgende stappen

* [B2B-berichten bewaken](../logic-apps/logic-apps-monitor-b2b-message.md)

