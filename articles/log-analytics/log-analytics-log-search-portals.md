---
title: Weergeven en analyseren van gegevens in Azure Log Analytics | Microsoft Docs
description: In dit artikel beschrijft de portals die u kunt gebruiken in de logboeken van Azure Log Analytics maken en bewerken.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 8fdf1bed0b75111abef4579565698f0c48b5d843
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52724142"
---
# <a name="viewing-and-analyzing-data-in-log-analytics"></a>Weergeven en analyseren van gegevens in Log Analytics
Er zijn twee opties beschikbaar zijn in de Azure-portal voor het analyseren van gegevens die zijn opgeslagen in Log analytics en voor het maken van query's voor ad-hoc-analyse. De query's die u maakt gebruik van deze portals kunnen worden gebruikt voor andere onderdelen, zoals waarschuwingen en dashboards.

## <a name="log-analytics-page"></a>Log Analytics-pagina
Open de pagina logboekanalyse van **logboeken** in het menu van Log Analytics. Dit is een nieuwe ervaring voor het werken met gegevens aan het logboek en het maken van query's. U kunt een inleiding tot deze portal en controleren van de functies op [aan de slag met de Log Analytics-pagina in de Azure-portal](query-language/get-started-analytics-portal.md).

De pagina Log Analytics biedt de volgende verbeteringen ten opzichte van de [zoeken in Logboeken (klassiek)](#log-search-classic) optreden.

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
> De Log Analytics-pagina heeft dezelfde functionaliteit als de portal Advanced Analytics is een extern hulpprogramma buiten de Azure-portal. De portal Advanced Analytics is nog steeds beschikbaar, maar koppelingen en verwijzingen naar deze in Azure portal worden vervangen door deze nieuwe pagina.

![Geavanceerde Analytics-portal](media/log-analytics-log-search-portals/advanced-analytics-portal.png)

### <a name="resource-logs"></a>Resource-Logboeken
De nieuwe ervaring voor Log Analytics kan worden geïntegreerd met verschillende Azure-resources zoals virtuele Machines. Dit betekent dat u kunt de pagina van de Log Analytics rechtstreeks via bewaking van de resource-menu openen zonder overstappen naar Azure Monitor of Log Analytics en het verlies van de context van de resource. **Logboeken** is nog niet is ingeschakeld voor alle Azure-resources, maar deze wordt weergegeven in het menu van de portal voor verschillende resources typen gestart.

Bij het openen van Log Analytics van een specifieke resource, het automatisch afgestemd voor logboekregistratie van records van die resource alleen.   Als u een query schrijven waarmee andere records bevat wilt, zou u nodig hebt om deze te openen vanuit het menu Log Analytics of Azure Monitor.

De volgende opties zijn nog niet beschikbaar via de weergave van resources van Log Analytics:

- Opslaan
- waarschuwing instellen
- Queryverkenner
- Overschakelen naar een andere werkruimte/resource (momenteel niet gepland)


### <a name="firewall-requirements"></a>Firewall-vereisten
Uw browser vereist toegang tot de volgende adressen voor toegang tot de Log Analytics-pagina en de portal Advanced Analytics.  Als uw browser de Azure-portal via een firewall openen is, moet u toegang tot deze adressen inschakelen.

| URI | IP | Poorten |
|:---|:---|:---|
| portal.loganalytics.io | Dynamisch | 80,443 |
| api.loganalytics.io    | Dynamisch | 80,443 |
| docs.loganalytics.io   | Dynamisch | 80,443 |


## <a name="log-search-classic"></a>Zoeken in Logboeken (klassiek)
Open de pagina voor het zoeken van logboek van **Logboeken (klassiek)** in het menu van Log Analytics of van **Log Analytics** in het menu van Azure Monitor. Dit is de klassieke pagina die wordt gebruikt voor het werken met Log Analytics-query's die beschikt niet over de aanvullende functies van de [pagina logboekanalyse](#log-analytics-page) hierboven vermeld.



![Meld u pagina zoeken](media/log-analytics-log-search-portals/log-search-portal.png)


## <a name="next-steps"></a>Volgende stappen

- Doorloop een [zelfstudie over het zoeken in Logboeken met](log-analytics-tutorial-viewdata.md) voor informatie over het maken van query's met behulp van de querytaal
- Doorloop een [les met behulp van de portal Advanced Analytics](query-language/get-started-analytics-portal.md) waarmee u op dezelfde manier werken als de Log Analytics-pagina.

