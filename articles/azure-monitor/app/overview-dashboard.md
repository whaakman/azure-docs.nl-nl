---
title: Een Dashboard met Azure Application Insights | Microsoft Docs
description: Toepassingen bewaken met Azure Application Insights en een Dashboard-functionaliteit.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: mbullwin
ms.openlocfilehash: d1823779f8a8070149811e2349fc9f4281072d38
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66497155"
---
# <a name="application-insights-overview-dashboard"></a>Application Insights een dashboard

Application Insights heeft altijd beschikbaar voor een overzicht overzichtsvenster voor een snelle, in een oogopslag beoordeling van de status en prestaties van uw toepassing. Een dashboard met de nieuwe biedt een snellere meer flexibele ervaring.

## <a name="how-do-i-test-out-the-new-experience"></a>Hoe test ik de nieuwe ervaring uit?

Een dashboard met de nieuwe start nu standaard:

![Overzicht voorbeeldvenster](./media/overview-dashboard/overview.png)

## <a name="better-performance"></a>Betere prestaties

Selectie van het tijdsbereik is vereenvoudigd naar een eenvoudige interface met één klik.

![Tijdsbereik](./media/overview-dashboard/app-insights-overview-dashboard-03.png)

Algehele zijn prestaties aanzienlijk toegenomen. U hebt één klik toegang tot populaire functies zoals **zoeken** en **Analytics**. Elke dynamisch bijwerken van de KPI-tegel standaard biedt inzicht in de bijbehorende Application Insights-functies. Kunnen geen aanvragen selecteren voor meer informatie over **fouten** onder de **onderzoeken** header:

![fouten](./media/overview-dashboard/app-insights-overview-dashboard-04.png)

## <a name="application-dashboard"></a>Toepassingsdashboard

Toepassingsdashboard maakt gebruik van de bestaande dashboardtechnologie binnen Azure om te voorzien in een volledig aanpasbare één deelvenster weergave van de status van de toepassing en de prestaties.

Voor toegang tot de standaard dashboard Selecteer _Toepassingsdashboard_ in de linkerbovenhoek.

![Dashboardweergave](./media/overview-dashboard/app-insights-overview-dashboard-05.png)

Als dit de eerste keer toegang tot het dashboard, wordt een standaardweergave gestart:

![Dashboardweergave](./media/overview-dashboard/0001-dashboard.png)

U kunt de standaardweergave houden als u wilt dat dit. Of u kunt ook toevoegen en verwijderen vanuit het dashboard om te aan te passen aan de behoeften van uw team.

> [!NOTE]
> Alle gebruikers met toegang tot de Application Insights-resource delen dezelfde toepassing dashboard-ervaring. Wijzigingen die zijn aangebracht door een gebruiker wordt gewijzigd, de weergave voor alle gebruikers.

Ga terug naar het overzicht-ervaring hoeft te selecteren:

![Overzicht-knop](./media/overview-dashboard/app-insights-overview-dashboard-07.png)

## <a name="troubleshooting"></a>Problemen oplossen

Als u selecteert **tegelinstellingen configureren** en stel een aangepast tijdsbereik dat overschrijdt 31 dagen in uw dashboard niet langer dan 31 dagen van gegevens, zelfs met de standaard-bewaarperiode van 90 dagen worden weergegeven. Er is momenteel geen oplossing voor dit gedrag.

## <a name="next-steps"></a>Volgende stappen

- [Trechters](../../azure-monitor/app/usage-funnels.md)
- [Retentie](../../azure-monitor/app/usage-retention.md)
- [Gebruikersstromen](../../azure-monitor/app/usage-flows.md)