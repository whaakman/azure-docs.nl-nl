---
title: Herstellen van een Azure datawarehouse - lokale en geografisch redundante | Microsoft Docs
description: Overzicht van de database-opties voor terugzetten voor het herstellen van een database in Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 3e01c65c-6708-4fd7-82f5-4e1b5f61d304
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 12/06/2017
ms.author: barbkess
ms.openlocfilehash: abf8f0b1005aec71812dc8ebfd12fe65250d7a0e
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="sql-data-warehouse-restore"></a>SQL Data Warehouse terugzetten
> [!div class="op_single_selector"]
> * [Overzicht][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

SQL Data Warehouse biedt zowel lokale als geografische herstelacties als onderdeel van de datawarehouse disaster recovery. Back-ups van gegevens datawarehouse gebruiken om te zetten van uw datawarehouse naar een herstelpunt in de primaire regio of geografisch redundante back-ups gebruiken om te zetten naar een ander geografische regio. Dit artikel wordt uitgelegd dat de details van het terugzetten van een datawarehouse.

## <a name="what-is-a-data-warehouse-restore"></a>Wat is er een datawarehouse te herstellen?
Een datawarehouse-herstellen van gegevens is een nieuw datawarehouse is gemaakt van een back-up van een bestaande of verwijderde datawarehouse. De herstelde datawarehouse wordt opnieuw gemaakt voor het datawarehouse van een back-up op een bepaald tijdstip. Omdat SQL Data Warehouse een gedistribueerd systeem is, wordt een datawarehouse gegevens terugzetten vanuit veel back-upbestanden die zijn opgeslagen in Azure BLOB's gemaakt. 

Database terugzetten is een essentieel onderdeel van een zakelijke continuïteit en noodherstel herstelstrategie omdat uw gegevens opnieuw worden gemaakt na het per ongeluk beschadigd of verwijderd.

Zie voor meer informatie:

* [Back-ups van SQL Data Warehouse](sql-data-warehouse-backups.md)
* [Overzicht van zakelijke continuïteit](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a>Datawarehouse-herstelpunten
Als een voordeel van het gebruik van Azure Premium-opslag wordt in SQL Data Warehouse momentopnamen van Azure Storage-Blob gebruikt om back-up van de primaire datawarehouse. Elke momentopname is een herstelpunt dat geeft de tijd dat de momentopname gestart. Voor het herstellen van een datawarehouse, kies een herstelpunt en voert u een restore-opdracht.  

SQL Data Warehouse herstelt altijd de back-up naar een nieuwe datawarehouse. U kunt het herstelde datawarehouse en de huidige houden of verwijder een van deze. Als u het huidige datawarehouse vervangt door de teruggezette datawarehouse wilt, kunt u deze kunt wijzigen.

Als u herstellen van een datawarehouse verwijderd of is onderbroken wilt, kunt u [Maak een ondersteuningsticket](sql-data-warehouse-get-started-create-support-ticket.md). 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore the last available restore point.

Yes, for the next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps the data warehouse and its snapshots for seven days just in case you need the data. After seven days, you won't be able to restore to any of the restore points. -->

## <a name="geo-redundant-restore"></a>Geografisch redundante terugzetten
U kunt uw datawarehouse terugzetten naar een willekeurige regio Azure SQL Data Warehouse op het niveau van uw gekozen prestatie ondersteunende. Houd er rekening mee dat 9000 en 18000 DWU worden niet ondersteund in alle regio's tijdens de preview.

> [!NOTE]
> Als u wilt een geografisch redundante herstel uitvoeren moet u niet hebt opgegeven buiten deze functie.
> 
> 

## <a name="restore-timeline"></a>Tijdlijn herstellen
U kunt een database herstellen naar elk beschikbaar herstelpunt binnen de afgelopen zeven dagen. Momentopnamen elke vier tot acht uur start en zijn beschikbaar voor de zeven dagen. Wanneer een momentopname ouder dan zeven dagen is, het verloopt en het herstelpunt is niet meer beschikbaar.

## <a name="restore-costs"></a>Kosten herstellen
De kosten voor opslag voor de herstelde datawarehouse wordt gefactureerd op de snelheid van Azure Premium-opslag. 

Als u een herstelde datawarehouse onderbreekt, wordt u in rekening gebracht voor opslag op de snelheid van Azure Premium-opslag. Het voordeel van onderbreken is dat u niet weet in rekening gebracht voor de computerbronnen van DWU.

Zie voor meer informatie over prijzen van SQL Data Warehouse [prijzen van SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="uses-for-restore"></a>Gebruikt voor herstel
De primair gebruik van datawarehouse terugzetten is om gegevens te herstellen na onbedoeld gegevensverlies of beschadigd.

U kunt ook gegevens datawarehouse terugzetten gebruiken voor het bewaren van een back-up langer dan zeven dagen. Wanneer de back-up is teruggezet, moet u het datawarehouse online zijn en kunt onderbreken voor onbepaalde tijd compute om kosten te besparen. De onderbroken database leidt ertoe dat de opslagkosten snelheid Azure Premium-opslag. 

## <a name="related-topics"></a>Verwante onderwerpen
### <a name="scenarios"></a>Scenario's
* Zie voor een overzicht zakelijke continuïteit [Business continuity-overzicht](../sql-database/sql-database-business-continuity.md)

<!-- ### Tasks -->

Datawarehouse herstelbewerking uit te voeren, als u wilt herstellen met behulp van:

* Azure portal, Zie [herstellen van een datawarehouse met de Azure portal](sql-data-warehouse-restore-database-portal.md)
* PowerShell-cmdlets raadpleegt [herstellen van een datawarehouse met behulp van PowerShell-cmdlets](sql-data-warehouse-restore-database-powershell.md)
* REST-API's, Zie [herstellen van een datawarehouse met de REST-API's](sql-data-warehouse-restore-database-rest-api.md)

<!-- ### Tutorials -->

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->
