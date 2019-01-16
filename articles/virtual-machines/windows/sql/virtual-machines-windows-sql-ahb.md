---
title: Het wijzigen van de licentiemodel voor een SQL-VM in Azure | Microsoft Docs
description: Informatie over het wijzigen van de licentieverlening voor een SQL-VM in Azure.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/14/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: c516bf9b48164f2ef8dc7fea6fb834bdae00a0d1
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54332146"
---
# <a name="how-to-change-the-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>Het wijzigen van de licentiemodel voor een virtuele machine van SQL Server in Azure
In dit artikel wordt beschreven hoe u de licentiemodel voor een virtuele machine van SQL Server in Azure met behulp van de nieuwe SQL-resourceprovider - **Microsoft.SqlVirtualMachine**. Er zijn twee modellen voor een virtuele machine (VM) die als host fungeert voor SQL Server - betalen per gebruik, licenties en uw eigen licentie (BYOL). En nu met behulp van PowerShell of Azure CLI, u kunt wijzigen welke licentiemodel maakt gebruik van uw SQL-VM. 

De **betalen per gebruik** model betekent dat de kosten per seconde van het uitvoeren van de virtuele machine van Azure de kosten van de SQL Server-licentie bevat.

De **Bring-your-own-license** model wordt ook wel bekend als de [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/), en kunt u uw eigen SQL Server-licentie gebruiken met een virtuele machine met SQL Server. Zie voor meer informatie over prijzen [SQL VM prijzen handleiding](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance).

Schakelen tussen de twee licentiemodellen worden in rekening gebracht **zonder uitvaltijd**, de virtuele machine niet opnieuw wordt opgestart, wordt toegevoegd **zonder extra kosten** (in feite AHB activering vermindert de kosten) en **met onmiddellijke ingang effectief**. 

## <a name="prerequisites"></a>Vereisten
Het gebruik van de resourceprovider van SQL-VM moet de SQL IaaS-extensie. Als u wilt doorgaan met het gebruik van de resourceprovider van SQL-VM, moet u als zodanig het volgende:
- Een [Azure-abonnement](https://azure.microsoft.com/free/).
- Een [SQL Server-VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) met de [SQL IaaS-extensie](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension) geïnstalleerd. 

## <a name="register-existing-sql-vm-with-new-resource-provider"></a>Bestaande SQL-VM met nieuwe resourceprovider registreren
De mogelijkheid om over te schakelen tussen licentiemodellen is een functie die wordt geleverd door de nieuwe SQL-VM resourceprovider (Microsoft.SqlVirtualMachine). Op dit moment als u wilt overschakelen van uw licentiemodel moet u eerst het registreren van de nieuwe provider voor uw abonnement en vervolgens uw bestaande virtuele machine met de nieuwe SQL-VM-resourceprovider te registreren. Als u wilt gebruikmaken van de resourceprovider van SQL-VM, moet u ook de SQL IaaS-extensie installeren. In dat geval kunt u voor het registreren van een virtuele machine die is geïmplementeerd met een VHD. Zie voor meer informatie, [SQL IaaS-extensie](virtual-machines-windows-sql-server-agent-extension.md). 

  >[!IMPORTANT]
  > Als u uw SQL-VM-resource, gaat u terug naar de vastgelegde licentie-instelling van de installatiekopie. 
  

### <a name="powershell"></a>PowerShell


Het volgende codefragment wordt u verbinding maken met Azure en controleer of welke abonnements-ID die u gebruikt. 
```PowerShell
# Connect to Azure
Connect-AzureRmAccount
Account: <account_name>

# Verify your subscription ID
Get-AzureRmContext

# Set the correct Azure Subscription ID
Set-AzureRmContext -SubscriptionId <Subscription_ID>
```

Het volgende codefragment eerst registreert de nieuwe SQL-resourceprovider voor uw abonnement en vervolgens uw bestaande SQL-VM voor de nieuwe resourceprovider geregistreerd. 

```powershell
# Register the new SQL resource provider for your subscription
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine

# Register your existing SQL VM with the new resource provider
# example: $vm=Get-AzureRmVm -ResourceGroupName AHBTest -Name AHBTest
$vm=Get-AzureRmVm -ResourceGroupName <ResourceGroupName> -Name <VMName>
New-AzureRmResource -ResourceName $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines -Properties @{virtualMachineResourceId=$vm.Id}
```

### <a name="portal"></a>Portal
U kunt ook de nieuwe SQL-VM-resourceprovider met behulp van de portal registreren. Op dit als volgt te werk:
1. Open de Azure-portal en Ga naar **alle Services**. 
1. Navigeer naar **abonnementen** en selecteer het abonnement van belang zijn.  
1. In de **abonnementen** blade, gaat u naar **Resourceprovider**. 
1. Type `sql` in het filter om de SQL-gerelateerde resourceproviders. 
1. Selecteer een *registreren*, *opnieuw registreren*, of *registratie* voor de **Microsoft.SqlVirtualMachine** provider afhankelijk van uw gewenste actie. 

  ![De provider wijzigen](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)


## <a name="use-powershell"></a>PowerShell gebruiken 
U kunt PowerShell gebruiken om te wijzigen van uw licentiemodel.  Zorg ervoor dat uw SQL-VM is al geregistreerd met de nieuwe SQL-resourceprovider voordat u overschakelt van de licentiemodel. 

Uw licentiemodel voor betalen per gebruik verandert het volgende codefragment in een BYOL (of met behulp van Azure Hybrid Benefit): 
```PowerShell
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="AHUB"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzureRmResource -Force 
``` 

Het volgende codefragment verandert de BYOL-model voor betalen per gebruik:
```PowerShell
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="PAYG"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzureRmResource -Force 
```

  >[!NOTE]
  > Als u wilt overschakelen tussen licenties, moet u de nieuwe SQL-VM-resourceprovider. Als u probeert deze opdrachten uitvoeren voordat u uw SQL-VM met de nieuwe provider registreert, kunt u deze fout kan optreden: `Get-AzureRmResource : The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. The property 'sqlServerLicenseType' cannot be found on this object. Verify that the property exists and can be set. ` Als u deze fout ziet, neemt u [registreren van uw SQL-VM met de nieuwe resourceprovider](#register-existing-SQL-vm-with-new-resource-provider). 
 

## <a name="use-azure-cli"></a>Azure CLI gebruiken
Azure CLI kunt u uw licentiemodel wijzigen.  Zorg ervoor dat uw SQL-VM is al geregistreerd met de nieuwe SQL-resourceprovider voordat u overschakelt van de licentiemodel. 

Uw licentiemodel voor betalen per gebruik verandert het volgende codefragment in een BYOL (of met behulp van Azure Hybrid Benefit):
```azurecli
az resource update -g <resource_group_name> -n <sql_virtual_machine_name> --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=AHUB
# example: az resource update -g AHBTest -n AHBTest --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=AHUB
```

Het volgende codefragment verandert de BYOL-model voor betalen per gebruik: 
```azurecli
az resource update -g <resource_group_name> -n <sql_virtual_machine_name> --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=PAYG
# example: az resource update -g AHBTest -n AHBTest --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=PAYG
```

  >[!NOTE]
  >Als u wilt overschakelen tussen licenties, moet u de nieuwe SQL-VM-resourceprovider. Als u probeert deze opdrachten uitvoeren voordat u uw SQL-VM met de nieuwe provider registreert, kunt u deze fout kan optreden: `The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. ` Als u deze fout ziet, neemt u [registreren van uw SQL-VM met de nieuwe resourceprovider](#register-existing-SQL-vm-with-new-resource-provider). 

## <a name="view-current-licensing"></a>Huidige licentie weergeven 

Het volgende codefragment kunt u uw huidige licentiemodel weergeven voor uw SQL-VM. 

```PowerShell
# View current licensing model for your SQL VM
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType
```

## <a name="known-errors"></a>Bekende problemen

### <a name="sql-iaas-extension-is-not-installed-on-virtual-machine"></a>SQL IaaS-extensie is niet geïnstalleerd op de virtuele Machine
De SQL IaaS-extensie is een vereiste die nodig zijn voor het registreren van uw SQL Server-VM met de SQL-VM-resourceprovider. Als u probeert te registreren van uw SQL Server-VM voordat u de SQL IaaS-extensie installeert, wordt u de volgende fout optreden:

`Sql IaaS Extension is not installed on Virtual Machine: '{0}'. Please make sure it is installed and in running state and try again later.`

U lost dit probleem, de SQL IaaS-extensie te installeren voordat u probeert te registreren van uw SQL Server-VM. 

  > [!NOTE]
  > Installeren van de SQL IaaS extensie wordt opnieuw opgestart voor de SQL Server-service en moet alleen worden uitgevoerd tijdens een onderhoudsvenster. Zie voor meer informatie, [installatie van de SQL IaaS-extensie](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension#installation). 

### <a name="cannot-validate-argument-on-parameter-sku"></a>Argument voor parameter 'Sku' kan niet worden gevalideerd.
U kunt deze fout kan optreden bij het wijzigen van uw SQL Server-VM-licentiemodel wanneer u Azure PowerShell > 4.0:

`Set-AzureRmResource : Cannot validate argument on parameter 'Sku'. The argument is null or empty. Provide an argument that is not null or empty, and then try the command again.`

U kunt deze fout oplossen, #opmerkingen op deze regels in de eerder genoemde PowerShell-codefragment wanneer u overschakelt van uw licentiemodel: 
```PowerShell
# the following code snippet is necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new()
```

Gebruik de volgende code om te controleren of uw versie van Azure PowerShell:

```PowerShell
Get-Module -ListAvailable -Name Azure -Refresh
```

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer informatie de volgende artikelen: 

* [Overzicht van SQL Server op een Windows VM](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server op een Windows VM-Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server op een Windows-VM prijsinformatie](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server op een Windows VM release-opmerkingen](virtual-machines-windows-sql-server-iaas-release-notes.md)


