---
title: Licentie model voor een SQL Server virtuele machine in azure wijzigen
description: Meer informatie over het overschakelen van licenties voor een virtuele SQL-machine in azure van ' betalen naar gebruik ' naar ' uw eigen licentie ' met behulp van de Azure Hybrid Benefit.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/05/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 37457d8ce1189f9282f4763633e944e3c2d639c9
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816732"
---
# <a name="how-to-change-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>Het wijzigen van het licentie model voor een SQL Server virtuele machine in azure
In dit artikel wordt beschreven hoe u het licentie model voor een SQL Server virtuele machine in azure wijzigt met behulp van de nieuwe SQL VM-resource provider- **micro soft. SqlVirtualMachine**.

Er zijn twee licentie modellen voor een virtuele machine (VM) die als host fungeert voor SQL Server-betalen per gebruik en Azure Hybrid Benefit (AHB). En nu met behulp van de Azure Portal, Azure CLI of Power shell kunt u het licentie model van uw SQL Server virtuele machine wijzigen. 

Het payg-model ( **betalen naar gebruik** ) betekent dat de kosten per seconde van het uitvoeren van de Azure-VM de kosten van de SQL Server licentie bevatten.
Met de [Azure Hybrid Benefit (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/) kunt u uw eigen SQL Server licentie gebruiken met een VM waarop SQL Server wordt uitgevoerd. 

Met Microsoft Azure hybride voor deel voor SQL Server kunt u SQL Server licenties met Software Assurance (' gekwalificeerde licentie ') gebruiken in azure Virtual Machines. Met Azure Hybrid Benefit voor SQL Server worden klanten geen kosten in rekening gebracht voor het gebruik van SQL Server licentie op de virtuele machine, maar moeten ze wel betalen voor de kost prijs van de onderliggende Cloud Compute (dat wil zeggen, het basis tarief), de opslag en de back-ups, maar ook I/O gekoppeld aan de u de SE van de Services (indien van toepassing).

Volgens de voor waarden van micro soft-producten moeten klanten aangeven dat ze Azure SQL Database (beheerde instantie, Elastische pool en Individuele database), Azure Data Factory, SQL Server Integration Services of SQL Server Virtual Machines onder Azure Hybrid Profiteer van SQL Server bij het configureren van werk belastingen op Azure. "

Om het gebruik van de Azure Hybrid Benefit voor SQL Server op Azure VM aan te geven en compatibel te zijn, zijn er drie opties:

1. Richt een virtuele machine in met behulp van een BYOL SQL Server installatie kopie van Azure Marketplace, die alleen beschikbaar is voor klanten met Enterprise Agreement.
1. Richt een virtuele machine in met behulp van een PAYG-SQL Server installatie kopie vanuit Azure Marketplace en activeer AHB.
1. Installeer SQL Server zelf op een virtuele Azure-machine, [Registreer de SQL Server-VM](virtual-machines-windows-sql-register-with-resource-provider.md) hand matig en activeer Ahb.

Het licentie type van SQL Server wordt ingesteld wanneer de virtuele machine wordt ingericht en kan op elk gewenst moment worden gewijzigd. Als u overschakelt tussen licentie modellen, wordt **geen uitval tijd**opgestart, wordt de virtuele machine niet opnieuw gestart, worden **er geen extra kosten** in rekening gebracht (in feite is het activeren van Ahb *lagere* kosten) en **direct van kracht**. 

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van de resource provider van de SQL-VM is de SQL IaaS-extensie vereist. Om verder te gaan met het gebruik van de resource provider van de SQL-VM, hebt u het volgende nodig:
- Een [Azure-abonnement](https://azure.microsoft.com/free/).
- [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default). 
- Een [SQL Server VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) die is geregistreerd bij de [resource provider](virtual-machines-windows-sql-register-with-resource-provider.md) van de SQL-VM is geïnstalleerd. 


## <a name="change-license-for-vms-already-registered-with-resource-provider"></a>Wijzigings licentie voor Vm's die al zijn geregistreerd bij de resource provider 

# <a name="azure-portaltabazure-portal"></a>[Azure-portal](#tab/azure-portal)

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

U kunt het licentie model rechtstreeks vanuit de portal wijzigen. 

1. Open de [Azure Portal](https://portal.azure.com) en start de [resource van de virtuele SQL-machines](virtual-machines-windows-sql-manage-portal.md#access-sql-virtual-machine-resource) voor uw SQL Server VM. 
1. Selecteer **configureren** onder **instellingen**. 
1. Selecteer de optie **Azure Hybrid Benefit** en schakel het selectie vakje in om te bevestigen dat u een SQL Server licentie hebt met Software Assurance. 
1. Selecteer **Toep assen** onder aan de pagina **configureren** . 

![AHB in Portal](media/virtual-machines-windows-sql-ahb/ahb-in-portal.png)


# <a name="az-clitabbash"></a>[AZ CLI](#tab/bash)

U kunt Azure CLI gebruiken om uw licentie model te wijzigen.  

Met het volgende code fragment wordt het betalen per gebruik-licentie model overgeschakeld naar BYOL (of met behulp van Azure Hybrid Benefit):

```azurecli-interactive
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
# example: az sql vm update -n AHBTest -g AHBTest --license-type AHUB

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type AHUB
```

Met het volgende code fragment kunt u uw eigen licentie model overzetten naar betalen per gebruik: 

```azurecli-interactive
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
# example: az sql vm update -n AHBTest -g AHBTest --license-type PAYG

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type PAYG
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
U kunt Power shell gebruiken om uw licentie model te wijzigen.

Met het volgende code fragment wordt het betalen per gebruik-licentie model overgeschakeld naar BYOL (of met behulp van Azure Hybrid Benefit):

```powershell-interactive
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

Met het volgende code fragment wordt uw BYOL-model overgeschakeld naar betalen per gebruik:

```powershell-interactive
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
---

## <a name="change-license-for-vms-not-registered-with-resource-provider"></a>Wijzigings licentie voor Vm's die niet zijn geregistreerd bij de resource provider

Als u een SQL Server VM van PAYG Azure Marketplace-installatie kopieën hebt ingericht, wordt het SQL-licentie type PAYG. Als u een SQL Server VM hebt ingericht met behulp van een BYOL-installatie kopie vanuit Azure Marketplace, wordt het licentie type AHUB. Alle SQL Server Vm's die zijn ingericht op basis van standaard (PAYG) of BYOL Azure Marketplace-installatie kopieën worden automatisch geregistreerd bij de resource provider van de SQL-VM, zodat ze het [licentie type](#change-license-for-vms-already-registered-with-resource-provider) kunnen wijzigen

U komt alleen in aanmerking voor zelf SQL Server op Azure VM via Azure Hybrid Benefit. u moet [deze vm's registreren bij de resource provider](virtual-machines-windows-sql-register-with-resource-provider.md) van de SQL-VM door de SQL Server licentie in te stellen als Ahb om aan te geven dat Het Ahb-gebruik is gebaseerd op de micro soft-product voorwaarden.

U kunt het licentie type van een SQL Server-VM alleen wijzigen als PAYG of AHB als de virtuele machine van SQL is geregistreerd bij de resource provider van de SQL-VM. en alle SQL-Vm's moeten worden geregistreerd met de SQL-VM RP voor naleving van licenties.

## <a name="remarks"></a>Opmerkingen

 - Azure Cloud Solution partner (CSP)-klanten kunnen gebruikmaken van de Azure Hybrid Benefit door eerst een betalen per gebruik-VM te implementeren en deze vervolgens te converteren naar uw eigen licentie, als deze actieve SA hebben.
 - Als u uw SQL Server VM-resource weghaalt, gaat u terug naar de hardcoded licentie-instelling van de installatie kopie. 
  - De mogelijkheid om het licentie model te wijzigen, is een functie van de resource provider van de SQL-VM. Wanneer u een Marketplace-installatie kopie implementeert via de Azure Portal wordt automatisch een SQL Server VM geregistreerd bij de resource provider. Klanten die zelf een SQL Server installeren, moeten echter [hun SQL Server virtuele machine](virtual-machines-windows-sql-register-with-resource-provider.md)hand matig registreren. 
- Voor het toevoegen van een SQL Server-VM aan een beschikbaarheidsset moet de virtuele machine opnieuw worden gemaakt. Daarom zullen Vm's die aan een beschikbaarheidsset zijn toegevoegd, worden teruggestuurd naar het standaard licentie type voor betalen per gebruik en AHB moet opnieuw worden ingeschakeld. 


## <a name="limitations"></a>Beperkingen

 - Het wijzigen van het licentie model is alleen beschikbaar voor klanten met Software Assurance.
 - Het wijzigen van het licentie model wordt alleen ondersteund voor de Standard-en Enter prise-editie van SQL Server. Licentie wijzigingen voor Express, Web en Developer worden niet ondersteund. 
 - Het wijzigen van het licentie model wordt alleen ondersteund voor virtuele machines die zijn geïmplementeerd met het Resource Manager-model. Vm's die met het klassieke model zijn geïmplementeerd, worden niet ondersteund. U kunt uw VM migreren van klassiek naar Resource Manager (ARM)-model en registreren bij SQL VM-resource provider. Zodra de VM is geregistreerd bij de resource provider van de SQL-VM, zijn wijzigingen in het licentie model beschikbaar op de VM. 
 - Het wijzigen van het licentie model is alleen ingeschakeld voor open bare Cloud installaties.
 - Het wijzigen van het licentie model wordt alleen ondersteund op virtuele machines met één NIC (netwerk interface). Op virtuele machines met meer dan één NIC moet u eerst een van de Nic's (met behulp van de Azure Portal) verwijderen voordat u de procedure probeert uit te voeren. Anders wordt er een fout bericht met de volgende strekking weer gegeven: `The virtual machine '\<vmname\>' has more than one NIC associated.`Hoewel het mogelijk is om de NIC weer aan de virtuele machine toe te voegen nadat u de licentie modus hebt gewijzigd, worden bewerkingen die worden uitgevoerd via de pagina SQL-configuratie in de Azure Portal, zoals automatische patching en back-up, niet meer beschouwd als ondersteund.


## <a name="known-errors"></a>Bekende fouten

### <a name="the-resource-microsoftsqlvirtualmachinesqlvirtualmachinesresource-group-under-resource-group-resource-group-was-not-found-the-property-sqlserverlicensetype-cannot-be-found-on-this-object-verify-that-the-property-exists-and-can-be-set"></a>De resource ' micro soft. SqlVirtualMachine/SqlVirtualMachines\</resource-group > ' onder resource groep\<' resource-group > ' is niet gevonden. De eigenschap sqlServerLicenseType is niet gevonden voor dit object. Controleer of de eigenschap bestaat en kan worden ingesteld.
Deze fout treedt op wanneer probeert het licentie model te wijzigen op een SQL Server VM die niet is geregistreerd bij de resource provider van de SQL-VM. U moet de resource provider registreren bij uw [abonnement](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-vm-resource-provider-with-subscription)en vervolgens uw SQL Server-VM registreren bij de SQL- [resource provider](virtual-machines-windows-sql-register-with-resource-provider.md). 

### <a name="cannot-validate-argument-on-parameter-sku"></a>Kan argument niet valideren voor de para meter ' SKU '
Deze fout kan optreden wanneer u probeert om het SQL Server VM-licentie model te wijzigen wanneer u Azure PowerShell > 4,0:`Set-AzResource: Cannot validate argument on parameter 'Sku'. The argument is null or empty. Provide an argument that is not null or empty, and then try the command again.`

Als u deze fout wilt oplossen, moet u de opmerkingen van deze regels in het eerder genoemde Power shell-code fragment opheffen bij het overschakelen van uw licentie model:

  ```powershell-interactive
  # the following code snippet is necessary if using Azure Powershell version > 4
  $SqlVm.Kind= "LicenseChange"
  $SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
  $SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new()
  ```
  
Gebruik de volgende code om uw Azure PowerShell-versie te controleren:
  
  ```powershell-interactive
  Get-Module -ListAvailable -Name Azure -Refresh
  ```

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer informatie de volgende artikelen: 

* [Overzicht van SQL Server op een Windows-VM](virtual-machines-windows-sql-server-iaas-overview.md)
* [Veelgestelde vragen over Windows-VM'S SQL Server](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server over de prijs informatie voor Windows-VM'S](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Opmerkingen bij de release van een Windows-VM SQL Server](virtual-machines-windows-sql-server-iaas-release-notes.md)


