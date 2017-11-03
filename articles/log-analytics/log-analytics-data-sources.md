---
title: Configureren van gegevensbronnen in OMS Log Analytics | Microsoft Docs
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
ms.date: 05/23/2017
ms.author: bwren
ms.openlocfilehash: 00d030a502cf70ea9a5dea767f560cdf2919573e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="data-sources-in-log-analytics"></a>Gegevensbronnen in Log Analytics
Log Analytics verzamelt gegevens van de verbonden bronnen in de OMS-werkruimte en slaat ze op in de OMS-opslagplaats.  De gegevens die worden verzameld van elk wordt gedefinieerd door de gegevensbronnen die u configureert.  Gegevens in de OMS-bibliotheek is opgeslagen als een reeks records.  Records van een bepaald type elke gegevensbron gemaakt met elk type met een eigen set eigenschappen.

![Meld u gegevensverzameling Analytics](./media/log-analytics-data-sources/overview.png)

Gegevensbronnen zijn anders dan de OMS-oplossingen die ook gegevens verzamelen van verbonden bronnen en records maken in de OMS-opslagplaats.  Oplossingen kunnen worden toegevoegd aan uw werkruimte in de galerie oplossingen en biedt doorgaans aanvullende analyseprogramma's in de OMS-portal.  

## <a name="summary-of-data-sources"></a>Samenvatting van gegevensbronnen
De gegevensbronnen die momenteel beschikbaar in logboekanalyse zijn worden weergegeven in de volgende tabel.  Elk heeft een koppeling naar een apart artikel bieden details voor de gegevensbron.

| Gegevensbron | Gebeurtenistype | Beschrijving |
|:--- |:--- |:--- |
| [Aangepaste logboeken](log-analytics-data-sources-custom-logs.md) |\<Logboeknaam\>_CL |Tekstbestanden op Windows of Linux-agents met de informatie in het foutenlogboek. |
| [Windows-gebeurtenislogboeken](log-analytics-data-sources-windows-events.md) |Gebeurtenis |Gebeurtenissen verzameld van het gebeurtenislogboek op Windows-computers. |
| [Windows-prestatiemeteritems](log-analytics-data-sources-performance-counters.md) |Perf |Prestatiemeteritems van Windows-computers worden verzameld. |
| [Linux-prestatiemeteritems](log-analytics-data-sources-performance-counters.md) |Perf |Prestatiemeteritems worden verzameld van Linux-computers. |
| [IIS-logboeken](log-analytics-data-sources-iis-logs.md) |W3CIISLog |Internet Information Services registreert W3C-indeling. |
| [Syslog](log-analytics-data-sources-syslog.md) |Syslog |Syslog-gebeurtenissen op Windows of Linux-computers. |

## <a name="configuring-data-sources"></a>Gegevensbronnen configureren
Configureren van gegevensbronnen van de **gegevens** menu van logboekanalyse **instellingen**.  Een willekeurige configuratie is geleverd aan alle verbonden bronnen in de OMS-werkruimte.  U uitsluiten niet alle agents momenteel van deze configuratie.

![Windows-gebeurtenissen configureren](./media/log-analytics-data-sources/configure-events.png)

1. Klik in de OMS-console op de **instellingen** tegel of de **instellingen** knop aan de bovenkant van het scherm.
2. Selecteer **gegevens**.
3. Klik op de gegevensbron te configureren.
4. Volg de koppeling naar de documentatie voor elke gegevensbron in de bovenstaande tabel voor meer informatie op hun configuratie.

> [!NOTE]
> U configureren niet op dit moment logboekanalyse gegevensbronnen in de Azure portal.

## <a name="data-collection"></a>Gegevensverzameling
Data source-configuraties worden geleverd aan agents die rechtstreeks zijn verbonden met logboekanalyse binnen een paar minuten.  De opgegeven gegevens worden verzameld van de agent en rechtstreeks aan Log Analytics wordt geleverd met tussenpozen die specifiek zijn voor elke gegevensbron.  Zie de documentatie voor elke gegevensbron voor deze specifieke informatie.

Voor System Center Operations Manager (SCOM) agents in een verbonden beheergroep, data source-configuraties omgezet in management packs en geleverd aan de beheergroep om de 5 minuten standaard.  De agent downloadt van het management pack zoals elke andere en de opgegeven gegevens verzamelt. De gegevens worden dat ofwel verzonden naar een beheerserver die de gegevens naar het Log Analytics doorstuurt, afhankelijk van de gegevensbron of verzendt de agent de gegevens met logboekanalyse zonder tussenkomst van de beheerserver. Raadpleeg [verzameling Gegevensdetails voor OMS-functies en oplossingen](log-analytics-add-solutions.md#data-collection-details) voor meer informatie.  Meer informatie vindt u details van SCOM en OMS verbinden en het wijzigen van de frequentie die configuratie wordt geleverd bij [integratie met System Center Operations Manager configureren](log-analytics-om-agents.md).

Als de agent kan niet verbinden met Log Analytics of Operations Manager, blijft het verzamelen van gegevens die u ermee kunt wanneer deze een verbinding tot stand brengt.  Als de hoeveelheid gegevens de maximale cachegrootte bereikt voor de client, of als de agent kan geen verbinding maken binnen 24 uur, kunnen gegevens verloren zijn.

## <a name="log-analytics-records"></a>Log Analytics-records
Alle gegevens die worden verzameld door Log Analytics wordt opgeslagen in de OMS-opslagplaats als records.  Records verzameld door andere gegevensbronnen hebben hun eigen set eigenschappen en worden aangeduid met hun **Type** eigenschap.  Zie de documentatie voor elke gegevensbron en de oplossing voor meer informatie over elk recordtype.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [oplossingen](log-analytics-add-solutions.md) functionaliteit toevoegen aan Log Analytics en ook het verzamelen van gegevens in de OMS-opslagplaats.
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) om de gegevens verzameld van gegevensbronnen en oplossingen te analyseren.  
* Configureer [waarschuwingen](log-analytics-alerts.md) proactief om u te waarschuwen van kritieke gegevens verzameld van gegevensbronnen en oplossingen.
