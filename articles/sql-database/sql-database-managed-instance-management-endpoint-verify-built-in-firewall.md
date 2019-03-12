---
title: Ontdek Azure SQL Database Managed Instance ingebouwde firewall | Microsoft Docs
description: Informatie over het controleren van de ingebouwde firewall-beveiliging in Azure SQL Database Managed Instance.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: f059ac4149a385a12b440db0ad6394a343a1f810
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57761919"
---
# <a name="verifying-the-managed-instance-built-in-firewall"></a>Controleren van de ingebouwde Managed Instance-firewall

Het beheerde exemplaar [verplichte beveiligingsregels voor binnenkomend verkeer](sql-database-managed-instance-connectivity-architecture.md#mandatory-inbound-security-rules) vereisen beheerpoorten 9000, 9003, 1438, 1440, 1452 moet openen vanuit **elke bron** op de Netwerkbeveiligingsgroep (NSG) die worden beveiligd met de beheerde Het exemplaar. Hoewel deze poorten geopend op het niveau van de NSG zijn, worden ze op het niveau van het netwerk beveiligd door de ingebouwde firewall.

## <a name="verify-firewall"></a>Controleer of de firewall

Gebruiken om te controleren of deze poorten, eventuele beveiligingsprogramma scanner voor het testen van deze poorten. De volgende schermafbeelding ziet u hoe u een van deze hulpprogramma's.

![Ingebouwde firewall controleren](./media/sql-database-managed-instance-management-endpoint/03_verify_firewall.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over beheerde exemplaren en -connectiviteit, [Azure SQL-Database beheerd exemplaar Connectiviteitsarchitectuur](sql-database-managed-instance-connectivity-architecture.md).