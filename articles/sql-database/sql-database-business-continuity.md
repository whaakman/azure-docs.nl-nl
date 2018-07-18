---
title: Bedrijfscontinuïteit met behulp van de cloud - databasherstel - SQL Database | Microsoft Docs
description: Ontdek hoe Azure SQL Database bedrijfscontinuïteit en databaseherstel met behulp van de cloud ondersteunt en u helpt om essentiële cloudtoepassingen ononderbroken uit te voeren.
keywords: bedrijfscontinuïteit, bedrijfscontinuïteit met de cloud, databasenoodherstel, databaseherstel
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.workload: On Demand
ms.date: 07/16/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: dfea1587cddbf7440771ca7007928f7e4054f61a
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39092287"
---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Overzicht van bedrijfscontinuïteit met Azure SQL Database

In dit overzicht worden de mogelijkheden beschreven die Azure SQL Database biedt voor bedrijfscontinuïteit en herstel na noodgevallen. Meer informatie over de opties, aanbevelingen en zelfstudies voor het herstellen van storingen die kunnen leiden tot gegevensverlies of ervoor zorgen dat uw databases en toepassingen niet meer beschikbaar zijn. Meer informatie over wat er gebeurt wanneer een gebruiker of toepassing-fout is van invloed op de integriteit van gegevens, een Azure-regio een storing heeft of uw toepassing onderhoud vereist.

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>SQL Database-functies die u kunt gebruiken voor bedrijfscontinuïteit

SQL Database bevat verschillende functies voor bedrijfscontinuïteit, zoals geautomatiseerde back-ups en optionele databasereplicatie. Elke functie heeft verschillende kenmerken voor geschatte hersteltijd (ERT) en mogelijk gegevensverlies voor recente transacties. Wanneer u bekend bent met deze opties, kunt u eruit kiezen en ze in de meeste scenario's samen gebruiken voor verschillende doeleinden. Tijdens het ontwikkelen van uw plan voor bedrijfscontinuïteit moet u weten wat de maximaal acceptabele tijd is voordat de toepassing volledig is hersteld na de storing. Dit is de beoogde hersteltijd (RTO). U moet ook weten wat de maximale hoeveelheid recente gegevens updates (tijdsinterval) de toepassing kan tolereren verliezen tijdens het herstellen na de storing: dit is uw beoogde herstelpunt (RPO).

De volgende tabel worden de ERT en RPO vergeleken voor elke servicelaag voor de drie meest voorkomende scenario's.

| Mogelijkheid | Basic | Standard | Premium  | Algemeen doel | Bedrijfskritiek
| --- | --- | --- | --- |--- |--- |
| Herstel naar een bepaald tijdstip vanuit back-up |Willekeurig herstelpunt binnen 7 dagen |Willekeurig herstelpunt binnen 35 dagen |Willekeurig herstelpunt binnen 35 dagen |Willekeurig herstelpunt binnen de geconfigureerde periode (maximaal 35 dagen)|Willekeurig herstelpunt binnen de geconfigureerde periode (maximaal 35 dagen)|
| Geo-herstellen vanaf back-ups via geo-replicatie |ERT < 12 u, RPO < 1 u |ERT < 12 u, RPO < 1 u |ERT < 12 u, RPO < 1 u |ERT < 12 u, RPO < 1 u|ERT < 12 u, RPO < 1 u|
| Herstel vanuit Azure Backup Vault |ERT < 12 u, RPO < 1 wk |ERT < 12 u, RPO < 1 wk |ERT < 12 u, RPO < 1 wk |ERT < 12 u, RPO < 1 wk|ERT < 12 u, RPO < 1 wk|
| Actieve geo-replicatie |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s|ERT < 30 s, RPO < 5 s|

### <a name="use-point-in-time-restore-to-recover-a-database"></a>Point-in-time restore gebruiken om een database te herstellen

SQL-Database voert automatisch wekelijks een combinatie van volledige databaseback-ups, differentiële back-ups per uur en transactie logboekback-ups om de vijf - tien minuten ter bescherming van uw bedrijf tegen gegevensverlies. Als u de [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md), en vervolgens deze back-ups worden opgeslagen in de RA-GRS-opslag gedurende 35 dagen voor databases in de lagen standaard en Premium-service en 7 dagen voor databases in de basis-servicelaag. Als de retentietermijn voor de servicelaag niet aan uw bedrijfsvereisten voldoet, verhoogt u de retentietermijn door [de servicelaag te wijzigen](sql-database-single-database-scale.md). Als u de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md), de bewaarperiode voor back-ups configureren tot maximaal 35 dagen is in het algemeen gebruik en de essentiële Business-lagen. De volledige en differentiële databaseback-ups worden ook gerepliceerd naar een [gekoppeld datacenter](../best-practices-availability-paired-regions.md) voor bescherming tegen een storing in een datacenter. Zie voor meer informatie, [automatische databaseback-ups](sql-database-automated-backups.md).

Als de maximale ondersteunde point-in-time restore (PITR) bewaarperiode is niet voldoende voor uw toepassing, kunt u deze uitbreiden door het configureren van een op de lange termijn (LTR) bewaarbeleid voor de database (s). Zie voor meer informatie, [geautomatiseerde back-ups](sql-database-automated-backups.md) en [langetermijnretentie](sql-database-long-term-retention.md).

U kunt deze automatische databaseback-ups gebruiken om een database te herstellen na diverse storingen, zowel binnen uw datacenter als naar een ander datacenter. Bij het gebruik van automatische databaseback-ups is de geschatte duur van het herstel afhankelijk van diverse factoren, waaronder het totale aantal databases dat op hetzelfde moment in dezelfde regio moet worden hersteld, de grootte van de database, de transactielogboekgrootte en de netwerkbandbreedte. De hersteltijd is meestal minder dan 12 uur. Het duurt langer om een zeer groot of zeer actieve database te herstellen. Zie voor meer informatie over de hersteltijd [database hersteltijd](sql-database-recovery-using-backups.md#recovery-time). Wanneer er naar een andere gegevensregio wordt hersteld, is het mogelijke gegevensverlies beperkt tot 1 uur door de geografisch redundante opslag van differentiële back-ups die elk uur worden uitgevoerd.

> [!IMPORTANT]
> Als u wilt herstellen met behulp van geautomatiseerde back-ups, moet u lid zijn van de rol Inzender voor SQL Server of de eigenaar van het abonnement zijn. Zie [RBAC: ingebouwde rollen](../role-based-access-control/built-in-roles.md). U kunt herstellen met behulp van Azure Portal, PowerShell of de REST-API. U kunt Transact-SQL niet gebruiken.
>

Gebruik automatische back-ups als uw mechanisme voor bedrijfscontinuïteit en herstel als uw toepassing:

* Niet wordt beschouwd als bedrijfskritiek.
* Beschikt niet over een SLA van de binding - een uitvaltijd van 24 uur of langer niet leidt tot financiële verplichting.
* Een lage wijzigingssnelheid van gegevens (laag aantal transacties per uur) heeft en het verliezen van maximaal een uur na de wijziging een acceptabel gegevensverlies is.
* Kostengevoelig is.

Als u snellere herstelmogelijkheid nodig hebt, gebruikt u [actieve geo-replicatie](sql-database-geo-replication-overview.md) (hierna besproken). Als u kunnen gegevens herstellen vanaf een periode van meer dan 35 dagen wilt, gebruikt u [langetermijnretentie](sql-database-long-term-retention.md). 

### <a name="use-active-geo-replication-and-auto-failover-groups-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Actieve geo-replicatie en automatische failover-groepen gebruiken om te verminderen hersteltijd en verlies van gegevens die zijn gekoppeld aan een herstel beperken

Naast het gebruik van databaseback-ups voor databaseherstel als er een bedrijfsonderbreking optreedt, kunt u [actieve geo-replicatie](sql-database-geo-replication-overview.md) het configureren van een database met maximaal vier leesbare secundaire databases in de regio's van uw keuze. Deze secundaire databases worden synchroon gehouden met de primaire database met behulp van een asynchroon replicatiemechanisme. Deze functie wordt gebruikt om te beveiligen tegen bedrijfsonderbreking geval van een storing in het datacenter of tijdens een upgrade van de toepassing. Actieve geo-replicatie kan ook worden gebruikt voor betere queryprestaties voor alleen-lezen query's voor geografisch verspreide gebruikers.

Geautomatiseerde om in te schakelen en transparante failover moet u uw databases geo-replicatie organiseren in groepen met behulp van de [automatische-failovergroep](sql-database-geo-replication-overview.md) functie van SQL-Database.

Als de primaire database onverwacht offline gaat of moet u deze offline te nemen voor onderhoudsactiviteiten, kunt u snel een secundaire om te worden van de primaire (ook wel een failover genoemd) en toepassingen configureren voor het verbinding maken met het opgewaardeerde primaire promoveren. Als uw toepassing verbinding met de databases met behulp van de listener voor de failover maakt, moet u niet wijzigen van de configuratie van SQL connection string na een failover. Met een geplande failover is er geen gegevensverlies. Met een niet-geplande failover is er echter mogelijk een geringe hoeveelheid gegevensverlies voor recente transacties vanwege de aard van de asynchrone replicatie. U kunt het failoverbeleid voor het minimaliseren van het verlies van gegevens met behulp van automatische failovergroepen, aanpassen. Na een failover kunt u later een failback uitvoeren, volgens een planning of wanneer het datacenter weer online komt. In alle gevallen ondervinden gebruikers een geringe uitvaltijd en moeten ze opnieuw verbinding maken.

> [!IMPORTANT]
> Voor het gebruik van actieve geo-replicatie en automatische failover-groepen, moet u eigenaar van het abonnement zijn of beheerdersmachtigingen in SQL Server. U kunt configureren en failover met behulp van Azure portal, PowerShell of de REST-API met behulp van Azure-abonnementsmachtigingen of met behulp van Transact-SQL met SQL Server-machtigingen.
> 

Actieve geo-replicatie en automatische failover-groepen gebruiken als uw toepassing voldoet aan een van deze criteria:

* Is bedrijfskritiek.
* Heeft een Service Level Agreement (SLA) op basis waarvan een uitvaltijd van 24 uur of langer niet is toegestaan.
* Downtime kan leiden tot financiële verplichting.
* Heeft een hoge frequentie van gegevenswijziging en een uur aan gegevensverlies is niet acceptabel.
* De extra kosten van actieve geo-replicatie zijn lager dan de mogelijke financiële verplichting en het bijbehorende bedrijfsverlies.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>

## <a name="recover-a-database-after-a-user-or-application-error"></a>Een database herstellen na een gebruikers- of toepassingsfout

Niemand is perfect! Een gebruiker kan mogelijk per ongeluk bepaalde gegevens verwijderen, per ongeluk een belangrijke tabel wissen of zelfs een volledige database verwijderen. Of een toepassing kan mogelijk per ongeluk goede gegevens overschrijven met beschadigde gegevens vanwege een toepassingsfout.

Dit zijn de opties voor herstel in dit scenario.

### <a name="perform-a-point-in-time-restore"></a>Herstel naar een bepaald tijdstip uitvoeren
U kunt de automatische back-ups gebruiken voor het herstellen van een kopie van de database naar een bekend goed punt in de tijd, mits dat punt zich binnen de retentietermijn van de database bevindt. Nadat de database is hersteld, kunt u de oorspronkelijke database vervangen door de herstelde database of de benodigde gegevens uit de herstelde gegevens naar de oorspronkelijke database kopiëren. Als de database maakt gebruik van actieve geo-replicatie, raden wij de vereiste gegevens van het herstelde kopie naar de oorspronkelijke database kopiëren. Als u de oorspronkelijke database door de herstelde database vervangen, moet u opnieuw configureren en synchroniseren van actieve geo-replicatie (dit kan erg enige tijd duren voor een grote database). Terwijl dit Hiermee herstelt u een database naar de laatst wordt beschikbare punt in tijd, de geo-secundaire herstelt naar een bepaald tijdstip momenteel niet ondersteund.

Zie [Herstel naar een bepaald tijdstip](sql-database-recovery-using-backups.md#point-in-time-restore) voor meer informatie over en gedetailleerde stappen voor het herstellen van een database naar een bepaald tijdstip met behulp van Azure Portal of PowerShell. U kunt niet herstellen met behulp van Transact-SQL.

### <a name="restore-a-deleted-database"></a>Een verwijderde database herstellen
Als de database is verwijderd, maar de logische server niet is verwijderd, kunt u de verwijderde database herstellen tot het punt waarop deze is verwijderd. Hiermee wordt een back-up teruggezet op dezelfde logische SQL-server als waarvan de database is verwijderd. U kunt de database herstellen met de oorspronkelijke naam of de herstelde database een nieuwe naam geven.

Zie [Een verwijderde database herstellen](sql-database-recovery-using-backups.md#deleted-database-restore) voor meer informatie over en gedetailleerde stappen voor het herstellen van een verwijderde database met behulp van Azure Portal of PowerShell. U kunt niet herstellen met behulp van Transact-SQL.

> [!IMPORTANT]
> Als de logische server is verwijderd, kunt u een verwijderde database niet herstellen.


### <a name="restore-backups-from-long-term-retention"></a>Back-ups herstellen vanuit een langetermijnretentie

Als het gegevensverlies is opgetreden buiten de huidige retentietermijn voor geautomatiseerde back-ups en uw database is geconfigureerd voor langetermijnretentie, kunt u vanuit een volledige back-up in de LTR-opslag herstellen naar een nieuwe database. Op dit punt kunt u de oorspronkelijke database vervangen door de herstelde database of de benodigde gegevens uit de herstelde gegevens naar de oorspronkelijke database kopiëren. Als u ophalen van een oude versie van uw database vóór een grote toepassingsupgrade wilt, voldoen aan een verzoek van auditors of een wettelijk bevel, dat kunt u een database met behulp van een volledige back-up opgeslagen in de Azure Backup-kluis maken.  Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Een database herstellen naar een andere regio na een storing in een regionaal Azure-datacenter
<!-- Explain this scenario -->

Hoewel zeldzaam, kan er een storing optreden in een Azure-datacenter. Wanneer er een storing optreedt, veroorzaakt deze een bedrijfsonderbreking die mogelijk slechts een paar minuten maar ook enkele uren kan duren.

* Een optie is om te wachten tot de database weer online komt wanneer de storing in het datacenter is verholpen. Dit werkt voor toepassingen waarvoor het niet erg is dat de database offline is. Bijvoorbeeld een ontwikkelingsproject of een gratis proefversie waaraan u niet voortdurend hoeft te werken. Wanneer een datacenter een storing heeft, weet u niet hoe lang de storing kan duren, zodat u deze optie werkt alleen als u uw database even niet nodig.
* Een andere optie is in een van beide niet naar een andere gegevensregio als u actieve geo-replicatie of te herstellen een database met behulp van geo-redundante databaseback-ups (geo-herstel). Failover duurt slechts enkele seconden, terwijl databaseherstel vanuit back-ups uur duurt.

Wanneer u actie onderneemt, hoe lang duurt het om te herstellen en hoeveel verlies van gegevens in rekening worden gebracht afhankelijk van hoe u bepaalt het gebruik van deze functies voor bedrijfscontinuïteit in uw toepassing. U wilt immers een combinatie van databaseback-ups en actieve geo-replicatie, al naar gelang de vereisten van uw toepassing gebruiken. Zie [Een toepassing ontwerpen voor noodherstel via de cloud](sql-database-designing-cloud-solutions-for-disaster-recovery.md) en [Strategieën voor noodherstel voor elastische groepen](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) voor een bespreking van overwegingen voor het ontwerp van toepassingen voor zelfstandige databases en voor elastische pools met behulp van deze functies voor bedrijfscontinuïteit.

De volgende secties bevatten een overzicht van de stappen voor het herstellen met behulp van de databaseback-ups of actieve geo-replicatie. Zie voor gedetailleerde stappen, waaronder planningsvereisten vereisten, stappen na herstel en informatie over het simuleren van een storing om uit te voeren een Dr-herstelanalyse [een SQL-Database herstellen na een storing](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Voorbereiden op een storing
Ongeacht de functie voor bedrijfscontinuïteit die u gebruikt, moet u:

* De doelserver identificeren en voorbereiden, waaronder firewallregels op serverniveau, aanmeldingen en machtigingen op het niveau van de hoofddatabase.
* Bepalen hoe clients en clienttoepassingen naar de nieuwe server worden omgeleid.
* Andere afhankelijkheden documenteren, zoals controle-instellingen en waarschuwingen.

Als u niet bereid goed brengen van uw toepassingen online na een failover of een databaseherstel meer tijd in beslag en waarschijnlijk ook problemen moet oplossen op een tijdstip van stress - een slechte combinatie.

### <a name="fail-over-to-a-geo-replicated-secondary-database"></a>Failover naar een secundaire database met geo-replicatie
Als u actieve geo-replicatie en automatische failover-groepen als uw herstelmechanisme gebruikt, kunt u een automatische failover-beleid configureren of gebruiken [handmatige failover](sql-database-disaster-recovery.md#fail-over-to-geo-replicated-secondary-server-in-the-failover-group). Na het starten, is de failover zorgt ervoor dat de secundaire naar de nieuwe primaire geworden en beginnen met het registreren van nieuwe transacties en reageren op query's, met minimaal gegevensverlies voor de gegevens die nog niet gerepliceerd. Zie voor meer informatie over het ontwerpen van het failoverproces [een toepassing ontwerpen voor noodherstel voor cloud](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> Wanneer het datacenter weer online komt wordt de oude primaire automatisch opnieuw verbinding maken met de nieuwe primaire en secundaire databases worden. Als u verplaatsen van de primaire terug naar de oorspronkelijke regio wilt, kunt u een geplande failover handmatig starten (failback). 
> 

### <a name="perform-a-geo-restore"></a>Geo-herstel uitvoeren
Als u automatische back-ups met geografisch redundante opslagreplicatie als uw herstelmechanisme [een databaseherstelbewerking met behulp van geo-herstel initiëren](sql-database-disaster-recovery.md#recover-using-geo-restore). Herstel vindt doorgaans plaats binnen 12 uur, met gegevensverlies van maximaal een uur (dit is afhankelijk van wanneer de laatste differentiële back-up per uur is gemaakt en gerepliceerd). Totdat het herstellen is voltooid, kan de database geen transacties registreren en niet reageren op query's. Terwijl dit Hiermee herstelt u een database naar de laatst wordt beschikbare punt in tijd, de geo-secundaire herstelt naar een bepaald tijdstip momenteel niet ondersteund.

> [!NOTE]
> Als het datacenter weer online komt voordat u uw toepassing naar de herstelde database schakelt, kunt u het herstel annuleren.  
>
>

### <a name="perform-post-failover--recovery-tasks"></a>Taken na failover/hersteltaken uitvoeren
Na herstel via een van beide herstelmechanismen moet u de volgende aanvullende taken uitvoeren voordat uw gebruikers en toepassingen opnieuw actief zijn:

* Clients en clienttoepassingen omleiden naar de nieuwe server en herstelde database
* Ervoor zorgen dat er geschikte firewallregels op serverniveau aanwezig zijn, zodat gebruikers verbinding kunnen maken (of [firewalls op databaseniveau](sql-database-firewall-configure.md#creating-and-managing-firewall-rules) gebruiken)
* Ervoor zorgen dat er geschikte aanmeldingen en machtigingen op hoofddatabaseniveau aanwezig zijn (of [ingesloten gebruikers](https://msdn.microsoft.com/library/ff929188.aspx) gebruiken)
* Controles configureren, indien van toepassing
* Waarschuwingen configureren, indien van toepassing

## <a name="upgrade-an-application-with-minimal-downtime"></a>Een toepassing upgraden met minimale uitvaltijd
Soms moet een toepassing offline worden genomen vanwege gepland onderhoud, zoals een toepassingsupgrade. [Toepassingsupgrades beheren](sql-database-manage-application-rolling-upgrade.md) wordt beschreven hoe u actieve geo-replicatie voor het inschakelen van rolling upgrades van uw cloudtoepassing uitvaltijd tijdens upgrades en een herstelpad kunt opgeven als er iets misgaat. 

## <a name="next-steps"></a>Volgende stappen
Zie [Een toepassing ontwerpen voor noodherstel via de cloud](sql-database-designing-cloud-solutions-for-disaster-recovery.md) en [Strategieën voor noodherstel voor elastische pools](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) voor een bespreking van overwegingen voor het ontwerp van toepassingen voor zelfstandige databases en voor elastische groepen.
