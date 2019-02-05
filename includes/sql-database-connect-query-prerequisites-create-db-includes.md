---
author: MightyPen
ms.service: sql-database
ms.topic: include
ms.date: 01/28/2019
ms.author: genemi
ms.openlocfilehash: edeaa932abe4d1882f957d0ed26aaddd97dabe3f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55198058"
---
<!-- sql-database-connect-query-prerequisites-create-db-includes.md -->

- Een Azure SQL Database op een [Logische server](https://docs.microsoft.com/azure/sql-database/sql-database-single-index) of [Beheerd exemplaar](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index). U kunt op een van de volgende manieren een database maken:

| Logische server | Beheerd exemplaar |
| --- | --- |
| [Portal](../articles/sql-database/sql-database-get-started-portal.md) | [Portal](../articles/sql-database/sql-database-managed-instance-get-started.md) |
| [CLI](../articles/sql-database/sql-database-get-started-cli.md) | |
| [PowerShell](../articles/sql-database/sql-database-get-started-powershell.md) | |

- **Alleen logische server** - een geconfigureerde-firewallregel op serverniveau waarmee u verbinding kunt maken met uw logische server. Zie [Een serverfirewallregel maken](../articles/sql-database/sql-database-get-started-portal-firewall.md) voor meer informatie.
- **Alleen beheerd exemplaar** - een geconfigureerde verbinding vanuit de computer die toegang krijgt tot het beheerde exemplaar. U kunt de volgende opties gebruiken:
  - [Virtuele Azure-machine](../articles/sql-database/sql-database-managed-instance-configure-vm.md) in hetzelfde Azure VNet als het beheerde exemplaar dat toegang heeft tot het exemplaar.
  - [Punt-naar-site-verbinding](../articles/sql-database/sql-database-managed-instance-configure-p2s.md) op uw computer waarmee u uw computer kunt aansluiten op het VNet waar het beheerde exemplaar is geplaatst en u het beheerde exemplaar kunt gebruiken zoals elke andere SQL Server in uw netwerk.
