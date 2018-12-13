---
title: Azure Resource Health gebruiken voor het bewaken van de gezondheid van SQL Database | Microsoft Docs
description: Azure Resource Health gebruiken voor het bewaken van de status van de SQL-Database, kunt u vaststellen en ondersteuning krijgen wanneer een Azure-probleem gevolgen heeft voor uw SQL-resources.
services: sql-database
ms.service: sql-database
ms.subservice: monitor
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: carlrab
manager: craigg
ms.date: 11/14/2018
ms.openlocfilehash: 8b982dbddf22393e276437c77007e18bc4d40042
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52867186"
---
# <a name="use-resource-health-to-troubleshoot-connectivity-for-azure-sql-database"></a>Resource Health oplossen met connectiviteit voor Azure SQL Database gebruiken

## <a name="overview"></a>Overzicht

[Resource Health](../service-health/resource-health-overview.md#getting-started) voor SQL Database u bij het diagnosticeren en ondersteuning krijgen helpt wanneer een Azure-probleem gevolgen heeft voor uw SQL-resources. Het biedt u informatie over de huidige en eerdere status van uw resources en helpt u problemen verhelpen. Resource Health biedt technische ondersteuning als u hulp nodig heeft bij problemen met Azure-services.

![Overzicht](./media/sql-database-resource-health/sql-resource-health-overview.jpg)

## <a name="health-checks"></a>Statuscontroles

Resource Health bepaalt de status van uw SQL-resource door te controleren geslaagde en mislukte aanmeldingen aan de resource. Op dit moment controleert resourcestatus voor uw SQL-database alleen mislukte aanmeldpogingen bij vanwege een systeemfout is en niet gebruikersfout. De status van de resourcestatus wordt elke 1-2 minuten bijgewerkt.

## <a name="health-states"></a>Statussen

### <a name="available"></a>Beschikbaar

De status van **beschikbaar** betekent dat aanmeldingen storingen als gevolg van fouten in het bestandssysteem voor uw SQL-resource niet de resourcestatus heeft gedetecteerd.

![Beschikbaar](./media/sql-database-resource-health/sql-resource-health-available.jpg)

### <a name="degraded"></a>Verminderd

De status **Gedegradeerd** betekent dat Resource Health een meerderheid aan geslaagde aanmeldingen, maar ook een aantal mislukte aanmeldingen, heeft gedetecteerd. Dit zijn de meest waarschijnlijke tijdelijke aanmeldingsfouten. Als u wilt de impact van problemen met verbindingen is veroorzaakt door tijdelijke aanmeldingsfouten voorkomen, implementeert u [logica voor opnieuw proberen](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors) in uw code.

![Verminderd](./media/sql-database-resource-health/sql-resource-health-degraded.jpg)

### <a name="unavailable"></a>Niet beschikbaar

De status van **niet beschikbaar** betekent dat de Resource Health consistente aanmeldingen bij de SQL-resource heeft gedetecteerd. Als uw resource in deze status voor een lange periode blijft, neem contact op met ondersteuning.

![Niet beschikbaar](./media/sql-database-resource-health/sql-resource-health-unavailable.jpg)

### <a name="unknown"></a>Onbekend

De status van **onbekende** geeft aan dat Resource Health informatie over deze bron voor meer dan 10 minuten nog niet ontvangen. Deze status is niet een definitieve indicatie van de status van de resource, is maar het een belangrijk gegevenspunt in het proces voor het oplossen van problemen.
Als de resource wordt uitgevoerd zoals verwacht, wordt de status van de resource op beschikbaar wijzigen na een paar minuten.
Als u problemen met de resource ondervindt, kan de status onbekend voorstellen dat een gebeurtenis in het platform is die betrekking hebben op de resource.

## <a name="historical-information"></a>Historische gegevens

U kunt toegang tot maximaal 14 dagen van de geschiedenis van in de sectie van de geschiedenis van status van Resource Health. De sectie bevat ook de reden downtime (indien beschikbaar) voor de downtimes gerapporteerd door Resource Health. Azure laat momenteel de downtime voor uw SQL-database met een granulariteit van twee minuten. De werkelijke downtime is waarschijnlijk minder dan een minuut – gemiddelde 8s versie is.

### <a name="downtime-reasons"></a>Oorzaken downtime

Wanneer uw SQL-Database downtime optreedt, uitgevoerd om een reden vast te stellen. Wanneer deze beschikbaar is, wordt de reden downtime gerapporteerd in de sectie Geschiedenis van Resource Health. Redenen voor downtime worden doorgaans 30 minuten na een gebeurtenis gepubliceerd.

#### <a name="planned-maintenance"></a>Gepland onderhoud

De Azure-infrastructuur wordt periodiek uitgevoerd voor gepland onderhoud: upgraden van hardware of software-onderdelen in het datacenter. Terwijl de database onderhoud ondergaat, kan de SQL beëindigen van sommige bestaande verbindingen en de nieuwe weigeren. De aanmelding fouten opgetreden tijdens het geplande onderhoud zijn gewoonlijk tijdelijk en logica voor opnieuw proberen vermindert de gevolgen. Als u aanmeldingsfouten zich blijft voordoen, neem contact op met ondersteuning.

#### <a name="reconfiguration"></a>Herconfiguratie

Wanneer worden beschouwd als tijdelijke omstandigheden, en van tijd tot tijd worden verwacht. Deze gebeurtenissen kunnen worden geactiveerd door load balancing- of software/hardwarestoringen. Productie met elke clienttoepassing die verbinding met een cloudservice voor de database maakt moet een logica voor opnieuw proberen van robuuste verbinding met uitstellogica implementeren, zoals deze zou zodat deze situaties worden verminderd en over het algemeen de fouten transparant voor de eindgebruiker moet.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Pogingslogica voor tijdelijke problemen](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors)
- [SQL-verbindingsfouten opsporen, diagnose vaststellen en problemen oplossen](./sql-database-connectivity-issues.md)
- Meer informatie over [Resource Health waarschuwingen configureren](/articles/service-health/resource-health-alert-arm-template-guide.md)
- Bekijk een overzicht van [Resource Health](/articles/service-health/resource-health-overview.md)
- [Resource Health Veelgestelde vragen](/articles/service-health/resource-health-faq.md)
