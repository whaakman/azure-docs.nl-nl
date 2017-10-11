---
title: Azure SQL Database Veelgestelde vragen | Microsoft Docs
description: Antwoorden op algemene vragen klanten vragen over cloud-databases en Azure SQL Database, van Microsoft relationele databasebeheersysteem (RDBMS) en -database als een service in de cloud.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 1da12abc-0646-43ba-b564-e3b049a6487f
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 02/07/2017
ms.author: sashan;carlrab
ms.openlocfilehash: 6ed02ead07c50b9a49e8868756b6f957d7b49b99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="sql-database-faq"></a>Veelgestelde vragen over SQL Database

## <a name="what-is-the-current-version-of-sql-database"></a>Wat is de huidige versie van SQL-Database?
De huidige versie van SQL Database is V12. Versie V11 buiten gebruik gesteld.

## <a name="what-is-the-sla-for-sql-database"></a>Wat is de SLA voor SQL-Database?
We garanderen dat klanten ten minste 99,99% van de tijd kunnen gebruikmaken van de verbinding tussen hun enkele of elastische Basic, Standard of Premium Microsoft Azure SQL Database en onze internetgateway. Zie voor meer informatie [SLA](http://azure.microsoft.com/support/legal/sla/).

## <a name="how-do-i-reset-the-password-for-the-server-admin"></a>Hoe stel ik het wachtwoord voor de serverbeheerder
In de [Azure-portal](https://portal.azure.com) klikt u op **SQL-Servers**, selecteert u de server uit de lijst en klik vervolgens op **wachtwoord opnieuw instellen**.

## <a name="how-do-i-manage-databases-and-logins"></a>Hoe kan ik databases en aanmeldingen beheren?
Zie [databases en aanmeldingen beheren](sql-database-manage-logins.md).

## <a name="how-do-i-make-sure-only-authorized-ip-addresses-are-allowed-to-access-a-server"></a>Hoe maak ik ervoor dat alleen geautoriseerde IP-adressen zijn toegestaan voor toegang tot een server?
Zie [procedure: firewall-instellingen configureren op de SQL-Database](sql-database-configure-firewall-settings.md).

## <a name="how-does-the-usage-of-sql-database-show-up-on-my-bill"></a>Hoe wordt het gebruik van SQL Database weergegeven op mijn factuur?
Facturen SQL-Database op een voorspelbare per uur tarief op basis van de servicetier + prestatieniveau voor individuele databases of edtu's per elastische pool. Werkelijke gebruik wordt berekend en pro rato per uur uitgevoerd, zodat uw factuur mogelijk breuken van een uur weer. Als een database voor 12 uur in een maand bestaat, ziet uw factuur u bijvoorbeeld informatie over het gebruik van 0,5 dagen. Servicelagen + prestatieniveau en edtu's per groep verder worden onderverdeeld in de factuur gemakkelijker om te zien van het aantal databasedagen die u voor elk in één maand gebruikt.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Wat gebeurt er als een individuele database minder dan een uur actief is of gebruikmaakt van een hogere servicelaag minder dan een uur?
U wordt gefactureerd voor elk uur een database bestaat met de hoogste servicelaag + prestatieniveau die toegepast tijdens dat uur, ongeacht de informatie over het gebruik of of de database minder dan een uur actief was. Als u een individuele database maken en verwijderen van vijf minuten later duidt uw factuur kosten met zich mee voor één database uur. 

Voorbeelden

* Als u een Basic-database maken en vervolgens onmiddellijk naar de Standard-S1 upgraden, u in rekening worden gebracht met de Standard-S1-frequentie voor het eerste uur.
* Als u een database vanuit Basic naar Premium om 10:00 uur bijwerkt en de upgrade is voltooid om 1:35 uur op de volgende dag u in rekening worden gebracht met de Premium-snelheid begint bij 1:00 uur 
* Als u een database van Premium naar Basic gedowngraded om 11:00 uur en 2:15 uur is voltooid, wordt de database wordt in rekening gebracht Premium tot en met 3:00 uur, waarna deze wordt in rekening gebracht volgens de tarieven voor het Basic.

## <a name="how-does-elastic-pool-usage-show-up-on-my-bill-and-what-happens-when-i-change-edtus-per-pool"></a>Hoe wordt gebruik elastische groep van toepassingen weergegeven op mijn factuur- en wat gebeurt er wanneer ik wijzigen edtu's per groep?
Elastische pool kosten weergeven van op uw factuur als elastische dtu's (edtu's) in de stappen die wordt weergegeven onder Edtu per pool op [de pagina met prijzen](https://azure.microsoft.com/pricing/details/sql-database/). Er zijn geen kosten per database voor elastische pools. U wordt gefactureerd voor elk uur die een adresgroep bestaat op het hoogste aantal edtu's, ongeacht de informatie over het gebruik of of de groep voor minder dan een uur actief was. 

Voorbeelden

* Als u een Standard elastische pool met 200 edtu's om 11:18 uur maken, vijf databases toevoegen aan de groep, u in rekening worden gebracht 200 edtu's voor het hele uur beginnen bij 11: 00 in de rest van de dag.
* Op dag 2, 5:05 uur Database 1 begint verbruikt 50 edtu's en constante via de dag bevat. Databases 2-5 fluctueren tussen 0 en 80 edtu's. Tijdens de dag, kunt u vijf andere databases die verschillende edtu's gedurende de dag verbruiken toevoegen. Dag 2 is een volledige dag tegen 200 eDTU in rekening gebracht. 
* Op dag 3, 5 uur u toevoegen een andere 15 databases. Databasegebruik verhoogt gedurende de dag naar het punt waar u besluit om te verhogen edtu's voor de toepassingen van 200 400 20:05 uur Kosten op het niveau van 200 eDTU waren van kracht tot 8 uur en verhoogt naar 400 edtu's voor de resterende vier uur. 

## <a name="elastic-pool-billing-and-pricing-information"></a>Elastische pool facturering en prijzen van informatie
Elastische pools worden gefactureerd per de volgende kenmerken:

* Een elastische pool wordt in rekening gebracht bij het maken ervan, zelfs wanneer er geen databases in de groep zijn.
* Een elastische pool wordt in rekening gebracht per uur. Dit is dezelfde softwarelicentiecontrole frequentie als voor prestatieniveaus van individuele databases.
* Als de grootte van een elastische groep is gewijzigd naar een nieuwe aantal edtu's, klikt u vervolgens de groep wordt niet in rekening gebracht volgens de nieuwe hoeveelheid edtu's tot het wijzigen van de grootte is voltooid. Dit volgt hetzelfde patroon als het wijzigen van het prestatieniveau van individuele databases.
* De prijs van een elastische groep is gebaseerd op het aantal edtu's van de pool. De prijs van een elastische groep is afhankelijk van het aantal en het gebruik van de elastische databases binnen deze.
* Prijs wordt berekend door de (aantal groepen met edtu's) x (eenheidsprijs per eDTU).

De prijs per eenheid eDTU voor een elastische groep is hoger dan de eenheidsprijs DTU voor één database in de servicetier hetzelfde. Zie [de prijsinformatie voor SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) voor meer informatie. 

Zie voor informatie over de lagen edtu's en -service, [SQL Database-opties en prestaties](sql-database-service-tiers.md).

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-pool-show-up-on-my-bill"></a>Hoe gaat het gebruik van actieve geo-replicatie in een elastische pool te zien op mijn factuur?
In tegenstelling tot individuele databases, met behulp van [actieve geo-replicatie](sql-database-geo-replication-overview.md) met elastische databases niet direct facturering gevolgen hebben.  U alleen worden in rekening gebracht voor de edtu's ingericht voor elk van de groepen (groep primaire en secundaire groep)

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Hoe is de auditingfunctie van invloed op mijn factuur?
Controle is ingebouwd in de service SQL Database zonder extra kosten en is beschikbaar voor Basic, Standard, Premium en RS Premium-databases. Echter, voor het opslaan van de controlelogboeken controle functie gebruikt die een Azure Storage-account en de tarieven voor tabellen en wachtrijen in Azure Storage-toepassing op basis van de grootte van het controlelogboek.

## <a name="how-do-i-find-the-right-service-tier-and-performance-level-for-single-databases-and-elastic-pools"></a>Hoe vind ik het juiste prijscategorie en prestatieniveau serviceniveau voor individuele databases en elastische pools
Er zijn een aantal hulpprogramma's voor u beschikbaar. 

* Lokale-database, gebruikt u de [DTU sizing advisor](http://dtucalculator.azurewebsites.net/) te raden de databases en het aantal dtu's vereist, en te evalueren meerdere databases voor elastische pools.
* Als een individuele database zou als voordeel dat in een pool, raadt intelligent engine van Azure een elastische pool als er een historisch gebruikspatroon dat ontstane. Zie [bewaken en beheren van een elastische groep met de Azure-portal](sql-database-elastic-pool-manage-portal.md). Zie voor meer informatie over hoe u de berekening zelf doet [prijs- en Prestatieoverwegingen voor een elastische pool](sql-database-elastic-pool.md)
* Zie voor of u moet een individuele database kiezen omhoog of omlaag [prestaties richtlijnen voor individuele databases](sql-database-performance-guidance.md).

## <a name="how-often-can-i-change-the-service-tier-or-performance-level-of-a-single-database"></a>Hoe vaak kan ik het serviceniveau voor laag of de prestaties van één database wijzigen?
U kunt de servicetier (tussen Basic, Standard, Premium en Premium RS) of het prestatieniveau binnen een service tier (bijvoorbeeld S1 aan S2) zo vaak als u wilt wijzigen. Voor de databases uit eerdere versies, kunt u het niveau van tier of prestaties service een totaal van vier keer in een periode van 24 uur.

## <a name="how-often-can-i-adjust-the-edtus-per-pool"></a>Hoe vaak kan ik de edtu's per groep aanpassen?
Zo vaak als u wilt.

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-pool"></a>Hoe lang duurt het wijzigen van het serviceniveau voor laag of de prestaties van één database of verplaatsen van een database in een elastische groep?
De servicelaag van een database wijzigen en verplaatsen van en naar een groep moet de database op het platform als een achtergrondbewerking worden gekopieerd. De servicelaag wijzigen kan enkele minuten tot enkele uren duren, afhankelijk van de grootte van de databases. In beide gevallen worden in de databases online en beschikbaar blijven tijdens het verplaatsen. Zie voor meer informatie over het wijzigen van individuele databases [wijzigen van de servicelaag van een database](sql-database-service-tiers.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Wanneer moet ik een individuele database versus elastische databases gebruiken?
In het algemeen elastische pools zijn ontworpen voor een typische [patroon software as a service (SaaS)-toepassing](sql-database-design-patterns-multi-tenancy-saas-applications.md), waarbij één database per klant of tenant. Kopen van afzonderlijke databases en overmatige inrichting om te voorzien in de variabele vraag en de piekvraag voor elke database, is vaak zeer onvoordelig. Beheren van de collectieve prestaties van de groep met toepassingen, en de databases omhoog en omlaag geschaald automatisch. Azure intelligent engine raadt aan om een pool voor databases wanneer een gebruikspatroon ontstane. Zie voor meer informatie [elastische pool richtlijnen](sql-database-elastic-pool.md).

## <a name="what-does-it-mean-to-have-up-to-200-of-your-maximum-provisioned-database-storage-for-backup-storage"></a>Wat betekent het dat ik maximaal 200% van mijn maximale ingerichte databaseopslag heb voor back-upopslag?
Back-upopslag is de opslag die is gekoppeld aan uw geautomatiseerde database back-ups die worden gebruikt voor [Point-In-Time-herstel](sql-database-recovery-using-backups.md#point-in-time-restore) en [geo-restore](sql-database-recovery-using-backups.md#geo-restore). Microsoft Azure SQL Database biedt maximaal 200% van uw maximale ingerichte databaseopslag of back-upopslag zonder extra kosten. Bijvoorbeeld, als er een Standard database-exemplaar met een ingerichte DB-grootte van 250 GB, worden u voorzien van 500 GB aan back-upopslag zonder extra kosten. Als uw database groter is dan de opgegeven back-upopslag, kunt u de bewaarperiode contact opnemen met de ondersteuning van Azure te verkleinen of betalen voor de aanvullende back-upopslag op standaard-geografisch redundante opslag met leestoegang (RA-GRS) tarief gefactureerd. Zie voor meer informatie over de facturering RA-GRS prijsinformatie voor opslag.

## <a name="im-moving-from-webbusiness-to-the-new-service-tiers-what-do-i-need-to-know"></a>Ik ben van Web/Business verplaatst naar de nieuwe Servicelagen, wat moet ik te weten?
Azure SQL-Web- en zakelijke databases zijn nu buiten gebruik gesteld. De lagen Basic, Standard, Premium, Premium RS en elastische vervangen de retiring Web en Business-databases. 

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Wat is er een vertraging van het verwachte replicatie geo-replicatie een database tussen twee regio's binnen de dezelfde Azure Geografie?
Er zijn momenteel ondersteunt een RPO van vijf seconden en de vertraging van replicatie is kleiner dan dat wanneer de secundaire geo-database wordt gehost in Azure gekoppelde regio aanbevolen en op de dezelfde servicelaag.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Wat is er een vertraging van de verwachte replicatie als secundaire geo-database wordt gemaakt in dezelfde regio bevinden als de primaire database?
Op basis van empirische gegevens, is er niet te veel verschil tussen regio intra- en vertraging tussen regio replicatie wanneer het Azure aanbevolen gekoppelde regio wordt gebruikt. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Als er een netwerkfout tussen twee regio's, hoe de Pogingslogica werkt wanneer geo-replicatie is ingesteld?
Als er een verbinding verbreken, wordt opnieuw geprobeerd aan elke 10 seconden om verbindingen opnieuw tot stand te brengen.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Wat kan ik doen om ervoor te zorgen dat een belangrijke wijziging in de primaire database wordt gerepliceerd?
De geo-secundaire is een replica async en we niet proberen om het volledige synchroniseren met de primaire. Maar bieden we een methode voor het afdwingen van synchronisatie om te controleren of de replicatie van belangrijke wijzigingen (bijvoorbeeld bijwerken van wachtwoorden). Geforceerde synchronisatie van invloed op prestaties omdat de aanroepende thread worden geblokkeerd totdat alle doorgevoerde transacties worden gerepliceerd. Zie voor meer informatie [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Welke hulpprogramma's beschikbaar zijn voor de vertraging van replicatie tussen de primaire database en de secundaire geo-database controleren?
We tonen de vertraging realtime replicatie tussen de primaire database en de geo-secundaire via een DMV. Zie voor meer informatie [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).

## <a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Een database verplaatsen naar een andere server in hetzelfde abonnement
* In de [Azure-portal](https://portal.azure.com), klikt u op **SQL-databases**, selecteert u een database in de lijst en klik vervolgens op **kopie**. Zie [kopiëren van een Azure SQL database](sql-database-copy.md) voor meer informatie.

## <a name="to-move-a-database-between-subscriptions"></a>Een database verplaatsen tussen abonnementen
* In de [Azure-portal](https://portal.azure.com), klikt u op **SQL-servers** en selecteer vervolgens de server die als host fungeert voor uw database in de lijst. Klik op **verplaatsen**, en selecteer vervolgens de resources te verplaatsen en het abonnement om naar te verplaatsen.
