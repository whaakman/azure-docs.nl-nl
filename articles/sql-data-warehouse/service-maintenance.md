---
title: Gepland onderhoud - Azure SQL Data Warehouse | Microsoft Docs
description: Informatie over het voorbereiden van gebeurtenissen voor gepland onderhoud aan uw Azure SQL Data Warehouse.
services: sql-data-warehouse
author: antvgski
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/19/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: a273ad4c256267865d3af324f0ef755a6cb75c5c
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="planning-for-maintenance-on-your-azure-sql-data-warehouse"></a>Planning voor onderhoud op uw Azure SQL datawarehouse

Informatie over het voorbereiden van gebeurtenissen voor gepland onderhoud op uw Azure SQL datawarehouse.

## <a name="what-is-a-planned-maintenance-event"></a>Wat is er een gebeurtenis gepland onderhoud?
Een gebeurtenis gepland onderhoud is een venster van de tijd wanneer uw datawarehouse zijn voor een onderhoudsbewerking offline. Deze gebeurtenissen zijn mogelijkheden voor het service-upgrades, nieuwe functies of patches toepassen. 

## <a name="frequency"></a>Frequentie
Gemiddeld gebeurtenis ten minste één gepland onderhoud elke maand. 

## <a name="notifications-and-downtime"></a>Meldingen en uitvaltijd
U ontvangt een melding voor elke gebeurtenis gepland onderhoud. De gebeurtenis onderhoud annuleert alle actieve query's en uw datawarehouse offline gaat. De verwachte uitval voor elke datawarehouse is ongeveer 30 minuten. U kunt een melding kunt verwachten wanneer onderhoud voltooid is. 

## <a name="setting-up-alerts"></a>Waarschuwingen instellen

Wordt u aangeraden [Azure Monitor](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md) gepland onderhoud logboek waarschuwingen instellen. De waarschuwingen kunt u plannen voor het vereiste onderhoud te minimaliseren, de gevolgen voor uw bedrijf. 

Meldingen instellen voor het gebruik van deze [Meld waarschuwing instructies](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md). 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het bewaken van [bewaken van uw werkbelasting](sql-data-warehouse-manage-monitor.md).
