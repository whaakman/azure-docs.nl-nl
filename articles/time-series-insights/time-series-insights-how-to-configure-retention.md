---
title: Bewaar termijn configureren in uw Azure Time Series Insights omgeving | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de Bewaar periode in uw Azure Time Series Insights omgeving kunt configureren.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 08/09/2019
ms.custom: seodec18
ms.openlocfilehash: e5cc1489af1dce3a9a57b96a3240c63e0395c33a
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2019
ms.locfileid: "68947230"
---
# <a name="configuring-retention-in-time-series-insights"></a>Bewaar periode in Time Series Insights configureren

In dit artikel wordt beschreven hoe u de **Bewaar tijd voor gegevens** en de **opslag limiet hebt overschreden** in azure time series Insights.

## <a name="summary"></a>Samenvatting

Elke Time Series Insights-omgeving (TSI) heeft een instelling voor het configureren van **gegevens retentie tijd**. De waarde ligt tussen 1 en 400 dagen. De gegevens worden verwijderd op basis van de opslag capaciteit van de omgeving of de Bewaar periode (1-400), afhankelijk van wat het eerste komt.

Voor elke TSI omgeving is een extra **opslag limiet**ingesteld. Met deze instelling bepaalt u het gedrag van ingang en leegmaken wanneer de maximale capaciteit van een omgeving wordt bereikt. Er zijn twee manieren om te kiezen:

- **Oude gegevens** opschonen prijs
- **Ingangs onderbrekingen**

Lees de informatie [over retentie in time series Insights](time-series-insights-concepts-retention.md)voor gedetailleerde gegevens om deze instellingen beter te begrijpen.  

## <a name="configure-data-retention"></a>Gegevensretentie configureren

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Ga naar uw bestaande Time Series Insights-omgeving. Selecteer **alle resources** in het menu aan de linkerkant van het Azure Portal. Selecteer uw Time Series Insights-omgeving.

1. Selecteer **configureren**onder de kop **instellingen** .

1. Selecteer de **Bewaar tijd voor gegevens** om de retentie te configureren met behulp van de schuif regelaar of typ een getal in het tekstvak.

1. Let op de capaciteits instelling, omdat deze configuratie invloed heeft op de maximale hoeveelheid gegevens gebeurtenissen en de totale opslag capaciteit voor het opslaan van gegevens.

1. De instelling voor het gedrag van de **opslag limiet is overschreden** . Selecteer **oude gegevens leegmaken** of ingangs gedrag **onderbreken** .

1. Selecteer **Opslaan** om de wijzigingen te configureren.

## <a name="next-steps"></a>Volgende stappen

- Lees voor meer informatie [uitleg over retentie in time series Insights](time-series-insights-concepts-retention.md).