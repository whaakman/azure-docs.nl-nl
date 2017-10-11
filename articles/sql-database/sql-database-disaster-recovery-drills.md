---
title: SQL Database-Noodhersteloefeningen | Microsoft Docs
description: Meer informatie over richtlijnen en aanbevolen procedures voor het gebruik van Azure SQL Database om uit te voeren van noodhersteloefeningen om ervoor te zorgen dat uw missie kritieke zakelijke toepassingen robuuste op fouten en storingen.
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
ms.workload: data-management
ms.date: 07/31/2016
ms.author: sashan
ms.openlocfilehash: 1b1d65a41a794a566287dcffe3581ac58e2a965f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="performing-disaster-recovery-drill"></a>Disaster Recovery inzoomen uitvoeren
Het is raadzaam dat validatie van de gereedheid van de toepassing voor herstelwerkstroom regelmatig wordt uitgevoerd. Verifiëren van de toepassingsgedrag en de gevolgen van het verlies van gegevens en/of de onderbreking moet u dat de failover is een goede gewoonte engineering. Het is ook een vereiste door de meeste industrienormen als onderdeel van zakelijke continuïteit-certificering.

Uitvoeren van een herstel na noodgevallen detailanalyse bestaat uit:

* Onderbreking van de laag Simulating gegevens
* Herstellen
* Toepassingsherstel integriteit post valideren

Afhankelijk van hoe u [ontworpen van uw toepassing voor bedrijfscontinuïteit](sql-database-business-continuity.md), de werkstroom uit te voeren van de analyse kan variëren. Hieronder worden de aanbevolen procedures voor het uitvoeren van een detailanalyse van het herstel na noodgevallen in de context van Azure SQL Database beschreven.

## <a name="geo-restore"></a>Geo-herstel
De mogelijke om gegevensverlies te voorkomen wanneer de uitvoering van een herstel na noodgevallen detailanalyse, wordt u aangeraden uitvoeren met behulp van een testomgeving door een kopie van de productie-omgeving maken en deze om te controleren of de werkstroom van de toepassing van de analyse.

#### <a name="outage-simulation"></a>Storing simulatie
U kunt om te simuleren de onderbreking, verwijderen of wijzig de naam van de brondatabase. Dit zorgt ervoor dat de problemen met de toepassingen.

#### <a name="recovery"></a>Herstel
* De geo-herstel van de database in een andere server uitvoeren, zoals wordt beschreven [hier](sql-database-disaster-recovery.md).
* Wijzig de configuratie van de toepassing verbinding maken met de herstelde database en volg de [configureren van een database na het herstel](sql-database-disaster-recovery.md) handleiding om het herstel te voltooien.

#### <a name="validation"></a>Validatie
* Voltooien van de analyse door te controleren of de integriteit post toepassingsherstel (inclusief verbindingsreeksen, aanmeldingen, testen van de basisfunctionaliteit of andere onderdeel validaties van standaardtoepassing ondertekeningen procedures).

## <a name="geo-replication"></a>Geo-replicatie
Voor een database die is beveiligd met geo-replicatie moet de oefening inzoomen geplande failover naar de secundaire database. De geplande failover zorgt ervoor dat de primaire en secundaire databases gesynchroniseerd blijven wanneer de rollen worden omgeschakeld. In tegenstelling tot de niet-geplande failover resulteren deze bewerking niet in een verlies van gegevens, zodat de analyse kan worden uitgevoerd in de productieomgeving.

#### <a name="outage-simulation"></a>Storing simulatie
Om te simuleren de onderbreking, kunt u de webtoepassing of de virtuele machine verbonden met de database uitschakelen. Dit resulteert in het verbindingsfouten voor de webserver en webclients.

#### <a name="recovery"></a>Herstel
* Zorg ervoor dat de configuratie van de toepassing in de regio DR verwijst naar de vorige secundaire de nieuwe primaire die volledig toegankelijk zijn wordt.
* Voer [geplande failover](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) om te maken van de secundaire database een nieuwe primaire
* Ga als volgt de [configureren van een database na het herstel](sql-database-disaster-recovery.md) handleiding om het herstel te voltooien.

#### <a name="validation"></a>Validatie
* Voltooien van de analyse door te controleren of de integriteit post toepassingsherstel (inclusief verbindingsreeksen, aanmeldingen, testen van de basisfunctionaliteit of andere onderdeel validaties van standaardtoepassing ondertekeningen procedures).

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over zakelijke continuïteit-scenario's [continuïteit scenario's](sql-database-business-continuity.md)
* Voor meer informatie over Azure SQL Database geautomatiseerde back-ups, Zie [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md)
* Zie voor meer informatie over het gebruik van automatische back-ups voor herstel, [een database herstellen vanuit back-ups service is gestart](sql-database-recovery-using-backups.md)
* Zie voor meer informatie over opties voor sneller herstel, [actieve geo-replicatie](sql-database-geo-replication-overview.md)  
