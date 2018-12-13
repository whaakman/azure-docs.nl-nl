---
title: SQL Database-Herstelanalyses | Microsoft Docs
description: Informatie over richtlijnen en aanbevolen procedures voor het uitvoeren van analyses voor noodherstel met Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 4ed98d80f54ba781e7230a7081b85a4437d20242
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52877010"
---
# <a name="performing-disaster-recovery-drill"></a>Dr-Herstelanalyse uitvoeren
Het verdient aanbeveling dat validatie van de gereedheid van de toepassing voor herstelwerkstroom regelmatig wordt uitgevoerd. Verifiëren van de werking van de toepassing en de gevolgen van het verlies van gegevens en/of de onderbreking omvat het dat de failover is een goede gewoonte engineering. Het is ook een vereiste door de meeste industrienormen als onderdeel van zakelijke continuïteit-certificering.

Een Dr-herstelanalyse uitvoeren bestaat uit:

* Simulating uitval van laag
* Herstellen
* Valideren van de toepassing integriteit na herstel

Afhankelijk van hoe u [ontworpen van uw toepassing voor zakelijke continuïteit](sql-database-business-continuity.md), de werkstroom voor het uitvoeren van de analyse kan variëren. Dit artikel beschrijft de aanbevolen procedures voor het uitvoeren van een Dr-herstelanalyse in de context van Azure SQL Database.

## <a name="geo-restore"></a>Geo-herstel
Uitvoeren om te voorkomen dat het verlies van gegevens als een Dr-herstelanalyse uitvoeren, met behulp van een testomgeving door het maken van een kopie van de productie-omgeving en deze om te verifiëren van de toepassing de werkstroom van de analyse.

#### <a name="outage-simulation"></a>Simulatie van de serviceonderbreking
Als u wilt de storing simuleren, kunt u de naam van de brondatabase wijzigen. Dit zorgt ervoor dat de toepassing-verbindingsfouten.

#### <a name="recovery"></a>Herstel
* De geo-herstel van de database in een andere server uitvoeren, zoals wordt beschreven [hier](sql-database-disaster-recovery.md).
* Wijzigen van de configuratie van de toepassing als u wilt verbinding maken met de herstelde database en volg de [configureren van een database na het herstel](sql-database-disaster-recovery.md) handleiding om het herstel te voltooien.

#### <a name="validation"></a>Validatie
* Voltooien van de analyse door te controleren of de toepassing integriteit na herstel (inclusief verbindingsreeksen, aanmeldingen, het testen van de basisfunctionaliteit of andere validaties deel van de standaardtoepassing ondertekeningen procedures).

## <a name="failover-groups"></a>Failover-groepen
Voor een database die wordt beveiligd met behulp van failover-groepen, moet u de oefening inzoomen geplande failover naar de secundaire server. De geplande failover zorgt ervoor dat de primaire en secundaire databases in de failovergroep gesynchroniseerd blijven wanneer de rollen zijn overgeschakeld. In tegenstelling tot de niet-geplande failover leidt met deze bewerking niet tot verlies van gegevens, zodat de analyse kan worden uitgevoerd in de productieomgeving.

#### <a name="outage-simulation"></a>Simulatie van de serviceonderbreking
Als u wilt de storing simuleren, kunt u de web-App of de virtuele machine verbonden met de database uitschakelen. Dit resulteert in de fouten in de netwerkconnectiviteit voor de webclients.

#### <a name="recovery"></a>Herstel
* Zorg ervoor dat de toepassingsconfiguratie in de DR-regio naar de vorige secundaire, wordt de nieuwe primaire voor volledige toegang.
* Initiëren [geplande failover](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) van de failovergroep vanaf de secundaire server.
* Ga als volgt de [configureren van een database na het herstel](sql-database-disaster-recovery.md) handleiding om het herstel te voltooien.

#### <a name="validation"></a>Validatie
Voltooien van de analyse door te controleren of de toepassing integriteit na herstel (met inbegrip van de verbinding, het basisfunctionaliteit testen en verdere controles die vereist zijn voor de ondertekeningen inzoomen).

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over zakelijke continuïteit-scenario's, [scenario's voor bedrijfscontinuïteit](sql-database-business-continuity.md).
* Voor meer informatie over Azure SQL Database geautomatiseerde back-ups, Zie [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md)
* Zie voor meer informatie over het gebruik van geautomatiseerde back-ups voor herstel, [een database herstellen vanuit back-ups service geïnitieerde](sql-database-recovery-using-backups.md).
* Zie voor meer informatie over opties voor sneller herstel, [actieve geo-replicatie en failover-groepen](sql-database-geo-replication-overview.md).  
