---
title: "Bedrijfscontinuïteit met behulp van de cloud - databasherstel - SQL Database | Microsoft Docs"
description: "Ontdek hoe Azure SQL Database bedrijfscontinuïteit en databaseherstel met behulp van de cloud ondersteunt en u helpt om essentiële cloudtoepassingen ononderbroken uit te voeren."
keywords: "bedrijfscontinuïteit, bedrijfscontinuïteit met de cloud, databasenoodherstel, databaseherstel"
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 18e5d3f1-bfe5-4089-b6fd-76988ab29822
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 08/25/2017
ms.author: sashan
ms.openlocfilehash: c2c5f18b736c83c281d56e0bdda977a9f8c48101
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Overzicht van bedrijfscontinuïteit met Azure SQL Database

In dit overzicht worden de mogelijkheden beschreven die Azure SQL Database biedt voor bedrijfscontinuïteit en herstel na noodgevallen. Meer informatie over de opties, aanbevelingen en zelfstudies voor het herstellen van verstoren gebeurtenissen die kunnen leiden tot gegevensverlies van of ervoor zorgen dat uw database en de toepassing niet meer beschikbaar. Meer informatie over wat te doen wanneer een gebruiker of toepassing de fout is van invloed op de integriteit van gegevens, een Azure-regio een storing is of uw toepassing onderhoud vereist.

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>SQL Database-functies die u kunt gebruiken voor bedrijfscontinuïteit

SQL Database bevat verschillende functies voor bedrijfscontinuïteit, zoals geautomatiseerde back-ups en optionele databasereplicatie. Elke functie heeft verschillende kenmerken voor geschatte hersteltijd (ERT) en mogelijk gegevensverlies voor recente transacties. Wanneer u bekend bent met deze opties, kunt u eruit kiezen en ze in de meeste scenario's samen gebruiken voor verschillende doeleinden. Tijdens het ontwikkelen van uw plan voor bedrijfscontinuïteit moet u weten wat de maximaal acceptabele tijd is voordat de toepassing volledig is hersteld na de storing. Dit is de beoogde hersteltijd (RTO). U moet ook de maximale hoeveelheid recente gegevens begrijpen updates (tijdsinterval) de toepassing kan tolereren verliezen tijdens het herstellen van na de gebeurtenis verstoren - dit is uw herstelpuntdoel (RPO).

In de volgende tabel worden de ERT en RPO vergeleken voor de drie meest voorkomende scenario's.

| Mogelijkheid | Basislaag | Standaardlaag | Premiumlaag |
| --- | --- | --- | --- |
| Herstel naar een bepaald tijdstip vanuit back-up |Willekeurig herstelpunt binnen 7 dagen |Willekeurig herstelpunt binnen 35 dagen |Willekeurig herstelpunt binnen 35 dagen |
| Geo-herstel van back-ups geo-replicatie |ERT < 12 u, RPO < 1 u |ERT < 12 u, RPO < 1 u |ERT < 12 u, RPO < 1 u |
| Herstel vanuit Azure Backup Vault |ERT < 12 u, RPO < 1 wk |ERT < 12 u, RPO < 1 wk |ERT < 12 u, RPO < 1 wk |
| Actieve geo-replicatie |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |

### <a name="use-database-backups-to-recover-a-database"></a>Databaseback-ups gebruiken om een database te herstellen

SQL-Database wordt automatisch uitgevoerd wekelijks een combinatie van volledige databaseback-ups, differentiële back-ups database per uur en transactie logboekback-ups om de vijf - tien minuten om te voorkomen dat uw bedrijf verlies van gegevens. Deze back-ups worden opgeslagen in geografisch redundante opslag voor 35 dagen voor databases in de Standard en Premium Servicelagen en 7 dagen voor databases in de laag Basic-service. Zie voor meer informatie [Servicelagen](sql-database-service-tiers.md). Als de retentietermijn voor de servicelaag niet aan uw bedrijfsvereisten voldoet, verhoogt u de retentietermijn door [de servicelaag te wijzigen](sql-database-service-tiers.md). De volledige en differentiële databaseback-ups worden ook gerepliceerd naar een [gekoppeld datacenter](../best-practices-availability-paired-regions.md) voor bescherming tegen een storing in een datacenter. Zie voor meer informatie [automatische databaseback-ups](sql-database-automated-backups.md).

Als de ingebouwde bewaarperiode niet voldoende is voor uw toepassing is, kunt u deze uitbreiden door het configureren van een bewaarbeleid uit de database op lange termijn. Zie voor meer informatie [lange bewaartermijn](sql-database-long-term-retention.md).

U kunt deze automatische databaseback-ups gebruiken om een database te herstellen na diverse storingen, zowel binnen uw datacenter als naar een ander datacenter. Bij het gebruik van automatische databaseback-ups is de geschatte duur van het herstel afhankelijk van diverse factoren, waaronder het totale aantal databases dat op hetzelfde moment in dezelfde regio moet worden hersteld, de grootte van de database, de transactielogboekgrootte en de netwerkbandbreedte. De hersteltijd is meestal minder dan 12 uur. Wanneer er naar een andere gegevensregio wordt hersteld, is het mogelijke gegevensverlies beperkt tot 1 uur door de geografisch redundante opslag van differentiële back-ups die elk uur worden uitgevoerd.

> [!IMPORTANT]
> Als u wilt herstellen met behulp van geautomatiseerde back-ups, moet u lid zijn van de rol Inzender voor SQL Server of de eigenaar van het abonnement zijn. Zie [RBAC: ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md). U kunt herstellen met behulp van Azure Portal, PowerShell of de REST-API. U kunt Transact-SQL niet gebruiken.
>

Gebruik automatische back-ups als uw mechanisme voor bedrijfscontinuïteit en herstel als uw toepassing:

* Niet wordt beschouwd als bedrijfskritiek.
* Heeft een binding SLA - een uitvaltijd van 24 uur of langer niet resulteren in financiële verplichting.
* Een lage wijzigingssnelheid van gegevens (laag aantal transacties per uur) heeft en het verliezen van maximaal een uur na de wijziging een acceptabel gegevensverlies is.
* Kostengevoelig is.

Als u sneller herstel nodig hebt, gebruikt u [actieve geo-replicatie](sql-database-geo-replication-overview.md) (naast besproken). Als u kunnen gegevens herstellen vanaf een periode die ouder zijn dan 35 dagen wilt, gebruikt u [lange bewaartermijn van de back-](sql-database-long-term-retention.md). 

### <a name="use-active-geo-replication-and-auto-failover-groups-in-preview-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Actieve geo-replicatie en automatische failover groepen (in preview) te verminderen hersteltijd en beperken tot verlies van gegevens die zijn gekoppeld aan een herstelbewerking gebruiken

Naast het gebruik van databaseback-ups voor databaseherstel als er een onderbreking van de zakelijke optreedt, kunt u [actieve geo-replicatie](sql-database-geo-replication-overview.md) voor het configureren van een database met maximaal vier leesbare secundaire databases in de regio's van uw keuze. Deze secundaire databases worden synchroon gehouden met de primaire database met behulp van een asynchroon replicatiemechanisme. Deze functie wordt gebruikt als bescherming tegen business wordt onderbroken als een datacentrum optreedt of tijdens een upgrade van de toepassing. Actieve geo-replicatie kan ook worden gebruikt om te zorgen voor betere queryprestaties voor alleen-lezen-query's voor geografisch verspreide gebruikers.

Geautomatiseerd om in te schakelen en transparante failover moet u uw databases geogerepliceerde indelen in groepen met behulp van de [automatische failover groep](sql-database-geo-replication-overview.md) functie van de SQL-Database (in preview).

Als de primaire database onverwacht offline gaat of u moet deze offline nemen voor onderhoudsactiviteiten, kunt u snel promoveren van een secundaire om te worden van de primaire (ook wel een failover) en toepassingen te verbinden met de gepromoveerde primaire configureren. Als uw toepassing verbinding met de database met behulp van de listener voor de failover maakt, hoeft u niet te wijzigen van de SQL-tekenreeks verbindingsconfiguratie na een failover. Met een geplande failover is er geen gegevensverlies. Met een niet-geplande failover is er echter mogelijk een geringe hoeveelheid gegevensverlies voor recente transacties vanwege de aard van de asynchrone replicatie. Automatische failover-groepen (in preview) kunt u de failover-beleid om de potentiële gegevensverlies te minimaliseren aanpassen. Na een failover kunt u later een failback uitvoeren, volgens een planning of wanneer het datacenter weer online komt. In alle gevallen ondervinden gebruikers een geringe uitvaltijd en moeten ze opnieuw verbinding maken.

> [!IMPORTANT]
> Voor het gebruik van actieve geo-replicatie en automatische failover-groepen (in preview), moet u eigenaar van het abonnement worden of hebt beheerdersmachtigingen in SQL Server. U kunt configureren en een failover uitvoeren met behulp van de Azure portal, PowerShell of de REST-API met behulp van Azure-abonnement machtigingen of met behulp van Transact-SQL met SQL Server-machtigingen.
> 

Gebruik van actieve geo-replicatie en automatische failover groepen (in preview) als uw toepassing voldoet aan een van deze criteria voldoen:

* Is bedrijfskritiek.
* Heeft een Service Level Agreement (SLA) op basis waarvan een uitvaltijd van 24 uur of langer niet is toegestaan.
* Uitvaltijd kan leiden tot financiële verplichting.
* Heeft een hoge frequentie van gegevenswijziging en een uur aan gegevensverlies is niet acceptabel.
* De extra kosten van actieve geo-replicatie zijn lager dan de mogelijke financiële verplichting en het bijbehorende bedrijfsverlies.

>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>

## <a name="recover-a-database-after-a-user-or-application-error"></a>Een database herstellen na een gebruikers- of toepassingsfout
* Niemand is perfect! Een gebruiker kan mogelijk per ongeluk bepaalde gegevens verwijderen, per ongeluk een belangrijke tabel wissen of zelfs een volledige database verwijderen. Of een toepassing kan mogelijk per ongeluk goede gegevens overschrijven met beschadigde gegevens vanwege een toepassingsfout.

Dit zijn de opties voor herstel in dit scenario.

### <a name="perform-a-point-in-time-restore"></a>Herstel naar een bepaald tijdstip uitvoeren
U kunt de automatische back-ups gebruiken voor het herstellen van een kopie van de database naar een bekend goed punt in de tijd, mits dat punt zich binnen de retentietermijn van de database bevindt. Nadat de database is hersteld, kunt u de oorspronkelijke database vervangen door de herstelde database of de benodigde gegevens uit de herstelde gegevens naar de oorspronkelijke database kopiëren. Als de database maakt gebruik van actieve geo-replicatie, raden wij aan de vereiste gegevens kopiëren van de herstelde kopie in de oorspronkelijke database. Als u de oorspronkelijke database door de teruggezette database vervangen, moet u opnieuw configureert en opnieuw synchroniseren van actieve geo-replicatie (dit kan erg enige tijd duren voor een grote database). Terwijl dit een database naar het laatste wordt is punt in tijd, secundaire geo-database herstellen naar een willekeurig punt in tijd momenteel niet ondersteund.

Zie [Herstel naar een bepaald tijdstip](sql-database-recovery-using-backups.md#point-in-time-restore) voor meer informatie over en gedetailleerde stappen voor het herstellen van een database naar een bepaald tijdstip met behulp van Azure Portal of PowerShell. U kunt niet herstellen met behulp van Transact-SQL.

### <a name="restore-a-deleted-database"></a>Een verwijderde database herstellen
Als de database is verwijderd, maar de logische server niet is verwijderd, kunt u de verwijderde database herstellen tot het punt waarop deze is verwijderd. Hiermee wordt een back-up teruggezet op dezelfde logische SQL-server als waarvan de database is verwijderd. U kunt de database herstellen met de oorspronkelijke naam of de herstelde database een nieuwe naam geven.

Zie [Een verwijderde database herstellen](sql-database-recovery-using-backups.md#deleted-database-restore) voor meer informatie over en gedetailleerde stappen voor het herstellen van een verwijderde database met behulp van Azure Portal of PowerShell. U kunt niet herstellen met behulp van Transact-SQL.

> [!IMPORTANT]
> Als de logische server is verwijderd, kunt u een verwijderde database niet herstellen.
>
>

### <a name="restore-from-azure-backup-vault"></a>Herstel vanuit Azure Backup Vault
Als de gegevensverlies buiten de huidige bewaarperiode voor automatische back-ups opgetreden en uw database is geconfigureerd voor lange bewaartermijn, kunt u naar een nieuwe database terugzetten vanuit een wekelijkse back-up in Azure Backup-kluis. Op dit punt kunt u de oorspronkelijke database vervangen door de herstelde database of de benodigde gegevens uit de herstelde gegevens naar de oorspronkelijke database kopiëren. Als u nodig hebt voor het ophalen van een oude versie van de database vóór de upgrade van een primaire toepassing, voldoen aan een aanvraag van auditors of een juridische volgorde, dat kunt u een database met behulp van een volledige back-up zijn opgeslagen in de Azure Backup-kluis.  Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Een database herstellen naar een andere regio na een storing in een regionaal Azure-datacenter
<!-- Explain this scenario -->

Hoewel zeldzaam, kan er een storing optreden in een Azure-datacenter. Wanneer er een storing optreedt, veroorzaakt deze een bedrijfsonderbreking die mogelijk slechts een paar minuten maar ook enkele uren kan duren.

* Een optie is om te wachten tot de database weer online komt wanneer de storing in het datacenter is verholpen. Dit werkt voor toepassingen waarvoor het niet erg is dat de database offline is. Bijvoorbeeld een ontwikkelingsproject of een gratis proefversie waaraan u niet voortdurend hoeft te werken. Wanneer een datacenter een storing heeft, weet u niet hoe lang de storing kan duren, zodat u deze optie werkt alleen als u uw database voor een tijdje niet nodig.
* Een andere optie is in beide niet verder met een ander gegevensgebied als u van actieve geo-replicatie of de herstellen gebruikmaakt een database met behulp van geografisch redundante back-ups (geo-herstel). Failover duurt slechts enkele seconden tijdens het databaseherstel vanuit back-ups uur duurt.

Wanneer u actie onderneemt, hoe lang duurt het om te herstellen en het verlies van hoeveel gegevens u is afhankelijk van hoe u wilt deze zakelijke continuïteit-functies gebruiken in uw toepassing. U kunt namelijk een combinatie van databaseback-ups en actieve geo-replicatie al naar gelang de toepassingsvereisten van uw gebruiken. Zie [Een toepassing ontwerpen voor noodherstel via de cloud](sql-database-designing-cloud-solutions-for-disaster-recovery.md) en [Strategieën voor noodherstel voor elastische groepen](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) voor een bespreking van overwegingen voor het ontwerp van toepassingen voor zelfstandige databases en voor elastische pools met behulp van deze functies voor bedrijfscontinuïteit.

De volgende secties bevatten een overzicht van de stappen voor het herstellen met behulp van de databaseback-ups of actieve geo-replicatie. Zie voor gedetailleerde stappen inclusief het plannen van vereisten, herstelstappen post en informatie over een storing van een simuleren om uit te voeren van een herstel na noodgevallen detailanalyse [een SQL-Database herstellen van een storing](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Voorbereiden op een storing
Ongeacht de functie voor bedrijfscontinuïteit die u gebruikt, moet u:

* De doelserver identificeren en voorbereiden, waaronder firewallregels op serverniveau, aanmeldingen en machtigingen op het niveau van de hoofddatabase.
* Bepalen hoe clients en clienttoepassingen naar de nieuwe server worden omgeleid.
* Andere afhankelijkheden documenteren, zoals controle-instellingen en waarschuwingen.

Als u niet bereid juist brengen van uw toepassingen online na een failover of een databaseherstel meer tijd kost en waarschijnlijk ook problemen moet oplossen op een tijdstip van stress - een ongeldige combinatie.

### <a name="fail-over-to-a-geo-replicated-secondary-database"></a>Failover naar een secundaire geo-replicatie-database
Als u van actieve geo-replicatie en automatische failover-groepen (in preview) als uw herstelfunctie gebruikmaakt, kunt u beleid voor automatische failover configureren of gebruiken [handmatige failover](sql-database-disaster-recovery.md#fail-over-to-geo-replicated-secondary-server-in-the-failover-group). Eenmaal is gestart, is de failover zorgt ervoor dat de secundaire worden de nieuwe primaire en gereed voor het registreren van nieuwe transacties en reageren op query's, met minimaal gegevensverlies voor de gegevens nog niet gerepliceerd. Zie voor meer informatie over het ontwerpen van het failoverproces [ontwerpen van een toepassing voor noodherstel voor cloud](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> Als het datacentrum weer online wordt gezet wordt de oude primaire automatisch opnieuw verbinding maken met de nieuwe primaire en secundaire databases worden. Als u aan de primaire back te verplaatsen naar de oorspronkelijke regio moet, kunt u een geplande failover handmatig starten (failback). 
> 

### <a name="perform-a-geo-restore"></a>Een geo-herstel uitvoeren
Als u automatische back-ups met replicatie geografisch redundante opslag als uw herstelfunctie [starten van een database-herstelbewerking met geo-restore](sql-database-disaster-recovery.md#recover-using-geo-restore). Herstel vindt doorgaans plaats binnen 12 uur, met gegevensverlies van maximaal een uur (dit is afhankelijk van wanneer de laatste differentiële back-up per uur is gemaakt en gerepliceerd). Totdat het herstellen is voltooid, kan de database geen transacties registreren en niet reageren op query's. Terwijl dit een database naar het laatste wordt is punt in tijd, secundaire geo-database herstellen naar een willekeurig punt in tijd momenteel niet ondersteund.

> [!NOTE]
> Als het datacentrum voordat u uw toepassing via naar de herstelde database overschakelt weer online komt, kunt u het herstel annuleren.  
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
Soms moet een toepassing worden gehouden offline vanwege gepland onderhoud, zoals een upgrade van de toepassing. [Beheren van toepassingsupgrades](sql-database-manage-application-rolling-upgrade.md) wordt beschreven hoe u actieve geo-replicatie inschakelen rollende upgrades van uw cloudtoepassing uitvaltijd tijdens upgrades en geef een herstelpad als er iets mis gaat. 

## <a name="next-steps"></a>Volgende stappen
Zie [Een toepassing ontwerpen voor noodherstel via de cloud](sql-database-designing-cloud-solutions-for-disaster-recovery.md) en [Strategieën voor noodherstel voor elastische pools](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) voor een bespreking van overwegingen voor het ontwerp van toepassingen voor zelfstandige databases en voor elastische groepen.
