---
title: Lezen-replica's in Azure portal voor Azure Database voor PostgreSQL beheren
description: Dit artikel beschrijft beheren van Azure Database for PostgreSQL lezen-replica's in Azure portal.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 37150f67e29dae0357c978cfaea9abeebeef428c
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55691402"
---
# <a name="how-to-create-and-manage-read-replicas-in-the-azure-portal"></a>Over het maken en beheren lezen-replica's in Azure portal

> [!IMPORTANT]
> De functie lezen replica is beschikbaar als openbare preview.


In dit artikel leert u hoe u kunt maken en beheren van lezen-replica's in de Azure Database for PostgreSQL-service met behulp van de Azure portal. Voor meer informatie over meer replica's [Lees de documentatie van de concepten](concepts-read-replicas.md).

## <a name="prerequisites"></a>Vereisten
- Een [Azure Database for PostgreSQL-server](quickstart-create-server-database-portal.md) die de hoofd-server is.

## <a name="prepare-the-master-server"></a>De hoofd-server voorbereiden
Deze stap master voorbereiding geldt voor algemeen gebruik en geoptimaliseerd voor geheugen-servers.

De **azure.replication_support** parameter moet worden ingesteld op de REPLICA op de hoofd-server. Wijzigen van deze parameter vereist een herstart van de server moet zijn van kracht.

1. Selecteer de bestaande Azure Database for PostgreSQL-server die u wilt gebruiken als een model in de Azure-portal.

2. Selecteer **serverparameters** in het menu aan de linkerkant.

3. Zoeken naar **azure.replication_support**.

   ![Azure Database voor PostgreSQL - azure.replication_support](./media/howto-read-replicas-portal/azure-replication-parameter.png)

4. Set **azure.replication_support** to REPLICA. **Sla** de wijziging.

   ![Azure Database voor PostgreSQL - REPLICA en opslaan](./media/howto-read-replicas-portal/save-parameter-replica.png)

5. Zodra opslaan voltooid is, ontvangt u een melding.

   ![Azure Database voor PostgreSQL - opslaan melding](./media/howto-read-replicas-portal/parameter-save-notification.png)

6. Start opnieuw op de server om toe te passen van de wijziging nadat deze is opgeslagen. Zie [de documentatie van opnieuw opstarten](howto-restart-server-portal.md) voor meer informatie over hoe u een server opnieuw te starten.


## <a name="create-a-read-replica"></a>Maken van een replica lezen
Lezen-replica's kunnen worden gemaakt met behulp van de volgende stappen uit:
1.  Selecteer de bestaande Azure Database for PostgreSQL-server die u wilt gebruiken als een master. 

2.  Schakel replicatie in het menu onder instellingen.

   Als u dit nog niet hebt ingesteld **azure.replication_support** naar REPLICA in het algemeen gebruik of hoofd- en opnieuw gestart is geoptimaliseerd voor geheugen de server, ziet u een bericht weergegeven dat u om dit te doen. Dit doen voordat u doorgaat met de maken.

3.  Selecteer toevoegen Replica.

   ![Azure Database voor PostgreSQL - replica toevoegen](./media/howto-read-replicas-portal/add-replica.png)

4.  Voer een naam voor de replica-server en klik op OK om te bevestigen van het maken van de replica.

   ![Azure Database voor PostgreSQL - de naam van replica](./media/howto-read-replicas-portal/name-replica.png) 

> [!IMPORTANT]
> Lezen-replica's worden gemaakt met de configuratie van de dezelfde server als de master. Nadat een replica is gemaakt, de prijscategorie (met uitzondering van en naar Basic), bewerking voor compute, vCores, opslag en back-up bewaarperiode het kunnen onafhankelijk worden gewijzigd van de hoofd-server.

> [!IMPORTANT]
> Voordat de configuratie van de server van een model is bijgewerkt met nieuwe waarden, moet de configuratie van de replica's worden bijgewerkt naar waarden gelijk zijn aan of groter zijn. Er wordt geprobeerd om te doen anders wordt een fout veroorzaken. Dit zorgt ervoor dat de replica's kunnen blijven van wijzigingen in het model zijn. 


Nadat de replica-server is gemaakt, kan deze worden weergegeven in het venster van de replicatie.

![Azure Database voor PostgreSQL - de nieuwe replica](./media/howto-read-replicas-portal/list-replica.png)
 

## <a name="stop-replication"></a>Replicatie stoppen

> [!IMPORTANT]
> Replicatie naar een server stoppen is niet ongedaan worden gemaakt. Wanneer u replicatie tussen een model en de replica is gestopt, kunnen deze kan niet ongedaan worden gemaakt. De replica-server vervolgens wordt een zelfstandige server en biedt nu ondersteuning voor zowel lees- en schrijfbewerkingen. Deze server kan niet opnieuw worden gemaakt in een replica.

Als u wilt stoppen met replicatie tussen een hoofd- en een replica is vanaf de Azure-portal, gebruikt u de volgende stappen uit:
1.  Selecteer uw master Azure Database for PostgreSQL-server in de Azure-portal.

2.  Schakel replicatie in het menu onder instellingen.

3.  Selecteer de replica-server die u wilt stoppen van replicatie voor.

   ![Azure Database voor PostgreSQL - replica selecteren](./media/howto-read-replicas-portal/select-replica.png)
 
4.  Selecteer stoppen van de replicatie.

   ![Azure Database voor PostgreSQL - Selecteer stoppen van de replicatie](./media/howto-read-replicas-portal/select-stop-replication.png)
 
5.  Controleer of dat u wilt stoppen van replicatie door te klikken op OK.

   ![Azure Database voor PostgreSQL - Controleer of replicatie stoppen](./media/howto-read-replicas-portal/confirm-stop-replication.png)
 

## <a name="delete-a-master"></a>Een model verwijderen

> [!IMPORTANT]
> Verwijderen van een hoofdserver reageert replicatie naar alle replicaservers. Replica-servers worden zelfstandige servers die bieden nu ondersteuning voor zowel lees- en schrijfbewerkingen.
Verwijderen van een model, volgt u dezelfde stappen als voor een zelfstandige Azure Database for PostgreSQL-server. Als een server verwijderen uit de Azure-portal, het volgende doen:

1.  Selecteer uw master Azure Database for PostgreSQL-server in de Azure-portal.

2.  Selecteer in het overzicht van de verwijderen.

   ![Azure Database for PostgreSQL - server verwijderen](./media/howto-read-replicas-portal/delete-server.png)
 
3.  Typ de naam van de hoofd-server en selecteer verwijderen om te bevestigen van verwijderen van de hoofd-server.

   ![Azure Database voor PostgreSQL - verwijderen bevestigen](./media/howto-read-replicas-portal/confirm-delete.png)
 

## <a name="delete-a-replica"></a>Een replica verwijderen
Als u wilt verwijderen van een replica lezen kunt u dezelfde stappen volgen als met het verwijderen van een hoofdserver hoger. Eerst opent u de pagina overzicht van de replica en selecteer verwijderen.

   ![Azure Database voor PostgreSQL - replica verwijderen](./media/howto-read-replicas-portal/delete-replica.png)
 
U kunt ook kunt u deze verwijderen uit de replicatie-venster.
1.  Selecteer uw master Azure Database for PostgreSQL-server in de Azure-portal.

2.  Schakel replicatie in het menu onder instellingen.

3.  Selecteer de replicaserver die u wilt verwijderen. 

   ![Azure Database voor PostgreSQL - replica selecteren](./media/howto-read-replicas-portal/select-replica.png)
 
4.  Selecteer replica verwijderen.

   ![Azure Database voor PostgreSQL - Selecteer replica verwijderen](./media/howto-read-replicas-portal/select-delete-replica.png)
 
5.  Typ de naam van de replica en selecteer verwijderen om de verwijdering van de replica te bevestigen.

   ![Azure Database voor PostgreSQL - bevestigen replica verwijderen](./media/howto-read-replicas-portal/confirm-delete-replica.png)
 

## <a name="monitor-a-replica"></a>Een replica bewaken
### <a name="max-lag-across-replicas"></a>Maximale vertraging voor replica 's
De **maximale vertraging voor replica's** ziet u de vertraging in bytes tussen de hoofd- en de meeste achtergebleven replica. 

1.  Selecteer in de Azure portal, de **master** Azure Database for PostgreSQL-server.

2.  Selecteer de metrische gegevens. Selecteer in het venster metrische gegevens **maximale vertraging voor replica's**.

    ![Azure Database voor PostgreSQL - Monitor maximale vertraging voor replica 's](./media/howto-read-replicas-portal/select-max-lag.png)
 
3.  Selecteer **Max** als de aggregatie. 

### <a name="replica-lag"></a>Vertraging van replica
De **Replica Lag** metrische gegevens geeft de tijd sinds de laatste transactie op deze replica opnieuw afgespeeld. Als er geen transacties plaatsvinden in uw model, geeft de metriek deze vertraging.

1.  Selecteer in de Azure portal een **replica** Azure Database for PostgreSQL-server.

2.  Selecteer de metrische gegevens. Selecteer in het venster metrische gegevens **Replica Lag**.

   ![Azure Database voor PostgreSQL - Monitor replica lag](./media/howto-read-replicas-portal/select-replica-lag.png)
 
3.  Selecteer **Max** als de aggregatie. 
 
## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [lezen-replica's in Azure Database for PostgreSQL](concepts-read-replicas.md).