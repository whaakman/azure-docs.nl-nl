---
title: Een bestaand virtueel netwerk configureren voor Azure SQL Database Managed Instance | Microsoft Docs
description: In dit artikel wordt beschreven hoe het configureren van een bestaand virtueel netwerk en subnet waarin u Azure SQL Database Managed Instance kunt implementeren.
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
ms.date: 01/03/2019
ms.openlocfilehash: 16684619ccf542783e425852a075eaa74e96e592
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54332180"
---
# <a name="configure-an-existing-virtual-network-for-azure-sql-database-managed-instance"></a>Een bestaand virtueel netwerk configureren voor Azure SQL Database Managed Instance

Azure SQL Database Managed Instance moet worden geïmplementeerd in een Azure [virtueel netwerk](../virtual-network/virtual-networks-overview.md) en het subnet alleen bestemd voor het beheerde instanties. U kunt de bestaande virtuele netwerk en subnet gebruiken als deze geconfigureerd volgens de [vereisten voor het virtuele netwerk van Managed Instance](sql-database-managed-instance-connectivity-architecture.md#network-requirements).

Als een van de volgende gevallen geldt voor u zijn, kunt u valideren en wijzigen van uw netwerk met behulp van het script in dit artikel wordt uitgelegd:

* U hebt een nieuw subnet die nog niet is geconfigureerd.
* Je niet zeker weet dat het subnet wordt uitgelijnd met de [vereisten](sql-database-managed-instance-connectivity-architecture.md#network-requirements).
* U wilt controleren of het subnet nog steeds voldoet aan de [vereisten voor de](sql-database-managed-instance-connectivity-architecture.md#network-requirements) nadat u wijzigingen hebt aangebracht.


> [!Note]
> U kunt een beheerd exemplaar maken alleen in virtuele netwerken die zijn gemaakt via het Azure Resource Manager-implementatiemodel. Azure-netwerken die zijn gemaakt via het klassieke implementatiemodel worden niet ondersteund. Grootte van het gatewaysubnet berekenen door de richtlijnen in de [bepaalt de grootte van het subnet voor beheerde instanties](sql-database-managed-instance-determine-size-vnet-subnet.md) artikel. U kunt de grootte van het subnet kan niet wijzigen nadat u de resources binnen implementeren.

## <a name="validate-and-modify-an-existing-virtual-network"></a>Valideren en te wijzigen van een bestaand virtueel netwerk

Als u maken van een beheerd exemplaar in een bestaand subnet wilt, raden we het volgende PowerShell-script voor het voorbereiden van het subnet:

```powershell
$scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/prepare-subnet'

$parameters = @{
    subscriptionId = '<subscriptionId>'
    resourceGroupName = '<resourceGroupName>'
    virtualNetworkName = '<virtualNetworkName>'
    subnetName = '<subnetName>'
    }

Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/prepareSubnet.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters
```

Het script bereidt het subnet in drie stappen:

1. Valideren: De geselecteerde virtuele netwerk en subnet voor de netwerkvereisten Managed Instance gevalideerd.
2. Bevestig als volgt: Het bevat een reeks wijzigingen die worden aangebracht moeten aan het voorbereiden van het subnet voor de implementatie van het beheerde exemplaar van de gebruiker. Dit wordt ook gevraagd om toestemming.
3. Voorbereiden: Juist configureert u het virtuele netwerk en subnet.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [wat is een beheerd exemplaar?](sql-database-managed-instance.md).
- Zie voor een zelfstudie die laat zien hoe u een virtueel netwerk maken en een database herstellen vanuit een databaseback-up maken van een beheerd exemplaar, [maken van een Azure SQL Database Managed Instance](sql-database-managed-instance-get-started.md).
- Zie voor DNS-problemen, [configureren van een aangepaste DNS](sql-database-managed-instance-custom-dns.md).
