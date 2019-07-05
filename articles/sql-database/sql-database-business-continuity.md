---
title: Bedrijfscontinuïteit met behulp van de cloud - databasherstel - SQL Database | Microsoft Docs
description: Ontdek hoe Azure SQL Database bedrijfscontinuïteit en databaseherstel met behulp van de cloud ondersteunt en u helpt om essentiële cloudtoepassingen ononderbroken uit te voeren.
keywords: bedrijfscontinuïteit, bedrijfscontinuïteit met de cloud, databasenoodherstel, databaseherstel
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
ms.date: 06/25/2019
ms.openlocfilehash: 2f3723e0a1b14edd6f516f3cc080501bea80d486
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442037"
---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Overzicht van bedrijfscontinuïteit met Azure SQL Database

**Zakelijke continuïteit** in Azure SQL Database verwijst naar de mechanismen, beleidsregels en procedures waarmee uw bedrijf om door te gaan met het besturingssysteem bij storingen van apparaten wordt onderbroken, met name naar de IT-infrastructuur. In de meeste gevallen, Azure SQL Database de verstorende gebeurtenissen die mogelijk optreden in de cloudomgeving en uw toepassingen en zakelijke processen die worden verwerkt. Er zijn echter enkele verstorende gebeurtenissen die kunnen niet worden verwerkt door SQL Database automatisch, zoals:

- Gebruiker per ongeluk verwijderd of een rij in een tabel bijgewerkt.
- Kwaadwillende is voltooid om te verwijderen van gegevens of een database verwijderen.
- Aardbeving veroorzaakt een stroomstoring en tijdelijk uitgeschakeld Datacenter.

In dit overzicht worden de mogelijkheden beschreven die Azure SQL Database biedt voor bedrijfscontinuïteit en herstel na noodgevallen. Meer informatie over de opties, aanbevelingen en zelfstudies voor het herstellen van storingen die kunnen leiden tot gegevensverlies of ervoor zorgen dat uw databases en toepassingen niet meer beschikbaar zijn. Meer informatie over wat er gebeurt wanneer een gebruiker of toepassing-fout is van invloed op de integriteit van gegevens, een Azure-regio een storing heeft of uw toepassing onderhoud vereist.

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>SQL Database-functies die u kunt gebruiken voor bedrijfscontinuïteit

Vanuit het perspectief van een database zijn er vier belangrijke mogelijke onderbreking van de scenario's:

- Lokale hardware- of fouten die betrekking hebben op de database-knooppunt, zoals een schijfstation fout.
- Beschadigde gegevens of verwijdering meestal veroorzaakt door een toepassing bug of menselijke fouten. Deze fouten zijn toepassingsspecifiek en doorgaans niet kunnen worden gedetecteerd door de database-service.
- Datacenter uitvalt, wordt mogelijk veroorzaakt door een natuurramp. In dit scenario moeten bepaalde mate van geografische redundantie met failover van de toepassing naar een ander datacenter.
- Upgraden of onderhoud fouten, onverwachte problemen die tijdens optreden gepland onderhoud aan de infrastructuur of upgrades mogelijk snelle terugdraaien naar een eerdere database staat.

Als u wilt de lokale hardware en softwarefouten oplossen, SQL-Database bevat een [architectuur voor hoge beschikbaarheid](sql-database-high-availability.md), die garandeert dat automatisch herstel van deze fouten met 99.995% beschikbaarheids-SLA.  

Als u wilt uw bedrijf te beschermen tegen verlies van gegevens, SQL-Database maakt automatisch volledige databaseback-ups wekelijks, differentiële databaseback-ups elke 12 uur, en de transactielogboeken elke 5 - 10 minuten. De back-ups worden opgeslagen in de RA-GRS-opslag voor ten minste zeven dagen voor alle Servicelagen. Alle service-lagen, behalve Basic ondersteuning configureerbare back-up bewaarperiode voor point-in-time-restore, maximaal 35 dagen. 

SQL Database biedt ook diverse functies voor bedrijfscontinuïteit, die u gebruiken kunt om te beperken van verschillende scenario's voor niet-geplande. 

- [Tijdelijke tabellen](sql-database-temporal-tables.md) maken het mogelijk rijversies te herstellen naar ieder gewenst tijdstip.
- [Ingebouwde geautomatiseerde back-ups](sql-database-automated-backups.md) en [verwijzen in Time Restore](sql-database-recovery-using-backups.md#point-in-time-restore) kunt u de volledige database naar een bepaald moment in de tijd binnen de geconfigureerde retentieperiode van 35 dagen herstellen naar.
- U kunt [een verwijderde database herstellen](sql-database-recovery-using-backups.md#deleted-database-restore) op het punt waarop deze is verwijderd als de **SQL Database-server is niet verwijderd**.
- [Langetermijnretentie van back-up](sql-database-long-term-retention.md) kunt u de back-ups op 10 jaar houden.
- [Actieve geo-replicatie](sql-database-active-geo-replication.md) kunt u maken leesbare replica's en handmatig failover naar een replica in het geval van een upgrade data center stroomstoring of toepassing.
- [Automatische-failovergroep](sql-database-auto-failover-group.md#auto-failover-group-terminology-and-capabilities) kan de toepassing automatisch herstel in het geval van een storing in het datacenter.

## <a name="recover-a-database-within-the-same-azure-region"></a>Herstellen van een database in dezelfde Azure-regio

Automatische databaseback-ups kunt u een database herstellen naar een tijdstip in het verleden. Op deze manier kunt u herstellen van beschadiging van gegevens veroorzaakt door menselijke fouten. De pakkettoegang-in-time-restore kunt u een nieuwe database maken in de server waarop ook de status van gegevens voorafgaand aan de foutieve gebeurtenis. Voor de meeste databases de herstelbewerkingen duurt minder dan 12 uur. Het duurt langer om een zeer groot of zeer actieve database te herstellen. Zie voor meer informatie over de hersteltijd, [database hersteltijd](sql-database-recovery-using-backups.md#recovery-time). 

Als de maximale ondersteunde back-up bewaarperiode voor point-in-time restore (PITR) niet voldoende is voor uw toepassing is, kunt u deze uitbreiden door het configureren van een op de lange termijn (LTR) bewaarbeleid voor de database (s). Zie voor meer informatie, [langetermijnretentie](sql-database-long-term-retention.md).

## <a name="recover-a-database-to-another-azure-region"></a>Een database herstellen naar een andere Azure-regio

Hoewel zeldzaam, kan er een storing optreden in een Azure-datacenter. Wanneer er een storing optreedt, veroorzaakt deze een bedrijfsonderbreking die mogelijk slechts een paar minuten maar ook enkele uren kan duren.

- Een optie is om te wachten tot de database weer online komt wanneer de storing in het datacenter is verholpen. Dit werkt voor toepassingen waarvoor het niet erg is dat de database offline is. Bijvoorbeeld een ontwikkelingsproject of een gratis proefversie waaraan u niet voortdurend hoeft te werken. Wanneer een datacenter een storing heeft, weet u niet hoe lang de storing kan duren, zodat u deze optie werkt alleen als u uw database even niet nodig.
- Een andere optie is om te herstellen van een database op elke server in een Azure-regio met behulp van [geografisch redundante databaseback-ups](sql-database-recovery-using-backups.md#geo-restore) (geo-herstel). Geo-herstel een geografisch redundante back-up gebruikt als de bron- en kan worden gebruikt om een database herstellen, zelfs als de database of het datacenter niet toegankelijk als gevolg van een storing is.
- Ten slotte, u kunt snel herstellen na een storing als u hebt geconfigureerd met behulp van een geo-secundaire [actieve geo-replicatie](sql-database-active-geo-replication.md) of een [automatische-failovergroep](sql-database-auto-failover-group.md) voor uw database of databases. Afhankelijk van uw keuze van deze technologieën, kunt u handmatige of automatische failover. Tijdens failover zelf slechts enkele seconden duurt, gaat de service ten minste 1 uur om deze te activeren. Dit is nodig om ervoor te zorgen dat de failover gerechtvaardigd is door de schaal van de serviceonderbreking. De failover kan ook leiden tot kleine gegevensverlies vanwege de aard van asynchrone replicatie. 

Tijdens het ontwikkelen van uw plan voor bedrijfscontinuïteit moet u weten wat de maximaal acceptabele tijd is voordat de toepassing volledig is hersteld na de verstorende gebeurtenis. De tijd die nodig is voor de toepassing volledig te herstellen, staat bekend als de beoogde hersteltijd (RTO). U moet ook weten wat de maximale periode van recente Gegevensupdates (tijdsinterval) de toepassing kan tolereren verliezen tijdens het herstellen van een niet-geplande storing. Het mogelijke gegevensverlies staat bekend als het beoogde herstelpunt (RPO).

Verschillende herstelmethoden bieden verschillende niveaus van RPO en RTO. U kunt een specifieke methode kiezen of een combinatie van methoden voor het herstel van de volledige toepassing achicethe gebruiken. De volgende tabel vergelijkt RPO en RTO van elke optie voor siteherstel.

| Herstelmethode | RTO | RPO |
| --- | --- | --- | 
| Geo-herstellen vanaf back-ups via geo-replicatie | 12 uur | 1 uur |
| Automatische failover-groepen | 1 uur | 5 s |
| Handmatige databasefailover | 30 s | 5 s |

> [!NOTE]
> *Handmatige databasefailover* verwijst naar de failover van één database naar de geo-replicatie secundaire met behulp van de [modus voor niet-geplande](sql-database-active-geo-replication.md#active-geo-replication-terminology-and-capabilities).
Zie de tabel eerder in dit artikel voor meer informatie van de automatische failover RTO en RPO.


Automatische failover-groepen gebruiken als uw toepassing voldoet aan een van deze criteria:

- Is bedrijfskritiek.
- Een service level agreement (SLA) die is niet toegestaan voor de 12 uur of meer van downtime heeft.
- Downtime kan leiden tot financiële verplichting.
- Heeft een hoge frequentie van de gegevens wijzigen en 1 uur aan gegevensverlies wordt niet geaccepteerd.
- De extra kosten van actieve geo-replicatie zijn lager dan de mogelijke financiële verplichting en het bijbehorende bedrijfsverlies.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>

U kunt een combinatie van databaseback-ups en actieve geo-replicatie, al naar gelang de vereisten van uw toepassing gebruiken. Zie voor een bespreking van overwegingen bij het ontwerp voor zelfstandige databases en elastische pools met behulp van deze functies voor bedrijfscontinuïteit, [een toepassing ontwerpen voor noodherstel voor cloud](sql-database-designing-cloud-solutions-for-disaster-recovery.md) en [noodherstel voor elastische pool strategieën voor herstel](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

De volgende secties bevatten een overzicht van de stappen voor het herstellen met behulp van de databaseback-ups of actieve geo-replicatie. Zie voor gedetailleerde stappen, waaronder planningsvereisten vereisten, stappen na herstel en informatie over het simuleren van een storing om uit te voeren een Dr-herstelanalyse [een SQL-Database herstellen na een storing](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Voorbereiden op een storing

Ongeacht de functie voor bedrijfscontinuïteit die u gebruikt, moet u:

- Identificeren en voorbereiden van de doelserver, met inbegrip van de IP-firewallregels op serverniveau, aanmeldingen en machtigingen op.
- Bepalen hoe clients en clienttoepassingen naar de nieuwe server worden omgeleid.
- Andere afhankelijkheden documenteren, zoals controle-instellingen en waarschuwingen.

Als u niet bereid goed brengen van uw toepassingen online na een failover of een databaseherstel meer tijd in beslag en waarschijnlijk ook problemen moet oplossen op een tijdstip van stress - een slechte combinatie.

### <a name="fail-over-to-a-geo-replicated-secondary-database"></a>Failover naar een secundaire database met geo-replicatie

Als u actieve geo-replicatie of automatische failover-groepen als uw herstelmechanisme gebruikt, kunt u een automatische failover-beleid configureren of gebruiken [handmatig niet-geplande failover](sql-database-active-geo-replication-portal.md#initiate-a-failover). Na het starten, is de failover zorgt ervoor dat de secundaire naar de nieuwe primaire geworden en beginnen met het registreren van nieuwe transacties en reageren op query's, met minimaal gegevensverlies voor de gegevens die nog niet gerepliceerd. Zie voor meer informatie over het ontwerpen van het failoverproces [een toepassing ontwerpen voor noodherstel voor cloud](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> Wanneer het datacenter weer online komt wordt de oude primaire automatisch opnieuw verbinding maken met de nieuwe primaire en secundaire databases worden. Als u verplaatsen van de primaire terug naar de oorspronkelijke regio wilt, kunt u een geplande failover handmatig starten (failback).

### <a name="perform-a-geo-restore"></a>Geo-herstel uitvoeren

Als u gebruikmaakt van de automatische back-ups met geografisch redundante opslag (standaard ingeschakeld), kunt u herstellen de database via [geo-herstel](sql-database-disaster-recovery.md#recover-using-geo-restore). Herstel vindt doorgaans plaats binnen 12 uur - verlies van gegevens van maximaal één uur bepaald door de wanneer de laatste logboekback-up is gemaakt en gerepliceerd. Totdat het herstellen is voltooid, kan de database geen transacties registreren en niet reageren op query's. Opmerking: de database naar de laatst beschikbare punt in tijd geo-herstel alleen hersteld.

> [!NOTE]
> Als het datacenter weer online komt voordat u uw toepassing naar de herstelde database schakelt, kunt u het herstel annuleren.

### <a name="perform-post-failover--recovery-tasks"></a>Taken na failover/hersteltaken uitvoeren

Na herstel via een van beide herstelmechanismen moet u de volgende aanvullende taken uitvoeren voordat uw gebruikers en toepassingen opnieuw actief zijn:

- Clients en clienttoepassingen omleiden naar de nieuwe server en herstelde database
- Zorg ervoor dat het juiste niveau van de server IP-firewall-regels is voldaan voor gebruikers verbinding maken of gebruiken [firewalls op databaseniveau](sql-database-firewall-configure.md#manage-server-level-ip-firewall-rules-using-the-azure-portal) zodat de juiste regels.
- Ervoor zorgen dat er geschikte aanmeldingen en machtigingen op hoofddatabaseniveau aanwezig zijn (of [ingesloten gebruikers](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) gebruiken)
- Controles configureren, indien van toepassing
- Waarschuwingen configureren, indien van toepassing

> [!NOTE]
> Als u van een failovergroep gebruikmaakt en verbinding maken met de databases met behulp van de lstener lezen / schrijven, wordt de omleiding na een failover automatisch en transparant plaatsvinden bij de toepassing.

## <a name="upgrade-an-application-with-minimal-downtime"></a>Een toepassing upgraden met minimale uitvaltijd

Soms moet een toepassing offline worden genomen vanwege gepland onderhoud, zoals een toepassingsupgrade. [Toepassingsupgrades beheren](sql-database-manage-application-rolling-upgrade.md) wordt beschreven hoe u actieve geo-replicatie voor het inschakelen van rolling upgrades van uw cloudtoepassing uitvaltijd tijdens upgrades en een herstelpad kunt opgeven als er iets misgaat.

## <a name="next-steps"></a>Volgende stappen

Zie voor een bespreking van ontwerpoverwegingen voor toepassingen voor zelfstandige databases en elastische pools, [een toepassing ontwerpen voor noodherstel voor cloud](sql-database-designing-cloud-solutions-for-disaster-recovery.md) en [elastische pool strategieën voor noodherstel](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
