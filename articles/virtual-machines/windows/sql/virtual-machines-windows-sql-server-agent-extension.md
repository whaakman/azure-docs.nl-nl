---
title: Automatiseer beheertaken op Azure Virtual Machines met de SQL Server IaaS Agent-extensie | Microsoft Docs
description: Dit artikel wordt beschreven hoe u voor het beheren van de SQL Server agent-extensie, welke specifieke SQL Server-beheertaken worden geautomatiseerd. Het gaat hierbij om automatische back-up, automatisch patchen en integratie van Azure Sleutelkluis.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: jroth
editor: ''
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/24/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 41023103dc30d16f599e847f9d324bc7bb4be11c
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798057"
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-iaas-agent-extension"></a>Automatiseer beheertaken op Azure Virtual Machines met de SQL Server IaaS Agent-extensie
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Klassiek](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md)

De SQL Server IaaS Agent-extensie (SqlIaasExtension) wordt uitgevoerd op virtuele Azure-machines beheertaken te automatiseren. Dit artikel bevat een overzicht en de services die worden ondersteund door de extensie, evenals de instructies voor installatie, status en verwijdering.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Voor de klassieke versie van dit artikel, raadpleegt u [SQL Server Agent-extensie voor SQL Server-VM's klassieke](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md).

Er zijn drie SQL beheerbaarheid modi voor de SQL IaaS-extensie: **Volledige**, **Lightweight**, en **NoAgent**. 

- **Volledige** modus voorziet in alle functionaliteit, maar een herstart van de SQL Server en de SA-machtigingen is vereist. Dit is de optie die wordt standaard geïnstalleerd en moet worden gebruikt voor het beheren van een SQL Server-VM met één exemplaar. 

- **Lightweight** is niet vereist voor het opnieuw opstarten van SQL Server, maar biedt alleen ondersteuning voor wijzigen van het licentietype en editie van SQL Server. Deze optie moet worden gebruikt voor SQL Server-VM's met meerdere exemplaren, of deelnemen aan een failover clusterexemplaar (FCI). 

- **NoAgent** is toegewezen voor SQL Server 2008 en SQL Server 2008 R2 is geïnstalleerd op Windows Server 2008. Voor informatie over het gebruik van `NoAgent` modus voor uw Windows Server 2008-installatiekopie, Zie [registratie van Windows Server 2008](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-server-2008r2-on-windows-server-2008-vms). 

## <a name="supported-services"></a>Ondersteunde services
De SQL Server IaaS Agent-extensie ondersteunt de volgende beheertaken:

| Beheer-functie | Description |
| --- | --- |
| **Automatische back-up van SQL** |Automatiseert de planning van de back-ups voor alle databases voor het standaardexemplaar of een [juist is geïnstalleerd](virtual-machines-windows-sql-server-iaas-faq.md#administration) benoemd exemplaar van SQL Server op de virtuele machine. Zie voor meer informatie, [automatische back-up voor SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md). |
| **Automatisch patchen van SQL** |Hiermee configureert u een onderhoudsvenster waarin belangrijke updates voor Windows met uw virtuele machine plaatsvinden kunnen, zodat u dat updates tijdens piektijden voor uw workload voorkomen kunt. Zie voor meer informatie, [automatische patches voor SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md). |
| **Integratie van Azure Key Vault** |Hiermee kunt u automatisch installeren en configureren van Azure Key Vault op uw SQL Server-VM. Zie voor meer informatie, [configureren van Azure Key Vault-integratie voor SQL Server op Azure Virtual machines (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md). |

Eenmaal geïnstalleerd en wordt uitgevoerd, beschikbaar de SQL Server IaaS Agent-extensie deze beheerfuncties van SQL Server in het venster van de virtuele machine in Azure portal en via Azure PowerShell voor SQL Server-marketplace-installatiekopieën en via Azure PowerShell voor handmatige installaties van de extensie. 

## <a name="prerequisites"></a>Vereisten
Vereisten voor het gebruik van de SQL Server IaaS Agent-extensie op de virtuele machine:

**Besturingssysteem**:

* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016
* Windows Server 2019 

**SQL Server-versies**:

* SQL Server 2008 
* SQL Server 2008 R2
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016
* SQL Server 2017

**Azure PowerShell**:

* [Downloaden en configureren van de meest recente Azure PowerShell-opdrachten](/powershell/azure/overview)

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]


## <a name="change-management-modes"></a>Modi voor beheer wijzigen

U kunt de huidige modus van uw SQL IaaS-agent bekijken met behulp van PowerShell: 

  ```powershell-interactive
     //Get the SqlVirtualMachine
     $sqlvm = Get-AzResource -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName  -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines
     $sqlvm.Properties.sqlManagement
  ```

Voor SQL Server-VM's waarvoor de *NoAgent* of *lichtgewicht* IaaS-extensie is geïnstalleerd, kunt u de modus voor upgraden *volledige* met behulp van de Azure portal. Het is niet mogelijk om te downgraden - Als u wilt doen, moet u volledig is de SQL IaaS-extensie verwijderen en opnieuw installeren. 

Naar de modus upgrade-agent *volledige*, doet u het volgende: 

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).
1. Navigeer naar uw [SQL virtuele machines](virtual-machines-windows-sql-manage-portal.md#access-sql-virtual-machine-resource) resource. 
1. Selecteer uw SQL Server virtuele machine en selecteer **overzicht**. 
1. Voor SQL-VM's met de *NoAgent* of *lichtgewicht* IaaS-modi, selecteert u het bericht voor **alleen updates voor het type en de editie van licentie beschikbaar zijn met de SQL IaaS-extensie**.

    ![Modus wijzigen vanuit de portal starten](media/virtual-machines-windows-sql-server-agent-extension/change-sql-iaas-mode-portal.png)

1. Ga akkoord met **Start de SQL Server-service opnieuw** selecteren door het selectievakje in en selecteer vervolgens **bevestigen** om bij te werken uw IaaS-modus op 'full'. 

    ![Volledig beheer voor IaaS-extensie inschakelen](media/virtual-machines-windows-sql-server-agent-extension/enable-full-mode-iaas.png)

##  <a name="installation"></a>Installatie
De SQL IaaS-extensie wordt geïnstalleerd tijdens de registratie van uw SQL Server-VM met de [SQL-VM-resourceprovider](virtual-machines-windows-sql-register-with-resource-provider.md#register-with-sql-vm-resource-provider). Echter, indien nodig, de SQL IaaS-agent kan ook worden geïnstalleerd handmatig met behulp van *volledige* of *lichtgewicht* modusinstallatie. 

De *volledige* SQL Server IaaS Agent-extensie wordt automatisch geïnstalleerd wanneer u een van de SQL Server galerie met installatiekopieën van virtuele machines met behulp van de Azure portal inrichten. 

### <a name="full-mode-installation"></a>Volledige modusinstallatie
De *volledige* SQL IaaS-extensie biedt volledig beheer voor één exemplaar van de SQL Server-VM. Als er een standaardexemplaar, klikt u vervolgens de extensie werkt met het standaardexemplaar en er geen ondersteuning voor het beheren van andere exemplaren. Als er geen standaard-exemplaar, maar slechts één benoemd exemplaar, wordt deze het benoemde exemplaar beheren. Als er geen standaardexemplaar is en er meerdere genoemde instanties zijn, mislukt de extensie te installeren. 

Installeren van de *volledige* modus van de SQL IaaS Start de SQL Server-service opnieuw. Om te voorkomen dat de SQL Server-service opnieuw te starten, installeert de *lichtgewicht* modus met beperkte beheermogelijkheden in plaats daarvan. 

Installeer SQL IaaS-agent met *volledige* modus met PowerShell:

  ```powershell-interactive
     // Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     // Register SQL VM with 'Full' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;sqlServerLicenseType='AHUB';sqlManagement='Full'}  
  
  ```

| Parameter | Acceptabele waarden                        |
| :------------------| :-------------------------------|
| **sqlServerLicenseType** | `'AHUB'`, of `'PAYG'`     |
| &nbsp;             | &nbsp;                          |


> [!WARNING]
> - Als de extensie niet al is geïnstalleerd, installeert de **volledige** extensie Start de SQL Server-service opnieuw. Gebruik **lichtgewicht** modus om te voorkomen dat de SQL Server-service opnieuw te starten. 
> - De SQL IaaS-extensie wordt bijgewerkt, wordt de SQL Server-service niet opnieuw opstarten. 

#### <a name="install-on-a-vm-with-a-single-named-sql-server-instance"></a>Installeren op een virtuele machine in een enkele benoemd exemplaar van SQL Server
De SQL IaaS-extensie werkt in een benoemd exemplaar op een SQL Server als het standaardexemplaar is verwijderd en de IaaS-uitbreiding opnieuw is geïnstalleerd.

Voor het gebruik van een benoemd exemplaar van SQL Server, het volgende doen:
   1. Implementeer een SQL Server-VM vanuit de marketplace. 
   1. De IaaS-extensie verwijderen vanuit de [Azure-portal](https://portal.azure.com).
   1. Verwijder de SQL-Server volledig in de SQL Server-VM.
   1. Installeer SQL Server in een benoemd exemplaar in de SQL Server-VM. 
   1. Installeer de IaaS-extensie van de Azure-portal.  


### <a name="install-in-lightweight-mode"></a>Installeren in de lichte modus
Lichte modus worden niet opnieuw opgestart uw SQL Server-service, maar biedt beperkte functionaliteit. 

Installeer SQL IaaS-agent met *lichtgewicht* modus met PowerShell:


  ```powershell-interactive
     // Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     // Register SQL VM with 'Lightweight' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;sqlServerLicenseType='AHUB';sqlManagement='LightWeight'}  
  
  ```

| Parameter | Acceptabele waarden                        |
| :------------------| :-------------------------------|
| **sqlServerLicenseType** | `'AHUB'`, of `'PAYG'`     |
| &nbsp;             | &nbsp;                          |


## <a name="get-status-of-sql-iaas-extension"></a>Status van SQL IaaS-extensie ophalen
Er is één manier om te controleren of de extensie is geïnstalleerd om de agentstatus in Azure portal weer te geven. Selecteer **alle instellingen** in het venster van de virtuele machine en klik vervolgens op **extensies**. U ziet de **SqlIaasExtension** extensie is die weergegeven.

![SQL Server IaaS Agent-extensie in Azure portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

U kunt ook de **Get-AzVMSqlServerExtension** Azure PowerShell-cmdlet.

   ```powershell-interactive
   Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
   ```

De vorige opdracht wordt bevestigd dat de agent is geïnstalleerd en biedt algemene statusinformatie. U kunt ook specifieke statusinformatie over automatische back-up en patchen met de volgende opdrachten ophalen.

   ```powershell-interactive
    $sqlext = Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings
   ```

## <a name="removal"></a>Verwijderen
In de Azure-portal, kunt u de extensie verwijderen door te klikken op het beletselteken op de **extensies** venster van de eigenschappen van uw virtuele machine. Klik vervolgens op **Verwijderen**.

![Verwijderen van de SQL Server IaaS Agent-extensie in Azure portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

U kunt ook de **Remove-AzVMSqlServerExtension** PowerShell-cmdlet.

   ```powershell-interactive
    Remove-AzVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SqlIaasExtension"
   ```

## <a name="next-steps"></a>Volgende stappen
Beginnen met een van de services die worden ondersteund door de extensie. Zie voor meer informatie de artikelen waarnaar wordt verwezen in de [ondersteunde services](#supported-services) sectie van dit artikel.

Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual Machines [SQL Server op Azure Virtual Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md).

