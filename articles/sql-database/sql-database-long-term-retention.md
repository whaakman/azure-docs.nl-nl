---
title: Azure SQL Database back-ups opslaan voor maximaal tien jaar | Microsoft Docs
description: Meer informatie over hoe Azure SQL Database opslaan back-ups ondersteunt maximaal tien jaar.
keywords: 
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 66fdb8b8-5903-4d3a-802e-af08d204566e
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/22/2016
ms.author: sashan
ms.openlocfilehash: 25e651203f804fbf32d632b5f83145a3f3f72a7f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="store-azure-sql-database-backups-for-up-to-10-years"></a>Azure SQL Database back-ups voor maximaal tien jaar opslaan
Veel toepassingen hebben regelgeving, compatibiliteit of andere zakelijke doeleinden die vereisen dat u voor het bewaren van back-ups buiten de 7 35 dagen geleverd door de Azure SQL Database [automatische back-ups](sql-database-automated-backups.md). Met behulp van de lange termijn bewaren van back-functie kunt u uw back-ups van SQL database opslaan in een Azure Recovery Services-kluis voor maximaal tien jaar. U kunt maximaal 1000 databases per kluis opslaan. Vervolgens kunt u back-up selecteren in de kluis om het te herstellen als een nieuwe database.

> [!IMPORTANT]
> Lange bewaartermijn van de back-up is momenteel in preview en beschikbaar zijn in de volgende gebieden: Australië-Oost, Australië-Zuidoost, Brazilië-Zuid, VS-midden, Oost-Azië, VS-Oost, VS-Oost 2, India centraal, India-Zuid, Japan-Oost, Japan-West, Noordelijk Centraal, VS, Noord Europa, Zuid-centraal VS, Zuidoost-Azië, West-Europa en VS-West.
>

> [!NOTE]
> U kunt maximaal 200 databases per kluis inschakelen tijdens een periode van 24 uur. Het is raadzaam dat u een afzonderlijke kluis voor elke server om te minimaliseren, de impact van deze limiet. 
> 

## <a name="how-sql-database-long-term-backup-retention-works"></a>De werking van de back-up op lange termijn bewaren van SQL-Database

Met bewaren van back-up op lange termijn, kunt u een SQL database-server koppelen aan een Azure Recovery Services-kluis. 

* In de dezelfde Azure-abonnement dat de SQL-server gemaakt en in dezelfde geografische regio en resourcegroep, moet u de kluis maken. 
* Configureert u een bewaarbeleid voor elke database. Het beleid zorgt ervoor dat de wekelijkse volledige back-ups moeten worden gekopieerd naar de Recovery Services-kluis en bewaard voor de opgegeven bewaarperiode (maximaal 10 jaar). 
* Vervolgens kunt u de database uit een van deze back-ups herstellen naar een nieuwe database in een willekeurige server in het abonnement. Azure-opslag maakt een kopie van de bestaande back-ups en het exemplaar heeft geen invloed van de prestaties op de bestaande database.

> [!TIP]
> Zie voor een uitleg handleiding [en herstel van back-up op lange termijn bewaren van Azure SQL Database configureren](sql-database-long-term-backup-retention-configure.md).

## <a name="enable-long-term-backup-retention"></a>Lange bewaartermijn van de back-up inschakelen

Lange bewaartermijn van back-up voor een database configureren:

1. Maak een Azure Recovery Services-kluis in dezelfde regio, abonnement en resourcegroep als uw SQL database-server. 
2. De server naar de kluis registreren.
3. Maak een beleid voor Azure Recovery Services-beveiliging.
4. Het beveiligingsbeleid van toepassing op de databases die langdurig bewaren van back-up vereisen.

Als u wilt configureren, beheren en een database herstellen via langdurig bewaren van back-up van automatische back-ups in een Azure Recovery Services-kluis, voert u een van de volgende opties:

* Met de Azure portal: klik op **lange bewaartermijn van de back-**, selecteert u een database en klik vervolgens op **configureren**. 

   ![Selecteer een database voor lange bewaartermijn van de back-up](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

* Met behulp van PowerShell: Ga naar [en herstel van back-up op lange termijn bewaren van Azure SQL Database configureren](sql-database-long-term-backup-retention-configure.md).

## <a name="restore-a-database-thats-stored-with-the-long-term-backup-retention-feature"></a>Een database die opgeslagen met de functie voor de lange termijn bewaren van back-up terugzetten

Herstellen vanuit back-up op lange termijn bewaren van back-up:

1. Lijst van de kluis waarin de back-up is opgeslagen.
2. Lijst van de container die is gekoppeld aan uw logische server.
3. Lijst van de gegevensbron in de kluis die is gekoppeld aan uw database.
4. Lijst van de herstelpunten die beschikbaar zijn voor het herstellen.
5. Herstel de database van het herstelpunt dat naar de doelserver in uw abonnement.

Als u wilt configureren, beheren en een database herstellen via langdurig bewaren van back-up van automatische back-ups in een Azure Recovery Services-kluis, voert u een van de volgende opties:

* Met de Azure portal: Ga naar [lange Backup-bewaartermijn met de Azure portal beheren](sql-database-long-term-backup-retention-configure.md). 

* Met behulp van PowerShell: Ga naar [lange Backup-bewaartermijn met behulp van PowerShell beheren](sql-database-long-term-backup-retention-configure.md).

## <a name="get-pricing-for-long-term-backup-retention"></a>Prijzen voor lange bewaartermijn van de back-ophalen

Lange bewaartermijn van back-up van een SQL-database wordt in rekening gebracht volgens de [Azure Backup-services prijzen tarieven](https://azure.microsoft.com/pricing/details/backup/).

Nadat de SQL database-server naar de kluis is geregistreerd, wordt u in rekening gebracht voor de totale opslag die wordt gebruikt door de wekelijkse back-ups zijn opgeslagen in de kluis.

## <a name="view-available-backups-that-are-stored-in-long-term-backup-retention"></a>Beschikbare back-ups weergeven die zijn opgeslagen in lange bewaartermijn van de back-up

Als u wilt configureren, beheren en een database herstellen via langdurig bewaren van back-up van automatische back-ups in een Azure Recovery Services-kluis met behulp van de Azure-portal, voert u een van de volgende opties:

* Met de Azure portal: Ga naar [lange Backup-bewaartermijn met de Azure portal beheren](sql-database-long-term-backup-retention-configure.md). 

* Met behulp van PowerShell: Ga naar [lange Backup-bewaartermijn met behulp van PowerShell beheren](sql-database-long-term-backup-retention-configure.md).

## <a name="disable-long-term-retention"></a>Lange bewaartermijn uitschakelen

De recovery-service wordt automatisch het opruimen van de back-ups op basis van het opgegeven bewaarbeleid verwerkt. 

Als u wilt stoppen met het verzenden van de back-ups voor een specifieke database naar de kluis, verwijder het bewaarbeleid voor die database.
  
```
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName 'RG1' -ServerName 'Server1' -DatabaseName 'DB1' -State 'Disabled' -ResourceId $policy.Id
```

> [!NOTE]
> De back-ups die zich al in de kluis worden hierdoor niet beïnvloed. Ze worden automatisch verwijderd door de herstelservice wanneer de bewaartermijn is verstreken.

## <a name="long-term-backup-retention-faq"></a>Lange bewaartermijn in back-Veelgestelde vragen

**Kan ik handmatig specifieke back-ups in de kluis verwijderen?**

Momenteel niet. Back-ups van ruimt de kluis automatisch wanneer de bewaartermijn is verlopen.

**Kan ik mijn server voor het opslaan van back-ups naar meer dan één kluis registreren?**

Nee, kunt u back-ups naar slechts één kluis momenteel opslaan op een tijdstip.

**Kan ik een kluis en de server hebben met verschillende abonnementen?**

Nee, de kluis en de server moeten zich in hetzelfde abonnement en dezelfde resourcegroep.

**Kan ik een kluis die ik in een regio die verschilt van mijn server regio gemaakt gebruiken?**

Nee, de kluis en de server, moet in dezelfde regio kopie tijd minimaliseren en verkeer kosten te voorkomen.

**Hoeveel databases kan ik opslaan in een kluis?**

Op dit moment kunnen ondersteuning we voor maximaal 1000 databases per kluis. 

**Het aantal kluizen kan ik maken per abonnement?**

U kunt maximaal 25 kluizen per abonnement maken.

**Hoeveel databases kan ik per dag per kluis configureren?**

U kunt 200 databases per dag per kluis instellen.

**Werkt langdurig bewaren van back-up met elastische pools?**

Ja. Elke database in de groep kan worden geconfigureerd met het bewaarbeleid.

**Kan ik een kiezen voor de tijd waarop de back-up is gemaakt?**

Nee, bepaalt de SQL-Database het back-upschema minimaliseren de invloed van de prestaties van uw databases.

**Ik heb transparante gegevensversleuteling is ingeschakeld voor de database. Kan ik deze met de kluis gebruiken?** 

Ja, transparante gegevensversleuteling wordt ondersteund. U kunt de database herstellen uit de kluis, zelfs als de oorspronkelijke database niet meer bestaat.

**Wat gebeurt er met de back-ups in de kluis als mijn abonnement wordt onderbroken?** 

Als uw abonnement wordt onderbroken, wordt de bestaande databases en de back-ups behouden. Nieuwe back-ups worden niet gekopieerd naar de kluis. Nadat u het abonnement opnieuw activeert, hervat de service back-ups kopiëren naar de kluis. Uw kluis wordt toegankelijk is voor de bewerkingen voor het herstellen met behulp van de back-ups die zijn gekopieerd er voordat u het abonnement is onderbroken. 

**Krijg ik toegang tot de back-upbestanden voor SQL-database zodat ik kan downloaden of deze naar de SQL-server terugzetten?**

Nee, momenteel niet.

**Is het mogelijk om meerdere planningen (dagelijks, wekelijks, maandelijks, jaarlijks) binnen een bewaarbeleid SQL.**

Nee, meerdere schema's zijn momenteel alleen beschikbaar voor back-ups van virtuele machine.

**Wat gebeurt er als we langdurig bewaren van back-up voor een database die is een secundaire actieve geo-replicatie-database hebt ingesteld?**

Omdat er geen back-ups op replica's onderneemt, is er momenteel geen optie voor het bewaren van de lange termijn back-up op secundaire databases. Het is echter belangrijk dat gebruikers langdurig bewaren van back-up op een secundaire actieve geo-replicatie-database instellen voor de volgende redenen:
* Wanneer een failover gebeurt en de database een primaire database wordt, duurt we een volledige back-up, die is geüpload naar de kluis.
* Er is geen extra kosten verbonden aan de klant voor het instellen van langdurig bewaren van back-up op een secundaire database.

## <a name="next-steps"></a>Volgende stappen
Omdat de databaseback-ups voorkomen dat gegevens per ongeluk beschadigd of verwijderd, zijn ze een essentieel onderdeel van een zakelijke continuïteit en noodherstelplan. Zie voor meer informatie over de andere SQL-Database-oplossingen voor bedrijfscontinuïteit, [Business continuity overview](sql-database-business-continuity.md).
