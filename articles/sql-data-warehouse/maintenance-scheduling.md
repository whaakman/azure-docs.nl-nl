---
title: Azure onderhoudsschema's (preview) | Microsoft Docs
description: Onderhoud plannen kan klanten van plan bent om de benodigde gepland onderhoud-gebeurtenissen die de Azure SQL Data Warehouse-service gebruikt om nieuwe functies, upgrades en patches.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 11/27/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: 0f9188ba26c818ce21e0951fad5b54c2ea5a893e
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53994909"
---
# <a name="use-maintenance-schedules-to-manage-service-updates-and-maintenance"></a>Onderhoudsschema's gebruiken voor het beheren van service-updates en onderhoud

Onderhoudsschema's is nu beschikbaar in alle regio's voor Azure SQL Data Warehouse. Deze functie integreert het geplande onderhoud Servicestatusmeldingen, Resource Health controleren bewaken en de Azure SQL Data Warehouse onderhoud Scheduler-service.

U onderhoud plannen van een bepaalde periode kiezen wanneer is het handig is voor het ontvangen van nieuwe functies, upgrades en patches. Kiest u een primaire en secundaire onderhoud binnen een periode van zeven dagen. Een voorbeeld is een primaire venster van zaterdag 22:00 tot zondag 01:00 uur en een tweede venster van woensdag 19:00 op 22:00 uur. Als SQL Data Warehouse onderhoud niet tijdens uw primaire onderhoudsvenster uitvoeren kan, zal proberen het onderhoud opnieuw tijdens uw secundaire onderhoudsvenster.

Alle nieuw gemaakte Azure SQL Data Warehouse exemplaren beschikken over een systeem gedefinieerde onderhoudsplanning toegepast tijdens de implementatie. Het schema kan worden bewerkt als u de implementatie is voltooid.

Elk onderhoudsvenster mag drie tot acht uur. Onderhoud kan worden uitgevoerd op elk gewenst moment in het venster. U moet een korte verlies van connectiviteit verwacht als de service implementeert u nieuwe code naar uw datawarehouse. 

Deze functie wilt gebruiken moet u een primaire en secundaire venster in afzonderlijke dagreeksen identificeren. Alle onderhoudsbewerkingen moeten zijn voltooid binnen de geplande onderhoudsmomenten. Er is geen onderhoud vindt plaats buiten het opgegeven onderhoudsvenster zonder voorafgaande kennisgeving. Als uw datawarehouse wordt onderbroken tijdens een geplande onderhoud, wordt deze bijgewerkt tijdens de bewerking hervat.  


## <a name="alerts-and-monitoring"></a>Waarschuwingen en bewaking

Integratie met meldingen van de servicestatus en de Resource-Monitor voor het controleren van status kan klanten Blijf op de hoogte van aanstaande onderhoud-activiteit. Het nieuwe automation maakt gebruik van Azure Monitor. U kunt bepalen hoe u om te worden geïnformeerd over aanstaande onderhoud. Ook kunt u bepalen welke geautomatiseerde stromen kunt u downtime beheren en beperken de gevolgen voor uw activiteiten.

Een 24-uurs voorafgaande melding die voorafgaat aan alle onderhoudsgebeurtenissen. Als u wilt exemplaar uitvaltijd te minimaliseren, zorg ervoor dat uw datawarehouse geen langlopende transacties voordat uw gekozen onderhoudsperiode. Als onderhoud wordt gestart, worden alle actieve sessies worden geannuleerd. Noncommitted transacties worden teruggedraaid en uw datawarehouse te maken met een korte verlies van verbinding. U krijgt een bericht onmiddellijk nadat de onderhoud is voltooid op uw datawarehouse. 

Als u een voorafgaande melding die dat onderhoud van plaatsvinden, maar SQL Data Warehouse onderhoud niet in die tijd uitvoeren kan hebt ontvangen, ontvangt u een melding over annulering van. Onderhoud wordt vervolgens hervat, tijdens de volgende geplande onderhoudsperiode.
 
Alle actieve onderhoudsgebeurtenissen worden weergegeven in de **servicestatus - gepland onderhoud** sectie. De geschiedenis van de Service omvat een volledige record van gebeurtenissen in het verleden. U kunt onderhoud via Azure Service Health check dashboard van de portal bewaken tijdens een actieve gebeurtenis.

### <a name="maintenance-schedule-availability"></a>Onderhoud plannen beschikbaarheid

Zelfs als gepland onderhoud is niet beschikbaar in de geselecteerde regio, kunt u bekijken en uw onderhoudsplanning op elk gewenst moment bewerken. Bij het plannen van onderhoud beschikbaar is in uw regio, de geïdentificeerde planning onmiddellijk van kracht op uw datawarehouse.


## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](viewing-maintenance-schedule.md) over het weergeven van een onderhoudsplanning. 
- [Meer informatie](changing-maintenance-schedule.md) over het wijzigen van een onderhoudsplanning.
- [Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) over het maken, weergeven en beheren van waarschuwingen met behulp van Azure Monitor.
- [Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) over webhookacties voor waarschuwingsregels.
- [Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) maken en beheren van Actiegroepen.
- [Meer informatie](https://docs.microsoft.com/azure/service-health/service-health-overview) over Azure Service Health.







