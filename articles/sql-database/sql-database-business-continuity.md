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
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/13/2016
ms.author: carlrab;sashan
translationtype: Human Translation
ms.sourcegitcommit: 747f6ca642a33c4ce9bcaacad4976e8eaed8fa44
ms.openlocfilehash: f642cfade2369f5c758ab45994c7cf3f37b6d4c5


---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Overzicht van bedrijfscontinuïteit met Azure SQL Database
In dit overzicht worden de mogelijkheden beschreven die Azure SQL Database biedt voor bedrijfscontinuïteit en herstel na noodgevallen. Het biedt opties, aanbevelingen en zelfstudies voor het herstellen van storingen die kunnen leiden tot gegevensverlies of ervoor zorgen dat uw database en toepassing niet meer beschikbaar zijn. Ook wordt besproken wat u moet doen wanneer een gebruiker of toepassingsfout de gegevensintegriteit in gevaar brengt, er een storing in een Azure-regio is of uw toepassing onderhoud vereist. 

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>SQL Database-functies die u kunt gebruiken voor bedrijfscontinuïteit
SQL Database bevat verschillende functies voor bedrijfscontinuïteit, zoals geautomatiseerde back-ups en optionele databasereplicatie. Elke functie heeft verschillende kenmerken voor geschatte hersteltijd (ERT) en mogelijk gegevensverlies voor recente transacties. Wanneer u bekend bent met deze opties, kunt u eruit kiezen en ze in de meeste scenario's samen gebruiken voor verschillende doeleinden. Tijdens het ontwikkelen van uw plan voor bedrijfscontinuïteit moet u weten wat de maximaal acceptabele tijd is voordat de toepassing volledig is hersteld na de storing. Dit is de beoogde hersteltijd (RTO). U moet ook weten wat de maximale hoeveelheid recente gegevensupdates (tijdsinterval) is die de toepassing kan verliezen tijdens het herstellen van de storing. Dit is het beoogde herstelpunt (RPO). 

In de volgende tabel worden de ERT en RPO vergeleken voor de drie meest voorkomende scenario's.

| Mogelijkheid | Basislaag | Standaardlaag | Premiumlaag |
| --- | --- | --- | --- |
| Herstel naar een bepaald tijdstip vanuit back-up |Willekeurig herstelpunt binnen 7 dagen |Willekeurig herstelpunt binnen 35 dagen |Willekeurig herstelpunt binnen 35 dagen |
| Geo-herstel vanuit back-ups via geo-replicatie |ERT < 12 u, RPO < 1 u |ERT < 12 u, RPO < 1 u |ERT < 12 u, RPO < 1 u |
| Herstel vanuit Azure Backup Vault |ERT < 12 u, RPO < 1 wk |ERT < 12 u, RPO < 1 wk |ERT < 12 u, RPO < 1 wk |
| Actieve Geo-replicatie |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |

### <a name="use-database-backups-to-recover-a-database"></a>Databaseback-ups gebruiken om een database te herstellen
Door SQL Database wordt automatisch wekelijks een combinatie van volledige databaseback-ups, differentiële databaseback-ups om het uur en transactielogboekback-ups om de vijf minuten uitgevoerd om uw bedrijf te beschermen tegen gegevensverlies. Deze back-ups worden opgeslagen in lokaal redundante opslag gedurende 35 dagen voor databases in de Standaard- en Premium-servicelaag en gedurende zeven dagen voor databases in de Basis-servicelaag. Zie [Servicelagen](sql-database-service-tiers.md) voor meer informatie over servicelagen. Als de retentietermijn voor de servicelaag niet aan uw bedrijfsvereisten voldoet, verhoogt u de retentietermijn door [de servicelaag te wijzigen](sql-database-scale-up.md). De volledige en differentiële databaseback-ups worden ook gerepliceerd naar een [gekoppeld datacenter](../best-practices-availability-paired-regions.md) voor bescherming tegen een storing in een datacenter. Zie [automatische databaseback-ups](sql-database-automated-backups.md) voor meer informatie.

Als de ingebouwde retentietermijn niet voldoende is voor uw toepassing, kunt u deze uitbreiden door voor uw database(s) langetermijnretentiebeleid te configureren. Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie. 

U kunt deze automatische databaseback-ups gebruiken om een database te herstellen na diverse storingen, zowel binnen uw datacenter als naar een ander datacenter. Bij het gebruik van automatische databaseback-ups is de geschatte duur van het herstel afhankelijk van diverse factoren, waaronder het totale aantal databases dat op hetzelfde moment in dezelfde regio moet worden hersteld, de grootte van de database, de transactielogboekgrootte en de netwerkbandbreedte. In de meeste gevallen bedraagt de hersteltijd minder dan 12 uur. Wanneer er naar een andere gegevensregio wordt hersteld, is het mogelijke gegevensverlies beperkt tot 1 uur door de geografisch redundante opslag van differentiële back-ups die elk uur worden uitgevoerd. 

> [!IMPORTANT]
> Als u wilt herstellen met behulp van geautomatiseerde back-ups, moet u lid zijn van de rol Inzender voor SQL Server of de eigenaar van het abonnement zijn. Zie [RBAC: ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md). U kunt herstellen met behulp van Azure Portal, PowerShell of de REST-API. U kunt Transact-SQL niet gebruiken.
> 
> 

Gebruik automatische back-ups als uw mechanisme voor bedrijfscontinuïteit en herstel als uw toepassing:

* Niet wordt beschouwd als bedrijfskritiek.
* Geen bindende SLA bevat en een uitvaltijd van 24 uur of langer daarom niet leidt tot een financiële verplichting.
* Een lage wijzigingssnelheid van gegevens (laag aantal transacties per uur) heeft en het verliezen van maximaal een uur na de wijziging een acceptabel gegevensverlies is. 
* Kostengevoelig is. 

Gebruik [Actieve geo-replicatie](sql-database-geo-replication-overview.md) (dit wordt hierna besproken) als u een snellere herstelmogelijkheid nodig hebt. Als u gegevens moet kunnen herstellen van een periode van meer dan 35 dagen, kunt u overwegen om uw database regelmatig te archiveren naar een BACPAC-bestand (een gecomprimeerd bestand met uw databaseschema en bijbehorende gegevens) dat wordt opgeslagen in Azure Blob Storage of op een andere locatie naar keuze. Zie [Een databasekopie maken](sql-database-copy.md) en [De databasekopie exporteren](sql-database-export.md) voor meer informatie over het maken van een voor transacties consistent databasearchief. 

### <a name="use-active-geo-replication-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Actieve geo-replicatie gebruiken om de hersteltijd te verminderen en het verlies van gegevens die gepaard gaan met een herstelbewerking, te beperken
Naast het gebruik van databaseback-ups voor databaseherstel in het geval van een bedrijfsonderbreking kunt u [Actieve geo-replicatie](sql-database-geo-replication-overview.md) gebruiken voor het configureren van een database om zo maximaal vier leesbare secundaire databases in de regio’s van uw keuze te hebben. Deze secundaire databases worden synchroon gehouden met de primaire database met behulp van een asynchroon replicatiemechanisme. Deze functie wordt gebruikt ter bescherming tegen bedrijfsonderbreking in het geval van een storing in een datacenter of tijdens een toepassingsupgrade. Actieve geo-replicatie kan ook worden gebruikt om te zorgen voor betere queryprestaties voor alleen-lezen query's voor geografisch verspreide gebruikers.

Als de primaire database onverwacht offline gaat of u deze offline moet halen voor onderhoudsactiviteiten, kunt u snel een secundaire database opwaarderen tot primaire database (ook wel een failover genoemd) en toepassingen configureren om verbinding te maken met de nieuwe opgewaardeerde primaire database. Met een geplande failover is er geen gegevensverlies. Met een niet-geplande failover is er echter mogelijk een geringe hoeveelheid gegevensverlies voor recente transacties vanwege de aard van de asynchrone replicatie. Na een failover kunt u later een failback uitvoeren, volgens een planning of wanneer het datacenter weer online komt. In alle gevallen ondervinden gebruikers een geringe uitvaltijd en moeten ze opnieuw verbinding maken. 

> [!IMPORTANT]
> Voor het gebruik van actieve geo-replicatie moet u eigenaar van het abonnement zijn of beheerdersmachtigingen in SQL Server hebben. U kunt configureren en een failover uitvoeren via Azure Portal, PowerShell of de REST-API met behulp van machtigingen in het abonnement of met behulp van Transact-SQL met behulp van machtigingen in SQL Server.
> 
> 

Gebruik actieve geo-replicatie als uw toepassing voldoet aan een van deze criteria:

* Is bedrijfskritiek.
* Heeft een Service Level Agreement (SLA) op basis waarvan een uitvaltijd van 24 uur of langer niet is toegestaan.
* Uitvaltijd leidt tot een financiële verplichting.
* Heeft een hoge frequentie van gegevenswijziging en een uur aan gegevensverlies is niet acceptabel.
* De extra kosten van actieve geo-replicatie zijn lager dan de mogelijke financiële verplichting en het bijbehorende bedrijfsverlies.

## <a name="recover-a-database-after-a-user-or-application-error"></a>Een database herstellen na een gebruikers- of toepassingsfout
* Niemand is perfect! Een gebruiker kan mogelijk per ongeluk bepaalde gegevens verwijderen, per ongeluk een belangrijke tabel wissen of zelfs een volledige database verwijderen. Of een toepassing kan mogelijk per ongeluk goede gegevens overschrijven met beschadigde gegevens vanwege een toepassingsfout. 

Dit zijn de opties voor herstel in dit scenario.

### <a name="perform-a-point-in-time-restore"></a>Herstel naar een bepaald tijdstip uitvoeren
U kunt de automatische back-ups gebruiken voor het herstellen van een kopie van de database naar een bekend goed punt in de tijd, mits dat punt zich binnen de retentietermijn van de database bevindt. Nadat de database is hersteld, kunt u de oorspronkelijke database vervangen door de herstelde database of de benodigde gegevens uit de herstelde gegevens naar de oorspronkelijke database kopiëren. Als de database gebruikmaakt van actieve geo-replicatie, raden wij u aan de vereiste gegevens van het herstelde kopie naar de oorspronkelijke database te kopiëren. Als u de oorspronkelijke database door de herstelde database vervangt, moet u actieve geo-replicatie opnieuw configureren en opnieuw synchroniseren (dit kan voor een grote database lang duren). 

Zie [Herstel naar een bepaald tijdstip](sql-database-recovery-using-backups.md#point-in-time-restore) voor meer informatie over en gedetailleerde stappen voor het herstellen van een database naar een bepaald tijdstip met behulp van Azure Portal of PowerShell. U kunt niet herstellen met behulp van Transact-SQL.

### <a name="restore-a-deleted-database"></a>Een verwijderde database herstellen
Als de database is verwijderd, maar de logische server niet is verwijderd, kunt u de verwijderde database herstellen tot het punt waarop deze is verwijderd. Hiermee wordt een back-up teruggezet op dezelfde logische SQL-server als waarvan de database is verwijderd. U kunt de database herstellen met de oorspronkelijke naam of de herstelde database een nieuwe naam geven.

Zie [Een verwijderde database herstellen](sql-database-recovery-using-backups.md#deleted-database-restore) voor meer informatie over en gedetailleerde stappen voor het herstellen van een verwijderde database met behulp van Azure Portal of PowerShell. U kunt niet herstellen met behulp van Transact-SQL.

> [!IMPORTANT]
> Als de logische server is verwijderd, kunt u een verwijderde database niet herstellen. 
> 
> 

### <a name="restore-from-azure-backup-vault"></a>Herstel vanuit Azure Backup Vault
Als het gegevensverlies buiten de huidige retentietermijn voor geautomatiseerde back-ups is opgetreden en uw database is geconfigureerd voor langetermijnretentie, kunt u vanuit een wekelijkse Azure Backup Vault herstellen naar een nieuwe database. Op dit punt kunt u de oorspronkelijke database vervangen door de herstelde database of de benodigde gegevens uit de herstelde gegevens naar de oorspronkelijke database kopiëren. Als u een oude versie van de database wilt ophalen van vóór een grote toepassingsupgrade, wilt voldoen aan een verzoek van auditors of een wettelijk bevel, kunt u een nieuwe database maken met behulp van een volledige back-up die is opgeslagen in Azure Backup Vault.  Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Een database herstellen naar een andere regio na een storing in een regionaal Azure-datacenter
<!-- Explain this scenario -->

Hoewel zeldzaam, kan er een storing optreden in een Azure-datacenter. Wanneer er een storing optreedt, veroorzaakt deze een bedrijfsonderbreking die mogelijk slechts een paar minuten maar ook enkele uren kan duren. 

* Een optie is om te wachten tot de database weer online komt wanneer de storing in het datacenter is verholpen. Dit werkt voor toepassingen waarvoor het niet erg is dat de database offline is. Bijvoorbeeld een ontwikkelingsproject of een gratis proefversie waaraan u niet voortdurend hoeft te werken. Wanneer een datacenter een storing heeft, weet u niet hoe lang de storing duurt. Deze optie werkt dus alleen als u de database even niet nodig hebt.
* Een andere optie is een failover naar een andere gegevensregio uit te voeren als u actieve geo-replicatie gebruikt of te herstellen met behulp van geo-redundante databaseback-ups (geo-herstel). Failover duurt slechts enkele seconden, terwijl het herstel vanuit back-ups uren in beslag neemt.

Wanneer u actie onderneemt, zijn de tijd die het kost om te herstellen en de hoeveelheid gegevensverlies waarmee u te maken krijgt in het geval van een storing in een datacenter, afhankelijk van hoe u besluit de functies voor bedrijfscontinuïteit die hierboven worden besproken, in uw toepassing te gebruiken. U kunt er namelijk voor kiezen een combinatie van databaseback-ups en actieve geo-replicatie te gebruiken al naar gelang de vereisten van uw toepassingsvereisten. Zie [Een toepassing ontwerpen voor noodherstel via de cloud](sql-database-designing-cloud-solutions-for-disaster-recovery.md) en [Strategieën voor noodherstel voor elastische groepen](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) voor een bespreking van overwegingen voor het ontwerp van toepassingen voor zelfstandige databases en voor elastische pools met behulp van deze functies voor bedrijfscontinuïteit.

De onderstaande secties bieden een overzicht van de stappen om te herstellen met behulp van de databaseback-ups of actieve geo-replicatie. Zie [Een SQL Database herstellen na een storing](sql-database-disaster-recovery.md) voor gedetailleerde stappen, waaronder planningsvereisten, stappen na herstel en informatie over het simuleren van een storing om een DR-herstelanalyse uit te voeren.

### <a name="prepare-for-an-outage"></a>Voorbereiden op een storing
Ongeacht de functie voor bedrijfscontinuïteit die u gebruikt, moet u:

* De doelserver identificeren en voorbereiden, waaronder firewallregels op serverniveau, aanmeldingen en machtigingen op het niveau van de hoofddatabase.
* Bepalen hoe clients en clienttoepassingen naar de nieuwe server worden omgeleid.
* Andere afhankelijkheden documenteren, zoals controle-instellingen en waarschuwingen. 

Als u niet op de juiste manier plant en voorbereidingen treft, neemt het online brengen van uw toepassingen na een failover of een herstel meer tijd in beslag en moeten er in tijden van stress waarschijnlijk ook een aantal problemen worden opgelost: een slechte combinatie.

### <a name="failover-to-a-geo-replicated-secondary-database"></a>Failover naar een secundaire database via geo-replicatie
Als u actieve geo-replicatie gebruikt als uw herstelmechanisme, kunt u [een failover naar een secundaire database via geo-replicatie afdwingen](sql-database-disaster-recovery.md#failover-to-geo-replicated-secondary-database). Binnen enkele seconden is de secundaire database opgewaardeerd tot nieuwe primaire database en klaar voor het registreren van nieuwe transacties en het reageren op alle query's, met slechts enkele seconden gegevensverlies voor de gegevens die nog niet waren gerepliceerd. Zie [Een toepassing ontwerpen voor noodherstel via de cloud](sql-database-designing-cloud-solutions-for-disaster-recovery.md) voor meer informatie over het automatiseren van het failoverproces.

> [!NOTE]
> Als het datacenter weer online komt, kunt u een failback uitvoeren naar de oorspronkelijke primaire database (of niet).
> 
> 

### <a name="perform-a-geo-restore"></a>Geo-herstel uitvoeren
Als u automatische back-ups met geografisch redundante opslagreplicatie als herstelfunctie wilt gebruiken, [start u een databaseherstelbewerking met behulp van geo-herstel](sql-database-disaster-recovery.md#recover-using-geo-restore). Herstel vindt doorgaans plaats binnen 12 uur, met gegevensverlies van maximaal een uur (dit is afhankelijk van wanneer de laatste differentiële back-up per uur is gemaakt en gerepliceerd). Totdat het herstellen is voltooid, kan de database geen transacties registreren en niet reageren op query's. 

> [!NOTE]
> Als het datacenter weer online komt voordat u uw toepassing hebt overgeschakeld naar de herstelde database, kunt u de herstelbewerking eenvoudigweg annuleren.  
> 
> 

### <a name="perform-post-failover--recovery-tasks"></a>Taken na failover/hersteltaken uitvoeren
Na herstel via een van beide herstelmechanismen moet u de volgende aanvullende taken uitvoeren voordat uw gebruikers en toepassingen opnieuw actief zijn:

* Clients en clienttoepassingen omleiden naar de nieuwe server en herstelde database
* Ervoor zorgen dat er geschikte firewallregels op serverniveau aanwezig zijn, zodat gebruikers verbinding kunnen maken (of [firewalls op databaseniveau](sql-database-firewall-configure.md#creating-database-level-firewall-rules) gebruiken)
* Ervoor zorgen dat er geschikte aanmeldingen en machtigingen op hoofddatabaseniveau aanwezig zijn (of [ingesloten gebruikers](https://msdn.microsoft.com/library/ff929188.aspx) gebruiken)
* Controles configureren, indien van toepassing
* Waarschuwingen configureren, indien van toepassing

## <a name="upgrade-an-application-with-minimal-downtime"></a>Een toepassing upgraden met minimale uitvaltijd
Soms moet een toepassing offline worden genomen vanwege gepland onderhoud, zoals een toepassingsupgrade. In [Toepassingsupgrades beheren](sql-database-manage-application-rolling-upgrade.md) wordt beschreven hoe u actieve geo-replicatie kunt gebruiken voor het inschakelen van rolling upgrades van uw cloudtoepassing om de uitvaltijd tijdens upgrades te minimaliseren en een herstelpad kunt opgeven in het geval er iets verkeerd gaat. In dit artikel worden twee verschillende methoden voor het uitvoeren van het upgradeproces en de voordelen en nadelen van elke optie nader bekeken.

## <a name="next-steps"></a>Volgende stappen
Zie [Een toepassing ontwerpen voor noodherstel via de cloud](sql-database-designing-cloud-solutions-for-disaster-recovery.md) en [Strategieën voor noodherstel voor elastische pools](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) voor een bespreking van overwegingen voor het ontwerp van toepassingen voor zelfstandige databases en voor elastische groepen.




<!--HONumber=Dec16_HO2-->


