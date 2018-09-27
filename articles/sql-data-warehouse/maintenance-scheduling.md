---
title: Azure onderhoudsschema's (Preview) | Microsoft Docs
description: Onderhoud plannen kan klanten van plan bent om de benodigde gepland onderhoudsgebeurtenissen maakt gebruik van de Azure SQL Data warehouse-service naar de nieuwe functies, upgrades en patches.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 09/20/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: 85e8327d1cac17059b2e91b1ea21becc23002c8e
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228339"
---
# <a name="using-maintenance-schedules-to-manage-service-updates-and-maintenance"></a>Met behulp van onderhoudsschema's voor het beheren van service-updates en onderhoud

Azure SQL Data Warehouse onderhoud plannen is nu in preview. Deze nieuwe functie wordt naadloos geïntegreerd het geplande onderhoud Servicestatusmeldingen, Resource Health controleren bewaken en de Azure SQL Data Warehouse onderhoud Scheduler-service.

Onderhoud plannen kunt u een bepaalde periode plannen wanneer is het handig is voor het ontvangen van nieuwe functies, upgrades en patches. Klanten selecteert een primaire en secundaire onderhoud binnen een periode van 7 dagen, dat wil zeggen zaterdag 22:00 – zondag 01:00 uur (primair) en woensdag 19:00 – 22:00 (secundair). Als er kan geen onderhoud uit te voeren tijdens uw primaire onderhoudsvenster, probeert we deze opnieuw tijdens uw secundaire onderhoudsvenster.

Alle nieuw gemaakte Azure SQL Data Warehouse exemplaren beschikken over een systeem gedefinieerde onderhoudsplanning toegepast tijdens de implementatie. Het schema kan worden bewerkt als u de implementatie is voltooid.

Elk onderhoudsvenster kan liggen tussen 3 en 8 uur elke, met 3hrs momenteel wordt de kortste beschikbare optie. Onderhoud kan worden uitgevoerd op elk gewenst moment in het venster en houd rekening met een korte verlies van verbinding als de service implementeert u nieuwe code naar uw datawarehouse. 

Tijdens de functiepreview vragen we klanten om hun primaire en secundaire windows in afzonderlijke dagreeksen vast te stellen.   
Alle onderhoudsbewerkingen moeten worden voltooid binnen de geplande onderhoudsmomenten en geen onderhoud vindt plaats buiten de opgegeven onderhoudsvensters zonder voorafgaande kennisgeving. Als uw datawarehouse wordt onderbroken tijdens een geplande onderhoud, wordt deze bijgewerkt tijdens de bewerking hervat.  


## <a name="alerts-and-monitoring"></a>Waarschuwingen en bewaking

Naadloze integratie met servicestatusmeldingen en de resourcestatus controleren monitor kan klanten Blijf op de hoogte van aanstaande onderhoud-activiteit. Het nieuwe automation maakt gebruik van de Azure Monitor en kan klanten om te bepalen hoe ze willen worden geïnformeerd over aanstaande onderhoud en welke geautomatiseerde stromen moet worden geactiveerd voor het beheren van downtime en de impact op hun activiteiten te minimaliseren.

Alle onderhoudsgebeurtenissen worden voorafgegaan door een voorafgaande melding die 24 uur. Als u wilt exemplaar uitvaltijd te minimaliseren, moet u ervoor zorgen dat er zijn geen langlopende transacties op uw datawarehouse voorafgaand aan het begin van uw gekozen onderhoudsperiode. Alle actieve sessies worden geannuleerd wanneer onderhoud wordt gestart, niet-doorgevoerde transacties worden teruggedraaid en uw datawarehouse te maken met een korte verlies van verbinding. U krijgt onmiddellijk nadat onderhoud aan uw datawarehouse is voltooid. 

Als u een voorafgaande melding die dat onderhoud van plaatsvinden, maar er kan geen onderhoud uit te voeren in die tijd ontvangen, ontvangt u een melding over annulering van. Onderhoud wordt vervolgens hervat, tijdens de volgende geplande onderhoudsperiode.
 
Alle actieve onderhoudsgebeurtenissen wordt weergegeven in de 'Service Health - gepland onderhoud' sectie. Een volledige record van gebeurtenissen in het verleden bewaard als onderdeel van de geschiedenis van servicestatus. Onderhoud kan worden gecontroleerd via de Azure Service Health check-portaldashboard tijdens een actieve gebeurtenis.

### <a name="maintenance-schedule-availability"></a>Onderhoud plannen beschikbaarheid

Zelfs als gepland onderhoud nog niet beschikbaar in de geselecteerde regio is, kunt u nog steeds bekijken en uw onderhoudsplanning op elk gewenst moment bewerken. Bij het plannen van onderhoud beschikbaar is in uw regio, de planning geïdentificeerd onmiddellijk van kracht op uw datawarehouse.


## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](viewing-maintenance-schedule.md) over het weergeven van een onderhoud plannen 
- [Meer informatie](changing-maintenance-schedule.md) over het wijzigen van een onderhoudsplanning
- [Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) over het maken, weergeven en beheren van waarschuwingen via Azure Monitor
- [Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) over Webhook-acties voor waarschuwingsregels
- [Meer informatie](https://docs.microsoft.com/azure/service-health/service-health-overview) over Azure Service Health







