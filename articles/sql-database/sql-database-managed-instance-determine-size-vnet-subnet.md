---
title: Azure SQL Database Managed Instance VNet/subnet grootte bepalen | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u voor het berekenen van de grootte van het subnet waarin de Azure SQL Database beheerde instanties worden geïmplementeerd.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 12/12/2018
ms.openlocfilehash: 97bb7ac79dc96d7f66ae330f5d6a603ca600f648
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54043365"
---
# <a name="determine-vnet-subnet-size-for-azure-sql-database-managed-instance"></a>Grootte van de VNet-subnet voor Azure SQL Database Managed Instance bepalen

Azure SQL Database Managed Instance moet worden geïmplementeerd in een Azure [virtueel netwerk (VNet)](../virtual-network/virtual-networks-overview.md).

Het aantal beheerde instanties dat kan worden geïmplementeerd in het subnet van VNet, is afhankelijk van de grootte van het subnet (subnetbereik).

Wanneer u een beheerd exemplaar maakt, wijst Azure een aantal virtuele machines, afhankelijk van de laag die u hebt geselecteerd tijdens het inrichten. Omdat deze virtuele machines gekoppeld aan het subnet zijn, ze hebben IP-adressen nodig. Voor hoge beschikbaarheid tijdens normale bewerkingen en onderhoud van de service, kan Azure extra virtuele machines toewijzen. Als gevolg hiervan is het aantal vereiste IP-adressen in een subnet groter is dan het aantal beheerde exemplaren in dat subnet.

Standaard is een beheerd exemplaar moet minimaal 16 IP-adressen in een subnet, en mag maximaal 256 IP-adressen gebruiken. Als gevolg hiervan kunt u subnetmaskers /28-/24 bij het definiëren van uw subnet IP-adresbereiken.

> [!IMPORTANT]
> Grootte van het gatewaysubnet met 16 IP-adressen is de minimumwaarde met beperkte mogelijkheden voor het verder beheerd exemplaar voor scale-out. Kiezen subnet met het voorvoegsel /27 of lager wordt sterk aanbevolen.

## <a name="determine-subnet-size"></a>Grootte van het gatewaysubnet bepalen

Als u van plan bent te implementeren van meerdere beheerde exemplaren binnen het subnet en wilt optimaliseren op de grootte van het gatewaysubnet, moet u deze parameters gebruiken om te vormen een berekening:

- Vijf IP-adressen worden gebruikt in Azure in het subnet voor de eigen behoeften
- Elk exemplaar algemeen moeten twee adressen
- Elke bedrijfskritiek exemplaar moeten vier adressen

**Voorbeeld**: U van plan bent om drie algemeen gebruik en twee bedrijven essentiële beheerde instanties. Dat betekent dat u moet 5 + 3 * 2 + 2 * 4 = 19 IP-adressen. Als het IP-adresbereiken zijn gedefinieerd in de macht van 2, moet u het IP-adresbereik van 32 (2 ^ 5) IP-adressen. Daarom moet u het subnet met het subnetmasker/27 reserveren.

> [!IMPORTANT]
> Berekening die wordt weergegeven boven zijn verouderd met verdere verbeteringen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [wat is een beheerd exemplaar](sql-database-managed-instance.md).
- Meer informatie over [connectiviteitsarchitectuur voor beheerd exemplaar](sql-database-managed-instance-connectivity-architecture.md).
- Zie hoe u [VNet waar u beheerde instanties implementeren maken](sql-database-managed-instance-create-vnet-subnet.md)
- Zie voor DNS-problemen, [een aangepaste DNS-server configureren](sql-database-managed-instance-custom-dns.md)
