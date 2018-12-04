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
ms.date: 12/03/2018
ms.openlocfilehash: 99ec559429d66becc20e038e43349f5369afac39
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52855830"
---
# <a name="discover-azure-sql-database-managed-instance-management-endpoint"></a>Ontdek Azure SQL Database Managed Instance-beheereindpunt 

## <a name="overview"></a>Overzicht
Azure SQL Managed Instance [virtueel cluster](sql-database-managed-instance-connectivity-architecture.md) beheereindpunt die gebruikmaakt van Microsoft voor het beheren van het exemplaar bevat.  

Beheereindpunt is beveiligd met ingebouwde firewall netwerk certificaat niveau en wederzijdse verificatie op toepassingsniveau. 

Wanneer verbindingen geïnitieerd worden vanuit binnen het beheerde exemplaar (CLR, gekoppelde server, back-up, auditlogboek enz.) het lijkt erop dat verkeer afkomstig is uit het beheer van eindpunt openbare IP-adres. U kunt toegang beperken tot openbare services van Managed Instance door in te stellen van firewallregels om toe te staan van dit IP-adres.

> [!NOTE]
> Dit is niet van toepassing op het instellen van de firewallregels voor Azure-services die zich in dezelfde regio als beheerd exemplaar als het platform heeft optimalisatie voor het verkeer dat wordt verstuurd tussen de services die zijn geplaatst. 

In dit artikel wordt uitgelegd hoe u management openbare IP-adres van het eindpunt en over de manier om te controleren of de ingebouwde firewall-beveiliging kan ophalen.

## <a name="finding-management-endpoint-public-ip-address"></a>Beheer van eindpunt openbare IP-adres zoeken
We gaan ervan uit dat MI host _mi-demo.xxxxxx.database.windows.net_. Voer _nslookup_ met behulp van de hostnaam.

![Het omzetten van interne host-naam](./media/sql-database-managed-instance-management-endpoint/01_find_internal_host.png)

Voer een andere _nslookup_ voor de naam van de gemarkeerde met removed_.vnet._segment. U krijgt het openbare IP-adres als gevolg van deze opdracht wordt uitgevoerd.

![Het omzetten van openbare IP-adres](./media/sql-database-managed-instance-management-endpoint/02_find_public_ip.png)

## <a name="verifying-managed-instance-built-in-firewall"></a>Managed Instance ingebouwde firewall controleren
Beheerd exemplaar [verplichte beveiligingsregels voor binnenkomend verkeer](sql-database-managed-instance-vnet-configuration.md#mandatory-inbound-security-rules) vereisen beheerpoorten 9000, 9003, 1438, 1440, 1452 moet openen vanuit een bron op Network Security Group die worden beveiligd met het beheerde exemplaar. Hoewel deze poorten geopend op het niveau van de NSG zijn, worden ze op het netwerkniveau van het beveiligd door ingebouwde firewall.

U kunt elke scanner beveiligingsprogramma om te controleren of deze poorten, gebruiken voor het testen van deze poorten. De volgende schermafbeelding ziet u hoe u een van deze hulpprogramma's.

![Ingebouwde firewall controleren](./media/sql-database-managed-instance-management-endpoint/03_verify_firewall.png)
