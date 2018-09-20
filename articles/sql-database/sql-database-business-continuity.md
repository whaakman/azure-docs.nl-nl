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
ms.date: 09/19/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 803dddd13ab3b18b6f161be9c0004d18c8d69f1e
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364191"
---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Overzicht van bedrijfscontinuïteit met Azure SQL Database

Azure SQL Database is een implementatie van de nieuwste stabiele Database-Engine van SQL Server geconfigureerd en geoptimaliseerd voor Azure-cloudomgeving waarmee [hoge beschikbaarheid](sql-database-high-availability.md) en bestendigheid met betrekking tot de fouten die mogelijk van invloed zijn op uw bedrijfsproces. **Zakelijke continuïteit** in Azure SQL Database verwijst naar de mechanismen, beleidsregels en procedures waarmee een bedrijf om door te gaan met het besturingssysteem bij storingen van apparaten wordt onderbroken, met name naar de IT-infrastructuur.  In de meeste gevallen zorgt Azure SQL Database de verstorende gebeurtenissen die mogelijk optreden in de cloudomgeving en uw zakelijke processen blijven werken. Er zijn echter enkele verstorende gebeurtenissen die kunnen niet worden verwerkt door de SQL-Database, zoals:
 - Gebruiker per ongeluk verwijderd of een rij in een tabel bijgewerkt.
 - Kwaadwillende is voltooid om te verwijderen van gegevens of een database verwijderen.
 - Aardbeving veroorzaakt een stroomstoring en tijdelijk uitgeschakeld Datacenter.
 
Dergelijke gevallen kunnen niet worden bepaald door de Azure SQL Database, dus moet u zou gebruik de functies voor bedrijfscontinuïteit in SQL-Database waarmee u uw gegevens herstellen en uw toepassingen die worden uitgevoerd.

In dit overzicht worden de mogelijkheden beschreven die Azure SQL Database biedt voor bedrijfscontinuïteit en herstel na noodgevallen. Meer informatie over de opties, aanbevelingen en zelfstudies voor het herstellen van storingen die kunnen leiden tot gegevensverlies of ervoor zorgen dat uw databases en toepassingen niet meer beschikbaar zijn. Meer informatie over wat er gebeurt wanneer een gebruiker of toepassing-fout is van invloed op de integriteit van gegevens, een Azure-regio een storing heeft of uw toepassing onderhoud vereist.

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>SQL Database-functies die u kunt gebruiken voor bedrijfscontinuïteit

Vanuit het perspectief van een database zijn er vier belangrijke mogelijke onderbreking van de scenario's:
- **Lokale hardware of software fouten** die betrekking hebben op de database-knooppunt, zoals een schijfstation fout.
- **Beschadigde gegevens of verwijdering** - meestal veroorzaakt door een toepassing bug of menselijke fouten.  Deze fouten zijn intrinsiek toepassingsspecifieke en kunnen niet als een regel worden gedetecteerd of automatisch worden verholpen door de infrastructuur.
- **Datacenter-onderbreking**, mogelijk veroorzaakt door een natuurramp.  In dit scenario moeten bepaalde mate van geografische redundantie met failover van de toepassing naar een ander datacenter.
- **Upgraden of onderhoud fouten** : onverwachte problemen die tijdens optreden geplande upgrades of onderhoud voor een toepassing of de database mogelijk snelle terugdraaien naar een eerdere database staat.

SQL Database biedt verschillende functies voor bedrijfscontinuïteit, met inbegrip van geautomatiseerde back-ups en optionele databasereplicatie die deze scenario's kunt beperken. U moet eerst, om te begrijpen hoe SQL-Database [architectuur voor hoge beschikbaarheid](sql-database-high-availability.md) biedt een beschikbaarheid van 99,99% en bestendigheid met betrekking tot bepaalde verstorende gebeurtenissen die invloed kunnen zijn op uw bedrijfsprocessen.
Vervolgens kunt u meer informatie over de aanvullende methoden die u gebruiken kunt om te herstellen van de verstorende gebeurtenissen die kunnen niet worden verwerkt door de SQL-Database-architectuur voor hoge beschikbaarheid, zoals:
 - [Tijdelijke tabellen](sql-database-temporal-tables.md) u mogelijk te herstellen van rij-versies van elk gewenst moment in-time.
 - [Ingebouwde geautomatiseerde back-ups](sql-database-automated-backups.md) en [verwijzen in Time Restore](sql-database-recovery-using-backups.md#point-in-time-restore) kunt u de volledige database herstellen naar een bepaald moment in de tijd binnen de afgelopen 35 dagen.
 - U kunt [een verwijderde database herstellen](sql-database-recovery-using-backups.md#deleted-database-restore) op het punt waarop deze is verwijderd als de **logische server is niet verwijderd**.
 - [Langetermijnretentie van back-up](sql-database-long-term-retention.md) kunt u de back-ups op 10 jaar houden.
 - [Geo-replicatie](sql-database-geo-replication-overview.md) kan de toepassing snel noodherstel in het geval van de schaal van een datacenterstoring kunnen uitvoeren.

Elke functie heeft verschillende kenmerken voor geschatte hersteltijd (ERT) en mogelijk gegevensverlies voor recente transacties. Wanneer u bekend bent met deze opties, kunt u eruit kiezen en ze in de meeste scenario's samen gebruiken voor verschillende doeleinden. Tijdens het ontwikkelen van uw plan voor bedrijfscontinuïteit, moet u de maximaal acceptabele tijd voordat de toepassing volledig is hersteld na de storing begrijpen. De tijd die nodig is voor de toepassing volledig te herstellen, staat bekend als de beoogde hersteltijd (RTO). U moet ook weten wat de maximale periode van recente Gegevensupdates (tijdsinterval) de toepassing kan tolereren verliezen tijdens het herstellen na de storing. De periode van updates die u in het ergste geval kan kwijtraken staat bekend als het beoogde herstelpunt (RPO).

De volgende tabel worden de ERT en RPO vergeleken voor elke servicelaag voor de drie meest voorkomende scenario's.

| Mogelijkheid | Basic | Standard | Premium  | Algemeen doel | Bedrijfskritiek
| --- | --- | --- | --- |--- |--- |
| Herstel naar een bepaald tijdstip vanuit back-up |Willekeurig herstelpunt binnen zeven dagen |Willekeurig herstelpunt binnen 35 dagen |Willekeurig herstelpunt binnen 35 dagen |Willekeurig herstelpunt binnen de geconfigureerde periode (maximaal 35 dagen)|Willekeurig herstelpunt binnen de geconfigureerde periode (maximaal 35 dagen)|
| Geo-herstellen vanaf back-ups via geo-replicatie |ERT < 12 u, RPO < 1 uur |ERT < 12 u, RPO < 1 uur |ERT < 12 u, RPO < 1 uur |ERT < 12 u, RPO < 1 uur|ERT < 12 u, RPO < 1 uur|
| Herstellen vanuit een langetermijnretentie voor SQL |ERT < 12 u, RPO < 1 wk |ERT < 12 u, RPO < 1 wk |ERT < 12 u, RPO < 1 wk |ERT < 12 u, RPO < 1 wk|ERT < 12 u, RPO < 1 wk|
| Actieve geo-replicatie |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s|ERT < 30 s, RPO < 5 s|

## <a name="recover-a-database-to-the-existing-server"></a>Een database aan de bestaande server herstellen

SQL-Database voert automatisch een combinatie van volledige databaseback-ups wekelijks, differentiële back-ups in het algemeen genomen elke 12 uur en transactie logboekback-ups elke 5-10 minuten voor uw bedrijf beschermen tegen gegevensverlies. De back-ups worden opgeslagen in de RA-GRS-opslag gedurende 35 dagen voor alle service-lagen, behalve de Servicelagen Basic DTU waarin de back-ups voor zeven dagen worden opgeslagen. Zie voor meer informatie, [automatische databaseback-ups](sql-database-automated-backups.md). U kunt een bestaande databaseformulier de geautomatiseerde back-ups naar een eerder tijdstip in herstellen tijd als een nieuwe database op dezelfde logische server met behulp van de Azure-portal, PowerShell of de REST-API. Zie voor meer informatie, [Point-in-time restore](sql-database-recovery-using-backups.md#point-in-time-restore).

Als de maximale ondersteunde point-in-time restore (PITR) bewaarperiode is niet voldoende voor uw toepassing, kunt u deze uitbreiden door het configureren van een op de lange termijn (LTR) bewaarbeleid voor de database (s). Zie voor meer informatie, [langetermijnretentie](sql-database-long-term-retention.md).

U kunt deze automatische databaseback-ups gebruiken om een database te herstellen na diverse storingen, zowel binnen uw datacenter als naar een ander datacenter. Bij het gebruik van automatische databaseback-ups is de geschatte duur van het herstel afhankelijk van diverse factoren, waaronder het totale aantal databases dat op hetzelfde moment in dezelfde regio moet worden hersteld, de grootte van de database, de transactielogboekgrootte en de netwerkbandbreedte. De hersteltijd is meestal minder dan 12 uur. Het duurt langer om een zeer groot of zeer actieve database te herstellen. Zie voor meer informatie over de hersteltijd, [database hersteltijd](sql-database-recovery-using-backups.md#recovery-time). Wanneer er naar een andere gegevensregio wordt hersteld, is het mogelijke gegevensverlies beperkt tot 1 uur met gebruik van geografisch redundante back-ups.

Gebruik van geautomatiseerde back-ups en [point-in-time restore](sql-database-recovery-using-backups.md#point-in-time-restore) als uw mechanisme voor zakelijke continuïteit en herstel als uw toepassing:

* Niet wordt beschouwd als bedrijfskritiek.
* Beschikt niet over een SLA van de binding - een uitvaltijd van 24 uur of langer niet leidt tot financiële verplichting.
* Een lage wijzigingssnelheid van gegevens (laag aantal transacties per uur) heeft en het verliezen van maximaal een uur na de wijziging een acceptabel gegevensverlies is.
* Kostengevoelig is.

Als u snellere herstelmogelijkheid nodig hebt, gebruikt u [actieve geo-replicatie](sql-database-geo-replication-overview.md) (hierna besproken). Als u kunnen gegevens herstellen vanaf een periode van meer dan 35 dagen wilt, gebruikt u [langetermijnretentie](sql-database-long-term-retention.md). 

## <a name="recover-a-database-to-another-region"></a>Een database herstellen naar een andere regio
<!-- Explain this scenario -->

Hoewel zeldzaam, kan er een storing optreden in een Azure-datacenter. Wanneer er een storing optreedt, veroorzaakt deze een bedrijfsonderbreking die mogelijk slechts een paar minuten maar ook enkele uren kan duren.

* Een optie is om te wachten tot de database weer online komt wanneer de storing in het datacenter is verholpen. Dit werkt voor toepassingen waarvoor het niet erg is dat de database offline is. Bijvoorbeeld een ontwikkelingsproject of een gratis proefversie waaraan u niet voortdurend hoeft te werken. Wanneer een datacenter een storing heeft, weet u niet hoe lang de storing kan duren, zodat u deze optie werkt alleen als u uw database even niet nodig.
* Een andere optie is om te herstellen van een database op elke server in een Azure-regio met behulp van [geografisch redundante databaseback-ups](sql-database-recovery-using-backups.md#geo-restore) (geo-herstel). Geo-herstel een geografisch redundante back-up gebruikt als de bron- en kan worden gebruikt om een database herstellen, zelfs als de database of het datacenter niet toegankelijk als gevolg van een storing is.
* Ten slotte kunt u snel een secundaire op een andere gegevensregio worden de primaire (ook wel een failover genoemd) en toepassingen te verbinden met het opgewaardeerde primaire als u actieve geo-replicatie configureren promoveren. Er is mogelijk een geringe hoeveelheid gegevensverlies voor recente transacties vanwege de aard van de asynchrone replicatie. U kunt het failoverbeleid voor het minimaliseren van het verlies van gegevens met behulp van automatische failovergroepen, aanpassen. In alle gevallen ondervinden gebruikers een geringe uitvaltijd en moeten ze opnieuw verbinding maken. Failover duurt slechts enkele seconden, terwijl databaseherstel vanuit back-ups uur duurt.

Als u wilt een failover naar een andere regio, kunt u [actieve geo-replicatie](sql-database-geo-replication-overview.md) het configureren van een database met maximaal vier leesbare secundaire databases in de regio's van uw keuze. Deze secundaire databases worden synchroon gehouden met de primaire database met behulp van een asynchroon replicatiemechanisme. 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>


> [!IMPORTANT]
> Voor het gebruik van actieve geo-replicatie en automatische failover-groepen, moet u eigenaar van het abonnement zijn of beheerdersmachtigingen in SQL Server. U kunt configureren en failover met behulp van Azure portal, PowerShell of de REST-API met behulp van Azure-abonnementsmachtigingen of met behulp van Transact-SQL met SQL Server-machtigingen.
> 

Deze functie wordt gebruikt om te beveiligen tegen bedrijfsonderbreking geval van een storing in het datacenter of tijdens een upgrade van de toepassing. Geautomatiseerde om in te schakelen en transparante failover moet u uw databases geo-replicatie organiseren in groepen met behulp van de [automatische-failovergroep](sql-database-geo-replication-overview.md) functie van SQL-Database. Actieve geo-replicatie en automatische failover-groepen gebruiken als uw toepassing voldoet aan een van deze criteria:

* Is bedrijfskritiek.
* Heeft een Service Level Agreement (SLA) op basis waarvan een uitvaltijd van 24 uur of langer niet is toegestaan.
* Downtime kan leiden tot financiële verplichting.
* Heeft een hoge frequentie van gegevenswijziging en een uur aan gegevensverlies is niet acceptabel.
* De extra kosten van actieve geo-replicatie zijn lager dan de mogelijke financiële verplichting en het bijbehorende bedrijfsverlies.

Wanneer u actie onderneemt, hoe lang duurt het om te herstellen en hoeveel verlies van gegevens in rekening worden gebracht afhankelijk van hoe u bepaalt het gebruik van deze functies voor bedrijfscontinuïteit in uw toepassing. U wilt immers een combinatie van databaseback-ups en actieve geo-replicatie, al naar gelang de vereisten van uw toepassing gebruiken. Zie voor een bespreking van ontwerpoverwegingen voor toepassingen voor zelfstandige databases en elastische pools met behulp van deze functies voor bedrijfscontinuïteit, [een toepassing ontwerpen voor noodherstel voor cloud](sql-database-designing-cloud-solutions-for-disaster-recovery.md) en [elastische strategieën voor noodherstel van toepassingen](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

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
Als u gebruikmaakt van de automatische back-ups met geografisch redundante opslag (standaard ingeschakeld), kunt u herstellen de database via [geo-herstel](sql-database-disaster-recovery.md#recover-using-geo-restore). Herstel vindt doorgaans plaats binnen 12 uur - verlies van gegevens van maximaal één uur bepaald door de wanneer de laatste per uur differentiële back-up is gemaakt en gerepliceerd. Totdat het herstellen is voltooid, kan de database geen transacties registreren en niet reageren op query's. Opmerking: de database naar de laatst beschikbare punt in tijd geo-herstel alleen hersteld.

> [!NOTE]
> Als het datacenter weer online komt voordat u uw toepassing naar de herstelde database schakelt, kunt u het herstel annuleren.  

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
Zie voor een bespreking van ontwerpoverwegingen voor toepassingen voor zelfstandige databases en elastische pools, [een toepassing ontwerpen voor noodherstel voor cloud](sql-database-designing-cloud-solutions-for-disaster-recovery.md) en [elastische pool strategieën voor noodherstel](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
