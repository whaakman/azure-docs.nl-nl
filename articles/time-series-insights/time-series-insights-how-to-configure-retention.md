---
title: Het configureren van bewaren in uw omgeving Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe bewaren in uw omgeving Azure Time Series Insights configureren.
ms.service: time-series-insights
services: time-series-insights
author: anshan
ms.author: anshan
manager: kfile
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 02/09/2018
ms.openlocfilehash: 9a236ba500af36e8fb1c8c089389ffd74b9d2abb
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34653916"
---
# <a name="configuring-retention-in-time-series-insights"></a>Configuratie van de bewaartermijn in tijd reeks inzichten
In dit artikel wordt beschreven hoe u **gegevensretentietijd** en **opslaglimiet overschreden gedrag** in Azure Time Series Insights.

Elke omgeving Time Series Insights (TSI) heeft een instelling voor het configureren van **gegevensretentietijd**. De waarde omvat van 1 tot 400 dagen. De gegevens worden verwijderd op basis van de omgeving capaciteit of bewaren opslagduur (1-400), afhankelijk van wat zich het eerste voordoet.

Elke omgeving TSI heeft een extra instelling **opslaglimiet overschreden gedrag**. Deze instelling bepaalt inkomende en opschonen gedrag als de maximale capaciteit van een omgeving is bereikt. Er zijn twee gedrag kunt kiezen uit:
- **Oude gegevens verwijderen** (standaard)  
- **Inkomend onderbreken**

Raadpleeg voor gedetailleerde informatie voor een beter begrip van deze instellingen [Understanding bewaren in tijd reeks inzichten](time-series-insights-concepts-retention.md).  

## <a name="configure-data-retention"></a>Gegevensretentie configureren

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoek uw bestaande Time Series Insights-omgeving. Selecteer **alle resources** in het menu aan de linkerkant van de Azure-portal. Selecteer uw Time Series Insights-omgeving.

3. Onder de **instellingen** kop, selecteer **configureren**.

4. Selecteer de **gegevensretentietijd** configureren van de retentie met behulp van de schuifregelaar of typ een getal in het tekstvak.

5. Opmerking de **capaciteit** instellen, aangezien deze configuratie heeft impact op de maximale hoeveelheid Gegevensgebeurtenissen en capaciteit van de totale opslagruimte voor het opslaan van gegevens. 

6. Schakelen tussen de **opslaglimiet overschreden gedrag** instelling. Selecteer **oude gegevens verwijderen** of **onderbreken inkomend** gedrag.

7. Selecteer **opslaan** voor het configureren van de wijzigingen.

## <a name="next-steps"></a>Volgende stappen
Raadpleeg voor meer informatie [Understanding bewaren in tijd reeks inzichten](time-series-insights-concepts-retention.md).
