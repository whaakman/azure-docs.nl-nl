---
title: 'Azure-portal: SQL-Database geo-replicatie | Microsoft Docs'
description: Geo-replicatie voor een enkele of gegroepeerde database in Azure SQL Database met behulp van de Azure-portal en initiëren failover configureren
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
ms.date: 12/03/2018
ms.openlocfilehash: 101b34cbd7ff869bd431cec0c60a189036cc2862
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52841826"
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-the-azure-portal-and-initiate-failover"></a>Actieve geo-replicatie configureren voor Azure SQL Database in Azure portal en failover initiëren

Dit artikel leest u hoe het configureren van [actieve geo-replicatie voor één en gepoolde databases](sql-database-geo-replication-overview.md#active-geo-replication-capabilities) in SQL-Database met behulp van de [Azure-portal](http://portal.azure.com) en het starten van failover.

Zie voor meer informatie over automatische failover-groepen met één en gepoolde databases [aanbevolen procedures van het gebruik van failover-groepen met één en gepoolde databases](sql-database-geo-replication-overview.md#best-practices-of-using-failover-groups-with-single-and-pooled-databases). Zie voor meer informatie over automatische failover-groepen met beheerde instanties (preview) [aanbevolen procedures van het gebruik van failover-groepen met één en gepoolde databases](sql-database-geo-replication-overview.md#best-practices-of-using-failover-groups-with-managed-instances).

## <a name="prerequisites"></a>Vereisten

Actieve geo-replicatie configureren met behulp van Azure portal, moet u de volgende bronnen:

* Een Azure SQL database: de primaire database die u wilt repliceren naar een andere geografische regio.

> [!Note]
Actieve geo-replicatie moet liggen tussen databases in hetzelfde abonnement.

## <a name="add-a-secondary-database"></a>Een secundaire database toevoegen

De volgende stappen maakt u een nieuwe secundaire database in een partnerschap geo-replicatie.  

Als u wilt toevoegen van een secundaire database, moet u de abonnement-eigenaar of mede-eigenaar zijn.

De secundaire database heeft dezelfde naam als de primaire database en dezelfde service-laag en grootte compute is standaard. De secundaire database kan zijn een individuele database of een database in een elastische pool. Zie voor meer informatie, [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) en [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md).
Nadat de secundaire server is gemaakt en seeding, begint de gegevens die van de primaire database worden gerepliceerd naar de nieuwe secundaire database.

> [!NOTE]
> Als de partnerdatabase al bestaat (bijvoorbeeld, als gevolg van een vorige geo-replicatierelatie beëindigen) mislukt de opdracht.

1. In de [Azure-portal](http://portal.azure.com), blader naar de database die u wilt instellen voor geo-replicatie.
2. Selecteer op de pagina SQL-database **geo-replicatie**, en selecteer vervolgens de regio voor het maken van de secundaire database. U kunt elke regio selecteren dan de regio die als host fungeert voor de primaire database, maar het is raadzaam de [gekoppelde regio](../best-practices-availability-paired-regions.md).

    ![Geo-replicatie configureren](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. Selecteer of configureer de server en de prijscategorie voor de secundaire database.

    ![Secundaire configureren](./media/sql-database-geo-replication-portal/create-secondary.png)
4. U kunt eventueel een secundaire database toevoegen aan een elastische pool. Klik op om de secundaire database in een pool **elastische pool** en selecteer een groep op de doelserver. Een pool moet al bestaan op de doelserver. Deze werkstroom worden niet gemaakt voor een groep.
5. Klik op **maken** om toe te voegen van de secundaire server.
6. De secundaire database wordt gemaakt en het seeden wordt gestart.

    ![Secundaire configureren](./media/sql-database-geo-replication-portal/seeding0.png)
7. Wanneer de seeding proces voltooid is, wordt de status ervan weergegeven in de secundaire database.

    ![Seeding voltooid](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a>Initieer een failover

De secundaire database kunt om te worden van de primaire worden overgeschakeld.  

1. In de [Azure-portal](http://portal.azure.com), blader naar de primaire database in de samenwerking met geo-replicatie.
2. Selecteer op de blade SQL Database **alle instellingen** > **geo-replicatie**.
3. In de **secundaire DATABASES** , selecteert u de database die u wilt worden van de nieuwe primaire en klikt u op **Failover**.

    ![Failover](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. Klik op **Ja** om te beginnen met de failover.

De opdracht wordt onmiddellijk de secundaire database in de primaire rol.

Er is een korte periode gedurende welke beide databases niet beschikbaar (orde van grootte seconden van 0 tot en met 25 zijn) terwijl de rollen zijn overgeschakeld. Als de primaire database meerdere secundaire databases heeft, configureert de andere secundaire replica's verbinding maken met de nieuwe primaire automatisch door de opdracht opnieuw. De gehele bewerking duurt minder dan een minuut in beslag onder normale omstandigheden.

> [!NOTE]
> Met deze opdracht is ontworpen voor snel herstel van de database in het geval van een storing. Dit activeert failover zonder synchronisatie van gegevens (geforceerde failover).  Als de primaire online is en vastleggen van transacties als gegevens verloren gaan door de opdracht is gegeven optreden.

## <a name="remove-secondary-database"></a>Secundaire database verwijderen

Deze bewerking permanent wordt de replicatie naar de secundaire database beëindigd en de rol van de secundaire server wordt gewijzigd in een normale lezen / schrijven-database. Als de verbinding met de secundaire database verbroken is, wordt de opdracht is geslaagd maar wordt de secundaire kiest, wordt er niet wordt lezen / schrijven pas na de verbinding wordt hersteld.  

1. In de [Azure-portal](http://portal.azure.com), blader naar de primaire database in de samenwerking met geo-replicatie.
2. Selecteer op de pagina SQL-database **geo-replicatie**.
3. In de **secundaire DATABASES** , selecteert u de database die u wilt verwijderen uit het partnerschap geo-replicatie.
4. Klik op **replicatie stoppen**.

    ![Secundaire verwijderen](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. Er wordt een bevestigingsvenster geopend. Klik op **Ja** naar de database verwijderen uit het partnerschap geo-replicatie. (Dit instellen op een alleen-lezen database geen deel uit van replicatie.)

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over actieve geo-replicatie, [actieve geo-replicatie](sql-database-geo-replication-overview.md).
* Zie voor een overzicht voor zakelijke continuïteit en scenario's, [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md).
