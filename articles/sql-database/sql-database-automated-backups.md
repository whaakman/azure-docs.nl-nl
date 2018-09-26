---
title: Azure SQL-Database automatische, geografisch redundante back-ups | Microsoft Docs
description: SQL-Database maakt een back-up van de lokale database om de paar minuten en maakt gebruik van Azure geografisch redundante opslag met leestoegang voor geo-redundantie automatisch.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 09/25/2018
ms.openlocfilehash: 5c6ebfcb7eae52915af24fc67e9b3c774656149d
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181138"
---
# <a name="learn-about-automatic-sql-database-backups"></a>Meer informatie over automatische back-ups van SQL-Database

SQL Database automatisch maakt databaseback-ups en geografisch redundante opslag met Azure-leestoegang (RA-GRS) gebruikt om geo-redundantie te verlenen. Deze back-ups worden gemaakt automatisch en zonder extra kosten. U hoeft te doen zodat ze zich voordoen. Databaseback-ups zijn een essentieel onderdeel van een strategie voor zakelijke continuïteit en noodherstel omdat ze uw gegevens tegen per ongeluk beschadigd of verwijderd beschermen. Als uw beveiligingsregels is vereist dat uw back-ups beschikbaar voor een lange periode zijn, kunt u een beleid met een back-up langetermijnbewaarperiode kunt configureren. Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-sql-database-backup"></a>Wat is er een back-up van SQL Database?

SQL-Database maakt gebruik van SQL Server-technologie maken [volledige](https://msdn.microsoft.com/library/ms186289.aspx), [differentiële](https://docs.microsoft.com/sql/relational-databases/backup-restore/differential-backups-sql-server), en [transactielogboek](https://msdn.microsoft.com/library/ms191429.aspx) back-ups voor de doeleinden van Point in time restore (PITR). De transactielogboekback-ups meestal elke 5-10 minuten en plaatsvinden differentiële back-ups in het algemeen elke 12 uur, met de frequentie op basis van de rekencapaciteit en hoeveelheid activiteit in een database. Transactielogboekback-ups, met volledige en differentiële back-ups, kunnen u een database herstellen naar een specifieke point-in-time op dezelfde server die als host fungeert voor de database. De back-ups worden opgeslagen in de RA-GRS-opslag-blobs die worden gerepliceerd naar een [gekoppeld Datacenter](../best-practices-availability-paired-regions.md) voor bescherming tegen een storing in het datacenter. Wanneer u een database herstelt, wordt de service zoekt uit welke volledige, differentiële en transactie-logboek back-ups moeten worden teruggezet.


U kunt deze back-ups te gebruiken:

* Een database herstellen naar een punt-in-time binnen de bewaarperiode liggen. Met deze bewerking wordt een nieuwe database maken in dezelfde server als de oorspronkelijke database.
* Een verwijderde database herstellen naar de tijd die is verwijderd of elk gewenst moment binnen de bewaarperiode liggen. De verwijderde database kan alleen worden hersteld op dezelfde server waar de oorspronkelijke database is gemaakt.
* Een database herstellen naar een andere geografische regio. Hiermee kunt u om te herstellen na een noodgeval geografische wanneer u geen toegang uw server en database tot. Het maakt een nieuwe database in een bestaande server overal ter wereld. 
* Een database van een specifieke langetermijnback-up herstellen als de database is geconfigureerd met een beleid met een langetermijnbewaarperiode (LTR). Hiermee kunt u een oude versie van de database om te voldoen aan een aanvraag voor naleving of uit te voeren van een oude versie van de toepassing herstellen. Zie [langetermijnretentie](sql-database-long-term-retention.md).
* Als u wilt een herstelbewerking uitvoert, Zie [-database herstellen vanuit back-ups](sql-database-recovery-using-backups.md).

> [!NOTE]
> In de Azure-opslag, de term *replicatie* verwijst naar het kopiëren van bestanden van de ene locatie naar een andere. De SQL *databasereplicatie* verwijst naar meerdere secundaire databases met een primaire database gesynchroniseerd te houden. 
> 

## <a name="how-long-are-backups-kept"></a>Hoe lang worden back-ups opgeslagen?
Elke SQL-Database back-up is de bewaarperiode voor een standaard die is gebaseerd op de servicelaag van de database en een verschil oplevert tussen de [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) en de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md). U kunt de bewaarperiode voor back-up voor een database bijwerken. Zie [bewaarperiode voor back-up wijzigen](#how-to-change-backup-retention-period) voor meer informatie.

Als u een database verwijdert, blijven SQL-Database de back-ups op dezelfde manier als zou voor een online-database. Als u een Basic-database met een bewaarperiode van zeven dagen verwijdert, wordt bijvoorbeeld een back-up die vier dagen oud is drie dagen opgeslagen.

Als u de back-ups langer dan de maximale bewaarperiode van PITR houden wilt, kunt u de back-eigenschappen voor het toevoegen van een of meer langdurige bewaarperioden met uw database wijzigen. Zie [langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.

> [!IMPORTANT]
> Als u de Azure SQL-server die als host fungeert voor SQL-databases verwijdert, worden alle elastische pools en databases die deel uitmaken van de server worden ook verwijderd en kunnen niet worden hersteld. U kunt een server verwijderd niet herstellen. Maar als u met een langetermijnbewaarperiode hebt geconfigureerd, wordt de back-ups voor de databases met LTR worden niet verwijderd en wordt deze databases kunnen worden hersteld.

### <a name="pitr-retention-period"></a>De bewaarperiode PITR
De bewaartermijn voor een database gemaakt met behulp van het op DTU gebaseerde aankoopmodel, is afhankelijk van de service tier:

* Basis-servicelaag is 1 week.
* Standaardservicelaag is vijf weken.
* Premium-servicelaag is vijf weken.

Als u de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md), de bewaarperiode voor back-ups is configureerbaar tot 35 dagen. 

Als u de huidige PITR-bewaarperiode verkorten, wordt alle bestaande back-ups ouder is dan de nieuwe bewaarperiode niet meer beschikbaar zijn. 

Als u de huidige PITR bewaarperiode verhoogt, wordt SQL Database de bestaande back-ups behouden totdat de langere bewaarperiode is bereikt.

## <a name="how-often-do-backups-happen"></a>Hoe vaak gebeurt er back-ups?
### <a name="backups-for-point-in-time-restore"></a>Back-ups voor point-in-time restore
SQL Database biedt ondersteuning voor self-service voor point-in-time restore (PITR) door het automatisch maken van volledige back-up, differentiële back-ups en transactielogboekback-ups. Volledige databaseback-ups wekelijks worden gemaakt, differentiële databaseback-ups worden in het algemeen gemaakt voor elke 12 uur en transactielogboekback-ups worden meestal elke 5-10 minuten, gemaakt met de frequentie op basis van de rekencapaciteit en hoeveelheid activiteit in een database. De eerste volledige back-up is gepland, onmiddellijk nadat een database wordt gemaakt. Deze meestal binnen 30 minuten is voltooid, maar het kan langer duren als de database van een aanzienlijke grootte is. Bijvoorbeeld, de eerste back-up kan dit langer duren voor een herstelde database of een databasekopie. Na de eerste volledige back-up, worden alle verdere back-ups automatisch gepland en beheerd op de achtergrond op de achtergrond. De precieze timing van alle databaseback-ups wordt bepaald door de service SQL Database, zoals het geeft een balans de werkbelasting van het algehele systeem tussen.

De PITR back-ups zijn geografisch redundante en beveiligd door [Azure Storage-overschrijdend-replicatie](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage)

Zie voor meer informatie, [Point-in-time restore](sql-database-recovery-using-backups.md#point-in-time-restore)

### <a name="backups-for-long-term-retention"></a>Back-ups voor langetermijnretentie
SQL Database biedt de mogelijkheid van het configureren van (LTR) met een langetermijnbewaarperiode van volledige back-ups voor maximaal tien jaar in Azure blob-opslag. Als LTR-beleid is ingeschakeld, worden de wekelijkse volledige back-ups automatisch gekopieerd naar een andere container voor RA-GRS-opslag. Om te voldoen aan verschillende nalevingsvereiste, kunt u verschillende bewaartermijnen voor wekelijkse, maandelijkse en/of jaarlijkse back-ups. Het opslagverbruik, is afhankelijk van de geselecteerde frequentie van back-ups en de retentie-periode. U kunt de [LTR-prijscalculator](https://azure.microsoft.com/pricing/calculator/?service=sql-database) om in te schatten van de kosten voor LTR-opslag. 

Zoals PITR, zijn de LTR back-ups geografisch redundante en beveiligd door [Azure Storage-overschrijdend-replicatie](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage).

Zie voor meer informatie, [langetermijnretentie](sql-database-long-term-retention.md).

## <a name="are-backups-encrypted"></a>Back-ups versleuteld?

Als uw database is versleuteld met TDE, wordt de back-ups worden automatisch versleuteld in rust, met inbegrip van LTR-back-ups. Als TDE is ingeschakeld voor een Azure SQL database, worden back-ups zijn eveneens versleuteld. Alle nieuwe Azure SQL-databases zijn geconfigureerd met TDE standaard ingeschakeld. Zie voor meer informatie over TDE [Transparent Data Encryption met Azure SQL Database](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="how-does-microsoft-ensure-backup-integrity"></a>Hoe garandeert Microsoft dat de integriteit van de back-up

Regelmatig test het technische team van Azure SQL Database automatisch het terugzetten van geautomatiseerde databaseback-ups van databases in de service. Bij het herstellen, databases, ontvangen ook integriteitscontroles met DBCC CHECKDB. Problemen met gevonden tijdens de integriteitscontrole resulteert in een waarschuwing aan het technische team. Zie voor meer informatie over de integriteit van gegevens in Azure SQL Database, [integriteit van gegevens in Azure SQL Database](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/).

## <a name="how-do-automated-backups-impact-my-compliance"></a>Hoe geautomatiseerde back-ups van invloed zijn op mijn naleving?

Wanneer u uw database van een servicelaag op basis van DTU, met de standaard PITR bewaarperiode van 35 dagen, naar een laag vCore-gebaseerde service migreert, wordt de bewaarperiode PITR behouden om ervoor te zorgen dat beleid voor gegevensherstel van uw toepassing niet worden gecompromitteerd. Als de bewaarperiode standaard niet voldoet aan uw vereisten voor naleving, kunt u de bewaarperiode voor PITR met PowerShell of REST-API wijzigen. Zie [bewaarperiode voor back-up wijzigen](#how-to-change-backup-retention-period) voor meer informatie.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="how-to-change-backup-retention-period"></a>Het wijzigen van de bewaarperiode voor back-up
U kunt de standaard-bewaarperiode met REST API of PowerShell wijzigen. De ondersteunde waarden zijn: 7, 14, 21, 28 of 35 dagen. De volgende voorbeelden laten zien hoe u PITR retentie wijzigen op 28 dagen. 

> [!NOTE]
> Thes APIs is alleen van invloed op de bewaarperiode PITR. Als u LTR voor uw database hebt geconfigureerd, zullen niet worden beïnvloed. Zie [langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie over het wijzigen van de LTR retentie-periode.

### <a name="change-pitr-backup-retention-period-using-powershell"></a>Wijzigen van de back-up bewaarperiode PITR met behulp van PowerShell
```powershell
Set-AzureRmSqlDatabaseBackupShortTermRetentionPolicy -ResourceGroupName resourceGroup -ServerName testserver -DatabaseName testDatabase -RetentionDays 28
```
> [!IMPORTANT]
> Deze API is opgenomen in de PowerShell-Module AzureRM.Sql vanaf versie [4.7.0-preview](https://www.powershellgallery.com/packages/AzureRM.Sql/4.7.0-preview). 

### <a name="change-pitr-retention-period-using-rest-api"></a>Wijzigen van de bewaarperiode PITR met behulp van REST-API
**Voorbeeld van een aanvraag**
```http
PUT https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/resourceGroup/providers/Microsoft.Sql/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default?api-version=2017-10-01-preview
```
**Aanvraagtekst**
```json
{
  "properties":{  
      "retentionDays":28
   }
}
```
**Voorbeeldantwoord**

Statuscode: 200
```json
{
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/providers/Microsoft.Sql/resourceGroups/resourceGroup/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default",
  "name": "default",
  "type": "Microsoft.Sql/resourceGroups/servers/databases/backupShortTermRetentionPolicies",
  "properties": {
    "retentionDays": 28
  }
}
```
Zie [back-up bewaren REST-API](https://docs.microsoft.com/rest/api/sql/backupshorttermretentionpolicies) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Databaseback-ups zijn een essentieel onderdeel van een strategie voor zakelijke continuïteit en noodherstel omdat ze uw gegevens tegen per ongeluk beschadigd of verwijderd beschermen. Zie voor meer informatie over de andere Azure SQL Database oplossingen voor bedrijfscontinuïteit, [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md).
- Als u wilt herstellen naar een eerder tijdstip met behulp van de Azure-portal, Zie [database herstellen naar een eerder tijdstip met behulp van de Azure-portal](sql-database-recovery-using-backups.md).
- Als u wilt herstellen naar een eerder tijdstip met behulp van PowerShell, Zie [database herstellen naar een eerder tijdstip met behulp van PowerShell](scripts/sql-database-restore-database-powershell.md).
- Om te configureren, beheren en herstellen vanuit een langetermijnretentie van automatische back-ups in Azure blob-opslag met de Azure-portal, Zie [beheren met behulp van de Azure-portal langetermijnretentie](sql-database-long-term-backup-retention-configure.md).
- Om te configureren, beheren en herstellen vanuit een langetermijnretentie van automatische back-ups in Azure BLOB-opslag met behulp van PowerShell, raadpleegt u [beheren met behulp van PowerShell langetermijnretentie](sql-database-long-term-backup-retention-configure.md).
