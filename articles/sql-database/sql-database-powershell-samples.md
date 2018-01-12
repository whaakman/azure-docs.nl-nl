---
title: Azure PowerShell-scriptvoorbeelden voor SQL-Database | Microsoft Docs
description: Azure PowerShell-script voorbeelden voor hulp bij het maken en beheren van Azure SQL Database-servers, elastische pools, databases en firewalls.
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: jhubbard
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: overview-samples, mvc
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: On Demand
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: fd60a8987e6c57cf72eb7766f6a1b784e46a894d
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="azure-powershell-samples-for-azure-sql-database"></a>Voorbeelden van Azure PowerShell voor Azure SQL Database

De volgende tabel bevat koppelingen naar Azure PowerShell-voorbeeldscripts voor Azure SQL Database.

| |  |
|---|---|
|**Een individuele database en een elastische pool maken**||
| [Maken van een individuele database en een firewallregel configureren](scripts/sql-database-create-and-configure-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Deze PowerShell-script één Azure SQL database maakt en configureert u een firewallregel op serverniveau. |
| [Maken van elastische pools en gegroepeerde databases verplaatsen](scripts/sql-database-move-database-between-pools-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Deze PowerShell-script maakt Azure SQL Database elastische pools, gegroepeerde databases worden verplaatst en prestatieniveaus wordt gewijzigd.|
|**Geo-replicatie en failover configureren**||
| [Configureren en failover een individuele database met behulp van actieve geo-replicatie](scripts/sql-database-setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Deze PowerShell-script configureert actieve geo-replicatie voor een enkele Azure SQL database en failover naar de secundaire replica. |
| [Configureren en failover een gegroepeerde database met behulp van actieve geo-replicatie](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Dit PowerShell-script, configureert u het actieve geo-replicatie voor een Azure SQL database in een elastische SQL-groep en failover naar de secundaire replica. |
| [Configureren en failover een failover-groep voor één database (preview)](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Deze PowerShell-script configureert u een failover-groep voor een Azure SQL Database-server-exemplaar, een database wordt toegevoegd aan de groep failover en failover naar de secundaire server |
|**De schaal van een individuele databases en een elastische pool**||
| [Een individuele database schalen](scripts/sql-database-monitor-and-scale-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Deze PowerShell-script monitors de maatstaven voor prestaties van een Azure SQL-database naar een hoger prestatieniveau op schaal en een waarschuwingsregel maakt op een van de maatstaven voor prestaties. |
| [Een elastische pool schalen](scripts/sql-database-monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Deze PowerShell-script monitors de maatstaven voor prestaties van een elastische pool Azure SQL Database naar een hoger prestatieniveau op schaal en een waarschuwingsregel maakt op een van de maatstaven voor prestaties.  |
| **Controle en detectie van bedreigingen** |
| [Controle en detectie van dreigingen configureren](scripts/sql-database-auditing-and-threat-detection-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Deze PowerShell-script configureert controle en threat beleidsregels voor een Azure SQL database. |
| **Terugzetten, kopiëren en importeren van een database**||
| [Een database herstellen](scripts/sql-database-restore-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Deze PowerShell-script voor een Azure SQL database herstelt vanuit een geografisch redundante back-up en een verwijderde Azure SQL-database naar de meest recente back-up hersteld. |
| [Een database kopiëren naar de nieuwe server](scripts/sql-database-copy-database-to-new-server-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Deze PowerShell-script maakt een kopie van een bestaande Azure SQL database in een nieuwe Azure SQL-server. |
| [Een database uit een Bacpac-bestand importeren](scripts/sql-database-import-from-bacpac-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Deze PowerShell-script importeert een database met een Azure SQL-server uit een Bacpac-bestand. |
| **Gegevens tussen databases synchroniseren**||
| [Gegevens synchroniseren tussen de SQL-databases](scripts/sql-database-sync-data-between-sql-databases.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Deze PowerShell-script configureert synchroniseren van gegevens te synchroniseren tussen meerdere Azure SQL-databases. |
| [Gegevens synchroniseren tussen on-premises SQL-Database en SQL Server](scripts/sql-database-sync-data-between-azure-onprem.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Deze PowerShell-script configureert synchroniseren van gegevens te synchroniseren tussen een Azure SQL database en een lokale SQL Server-database. |
| [Het synchroniseren van de SQL-gegevens synchroniseren schema bijwerken](scripts/sql-database-sync-update-schema.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Deze PowerShell-script toevoegen of verwijderen van items uit het schema van de synchronisatie synchroniseren van gegevens. |
|||
