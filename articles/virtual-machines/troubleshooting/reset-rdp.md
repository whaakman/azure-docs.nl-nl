---
title: Het wachtwoord of de configuratie van extern bureaublad op een Windows-VM opnieuw instellen | Microsoft Docs
description: Informatie over het opnieuw instellen van wachtwoord van een account of extern bureaublad-services op een Windows-VM met behulp van de Azure portal of Azure PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 03/23/2018
ms.author: cynthn
ms.openlocfilehash: a8db7ef82136bae51c99bcfd2a4743e09ebf5712
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47413540"
---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>De extern bureaublad-service of het aanmeldingswachtwoord op een Windows-VM opnieuw instellen
Als u geen verbinding maken met een Windows virtuele machine (VM), kunt u het lokale administrator-wachtwoord opnieuw instellen of opnieuw instellen van de configuratie van de extern bureaublad-service (niet ondersteund op Windows-domeincontrollers). U kunt de Azure-portal of de extensie voor VM-toegang in Azure PowerShell om het wachtwoord opnieuw in te gebruiken. Nadat u de virtuele machine hebt aangemeld, moet u het wachtwoord voor die gebruiker opnieuw instellen.  
Als u PowerShell gebruikt, zorg ervoor dat u hebt de [meest recente PowerShell-module geïnstalleerd en geconfigureerd](/powershell/azure/overview) en bent aangemeld bij uw Azure-abonnement. U kunt ook [deze stappen uitvoeren voor virtuele machines die zijn gemaakt met het klassieke implementatiemodel](https://docs.microsoft.com/azure/virtual-machines/windows/classic/reset-rdp).

## <a name="ways-to-reset-configuration-or-credentials"></a>Manieren om te configureren of de referenties opnieuw instellen
U kunt Extern bureaublad-services en referenties herstellen in een aantal verschillende manieren, afhankelijk van uw behoeften:

- [Opnieuw instellen met behulp van de Azure portal](#azure-portal)
- [Opnieuw instellen met behulp van Azure PowerShell](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Azure Portal
Om uit te breiden de portal-menu, klikt u op de drie balken in de linkerbovenhoek en klik vervolgens op **virtuele machines**:

![Bladeren naar uw Azure-VM](./media/reset-rdp/Portal-Select-VM.png)

### <a name="reset-the-local-administrator-account-password"></a>**Het lokale administrator-accountwachtwoord opnieuw instellen**

Selecteer uw Windows-machine en klik vervolgens op **ondersteuning + probleemoplossing** > **wachtwoord opnieuw instellen**. De blade wachtwoord opnieuw instellen wordt weergegeven:

![Pagina voor het opnieuw instellen van wachtwoorden](./media/reset-rdp/Portal-RM-PW-Reset-Windows.png)

Voer de gebruikersnaam en een nieuw wachtwoord en klik op **Update**. Probeer opnieuw verbinding te maken met uw virtuele machine.

### <a name="reset-the-remote-desktop-service-configuration"></a>**De configuratie van extern bureaublad-service opnieuw instellen**

Selecteer uw Windows-machine en klik vervolgens op **ondersteuning + probleemoplossing** > **wachtwoord opnieuw instellen**. De blade wachtwoord opnieuw instellen wordt weergegeven. 

![RDP-configuratie opnieuw instellen](./media/reset-rdp/Portal-RM-RDP-Reset.png)

Selecteer **alleen configuratie opnieuw instellen van** uit de vervolgkeuzelijst, klik vervolgens op **Update**. Probeer opnieuw verbinding te maken met uw virtuele machine.


## <a name="vmaccess-extension-and-powershell"></a>VMAccess-extensie en PowerShell
Zorg ervoor dat u hebt de [meest recente PowerShell-module geïnstalleerd en geconfigureerd](/powershell/azure/overview) en bent aangemeld bij uw Azure-abonnement met de `Connect-AzureRmAccount` cmdlet.

### <a name="reset-the-local-administrator-account-password"></a>**Het lokale administrator-accountwachtwoord opnieuw instellen**
Opnieuw instellen van wachtwoord of gebruikersnaam naam van de beheerder met de [Set AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-cmdlet. De typeHandlerVersion moet 2.0 of hoger, als u versie 1 is afgeschaft. 

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
> Als u een andere naam dan de huidige lokale administrator-account op de virtuele machine, wordt de VMAccess-extensie toevoegen van een lokale administrator-account met die naam en het opgegeven wachtwoord toewijzen aan dat account. Als het lokale beheerdersaccount op de virtuele machine bestaat, wordt het wachtwoord wordt opnieuw kunt instellen en als het account is uitgeschakeld, ingeschakeld de VMAccess-extensie.

### <a name="reset-the-remote-desktop-service-configuration"></a>**De configuratie van extern bureaublad-service opnieuw instellen**
Externe toegang opnieuw instellen met uw virtuele machine met de [Set AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-cmdlet. Het volgende voorbeeld wordt de extensie voor toegang met de naam `myVMAccess` op de virtuele machine met de naam `myVM` in de `myResourceGroup` resourcegroep:

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> Een virtuele machine kunt op elk gewenst moment kunt slechts één VM access-agent hebben. Agenteigenschappen in te stellen de toegang tot de VM, de `-ForceRerun` optie kan worden gebruikt. Bij het gebruik van `-ForceRerun`, zorg ervoor dat u dezelfde naam voor de virtuele machine toegang agent zoals gebruikt in alle vorige opdrachten.

Als u nog steeds kan niet extern verbinding maken met uw virtuele machine, ziet u meer stappen om te proberen op [problemen oplossen met extern bureaublad-verbindingen met een op basis van Windows Azure-machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Als u verbinding met de Windows-domeincontroller verliest, moet u deze herstellen vanuit een back-up van domain controller.

## <a name="next-steps"></a>Volgende stappen
Als de uitbreiding van de toegang tot Azure-VM niet reageert en niet het wachtwoord opnieuw instellen, kunt u [opnieuw instellen van de lokale Windows-wachtwoord offline](../windows/reset-local-password-without-agent.md). Deze methode is een meer geavanceerde proces en moet u de virtuele harde schijf van de problematische virtuele machine verbinden met een andere virtuele machine. Volg de stappen beschreven in dit artikel eerst en proberen alleen de methode offline wachtwoord opnieuw instellen als laatste redmiddel.

[Azure VM-extensies en functies](../extensions/features-windows.md)

[Verbinding maken met een Azure-machine met RDP of SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Problemen oplossen met extern bureaublad-verbindingen met een op basis van Windows Azure-machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

