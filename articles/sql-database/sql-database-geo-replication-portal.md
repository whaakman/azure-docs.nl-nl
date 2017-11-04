---
title: 'Azure-portal: SQL-Database geo-replicatie | Microsoft Docs'
description: "Geo-replicatie configureren voor Azure SQL Database in de Azure-portal en de initiëren failover"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: d0b29822-714f-4633-a5ab-fb1a09d43ced
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 03/06/2016
ms.author: carlrab
ms.openlocfilehash: d880a2f1aef3fa1bfc824f14650088bac4cb8693
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-the-azure-portal-and-initiate-failover"></a>Actieve geo-replicatie configureren voor Azure SQL Database in de Azure-portal en de initiëren failover

Dit artikel ziet u het actieve geo-replicatie configureren voor SQL-Database in de [Azure-portal](http://portal.azure.com) en het starten van failover.

Zie voor het initiëren van failover met de Azure-portal [initiëren van een geplande of niet-geplande failover voor Azure SQL Database met de Azure portal](sql-database-geo-replication-portal.md).

Actieve geo-replicatie configureren met behulp van de Azure-portal, moet u de volgende bronnen:

* Een Azure SQL database: de primaire database die u wilt repliceren naar een andere geografische regio.

> [!Note]
Actieve geo-replicatie moet liggen tussen databases in hetzelfde abonnement.

## <a name="add-a-secondary-database"></a>Toevoegen van een secundaire database
De volgende stappen wordt een nieuwe secundaire database maken in een associatie geo-replicatie.  

Als u wilt toevoegen van een secundaire database, moet u de eigenaar van abonnement of mede-eigenaar.

De secundaire database heeft dezelfde naam als de primaire database en is standaard hetzelfde. De secundaire database is een individuele database of een database in een elastische pool. Zie voor meer informatie [Servicelagen](sql-database-service-tiers.md).
Nadat de secundaire is gemaakt en wordt gemaakt, begint de gegevens van de primaire database te repliceren naar de nieuwe secundaire database.

> [!NOTE]
> Als de partner-database (bijvoorbeeld als gevolg van een vorige geo-replicatie relatie beëindigd) bestaat, mislukt de opdracht.
> 

1. In de [Azure-portal](http://portal.azure.com), blader naar de database die u wilt instellen voor de geo-replicatie.
2. Selecteer op de pagina SQL-database **geo-replicatie**, en selecteer vervolgens de regio waarin u wilt maken van de secundaire database. U kunt een willekeurige regio selecteren dan de regio die als host fungeert voor de primaire database, maar het is raadzaam de [gekoppelde regio](../best-practices-availability-paired-regions.md).
   
    ![Geo-replicatie configureren](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. Selecteer of de server en de prijscategorie voor de secundaire database configureren.
   
    ![Secundaire configureren](./media/sql-database-geo-replication-portal/create-secondary.png)
4. U kunt desgewenst een secundaire database toevoegen aan een elastische pool. Klik op om de secundaire database in een pool **elastische pool** en selecteer een groep op de doelserver. Een groep moet al bestaan op de doelserver. Deze werkstroom wordt niet gemaakt voor een groep.
5. Klik op **maken** om toe te voegen van de secundaire.
6. De secundaire database wordt gemaakt en het seeding wordt gestart.
   
    ![Secundaire configureren](./media/sql-database-geo-replication-portal/seeding0.png)
7. Wanneer de seeding proces voltooid is, wordt de status weergegeven in de secundaire database.
   
    ![Seeding voltooid](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a>Initieer een failover

De bewerking van de secundaire database kan worden uitgevoerd als de primaire.  

1. In de [Azure-portal](http://portal.azure.com), blader naar de primaire database in de samenwerking geo-replicatie.
2. Selecteer op de blade SQL Database **alle instellingen** > **geo-replicatie**.
3. In de **SECUNDAIREN** , selecteert u de database die u wilt worden van de nieuwe primaire en klikt u op **Failover**.
   
    ![failover](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. Klik op **Ja** om te beginnen met de failover.

De opdracht switches onmiddellijk de secundaire database aan de primaire rol. 

Er is een korte periode gedurende welke beide databases niet beschikbaar (orde van grootte van 0 tot en met 25 seconden zijn) terwijl de rollen worden omgeschakeld. Als de primaire database meerdere secundaire databases heeft, configureert de andere secundaire replica's verbinding maken met de nieuwe primaire automatisch door de opdracht opnieuw. De hele bewerking duurt minder dan een minuut in beslag onder normale omstandigheden. 

> [!NOTE]
> Met deze opdracht is ontworpen voor snel herstel van de database bij een stroomstoring. Dit activeert failover zonder gegevenssynchronisatie (gedwongen failover).  Als de primaire online is en het doorvoeren van transacties wanneer er gegevens verloren gaan is uitgegeven door de opdracht kan zich voordoen. 
> 
> 

## <a name="remove-secondary-database"></a>Secundaire database verwijderen
Deze bewerking permanent beëindigt de replicatie naar de secundaire database en de rol van de secundaire verandert in een normale lezen / schrijven-database. Als de verbinding met de secundaire database verbroken is, wordt de opdracht is uitgevoerd maar wordt de secundaire biedt niet alleen-lezen nadat verbindingen worden wordt hersteld.  

1. In de [Azure-portal](http://portal.azure.com), blader naar de primaire database in de samenwerking geo-replicatie.
2. Selecteer op de pagina SQL-database **geo-replicatie**.
3. In de **SECUNDAIREN** , selecteert u de database die u wilt verwijderen uit de samenwerking geo-replicatie.
4. Klik op **Stop replicatie al**.
   
    ![Secundaire verwijderen](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. Er wordt een bevestigingsvenster geopend. Klik op **Ja** naar de database verwijderen uit de samenwerking geo-replicatie. (Deze instellen met een alleen-lezen database geen deel uit van replicatie.)

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over actieve geo-replicatie, [actieve geo-replicatie](sql-database-geo-replication-overview.md).
* Zie voor een overzicht van zakelijke continuïteit en scenario's [Business continuity overview](sql-database-business-continuity.md).

