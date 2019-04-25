---
title: Het configureren van een Azure SQL-Database beheerd exemplaar | Microsoft Docs
description: Informatie over het configureren en beheren van beheerd exemplaar voor Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlr
manager: craigg
ms.date: 04/16/2019
ms.openlocfilehash: 886f06e8640891ac09d1e4624335a7bfebcd3def
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60340788"
---
# <a name="how-to-use-a-managed-instance-in-azure-sql-database"></a>Over het gebruik van een beheerd exemplaar in Azure SQL Database

In dit artikel vindt u diverse handleidingen, scripts en uitleg dat u helpen kan te beheren en configureren uw beheerde exemplaar.

## <a name="migration"></a>Migratie

- [Migreren naar een beheerd exemplaar](sql-database-managed-instance-migrate.md) : meer informatie over de aanbevolen migratieproces en hulpprogramma's voor migratie naar een beheerd exemplaar.

- [TDE cert migreren naar een beheerd exemplaar](sql-database-managed-instance-migrate-tde-certificate.md) : als uw SQL Server-database wordt beschermd met transparante gegevensversleuteling (TDE), moet u voor het migreren van certificaten die een beheerd exemplaar voor het ontsleutelen van de back-up die u wilt herstellen in Azure kunt gebruiken.

## <a name="network-configuration"></a>Netwerkconfiguratie

- [Bepaalt de grootte van het subnet van een beheerd exemplaar](sql-database-managed-instance-determine-size-vnet-subnet.md) – voor het beheerde exemplaar wordt geplaatst in worden op subnet dat kan niet worden gewijzigd nadat u de resources binnen hebt toegevoegd. Daarom moet u berekenen welke IP-adresbereik zijn vereist voor het subnet, afhankelijk van het aantal en typen van exemplaren die u wilt implementeren in het subnet.
- [Nieuwe VNet en subnet voor een beheerd exemplaar](sql-database-managed-instance-create-vnet-subnet.md) – Azure VNet en subnet waarvoor u wilt implementeren van uw beheerde exemplaren moeten worden geconfigureerd volgens de [vereisten voor de hier beschreven](sql-database-managed-instance-connectivity-architecture.md#network-requirements). In deze handleiding vindt u de eenvoudigste manier om uw nieuwe VNet en subnet juist geconfigureerd voor beheerde exemplaren maken.
- [Configureren van bestaande VNet en subnet voor een beheerd exemplaar](sql-database-managed-instance-configure-vnet-subnet.md) – als u configureren van uw bestaande VNet en subnet wilt voor het implementeren van beheerde exemplaren binnen, Hier vindt u het script waarmee wordt gecontroleerd of de [vereisten voor de](sql-database-managed-instance-connectivity-architecture.md#network-requirements) en controleer uw subnet op basis van de vereisten configureert.
- [Configureren van aangepaste DNS](sql-database-managed-instance-custom-dns.md) : u moet aangepaste DNS configureren als u wilt toegang krijgen tot externe bronnen voor de aangepaste domeinen van uw beheerde exemplaar via een gekoppelde server van db e-mailprofielen.
- [Netwerkconfiguratie synchroniseren](sql-database-managed-instance-sync-network-configuration.md) -het gebeuren dat hoewel u [geïntegreerd van uw app met een Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md), kunt u&#39;t verbinding maken met een beheerd exemplaar. Wat die kunt u proberen is om te vernieuwen voor de configuratie van netwerken voor uw service-plan.
- [Beheer van IP-adres van eindpunt vinden](sql-database-managed-instance-find-management-endpoint-ip-address.md) – openbaar eindpunt voor management-toepassing maakt gebruik van het beheerde exemplaar. IP-adres van het eindpunt voor het script dat wordt beschreven hier gebruiken, kunt u bepalen.
- [Controleer of de ingebouwde firewallbeveiliging](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md) – voor het beheerde exemplaar is beveiligd met ingebouwde firewall waarmee het verkeer alleen op de poorten die nodig zijn. U kunt controleren en controleer of de ingebouwde firewallregels met behulp van het script in deze handleiding beschreven.
- [Verbinding maken met toepassingen](sql-database-managed-instance-connect-app.md) – voor het beheerde exemplaar wordt geplaatst in uw eigen persoonlijke Azure-VNet met particuliere IP-adres. Meer informatie over andere patronen voor het verbinden van de toepassingen op uw beheerde exemplaar.

## <a name="feature-configuration"></a>Functieconfiguratie

- [Transactionele replicatie](replication-with-sql-database-managed-instance.md) kunt u uw gegevens repliceren tussen beheerde exemplaren of van on-premises SQL Server tot een beheerd exemplaar, en vice versa. Meer informatie vinden over het gebruik en transactionele replicatie configureren in deze handleiding.
- [Detectie van bedreigingen instellen](sql-database-managed-instance-threat-detection.md) – [detectie van bedreigingen](sql-database-threat-detection-overview.md) is een ingebouwde Azure SQL Database-functie die verschillende potentiële aanvallen, zoals SQL-injectie of toegang vanaf verdachte locaties gedetecteerd. In deze handleiding leert u hoe u inschakelen en configureren van [detectie van bedreigingen](sql-database-threat-detection-overview.md) voor een beheerd exemplaar.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [instructies gidsen voor individuele databases](sql-database-howto-single-database.md)