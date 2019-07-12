---
title: SQL Server-VM's in Azure met behulp van de Azure-portal beheren | Microsoft Docs
description: Leer hoe u toegang tot de SQL-VM-resource in Azure portal voor een SQL Server-VM die wordt gehost op Azure.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 59a85e855c9fab9f2a3437c83c867b8076f55049
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607209"
---
# <a name="manage-sql-server-vms-in-azure-using-the-azure-portal"></a>SQL Server-VM's in Azure met behulp van de Azure-portal beheren

Er is een nieuw toegangspunt voor het beheren van uw SQL Server-VM op Azure met de [Azure-portal](https://portal.azure.com). 

De **SQL virtuele machines** resource is nu een onafhankelijke management-service waarmee u al uw SQL Server-VM's tegelijkertijd weergeven en wijzigen van instellingen die zijn toegewezen aan SQL Server: 

![SQL-resource voor virtuele machines](media/virtual-machines-windows-sql-manage-portal/sql-vm-manage.png)


## <a name="remarks"></a>Opmerkingen

- De **SQL virtuele machines** bron is de aanbevolen methode om te bekijken en beheren van uw SQL Server-VM's. Echter, momenteel, de **SQL virtuele machines** resource biedt geen ondersteuning voor het beheer van [einde van ondersteuning (EOS)](virtual-machines-windows-sql-server-2008-eos-extend-support.md) SQL Server-VM's. Voor het beheren van instellingen voor uw EOS SQL Server-VM's, gebruikt u de afgeschafte [tabblad voor SQL Server-configuratie](#access-sql-server-configuration-tab) in plaats daarvan. 
- De **SQL virtuele machines** resource is alleen beschikbaar voor SQL Server-VM's waarvoor [geregistreerd bij de resourceprovider van de SQL-VM](virtual-machines-windows-sql-register-with-resource-provider.md). 


## <a name="access-sql-virtual-machine-resource"></a>Toegang tot SQL VM-resource
Voor toegang tot de SQL-bron voor virtuele machines, het volgende doen:

1. Open de [Azure Portal](https://portal.azure.com). 
1. Selecteer **alle Services**. 
1. Type `SQL virtual machines` in het zoekvak in.
1. (Optioneel): Selecteer de ster naast **SQL virtuele machines** deze optie aan uw Favorieten toevoegen. 
1. Selecteer **SQL virtuele machines**. 

   ![SQL-VM virtuele machines zoeken in alle services](media/virtual-machines-windows-sql-manage-portal/sql-vm-search.png)

1. Hiermee worden alle SQL Server-VM's beschikbaar zijn binnen het abonnement. Selecteer de versie die u wilt beheren om te starten de **SQL virtuele machines** resource. Gebruik het zoekvak als uw SQL Server-VM niet snel duidelijk is. 

![Alle beschikbare SQL-VM 's](media/virtual-machines-windows-sql-manage-portal/all-sql-vms.png)

Selecteren van uw SQL Server-VM wordt geopend de **SQL virtuele machines** resource: 


![SQL-resource voor virtuele machines](media/virtual-machines-windows-sql-manage-portal/sql-vm-resource.png)

  > [!TIP]
  > De **SQL virtuele machines** resource is bedoeld voor specifieke SQL Server-instellingen. Selecteer de naam van de virtuele machine in de **virtuele machine** veld om te navigeren naar de instellingen die specifiek zijn voor de virtuele machine, maar niet exclusief voor SQL Server zijn. 

## <a name="access-sql-server-configuration-tab"></a>Tabblad voor toegang tot SQL Server-configuratie
Het tabblad van de configuratie van SQL Server is afgeschaft. Op dit moment is dit de enige methode voor het beheren van [einde van ondersteuning (EOS)](virtual-machines-windows-sql-server-2008-eos-extend-support.md) SQL Server-VM's en SQL Server-VM's die niet zijn [geregistreerd bij de resourceprovider van de SQL-VM](virtual-machines-windows-sql-register-with-resource-provider.md).

Voor toegang tot de afgeschafte tabblad configuratie van SQL server, moet u navigeren naar de **virtuele machines** resource. Om dit te doen, het volgende doen:

1. Open de [Azure Portal](https://portal.azure.com). 
1. Selecteer **alle Services**. 
1. Type `virtual machines` in het zoekvak in.
1. (Optioneel): Selecteer de ster naast **virtuele machines** deze optie aan uw Favorieten toevoegen. 
1. Selecteer **virtuele machines**. 

   ![Zoeken naar virtuele machines](media/virtual-machines-windows-sql-manage-portal/vm-search.png)

1. Hiermee worden alle virtuele machines in het abonnement. Selecteer de versie die u wilt beheren om te starten de **virtuele machine** resource. Gebruik het zoekvak als uw SQL Server-VM niet snel duidelijk is. 
1. Selecteer **SQL Server-configuratie** in de **instellingen** deelvenster voor het beheren van uw SQL-Server. 

![SQL Server-configuratie](media/virtual-machines-windows-sql-manage-portal/sql-vm-configuration.png)

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer informatie de volgende artikelen: 

* [Overzicht van SQL Server op een Windows VM](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server op een Windows VM-Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server op een Windows-VM prijsinformatie](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server op een Windows VM release-opmerkingen](virtual-machines-windows-sql-server-iaas-release-notes.md)


