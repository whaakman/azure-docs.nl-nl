---
title: Virtuele machine-extensies en functies voor Windows in Azure | Microsoft Docs
description: Meer informatie over welke extensies beschikbaar zijn voor virtuele machines in Azure, gegroepeerd op wat ze bieden of verbeteren.
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 999d63ee-890e-432e-9391-25b3fc6cde28
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/06/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1ce0eebd2585c9457d7f922898d7f2fa3e7ffad7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="virtual-machine-extensions-and-features-for-windows"></a>Virtuele machine-extensies en functies voor Windows

Virtuele machine van Azure-extensies zijn kleine toepassingen waarmee de configuratie en automatisering taken na de implementatie op virtuele machines in Azure. Als een virtuele machine vereist een software-installatie, beveiliging tegen virussen of Docker-configuratie, kan er bijvoorbeeld een VM-extensie worden gebruikt om deze taken te voltooien. Azure VM-extensies kunnen worden uitgevoerd met behulp van de Azure CLI, PowerShell, Azure resourcemanager-sjablonen en de Azure-portal. Uitbreidingen worden gebundeld met een nieuwe implementatie van de virtuele machine of eventuele bestaande systeem uitgevoerd.

Dit document bevat een overzicht van de virtuele machine-extensies vereisten voor het gebruik van uitbreidingen van de virtuele machine en richtlijnen over het detecteren, beheren en verwijderen van uitbreidingen van de virtuele machine. Dit document vindt u algemene informatie omdat veel VM-extensies beschikbaar zijn, elk met een potentieel unieke configuratie. Extensie-specifieke informatie vindt u in elk document specifiek is voor de afzonderlijke extensie.

## <a name="use-cases-and-samples"></a>Gebruiksvoorbeelden en voorbeelden

Er zijn veel verschillende Azure VM-extensies beschikbaar, elk met een specifieke aanvraag gebruikt. Er zijn enkele gebruiksvoorbeelden:

- Status van PowerShell gewenste configuraties toepassen op een virtuele machine met behulp van de DSC-uitbreiding voor Windows. Zie voor meer informatie [Azure gewenst State configuration-uitbreiding](extensions-dsc-overview.md).
- Configureer de bewaking van de virtuele machine met behulp van de Microsoft Monitoring Agent VM-extensie. Zie voor meer informatie [verbinding maken met Azure virtuele machines met logboekanalyse](../../log-analytics/log-analytics-azure-vm-extension.md).
- Bewaking van uw Azure-infrastructuur met de extensie Datadog configureren. Zie voor meer informatie de [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).
- Een virtuele machine van Azure met behulp van Chef configureren. Zie voor meer informatie [Azure automatisering van de implementatie van de virtuele machine met Chef](chef-automation.md).

Een aangepast Script-uitbreiding is naast processpecifieke uitbreidingen beschikbaar voor Windows- en Linux virtuele machines. De extensie voor aangepaste scripts voor Windows kunt een PowerShell-script worden uitgevoerd op een virtuele machine. Dit is handig bij het ontwerpen van Azure-implementaties die moeten worden geconfigureerd afgezien van wat systeemeigen Azure tooling kan bieden. Zie voor meer informatie [Windows VM aangepaste scriptextensie](extensions-customscript.md).


## <a name="prerequisites"></a>Vereisten

De extensie van elke virtuele machine mogelijk een eigen set vereisten. De Docker-VM-extensie heeft bijvoorbeeld een vereiste van een ondersteunde Linux-distributie. Vereisten van afzonderlijke uitbreidingen worden beschreven in de documentatie van extensie-specifieke.

### <a name="azure-vm-agent"></a>Azure VM-agent
De Azure VM-agent beheert interactie tussen Azure een virtuele machine en de domeincontroller van de Azure-infrastructuur. De VM-agent is verantwoordelijk voor veel functionele aspecten van het implementeren en beheren van virtuele machines in Azure, waaronder het uitvoeren van de VM-extensies. De Azure VM-agent is geïnstalleerd op Azure Marketplace-installatiekopieën en kan worden geïnstalleerd op ondersteunde besturingssystemen.

Zie voor informatie over ondersteunde besturingssystemen en installatie-instructies, [virtuele machine van Azure-agent](agent-user-guide.md).

## <a name="discover-vm-extensions"></a>VM-extensies detecteren
Veel andere VM-extensies zijn beschikbaar voor gebruik met virtuele machines in Azure. Voer de volgende opdracht met de Azure Resource Manager PowerShell-module voor een volledig overzicht. Zorg ervoor dat de gewenste locatie opgeven wanneer u deze opdracht uitvoert.

```powershell
Get-AzureRmVmImagePublisher -Location WestUS | `
Get-AzureRmVMExtensionImageType | `
Get-AzureRmVMExtensionImage | Select Type, Version
```

## <a name="run-vm-extensions"></a>VM-extensies worden uitgevoerd

Uitbreidingen van de virtuele machine van Azure kunnen worden uitgevoerd op de bestaande virtuele machines die is handig als u wilt configuratiewijzigingen aanbrengen of connectiviteit op een reeds geïmplementeerde virtuele machine herstellen. VM-extensies kunnen ook worden gebundeld met implementaties van Azure Resource Manager-sjabloon. Via uitbreidingen met Resource Manager-sjablonen kunt u virtuele machines kunnen worden geïmplementeerd en geconfigureerd zonder tussenkomst van de na de implementatie van Azure inschakelen.

De volgende manieren kunnen worden gebruikt voor het uitvoeren van een uitbreiding op een bestaande virtuele machine.

### <a name="powershell"></a>PowerShell

Er bestaan verschillende PowerShell-opdrachten voor het uitvoeren van afzonderlijke uitbreidingen. Voer de volgende PowerShell-opdrachten voor een overzicht.

```powershell
get-command Set-AzureRM*Extension* -Module AzureRM.Compute
```

Dit biedt de uitvoer ziet er als volgt:

```powershell
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Set-AzureRmVMAccessExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMADDomainExtension                     2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMAEMExtension                          2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBackupExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBginfoExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMChefExtension                         2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMCustomScriptExtension                 2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiagnosticsExtension                  2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiskEncryptionExtension               2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDscExtension                          2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMExtension                             2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMSqlServerExtension                    2.2.0      AzureRM.Compute
```

Het volgende voorbeeld wordt de extensie voor aangepaste scripts voor het downloaden van een script van een GitHub-opslagplaats naar de doel-virtuele machine en voer het script. Zie voor meer informatie over de aangepaste scriptextensie [extensieoverzicht aangepast Script](extensions-customscript.md).

```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" -Name "myCustomScript" `
    -FileUri "https://raw.githubusercontent.com/neilpeterson/nepeters-azure-templates/master/windows-custom-script-simple/support-scripts/Create-File.ps1" `
    -Run "Create-File.ps1" -Location "West US"
```

In dit voorbeeld wordt de toegang van de VM-extensie gebruikt om in te stellen het beheerderswachtwoord van een virtuele machine van Windows. Zie voor meer informatie over de toegang van de VM-extensie [opnieuw instellen van extern bureaublad-service in een Windows-VM](reset-rdp.md).

```powershell
$cred=Get-Credential

Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myVMAccess" `
    -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

De `Set-AzureRmVMExtension` opdracht kan worden gebruikt voor het starten van een VM-extensie. Zie voor meer informatie de [Set AzureRmVMExtension verwijzing](https://msdn.microsoft.com/en-us/library/mt603745.aspx).


### <a name="azure-portal"></a>Azure Portal

Een VM-extensie kan worden toegepast op een bestaande virtuele machine via de Azure portal. Om dit te doen, selecteert u de virtuele machine die u wilt gebruiken, kiest u **extensies**, en klik op **toevoegen**. Dit biedt een lijst met beschikbare uitbreidingen. Selecteer de gewenste en volg de stappen in de wizard.

De volgende afbeelding ziet u de installatie van de Microsoft Antimalware-extensie van de Azure-portal.

![Antimalware-uitbreiding installeren](./media/extensions-features/installantimalwareextension.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen

VM-extensies worden toegevoegd aan een Azure Resource Manager-sjabloon en uitgevoerd met de implementatie van de sjabloon. Uitbreidingen met een sjabloon implementeren is handig voor het volledig geconfigureerde Azure-implementaties maken. Bijvoorbeeld is de volgende JSON afkomstig van een Resource Manager-sjabloon die u implementeert een set van taakverdeling op virtuele machines en een Azure SQL database en installeert een toepassing .NET Core op elke virtuele machine. De VM-extensie zorgt voor de software-installatie.

Zie voor meer informatie de [volledige Resource Manager-sjabloon](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

Zie voor meer informatie [Azure Resource Manager-sjablonen samenstellen met Windows VM-extensies](template-description.md#extensions).

## <a name="secure-vm-extension-data"></a>Beveiligde gegevens van de VM-extensie

Wanneer u een VM-extensie uitvoert, is het mogelijk met gevoelige informatie zoals referenties, namen van opslagaccounts en toegangssleutels voor opslag-account nodig. Veel VM-extensies bevatten een beveiligde configuratie die gegevens versleutelt en ontsleutelt deze alleen in de doel-virtuele machine. Elke uitbreiding heeft een specifieke beveiligde configuratieschema die worden uiteengezet in de documentatie van de extensie-specifieke.

Het volgende voorbeeld ziet een exemplaar van de extensie voor aangepaste scripts voor Windows. U ziet dat de opdracht voor het uitvoeren van een set referenties bevat. In dit voorbeeld de opdracht wordt uitgevoerd niet versleuteld.


```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ],
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

Beveiligen van de tekenreeks kan worden uitgevoerd door het verplaatsen van de **opdracht wordt uitgevoerd** eigenschap in op de **beveiligd** configuratie.

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

## <a name="troubleshoot-vm-extensions"></a>VM-extensies oplossen

Elk VM-extensie wellicht specifieke stappen voor probleemoplossing. Bijvoorbeeld, wanneer u de extensie voor aangepaste scripts, script uitvoering details zijn te vinden lokaal op de virtuele machine waarop de uitbreiding is uitgevoerd. Probleemoplossing-extensie-specifieke worden beschreven in de documentatie van de extensie-specifieke.

De volgende stappen van toepassing op alle uitbreidingen van de virtuele machine.

### <a name="view-extension-status"></a>Status van de extensie weergeven

Nadat de extensie van een virtuele machine is uitgevoerd voor een virtuele machine, moet u de volgende PowerShell-opdracht gebruiken om te retourneren van de status van extensie. Parameternamen voorbeeld vervangen door uw eigen waarden. De `Name` parameter heeft de naam van de extensie tijdens het uitvoeren.

```PowerShell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

De uitvoer ziet er als volgt:

```json
ResourceGroupName       : myResourceGroup
VMName                  : myVM
Name                    : myExtensionName
Location                : westus
Etag                    : null
Publisher               : Microsoft.Azure.Extensions
ExtensionType           : DockerExtension
TypeHandlerVersion      : 1.0
Id                      : /subscriptions/mySubscriptionIS/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM/extensions/myExtensionName
PublicSettings          :
ProtectedSettings       :
ProvisioningState       : Succeeded
Statuses                :
SubStatuses             :
AutoUpgradeMinorVersion : False
ForceUpdateTag          :
```

Status van extensie-uitvoering kan worden gevonden in de Azure portal. Selecteer de virtuele machine om de status van een uitbreiding, weergeven, kiest u **extensies**, en selecteer de gewenste extensie.

### <a name="rerun-vm-extensions"></a>VM-extensies opnieuw uitvoeren

Mogelijk zijn er gevallen waarin de extensie van een virtuele machine moet opnieuw worden gestart. U kunt dit doen door de uitbreiding verwijderen en vervolgens opnieuw uit te voeren de uitbreiding met een methode van de uitvoering van uw keuze. Voer de volgende opdracht met de Azure PowerShell-module voor het verwijderen van extensie. Parameternamen voorbeeld vervangen door uw eigen waarden.

```powershell
Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Extensie kan ook worden verwijderd met de Azure portal. Dit doet u als volgt:

1. Selecteer een virtuele machine.
2. Selecteer **extensies**.
3. Kies de gewenste extensie.
4. Selecteer **verwijderen**.

## <a name="common-vm-extensions-reference"></a>Algemene referentie voor VM-extensies
| Naam van de uitbreiding | Beschrijving | Meer informatie |
| --- | --- | --- |
| Extensie voor aangepaste scripts voor Windows |Scripts uitvoeren op een virtuele machine van Azure |[Extensie voor aangepaste scripts voor Windows](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| DSC-extensie voor Windows |PowerShell DSC (Desired State Configuration)-extensie |[DSC-extensie voor Windows](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Azure Diagnostics-extensie |Azure Diagnostics beheren |[Azure Diagnostics-extensie](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Uitbreiding van de toegang tot Azure VM |Gebruikers en referenties beheren |[Uitbreiding van de VM-toegang voor Linux](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
