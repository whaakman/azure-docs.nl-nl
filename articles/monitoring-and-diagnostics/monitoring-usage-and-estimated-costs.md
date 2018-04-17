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
ms.openlocfilehash: ce295c449b01de4fa99df9198805a6b0727c0d18
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="monitoring-usage-and-estimated-costs"></a>Bewaking en de geschatte kosten

In de hub Monitor van de Azure portal, de **gebruik en de geschatte kosten** pagina is ontworpen voor meer informatie over het gebruik van core bewakingsfuncties zoals [waarschuwingen, metrische gegevens, meldingen](https://azure.microsoft.com/pricing/details/monitor/), [Meld Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) en [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). Dit omvat ook gebruik van logboekanalyse aangeschaft via de inzichten en Analytics bieden voor klanten op de prijscategorie plannen vóór April 2018 beschikbaar is.

Op deze pagina kunnen gebruikers bekijken hun gebruik van deze resources voor de afgelopen 31 dagen per abonnement met drill-modules voor een overzicht van de trend van gebruik tijdens deze periode geaggregeerd. Er is een grote hoeveelheid gegevens die nodig zijn om te verzamelen en deze schatting, dus zorg geduld terwijl de pagina wordt geladen.
Hier volgt een voorbeeld van de bewaking gebruiks- en een schatting van de resulterende kosten:

![Gebruik en de geschatte kosten van het portal-schermafbeelding](./media/monitoring-usage-and-estimated-costs/001.png)

Op de koppeling in de kolom van de maandelijkse gebruiksgegevens te klikken, wordt een grafiek met de trends in gebruik gedurende de afgelopen 31 dagen geopend:

![Inbegrepen per knooppunt schermafbeelding 671.47 GB](./media/monitoring-usage-and-estimated-costs/002.png)

Hier is een andere vergelijkbare gebruik en de kosten in dit geval samenvatting, voor een abonnement in het nieuwe April 2018 op basis van verbruik prijsmodel. Houd rekening met het ontbreken van een facturering op basis van een knooppunt en die gegevensopname en bewaren voor logboekanalyse en Application Insights nu op een nieuwe algemene meter worden gerapporteerd.

![Gebruik en de geschatte kosten van het portal-schermafbeelding](./media/monitoring-usage-and-estimated-costs/003.png)

## <a name="new-pricing-model"></a>Nieuw prijsmodel

In April 2018, is een nieuw bewaking prijsmodel uitgebracht.  Deze functies cloud-vriendelijk, op basis van verbruik prijzen. U betaalt alleen voor wat u, zonder verbintenissen op basis van een knooppunt gebruikt. Details van het nieuwe prijsmodel zijn beschikbaar voor [waarschuwingen, metrische gegevens, meldingen](https://azure.microsoft.com/pricing/details/monitor/), [logboekanalyse](https://azure.microsoft.com/pricing/details/log-analytics/) en [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/).

Voor de voorbereiding op klanten logboekanalyse of Application Insights na 2 April 2018, is het nieuwe prijsmodel de enige optie. Voor klanten die u hebt al deze services gebruikt, verplaatsen naar het nieuwe prijsmodel is optioneel.

## <a name="assessing-the-impact-of-the-new-pricing-model"></a>De gevolgen van het nieuwe prijsmodel beoordelen

Het nieuwe prijsmodel wordt andere effecten hebben voor elke klant op basis van hun bewaking gebruikspatronen. Voor klanten die al gebruikmaken van logboekanalyse of Application Insights vóór April 2 2018, de **gebruik en de geschatte kosten** pagina in de Azure-Monitor biedt een manier om in te schatten eventuele wijzigingen in de kosten als u naar het nieuwe prijsmodel en biedt de een manier om een abonnement in het nieuwe model. Het nieuwe prijsmodel zijn nuttig voor de meeste klanten, maar voor klanten met gebruikspatronen vooral hoog gegevens of in hogere kosten regio's, kan dit niet het geval zijn.

Om te zien van een schatting maken van uw kosten voor de abonnementen die u hebt geselecteerd op de **gebruik en de geschatte kosten** pagina, klikt u op de blauwe banner boven aan de pagina. Het is raadzaam om te doen dit een abonnement op een tijdstip, omdat dat het niveau waarop het nieuwe prijsmodel kan worden vastgesteld.

![Selecteer model-schermopname prijzen](./media/monitoring-usage-and-estimated-costs/004.png)

Nu ziet u een vergelijkbare versie van deze pagina met een groen banner:

![Selecteer model-schermopname prijzen](./media/monitoring-usage-and-estimated-costs/005.png)

Hier ziet u een andere set meters – de meters die met het nieuwe prijsmodel overeenkomen. Bijvoorbeeld gegevens opname meters zoals

1. Inzicht en Analytics\Overage per knooppunt
2. Inzicht en Analytics\Included per knooppunt
3. Overschrijding toepassingsgegevens Insights\Basic
4. Toepassingsgegevens Insights\Included

worden gecombineerd tot een nieuwe algemene gegevens opname meter aangeroepen **gedeeld Services\Data opname** aangezien het nieuw prijsmodel geen per knooppunt opgenomen gegevens rechten.

Een andere wijziging u ziet dat de gegevens in logboekanalyse ingenomen is of Application Insights in regio's met hogere kosten worden weergegeven met de nieuwe landinstellingen meters goed navenant, bijvoorbeeld **' gegevensopname (VS-West-Centraal)**.

> [!NOTE]
> De kosten per abonnement geschatte heeft geen rekening te houden in de rechten van de account niveau knooppunt van het abonnement Operations Management Suite (OMS). Raadpleeg in dat geval uw accountvertegenwoordiger voor een diepgaandere bespreking van de nieuwe prijsmodel.

## <a name="new-pricing-model-and-operations-management-suite-subscription-entitlements"></a>Nieuwe prijzen model en rechten voor Operations Management Suite-abonnement

Klanten die zijn gekocht van Microsoft Operations Management Suite E1 en E2 komen in aanmerking voor de per knooppunt gegevens opname rechten voor [logboekanalyse](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite) en [Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-pricing#the-price-plans). Voor het ontvangen van deze rechten voor logboekanalyse werkruimten of Application Insights-resources in een bepaald abonnement, dat abonnement de prijzen voor het model moeten blijven in de pre-April 2018 prijscategorie model waar de logboekanalyse "Per knooppunt (OMS)' prijscategorie en de Application Insights 'Enterprise' plan prijzen zijn beschikbaar. Afhankelijk van het aantal knooppunten van het pakket dat uw organisatie heeft aangeschaft, enkele verplaatsen abonnementen op het nieuwe prijsmodel nog steeds mogelijk om, maar dit moet zorgvuldig gebeuren. 

## <a name="changes-when-moving-to-the-new-pricing-model"></a>Wijzigingen bij het verplaatsen naar het nieuwe prijsmodel

Een abonnement verplaatsen naar het nieuwe prijsmodel verandert de prijscategorie voor elke logboekanalyse in een nieuwe laag voor de Per GB en eventuele (aangeroepen 'pergb2018' in Azure Resource Manager) wordt het verplaatst. Verplaatsing, wijzigt u ook een Application Insights-resources in het plan Enterprise naar de Basic-abonnement. De effecten van deze worden weergegeven op de schatting van de kosten die hierboven worden beschreven. 

## <a name="moving-to-the-new-pricing-model"></a>Verplaatsen naar het nieuwe prijsmodel

Als u hebt besloten vast te stellen de nieuw prijsmodel voor een abonnement, klikt u op de **prijzen modelselectie** optie boven aan de **gebruik en de geschatte kosten** pagina:

![Gebruik een geschatte kosten in de nieuwe prijscategorie model-schermopname controleren](./media/monitoring-usage-and-estimated-costs/006.png)

Hiermee opent u de **modelselectie prijzen** pagina met elk van de abonnementen die u bekeek op de vorige pagina:

![Schermafbeelding van de selectie prijscategorie model](./media/monitoring-usage-and-estimated-costs/007.png)

Als u een abonnement naar het nieuwe prijsmodel verplaatst, wilt u zojuist hebt Schakel het selectievakje in en klik op **opslaan**.  U kunt verplaatsen terug naar het oudere prijsmodel op dezelfde manier. Houd er rekening mee dat abonnement eigenaar of bijdrager machtigingen vereist zijn voor het wijzigen van het prijsmodel.
