---
title: Opnieuw instellen van het wachtwoord of de configuratie van extern bureaublad op een Windows-VM in Azure | Microsoft Docs
description: Leer hoe u een wachtwoord opnieuw instellen of extern bureaublad-services op een Windows-VM gemaakt met behulp van het klassieke implementatiemodel met behulp van de Azure portal of Azure PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: cynthn
ms.openlocfilehash: bbe8059b3a239570c2c9b25586dae9adbe25312d
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37931375"
---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm-created-using-the-classic-deployment-model"></a>Opnieuw instellen van de extern bureaublad-service of het aanmeldingswachtwoord in een Windows-VM gemaakt met behulp van het klassieke implementatiemodel
> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. U kunt ook [deze stappen uitvoeren voor virtuele machines die zijn gemaakt met het Resource Manager-implementatiemodel](../reset-rdp.md).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]


Als u geen verbinding maken met een Windows virtuele machine (VM), kunt u het lokale administrator-wachtwoord opnieuw instellen of opnieuw instellen van de configuratie van de extern bureaublad-service. U kunt de Azure-portal of de extensie voor VM-toegang in Azure PowerShell om het wachtwoord opnieuw in te gebruiken.

## <a name="ways-to-reset-configuration-or-credentials"></a>Manieren om te configureren of de referenties opnieuw instellen
U kunt Extern bureaublad-services en referenties herstellen in een aantal verschillende manieren, afhankelijk van uw behoeften:

- [Opnieuw instellen met behulp van de Azure portal](#azure-portal)
- [Opnieuw instellen met behulp van Azure PowerShell](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Azure Portal
U kunt de [Azure-portal](https://portal.azure.com) opnieuw instellen van de extern bureaublad-service. Om uit te breiden de portal-menu, klikt u op de drie balken in de linkerbovenhoek en klik vervolgens op **virtuele machines (klassiek)**:

![Bladeren naar uw Azure-VM](./media/reset-rdp/Portal-Select-Classic-VM.png)

Selecteer uw Windows-machine en klik vervolgens op **extern opnieuw instellen...** . Het volgende dialoogvenster wordt weergegeven de extern bureaublad-configuratie opnieuw instellen:

![Pagina voor RDP-configuratie opnieuw instellen](./media/reset-rdp/Portal-RDP-Reset-Windows.png)

U kunt ook de gebruikersnaam en het wachtwoord van het lokale administrator-account opnieuw. Uw virtuele machine, klik op **ondersteuning + probleemoplossing** > **wachtwoord opnieuw instellen**. De blade wachtwoord opnieuw instellen wordt weergegeven:

![Pagina voor het opnieuw instellen van wachtwoorden](./media/reset-rdp/Portal-PW-Reset-Windows.png)

Nadat u de nieuwe gebruikersnaam en wachtwoord opgeven, klikt u op **opslaan**.

## <a name="vmaccess-extension-and-powershell"></a>VMAccess-extensie en PowerShell
Zorg ervoor dat de VM-Agent is geïnstalleerd op de virtuele machine. De VMAccess-extensie hoeven niet te worden geïnstalleerd voordat u deze kunt gebruiken, zolang de VM-Agent beschikbaar is. Controleren of de VM-Agent al is geïnstalleerd met behulp van de volgende opdracht uit. (Vervang 'myCloudService' en 'myVM' door de namen van uw cloudservice en uw virtuele machine, respectievelijk. U kunt informatie over deze namen door te voeren `Get-AzureVM` zonder parameters.)

```powershell
$vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
write-host $vm.VM.ProvisionGuestAgent
```

Als de **write-host** opdracht geeft **waar**, de VM-Agent is geïnstalleerd. Als deze wordt weergegeven **False**, Zie de instructies en een koppeling naar de download in de [VM-Agent en -extensies - deel 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure-blogbericht.

Als u de virtuele machine met behulp van de portal hebt gemaakt, controleert u of `$vm.GetInstance().ProvisionGuestAgent` retourneert **waar**. Als dat niet het geval is, kunt u dit instellen met behulp van deze opdracht:

```powershell
$vm.GetInstance().ProvisionGuestAgent = $true
```

Deze opdracht zorgt ervoor dat de volgende fout wanneer u de **Set AzureVMExtension** opdracht in de volgende stappen uit: "Inrichten Guest Agent moet zijn ingeschakeld op de VM-object voordat u de IaaS-VM-extensie voor toegang."

### <a name="reset-the-local-administrator-account-password"></a>**Het lokale administrator-accountwachtwoord opnieuw instellen**
Een referentie aanmelden met de naam van de huidige lokale administrator-account en een nieuw wachtwoord maken en voer vervolgens de `Set-AzureVMAccessExtension` als volgt.

```powershell
$cred=Get-Credential
Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password  | Update-AzureVM
```

Als u een andere naam dan het huidige account typt, de VMAccess-extensie wijzigt de naam van het lokale administrator-account, het wachtwoord toegewezen aan dit account, en problemen met een extern bureaublad afmelden. Als het lokale administrator-account is uitgeschakeld, wordt het door de VMAccess-extensie kunt.

Deze opdrachten worden ook de configuratie van de extern bureaublad-service opnieuw.

### <a name="reset-the-remote-desktop-service-configuration"></a>**De configuratie van extern bureaublad-service opnieuw instellen**
Als u wilt herstellen van de configuratie van de extern bureaublad-service, moet u de volgende opdracht uitvoeren:

```powershell
Set-AzureVMAccessExtension –vm $vm | Update-AzureVM
```

De VMAccess-extensie wordt uitgevoerd twee opdrachten op de virtuele machine:

```powershell
netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes
```

Met deze opdracht wordt de ingebouwde Windows Firewall-groep waarmee inkomend verkeer voor extern bureaublad, die gebruikmaakt van TCP-poort 3389.

```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0
```

Met deze opdracht wordt de fDenyTSConnections registerwaarde ingesteld op 0, het inschakelen van extern bureaublad-verbindingen.

## <a name="next-steps"></a>Volgende stappen
Als de uitbreiding van de toegang tot Azure-VM niet reageert en niet het wachtwoord opnieuw instellen, kunt u [opnieuw instellen van de lokale Windows-wachtwoord offline](../reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Deze methode is een meer geavanceerde proces en moet u de virtuele harde schijf van de problematische virtuele machine verbinden met een andere virtuele machine. Volg de stappen beschreven in dit artikel eerst en proberen alleen de methode offline wachtwoord opnieuw instellen als laatste redmiddel.

[Azure VM-extensies en functies](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Verbinding maken met een Azure-machine met RDP of SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Problemen oplossen met extern bureaublad-verbindingen met een op basis van Windows Azure-machine](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

