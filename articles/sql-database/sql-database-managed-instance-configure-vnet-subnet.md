---
title: Azure SQL Database Managed Instance bestaande VNET/subnet configureren | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u een bestaand virtueel netwerk (VNet) en het subnet waarin u Azure SQL Database Managed Instance kunt implementeren.
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
ms.openlocfilehash: 1718177a0902bc7049eb6986e5a1d128eeb3f233
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54040955"
---
# <a name="configure-an-existing-vnet-for-azure-sql-database-managed-instance"></a>Een bestaand VNet configureren voor Azure SQL Database Managed Instance

Azure SQL Database Managed Instance moet worden geïmplementeerd in een Azure [virtueel netwerk (VNet)](../virtual-network/virtual-networks-overview.md) en het subnet alleen bestemd voor het beheerde instanties. U kunt de bestaande VNet en subnet gebruiken als deze is geconfigureerd volgens de [vereisten voor beheerd exemplaar VNet](sql-database-managed-instance-connectivity-architecture.md#network-requirements).

Hebt u een nieuw subnet die nog niet is geconfigureerd, u niet zeker bent het subnet wordt uitgelijnd met de [vereisten](sql-database-managed-instance-connectivity-architecture.md#network-requirements), of u wilt controleren is het subnet nog steeds compatibel zijn met de [vereisten voor de](sql-database-managed-instance-connectivity-architecture.md#network-requirements) nadat aantal wijzigingen die u hebt aangebracht, kunt u valideren en te wijzigen van uw netwerk met behulp van het script in deze sectie wordt uitgelegd.

  > [!Note]
  > U kunt alleen een beheerd exemplaar maken in Resource Manager-virtuele netwerken. Azure VNets die zijn geïmplementeerd met het klassieke implementatiemodel gebruikt, zijn niet suported. Zorg ervoor dat u de grootte van het gatewaysubnet berekenen door de richtlijnen in de [bepaalt de grootte van het subnet voor beheerde instanties](sql-database-managed-instance-determine-size-vnet-subnet.md) artikel, omdat het subnet kan niet worden gewijzigd nadat u de resources binnen implementeren.

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

De voorbereiding van het subnet is gedaan in drie eenvoudige stappen:

1. Valideren: de geselecteerde virtuele netwerk en subnet worden gevalideerd voor beheerd exemplaar netwerkvereisten.
2. Controleer of - gebruiker een set wijzigingen die moeten worden aangebracht in het subnet voor de Managed Instance-implementatie voorbereiden en wordt gevraagd om toestemming wordt weergegeven.
3. Voorbereiden: virtueel netwerk en subnet juist zijn geconfigureerd.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [wat is een beheerd exemplaar](sql-database-managed-instance.md)
- Zie voor een zelfstudie waarin wordt getoond hoe een VNet maken en een database herstellen vanuit een databaseback-up maken van een beheerd exemplaar, [het maken van een Azure SQL Database Managed Instance](sql-database-managed-instance-get-started.md).
- Zie voor DNS-problemen, [configureren van een aangepaste DNS](sql-database-managed-instance-custom-dns.md).
