---
title: Bewaren configureren in uw Azure Time Series Insights-omgeving | Microsoft Docs
description: In dit artikel wordt beschreven hoe u bewaren in uw Azure Time Series Insights-omgeving configureren.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: 3235844c8750003d08e996f5065fcef256c2d244
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165669"
---
# <a name="configuring-retention-in-time-series-insights"></a>Configuratie van de bewaartermijn in Time Series Insights

In dit artikel wordt beschreven hoe u **gegevensretentietijd** en **-limiet voor opslag overschreden gedrag** in Azure Time Series Insights.

## <a name="summary"></a>Samenvatting

Elke omgeving Time Series Insights (TSI) heeft een instelling voor het configureren van **gegevensretentietijd**. De waarde omvat van 1 tot 400 dagen. De gegevens worden verwijderd op basis van de omgeving capaciteit of bewaring opslagduur (1-400), afhankelijk van wat het eerste komt.

Elke TSI-omgeving heeft een instelling voor extra **-limiet voor opslag overschreden gedrag**. Deze instelling wordt bepaald gedrag van inkomend verkeer en opschonen na het verstrijken van de maximale capaciteit van een omgeving. Er zijn twee gedrag kiezen uit:

- **Verwijderen van oude gegevens** (standaard)
- **Inkomende gegevens onderbreken**

Raadpleeg voor gedetailleerde informatie voor meer informatie over deze instellingen [Understanding bewaren in Time Series Insights](time-series-insights-concepts-retention.md).  

## <a name="configure-data-retention"></a>Gegevensretentie configureren

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Ga naar uw bestaande Time Series Insights-omgeving. Selecteer **alle resources** in het menu aan de linkerkant van de Azure-portal. Selecteer uw Time Series Insights-omgeving.

1. Onder de **instellingen** kop, selecteer **configureren**.

1. Selecteer de **gegevensretentietijd** te configureren van de bewaarperiode met behulp van de schuifregelaar of typ een getal in het tekstvak.

1. Houd er rekening mee de **capaciteit** instellen, aangezien deze configuratie zorgt ervoor dat de maximale hoeveelheid Gegevensgebeurtenissen en de totale opslagcapaciteit voor het opslaan van gegevens.

1. Schakelen tussen de **-limiet voor opslag overschreden gedrag** instelling. Selecteer **oude gegevens opschonen** of **onderbreken inkomend** gedrag.

1. Selecteer **opslaan** het configureren van de wijzigingen.

## <a name="next-steps"></a>Volgende stappen

- Raadpleeg voor meer informatie, [Understanding bewaren in Time Series Insights](time-series-insights-concepts-retention.md).