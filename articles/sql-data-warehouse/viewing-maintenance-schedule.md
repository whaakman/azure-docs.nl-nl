---
title: Azure onderhoudsschema's (preview) | Microsoft Docs
description: Onderhoud plannen kan klanten van plan bent om de benodigde gepland onderhoud-gebeurtenissen die gebruikmaakt van de Azure SQL Data warehouse-service naar de nieuwe functies, upgrades en patches.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
ms.date: 11/27/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: 61537a4cb8ca1e6e98f2fa04959861eb03480fe9
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247252"
---
# <a name="view-a-maintenance-schedule"></a>Een onderhoudsplanning weergeven 

## <a name="portal"></a>Portal

Standaard hebben alle nieuwe Azure SQL Data Warehouse-exemplaren een acht uur primaire en secundaire onderhoudsvenster toegepast tijdens de implementatie. Als u snel de implementatie is voltooid, kunt u de windows wijzigen. Er is geen onderhoud vindt plaats buiten het opgegeven onderhoudsvenster zonder voorafgaande kennisgeving.

Als u wilt weergeven van de onderhoudsplanning die aan uw datawarehouse is toegepast, voert u de volgende stappen uit:

1.  Meld u aan bij [Azure Portal](https://portal.azure.com/).
2.  Selecteer het datawarehouse dat u wilt weergeven. 
3.  De geselecteerde datawarehouse wordt geopend op de overzichtsblade. De onderhoudsplanning dat wordt toegepast op het datawarehouse wordt hieronder weergegeven **onderhoudsplanning (preview)**.

![Overzichtsblade](media/sql-data-warehouse-maintenance-scheduling/clear-overview-blade.PNG)

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) over het maken, weergeven en beheren van waarschuwingen met behulp van Azure Monitor.
- [Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) over webhookacties voor waarschuwingsregels.
- [Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) maken en beheren van Actiegroepen.
- [Meer informatie](https://docs.microsoft.com/azure/service-health/service-health-overview) over Azure Service Health.


