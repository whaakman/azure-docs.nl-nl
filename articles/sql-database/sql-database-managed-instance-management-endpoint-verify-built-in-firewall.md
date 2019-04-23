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
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 774455a2901782ef52b213c6a13c17636e28b1a4
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59788102"
---
# <a name="verifying-the-managed-instance-built-in-firewall"></a>Controleren van de ingebouwde Managed Instance-firewall

Het beheerde exemplaar [verplichte beveiligingsregels voor binnenkomend verkeer](sql-database-managed-instance-connectivity-architecture.md#mandatory-inbound-security-rules) vereisen beheerpoorten 9000, 9003, 1438, 1440, 1452 moet openen vanuit **elke bron** op de Netwerkbeveiligingsgroep (NSG) die worden beveiligd met de beheerde Het exemplaar. Hoewel deze poorten geopend op het niveau van de NSG zijn, worden ze op het niveau van het netwerk beveiligd door de ingebouwde firewall.

## <a name="verify-firewall"></a>Controleer of de firewall

Gebruiken om te controleren of deze poorten, eventuele beveiligingsprogramma scanner voor het testen van deze poorten. De volgende schermafbeelding ziet u hoe u een van deze hulpprogramma's.

![Ingebouwde firewall controleren](./media/sql-database-managed-instance-management-endpoint/03_verify_firewall.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over beheerde exemplaren en -connectiviteit, [Azure SQL-Database beheerd exemplaar Connectiviteitsarchitectuur](sql-database-managed-instance-connectivity-architecture.md).