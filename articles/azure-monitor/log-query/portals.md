---
title: Weergeven en analyseren van logboekgegevens in Azure Monitor | Microsoft Docs
description: Dit artikel wordt beschreven met behulp van Log Analytics in Azure portal maken en bewerken van logboekquery's in Azure Monitor.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/22/2018
ms.author: bwren
ms.openlocfilehash: 9567f8a6b581d7c246ebaa8eb8d72ad201bf2641
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55990408"
---
# <a name="viewing-and-analyzing-log-data-in-azure-monitor"></a>Weergeven en analyseren van logboekgegevens in Azure Monitor
Log Analytics is de primaire ervaring voor het werken met gegevens aan het logboek en het maken van query's in Azure Monitor. Log Analytics van Open **logboeken** in de **Azure Monitor** menu. U kunt een inleiding tot deze portal en controleren van de functies op [aan de slag met Log Analytics in Azure portal](get-started-portal.md).

Log Analytics biedt de volgende functies voor het werken met Logboeken-query's.

* Meerdere tabbladen: afzonderlijke tabbladen om te werken met meerdere query's maken.
* Uitgebreide visualisaties – verschillende opties voor grafieken.
* Verbeterde Intellisense en taal automatisch aanvullen.
* Syntaxismarkering – verbetert dit de leesbaarheid van query's. 
* Queryverkenner – toegang opgeslagen query's en functies.
* Schema weergeven: Bekijk de structuur van uw gegevens om te helpen bij het schrijven van query's.
* Delen: maken van koppelingen naar query's of pincode query's naar een gedeelde Azure-dashboard.
* Slimme analyse - identificeert pieken in de grafieken en een snelle analyse van de oorzaak.
* Kolom selecteren – sorteren en groeperen van kolommen in de queryresultaten.

> [!NOTE]
> Log Analytics heeft dezelfde functionaliteit als de portal Advanced Analytics is een extern hulpprogramma buiten de Azure-portal. De portal Advanced Analytics is nog steeds beschikbaar, maar koppelingen en verwijzingen naar deze in Azure portal worden vervangen door deze nieuwe pagina.

![Log Analytics](media/portals/log-analytics.png)

### <a name="resource-logs"></a>Resource-Logboeken
Log Analytics kan worden geïntegreerd met verschillende Azure-resources zoals virtuele Machines. Dit betekent dat u Log Analytics rechtstreeks via de bewaking resourcemenu openen kunt zonder dat u overschakelt naar Azure Monitor en verlies van de context van de resource. **Logboeken** is nog niet is ingeschakeld voor alle Azure-resources, maar deze wordt weergegeven in het menu van de portal voor verschillende resources typen gestart.

Bij het openen van Log Analytics van een specifieke resource, het automatisch afgestemd voor logboekregistratie van records van die resource alleen.   Als u een query schrijven waarmee andere records bevat wilt, zou u nodig hebt om deze te openen in het menu Azure Monitor.

De volgende opties zijn nog niet beschikbaar via de weergave van resources van Log Analytics:

- Opslaan
- waarschuwing instellen
- Queryverkenner
- Overschakelen naar een andere werkruimte/resource (momenteel niet gepland)


### <a name="firewall-requirements"></a>Firewall-vereisten
Uw browser vereist toegang tot de volgende adressen voor toegang tot Log Analytics.  Als uw browser de Azure-portal via een firewall openen is, moet u toegang tot deze adressen inschakelen.

| URI | IP | Poorten |
|:---|:---|:---|
| portal.loganalytics.io | Dynamisch | 80,443 |
| api.loganalytics.io    | Dynamisch | 80,443 |
| docs.loganalytics.io   | Dynamisch | 80,443 |


## <a name="log-search-classic"></a>Zoeken in Logboeken (klassiek)
Zoeken in Logboeken is de traditionele ervaring in Azure portal voor het uitvoeren van query's en analyseren van logboekgegevens in Azure Monitor. Dit wordt binnenkort stopgezet, maar is momenteel nog steeds beschikbaar. Zoeken in Logboeken van Open **Logboeken (klassiek)** in het menu van Log Analytics.



![Zoeken in logboeken](media/portals/log-search-portal.png)


## <a name="next-steps"></a>Volgende stappen

- Doorloop een [zelfstudie met behulp van Log Analytics](../../azure-monitor/log-query/get-started-portal.md).
- Doorloop een [zelfstudie over het zoeken in Logboeken met](../../azure-monitor/learn/tutorial-viewdata.md).

