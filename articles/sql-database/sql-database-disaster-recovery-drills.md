---
title: SQL Database-Noodhersteloefeningen | Microsoft Docs
description: Informatie over richtlijnen en aanbevolen procedures voor het uitvoeren van noodhersteloefeningen met Azure SQL Database.
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: b44a269c-fe2a-404f-b013-290030860bd1
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.date: 10/20/2016
ms.workload: Inactive
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 73c2cbe978c980cbe1269b34cdb9f5ff86113e61
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="performing-disaster-recovery-drill"></a>Disaster Recovery inzoomen uitvoeren
Het is raadzaam dat validatie van de gereedheid van de toepassing voor herstelwerkstroom regelmatig wordt uitgevoerd. Verifiëren van de toepassingsgedrag en de gevolgen van het verlies van gegevens en/of de onderbreking moet u dat de failover is een goede gewoonte engineering. Het is ook een vereiste door de meeste industrienormen als onderdeel van zakelijke continuïteit-certificering.

Uitvoeren van een herstel na noodgevallen detailanalyse bestaat uit:

* Onderbreking van de laag Simulating gegevens
* Herstellen
* Toepassingsherstel integriteit post valideren

Afhankelijk van hoe u [ontworpen van uw toepassing voor bedrijfscontinuïteit](sql-database-business-continuity.md), de werkstroom uit te voeren van de analyse kan variëren. In dit artikel beschrijft de aanbevolen procedures voor het uitvoeren van een detailanalyse van het herstel na noodgevallen in de context van Azure SQL Database.

## <a name="geo-restore"></a>Geo-herstel
De mogelijke om gegevensverlies te voorkomen wanneer de uitvoering van een herstel na noodgevallen detailanalyse, uitvoeren met behulp van een testomgeving door een kopie van de productie-omgeving maken en deze om te controleren of de werkstroom van de toepassing van de analyse.

#### <a name="outage-simulation"></a>Storing simulatie
Om te simuleren de onderbreking, kunt u de brondatabase wijzigen. Dit zorgt ervoor dat de problemen met de toepassingen.

#### <a name="recovery"></a>Herstel
* De geo-herstel van de database in een andere server uitvoeren, zoals wordt beschreven [hier](sql-database-disaster-recovery.md).
* Wijzig de configuratie van de toepassing verbinding maken met de herstelde database en volg de [configureren van een database na het herstel](sql-database-disaster-recovery.md) handleiding om het herstel te voltooien.

#### <a name="validation"></a>Validatie
* Voltooien van de analyse door te controleren of de integriteit post toepassingsherstel (inclusief verbindingsreeksen, aanmeldingen, testen van de basisfunctionaliteit of andere onderdeel validaties van standaardtoepassing ondertekeningen procedures).

## <a name="failover-groups"></a>Failover-groepen
Voor een database die wordt beveiligd met behulp van failover-groepen, moet u de oefening inzoomen geplande failover naar de secundaire server. De geplande failover zorgt ervoor dat de primaire en secundaire databases in de groep failover synchroon blijven wanneer de rollen worden omgeschakeld. In tegenstelling tot de niet-geplande failover resulteren deze bewerking niet in een verlies van gegevens, zodat de analyse kan worden uitgevoerd in de productieomgeving.

#### <a name="outage-simulation"></a>Storing simulatie
Om te simuleren de onderbreking, kunt u de webtoepassing of de virtuele machine verbonden met de database uitschakelen. Dit resulteert in het verbindingsfouten voor de webserver en webclients.

#### <a name="recovery"></a>Herstel
* Zorg ervoor dat de toepassingsconfiguratie in de DR regio-verwijst naar de vorige secundaire, wordt de nieuwe primaire die toegankelijk zijn.
* Initiëren [geplande failover](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) van de groep failover van de secundaire server.
* Ga als volgt de [configureren van een database na het herstel](sql-database-disaster-recovery.md) handleiding om het herstel te voltooien.

#### <a name="validation"></a>Validatie
Voltooien van de analyse door te controleren of de integriteit post toepassingsherstel (inclusief connectiviteit, het basisfunctionaliteit testen en verdere controles die vereist zijn voor de ondertekeningen inzoomen).

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over zakelijke continuïteit-scenario's [continuïteit scenario's](sql-database-business-continuity.md).
* Voor meer informatie over Azure SQL Database geautomatiseerde back-ups, Zie [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md)
* Zie voor meer informatie over het gebruik van automatische back-ups voor herstel, [een database herstellen vanuit back-ups service geïnitieerde](sql-database-recovery-using-backups.md).
* Zie voor meer informatie over opties voor sneller herstel, [actieve geo-replicatie en failover groepen](sql-database-geo-replication-overview.md).  
