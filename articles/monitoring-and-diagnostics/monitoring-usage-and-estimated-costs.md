---
title: Bewaking van gebruik en de geschatte kosten in Azure Monitor | Microsoft Docs
description: Overzicht van het proces van het gebruik van Azure Monitor gebruiks- en de geschatte kosten van pagina
author: dalekoetke
manager: carmonmills
editor: mrbullwinkle
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2018
ms.author: Dale.Koetke;mbullwin
ms.openlocfilehash: f25c39b602449be3ab9d1cd7e67d6fcfc78afb17
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="monitoring-usage-and-estimated-costs"></a>Bewaking en de geschatte kosten

In de hub Monitor van de Azure portal, de **gebruik en de geschatte kosten** pagina wordt uitgelegd dat het gebruik van kernbewaking functies zoals [waarschuwingen, metrische gegevens, meldingen](https://azure.microsoft.com/pricing/details/monitor/), [Azure Log Analytics ](https://azure.microsoft.com/pricing/details/log-analytics/), en [Azure Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). Dit omvat ook gebruik van logboekanalyse die zijn aangeschaft via de inzichten en Analytics bieden voor klanten op de prijscategorie plannen April 2018 beschikbaar is.

Op deze pagina kunnen gebruikers bekijken hun Resourcegebruik voor de afgelopen 31 dagen geaggregeerd per abonnement. Inzoomen-modules weergeven trends in gebruik gedurende de periode 31 dagen. Grote hoeveelheden gegevens moet worden gecombineerd voor deze schatting, dus zorg geduld terwijl de pagina wordt geladen.

In dit voorbeeld ziet u bewaking gebruiks- en een schatting van de resulterende kosten:

![Gebruik en de geschatte kosten van het portal-schermafbeelding](./media/monitoring-usage-and-estimated-costs/001.png)

Selecteer de koppeling in de maandelijkse gebruiksgegevens kolom openen van een diagram dat aangeeft van trends in gebruik gedurende de afgelopen 31 dagen:

![Inbegrepen per knooppunt balk schermafbeelding van de grafiek](./media/monitoring-usage-and-estimated-costs/002.png)

Hier is een andere vergelijkbare gebruik en kosten samenvatting. Dit voorbeeld ziet een abonnement in het nieuwe April 2018 op basis van verbruik prijsmodel. Let op het ontbreken van een facturering op basis van een knooppunt. Gegevensopname en retentie voor logboekanalyse en Application Insights wordt nu een nieuwe algemene meter gerapporteerd.

![Gebruik en de geschatte kosten portal schermafbeelding - April 2018 prijzen](./media/monitoring-usage-and-estimated-costs/003.png)

## <a name="new-pricing-model"></a>Nieuw prijsmodel

In April 2018, is een nieuw bewaking prijsmodel uitgebracht. Biedt prijsstelling cloud-vriendelijk, op basis van verbruik. U betaalt alleen voor wat u, zonder verbintenissen op basis van een knooppunt gebruikt. Details van het nieuwe prijsmodel zijn beschikbaar voor [waarschuwingen, metrische gegevens, meldingen](https://azure.microsoft.com/pricing/details/monitor/), [logboekanalyse](https://azure.microsoft.com/pricing/details/log-analytics/), en [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/).

Het nieuwe prijsmodel is voor klanten die begint met het gebruik van logboekanalyse of Application Insights na 2 April 2018, de enige optie. Voor klanten die al gebruikmaken van deze services, verplaatsen naar het nieuwe prijsmodel is optioneel.

## <a name="assessing-the-impact-of-the-new-pricing-model"></a>De gevolgen van het nieuwe prijsmodel beoordelen

Het nieuwe prijsmodel heeft verschillende effecten voor elke klant op basis van hun bewaking gebruikspatronen. Voor klanten die zijn met logboekanalyse of Application Insights voordat 2 April 2018, de **gebruik en de geschatte kosten** pagina in de Azure-Monitor maakt een schatting van eventuele wijzigingen in de kosten als ze naar het nieuwe prijsmodel. Het biedt de manier waarop een abonnement verplaatsen naar het nieuwe model. Voor de meeste klanten is het nieuwe prijsmodel voordeligste. Voor klanten met gebruikspatronen vooral hoog gegevens of in hogere kosten regio's, kan dit niet het geval zijn.

Om te zien van een schatting maken van uw kosten voor de abonnementen die u hebt gekozen op de **gebruik en de geschatte kosten** pagina, selecteert u de blauwe banner boven aan de pagina. Het is raadzaam te doen dit een abonnement op een tijdstip, omdat dit het niveau waarop het nieuwe prijsmodel kan worden vastgesteld.

![Gebruik en de geschatte kosten in nieuwe prijscategorie model-schermopname controleren](./media/monitoring-usage-and-estimated-costs/004.png)

De nieuwe pagina ziet u een vergelijkbare versie van de vorige pagina met een groen banner:

![Gebruik en de geschatte kosten in de huidige prijscategorie model-schermopname controleren](./media/monitoring-usage-and-estimated-costs/005.png)

De pagina bevat ook een andere set meters die met het nieuwe prijsmodel overeenkomen. Deze lijst is een voorbeeld:

- Inzicht en Analytics\Overage per knooppunt
- Inzicht en Analytics\Included per knooppunt
- Overschrijding toepassingsgegevens Insights\Basic
- Toepassingsgegevens Insights\Included

Het nieuwe prijsmodel geen opgenomen gegevens op basis van een knooppunt toewijzingen. Daarom deze gegevens opneemt meters worden gecombineerd tot een nieuwe algemene gegevens opname meter aangeroepen **gedeeld Services\Data opname**. 

Er is een andere wijziging gegevens ingenomen in logboekanalyse of Application Insights in regio's met hogere kosten. Gegevens voor deze gebieden hoge kosten worden weergegeven met de nieuwe landinstellingen meters. Een voorbeeld is **gegevensopname (ons West Central)**.

> [!NOTE]
> De geschatte kosten komen niet in de account-niveau per knooppunt rechten van het abonnement Operations Management Suite (OMS) factor per abonnement. Raadpleeg uw accountvertegenwoordiger in dat geval voor een diepgaandere bespreking van de nieuwe prijsmodel.

## <a name="new-pricing-model-and-operations-management-suite-subscription-entitlements"></a>Nieuwe prijzen model en rechten voor Operations Management Suite-abonnement

Klanten die zijn gekocht van Microsoft Operations Management Suite E1 en E2 in aanmerking komen voor per knooppunt gegevens opname rechten voor [logboekanalyse](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite) en [Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-pricing#the-price-plans). Voor het ontvangen van deze rechten voor Log Analytics-werkruimten of Application Insights moeten in het pre-April 2018 prijsmodel resources in een bepaald abonnement, dat abonnement model de prijzen blijven. Dat is waar de logboekanalyse "Per knooppunt (OMS)' prijscategorie en de Application Insights 'Enterprise' prijsstelling beschikbaar zijn. Verplaatsen van dat sommige abonnementen op het nieuwe prijsmodel nog steeds mogelijk om afhankelijk van het aantal knooppunten van het pakket dat uw organisatie heeft aangeschaft. Maar dit moet zorgvuldig gebeuren.

## <a name="changes-when-youre-moving-to-the-new-pricing-model"></a>Verandert wanneer u naar het nieuwe prijsmodel verplaatsen wilt

Een abonnement verplaatsen naar het nieuwe prijsmodel verandert de prijscategorie voor elke logboekanalyse in een nieuwe laag voor de Per GB en eventuele (aangeroepen 'pergb2018' in Azure Resource Manager) wordt het verplaatst. Verplaatsing, wijzigt u ook een Application Insights-resources in het plan Enterprise naar de Basic-abonnement. De schatting van de kosten worden de effecten van deze wijzigingen.

## <a name="moving-to-the-new-pricing-model"></a>Verplaatsen naar het nieuwe prijsmodel

Als u hebt besloten vast te stellen de nieuw prijsmodel voor een abonnement, selecteert u de **prijzen modelselectie** optie boven aan de **gebruik en de geschatte kosten** pagina:

![Gebruik en de geschatte kosten in nieuwe prijscategorie model-schermopname controleren](./media/monitoring-usage-and-estimated-costs/006.png)

De **modelselectie prijzen** pagina wordt geopend. Het bevat een overzicht van elk van de abonnementen die u op de vorige pagina weergegeven:

![Schermafbeelding van de selectie prijscategorie model](./media/monitoring-usage-and-estimated-costs/007.png)

Selecteer het selectievakje in om een abonnement naar het nieuwe prijsmodel verplaatst, en selecteer vervolgens **opslaan**. U kunt verplaatsen terug naar het oudere prijsmodel op dezelfde manier. Houd er rekening mee dat eigenaar van het abonnement of Inzender-rechten zijn nodig voor het wijzigen van het prijsmodel.
