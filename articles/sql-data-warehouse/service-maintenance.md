---
title: Gepland onderhoud - Azure SQL Data Warehouse | Microsoft Docs
description: Informatie over het voorbereiden van gepland onderhoudsgebeurtenissen naar uw Azure SQL Data Warehouse.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/19/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: 05f82ae571bea41c15099b52282f4efe29b2c763
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55468407"
---
# <a name="planning-for-maintenance-on-your-azure-sql-data-warehouse"></a>Planning voor onderhoud op uw Azure SQL datawarehouse

Informatie over het voorbereiden voor gelijktijdige onderhoudsgebeurtenissen op uw Azure SQL datawarehouse.

## <a name="what-is-a-planned-maintenance-event"></a>Wat is een geplande onderhoudsgebeurtenis?
Gepland onderhoud is een periode wanneer uw datawarehouse moet voor een onderhoudsbewerking offline zijn. Deze gebeurtenissen zijn mogelijkheden voor het toepassen van service-upgrades, nieuwe functies of patches. 

## <a name="frequency"></a>Frequentie
Gemiddeld, ten minste één gepland onderhoud de gebeurtenis elke maand. 

## <a name="notifications-and-downtime"></a>Meldingen en uitvaltijd
U ontvangt een melding voor elke geplande onderhoudsgebeurtenis. De onderhoudsgebeurtenis annuleert alle actieve query's en uw datawarehouse offline gaat. De verwachte uitvaltijd voor elke datawarehouse is ongeveer 30 minuten. U krijgt een melding wanneer het onderhoud is voltooid. 

## <a name="setting-up-alerts"></a>Instellen van waarschuwingen

Wordt u aangeraden [Azure Monitor](../azure-monitor/platform/alerts-activity-log-service-notifications.md) gepland voor het instellen van waarschuwingen voor onderhoud. De waarschuwingen kunt u van plan bent voor het vereiste onderhoud om te beperken de gevolgen voor uw bedrijf. 

Als u meldingen instelt, gebruikt u deze [melden waarschuwing instructies](../azure-monitor/platform/alerts-activity-log-service-notifications.md). 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het controleren van [uw workload controleren](sql-data-warehouse-manage-monitor.md).
