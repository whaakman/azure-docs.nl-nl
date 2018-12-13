---
title: Ontdek Azure SQL Database Managed Instance-beheereindpunt | Microsoft Docs
description: Meer informatie over het krijgen van Azure SQL Database Managed Instance management eindpunt openbare IP-adres te controleren of de ingebouwde firewall-beveiliging
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: fc04422f5e32a77fff42dc07e2441e84c9b15866
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53271875"
---
# <a name="discover-azure-sql-database-managed-instance-management-endpoint"></a>Ontdek Azure SQL Database Managed Instance-beheereindpunt

De Azure SQL Database Managed Instance [virtueel cluster](sql-database-managed-instance-connectivity-architecture.md) bevat een beheereindpunt die gebruikmaakt van Microsoft voor het beheren van het beheerde exemplaar. Het eindpunt is beveiligd met ingebouwde firewall netwerk certificaat niveau en wederzijdse verificatie op toepassingsniveau.

Wanneer verbindingen geïnitieerd worden vanuit binnen het beheerde exemplaar (back-up, auditlogboek) wordt weergegeven dat verkeer afkomstig is uit het beheer van eindpunt openbare IP-adres. U kunt toegang beperken tot openbare services van Managed Instance door in te stellen van firewallregels om toe te staan alleen het beheerd exemplaar IP-adres.

> [!NOTE]
> Dit is niet van toepassing op het instellen van de firewallregels voor Azure-services die zich in dezelfde regio als beheerd exemplaar als het Azure-platform heeft een optimalisatie voor het verkeer dat wordt verstuurd tussen de services die zijn geplaatst.

In dit artikel wordt uitgelegd hoe u management openbare IP-adres van het eindpunt en over de manier om te controleren of de ingebouwde firewall-beveiliging kan ophalen.

## <a name="finding-the-management-endpoint-public-ip-address"></a>Zoeken naar openbare IP-adres van het management eindpunt

We gaan ervan uit dat Managed Instance host `mi-demo.xxxxxx.database.windows.net`. Voer `nslookup` met behulp van de hostnaam.

![Het omzetten van interne host-naam](./media/sql-database-managed-instance-management-endpoint/01_find_internal_host.png)

Voer een andere `nslookup` voor het verwijderen van de naam van de gemarkeerde de `.vnet.` segment. U krijgt het openbare IP-adres als gevolg van deze opdracht wordt uitgevoerd.

![Het omzetten van openbare IP-adres](./media/sql-database-managed-instance-management-endpoint/02_find_public_ip.png)

## <a name="verifying-the-managed-instance-built-in-firewall"></a>Controleren van de ingebouwde Managed Instance-firewall

Het beheerde exemplaar [verplichte beveiligingsregels voor binnenkomend verkeer](sql-database-managed-instance-vnet-configuration.md#mandatory-inbound-security-rules) vereisen beheerpoorten 9000, 9003, 1438, 1440, 1452 moet openen vanuit **elke bron** op de Netwerkbeveiligingsgroep (NSG) die worden beveiligd met de beheerde Het exemplaar. Hoewel deze poorten geopend op het niveau van de NSG zijn, worden ze op het niveau van het netwerk beveiligd door de ingebouwde firewall.

Gebruiken om te controleren of deze poorten, eventuele beveiligingsprogramma scanner voor het testen van deze poorten. De volgende schermafbeelding ziet u hoe u een van deze hulpprogramma's.

![Ingebouwde firewall controleren](./media/sql-database-managed-instance-management-endpoint/03_verify_firewall.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over beheerde exemplaren en -connectiviteit, [Azure SQL-Database beheerd exemplaar Connectiviteitsarchitectuur](sql-database-managed-instance-connectivity-architecture.md).