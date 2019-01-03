---
title: Aanbevolen procedures voor het kiezen van een Time Series-ID in Azure Time Series Insights-Preview | Microsoft Docs
description: Informatie over aanbevolen procedures als u ervoor een Time Series-ID in Azure Time Series Insights Preview kiest.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: 44c6c8a55bbcbf76cf48fd17a6e52d188a5e99fc
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53556608"
---
# <a name="best-practices-for-choosing-a-time-series-id"></a>Aanbevolen procedures voor het kiezen van een Time Series-ID

In dit artikel bevat informatie over de Azure Time Series Insights Preview partitiesleutel, de Time Series-ID en de aanbevolen procedures voor het kiezen van een.

## <a name="choose-a-time-series-id"></a>Een Time Series-id kiezen

Het kiezen van een Time Series-ID is, zoals een partitiesleutel voor een database kiezen. Er is een belangrijke beslissing die moet worden gemaakt tijdens de ontwerpfase. U kunt een bestaande Time Series Insights Preview-omgeving voor het gebruik van een andere tijd reeks id niet bijwerken Wanneer u een omgeving is gemaakt met een Time Series-ID, is het beleid met andere woorden, een onveranderbare eigenschap die niet kan worden gewijzigd.

> [!IMPORTANT]
> De Time Series-ID is hoofdlettergevoelig en onveranderbare (deze kan niet worden gewijzigd nadat deze is ingesteld).

Met die in gedachten is het essentieel de juiste Time Series-ID selecteren. Wanneer u een Time Series-ID selecteert, houd rekening met de volgende deze aanbevolen procedures:
* Kies de naam van een eigenschap die heeft een breed scala aan waarden en zelfs toegangspatronen. Het is een aanbevolen procedure om een partitiesleutel met veel verschillende waarden (bijvoorbeeld, honderden of duizenden). Voor veel klanten is dit iets, zoals de apparaat-id of SensorID in uw JSON.
* De Time Series-ID moet uniek zijn op het niveau van de leaf-knooppunt van uw [Tijdreeksmodel](./time-series-insights-update-tsm.md).
* Een tekenreeks voor naam van Time Series-ID-eigenschap kan maximaal 128 tekens hebben kunnen, en eigenschapswaarden van Time Series-ID maximaal 1024 tekens.
* Als sommige unieke waarden voor Time Series-ID-eigenschap ontbreekt, wordt deze behandeld als null-waarden, die Neem deel aan de beperking voor uniekheid.

Bovendien kunt u maximaal *drie* (3) sleuteleigenschappen als uw Time Series-ID.

  > [!NOTE]
  > Uw *drie* (3) sleuteleigenschappen moeten tekenreeksen zijn.

De volgende scenario's beschreven meer dan één sleuteleigenschap selecteren als uw Time Series-ID:  

### <a name="scenario-1"></a>Scenario 1

* U hebt een verouderde vloten van assets, elk met een unieke sleutel. 
* Bijvoorbeeld, een vloot uniek wordt geïdentificeerd door de eigenschap *deviceId* en een andere waarbij de eigenschap uniek is *objectId*. Geen van beide vloot bevat de unieke eigenschap van de andere vloot. In dit voorbeeld zou u twee sleutels, de apparaat-id en de object-id selecteren als unieke sleutels. 
* We accepteren null-waarden en het ontbreken van de aanwezigheid van een eigenschap in de nettolading telt als een `null` waarde. Dit is ook de juiste methode voor het afhandelen van verzenden van gegevens naar bronnen van twee verschillende gebeurtenissen waarin de gegevens in de bron van elke gebeurtenis een unieke Time Series-ID heeft.

### <a name="scenario-2"></a>Scenario 2

* U moet meerdere eigenschappen die moeten uniek zijn binnen de dezelfde vloot van activa. 
* Stel dat bijvoorbeeld, u bent een fabrikant slimme gebouwen en sensoren in elke ruimte implementeren. In elke ruimte, hebt u doorgaans de dezelfde waarden voor *sensorId*, zoals *sensor1*, *sensor2*, en *sensor3*.
* Bovendien uw gebouw heeft overlappende floor en ruimte getallen tussen sites in de eigenschap *flrRm*, zoals waarden hebben *1a*, *2b*, *3a* , enzovoort.
* Ten slotte hebt u een eigenschap *locatie*, die de waarden zoals bevat *Redmond*, *Barcelona*, en *Tokio*. Voor het maken van uniekheid, zou u de volgende drie eigenschappen aanwijzen als de sleutels van uw Time Series-ID: *sensorId*, *flrRm*, en *locatie*.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [gegevensmodellering](./time-series-insights-update-tsm.md).
* Plan uw [Azure Time Series Insights (preview) omgeving](./time-series-insights-update-plan.md).
