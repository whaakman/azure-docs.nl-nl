---
title: Licentiemodel voor SQL Server-VM in Azure wijzigen
description: Informatie over het wijzigen van de licentieverlening voor een SQL-VM in Azure van 'betalen per gebruik' naar 'bring-your-own-license' met behulp van Azure Hybrid Benefit.
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
ms.date: 02/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 78ad784a45d2b0063932791daedc9b1ec1aafd72
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786767"
---
# <a name="how-to-change-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>Het wijzigen van licentiemodel voor een virtuele machine van SQL Server in Azure
In dit artikel wordt beschreven hoe u de licentiemodel voor een virtuele machine van SQL Server in Azure met behulp van de nieuwe SQL-VM-resourceprovider - **Microsoft.SqlVirtualMachine**.

Er zijn twee licentiemodellen voor een virtuele machine (VM) die als host fungeert voor SQL Server - betalen per gebruik en Azure Hybrid Benefit (AHB). En nu, met de Azure portal, Azure CLI of PowerShell kunt u wijzigen licentiemodel van uw SQL Server-VM. 

De **betalen per gebruik** model (betalen per gebruik) betekent dat de kosten per seconde van het uitvoeren van de virtuele machine van Azure de kosten van de SQL Server-licentie bevat.
De [Azure Hybrid Benefit (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/) kunt u uw eigen SQL Server-licentie aan een virtuele machine met SQL Server gebruiken. 

Microsoft Azure Hybrid Benefit voor SQL Server kunt met behulp van SQL Server-licenties met Software Assurance ("gekwalificeerde License') op Azure Virtual Machines. Klanten worden niet in rekening gebracht voor het gebruik van SQL Server-licentie op de virtuele machine met Azure Hybrid Benefit voor SQL Server, maar ze nog steeds de kosten van de onderliggende cloud Computing (dat wil zeggen, het basistarief), opslag en back-ups, evenals i/o die zijn gekoppeld aan hun u moeten betalen Tweede editie van de services (zoals van toepassing).

Op basis van voorwaarden voor Microsoft-Product "klanten dient aan te geven dat deze gebruikmaakt van Azure SQL-Database (voor het beheerde exemplaar, elastische Pools en individuele Database), Azure Data Factory, SQL Server integratieservices of SQL Server Virtual Machines onder Azure Hybrid Benefit voor SQL Server bij het configureren van werkbelastingen op Azure."

Voor het geven van het gebruik van Azure Hybrid Benefit voor SQL Server op Azure VM en compatibel zijn, zijn er drie opties:

1. Richt een virtuele machine met een BYOL SQL Server-installatiekopie uit de Azure marketplace, alleen beschikbaar voor klanten met Enterprise Agreement.
1. Richt een virtuele machine met een betalen per gebruik SQL Server-installatiekopie uit Azure marketplace en AHB activeren.
1. Installeer SQL Server op een Azure-VM zelf handmatig [registreren hun SQL Server-VM](virtual-machines-windows-sql-register-with-resource-provider.md) en AHB moet worden geactiveerd.

Licentietype van SQL Server wordt ingesteld wanneer de virtuele machine is ingericht en kan op elk gewenst moment later worden gewijzigd. Schakelen tussen licentiemodellen leidt tot **zonder uitvaltijd**, de virtuele machine niet opnieuw wordt opgestart, wordt toegevoegd **zonder extra kosten** (in feite activeren AHB *vermindert* kosten) en **met onmiddellijke ingang effectief**. 

## <a name="prerequisites"></a>Vereisten

Het gebruik van de resourceprovider van SQL-VM moet de SQL IaaS-extensie. Als u wilt doorgaan met het gebruik van de resourceprovider van SQL-VM, moet u als zodanig het volgende:
- Een [Azure-abonnement](https://azure.microsoft.com/free/).
- [Software assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default). 
- Een [SQL Server-VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) geregistreerd bij de [SQL-VM-resourceprovider](virtual-machines-windows-sql-register-with-resource-provider.md) geïnstalleerd. 


## <a name="change-license-for-vms-already-registered-with-resource-provider"></a>Licentie voor virtuele machines die al zijn geregistreerd bij de resourceprovider wijzigen 

# <a name="azure-portaltabazure-portal"></a>[Azure-portal](#tab/azure-portal)

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

U kunt de licentiemodel rechtstreeks vanuit de portal wijzigen. 

1. Open de [Azure-portal](https://portal.azure.com) en start de [SQL-resource voor virtuele machines](virtual-machines-windows-sql-manage-portal.md#access-sql-virtual-machine-resource) voor uw SQL Server-VM. 
1. Selecteer **configureren** onder **instellingen**. 
1. Selecteer de **Azure Hybrid Benefit** optie en selecteert u het selectievakje in om te bevestigen dat u een SQL Server-licentie met Software Assurance hebt. 
1. Selecteer **toepassen** aan de onderkant van de **configureren** pagina. 

![AHB in Portal](media/virtual-machines-windows-sql-ahb/ahb-in-portal.png)


# <a name="az-clitabbash"></a>[AZ CLI](#tab/bash)

Azure CLI kunt u uw licentiemodel wijzigen.  

Uw licentiemodel voor betalen per gebruik verandert het volgende codefragment in een BYOL (of met behulp van Azure Hybrid Benefit):

```azurecli-interactive
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
# example: az sql vm update -n AHBTest -g AHBTest --license-type AHUB

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type AHUB
```

Het volgende codefragment wordt het model bring-your-own-license naar betalen per gebruik: 

```azurecli-interactive
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
# example: az sql vm update -n AHBTest -g AHBTest --license-type PAYG

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type PAYG
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
U kunt PowerShell gebruiken om te wijzigen van uw licentiemodel.

Uw licentiemodel voor betalen per gebruik verandert het volgende codefragment in een BYOL (of met behulp van Azure Hybrid Benefit):

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

Het volgende codefragment verandert de BYOL-model voor betalen per gebruik:

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

## <a name="change-license-for-vms-not-registered-with-resource-provider"></a>Licentie voor virtuele machines die niet zijn geregistreerd bij de resourceprovider wijzigen

Als u een SQL Server-VM van betalen per gebruik Azure Marketplace-installatiekopieën ingericht worden SQL-licentietype betalen per gebruik. Als u een SQL Server-VM met een BYOL-installatiekopie uit de Azure Marketplace ingericht worden het licentietype AHUB. Alle SQL Server-VM's ingericht vanuit standaard (betalen per gebruik) of BYOL Azure Marketplace-installatiekopieën wordt automatisch geregistreerd bij de SQL-VM-resourceprovider, zodat ze kunnen wijzigen de [licentietype](#change-license-for-vms-already-registered-with-resource-provider)

U komt alleen in aanmerking voor het installeren van SQL Server op virtuele Azure-machine via Azure Hybrid Benefit zelf en moet u [deze virtuele machines met SQL-VM-resourceprovider registreren](virtual-machines-windows-sql-register-with-resource-provider.md) door in te stellen AHB om aan te geven van het gebruik AHB volgens de SQL Server-licentie Voorwaarden voor Microsoft-Product.

U kunt het licentietype van een SQL Server-VM als betalen per gebruik of AHB alleen wijzigen als de SQL-VM is geregistreerd bij de resourceprovider van SQL-VM; en alle SQL-VM's moeten worden geregistreerd met SQL VM RP voor compatibiliteit van licentie.

## <a name="remarks"></a>Opmerkingen

 - Azure Cloud Solution Partner (CSP)-klanten kunnen gebruikmaken van Azure Hybrid Benefit door eerst een betalen per gebruik virtuele machine te implementeren en deze vervolgens converteren naar bring-your-own-license als ze beschikken over actieve Software Assurance.
 - Als u uw SQL Server-VM-resource, gaat u terug naar de vastgelegde licentie-instelling van de installatiekopie. 
  - De mogelijkheid om te wijzigen van de licentiemodel is een functie van de SQL-VM-resourceprovider. Automatisch implementeren van een marketplace-installatiekopie via de Azure-portal, wordt een virtuele machine van SQL Server geregistreerd bij de resourceprovider. Klanten die SQL Server zelf installeert wordt moet echter handmatig [registreren hun SQL Server-VM](virtual-machines-windows-sql-register-with-resource-provider.md). 
- Een SQL Server-VM toevoegen aan een beschikbaarheidsset is vereist dat de virtuele machine opnieuw wordt gemaakt. Als dergelijke ziet u alle virtuele machines die zijn toegevoegd aan een beschikbaarheidsset set wordt gaat u terug naar het standaardtype voor betalen per gebruik-licentie en AHB moet opnieuw worden ingeschakeld. 


## <a name="limitations"></a>Beperkingen

 - Wijzigen van de licentiemodel is alleen beschikbaar voor klanten met software assurance.
 - Wijzigen van de licentiemodel wordt alleen ondersteund voor de standard- en enterprise-editie van SQL Server. Licentiewijzigingen in de voor de Express-, Web- en Developer worden niet ondersteund. 
 - Wijzigen van de licentiemodel wordt alleen ondersteund voor virtuele machines die zijn geïmplementeerd met behulp van de Resource Manager-model. Virtuele machines die zijn geïmplementeerd met behulp van het klassieke model, worden niet ondersteund. 
 - Wijzigen van de licentiemodel is alleen ingeschakeld voor installaties van de openbare Cloud.
 - Wijzigen van de licentiemodel wordt alleen ondersteund op virtuele machines met één NIC (network interface). Op virtuele machines met meer dan één NIC, u moet eerst een verwijderen van de NIC's (met behulp van de Azure-portal) voordat u de procedure. Anders wordt u uitvoeren in een vergelijkbaar met de volgende fout: `The virtual machine '\<vmname\>' has more than one NIC associated.` Hoewel u mogelijk de NIC terug naar de virtuele machine toevoegen nadat u de licentiemodus wijzigen, bewerkingen die via de pagina SQL-configuratie in Azure portal, zoals automatische toepassing van patches en back-up wordt niet meer worden beschouwd als ondersteund.


## <a name="known-errors"></a>Bekende problemen

### <a name="the-resource-microsoftsqlvirtualmachinesqlvirtualmachinesresource-group-under-resource-group-resource-group-was-not-found-the-property-sqlserverlicensetype-cannot-be-found-on-this-object-verify-that-the-property-exists-and-can-be-set"></a>De Resource ' Microsoft.SqlVirtualMachine/SqlVirtualMachines/\<resource-group >' onder de resourcegroep '\<resource-group >' is niet gevonden. De eigenschap 'sqlServerLicenseType' kan niet worden gevonden voor dit object. Controleer of de eigenschap bestaat en kan worden ingesteld.
Deze fout treedt op wanneer u probeert te wijzigen van de licentiemodel op een SQL Server-VM die is niet geregistreerd bij de resourceprovider van de SQL-VM. U moet de resourceprovider te registreren uw [abonnement](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-vm-resource-provider-with-subscription), en registreert u uw SQL Server-VM met de SQL [resourceprovider](virtual-machines-windows-sql-register-with-resource-provider.md). 

### <a name="cannot-validate-argument-on-parameter-sku"></a>Argument voor parameter 'Sku' kan niet worden gevalideerd.
U kunt deze fout kan optreden bij het wijzigen van uw SQL Server-VM-licentiemodel wanneer u Azure PowerShell > 4.0: `Set-AzResource: Cannot validate argument on parameter 'Sku'. The argument is null or empty. Provide an argument that is not null or empty, and then try the command again.`

U kunt deze fout oplossen, #opmerkingen op deze regels in de eerder genoemde PowerShell-codefragment wanneer u overschakelt van uw licentiemodel:

  ```powershell-interactive
  # the following code snippet is necessary if using Azure Powershell version > 4
  $SqlVm.Kind= "LicenseChange"
  $SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
  $SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new()
  ```
  
Gebruik de volgende code om te controleren of uw versie van Azure PowerShell:
  
  ```powershell-interactive
  Get-Module -ListAvailable -Name Azure -Refresh
  ```

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer informatie de volgende artikelen: 

* [Overzicht van SQL Server op een Windows VM](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server op een Windows VM-Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server op een Windows-VM prijsinformatie](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server op een Windows VM release-opmerkingen](virtual-machines-windows-sql-server-iaas-release-notes.md)


