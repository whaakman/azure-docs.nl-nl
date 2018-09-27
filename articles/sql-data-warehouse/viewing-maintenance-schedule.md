---
title: Azure onderhoudsschema's (Preview) | Microsoft Docs
description: Onderhoud plannen kan klanten van plan bent om de benodigde gepland onderhoudsgebeurtenissen maakt gebruik van de Azure SQL Data warehouse-service naar de nieuwe functies, upgrades en patches.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 10/06/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: 6fbf914c8035344d33e8d2739fb9d92d5757c3d1
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228326"
---
# <a name="viewing-a-maintenance-schedule"></a>Een onderhoudsplanning weergeven 

## <a name="portal"></a>Portal

Standaard alle nieuw gemaakte Azure SQL Data Warehouse exemplaren een 8 uur door het systeem gedefinieerde primaire en secundaire onderhoudsvenster toegepast tijdens de implementatie hebben, kan dit worden bewerkt als binnenkort implementatie voltooid is. Er is geen onderhoud vindt plaats buiten de opgegeven onderhoudsvensters zonder voorafgaande kennisgeving.
De volgende stappen om de onderhoudsplanning die is toegepast op uw datawarehouse in de portal weer te geven.

De volgende stappen om de onderhoudsplanning die is toegepast op uw datawarehouse in de portal weer te geven.
1.  Meld u aan bij [Azure Portal](https://portal.azure.com/).
2.  Selecteer het datawarehouse dat u wilt weergeven. 
3.  De geselecteerde Azure SQL Data Warehouse wordt geopend op de overzichtsblade. De onderhoudsplanning toegepast op het geselecteerde datawarehouse worden weergegeven onder de onderhoudsplanning (preview).

![Overzichtsblade](media/sql-data-warehouse-maintenance-scheduling/clear-overview-blade.PNG)

## <a name="next-steps"></a>Volgende stappen
[Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) over het maken, weergeven en beheren van waarschuwingen via Azure Monitor.
[Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) over Webhook-acties voor waarschuwingsregels.
[Meer informatie](https://docs.microsoft.com/azure/service-health/service-health-overview) over Azure Service Health


