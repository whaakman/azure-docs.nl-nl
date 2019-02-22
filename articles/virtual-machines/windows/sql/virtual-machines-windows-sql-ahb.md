---
title: Het wijzigen van de licentiemodel voor SQL Server-VM in Azure | Microsoft Docs
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
ms.openlocfilehash: 0fbd15b1d29d877ad0066d01945389665126bbcc
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56649251"
---
# <a name="how-to-change-the-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>Het wijzigen van de licentiemodel voor een virtuele machine van SQL Server in Azure
In dit artikel wordt beschreven hoe u de licentiemodel voor een virtuele machine van SQL Server in Azure met behulp van de nieuwe SQL-VM-resourceprovider - **Microsoft.SqlVirtualMachine**. Er zijn twee modellen voor een virtuele machine (VM) die als host fungeert voor SQL Server - betalen per gebruik, licenties en uw eigen licentie (BYOL). En nu met behulp van PowerShell of Azure CLI, u kunt wijzigen welke licentiemodel maakt gebruik van uw SQL Server-VM. 

De **betalen per gebruik** model (betalen per gebruik) betekent dat de kosten per seconde van het uitvoeren van de virtuele machine van Azure de kosten van de SQL Server-licentie bevat.

De **Bring-your-own-license** (BYOL)-model is ook bekend als de [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/), en kunt u uw eigen SQL Server-licentie gebruiken met een virtuele machine met SQL Server. Zie voor meer informatie over prijzen [prijzen voor SQL Server-VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance).

Schakelen tussen de twee licentiemodellen worden in rekening gebracht **zonder uitvaltijd**, de virtuele machine niet opnieuw wordt opgestart, wordt toegevoegd **zonder extra kosten** (in feite activeren AHB *vermindert* kosten) en **met onmiddellijke ingang effectief**. 

  >[!NOTE]
  > - De mogelijkheid om te converteren van de licentiemodel is momenteel alleen beschikbaar bij het starten met een betalen per gebruik SQL Server-VM-installatiekopie. Als u met een bring-your-own-license-installatiekopie uit de portal begint, wordt het niet mogelijk om te converteren van die installatiekopie naar betalen per gebruik. 
  > - CSP-klanten kunnen gebruikmaken van het voordeel AHB door eerst een betalen per gebruik virtuele machine te implementeren en vervolgens te converteren naar bring-your-own-license. 
  > - Deze mogelijkheid is momenteel alleen ingeschakeld voor installaties van de openbare Cloud.


## <a name="prerequisites"></a>Vereisten
Het gebruik van de resourceprovider van SQL-VM moet de SQL IaaS-extensie. Als u wilt doorgaan met het gebruik van de resourceprovider van SQL-VM, moet u als zodanig het volgende:
- Een [Azure-abonnement](https://azure.microsoft.com/free/).
- Een [SQL Server-VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) met de [SQL IaaS-extensie](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension) geïnstalleerd. 


## <a name="register-existing-sql-server-vm-with-sql-resource-provider"></a>Bestaande SQL Server-VM met SQL-resourceprovider registreren
De mogelijkheid om over te schakelen tussen licentiemodellen is een functie die wordt geleverd door de nieuwe SQL-VM resourceprovider (Microsoft.SqlVirtualMachine). SQL Server-VM's geïmplementeerd nadat December 2018 worden automatisch geregistreerd bij de nieuwe resourceprovider. Bestaande virtuele machines die zijn geïmplementeerd voordat u deze datum moeten echter handmatig worden geregistreerd bij de resourceprovider voordat ze kunnen hun licentiemodel overschakelen. 

  > [!NOTE] 
  > Als u uw SQL-VM-resource, gaat u terug naar de vastgelegde licentie-instelling van de installatiekopie. 

### <a name="register-sql-resource-provider-with-your-subscription"></a>SQL-resourceprovider registreren bij uw abonnement 

Voor het registreren van uw SQL Server-VM met de SQL-resourceprovider, moet u de resourceprovider registreren bij uw abonnement. U kunt doen met PowerShell of met de Azure-portal. 

#### <a name="using-powershell"></a>PowerShell gebruiken
Het volgende codefragment wordt de SQL-resourceprovider geregistreerd met uw Azure-abonnement. 

```powershell
# Register the new SQL resource provider for your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```

#### <a name="using-azure-portal"></a>Azure Portal gebruiken
De volgende stappen wordt de SQL-resourceprovider geregistreerd met uw Azure-abonnement met behulp van de Azure portal. 

1. Open de Azure-portal en Ga naar **alle Services**. 
1. Navigeer naar **abonnementen** en selecteer het abonnement van belang zijn.  
1. In de **abonnementen** blade, gaat u naar **resourceproviders**. 
1. Type `sql` in het filter om de SQL-gerelateerde resourceproviders. 
1. Selecteer een *registreren*, *opnieuw registreren*, of *registratie* voor de **Microsoft.SqlVirtualMachine** provider afhankelijk van uw gewenste actie. 

  ![De provider wijzigen](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

### <a name="register-sql-server-vm-with-sql-resource-provider"></a>SQL Server-VM met SQL-resourceprovider registreren
Zodra de SQL-resourceprovider is geregistreerd bij uw abonnement, kunt u PowerShell gebruiken voor het registreren van uw SQL Server-VM met de SQL-resourceprovider. 


```powershell
# Register your existing SQL Server VM with the new resource provider
# example: $vm=Get-AzureRmVm -ResourceGroupName AHBTest -Name AHBTest
$vm=Get-AzureRmVm -ResourceGroupName <ResourceGroupName> -Name <VMName>
New-AzureRmResource -ResourceName $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines -Properties @{virtualMachineResourceId=$vm.Id}
```


## <a name="use-powershell"></a>PowerShell gebruiken 
U kunt PowerShell gebruiken om te wijzigen van uw licentiemodel.  Zorg ervoor dat uw virtuele SQL Server-machine is al geregistreerd met de nieuwe SQL-resourceprovider voordat u overschakelt van de licentiemodel. 

Uw licentiemodel voor betalen per gebruik verandert het volgende codefragment in een BYOL (of met behulp van Azure Hybrid Benefit): 
```PowerShell
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="AHUB"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzResource -Force 
``` 

Het volgende codefragment verandert de BYOL-model voor betalen per gebruik:
```PowerShell
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="PAYG"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzResource -Force 
```

  >[!NOTE]
  > Als u wilt overschakelen tussen licenties, moet u de nieuwe SQL-VM-resourceprovider. Als u probeert deze opdrachten uitvoeren voordat u uw SQL Server-VM met de nieuwe provider registreert, kunt u deze fout kan optreden: `Get-AzResource : The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. The property 'sqlServerLicenseType' cannot be found on this object. Verify that the property exists and can be set. ` Als u deze fout ziet, Registreer u uw SQL Server-VM met de nieuwe resourceprovider. 

 

## <a name="use-azure-cli"></a>Azure CLI gebruiken
Azure CLI kunt u uw licentiemodel wijzigen.  Zorg ervoor dat uw virtuele SQL Server-machine is al geregistreerd met de nieuwe SQL-resourceprovider voordat u overschakelt van de licentiemodel. 

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
  >Als u wilt overschakelen tussen licenties, moet u de nieuwe SQL-VM-resourceprovider. Als u probeert deze opdrachten uitvoeren voordat u uw SQL Server-VM met de nieuwe provider registreert, kunt u deze fout kan optreden: `The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. ` Als u deze fout ziet, neemt u [registreren van uw SQL Server-VM met de nieuwe resourceprovider](#register-existing-sql-server-vm-with-sql-resource-provider). 

## <a name="view-current-licensing"></a>Huidige licentie weergeven 

Het volgende codefragment kunt u uw huidige licentiemodel weergeven voor uw SQL Server-VM. 

```PowerShell
# View current licensing model for your SQL Server VM
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
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

`Set-AzResource : Cannot validate argument on parameter 'Sku'. The argument is null or empty. Provide an argument that is not null or empty, and then try the command again.`

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


