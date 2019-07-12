---
title: SQL Server virtuele machine in Azure met de SQL-VM-resourceprovider registreren | Microsoft Docs
description: Uw SQL Server-machine registreren bij de resourceprovider van SQL-VM voor het verbeteren van beheerbaarheid.
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
ms.date: 06/24/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: c78b5d71fffbf579b15f747c048bd65259039749
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786739"
---
# <a name="register-sql-server-virtual-machine-in-azure-with-the-sql-vm-resource-provider"></a>SQL Server virtuele machine in Azure met de SQL-VM-resourceprovider registreren

In dit artikel wordt beschreven hoe u uw Azure SQL-Server virtuele machine (VM) met de SQL-VM-resourceprovider registreren. 

Automatisch implementeren van een marketplace-installatiekopie voor SQL Server-VM via de Azure-portal, wordt de SQL Server-VM met de resourceprovider geregistreerd. Echter, als u kiest zelf het installeren van SQL Server op een Azure-Machine in plaats van een afbeelding kiezen uit de Azure Marketplace, klikt u moet registreren uw SQL Server-VM met de resourceprovider voor:

-  **Naleving** – conform de bepalingen van de Microsoft-producten, klanten die gebruikmaken van de [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/) dient aan te geven aan Microsoft bij het gebruik van Azure Hybrid Benefit - en om dit te doen, moeten ze zich registreren bij de resourceprovider van de SQL-VM. 

-  **Functie voordelen** -Hiermee ontgrendelt u uw SQL Server-machine registreren bij de resourceprovider [automatisch patchen](virtual-machines-windows-sql-automated-patching.md), [automatische back-ups](virtual-machines-windows-sql-automated-backup-v2.md), mogelijkheden voor bewaking en beheer, evenals [licentieverlening](virtual-machines-windows-sql-ahb.md) en [edition](virtual-machines-windows-sql-change-edition.md) flexibiliteit. Eerder waren deze alleen beschikbaar voor SQL Server-VM-installatiekopieën op Azure Marketplace.

Automatische installatie van SQL Server op een Azure VM of het inrichten van een Azure-VM vanuit een aangepaste VHD met SQL Server is compatibel met via Azure Hybrid Benefit voor SQL Server met de voorwaarde dat klanten aan te geven die gebruikmaken van Microsoft registreren met de SQL-VM-resource de provider. 

Voor het gebruik van de resourceprovider van SQL-VM, moet u ook de SQL-VM-resourceprovider registreren bij uw abonnement. Dit kan worden bewerkstelligd met de Azure portal, Azure CLI en PowerShell. 

## <a name="prerequisites"></a>Vereisten

Voor het registreren van uw SQL Server-VM met de resourceprovider, moet u het volgende: 

- Een [Azure-abonnement](https://azure.microsoft.com/free/).
- Een [SQL Server-machine](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision). 
- [Azure CLI](/cli/azure/install-azure-cli) en [PowerShell](/powershell/azure/new-azureps-module-az). 

## <a name="register-with-sql-vm-resource-provider"></a>Registreren bij de resourceprovider van SQL-VM
Als de [SQL IaaS-extensie](virtual-machines-windows-sql-server-agent-extension.md) al is geïnstalleerd op de virtuele machine, en vervolgens registreren bij de resourceprovider van SQL-VM, is een metagegevensresource van het type Microsoft.SqlVirtualMachine/SqlVirtualMachines te maken. Hieronder volgt het codefragment om te registreren bij de SQL-VM-resourceprovider als de SQL IaaS-extensie is al geïnstalleerd op de virtuele machine. U moet opgeven van het type van de SQL Server-licentie nodig bij het registreren bij de SQL-VM-resourceprovider als een ' betalen per gebruik ' of 'AHUB'. 

Registreer SQL Server-VM met behulp van PowerShell met het volgende codefragment:

  ```powershell-interactive
     // Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     // Register with SQL VM resource provider
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;sqlLicenseType='AHUB'}  
  
  ```

Als de SQL IaaS-extensie is niet geïnstalleerd op de virtuele machine, kunt klikt u vervolgens u registreren bij de resourceprovider van SQL-VM door de lichtgewicht SQL management-modus op te geven. In de Beheermodus voor lichtgewicht SQL, SQL-VM-resourceprovider wordt automatisch installeren SQL IaaS-extensie in [lichte modus](virtual-machines-windows-sql-server-agent-extension.md#install-in-lightweight-mode) en controleer of SQL Server metagegevens van het exemplaar; deze wordt niet SQL Server-service opnieuw te starten. U moet opgeven van het type van de SQL Server-licentie nodig bij het registreren bij de SQL-VM-resourceprovider als een ' betalen per gebruik ' of 'AHUB'. 

Registreer SQL Server-VM in de lichte modus met SQL management met behulp van PowerShell met het volgende codefragment:

  ```powershell-interactive
     // Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     // Register SQL VM with 'Lightweight' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;sqlLicenseType='AHUB';sqlManagement='LightWeight'}  
  
  ```
Registreren bij de resourceprovider van SQL-VM in [lichte modus](virtual-machines-windows-sql-server-agent-extension.md#install-in-lightweight-mode) wordt naleving garanderen en flexibele licentiëring inschakelen, evenals in-place updates voor SQL Server-editie. Failover Cluster Instances en meerdere exemplaren implementaties kunnen worden geregistreerd bij de resourceprovider van SQL-VM in de lichte modus. U kunt de instructies vinden in Azure portal om te upgraden naar volgen [in de modus volledig](virtual-machines-windows-sql-server-agent-extension.md#full-mode-installation) en op elk gewenst moment de functieset uitgebreide beheerbaarheid met SQL Server opnieuw opstarten in te schakelen. 

## <a name="register-sql-server-2008r2-on-windows-server-2008-vms"></a>SQL Server 2008/R2 op Windows Server 2008-VM's registreren

SQL Server 2008 en 2008 R2 te installeren op Windows Server 2008 kunnen worden geregistreerd met de SQL-VM resource opgeven in de [NoAgent](virtual-machines-windows-sql-server-agent-extension.md) modus. Deze optie zorgen voor naleving en kunt de SQL Server-VM in Azure portal worden bewaakt met beperkte functionaliteit.

De volgende tabel worden de acceptabele waarden voor de parameters die zijn opgegeven tijdens de registratie:

| Parameter | Acceptabele waarden                                 |
| :------------------| :--------------------------------------- |
| **sqlLicenseType** | `'AHUB'`, of `'PAYG'`                    |
| **sqlImageOffer**  | `'SQL2008-WS2008'` of `'SQL2008R2-WS2008`|
| &nbsp;             | &nbsp;                                   |


Gebruik de volgende Powershell-codefragment voor het registreren van uw SQL Server 2008 of 2008 R2 op Windows Server 2008-exemplaar:  

  ```powershell-interactive
     // Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
    New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
      -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
      -Properties @{virtualMachineResourceId=$vm.Id;sqlLicenseType='AHUB'; `
       sqlManagement='NoAgent';sqlImageSku='Standard';sqlImageOffer='SQL2008R2-WS2008'}
  ```



## <a name="verify-registration-status"></a>Controleer of de registratiestatus
U kunt controleren als uw SQL-Server is al geregistreerd bij de resourceprovider van de SQL-VM met de Azure portal, Azure CLI of PowerShell. 

# <a name="azure-portaltabazure-portal"></a>[Azure Portal](#tab/azure-portal)
U kunt de status van inschrijving met Azure portal controleren door het volgende te doen.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com). 
1. Navigeer naar uw [SQL virtuele machines](virtual-machines-windows-sql-manage-portal.md).
1. Selecteer uw SQL Server-machine in de lijst. Als uw SQL Server-machine hier niet wordt vermeld, is het waarschijnlijk dat uw SQL Server-VM is niet geregistreerd bij de resourceprovider van de SQL-VM. 
1. Bekijk de waarde onder `Status`. Als `Status = Succeeded`, en vervolgens de SQL Server-VM is geregistreerd bij de resourceprovider van SQL-VM is. 

    ![Controleer of de status met SQL-RP-registratie](media/virtual-machines-windows-sql-register-with-rp/verify-registration-status.png)

# <a name="az-clitabbash"></a>[AZ CLI](#tab/bash)

Controleer of de huidige registratiestatus van SQL Server-VM met de volgende AZ-CLI-opdracht. `ProvisioningState` ziet u `Succeeded` als de registratie gelukt is. 

  ```azurecli-interactive
  az sql vm show -n <vm_name> -g <resource_group>
  ```


# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Controleer of de huidige registratiestatus van SQL Server-VM met de volgende PowerShell-cmdlet. `ProvisioningState` ziet u `Succeeded` als de registratie gelukt is. 

  ```powershell-interactive
  Get-AzResource -ResourceName <vm_name> -ResourceGroupName <resource_group> -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines
  ```
Een fout geeft aan dat de SQL Server-VM niet is geregistreerd bij de resourceprovider. 

---

## <a name="register-sql-vm-resource-provider-with-subscription"></a>SQL-VM-resourceprovider registreren met abonnement 

Voor het registreren van uw SQL Server-VM met de resourceprovider van SQL-VM, moet u de resourceprovider registreren bij uw abonnement. U kunt dit doen met de Azure portal of Azure CLI.

# <a name="azure-portaltabazure-portal"></a>[Azure Portal](#tab/azure-portal)


De SQL-VM-resourceprovider aan uw Azure-abonnement met behulp van de Azure portal, de volgende stappen wordt geregistreerd. 

1. Open de Azure-portal en Ga naar **alle Services**. 
1. Navigeer naar **abonnementen** en selecteer het abonnement van belang zijn.  
1. Op de **abonnementen** pagina, gaat u naar **resourceproviders**. 
1. Type `sql` in het filter om de SQL-gerelateerde resourceproviders. 
1. Selecteer een *registreren*, *opnieuw registreren*, of *registratie* voor de **Microsoft.SqlVirtualMachine** provider afhankelijk van uw gewenste actie. 

   ![De provider wijzigen](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

# <a name="az-clitabbash"></a>[AZ CLI](#tab/bash)

Het volgende codefragment wordt de resourceprovider van SQL-VM naar uw Azure-abonnement registreren. 

```azurecli-interactive
# Register the new SQL VM resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Het volgende PowerShell-codefragment wordt de resourceprovider van SQL-VM naar uw Azure-abonnement registreren.

```powershell-interactive
# Register the new SQL VM resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```
---

## <a name="remarks"></a>Opmerkingen

 - De SQL-VM-resource-provider biedt alleen ondersteuning voor SQL Server-VM's geïmplementeerd met behulp van Resource Manager. SQL Server-VM's geïmplementeerd met behulp van die het klassieke model, worden niet ondersteund. 
 - De SQL-VM-resource-provider biedt alleen ondersteuning voor SQL Server-VM's geïmplementeerd naar de openbare cloud. Implementaties naar de persoonlijke of government-cloud, worden niet ondersteund. 

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer informatie de volgende artikelen: 

* [Overzicht van SQL Server op een Windows VM](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server op een Windows VM-Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server op een Windows-VM prijsinformatie](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server op een Windows VM release-opmerkingen](virtual-machines-windows-sql-server-iaas-release-notes.md)


