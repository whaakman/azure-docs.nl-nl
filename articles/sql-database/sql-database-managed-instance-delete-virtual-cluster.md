---
title: VNet na het verwijderen van Azure SQL-Database beheerd exemplaar verwijderen | Microsoft Docs
description: Informatie over het VNet worden verwijderd nadat het verwijderen van Azure SQL-Database beheerd exemplaar.
services: sql-database
ms.service: sql-database
ms.subservice: management
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: douglas, carlrab, sstein
manager: craigg
ms.date: 05/07/2019
ms.openlocfilehash: 61f6c25031c4906e65c2f75a7679600741e8311a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65791380"
---
# <a name="delete-subnet-after-deleting-azure-sql-database-managed-instance"></a>Subnet na het verwijderen van Azure SQL-Database beheerd exemplaar verwijderen

In dit artikel bevat richtlijnen over het handmatig verwijderen van subnet nadat het verwijderen van de laatste Azure SQL-Database beheerd exemplaar die zich bevinden in het.

De [virtueel cluster](sql-database-managed-instance-connectivity-architecture.md#virtual-cluster-connectivity-architecture) die is opgenomen als het verwijderde beheerd exemplaar van het verwijderen van instantie 12 uur worden bewaard. De virtuele cluster wordt actief standaard voor snellere het maken van beheerde exemplaren in hetzelfde subnet opgeslagen. Een lege virtuele cluster te houden, is gratis. Tijdens deze periode, kan niet het subnet dat is gekoppeld aan de virtuele-cluster worden verwijderd.

Onmiddellijke versie van het subnet dat wordt gebruikt door een lege virtuele cluster is mogelijk via handmatige verwijdering van de virtuele-cluster. Het verwijderen van de virtuele-cluster kan worden bereikt via Azure portal of virtuele clusters API.

> [!NOTE]
> De virtuele cluster mag geen beheerde exemplaren voor de verwijdering om succesvol te zijn.

## <a name="delete-virtual-cluster-from-azure-portal"></a>Virtueel cluster verwijderen uit Azure portal

Als u wilt verwijderen van een virtueel cluster met behulp van Azure portal, zoeken naar de virtuele clusterbronnen met behulp van de ingebouwde zoekquery.

![Zoeken naar virtueel cluster.](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-search.png)

Nadat u de virtuele cluster die u wilt verwijderen hebt gevonden, selecteert u deze resource en selecteer de optie voor het verwijderen. U wordt gevraagd om de virtuele cluster verwijdering te bevestigen.

![Virtueel cluster verwijderen.](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-delete.png)

Bevestiging dat de virtuele-cluster is verwijderd, wordt geleverd in Azure portal meldingen. Geslaagde verwijdering van de virtuele cluster versies onmiddellijk het subnet voor verdere hergebruik.

## <a name="delete-virtual-cluster-using-api"></a>Verwijderen van virtueel cluster met behulp van API

Verwijderen van een virtuele cluster via API de URI-parameters dat is opgegeven in de [virtuele clusters verwijderen methode](https://docs.microsoft.com/rest/api/sql/virtualclusters/delete).

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [wat is een beheerd exemplaar?](sql-database-managed-instance.md).
- Meer informatie over [connectiviteitsarchitectuur in het beheerde exemplaar](sql-database-managed-instance-connectivity-architecture.md).
- Meer informatie over het [wijzigen van een bestaand virtueel netwerk voor beheerd exemplaar](sql-database-managed-instance-configure-vnet-subnet.md).
- Zie voor een zelfstudie die laat zien hoe u een virtueel netwerk maken en een database herstellen vanuit een databaseback-up maken van een beheerd exemplaar, [maken van een Azure SQL Database Managed Instance](sql-database-managed-instance-get-started.md).
- Zie voor DNS-problemen, [configureren van een aangepaste DNS](sql-database-managed-instance-custom-dns.md).
