---
title: Opnieuw instellen van het wachtwoord of de configuratie van extern bureaublad op een virtuele machine van Windows | Microsoft Docs
description: Informatie over het opnieuw instellen van wachtwoord van een account of extern bureaublad-services op een Windows-VM met de Azure-portal of Azure PowerShell.
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 12/06/2017
ms.author: genli
ms.openlocfilehash: b5c6c6e06f6e4173730e6b030b86f443c58aa0f0
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Het opnieuw instellen van de extern bureaublad-service of het aanmeldingswachtwoord in een Windows-VM
Als u geen verbinding maken met een Windows virtuele machine (VM), kunt u het lokale administrator-wachtwoord opnieuw instellen of opnieuw instellen van de configuratie van de extern bureaublad-service (wordt niet ondersteund op Windows-domeincontrollers). U kunt de Azure portal of de toegang van de VM-extensie in Azure PowerShell om het wachtwoord opnieuw in te gebruiken. Als u met behulp van PowerShell, controleert u of u hebt de [nieuwste PowerShell-module geïnstalleerd en geconfigureerd](/powershell/azure/overview) en u bent aangemeld bij uw Azure-abonnement. U kunt ook [deze stappen uitvoert voor virtuele machines die zijn gemaakt met het klassieke implementatiemodel](https://docs.microsoft.com/azure/virtual-machines/windows/classic/reset-rdp).

## <a name="ways-to-reset-configuration-or-credentials"></a>Op de beginwaarden configuratie of referenties
U kunt Extern bureaublad-services en referenties herstellen op verschillende manieren, afhankelijk van uw behoeften:

- [Opnieuw instellen met de Azure-portal](#azure-portal)
- [Opnieuw instellen met Azure PowerShell](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Azure Portal
Als het menu van de portal wilt uitbreiden, klikt u op de drie balken in de linkerbovenhoek en klik vervolgens op **virtuele machines**:

![Blader naar uw Azure VM](./media/reset-rdp/Portal-Select-VM.png)

### <a name="reset-the-local-administrator-account-password"></a>**Het lokale beheerderswachtwoord opnieuw instellen**

Selecteer uw Windows-machine en klik vervolgens op **ondersteuning + probleemoplossing** > **wachtwoord opnieuw instellen**. De blade van wachtwoord opnieuw instellen wordt weergegeven:

![Pagina voor wachtwoordherstel](./media/reset-rdp/Portal-RM-PW-Reset-Windows.png)

De gebruikersnaam en nieuw wachtwoord invoeren en klik vervolgens op **Update**. Probeer opnieuw verbinding te maken met uw virtuele machine.

### <a name="reset-the-remote-desktop-service-configuration"></a>**De configuratie van extern bureaublad-service opnieuw instellen**

Selecteer uw Windows-machine en klik vervolgens op **ondersteuning + probleemoplossing** > **wachtwoord opnieuw instellen**. De blade van wachtwoord opnieuw instellen wordt weergegeven. 

![RDP-configuratie opnieuw instellen](./media/reset-rdp/Portal-RM-RDP-Reset.png)

Selecteer **alleen opnieuw instellen van configuratie** uit de vervolgkeuzelijst, klik vervolgens op **Update**. Probeer opnieuw verbinding te maken met uw virtuele machine.


## <a name="vmaccess-extension-and-powershell"></a>VMAccess-extensie en PowerShell
Zorg ervoor dat u hebt de [nieuwste PowerShell-module geïnstalleerd en geconfigureerd](/powershell/azure/overview) en u bent aangemeld bij uw Azure-abonnement met de `Login-AzureRmAccount` cmdlet.

### <a name="reset-the-local-administrator-account-password"></a>**Het lokale beheerderswachtwoord opnieuw instellen**
Opnieuw instellen van de naam van beheerder wachtwoord of gebruikersnaam met de [Set AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-cmdlet. Hiermee maakt u referenties voor uw account als volgt:

```powershell
$cred=Get-Credential
```

> [!NOTE] 
> Als u een andere naam dan het lokale beheerdersaccount op de virtuele machine typt, wordt de VMAccess-extensie toevoegen van een lokale administrator-account met die naam en het opgegeven wachtwoord toewijzen aan dit account. Als het lokale beheerdersaccount op de virtuele machine bestaat, wordt het herstellen van het wachtwoord en als het account is uitgeschakeld, ingeschakeld de VMAccess-extensie.


Het volgende voorbeeld wordt de virtuele machine met de naam bijgewerkt `myVM` in de resourcegroep met de naam `myResourceGroup` naar de opgegeven referenties.

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myVMAccess" -Location WestUS -UserName $cred.GetNetworkCredential().UserName -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

### <a name="reset-the-remote-desktop-service-configuration"></a>**De configuratie van extern bureaublad-service opnieuw instellen**
Externe toegang opnieuw instellen met uw virtuele machine met de [Set AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-cmdlet. Het volgende voorbeeld wordt de uitbreiding voor toegang met de naam `myVMAccess` op de virtuele machine met de naam `myVM` in de `myResourceGroup` resourcegroep:

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> Een virtuele machine kan slechts één virtuele machine toegang agent hebben op elk gewenst moment. De virtuele machine toegang heeft, Agenteigenschappen instellen de `-ForceRerun` optie kan worden gebruikt. Wanneer u `-ForceRerun`, zorg ervoor dat u dezelfde naam voor de virtuele machine toegang agent zoals gebruikt in alle vorige opdrachten gebruiken.

Als u nog steeds kan niet extern verbinding met uw virtuele machine maakt, raadpleegt u extra stappen uitvoeren om bij de [problemen met extern bureaublad-verbindingen met een op basis van Windows Azure virtuele machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


## <a name="next-steps"></a>Volgende stappen
Als u de uitbreiding van de toegang tot Azure VM reageert niet en kan het wachtwoord opnieuw instellen, kunt u [opnieuw instellen van het lokale Windows-wachtwoord offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Deze methode is een meer geavanceerde proces en moet u de virtuele harde schijf van de VM is problematisch verbinding met een andere virtuele machine. Volg de stappen beschreven in dit artikel eerst en probeert u alleen de methode offline wachtwoord opnieuw instellen als een laatste toevlucht.

[Azure VM-extensies en functies](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Verbinding maken met een virtuele machine van Azure met RDP of SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Problemen met extern bureaublad-verbindingen met een op basis van Windows Azure virtuele machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

