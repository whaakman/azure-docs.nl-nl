---
title: Onderhouds planningen voor Azure (preview) | Microsoft Docs
description: Met onderhouds planning kunnen klanten de nodige geplande onderhouds gebeurtenissen plannen die de Azure SQL Data Warehouse-service gebruikt om nieuwe functies, upgrades en patches uit te rollen.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
ms.date: 07/16/2019
ms.author: anvang
ms.reviewer: jrasnick
ms.openlocfilehash: 3875106e8c6301c95bc8d0fbce6a1c0400d07f78
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68278114"
---
# <a name="use-maintenance-schedules-to-manage-service-updates-and-maintenance"></a>Onderhouds planningen gebruiken voor het beheren van service-updates en-onderhoud

Onderhouds planningen zijn nu beschikbaar in alle Azure SQL Data Warehouse regio's. Deze functie integreert de Service Health geplande onderhouds meldingen, Resource Health controle en de Azure SQL Data Warehouse onderhouds plannings service.

U kunt onderhouds planning gebruiken om een tijd venster te kiezen wanneer het handig is om nieuwe functies, upgrades en patches te ontvangen. U kiest een primair en secundair onderhouds venster binnen een periode van zeven dagen. Een voor beeld is een primair venster van zaterdag 22:00 tot zondag 01:00 en een secundair venster van woensdag 19:00 tot 22:00. Als SQL Data Warehouse geen onderhoud kunt uitvoeren tijdens het primaire onderhouds venster, wordt het onderhoud opnieuw geprobeerd tijdens het secundaire onderhouds venster. Service onderhoud kan optreden in zowel het primaire als het secundaire Windows-venster. Om ervoor te zorgen dat alle onderhouds bewerkingen snel worden voltooid, kunnen DW400 (c) en lagere Data Warehouse-lagen het onderhoud volt ooien buiten een aangewezen onderhouds venster.

Voor alle nieuw gemaakte Azure SQL Data Warehouse instanties wordt een door het systeem gedefinieerde onderhouds planning toegepast tijdens de implementatie. De planning kan worden bewerkt zodra de implementatie is voltooid.

Elk onderhouds venster kan drie tot acht uur duren. Onderhoud kan op elk gewenst moment in het venster worden uitgevoerd. Wanneer onderhoud wordt gestart, worden alle actieve sessies geannuleerd en worden niet-doorgevoerde trans acties teruggedraaid. U moet rekening houden met meerdere korte verliezen in connectiviteit wanneer de service nieuwe code implementeert in uw data warehouse. U ontvangt een melding zodra het onderhoud is voltooid voor uw data warehouse

Als u deze functie wilt gebruiken, moet u een primair en secundair venster binnen verschillende datumbereiken identificeren. Alle onderhouds bewerkingen moeten worden voltooid binnen het geplande onderhouds venster. Er vindt geen onderhoud plaats buiten het opgegeven onderhouds venster zonder voorafgaande melding. Als uw data warehouse tijdens een gepland onderhoud wordt onderbroken, wordt het bijgewerkt tijdens de hervatting.  

## <a name="alerts-and-monitoring"></a>Waarschuwingen en bewaking

Dankzij de integratie met Service Health meldingen en de Resource Health controle controle kunnen klanten op de hoogte blijven van de bedreigende onderhouds activiteiten. De nieuwe automatisering maakt gebruik van Azure Monitor. U kunt bepalen hoe u op de hoogte wilt worden gesteld van aanstaande onderhouds gebeurtenissen. Bepaal ook welke geautomatiseerde stromen u kunnen helpen bij het beheren van downtime en de gevolgen voor uw bewerkingen tot een minimum te beperken.

Er wordt een melding van 24 uur voorgeschoten vóór alle onderhouds gebeurtenissen, met de huidige uitzonde ring van DW400c en lagere lagen. Zorg ervoor dat uw data warehouse geen langlopende trans acties heeft vóór de gekozen onderhouds periode om de uitval tijd van het exemplaar te minimaliseren.

> [!NOTE]
> In het geval dat er een essentiële update moet worden geïmplementeerd, kunnen geavanceerde meldings tijden aanzienlijk worden verkleind.

Als u een voorschot bericht hebt ontvangen dat er onderhoud wordt uitgevoerd, maar SQL Data Warehouse geen onderhoud kan uitvoeren tijdens die periode, ontvangt u een melding over de annulering. Het onderhoud wordt vervolgens hervat tijdens de volgende geplande onderhouds periode.

Alle actieve onderhouds gebeurtenissen worden weer gegeven in de sectie **service Health gepland onderhoud** . De Service Health geschiedenis bevat een volledige record met gebeurtenissen in het verleden. U kunt onderhoud bewaken via het Azure Service Health dash board van de portal controleren tijdens een actieve gebeurtenis.

### <a name="maintenance-schedule-availability"></a>Beschik baarheid onderhouds planning

Zelfs als de onderhouds planning niet beschikbaar is in de geselecteerde regio, kunt u op elk gewenst moment uw onderhouds planning weer geven en bewerken. Wanneer de onderhouds planning in uw regio beschikbaar wordt, wordt de geïdentificeerde planning onmiddellijk actief in uw data warehouse.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](viewing-maintenance-schedule.md) over het bekijken van een onderhouds planning.
- Meer [informatie](changing-maintenance-schedule.md) over het wijzigen van een onderhouds planning.
- Meer [informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) over het maken, weer geven en beheren van waarschuwingen met behulp van Azure monitor.
- Meer [informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) over webhook-acties voor logboek waarschuwings regels.
- [Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) Actie groepen maken en beheren.
- Meer [informatie](https://docs.microsoft.com/azure/service-health/service-health-overview) over Azure service health.
