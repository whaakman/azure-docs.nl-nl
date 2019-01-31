---
title: Noodherstel voor SQL Database | Microsoft Docs
description: Leer hoe u een database herstellen van een datacenter regionale stroomstoring of fout met de Azure SQL Database van actieve geo-replicatie, en mogelijkheden voor geo-herstel.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 61957b89bd5ec9a42d6fd03c7009187d71e448f4
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55474782"
---
# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Herstellen van een Azure SQL Database of een failover naar een secundaire

Azure SQL Database biedt de volgende mogelijkheden voor het herstellen van een storing:

- [Actieve Geo-replicatie](sql-database-active-geo-replication.md)
- [Groepen voor automatische failover](sql-database-auto-failover-group.md)
- [Geo-restore](sql-database-recovery-using-backups.md#point-in-time-restore)
- [Zone-redundante databases](sql-database-high-availability.md)

Zie voor meer informatie over scenario's voor zakelijke continuïteit en de onderdelen van deze scenario's ondersteunen, [bedrijfscontinuïteit](sql-database-business-continuity.md).

> [!NOTE]
> Als u zone-redundante Premium en bedrijfskritiek databases of pools, het herstelproces is geautomatiseerd en de rest van dit materiaal is niet van toepassing.

## <a name="prepare-for-the-event-of-an-outage"></a>Voorbereiden voor het geval van een storing

Voor succesvol gebruik van herstel naar een andere gegevensregio met behulp van failover-groepen of geografisch redundante back-ups, moet u een server voorbereiden in een ander datacenter storing om te worden van de nieuwe primaire server moet de noodzaak zich voordoen, evenals goed gedefinieerde beschreven stappen hebt en getest om te controleren of een soepel herstel. Deze stappen voor gegevensvoorbereiding zijn onder andere:

- Identificeer de SQL Database-server in een andere regio om te worden van de nieuwe primaire server. Voor geo-restore, dit is doorgaans een server in de [gekoppelde regio](../best-practices-availability-paired-regions.md) voor de regio waarin uw database zich bevindt. Dit elimineert de kosten voor extra verkeer tijdens de bewerkingen voor geo-herstellen.
- Identificeer en (optioneel), de firewall op serverniveau regels definiëren die nodig zijn voor gebruikers toegang hebben tot de nieuwe primaire database.
- Bepalen hoe u gaat om gebruikers te leiden naar de nieuwe primaire server, zoals door het veranderen van tekenreeksen voor databaseverbindingen of door het veranderen van DNS-vermeldingen.
- Identificeer en (optioneel) maakt, de aanmeldingen die moeten worden gebruikt in de database master op de nieuwe primaire server en zorg ervoor dat deze aanmeldingen geschikte machtigingen hebben in de hoofddatabase, indien van toepassing. Zie voor meer informatie, [SQL Database-beveiliging na herstel na noodgevallen](sql-database-geo-replication-security-config.md)
- Identificeren waarschuwingsregels die moeten worden bijgewerkt om toe te wijzen naar de nieuwe primaire database.
- Documenteer de configuratie van de controle op de huidige primaire database
- Voer een [Dr-herstelanalyse](sql-database-disaster-recovery-drills.md). U kunt voor het simuleren van een storing voor geo-restore, verwijderen of wijzig de naam van de brondatabase om probleem met de toepassing. Als u wilt simuleren van een storing met behulp van failover-groepen, kunt u de web-App of virtuele machine verbonden met de database of failover van de database als u de connectiviteit van toepassingsfouten uitschakelen.

## <a name="when-to-initiate-recovery"></a>Bij het initiëren van herstellen

De toepassing heeft gevolgen voor de herstelbewerking wordt uitgevoerd. Het wijzigen van de SQL-verbindingsreeks of met behulp van DNS-omleiding vereist en kan leiden tot verlies van permanente gegevens. Er moet daarom worden uitgevoerd alleen wanneer de onderbreking is waarschijnlijk langer dan de beoogde hersteltijd van uw toepassing. U moet uitvoeren van regelmatige controle van de status van de toepassing en gebruik van de volgende gegevenspunten om te bevestigen dat het herstel is gewaarborgd wanneer de toepassing naar productie is geïmplementeerd:

1. De verbindingsfout is permanent van de toepassingslaag met de database.
2. De Azure-portal ziet u een waarschuwing over een incident in de regio met brede invloed.

> [!NOTE]
> Als u failover-groepen gebruikt en ervoor automatische failover kiest, wordt het herstelproces is automatisch en transparant voor de toepassing.

U kunt de volgende herstelopties overwegen, afhankelijk van de tolerantie van uw toepassing downtime en mogelijke business aansprakelijkheid.

Gebruik de [herstelbare Database ophalen](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) om op te halen van de meest recente herstelpunt voor Geo-replicatie.

## <a name="wait-for-service-recovery"></a>Wachten op herstel

De Azure teams werken hard om terug te zetten van de beschikbaarheid van services zoals snel, afhankelijk van de hoofdmap, maar mogelijk de oorzaak kan uren of dagen duren.  Als uw toepassing significante downtime die u gewoon wachten op het herstel tolereren kan kunt te voltooien. In dit geval is geen actie van uw kant vereist. U kunt de status van de huidige service zien op onze [Azure Service Health Dashboard](https://azure.microsoft.com/status/). Na het herstel van de regio de beschikbaarheid van uw toepassing wordt hersteld.

## <a name="fail-over-to-geo-replicated-secondary-server-in-the-failover-group"></a>Failover naar een secundaire server in het failover-groep met geo-replicatie

Als uitvaltijd van uw toepassing tot zakelijke aansprakelijkheid leiden kan, moet u failover-groepen gebruiken. Hiermee kan de toepassing om snel te herstellen beschikbaarheid in een andere regio in geval van een storing. Zie voor een zelfstudie [een geografisch gedistribueerde database implementeren](sql-database-implement-geo-distributed-database.md).

U moet Start de failover naar de secundaire server met behulp van een van de ondersteunde methoden voor het herstellen van de beschikbaarheid van de database (s).

Gebruik een van de volgende handleidingen voor failover naar een secundaire database met geo-replicatie:

- [Failover naar een geo-replicatie secundaire server met behulp van de Azure portal](sql-database-geo-replication-portal.md)
- [Een failover uitvoeren naar de secundaire server met behulp van PowerShell](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)

## <a name="recover-using-geo-restore"></a>Herstellen met behulp van geo-herstel

Als uitvaltijd van uw toepassing niet in bedrijf aansprakelijkheid resulteert kunt u [geo-herstel](sql-database-recovery-using-backups.md) als een methode voor het herstellen van uw toepassing (s). Het maakt een kopie van de database van de meest recente geografisch redundante back-up.

## <a name="configure-your-database-after-recovery"></a>Uw database na het herstel configureren

Als u van geo-herstel gebruikmaakt te herstellen na een storing, moet u ervoor zorgen dat de verbinding met de nieuwe databases correct is geconfigureerd, zodat de normale toepassing-functie kan worden hervat. Dit is een controlelijst met taken om voor te bereiden van de productie van de herstelde database.

### <a name="update-connection-strings"></a>Verbindingsreeksen bijwerken

Omdat de herstelde database zich in een andere server bevindt, moet u de verbindingsreeks van uw toepassing om te verwijzen naar die server bijwerken.

Zie voor meer informatie over het wijzigen van tekenreeksen voor databaseverbindingen, de juiste talen voor uw [verbindingsbibliotheek](sql-database-libraries.md).

### <a name="configure-firewall-rules"></a>Firewall-regels configureren

U moet ervoor zorgen dat de firewallregels op server en op de database geconfigureerd overeenkomen met die die zijn geconfigureerd op de primaire server en de primaire database. Zie voor meer informatie, [het: Configureer Firewall-instellingen (Azure SQL Database)](sql-database-configure-firewall-settings.md).

### <a name="configure-logins-and-database-users"></a>Aanmeldingen en databasegebruikers configureren

U moet ervoor zorgen dat alle aanmeldingen die worden gebruikt door uw toepassing zijn op de server die als host de herstelde database fungeert. Zie voor meer informatie, [beveiligingsconfiguratie voor geo-replicatie](sql-database-geo-replication-security-config.md).

> [!NOTE]
> U moet configureren en testen van uw server firewall-regels en aanmeldingen (en hun machtigingen) tijdens noodherstelanalyse. Deze objecten op serverniveau en hun configuratie mogelijk niet beschikbaar tijdens de storing.

### <a name="setup-telemetry-alerts"></a>Telemetrie-waarschuwingen instellen

Zo moet u ervoor dat de bestaande instellingen voor de waarschuwingsregel worden bijgewerkt om toe te wijzen aan de herstelde database en de andere server.

Zie voor meer informatie over database-waarschuwingsregels [waarschuwing meldingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) en [servicestatus bijhouden](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Controle inschakelen

Als controle is vereist voor toegang tot uw database, moet u controleren na het herstellen van de database inschakelt. Zie voor meer informatie, [Databasecontrole](sql-database-auditing.md).

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over Azure SQL Database geautomatiseerde back-ups, Zie [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md)
- Zie voor meer informatie over zakelijke continuïteit ontwerp en de recovery-scenario's, [scenario's voor bedrijfscontinuïteit](sql-database-business-continuity.md)
- Zie voor meer informatie over het gebruik van geautomatiseerde back-ups voor herstel, [een database herstellen vanuit back-ups service gestart](sql-database-recovery-using-backups.md)
