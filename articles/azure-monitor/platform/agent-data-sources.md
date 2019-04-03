---
title: Gegevensbronnen van de agent configureren in Azure Monitor | Microsoft Docs
description: Gegevensbronnen definiëren de logboekgegevens Azure Monitor verzameld van agents en andere bronnen die zijn verbonden.  Dit artikel beschrijft het begrip van hoe Azure Monitor maakt gebruik van gegevensbronnen, worden de details van het configureren van deze uitgelegd en bevat een samenvatting van de verschillende gegevensbronnen beschikbaar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2018
ms.author: bwren
ms.openlocfilehash: d7d4aa89c4dcf2ac9cc0c393e0481cae1f3aeaf2
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58847006"
---
# <a name="agent-data-sources-in-azure-monitor"></a>Agent-gegevensbronnen in Azure Monitor
De gegevens die door Azure Monitor worden verzameld van agents wordt gedefinieerd door de gegevensbronnen die u configureert.  De gegevens van agents wordt opgeslagen als [logboekgegevens](data-platform-logs.md) met een set records.  Elke gegevensbron maakt van een bepaald type-records met elk type met een eigen set eigenschappen.

![Logboek gegevens verzamelen](media/agent-data-sources/overview.png)

## <a name="summary-of-data-sources"></a>Overzicht van gegevensbronnen
De volgende tabel geeft een lijst van de agent-gegevensbronnen die momenteel beschikbaar in Azure Monitor zijn.  Elk heeft een koppeling naar een apart artikel geven details voor de gegevensbron.   Het bevat ook informatie over hun methode en de frequentie van de verzameling. 


| Gegevensbron | Platform | Microsoft monitoring agent | Operations Manager-agent | Azure Storage | Operations Manager vereist? | Operations Manager-agent gegevens verzonden via de beheergroep | Verzamelingsfrequentie |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [Aangepaste logboeken](data-sources-custom-logs.md) | Windows |&#8226; |  | |  |  | bij ontvangst |
| [Aangepaste logboeken](data-sources-custom-logs.md) | Linux   |&#8226; |  | |  |  | bij ontvangst |
| [IIS-logboeken](data-sources-iis-logs.md) | Windows |&#8226; |&#8226; |&#8226; |  |  |afhankelijk van de instelling Rollover voor logboekbestand |
| [Prestatiemeteritems](data-sources-performance-counters.md) | Windows |&#8226; |&#8226; |  |  |  |Als gepland, minimaal 10 seconden |
| [Prestatiemeteritems](data-sources-performance-counters.md) | Linux |&#8226; |  |  |  |  |Als gepland, minimaal 10 seconden |
| [Syslog](data-sources-syslog.md) | Linux |&#8226; |  |  |  |  |uit Azure storage: 10 minuten. van agent: bij aankomst |
| [Windows-gebeurtenislogboeken](data-sources-windows-events.md) |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; | bij ontvangst |


## <a name="configuring-data-sources"></a>Gegevensbronnen configureren
Configureren van gegevensbronnen van de **gegevens** in het menu in **geavanceerde instellingen** voor de werkruimte.  Elke configuratie is afgeleverd bij alle verbonden bronnen in uw werkruimte.  U uitsluiten niet op dit moment geen agents van deze configuratie.

![Windows-gebeurtenissen configureren](media/agent-data-sources/configure-events.png)

1. Selecteer in de Azure portal, **Log Analytics-werkruimten** > uw werkruimte > **geavanceerde instellingen**.
2. Selecteer **gegevens**.
3. Klik op de gegevensbron die u wilt configureren.
4. Volg de koppeling naar de documentatie voor elke gegevensbron in de bovenstaande tabel voor meer informatie over de configuratie.


## <a name="data-collection"></a>Gegevensverzameling
Data source-configuraties geleverd aan agents die rechtstreeks zijn verbonden met Azure Monitor binnen een paar minuten.  De opgegeven gegevens worden verzameld van de agent en rechtstreeks aan Azure Monitor worden geleverd met tussenpozen die specifiek zijn voor elke gegevensbron.  Zie de documentatie voor elke gegevensbron voor deze specifieke informatie.

Voor System Center Operations Manager-agents in een verbonden beheergroep, worden de gegevensbronconfiguraties vertaald in management packs en geleverd aan de beheergroep om de 5 minuten standaard.  De agent downloadt van het managementpack, zoals elk ander en de opgegeven gegevens verzamelt. Afhankelijk van de gegevensbron, de gegevens worden dat beide verzonden naar een beheerserver die de gegevens naar de Azure Monitor doorstuurt of de agent verzendt de gegevens naar Azure Monitor zonder tussenkomst van de beheerserver. Zie [details van de verzameling gegevens voor het bewaken van oplossingen in Azure](../insights/solutions-inventory.md) voor meer informatie.  U kunt lezen over de details van het verbinden van Operations Manager en Azure Monitor en wijzigen van de frequentie die configuratie wordt geleverd bij [integratie configureren met System Center Operations Manager](om-agents.md).

Als de agent, kan geen verbinding maken met Azure Monitor of Operations Manager, blijft het verzamelen van gegevens die deze leveren wanneer er een verbinding tot stand brengt.  Gegevens kunnen worden verbroken als de hoeveelheid gegevens bereikt de maximale cachegrootte voor de client, of als de agent niet kan geen verbinding binnen 24 uur.

## <a name="log-records"></a>Records in logboek registreren
Alle logboekgegevens die zijn verzameld door Azure Monitor is in de werkruimte opgeslagen als records.  Records die zijn verzameld door andere gegevensbronnen hebben hun eigen set eigenschappen en worden aangeduid met hun **Type** eigenschap.  Zie de documentatie voor elke gegevensbron en de oplossing voor meer informatie over elk recordtype.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [bewakingsoplossingen](../insights/solutions.md) die functionaliteit toevoegen aan Azure Monitor en ook gegevens verzamelen in de werkruimte.
* Meer informatie over [query's bijgehouden](../log-query/log-query-overview.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen voor de controle te analyseren.  
* Configureer [waarschuwingen](alerts-overview.md) om proactief te waarschuwen u van kritieke gegevens die worden verzameld van gegevensbronnen en oplossingen voor de controle.