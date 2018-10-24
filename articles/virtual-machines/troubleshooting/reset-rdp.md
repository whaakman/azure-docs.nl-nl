---
title: Extern bureaublad-Services of de administrator-wachtwoord in een Windows-VM opnieuw instellen | Microsoft Docs
description: Leer hoe u een wachtwoord of de extern bureaublad-Services op een Windows-VM opnieuw instellen met behulp van de Azure portal of Azure PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 10/23/2018
ms.author: genli
ms.openlocfilehash: 470834da9e9a571594789dedcbb0a67b55abd799
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49953709"
---
# <a name="reset-remote-desktop-services-or-its-administrator-password-in-a-windows-vm"></a>Extern bureaublad-Services of de administrator-wachtwoord in een Windows-VM opnieuw instellen
Als u geen verbinding maken met een Windows virtuele machine (VM), kunt u uw lokale administrator-wachtwoord opnieuw instellen of opnieuw instellen van de extern bureaublad-Services-configuratie (niet ondersteund op Windows-domeincontrollers). Als u wilt het wachtwoord opnieuw instellen, kunt u Azure portal of de extensie voor VM-toegang in Azure PowerShell gebruiken. Nadat u zich hebt aangemeld bij de virtuele machine, het wachtwoord opnieuw instellen voor de desbetreffende lokale beheerder.  
Als u PowerShell gebruikt, zorg ervoor dat u hebt de [meest recente PowerShell-module geïnstalleerd en geconfigureerd](/powershell/azure/overview) en bent aangemeld bij uw Azure-abonnement. U kunt ook [deze stappen uitvoeren voor virtuele machines die zijn gemaakt met het klassieke implementatiemodel](https://docs.microsoft.com/azure/virtual-machines/windows/classic/reset-rdp).

U kunt Extern bureaublad-Services en de referenties opnieuw instellen in de volgende manieren:

- [Opnieuw instellen met behulp van Azure portal](#reset-by-using-the-azure-portal)
- [Opnieuw instellen met behulp van de VMAccess-extensie en PowerShell](#reset-by-using-the-vmaccess-extension-and-powershell)

## <a name="reset-by-using-the-azure-portal"></a>Opnieuw instellen met behulp van Azure portal
Meld u aan bij Azure portal en selecteer vervolgens **virtuele machines** in het menu links.

### <a name="reset-the-local-administrator-account-password"></a>**Het lokale administrator-accountwachtwoord opnieuw instellen**

Selecteer uw Windows-VM en selecteer vervolgens **wachtwoord opnieuw instellen** onder **ondersteuning + probleemoplossing**. De **wachtwoord opnieuw instellen** venster wordt weergegeven.

Selecteer **wachtwoord opnieuw instellen**, voer een gebruikersnaam en wachtwoord en selecteer vervolgens **Update**. Probeer opnieuw verbinding te maken met uw virtuele machine.

### <a name="reset-the-remote-desktop-services-configuration"></a>**De configuratie van extern bureaublad-Services opnieuw instellen**

Selecteer uw Windows-VM en selecteer vervolgens **wachtwoord opnieuw instellen** onder **ondersteuning + probleemoplossing**. De **wachtwoord opnieuw instellen** venster wordt weergegeven. 

Selecteer **alleen configuratie opnieuw instellen van** en selecteer vervolgens **Update**. Probeer opnieuw verbinding te maken met uw virtuele machine.


## <a name="reset-by-using-the-vmaccess-extension-and-powershell"></a>Opnieuw instellen met behulp van de VMAccess-extensie en PowerShell
Zorg ervoor dat u hebt de [meest recente PowerShell-module geïnstalleerd en geconfigureerd](/powershell/azure/overview) en bent aangemeld bij uw Azure-abonnement met behulp van de [Connect-AzureRmAccount](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet.

### <a name="reset-the-local-administrator-account-password"></a>**Het lokale administrator-accountwachtwoord opnieuw instellen**
Opnieuw instellen van wachtwoord of gebruikersnaam naam van de beheerder met de [Set AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-cmdlet. De `typeHandlerVersion` instelling moet worden 2.0 of hoger, omdat de versie 1 is afgeschaft. 

```powershell
$SubID = "<SUBSCRIPTION ID>" 
$RgName = "<RESOURCE GROUP NAME>" 
$VmName = "<VM NAME>" 
$Location = "<LOCATION>" 
 
Connect-AzureRmAccount 
Select-AzureRMSubscription -SubscriptionId $SubID 
Set-AzureRmVMAccessExtension -ResourceGroupName $RgName -Location $Location -VMName $VmName -Credential (get-credential) -typeHandlerVersion "2.0" -Name VMAccessAgent 
```

> [!NOTE] 
> Als u een andere naam dan de huidige lokale administrator-account op de virtuele machine invoeren, wordt de VMAccess-extensie toevoegen van een lokale administrator-account met die naam en het opgegeven wachtwoord toewijzen aan dat account. Als het lokale beheerdersaccount op de virtuele machine bestaat, wordt de VMAccess-extensie het wachtwoord opnieuw instellen. Als het account is uitgeschakeld, wordt dit door de VMAccess-extensie inschakelen.

### <a name="reset-the-remote-desktop-services-configuration"></a>**De configuratie van extern bureaublad-Services opnieuw instellen**
Externe toegang opnieuw instellen met uw virtuele machine met de [Set AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-cmdlet. Het volgende voorbeeld wordt de extensie voor toegang met de naam `myVMAccess` op de virtuele machine met de naam `myVM` in de `myResourceGroup` resourcegroep:

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> Een virtuele machine kunt op elk gewenst moment kunt slechts één VM access-agent hebben. U kunt de VM-toegang Agenteigenschappen instellen met de `-ForceRerun` optie. Bij het gebruik `-ForceRerun`, zorg ervoor dat u dezelfde naam voor de virtuele machine toegang-agent die u hebt gebruikt in alle vorige opdrachten.

Als u nog steeds kan niet extern verbinding maken met uw virtuele machine, Zie [problemen oplossen met extern bureaublad-verbindingen met een op basis van Windows Azure-machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Als u de verbinding met de Windows-domeincontroller verliest, moet u deze herstellen vanuit een back-up van domain controller.

## <a name="next-steps"></a>Volgende stappen
Als de uitbreiding van de toegang tot Azure-VM niet reageert en niet het wachtwoord opnieuw instellen, kunt u [opnieuw instellen van de lokale Windows-wachtwoord offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Deze methode is geavanceerdere en moet u de virtuele harde schijf van de problematische virtuele machine verbinden met een andere virtuele machine. Volg de stappen beschreven in dit artikel eerst en probeert u de offline wachtwoord opnieuw instellen van methode alleen als u deze stappen niet werken.

[Meer informatie over Azure VM-extensies en functies](../extensions/features-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Verbinding maken met een Azure-machine met RDP of SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Problemen oplossen met extern bureaublad-verbindingen met een op basis van Windows Azure-machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

