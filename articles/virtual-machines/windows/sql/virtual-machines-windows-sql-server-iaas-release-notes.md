---
title: SQL Server op Azure VM-Release-opmerkingen | Microsoft Docs
description: Meer informatie over de nieuwe functies en verbeteringen van SQL Server op een Azure-VM
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/13/2018
ms.author: mathoma
ms.openlocfilehash: 44aee447c7f935cd67ca55902c53c5f6f9eb6fda
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52500303"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>SQL Server op de opmerkingen bij de release van de virtuele Machine van Azure

Met Azure kunt u een virtuele machine met een installatiekopie van SQL Server ingebouwde implementeren. In dit artikel bevat de nieuwe functies en verbeteringen die u in de meest recente versie van SQL Server is geïmplementeerd op een virtuele machine van Azure verwachten kunt. 


## <a name="november-2018"></a>November 2018
- **Nieuwe SQL-resourceprovider**: Er is een nieuwe resourceprovider voor SQL-VM's die voor betere beheer van uw virtuele machine zorgen. Zie voor meer informatie over het registreren van uw virtuele machine [oudere SQL-VM met nieuwe resourceprovider registreren](virtual-machines-windows-sql-ahb.md#register-legacy-sql-vm-with-new-resource-provider).
- **Schakel over licentiemodel**: U kunt nu schakelen tussen het licentiemodel met betalen per gebruik en bring-your-own-van uw SQL-VM met Azure CLI of Powershell. Zie voor meer informatie, [de licentiemodel voor een SQL-VM wijzigen](virtual-machines-windows-sql-ahb.md)



## <a name="additional-resources"></a>Aanvullende resources

**Windows-VM's**:

* [Overzicht van SQL Server op een Windows VM](virtual-machines-windows-sql-server-iaas-overview.md).
* [Een SQL Server Windows VM inrichten](virtual-machines-windows-portal-sql-server-provision.md)
* [Een Database migreren naar SQL Server op een Azure-VM](virtual-machines-windows-migrate-sql.md)
* [Hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md)
* [Aanbevolen procedures voor prestaties voor SQL Server op virtuele machines van Azure](virtual-machines-windows-sql-performance.md)
* [Toepassingspatronen en ontwikkelingsstrategieën voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Virtuele Linux-machines**:

* [Overzicht van SQL Server op een virtuele Linux-machine](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Een SQL Server Linux-VM inrichten](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [Veelgestelde vragen over (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Documentatie voor SQL Server op Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
