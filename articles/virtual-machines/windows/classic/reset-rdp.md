---
title: Opnieuw instellen van het wachtwoord of de configuratie van extern bureaublad op een Windows-virtuele machine in Azure | Microsoft Docs
description: Informatie over het opnieuw instellen van een wachtwoord of extern bureaublad-services op een Windows-virtuele machine gemaakt met behulp van het klassieke implementatiemodel met de Azure-portal of Azure PowerShell.
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: iainfou
ms.openlocfilehash: 43e5cf1ab3bc3121d7e3915ea0785998e0ee2fc6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm-created-using-the-classic-deployment-model"></a>Het opnieuw instellen van de extern bureaublad-service of het aanmeldingswachtwoord op een Windows-virtuele machine gemaakt met behulp van het klassieke implementatiemodel
> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. U kunt ook [deze stappen uitvoert voor virtuele machines die zijn gemaakt met het implementatiemodel van Resource Manager](../reset-rdp.md).

Als u geen verbinding maken met een Windows virtuele machine (VM), kunt u het lokale administrator-wachtwoord opnieuw instellen of opnieuw instellen van de configuratie van de extern bureaublad-service. U kunt de Azure portal of de toegang van de VM-extensie in Azure PowerShell om het wachtwoord opnieuw in te gebruiken.

## <a name="ways-to-reset-configuration-or-credentials"></a>Op de beginwaarden configuratie of referenties
U kunt Extern bureaublad-services en referenties herstellen op verschillende manieren, afhankelijk van uw behoeften:

- [Opnieuw instellen met de Azure-portal](#azure-portal)
- [Opnieuw instellen met Azure PowerShell](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Azure Portal
U kunt de [Azure-portal](https://portal.azure.com) opnieuw instellen van de extern bureaublad-service. Als het menu van de portal wilt uitbreiden, klikt u op de drie balken in de linkerbovenhoek en klik vervolgens op **virtuele machines (klassiek)**:

![Blader naar uw Azure VM](./media/reset-rdp/Portal-Select-Classic-VM.png)

Selecteer uw Windows-machine en klik op **extern opnieuw instellen...** . Het volgende dialoogvenster wordt weergegeven voor de extern bureaublad-configuratie opnieuw instellen:

![Pagina RDP-configuratie opnieuw instellen](./media/reset-rdp/Portal-RDP-Reset-Windows.png)

U kunt ook de gebruikersnaam en het wachtwoord van het lokale administrator-account herstellen. Van uw virtuele machine, klikt u op **ondersteuning + probleemoplossing** > **wachtwoord opnieuw instellen**. De blade van wachtwoord opnieuw instellen wordt weergegeven:

![Pagina voor wachtwoordherstel](./media/reset-rdp/Portal-PW-Reset-Windows.png)

Nadat u de nieuwe gebruikersnaam en het wachtwoord invoert, klikt u op **opslaan**.

## <a name="vmaccess-extension-and-powershell"></a>VMAccess-extensie en PowerShell
Zorg ervoor dat de VM-Agent is geïnstalleerd op de virtuele machine. De VMAccess-extensie hoeft niet te worden geïnstalleerd voordat u gebruiken kunt, zolang de VM-Agent beschikbaar is. Controleren of de VM-Agent al is geïnstalleerd met behulp van de volgende opdracht. (Vervang 'myCloudService' en 'myVM' door de namen van de cloudservice en de virtuele machine, respectievelijk. U kunt meer informatie over deze namen door het uitvoeren van `Get-AzureVM` zonder parameters.)

```powershell
$vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
write-host $vm.VM.ProvisionGuestAgent
```

Als de **write-host** opdracht geeft **True**, de VM-Agent is geïnstalleerd. Als deze wordt weergegeven in **False**, Zie de instructies en een koppeling naar het downloaden van de [VM-Agent en -extensies: Part 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure blogbericht.

Als u de virtuele machine met behulp van de portal hebt gemaakt, controleert u of `$vm.GetInstance().ProvisionGuestAgent` retourneert **True**. Als dat niet het geval is, kunt u dit instellen met behulp van deze opdracht:

```powershell
$vm.GetInstance().ProvisionGuestAgent = $true
```

Deze opdracht zorgt ervoor dat de volgende fout wanneer u de **Set AzureVMExtension** opdracht in de volgende stappen: "Inrichten Guest-Agent moet zijn ingeschakeld op het VM-object voordat u de uitbreiding voor IaaS VM-toegang."

### <a name="reset-the-local-administrator-account-password"></a>**Het lokale beheerderswachtwoord opnieuw instellen**
Een referentie aanmelden met de naam van de huidige lokale administrator-account en een nieuw wachtwoord maken en voer vervolgens de `Set-AzureVMAccessExtension` als volgt.

```powershell
$cred=Get-Credential
Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password  | Update-AzureVM
```

Als u een andere naam dan de huidige account typt, de VMAccess-extensie wijzigt de naam van het lokale administrator-account, wordt het wachtwoord wordt toegewezen aan dit account, en problemen met een extern bureaublad afmelding plaatsvindt. Als het lokale administrator-account is uitgeschakeld, schakelt u de VMAccess-extensie deze.

Deze opdrachten worden ook de configuratie van de extern bureaublad-service opnieuw.

### <a name="reset-the-remote-desktop-service-configuration"></a>**De configuratie van extern bureaublad-service opnieuw instellen**
Als u de configuratie van de extern bureaublad-service herstellen, moet u de volgende opdracht uitvoeren:

```powershell
Set-AzureVMAccessExtension –vm $vm | Update-AzureVM
```

De VMAccess-extensie twee opdrachten uitgevoerd op de virtuele machine:

```powershell
netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes
```

Deze opdracht kunt de ingebouwde Windows Firewall-groep waarmee binnenkomend verkeer voor extern bureaublad, die gebruikmaakt van TCP-poort 3389.

```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0
```

Met deze opdracht wordt de fDenyTSConnections registerwaarde ingesteld op 0, het inschakelen van extern bureaublad-verbindingen.

## <a name="next-steps"></a>Volgende stappen
Als u de uitbreiding van de toegang tot Azure VM reageert niet en kan het wachtwoord opnieuw instellen, kunt u [opnieuw instellen van het lokale Windows-wachtwoord offline](../reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Deze methode is een meer geavanceerde proces en moet u de virtuele harde schijf van de VM is problematisch verbinding met een andere virtuele machine. Volg de stappen beschreven in dit artikel eerst en probeert u alleen de methode offline wachtwoord opnieuw instellen als een laatste toevlucht.

[Azure VM-extensies en functies](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Verbinding maken met een virtuele machine van Azure met RDP of SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Problemen met extern bureaublad-verbindingen met een op basis van Windows Azure virtuele machine](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

