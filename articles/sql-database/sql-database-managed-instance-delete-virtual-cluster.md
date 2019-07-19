---
title: Een subnet verwijderen na het verwijderen van een door Azure SQL Database beheerd exemplaar | Microsoft Docs
description: Meer informatie over het verwijderen van een virtueel Azure-netwerk na het verwijderen van een door Azure SQL Database beheerd exemplaar.
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: douglas, carlrab, sstein
manager: craigg
ms.date: 06/26/2019
ms.openlocfilehash: ead7ea91e172f608c5364e4d5164d2a71dbf2f5f
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68297628"
---
# <a name="delete-a-subnet-after-deleting-an-azure-sql-database-managed-instance"></a>Een subnet verwijderen na het verwijderen van een door Azure SQL Database beheerd exemplaar

Dit artikel bevat richt lijnen voor het hand matig verwijderen van een subnet na het verwijderen van de laatste beheerde instantie van Azure SQL Database.

SQL Database gebruikt een [virtueel cluster](sql-database-managed-instance-connectivity-architecture.md#virtual-cluster-connectivity-architecture) om het verwijderde beheerde exemplaar te bevatten. Het virtuele cluster blijft 12 uur na het verwijderen van het exemplaar aanwezig, zodat u snel beheerde exemplaren in hetzelfde subnet kunt maken. Er worden geen kosten in rekening gebracht voor het bewaren van een leeg virtueel cluster. Gedurende deze periode kan het subnet dat is gekoppeld aan het virtuele-cluster, niet worden verwijderd.

Als u 12 uur niet wilt wachten en de voor keur geeft om het virtuele cluster en het bijbehorende subnet direct te verwijderen, kunt u dit hand matig doen. Verwijder het virtuele cluster hand matig met behulp van de Azure Portal of de API voor virtuele clusters.

> [!NOTE]
> Het virtuele cluster mag geen beheerde instanties bevatten zodat de verwijdering kan worden voltooid.

## <a name="delete-virtual-cluster-from-the-azure-portal"></a>Virtueel cluster verwijderen uit de Azure Portal

Als u een virtueel cluster wilt verwijderen met behulp van de Azure Portal, zoekt u de virtuele cluster resources.

![Scherm afbeelding van de Azure Portal, waarbij het zoekvak is gemarkeerd](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-search.png)

Nadat u het virtuele cluster dat u wilt verwijderen hebt gevonden, selecteert u deze resource en selecteert u **verwijderen**. U wordt gevraagd om het verwijderen van het virtuele cluster te bevestigen.

![Scherm afbeelding van het dash board Azure Portal virtuele clusters, met de optie verwijderen gemarkeerd](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-delete.png)

In het gebied Azure Portal meldingen ziet u een bevestiging dat het virtuele cluster is verwijderd. Als het virtuele cluster is verwijderd, wordt het subnet onmiddellijk vrijgegeven voor hergebruik.

> [!TIP]
> Als er geen beheerde exemplaren worden weer gegeven in het virtuele cluster en u het virtuele cluster niet kunt verwijderen, moet u ervoor zorgen dat er geen doorlopende implementatie van de instantie wordt uitgevoerd. Dit omvat gestarte en geannuleerde implementaties die nog worden uitgevoerd. Het tabblad implementaties controleren van de resource groep waarop het exemplaar is geïmplementeerd, geeft aan dat er implementaties worden uitgevoerd. In dit geval moet u wachten tot de implementatie is voltooid, het beheerde exemplaar verwijderen en vervolgens het virtuele cluster.

## <a name="delete-virtual-cluster-by-using-the-api"></a>Virtueel cluster verwijderen met de API

Als u een virtueel cluster wilt verwijderen via de API, gebruikt u de URI-para meters die zijn opgegeven in de methode voor het [verwijderen van virtuele clusters](https://docs.microsoft.com/rest/api/sql/virtualclusters/delete).

## <a name="next-steps"></a>Volgende stappen

- Zie [Wat is een beheerd exemplaar?](sql-database-managed-instance.md)voor een overzicht.
- Meer informatie over [connectiviteits architectuur in het beheerde exemplaar](sql-database-managed-instance-connectivity-architecture.md).
- Meer informatie over het [wijzigen van een bestaand virtueel netwerk voor een beheerd exemplaar](sql-database-managed-instance-configure-vnet-subnet.md).
- Voor een zelf studie waarin wordt getoond hoe u een virtueel netwerk maakt, een beheerd exemplaar maakt en een Data Base herstelt vanuit een back-up van de data base, raadpleegt u [een Azure SQL database beheerd exemplaar maken](sql-database-managed-instance-get-started.md).
- Zie [Configuring a Custom DNS](sql-database-managed-instance-custom-dns.md)(Engelstalig) voor DNS-problemen.
