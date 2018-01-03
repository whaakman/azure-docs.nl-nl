---
title: Sleutelkluis integreert met SQL Server op Windows-machines in Azure (Resource Manager) | Microsoft Docs
description: Informatie over het automatiseren van de configuratie van SQL Server-versleuteling voor gebruik met Azure Sleutelkluis. Dit onderwerp wordt uitgelegd hoe u Azure Sleutelkluis-integratie met SQL Server virtuele machines die zijn gemaakt met Resource Manager.
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: cd66dfb1-0e9b-4fb0-a471-9deaf4ab4ab8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/23/2017
ms.author: jroth
ms.openlocfilehash: 32b9564fa5c9ca6864ade343fda309b2c3edf123
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-resource-manager"></a>Configureren van Azure Sleutelkluis-integratie voor SQL Server op Azure Virtual Machines (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-ps-sql-keyvault.md)
> * [Klassiek](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a>Overzicht
Er zijn meerdere onderdelen van SQL Server-versleuteling, zoals [transparante gegevensversleuteling (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [kolom: versleuteling op bestandsniveau (wissen)](https://msdn.microsoft.com/library/ms173744.aspx), en [back-upversleuteling](https://msdn.microsoft.com/library/dn449489.aspx). Deze vormen van versleuteling hoeft te beheren en opslaan van de cryptografische sleutels die u voor versleuteling gebruikt. De service Azure Key Vault (Azure Sleutelkluis) is ontworpen voor het verbeteren van de beveiliging en beheer van deze sleutels op een locatie met veilige en maximaal beschikbaar. De [SQL Server-Connector](http://www.microsoft.com/download/details.aspx?id=45344) kan SQL-Server in het gebruik van deze sleutels van Azure Sleutelkluis.

Als u met SQL Server met on-premises machines, er zijn [stappen die u kunt volgen om toegang tot Azure Key Vault vanuit uw on-premises SQL Server-machine](https://msdn.microsoft.com/library/dn198405.aspx). Maar voor SQL Server in Azure VM's, kunt u tijd besparen met behulp van de *Azure Sleutelkluis-integratie* functie.

Wanneer deze functie is ingeschakeld, en deze automatisch de SQL Server-Connector installeert, configureert u de EKM-provider voor toegang tot Azure Sleutelkluis, maakt u de referentie zodat u toegang tot uw kluis. Als u de stappen in de documentatie van de eerder genoemde lokale bekeken, ziet u dat deze functie stap 2 en 3 automatiseert. Het enige dat u toch wilt handmatig doen is de sleutelkluis en de sleutels te maken. Van daaruit wordt de volledige installatie van de SQL-VM geautomatiseerd. Zodra deze functie kan voor deze installatie is voltooid, kunt u de T-SQL-instructies om te beginnen met het versleutelen van uw databases of back-ups normaal uitvoeren.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Inschakelen en configureren van Azure Sleutelkluis-integratie
U kunt Azure Sleutelkluis-integratie tijdens het inrichten of configureren voor een bestaande virtuele machines.

### <a name="new-vms"></a>Nieuwe virtuele machines
Als u een nieuwe virtuele SQL Server-machine met Resource Manager inricht, biedt de Azure portal een stap Azure Sleutelkluis-integratie in te schakelen. De functie Azure Sleutelkluis is alleen beschikbaar voor de Enterprise, Developer en evaluatie-editie van SQL Server.

![Integratie van Azure Sleutelkluis in SQL](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Zie voor een gedetailleerd overzicht van de inrichting [een SQL Server-machine inrichten in de Azure Portal](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Bestaande virtuele machines
Selecteer de virtuele machine van SQL Server voor een bestaande SQL Server-virtuele machines. Selecteer vervolgens de **SQL Server-configuratiebestand** sectie van de **instellingen** blade.

![SQL Azure Sleutelkluis-integratie voor bestaande virtuele machines](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

In de **SQL Server-configuratiebestand** blade, klikt u op de **bewerken** knop in de sectie geautomatiseerde Sleutelkluis-integratie.

![SQL Azure Sleutelkluis-integratie voor bestaande virtuele machines configureren](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Wanneer u klaar bent, klikt u op de **OK** knop aan de onderkant van de **SQL Server-configuratiebestand** blade uw wijzigingen op te slaan.

> [!NOTE]
> U kunt ook Azure Sleutelkluis-integratie met een sjabloon configureren. Zie voor meer informatie [Azure quickstart-sjabloon voor Azure Sleutelkluis-integratie](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).
> 
> 

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

