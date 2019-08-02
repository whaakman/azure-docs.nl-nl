---
title: SQL Server virtuele machine registreren in azure met de resource provider van de SQL-VM | Microsoft Docs
description: Registreer uw SQL Server-VM met de resource provider van de SQL-VM om de beheer baarheid te verbeteren.
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
ms.openlocfilehash: 95a167cbc8fde4488e3f46ffd850f0619cf1b651
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68305871"
---
# <a name="register-sql-server-virtual-machine-in-azure-with-the-sql-vm-resource-provider"></a>SQL Server virtuele machine registreren in azure met de resource provider van de SQL-VM

In dit artikel wordt beschreven hoe u uw Azure SQL Server virtuele machine (VM) registreert bij de resource provider van de SQL-VM. 

Als u een installatie kopie van een SQL Server VM-Marketplace implementeert via de Azure Portal, wordt de SQL Server VM automatisch geregistreerd bij de resource provider. Als u echter zelf SQL Server wilt installeren op een virtuele Azure-machine in plaats van een installatie kopie te kiezen vanuit Azure Marketplace, moet u de SQL Server-VM voor de resource provider vandaag nog registreren voor:

-  **Naleving** : volgens de product voorwaarden van micro soft moeten klanten die gebruikmaken van de [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/) , aan micro soft aangeven wanneer ze de Azure Hybrid Benefit-en gebruiken om dit te doen, ze moeten zich registreren bij de resource provider van de SQL-VM. 

-  **Functie voordelen** : als u uw SQL Server-VM registreert bij de resource provider, worden [automatisch patches](virtual-machines-windows-sql-automated-patching.md), [automatische back-ups](virtual-machines-windows-sql-automated-backup-v2.md), bewakings-en beheer mogelijkheden en de flexibiliteit van [licenties](virtual-machines-windows-sql-ahb.md) en [edities](virtual-machines-windows-sql-change-edition.md) ontgrendeld. Voorheen waren deze alleen beschikbaar voor het SQL Server van VM-installatie kopieën vanuit Azure Marketplace.

Automatische installatie van SQL Server op een virtuele machine in azure of het inrichten van een Azure-VM vanaf een aangepaste VHD met SQL Server is conform Azure Hybrid Benefit voor SQL Server met de voor waarde dat klanten aangeven dat ze gebruiken voor micro soft door zich te registreren bij een SQL-VM-resource providers. 

Als u de resource provider van de SQL-VM wilt gebruiken, moet u ook de resource provider van de SQL-VM registreren bij uw abonnement. Dit kan worden bereikt met de Azure Portal, Azure CLI en Power shell. 

## <a name="prerequisites"></a>Vereisten

Als u uw SQL Server-VM wilt registreren bij de resource provider, hebt u het volgende nodig: 

- Een [Azure-abonnement](https://azure.microsoft.com/free/).
- Een [SQL Server-VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision). 
- [Azure cli](/cli/azure/install-azure-cli) en [Power shell](/powershell/azure/new-azureps-module-az). 

## <a name="register-with-sql-vm-resource-provider"></a>Registreren bij SQL-VM-resource provider
Als de [SQL IaaS-uitbrei ding](virtual-machines-windows-sql-server-agent-extension.md) al is geïnstalleerd op de virtuele machine, moet u zich registreren met de resource provider van de SQL-VM, maar een meta gegevens bron van het type micro soft. SqlVirtualMachine/SqlVirtualMachines maken. Hieronder ziet u het code fragment dat moet worden geregistreerd bij de resource provider van de SQL-VM als de SQL IaaS-uitbrei ding al is geïnstalleerd op de virtuele machine. U moet het type SQL Server vereiste licentie opgeven wanneer u zich registreert bij de resource provider van de SQL-VM als ' PAYG ' of ' AHUB '. 

Registreer SQL Server VM met behulp van Power shell met het volgende code fragment:

  ```powershell-interactive
     # Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     # Register with SQL VM resource provider
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB'}  
  
  ```

Als de SQL IaaS-extensie niet is geïnstalleerd op de virtuele machine, kunt u zich registreren bij de SQL-VM-resource provider door de Lightweight SQL Management-modus op te geven. In de Lightweight SQL-beheer modus installeert SQL VM-resource provider automatisch SQL IaaS-uitbrei ding in de [Lightweight-modus](virtual-machines-windows-sql-server-agent-extension.md#install-in-lightweight-mode) en worden de meta gegevens van SQL Server-exemplaren gecontroleerd. Hiermee wordt de SQL Server-service niet opnieuw opgestart. U moet het type SQL Server vereiste licentie opgeven wanneer u zich registreert bij de resource provider van de SQL-VM als ' PAYG ' of ' AHUB '. 

Registreer SQL Server VM in de Lightweight SQL Management-modus met behulp van Power shell met het volgende code fragment:

  ```powershell-interactive
     # Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     # Register SQL VM with 'Lightweight' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB';sqlManagement='LightWeight'}  
  
  ```

Als u zich registreert bij de resource provider van de SQL-VM in de [Lightweight-modus](virtual-machines-windows-sql-server-agent-extension.md#install-in-lightweight-mode) , wordt de naleving gegarandeerd en kunnen flexibele licentie verlening worden ingeschakeld, evenals in-place SQL Server editie-updates. Failover-cluster instanties en implementaties met meerdere exemplaren kunnen alleen worden geregistreerd bij de resource provider van de SQL-VM in de Lightweight-modus. U kunt de instructies op Azure Portal volgen om een upgrade uit te voeren naar de [volledige modus](virtual-machines-windows-sql-server-agent-extension.md#full-mode-installation) en de uitgebreide beheer baarheid-functieset in te scha kelen met een SQL Server elke keer opnieuw op te starten. 

## <a name="register-sql-server-2008r2-on-windows-server-2008-vms"></a>SQL Server 2008/R2 registreren op virtuele machines met Windows Server 2008

SQL Server 2008 en 2008 R2 geïnstalleerd op Windows Server 2008 kunnen worden geregistreerd met de SQL-VM-resource in [](virtual-machines-windows-sql-server-agent-extension.md) de modus voor niet-agents. Met deze optie wordt de naleving gegarandeerd en kan de SQL Server virtuele machine worden bewaakt in de Azure Portal met beperkte functionaliteit.

De volgende tabel bevat een overzicht van de acceptabele waarden voor de para meters die tijdens de registratie worden opgegeven:

| Parameter | Acceptabele waarden                                 |
| :------------------| :--------------------------------------- |
| **sqlLicenseType** | `'AHUB'`, of`'PAYG'`                    |
| **sqlImageOffer**  | `'SQL2008-WS2008'` of `'SQL2008R2-WS2008`|
| &nbsp;             | &nbsp;                                   |


Gebruik het volgende Power shell-code fragment om uw SQL Server 2008 of 2008 R2 te registreren op Windows Server 2008-exemplaar:  

  ```powershell-interactive
     # Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
    New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
      -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
      -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB'; `
       sqlManagement='NoAgent';sqlImageSku='Standard';sqlImageOffer='SQL2008R2-WS2008'}
  ```

## <a name="verify-registration-status"></a>Registratie status verifiëren
U kunt controleren of uw SQL Server al is geregistreerd bij de resource provider van de SQL-VM met behulp van de Azure Portal, Azure CLI of Power shell. 

### <a name="azure-portal"></a>Azure Portal 
Ga als volgt te werk om de status van de registratie te controleren met behulp van de Azure Portal.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com). 
1. Navigeer naar uw [virtuele SQL-machines](virtual-machines-windows-sql-manage-portal.md).
1. Selecteer uw SQL Server-VM in de lijst. Als uw SQL Server VM hier niet wordt weer gegeven, is het waarschijnlijk dat uw SQL Server VM niet is geregistreerd bij de resource provider van de SQL-VM. 
1. Bekijk de waarde onder `Status`. Als `Status = Succeeded`, is de SQL Server VM geregistreerd bij de resource provider van de SQL-VM. 

    ![Status controleren met SQL RP-registratie](media/virtual-machines-windows-sql-register-with-rp/verify-registration-status.png)

### <a name="az-cli"></a>AZ CLI

Controleer de huidige registratie status van SQL Server-VM met de volgende AZ CLI-opdracht. `ProvisioningState`geeft aan `Succeeded` of de registratie is geslaagd. 

  ```azurecli-interactive
  az sql vm show -n <vm_name> -g <resource_group>
  ```


### <a name="powershell"></a>PowerShell

Controleer de huidige SQL Server VM-registratie status met de volgende Power shell-cmdlet. `ProvisioningState`geeft aan `Succeeded` of de registratie is geslaagd. 

  ```powershell-interactive
  Get-AzResource -ResourceName <vm_name> -ResourceGroupName <resource_group> -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines
  ```
Een fout geeft aan dat de SQL Server VM niet is geregistreerd bij de resource provider. 

---

## <a name="register-sql-vm-resource-provider-with-subscription"></a>De resource provider van de SQL-VM registreren met het abonnement 

Als u uw SQL Server-VM wilt registreren bij de resource provider van de SQL-VM, moet u de resource provider registreren bij uw abonnement. U kunt dit doen met de Azure Portal of met Azure CLI.

### <a name="azure-portal"></a>Azure Portal

Met de volgende stappen wordt de resource provider van de SQL-VM geregistreerd bij uw Azure-abonnement met behulp van de Azure Portal. 

1. Open de Azure Portal en ga naar **alle services**. 
1. Navigeer naar **abonnementen** en selecteer het gewenste abonnement.  
1. Ga op de pagina **abonnementen** naar **resource providers**. 
1. Typ `sql` het filter om de SQL-gerelateerde resource providers te openen. 
1. Selecteer *registreren*, *opnieuw registreren*of *registratie ongedaan* maken voor de **micro soft. SqlVirtualMachine** -provider, afhankelijk van de gewenste actie. 

   ![De provider wijzigen](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

### <a name="az-cli"></a>AZ CLI
Met het volgende code fragment wordt de resource provider van de SQL-VM geregistreerd bij uw Azure-abonnement. 

```azurecli-interactive
# Register the new SQL VM resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

### <a name="powershell"></a>PowerShell

Met het volgende Power shell-code fragment wordt de resource provider van de SQL-VM geregistreerd bij uw Azure-abonnement.

```powershell-interactive
# Register the new SQL VM resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```
---

## <a name="remarks"></a>Opmerkingen

 - De resource provider van de SQL-VM ondersteunt alleen SQL Server Vm's die zijn geïmplementeerd met Resource Manager. SQL Server Vm's die met het klassieke model zijn geïmplementeerd, worden niet ondersteund. 
 - De resource provider van de SQL-VM ondersteunt alleen SQL Server Vm's die in de open bare Cloud zijn geïmplementeerd. Implementaties naar de privé-of Government-Cloud worden niet ondersteund. 
 
## <a name="frequently-asked-questions"></a>Veelgestelde vragen 

**Moet ik mijn SQL Server VM registreren op basis van een SQL-installatie kopie op Azure Marketplace?**

Nee. Micro soft autoregistreert Vm's die zijn ingericht op basis van de SQL-installatie kopieën op de Azure Marketplace. Registratie met de resource provider van de SQL-VM is alleen vereist als de virtuele machine niet is ingericht op basis van de SQL-installatie kopieën op de Azure Marketplace en SQL Server zelf is geïnstalleerd.

**Is de SQL VM-resourceprovider beschikbaar voor alle klanten?** 

Ja. Klanten moeten hun SQL Server VM registreren bij de resource provider van de SQL-VM als ze geen SQL Server installatie kopie gebruiken van de Azure Marketplace en zelf geïnstalleerde SQL Server of hun aangepaste VHD. Vm's die eigendom zijn van alle typen abonnementen (direct, EA en CSP) kunnen worden geregistreerd bij de resource provider van de SQL-VM.

**Moet ik registreren bij de SQL-VM-resource provider als op mijn SQL Server-VM al de SQL IaaS-extensie is geïnstalleerd?**

Als uw SQL Server virtuele machine zelf is geïnstalleerd, niet is ingericht op basis van de SQL-installatie kopieën op de Azure Marketplace, moet u zich registreren bij de SQL-VM-resource provider, zelfs als u de SQL IaaS-extensie hebt geïnstalleerd. Registreren met de resource provider van de SQL-VM maakt een nieuwe resource van het type micro soft. SqlVirtualMachines. Als u de SQL IaaS-extensie installeert, wordt deze resource niet gemaakt.

**Wat is de standaard modus voor SQL-beheer bij de registratie van de resource provider van een SQL-VM?**

De standaard SQL-beheer modus bij het registreren met de SQL-VM RP is _vol_. Als de SQL Management-eigenschap niet is ingesteld bij het registreren met een SQL-VM-resource provider, wordt de modus ingesteld als volledige beheer baarheid. De SQL IaaS-extensie die is geïnstalleerd op de VM, is de vereiste voor het registreren van de resource provider van de SQL-VM in de volledige beheer modus.

**Wat zijn de vereisten voor de registratie bij de resource provider van de SQL-VM?**

Er zijn geen vereisten om te registreren bij de resource provider van de SQL-VM in de modus licht gewicht of op de agent. De vereisten voor het registreren van de SQL-VM-resource provider in de volledige modus is de SQL IaaS-extensie op de VM geïnstalleerd.

**Kan ik registreren bij een SQL VM resource provider als ik de SQL IaaS-extensie niet op de VM heb geïnstalleerd?**

Ja, u kunt zich registreren bij de SQL-VM-resource provider in de modus licht gewicht beheer als u geen SQL IaaS-extensie hebt geïnstalleerd op de VM. In de Lightweight-modus gebruikt SQL VM resource provider een console-app om de versie en editie van het SQL-exemplaar te controleren. De console-app wordt vanzelf afgesloten nadat u hebt gecontroleerd of er ten minste één SQL-exemplaar op de VM wordt uitgevoerd. Registreren met de resource provider van de SQL-VM in de Lightweight-modus wordt SQL Server niet opnieuw gestart en er wordt geen agent gemaakt op de VM.

**Wordt geregistreerd bij de resource provider van een SQL-VM, wordt een agent geïnstalleerd op mijn VM?**

Nee. Bij het registreren met de resource provider van de SQL-VM wordt alleen een nieuwe meta gegevens resource gemaakt en wordt er geen agent geïnstalleerd op de VM. De SQL IaaS-extensie is alleen nodig voor het inschakelen van volledig beheer. Als u de beheer baarheids modus van Lightweight naar Full wilt bijwerken, wordt de SQL IaaS-extensie geïnstalleerd en wordt SQL Server opnieuw opgestart.

**Wordt de registratie van de SQL-VM-resource provider SQL Server op mijn VM opnieuw gestart?**

Nee. Bij het registreren met de resource provider van de SQL-VM wordt alleen een nieuwe meta gegevens resource gemaakt en wordt de SQL Server op de VM niet opnieuw opgestart. Het opnieuw starten van SQL Server is alleen nodig om de SQL IaaS-extensie te installeren. en de SQL IaaS-extensie is vereist voor het inschakelen van volledige beheer baarheid. Als u de beheer baarheids modus van Lightweight naar Full bijwerkt, wordt de SQL IaaS-extensie geïnstalleerd en wordt SQL Server opnieuw opgestart.

**Wat is het verschil tussen Lightweight en no-agent beheer modi bij het registreren met een SQL-VM-resource provider?** 

Geen agent beheer modus is alleen beschikbaar voor SQL Server 2008/R2 op Windows Server 2008. en dit is de enige beschik bare beheer modus voor deze versies. Voor alle andere versies van SQL Server zijn de twee beschik bare beheer modi licht gewicht en volledig. Voor geen enkele agent modus moeten SQL Server versie-en editie-eigenschappen worden ingesteld door de klant. in de Lightweight-modus wordt de VM opgevraagd om de versie en editie van het SQL-exemplaar te vinden.

**Kan ik bij een SQL-VM-resource provider registreren in de modus licht gewicht of zonder agent met Azure CLI?**

Nee. De eigenschap SQL-beheer modus is alleen beschikbaar bij het registreren van de resource provider van de SQL-VM met Azure PowerShell. Azure CLI biedt geen ondersteuning voor het instellen van de SQL-beheer baarheid-eigenschap en wordt altijd geregistreerd bij de resource provider van de SQL-VM in de standaard modus: volledige beheer baarheid.

**Kan ik registreren bij een SQL-VM-resource provider zonder het SQL-licentie type op te geven?**

Nee. Het SQL-licentie type is geen optionele eigenschap bij het registreren met de SQL-VM RP. U moet het SQL-licentie type instellen als PAYG of AHUB bij het registreren van de resource provider van de SQL-VM in alle beheer modi (geen agent, licht gewicht en volledig) met AZ CLI en Power shell.

**Kan ik de SQL IaaS-extensie upgraden van de modus no-agent naar de volledige modus?**

Nee. Het bijwerken van de SQL-beheer modus naar Full of Lightweight is niet beschikbaar voor de modus no-agent. Dit is een technische beperking van Windows Server 2008.

**Kan ik een upgrade uitvoeren van de SQL IaaS-uitbrei ding van de licht modus naar de volledige modus?**

Ja. Het bijwerken van de SQL-beheer baarheids modus van Lightweight naar Full wordt ondersteund via Power shell of Azure Portal; en hiervoor moet SQL Server opnieuw worden gestart.

**Kan ik de SQL IaaS-uitbrei ding van de volledige modus verlagen naar de modus zonder agent of licht gewicht?**

Nee. Het downgrade van de beheer baarheids modus van de SQL IaaS-extensie wordt niet ondersteund. De modus voor SQL-beheer baarheid kan niet worden gedowngraded van de modus Full naar Lightweight of no-agent. en kan niet worden overdowngrade van de modus licht naar geen agent. De beheer baarheids modus wijzigen van volledig beheer; Verwijder de SQL IaaS-extensie; Verwijder de resource Micorsoft. SqlVirtualMachine en registreer SQL Server VM opnieuw bij de resource provider van de SQL-VM.

**Kan ik bij Azure Portal van de resource provider van een SQL-VM registreren?**

Nee. Registratie met de resource provider van de SQL-VM is niet beschikbaar in de Azure Portal. Registreren met de resource provider voor de SQL-VM in de volledige beheer modus wordt ondersteund met Azure CLI of Power shell. en registreren met de resource provider van de SQL-VM in Lightweight of de beheer modus zonder agent wordt alleen ondersteund door Azure PowerShell-Api's.

**Kan ik een virtuele machine met een SQL-VM-resource provider registreren voordat SQL Server wordt geïnstalleerd?**

Nee. Voor de virtuele machine moet ten minste één SQL-exemplaar zijn geregistreerd bij de resource provider van de SQL-VM. Als er geen SQL-exemplaar op de virtuele machine aanwezig is, heeft de nieuwe Micosoft. SqlVirtualMachine-resource de status mislukt.

**Kan ik een virtuele machine met een SQL-VM-resource registreren als er meerdere SQL-exemplaren zijn?**

Ja. De resource provider van de SQL-VM registreert slechts één SQL-exemplaar. De resource provider van de SQL-VM registreert het standaard SQL-exemplaar in het geval van meerdere exemplaren. Als er geen standaard exemplaar is, wordt alleen de registratie in Lightweight-modus ondersteund. Als u een upgrade wilt uitvoeren van Lightweight naar full managed-modus, moet het SQL-standaard exemplaar bestaan of moet de virtuele machine slechts één benoemd SQL-exemplaar hebben.

**Kan ik een SQL Server-failovercluster registreren met een SQL-VM-resource provider?**

Ja. SQL FCI-exemplaren op een virtuele Azure-machine kunnen worden geregistreerd bij de resource provider van de SQL-VM in de Lightweight-modus. SQL FCI-exemplaren kunnen echter niet worden bijgewerkt naar de volledige beheer modus.

**Kan ik mijn VM registreren met de SQL-VM RP als AlwaysOn-beschikbaarheids groep is geconfigureerd?**

Ja. Er zijn geen beperkingen voor het registreren van een SQL Server-exemplaar op een virtuele machine van Azure met SQL-VM-resource provider als deel nemen aan een AlwaysOn-beschikbaarheids groep configuratie.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer informatie de volgende artikelen: 

* [Overzicht van SQL Server op een Windows-VM](virtual-machines-windows-sql-server-iaas-overview.md)
* [Veelgestelde vragen over Windows-VM'S SQL Server](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server over de prijs informatie voor Windows-VM'S](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Opmerkingen bij de release van een Windows-VM SQL Server](virtual-machines-windows-sql-server-iaas-release-notes.md)
