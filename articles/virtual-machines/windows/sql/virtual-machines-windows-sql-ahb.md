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
ms.date: 02/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 1fb67600ea01629e7bf3ab4c7c470e4727b0e923
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393177"
---
# <a name="how-to-change-the-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>Het wijzigen van de licentiemodel voor een virtuele machine van SQL Server in Azure
In dit artikel wordt beschreven hoe u de licentiemodel voor een virtuele machine van SQL Server in Azure met behulp van de nieuwe SQL-VM-resourceprovider - **Microsoft.SqlVirtualMachine**. Er zijn twee modellen voor een virtuele machine (VM) die als host fungeert voor SQL Server - betalen per gebruik, licenties en uw eigen licentie (BYOL). En nu, met de Azure portal, Azure CLI of PowerShell kunt u wijzigen welke licentiemodel maakt gebruik van uw SQL Server-VM. 

De **betalen per gebruik** model (betalen per gebruik) betekent dat de kosten per seconde van het uitvoeren van de virtuele machine van Azure de kosten van de SQL Server-licentie bevat.

De **bring-your-own-license** (BYOL)-model is ook bekend als de [Azure Hybrid Benefit (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/), en kunt u uw eigen SQL Server-licentie gebruiken met een virtuele machine met SQL Server. Zie voor meer informatie over prijzen [prijzen voor SQL Server-VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance).

Schakelen tussen de twee licentiemodellen worden in rekening gebracht **zonder uitvaltijd**, de virtuele machine niet opnieuw wordt opgestart, wordt toegevoegd **zonder extra kosten** (in feite activeren AHB *vermindert* kosten) en **met onmiddellijke ingang effectief**. 

## <a name="remarks"></a>Opmerkingen


 - Azure Cloud Solution Partner (CSP)-klanten kunnen gebruikmaken van Azure Hybrid Benefit door eerst een betalen per gebruik virtuele machine te implementeren en vervolgens te converteren naar bring-your-own-license. 
 - Wanneer u een aangepaste SQL Server-VM-installatiekopie met de resourceprovider registreert, geeft u het licentietype = 'AHUB'. Typ leeg verlaten van de licentie of 'Betalen per gebruik' opgeeft, zal de registratie mislukt. 
 - Als u uw SQL Server-VM-resource, gaat u terug naar de vastgelegde licentie-instelling van de installatiekopie. 
 - Een SQL Server-VM toevoegen aan een beschikbaarheidsset is vereist dat de virtuele machine opnieuw wordt gemaakt. Als dergelijke ziet u alle virtuele machines die zijn toegevoegd aan een beschikbaarheid set wordt gaat u terug naar het standaardtype voor betalen per gebruik-licentie en AHB moet opnieuw worden ingeschakeld. 
 - De mogelijkheid om te wijzigen van de licentiemodel is een functie van de SQL-VM-resourceprovider. Automatisch implementeren van een marketplace-installatiekopie via de Azure-portal, wordt een virtuele machine van SQL Server geregistreerd bij de resourceprovider. Klanten die SQL Server zelf installeert wordt moet echter handmatig [registreren hun SQL Server-VM](#register-sql-server-vm-with-the-sql-vm-resource-provider). 
 

 
## <a name="limitations"></a>Beperkingen

 - De mogelijkheid om het licentiemodel te converteren is momenteel alleen beschikbaar voor een VM-installatiekopie van SQL Server op basis van betalen per gebruik. Als u een BYOL-installatiekopie (Bring Your Own License) hebt, kunt u die installatiekopie niet converteren naar betalen per gebruik.
 - Wijzigen van de licentiemodel wordt alleen ondersteund voor virtuele machines die zijn geïmplementeerd met behulp van de Resource Manager-model. Virtuele machines die zijn geïmplementeerd met behulp van het klassieke model, worden niet ondersteund. 
 - Wijzigen van de licentiemodel is alleen ingeschakeld voor installaties van de openbare Cloud.
 - Wijzigen van de licentiemodel wordt alleen ondersteund op virtuele machines met één NIC (network interface). Op virtuele machines met meer dan één NIC, u moet eerst een verwijderen van de NIC's (met behulp van de Azure-portal) voordat u de procedure. Anders wordt u uitvoeren in een vergelijkbaar met de volgende fout: `The virtual machine '\<vmname\>' has more than one NIC associated.` Hoewel u mogelijk de NIC terug naar de virtuele machine toevoegen nadat u de licentiemodus wijzigen, bewerkingen die via de blade SQL-configuratie, zoals automatische toepassing van patches en back-up wordt niet meer worden beschouwd als ondersteund.

## <a name="prerequisites"></a>Vereisten

Het gebruik van de resourceprovider van SQL-VM moet de SQL IaaS-extensie. Als u wilt doorgaan met het gebruik van de resourceprovider van SQL-VM, moet u als zodanig het volgende:
- Een [Azure-abonnement](https://azure.microsoft.com/free/).
- [Software assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default). 
- Een *betalen per gebruik* [SQL Server-VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) met de [SQL IaaS-extensie](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension) geïnstalleerd. 

## <a name="with-the-azure-portal"></a>Met de Azure Portal

U kunt de licentiemodel rechtstreeks vanuit de portal wijzigen. 

1. Navigeer naar uw SQL Server-machine binnen de [Azure-portal](https://portal.azure.com). 
1. Selecteer **SQL Server-configuratie** in de **instellingen** deelvenster. 
1. Selecteer **bewerken** in de **SQL Server-licentie** deelvenster te wijzigen van de licentie. 

![AHB in Portal](media/virtual-machines-windows-sql-ahb/ahb-in-portal.png)

  >[!NOTE]
  > Deze optie is niet beschikbaar voor bring-your-own-license-installatiekopieën. 

## <a name="with-azure-cli"></a>Met Azure CLI

Azure CLI kunt u uw licentiemodel wijzigen.  

Uw licentiemodel voor betalen per gebruik verandert het volgende codefragment in een BYOL (of met behulp van Azure Hybrid Benefit):

```azurecli
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
# example: az sql vm update -n AHBTest -g AHBTest --license-type AHUB

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type AHUB
```

Het volgende codefragment wordt het model bring-your-own-license naar betalen per gebruik: 

```azurecli
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
# example: az sql vm update -n AHBTest -g AHBTest --license-type PAYG

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type PAYG
```
## <a name="with-powershell"></a>Met PowerShell
U kunt PowerShell gebruiken om te wijzigen van uw licentiemodel.

Uw licentiemodel voor betalen per gebruik verandert het volgende codefragment in een BYOL (of met behulp van Azure Hybrid Benefit):

```powershell
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
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

```powershell
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="PAYG"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzResource -Force 
```


## <a name="register-sql-server-vm-with-the-sql-vm-resource-provider"></a>SQL Server-VM met de SQL-VM-resourceprovider registreren
In bepaalde situaties moet u mogelijk handmatig registreren van uw SQL Server-VM met de SQL-VM-resourceprovider. Om dit te doen, moet u mogelijk ook handmatig de resourceprovider te registreren bij uw abonnement. 


### <a name="register-sql-vm-resource-provider-with-subscription"></a>SQL-VM-resourceprovider registreren met abonnement 

Voor het registreren van uw SQL Server-VM met de SQL-resourceprovider, moet u de resourceprovider registreren bij uw abonnement. U kunt dit doen met de Azure portal of Azure CLI. 

#### <a name="with-the-azure-portal"></a>Met de Azure Portal
De SQL-resourceprovider aan uw Azure-abonnement met behulp van de Azure portal, de volgende stappen wordt geregistreerd. 

1. Open de Azure-portal en Ga naar **alle Services**. 
1. Navigeer naar **abonnementen** en selecteer het abonnement van belang zijn.  
1. In de **abonnementen** blade, gaat u naar **resourceproviders**. 
1. Type `sql` in het filter om de SQL-gerelateerde resourceproviders. 
1. Selecteer een *registreren*, *opnieuw registreren*, of *registratie* voor de **Microsoft.SqlVirtualMachine** provider afhankelijk van uw gewenste actie. 

   ![De provider wijzigen](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

#### <a name="with-azure-cli"></a>Met Azure CLI
Het volgende codefragment wordt de resourceprovider van SQL-VM naar uw Azure-abonnement registreren. 

```azurecli
# Register the new SQL resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

#### <a name="with-powershell"></a>Met PowerShell

Het volgende codefragment wordt de SQL-resourceprovider aan uw Azure-abonnement registreren.

```powershell
# Register the new SQL resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```

### <a name="register-sql-server-vm-with-sql-resource-provider"></a>SQL Server-VM met SQL-resourceprovider registreren
Zodra de SQL-VM-resourceprovider is geregistreerd bij uw abonnement, kunt u uw SQL Server-VM vervolgens registreren met de resourceprovider met behulp van Azure CLI. 

#### <a name="with-azure-cli"></a>Met Azure CLI
Registreer SQL Server-VM met behulp van Azure CLI met het volgende codefragment: 

```azurecli
# Register your existing SQL Server VM with the new resource provider
az sql vm create -n <VMName> -g <ResourceGroupName> -l <VMLocation>
```
#### <a name="with-powershell"></a>Met PowerShell
Registreer SQL Server-VM met behulp van PowerShell met het volgende codefragment:

```powershell
# Register your existing SQL Server VM with the new resource provider
# example: $vm=Get-AzVm -ResourceGroupName AHBTest -Name AHBTest
$vm=Get-AzVm -ResourceGroupName <ResourceGroupName> -Name <VMName>
New-AzResource -ResourceName $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines -Properties @{virtualMachineResourceId=$vm.Id}
```



## <a name="known-errors"></a>Bekende problemen

### <a name="sql-iaas-extension-is-not-installed-on-virtual-machine"></a>SQL IaaS-extensie is niet geïnstalleerd op de virtuele Machine
De SQL IaaS-extensie is een vereiste die nodig zijn voor het registreren van uw SQL Server-VM met de SQL-VM-resourceprovider. Als u probeert te registreren van uw SQL Server-VM voordat u de SQL IaaS-extensie installeert, wordt u de volgende fout optreden:

`Sql IaaS Extension is not installed on Virtual Machine: '{0}'. Please make sure it is installed and in running state and try again later.`

U lost dit probleem, de SQL IaaS-extensie te installeren voordat u probeert te registreren van uw SQL Server-VM. 

  > [!NOTE]
  > Installeren van de SQL IaaS extensie wordt opnieuw opgestart voor de SQL Server-service en moet alleen worden uitgevoerd tijdens een onderhoudsvenster. Zie voor meer informatie, [installatie van de SQL IaaS-extensie](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension#installation). 


### <a name="the-resource-microsoftsqlvirtualmachinesqlvirtualmachinesresource-group-under-resource-group-resource-group-was-not-found-the-property-sqlserverlicensetype-cannot-be-found-on-this-object-verify-that-the-property-exists-and-can-be-set"></a>De Resource ' Microsoft.SqlVirtualMachine/SqlVirtualMachines/\<resource-group >' onder de resourcegroep '\<resource-group >' is niet gevonden. De eigenschap 'sqlServerLicenseType' kan niet worden gevonden voor dit object. Controleer of de eigenschap bestaat en kan worden ingesteld.
Deze fout treedt op wanneer u probeert te wijzigen van de licentiemodel op een SQL Server-VM die is niet geregistreerd bij de SQL-resourceprovider. U moet de resourceprovider te registreren uw [abonnement](#register-sql-vm-resource-provider-with-subscription), en registreert u uw SQL Server-VM met de SQL [resourceprovider](#register-sql-server-vm-with-sql-resource-provider). 

### <a name="cannot-validate-argument-on-parameter-sku"></a>Argument voor parameter 'Sku' kan niet worden gevalideerd.
U kunt deze fout kan optreden bij het wijzigen van uw SQL Server-VM-licentiemodel wanneer u Azure PowerShell > 4.0: Set-AzResource : Kan het argument 'Sku-parameter niet valideren. Het argument is null of leeg zijn. Geef een argument dat niet null of leeg zijn en probeer het vervolgens opnieuw de opdracht.
U kunt deze fout oplossen, #opmerkingen op deze regels in de eerder genoemde PowerShell-codefragment wanneer u overschakelt van uw licentiemodel:

```powershell
# the following code snippet is necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new()
```

Gebruik de volgende code om te controleren of uw versie van Azure PowerShell:

```powershell
Get-Module -ListAvailable -Name Azure -Refresh
```

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer informatie de volgende artikelen: 

* [Overzicht van SQL Server op een Windows VM](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server op een Windows VM-Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server op een Windows-VM prijsinformatie](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server op een Windows VM release-opmerkingen](virtual-machines-windows-sql-server-iaas-release-notes.md)


