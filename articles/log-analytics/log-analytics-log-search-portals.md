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
ms.devlang: na
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: magoedte; bwren
ms.component: na
ms.openlocfilehash: 386aad94461fa3f2ceafb7564342797eefa2f086
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2018
ms.locfileid: "42060854"
---
# <a name="viewing-and-analyzing-data-in-log-analytics"></a>Weergeven en analyseren van gegevens in Log Analytics
Er zijn twee opties beschikbaar zijn in de Azure-portal voor het analyseren van gegevens die zijn opgeslagen in Log analytics en voor het maken van query's voor ad-hoc-analyse. De query's die u maakt gebruik van deze portals kunnen worden gebruikt voor andere onderdelen, zoals waarschuwingen en dashboards.

## <a name="log-analytics-page-preview"></a>Pagina voor log Analytics (preview)
Open de pagina logboekanalyse van **Logboeken (preview)** in het menu van Log Analytics. Dit is een nieuwe ervaring voor het werken met gegevens aan het logboek en het maken van query's. U kunt een inleiding tot deze portal en controleren van de functies op [aan de slag met de Log Analytics-pagina in de Azure-portal](query-language/get-started-analytics-portal.md).

De pagina Log Analytics biedt de volgende verbeteringen ten opzichte van de [zoeken in logboeken](#log-search) optreden.

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


### <a name="firewall-requirements"></a>Firewall-vereisten
Uw browser vereist toegang tot de volgende adressen voor toegang tot de Log Analytics-pagina en de portal Advanced Analytics.  Als uw browser de Azure-portal via een firewall openen is, moet u toegang tot deze adressen inschakelen.

| URI | IP | Poorten |
|:---|:---|:---|
| portal.loganalytics.io | Dynamisch | 80,443 |
| api.loganalytics.io    | Dynamisch | 80,443 |
| docs.loganalytics.io   | Dynamisch | 80,443 |


## <a name="log-search"></a>Zoekopdrachten in logboeken
Open de pagina voor het zoeken van logboek van **logboeken** in het menu van Log Analytics of van **Log Analytics** in het menu van Azure Monitor. Dit is geschikt voor het analyseren van logboekgegevens met behulp van eenvoudige query's. Het biedt meerdere functies voor bewerken query's zonder een volledige kennis van de querytaal.  U krijgt een overzicht van deze functies in [zoekopdrachten in Logboeken maken in Azure Log Analytics met behulp van zoeken in logboeken](log-analytics-log-search-log-search-portal.md). 


![Meld u pagina zoeken](media/log-analytics-log-search-portals/log-search-portal.png)


## <a name="next-steps"></a>Volgende stappen

- Doorloop een [zelfstudie over het zoeken in Logboeken met](log-analytics-tutorial-viewdata.md) voor informatie over het maken van query's met behulp van de querytaal
- Doorloop een [les met behulp van de portal Advanced Analytics](query-language/get-started-analytics-portal.md) waarmee u op dezelfde manier werken als de Log Analytics-pagina.

