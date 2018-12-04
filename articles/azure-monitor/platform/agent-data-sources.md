---
title: Gegevensbronnen in Azure Log Analytics configureren | Microsoft Docs
description: Gegevensbronnen definiëren de gegevens die Log Analytics verzamelt van agents en andere bronnen die zijn verbonden.  Dit artikel beschrijft het begrip van hoe Log Analytics maakt gebruik van gegevensbronnen, worden de details van het configureren van deze uitgelegd en bevat een samenvatting van de verschillende gegevensbronnen beschikbaar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 022dd52e05b9227e3ce0920a083f75413a2eebc8
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52836913"
---
# <a name="data-sources-in-log-analytics"></a>Gegevensbronnen in Log Analytics
Log Analytics verzamelt gegevens van uw verbonden bronnen en slaat ze op in uw Log Analytics-werkruimte.  De gegevens die worden verzameld van elke wordt gedefinieerd door de gegevensbronnen die u configureert.  Gegevens in Log Analytics wordt opgeslagen als een set records.  Elke gegevensbron maakt van een bepaald type-records met elk type met een eigen set eigenschappen.

![Analytics-gegevens verzamelen](./media/agent-data-sources/overview.png)

Gegevensbronnen zijn anders dan [beheeroplossingen](../../azure-monitor/insights/solutions.md), die ook gegevens verzamelen uit de verbonden bronnen en -records maken in Log Analytics.  Naast het verzamelen van gegevens, omvatten oplossingen zoekopdrachten in Logboeken en weergaven om u te helpen u bij het analyseren van de werking van een bepaalde toepassing of service.


## <a name="summary-of-data-sources"></a>Overzicht van gegevensbronnen
De volgende tabel bevat de gegevensbronnen die momenteel beschikbaar in Log Analytics zijn.  Elk heeft een koppeling naar een apart artikel geven details voor de gegevensbron.   Het bevat ook informatie over hun methode en de frequentie van verzamelen van gegevens in Log Analytics.  Gebruik de informatie in dit artikel om te identificeren van de verschillende oplossingen beschikbaar en om te begrijpen van de stroom en verbinding eisen voor verschillende oplossingen. Zie voor een uitleg van de kolommen [details van de verzameling gegevens voor de beheeroplossingen in Azure](../../azure-monitor/insights/solutions-inventory.md).


| Gegevensbron | Platform | Microsoft monitoring agent | Operations Manager-agent | Azure Storage | Operations Manager vereist? | Operations Manager-agent gegevens verzonden via de beheergroep | Verzamelingsfrequentie |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Aangepaste logboeken](data-sources-custom-logs.md) | Windows |&#8226; |  | |  |  | bij ontvangst |
| [Aangepaste logboeken](data-sources-custom-logs.md) | Linux   |&#8226; |  | |  |  | bij ontvangst |
| [IIS-logboeken](data-sources-iis-logs.md) | Windows |&#8226; |&#8226; |&#8226; |  |  |afhankelijk van de instelling Rollover voor logboekbestand |
| [Prestatiemeteritems](data-sources-performance-counters.md) | Windows |&#8226; |&#8226; |  |  |  |Als gepland, minimaal 10 seconden |
| [Prestatiemeteritems](data-sources-performance-counters.md) | Linux |&#8226; |  |  |  |  |Als gepland, minimaal 10 seconden |
| [Syslog](data-sources-syslog.md) | Linux |&#8226; |  |  |  |  |uit Azure storage: 10 minuten. van agent: bij aankomst |
| [Windows-gebeurtenislogboeken](data-sources-windows-events.md) |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; | bij ontvangst |


## <a name="configuring-data-sources"></a>Gegevensbronnen configureren
Configureren van gegevensbronnen van de **gegevens** in het menu in Log Analytics **geavanceerde instellingen**.  Elke configuratie is afgeleverd bij alle verbonden bronnen in uw werkruimte.  U uitsluiten niet op dit moment geen agents van deze configuratie.

![Windows-gebeurtenissen configureren](./media/agent-data-sources/configure-events.png)

1. Selecteer in de Azure portal, **Log Analytics** > uw werkruimte > **geavanceerde instellingen**.
2. Selecteer **gegevens**.
3. Klik op de gegevensbron die u wilt configureren.
4. Volg de koppeling naar de documentatie voor elke gegevensbron in de bovenstaande tabel voor meer informatie over de configuratie.


## <a name="data-collection"></a>Gegevensverzameling
Data source-configuraties geleverd aan agents die rechtstreeks zijn verbonden met Log Analytics binnen een paar minuten.  De opgegeven gegevens zijn verzameld van de agent en rechtstreeks aan Log Analytics wordt geleverd met tussenpozen die specifiek zijn voor elke gegevensbron.  Zie de documentatie voor elke gegevensbron voor deze specifieke informatie.

Voor System Center Operations Manager-agents in een verbonden beheergroep, worden de gegevensbronconfiguraties vertaald in management packs en geleverd aan de beheergroep om de 5 minuten standaard.  De agent downloadt van het managementpack, zoals elk ander en de opgegeven gegevens verzamelt. De gegevens worden dat beide verzonden naar een beheerserver die de gegevens naar de met Log Analytics doorstuurt, afhankelijk van de gegevensbron, of de agent verzendt de gegevens naar Log Analytics zonder tussenkomst van de beheerserver. Zie [details van de verzameling gegevens voor de beheeroplossingen in Azure](../../azure-monitor/insights/solutions-inventory.md) voor meer informatie.  U kunt lezen over de details van het verbinden van Operations Manager en Log Analytics en het wijzigen van de frequentie die configuratie wordt geleverd bij [integratie configureren met System Center Operations Manager](../../log-analytics/log-analytics-om-agents.md).

Als de agent kan geen verbinding met Log Analytics of Operations Manager, blijft het verzamelen van gegevens die deze leveren wanneer er een verbinding tot stand brengt.  Gegevens kunnen worden verbroken als de hoeveelheid gegevens bereikt de maximale cachegrootte voor de client, of als de agent niet kan geen verbinding binnen 24 uur.

## <a name="log-analytics-records"></a>Log Analytics-records
Alle gegevens die zijn verzameld door Log Analytics is in de werkruimte opgeslagen als records.  Records die zijn verzameld door andere gegevensbronnen hebben hun eigen set eigenschappen en worden aangeduid met hun **Type** eigenschap.  Zie de documentatie voor elke gegevensbron en de oplossing voor meer informatie over elk recordtype.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [oplossingen](../../azure-monitor/insights/solutions.md) die functionaliteit toevoegen aan Log Analytics en ook gegevens verzamelen in de werkruimte.
* Meer informatie over [zoekopdrachten](../../azure-monitor/log-query/log-query-overview.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen te analyseren.  
* Configureer [waarschuwingen](../../monitoring-and-diagnostics/monitoring-overview-alerts.md) om proactief te waarschuwen u van kritieke gegevens die worden verzameld van gegevensbronnen en oplossingen.
