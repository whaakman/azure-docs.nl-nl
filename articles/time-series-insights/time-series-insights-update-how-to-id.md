---
title: Aanbevolen procedures bij het kiezen van een Time Series-ID in Azure Time Series Insights | Microsoft Docs
description: Informatie over aanbevolen procedures bij het kiezen van een Time Series-ID in Azure Time Series Insights
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.openlocfilehash: 86a23e96a0f9b92b4b0551e41c44cf3249b0f7e3
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52873390"
---
# <a name="best-practices-when-choosing-a-time-series-id"></a>Aanbevolen procedures bij het kiezen van een Time Series-ID

In dit document bevat informatie over de partitiesleutel Azure Time Series Insights (Preview) de **Time Series-ID**, en aanbevolen procedures om een te kiezen.

## <a name="choose-a-time-series-id"></a>Een Time Series-id kiezen

De keuze van de **Time Series-ID** is vergelijkbaar met een partitiesleutel voor een database selecteren. Het is daarom een belangrijke beslissing die moet worden gemaakt tijdens de ontwerpfase. U kunt een bestaande omgeving TSI (Preview) voor het gebruik van een andere niet bijwerken **Time Series-ID**. Met andere woorden, nadat een omgeving is gemaakt met een **Time Series-ID**, het beleid kan niet worden gewijzigd omdat deze een onveranderbare eigenschap.

> [!IMPORTANT]
> De **Time Series-ID** is hoofdlettergevoelig en onveranderbare (deze kan niet worden gewijzigd nadat deze is ingesteld).

Met die in gedachten, selecteren de juiste **Time Series-ID** is van essentieel belang. Houd rekening met de volgende eigenschappen bij het selecteren van een **Time Series-ID**:

> [!div class="checklist"]
> * Kies de naam van een eigenschap die heeft een breed scala aan waarden en zelfs toegangspatronen. Het is een aanbevolen procedure om een partitiesleutel met veel verschillende waarden (bijvoorbeeld, honderden of duizenden). Voor veel klanten, dit is zoiets als **DeviceID**, **SensorID**, enz. in de JSON.
> * Deze moet uniek zijn op het niveau van de leaf-knooppunt van uw [Tijdreeksmodel](./time-series-insights-update-tsm.md).
> * Een **Time Series-ID** tekenreeks voor de eigenschap naam mag maximaal 128 tekens en **Time Series-ID** eigenschapswaarden mag maximaal 1024 tekens.
> * Als een unieke **Time Series-ID** eigenschapswaarden ontbreken, worden deze behandeld als null-waarden, die Neem deel aan de beperking voor uniekheid.

Bovendien kunt u maximaal **drie** (3) sleuteleigenschappen als uw **Time Series-ID**.

  > [!NOTE]
  > Uw **drie** (3) sleuteleigenschappen moeten tekenreeksen zijn.

De volgende scenario's beschreven voor het selecteren van meer dan één sleuteleigenschap als uw **Time Series-ID**:  

* Scenario 1:

  * U hebt een verouderde vloten van assets die elk unieke sleutels hebben. 
  * Bijvoorbeeld, een vloot uniek wordt geïdentificeerd door de eigenschap **deviceId** en een andere waarbij de eigenschap uniek is **objectId**.  In beide vloten is van de andere vloot unieke eigenschap niet aanwezig. In dit voorbeeld selecteert u twee sleutels **deviceId**, en **objectId** als unieke sleutels. 
  * We accepteren null-waarden en het ontbreken van de aanwezigheid van een eigenschap in de nettolading wordt geteld als een `null` waarde.  Dit moet ook de juiste methode voor het afhandelen van verzenden van gegevens naar bronnen van twee verschillende gebeurtenissen waar de gegevens in de bron van elke gebeurtenis een andere heeft unieke **Time Series-ID**.

* Scenario 2:

  * U moet meerdere eigenschappen uniekheid weergeven in de dezelfde vloot van activa. Stel dat bijvoorbeeld, u bent een fabrikant slimme gebouwen en sensoren in elke ruimte implementeren. In elke ruimte, hebt u doorgaans de dezelfde waarden voor **sensorId**, waaronder **sensor1**, **sensor2**, **sensor3**, enzovoort.
  * Daarnaast hebt u overlappende floor en ruimte getallen tussen sites in de eigenschap **flrRm** waarin waarden zoals `1a`, `2b`, `3a`, enzovoort.
  * Ten slotte hebt u een eigenschap **locatie** die waarden zoals bevat `Redmond`, `Barcelona`, `Tokyo`, enzovoort. Voor het maken van uniekheid, zou u alle drie deze eigenschappen als aanwijzen uw **Time Series-ID** sleutels – **sensorId**, **flrRm**, en **locatie**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [gegevensmodellering](./time-series-insights-update-tsm.md).

Plan uw [Azure Time Series Insights (Preview) omgeving](./time-series-insights-update-plan.md).