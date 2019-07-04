---
title: Een subnet na het verwijderen van een Azure SQL-Database beheerd exemplaar verwijderen | Microsoft Docs
description: Leer hoe u een Azure-netwerk na het verwijderen van een Azure SQL-Database beheerd exemplaar verwijderen.
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
ms.date: 06/26/2019
ms.openlocfilehash: 4679ecda210fa78aad4315bc6602b67dd1795ce9
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67427973"
---
# <a name="delete-a-subnet-after-deleting-an-azure-sql-database-managed-instance"></a>Een subnet na het verwijderen van een Azure SQL-Database beheerd exemplaar verwijderen

In dit artikel bevat richtlijnen over het handmatig verwijderen van een subnet nadat het verwijderen van de laatste Azure SQL-Database beheerd exemplaar die zich bevinden in het.

SQL Database maakt gebruik van een [virtueel cluster](sql-database-managed-instance-connectivity-architecture.md#virtual-cluster-connectivity-architecture) bevat het verwijderde beheerde exemplaar. De virtuele-cluster zich blijft voordoen voor 12 uur na het verwijderen van instantie, zodat u snel beheerde exemplaren maken in hetzelfde subnet. Er zijn geen kosten voor het bewaren van een lege virtuele cluster. Gedurende deze periode kan het subnet dat is gekoppeld aan het virtuele-cluster, niet worden verwijderd.

Als u niet wilt 12 uur wachten en wil onmiddellijk verwijderen van de virtuele-cluster en het subnet ervan bevindt, kunt u dit handmatig doen. De virtuele cluster handmatig verwijderen met behulp van de Azure-portal of de virtuele clusters API.

> [!NOTE]
> De virtuele cluster mag geen beheerde exemplaren voor de verwijdering om succesvol te zijn.

## <a name="delete-virtual-cluster-from-the-azure-portal"></a>Virtueel cluster verwijderen uit de Azure portal

Als u wilt verwijderen van een virtueel cluster met behulp van Azure portal, zoek naar de virtuele cluster-resources.

![Schermafbeelding van de Azure portal, met het zoekvak gemarkeerd](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-search.png)

Nadat u de virtuele cluster die u wilt verwijderen, selecteert u deze resource en selecteer **verwijderen**. U wordt gevraagd om de virtuele cluster verwijdering te bevestigen.

![Schermafbeelding van de Azure portal virtuele clusters dashboard met de optie voor het verwijderen is gemarkeerd](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-delete.png)

De Azure portal meldingengebied ziet u een bevestiging dat de virtuele-cluster is verwijderd. Geslaagde verwijdering van de virtuele cluster versies onmiddellijk het subnet voor hergebruik.

> [!TIP]
> Als er geen beheerde exemplaren wordt weergegeven in de virtuele-cluster zijn, en u kan niet worden verwijderd van de virtuele-cluster, zorgt u ervoor dat u geen implementatie van een doorlopende exemplaar wordt uitgevoerd. Dit omvat de slag te gaan en geannuleerde implementaties die nog steeds uitgevoerd worden. Implementaties tabblad van de resourcegroep waarin het exemplaar is geïmplementeerd om te controleren, geeft alle implementaties wordt uitgevoerd. In dit geval await voor implementatie om te voltooien, beheerd exemplaar en vervolgens de virtuele-cluster verwijderen.

## <a name="delete-virtual-cluster-by-using-the-api"></a>Virtueel cluster verwijderen met behulp van de API

Als u wilt verwijderen van een virtueel cluster via de API, gebruikt u de URI-parameters dat is opgegeven in de [virtuele clusters verwijderen methode](https://docs.microsoft.com/rest/api/sql/virtualclusters/delete).

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [wat is een beheerd exemplaar?](sql-database-managed-instance.md).
- Meer informatie over [connectiviteitsarchitectuur in het beheerde exemplaar](sql-database-managed-instance-connectivity-architecture.md).
- Meer informatie over het [wijzigen van een bestaand virtueel netwerk voor beheerd exemplaar](sql-database-managed-instance-configure-vnet-subnet.md).
- Zie voor een zelfstudie die laat zien hoe u een virtueel netwerk maken en een database herstellen vanuit een databaseback-up maken van een beheerd exemplaar, [maken van een Azure SQL Database Managed Instance](sql-database-managed-instance-get-started.md).
- Zie voor DNS-problemen, [configureren van een aangepaste DNS](sql-database-managed-instance-custom-dns.md).
