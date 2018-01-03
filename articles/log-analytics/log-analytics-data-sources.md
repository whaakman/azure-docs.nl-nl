---
title: Gegevensbronnen configureren in Azure Log Analytics | Microsoft Docs
description: "De gegevens of Log Analytics verzamelt van agents en andere bronnen verbonden definiëren gegevensbronnen  Dit artikel wordt het concept van hoe Log Analytics maakt gebruik van gegevensbronnen, worden de details van het configureren ervan uitgelegd en bevat een samenvatting van de verschillende beschikbare gegevensbronnen."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/19/2017
ms.author: bwren
ms.openlocfilehash: 4237df0934d6191b77ff82c86a66585e72191ac9
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="data-sources-in-log-analytics"></a>Gegevensbronnen in Log Analytics
Log Analytics verzamelt gegevens van uw verbonden bronnen en slaat ze op in de werkruimte voor logboekanalyse.  De gegevens die worden verzameld van elk wordt gedefinieerd door de gegevensbronnen die u configureert.  Gegevens in Log Analytics wordt opgeslagen als een reeks records.  Records van een bepaald type elke gegevensbron gemaakt met elk type met een eigen set eigenschappen.

![Meld u gegevensverzameling Analytics](./media/log-analytics-data-sources/overview.png)

Gegevensbronnen zijn anders dan [beheeroplossingen](log-analytics-add-solutions.md), die ook gegevens verzamelen van verbonden bronnen en records maken in logboekanalyse.  Naast het verzamelen van gegevens, omvatten oplossingen logboek zoekopdrachten en weergaven voor hulp bij het analyseren van de werking van een bepaalde toepassing of service.


## <a name="summary-of-data-sources"></a>Samenvatting van gegevensbronnen
De gegevensbronnen die momenteel beschikbaar in logboekanalyse zijn worden weergegeven in de volgende tabel.  Elk heeft een koppeling naar een apart artikel bieden details voor de gegevensbron.

| Gegevensbron | Gebeurtenistype | Beschrijving |
|:--- |:--- |:--- |
| [Aangepaste logboeken](log-analytics-data-sources-custom-logs.md) |\<Logboeknaam\>_CL |Tekstbestanden op Windows of Linux-agents met de informatie in het foutenlogboek. |
| [Windows-gebeurtenislogboeken](log-analytics-data-sources-windows-events.md) |Gebeurtenis |Gebeurtenissen verzameld van de gebeurtenis aanmelding Windows-computers. |
| [Windows-prestatiemeteritems](log-analytics-data-sources-performance-counters.md) |Perf |Prestatiemeteritems van Windows-computers worden verzameld. |
| [Linux-prestatiemeteritems](log-analytics-data-sources-performance-counters.md) |Perf |Prestatiemeteritems worden verzameld van Linux-computers. |
| [IIS-logboeken](log-analytics-data-sources-iis-logs.md) |W3CIISLog |Internet Information Services registreert W3C-indeling. |
| [Syslog](log-analytics-data-sources-syslog.md) |Syslog |Syslog-gebeurtenissen op Windows of Linux-computers. |

## <a name="configuring-data-sources"></a>Gegevensbronnen configureren
Configureren van gegevensbronnen van de **gegevens** menu van logboekanalyse **geavanceerde instellingen**.  Een willekeurige configuratie is geleverd aan alle verbonden bronnen in uw werkruimte.  U uitsluiten niet alle agents momenteel van deze configuratie.

![Windows-gebeurtenissen configureren](./media/log-analytics-data-sources/configure-events.png)

1. Selecteer in de Azure-portal **logboekanalyse** > uw werkruimte > **geavanceerde instellingen**.
2. Selecteer **gegevens**.
3. Klik op de gegevensbron die u wilt configureren.
4. Volg de koppeling naar de documentatie voor elke gegevensbron in de bovenstaande tabel voor meer informatie op hun configuratie.


## <a name="data-collection"></a>Gegevensverzameling
Data source-configuraties worden geleverd aan agents die rechtstreeks zijn verbonden met logboekanalyse binnen een paar minuten.  De opgegeven gegevens worden verzameld van de agent en rechtstreeks aan Log Analytics wordt geleverd met tussenpozen die specifiek zijn voor elke gegevensbron.  Zie de documentatie voor elke gegevensbron voor deze specifieke informatie.

System Center Operations Manager-agent in een verbonden beheergroep, data source-configuraties omgezet in management packs en geleverd aan de beheergroep om de 5 minuten standaard.  De agent downloadt van het management pack zoals elke andere en de opgegeven gegevens verzamelt. De gegevens worden dat ofwel verzonden naar een beheerserver die de gegevens naar het Log Analytics doorstuurt, afhankelijk van de gegevensbron of verzendt de agent de gegevens met logboekanalyse zonder tussenkomst van de beheerserver. Raadpleeg [verzameling Gegevensdetails](log-analytics-add-solutions.md#data-collection-details) voor meer informatie.  Meer informatie vindt u meer informatie over het verbinden van Operations Manager en Log Analytics en het wijzigen van de frequentie die configuratie wordt geleverd bij [integratie met System Center Operations Manager configureren](log-analytics-om-agents.md).

Als de agent kan niet verbinden met Log Analytics of Operations Manager, blijft het verzamelen van gegevens die u ermee kunt wanneer deze een verbinding tot stand brengt.  Als de hoeveelheid gegevens de maximale cachegrootte bereikt voor de client, of als de agent kan geen verbinding maken binnen 24 uur, kunnen gegevens verloren zijn.

## <a name="log-analytics-records"></a>Log Analytics-records
Alle gegevens die worden verzameld door Log Analytics wordt opgeslagen in de werkruimte als records.  Records verzameld door andere gegevensbronnen hebben hun eigen set eigenschappen en worden aangeduid met hun **Type** eigenschap.  Zie de documentatie voor elke gegevensbron en de oplossing voor meer informatie over elk recordtype.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [oplossingen](log-analytics-add-solutions.md) die functionaliteit toevoegen aan logboekanalyse en gegevens ook in de werkruimte te verzamelen.
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) om de gegevens verzameld van gegevensbronnen en oplossingen te analyseren.  
* Configureer [waarschuwingen](log-analytics-alerts.md) proactief om u te waarschuwen van kritieke gegevens verzameld van gegevensbronnen en oplossingen.
