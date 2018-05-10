---
title: Azure VM-extensies en functies voor Windows | Microsoft Docs
description: Meer informatie over welke extensies beschikbaar zijn voor virtuele machines in Azure, gegroepeerd op wat ze bieden of verbeteren.
services: virtual-machines-windows
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 999d63ee-890e-432e-9391-25b3fc6cde28
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/30/2018
ms.author: danis
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 88852fe7843e24fde50749e2f994bcfeb596305d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="virtual-machine-extensions-and-features-for-windows"></a>Virtuele machine-extensies en functies voor Windows

Azure virtuele machine (VM) extensies zijn kleine toepassingen waarmee de configuratie en automatisering taken na de implementatie op Azure Virtual machines. Bijvoorbeeld, als een virtuele machine is vereist voor software-installatie, beveiliging tegen virussen, of een script daarbinnen uitvoeren, kan een VM-extensie kan worden gebruikt. Azure VM-extensies kunnen worden uitgevoerd met de Azure CLI, PowerShell, Azure resourcemanager-sjablonen en de Azure-portal. Uitbreidingen worden gebundeld met een nieuwe VM-implementatie of uitvoeren op een bestaande systeem.

In dit artikel biedt een overzicht van de VM-extensies voor vereisten voor het gebruik van Azure VM-extensies en instructies voor het detecteren, beheren en verwijderen van de VM-extensies. Dit artikel vindt u algemene informatie omdat veel VM-extensies beschikbaar zijn, elk met een potentieel unieke configuratie. Extensie-specifieke informatie vindt u in elk document specifiek is voor de afzonderlijke extensie.

## <a name="use-cases-and-samples"></a>Gebruiksvoorbeelden en voorbeelden

Enkele andere Azure VM-extensies beschikbaar zijn, elk met een specifieke gebruiksvoorbeeld. Voorbeelden zijn:

- Status van PowerShell gewenste configuraties toepassen op een virtuele machine met de DSC-extensie voor Windows. Zie voor meer informatie [Azure gewenst State configuration-uitbreiding](dsc-overview.md).
- Bewaking van een virtuele machine met de Microsoft Monitoring Agent VM-extensie te configureren. Zie voor meer informatie [Azure-machines verbinding maken met logboekanalyse](../../log-analytics/log-analytics-azure-vm-extension.md).
- Een Azure VM configureren met behulp van Chef. Zie voor meer informatie [automatiseren Azure VM-implementatie met Chef](../windows/chef-automation.md).
- Bewaking van uw Azure-infrastructuur met de extensie Datadog configureren. Zie voor meer informatie de [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).


Een aangepast Script-uitbreiding is naast processpecifieke uitbreidingen beschikbaar voor Windows- en Linux virtuele machines. De extensie voor aangepaste scripts voor Windows kunt een PowerShell-script worden uitgevoerd op een virtuele machine. Aangepaste scripts zijn handig voor het ontwerpen van Azure-implementaties die moeten worden geconfigureerd afgezien van wat systeemeigen Azure tooling kan bieden. Zie voor meer informatie [Windows VM aangepaste scriptextensie](custom-script-windows.md).

## <a name="prerequisites"></a>Vereisten

Voor het afhandelen van de uitbreiding op de virtuele machine, moet u de Azure Linux Agent is geïnstalleerd. Een aantal afzonderlijke uitbreidingen hebben vereisten, zoals toegang tot bronnen of afhankelijkheden.

### <a name="azure-vm-agent"></a>Azure VM-agent

De Azure VM-agent beheert interactie tussen een Azure-virtuele machine en de domeincontroller van de Azure-infrastructuur. De VM-agent is verantwoordelijk voor veel functionele aspecten van het implementeren en beheren van Azure Virtual machines, waaronder het uitvoeren van de VM-extensies. De Azure VM-agent is geïnstalleerd op Azure Marketplace-installatiekopieën en kan handmatig worden geïnstalleerd op ondersteunde besturingssystemen. De Azure VM-Agent voor Windows, wordt de Windows-gastagent genoemd.

Zie voor informatie over ondersteunde besturingssystemen en installatie-instructies, [virtuele machine van Azure-agent](agent-windows.md).

#### <a name="supported-agent-versions"></a>Ondersteunde agent-versies

Om de best mogelijke ervaring bieden, zijn er minimaal vereiste versies van de agent. Raadpleeg [dit artikel](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) voor meer informatie.

#### <a name="supported-oses"></a>Ondersteunde besturingssystemen

De Windows Guest-agent wordt uitgevoerd op meerdere besturingssystemen, maar het framework extensies een limiet voor de besturingssystemen die uitbreidingen kent. Zie voor meer informatie [in dit artikel] (https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems ).

Bepaalde extensies worden niet ondersteund in alle besturingssystemen en verzendt *fout Code 51, 'Niet-ondersteunde OS'*. Raadpleeg de documentatie van de afzonderlijke extensie voor ondersteuning.

#### <a name="network-access"></a>Netwerktoegang

-Extensiepakketten worden gedownload vanuit de opslagplaats van de extensie Azure Storage en extensie status uploads worden gepubliceerd naar Azure Storage. Als u [ondersteund](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) versie van de agents, hoeft u niet dat toegang tot Azure Storage in het gebied van de virtuele machine als de agent kan worden gebruikt om te leiden van de communicatie naar de controller Azure-infrastructuur voor communicatie van agent. Als u van een niet-ondersteunde versie van de agent gebruikmaakt, moet u uitgaand verkeer toestaan naar Azure-opslag in deze regio van de virtuele machine.

> [!IMPORTANT]
> Als u toegang tot hebben geblokkeerd *168.63.129.1* met de Gast-firewall, klikt u vervolgens extensies mislukken ongeacht de bovenstaande.

Agents kunnen alleen worden gebruikt voor het downloaden van extensiepakketten en rapporteringsstatus. Bijvoorbeeld als een installatie van de extensie moet een script downloaden vanuit GitHub (aangepast Script) of moet toegang tot Azure Storage (Azure Backup), klikt u vervolgens extra firewall/netwerk beveiliging groep poorten moeten worden geopend. Verschillende extensies hebben verschillende vereisten, omdat ze toepassingen in hun eigen rechts. Voor uitbreidingen waarvoor toegang tot Azure Storage, kunt u toegang met behulp van Azure NSG Service-Tags voor [opslag](https://docs.microsoft.com/en-us/azure/virtual-network/security-overview#service-tags).

De Gastagent Windows heeft geen ondersteuning voor het omleiden van aanvragen van de agent-verkeer via proxy-server.

## <a name="discover-vm-extensions"></a>VM-extensies detecteren

Veel andere VM-extensies zijn beschikbaar voor gebruik met Azure Virtual machines. Als een volledige lijst wilt weergeven, gebruikt [Get-AzureRmVMExtensionImage](/powershell/module/azurerm.compute/get-azurermvmextensionimage). Het volgende voorbeeld worden alle beschikbare uitbreidingen in de *WestUS* locatie:

```powershell
Get-AzureRmVmImagePublisher -Location "WestUS" | `
Get-AzureRmVMExtensionImageType | `
Get-AzureRmVMExtensionImage | Select Type, Version
```

## <a name="run-vm-extensions"></a>VM-extensies worden uitgevoerd

Azure VM-extensies worden uitgevoerd op de bestaande virtuele machines, die is handig als u wilt configuratiewijzigingen aanbrengen of connectiviteit op een reeds geïmplementeerde virtuele machine herstellen. VM-extensies kunnen ook worden gebundeld met implementaties van Azure Resource Manager-sjabloon. Via uitbreidingen met Resource Manager-sjablonen kunt virtuele Azure-machines worden geïmplementeerd en geconfigureerd zonder tussenkomst van de na de implementatie.

De volgende manieren kunnen worden gebruikt voor het uitvoeren van een uitbreiding op een bestaande virtuele machine.

### <a name="powershell"></a>PowerShell

Er bestaan verschillende PowerShell-opdrachten voor het uitvoeren van afzonderlijke uitbreidingen. Als een lijst wilt weergeven, gebruikt [Get-Command](/powershell/module/microsoft.powershell.core/get-command) en filtert u op *extensie*:

```powershell
Get-Command Set-AzureRM*Extension* -Module AzureRM.Compute
```

Dit biedt de uitvoer ziet er als volgt:

```powershell
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Set-AzureRmVMAccessExtension                       4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMADDomainExtension                     4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMAEMExtension                          4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBackupExtension                       4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBginfoExtension                       4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMChefExtension                         4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMCustomScriptExtension                 4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiagnosticsExtension                  4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiskEncryptionExtension               4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDscExtension                          4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMExtension                             4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMSqlServerExtension                    4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVmssDiskEncryptionExtension             4.5.0      AzureRM.Compute
```

Het volgende voorbeeld wordt de extensie voor aangepaste scripts voor het downloaden van een script van een GitHub-opslagplaats naar de doel-virtuele machine en voer het script. Zie voor meer informatie over de aangepaste scriptextensie [extensieoverzicht aangepast Script](custom-script-windows.md).

```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" -Name "myCustomScript" `
    -FileUri "https://raw.githubusercontent.com/neilpeterson/nepeters-azure-templates/master/windows-custom-script-simple/support-scripts/Create-File.ps1" `
    -Run "Create-File.ps1" -Location "West US"
```

In het volgende voorbeeld wordt de toegang van de VM-extensie gebruikt het beheerderswachtwoord van een virtuele Windows opnieuw wordt ingesteld op een tijdelijk wachtwoord. Zie voor meer informatie over de toegang van de VM-extensie [opnieuw instellen van extern bureaublad-service in een Windows-VM](../windows/reset-rdp.md). Als u dit hebt uitgevoerd, moet u bij de eerste aanmelding het wachtwoord opnieuw instellen:

```powershell
$cred=Get-Credential

Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myVMAccess" `
    -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

De `Set-AzureRmVMExtension` opdracht kan worden gebruikt voor het starten van een VM-extensie. Zie voor meer informatie de [Set AzureRmVMExtension verwijzing](https://msdn.microsoft.com/en-us/library/mt603745.aspx).


### <a name="azure-portal"></a>Azure Portal

VM-extensies kunnen worden toegepast op een bestaande virtuele machine via de Azure portal. Selecteer de virtuele machine in de portal, kies **extensies**, selecteer daarna **toevoegen**. Kies de extensie die u wilt dat in de lijst met beschikbare uitbreidingen en volg de instructies in de wizard.

Het volgende voorbeeld ziet u de installatie van de Microsoft Antimalware-extensie van de Azure-portal:

![Antimalware-uitbreiding installeren](./media/features-windows/installantimalwareextension.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen

VM-extensies worden toegevoegd aan een Azure Resource Manager-sjabloon en uitgevoerd met de implementatie van de sjabloon. Wanneer u een uitbreiding met een sjabloon implementeert, kunt u volledig geconfigureerde Azure-implementaties. Bijvoorbeeld de volgende JSON wordt gemaakt van een Resource Manager sjabloon implementeert een set van netwerktaakverdeling VM's en een Azure SQL database en vervolgens installeert u een .NET Core-toepassing op elke virtuele machine. De VM-extensie zorgt voor de software-installatie.

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
    "typeHandlerVersion": "1.9",
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

Zie voor meer informatie over het maken van Resource Manager-sjablonen [Azure Resource Manager-sjablonen samenstellen met Windows VM-extensies](../windows/template-description.md#extensions).

## <a name="secure-vm-extension-data"></a>Beveiligde gegevens van de VM-extensie

Wanneer u een VM-extensie uitvoert, is het mogelijk nodig is met gevoelige informatie zoals referenties, namen van opslagaccounts en toegangssleutels voor opslag-account. Veel VM-extensies bevatten een beveiligde configuratie die gegevens versleutelt en ontsleutelt deze alleen binnen het doel-virtuele machine. Elke uitbreiding heeft een specifieke beveiligde configuratieschema en elk wordt beschreven in de uitbreiding specifieke documentatie bij.

Het volgende voorbeeld ziet een exemplaar van de extensie voor aangepaste scripts voor Windows. De opdracht uit te voeren bevat een set referenties. In dit voorbeeld wordt de opdracht wordt uitgevoerd niet versleuteld:

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
    "typeHandlerVersion": "1.9",
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

Het verplaatsen van de **opdracht wordt uitgevoerd** eigenschap in op de **beveiligd** configuratie beveiligt de tekenreeks kan worden uitgevoerd, zoals wordt weergegeven in het volgende voorbeeld:

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
    "typeHandlerVersion": "1.9",
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

### <a name="how-do-agents-and-extensions-get-updated"></a>Hoe agents en -uitbreidingen bijgewerkt?

De Agents en -extensies delen de dezelfde updatemechanisme. Sommige updates vereisen geen extra firewallregels.

Wanneer een update beschikbaar is, wordt deze alleen geïnstalleerd op de virtuele machine wanneer er een wijziging in de uitbreidingen en andere wijzigingen van de VM-Model, zoals:

- Gegevensschijven
- Extensies
- Boot diagnostics container
- Gastbesturingssysteem geheimen
- VM-grootte
- Netwerkprofiel

Uitgevers stellen updates beschikbaar te regio's op verschillende tijdstippen, zodat het mogelijk dat u kunt virtuele machines in verschillende regio's op verschillende versies hebben.

#### <a name="listing-extensions-deployed-to-a-vm"></a>Aanbieding-extensies die zijn geïmplementeerd op een virtuele machine

```powershell
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -VMName "myVM"
$vm.Extensions | select Publisher, VirtualMachineExtensionType, TypeHandlerVersion
```

```powershell
Publisher             VirtualMachineExtensionType          TypeHandlerVersion
---------             ---------------------------          ------------------
Microsoft.Compute     CustomScriptExtension                1.9
```

#### <a name="agent-updates"></a>Agent-updates

Bevat alleen de Windows-Gastagent *code afhandeling van extensie*, wordt de *Windows inrichting code* is gescheiden. U kunt de Windows Guest-Agent verwijderen. U kunt de automatische update van het venster Guest-Agent niet uitschakelen.

De *code afhandeling van extensie* verantwoordelijk is voor communicatie met de Azure-infrastructuur en verwerken van de VM-extensies bewerkingen, zoals is geïnstalleerd, rapportage over de status, de afzonderlijke uitbreidingen bijgewerkt en deze te verwijderen. Updates bevatten beveiligingsverbeteringen, oplossingen voor problemen en verbeteringen van de *code afhandeling van extensie*.

Als u wilt controleren welke versie u uitvoert, Zie [opsporen Windows Guest-Agent geïnstalleerd](agent-windows.md#detect-the-vm-agent).

#### <a name="extension-updates"></a>Extensie-updates

Wanneer een uitbreidingsupdate beschikbaar is, wordt de Windows-Gastagent downloadt en upgrades van de extensie. Automatische verlenging updates zijn *secundaire* of *Hotfix*. U kunt aanmelden of afmelden extensies *secundaire* bijgewerkt wanneer u de extensie inrichten. Het volgende voorbeeld ziet u het automatisch bijwerken van secundaire versies in Resource Manager-sjabloon met *autoUpgradeMinorVersion ': true,'*:

```json
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.9",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
```

Als u de nieuwste secundaire versie oplossingen voor problemen, is het raadzaam dat u altijd automatisch bijwerken in uw implementaties extensie selecteren. Hotfix-updates die beveiligings- of sleutel bugfixes uitvoeren kunnen niet worden heeft zich afgemeld.

### <a name="how-to-identify-extension-updates"></a>Extensie updates identificeren

#### <a name="identifying-if-the-extension-is-set-with-autoupgrademinorversion-on-a-vm"></a>Bepalen of de uitbreiding met autoUpgradeMinorVersion is ingesteld op een virtuele machine

U kunt zien uit het model van de virtuele machine als de extensie is ingericht met 'autoUpgradeMinorVersion'. Gebruiken om te controleren, [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm) en geef de resourcegroep en de VM naam als volgt:

```powerShell
 $vm = Get-AzureRmVm -ResourceGroupName "myResourceGroup" -VMName "myVM"
 $vm.Extensions
```

De volgende voorbeelduitvoer wordt weergegeven die *autoUpgradeMinorVersion* is ingesteld op *true*:

```powershell
ForceUpdateTag              :
Publisher                   : Microsoft.Compute
VirtualMachineExtensionType : CustomScriptExtension
TypeHandlerVersion          : 1.9
AutoUpgradeMinorVersion     : True
```

#### <a name="identifying-when-an-autoupgrademinorversion-occurred"></a>Om te identificeren wanneer een autoUpgradeMinorVersion opgetreden

Logboeken wilt zien wanneer een update voor de uitbreiding is opgetreden, Controleer de agent op de virtuele machine op *C:\WindowsAzure\Logs\WaAppAgent.log*

In het volgende voorbeeld wordt de virtuele machine had *Microsoft.Compute.CustomScriptExtension 1.8* geïnstalleerd. Er is een hotfix beschikbaar naar versie *1,9*:

```powershell
[INFO]  Getting plugin locations for plugin 'Microsoft.Compute.CustomScriptExtension'. Current Version: '1.8', Requested Version: '1.9'
[INFO]  Auto-Upgrade mode. Highest public version for plugin 'Microsoft.Compute.CustomScriptExtension' with requested version: '1.9', is: '1.9'
```

## <a name="agent-permissions"></a>Agent machtigingen

Voor het uitvoeren van de taken, de agent moet worden uitgevoerd als *lokaal systeem*.

## <a name="troubleshoot-vm-extensions"></a>VM-extensies oplossen

Elk VM-extensie mogelijk naar de extensie specifieke stappen voor probleemoplossing. Bijvoorbeeld, wanneer u de extensie voor aangepaste scripts, script uitvoering details zijn te vinden lokaal op de virtuele machine waar de extensie is uitgevoerd. Probleemoplossing-extensie-specifieke worden beschreven in de documentatie van de extensie-specifieke.

De volgende stappen van toepassing op alle VM-extensies.

1. Om te zien in het logboek van de Windows Guest-Agent, de activiteit kijken wanneer de uitbreiding is ingericht in *C:\WindowsAzure\Logs\WaAppAgent.txt*

2. Raadpleeg de logboeken van de werkelijke extensie voor meer informatie in *C:\WindowsAzure\Logs\Plugins\<Extensienaam >*

3. Controleer de extensie specifieke documentatie bij het oplossen van problemen secties voor foutcodes, bekende problemen, enzovoort.

4. Bekijk de systeemlogboeken. Controleer of er andere bewerkingen die mogelijk met de extensie, zoals een langdurige installatie van een andere toepassing die exclusieve package manager toegang nodig hebben verstoord.

### <a name="common-reasons-for-extension-failures"></a>Veelvoorkomende redenen voor uitbreiding fouten

1. Uitbreidingen moeten 20 minuten om uit te voeren (uitzonderingen zijn de extensies CustomScript, Chef en DSC waarvoor 90 minuten). Als uw implementatie ditmaal overschrijdt, wordt dit is gemarkeerd als een time-out. De oorzaak hiervan kan zijn veroorzaakt door onvoldoende bronnen virtuele machines, andere VM configuraties/opstarten taken hoge hoeveelheden resource gebruiken terwijl de uitbreiding wordt geprobeerd om in te richten.

2. Minimale vereisten niet voldaan aan. Bepaalde extensies zijn afhankelijk van VM-SKU's, zoals HPC-installatiekopieën. Uitbreidingen mogelijk bepaalde netwerken toegangsvereisten, zoals communicatie met Azure Storage of openbare services. Andere voorbeelden mogelijk toegang tot het pakket, onvoldoende schijfruimte of beveiligingsbeperkingen opslagplaatsen.

3. Exclusieve package manager toegang. In sommige gevallen kunnen optreden een langdurige VM-configuratie en installatie van de extensie conflicterende, waar ze beide exclusieve toegang tot de package manager nodig.

### <a name="view-extension-status"></a>Status van de extensie weergeven

Nadat u een VM-extensie is uitgevoerd voor een virtuele machine, gebruikt u [Get-AzureRmVM ](/powershell/module/azurerm.compute/get-azurermvm) te retourneren van de status van extensie. *Substatus [0]* blijkt dat de inrichting van de extensie is voltooid, wat betekent dat deze geslaagde geïmplementeerd op de virtuele machine, maar de uitvoering van de extensie in de virtuele machine is mislukt, *substatus [1]*.

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -VMName "myVM" -Status
```

De uitvoer is vergelijkbaar met de volgende voorbeelduitvoer:

```powershell
Extensions[0]           :
  Name                  : CustomScriptExtension
  Type                  : Microsoft.Compute.CustomScriptExtension
  TypeHandlerVersion    : 1.9
  Substatuses[0]        :
    Code                : ComponentStatus/StdOut/succeeded
    Level               : Info
    DisplayStatus       : Provisioning succeeded
    Message             : Windows PowerShell \nCopyright (C) Microsoft Corporation. All rights reserved.\n
  Substatuses[1]        :
    Code                : ComponentStatus/StdErr/succeeded
    Level               : Info
    DisplayStatus       : Provisioning succeeded
    Message             : The argument 'cseTest%20Scriptparam1.ps1' to the -File parameter does not exist. Provide the path to an existing '.ps1' file as an argument to the

-File parameter.
  Statuses[0]           :
    Code                : ProvisioningState/failed/-196608
    Level               : Error
    DisplayStatus       : Provisioning failed
    Message             : Finished executing command
```

Status van extensie-uitvoering kan worden gevonden in de Azure portal. Om weer te geven de status van een uitbreiding, selecteert u de virtuele machine, kiest u **extensies**, selecteert u de gewenste extensie.

### <a name="rerun-vm-extensions"></a>VM-extensies opnieuw uitvoeren

Mogelijk zijn er gevallen waarin een VM-extensie moet opnieuw worden gestart. U kunt een uitbreiding opnieuw uitvoeren te verwijderen en vervolgens opnieuw uit te voeren de uitbreiding met een methode van de uitvoering van uw keuze. U kunt een uitbreiding met [verwijderen AzureRmVMExtension](/powershell/module/AzureRM.Compute/Remove-AzureRmVMExtension) als volgt:

```powershell
Remove-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myExtensionName"
```

U kunt ook verwijderen extensie in de Azure portal als volgt:

1. Selecteer een virtuele machine.
2. Kies **extensies**.
3. Selecteer de gewenste extensie.
4. Kies **verwijderen**.

## <a name="common-vm-extensions-reference"></a>Algemene referentie voor VM-extensies
| Naam van de uitbreiding | Beschrijving | Meer informatie |
| --- | --- | --- |
| Extensie voor aangepaste scripts voor Windows |Scripts uitvoeren op een virtuele machine van Azure |[Extensie voor aangepaste scripts voor Windows](custom-script-windows.md) |
| DSC-extensie voor Windows |PowerShell DSC (Desired State Configuration)-extensie |[DSC-extensie voor Windows](dsc-overview.md) |
| Azure Diagnostics-extensie |Azure Diagnostics beheren |[Azure Diagnostics-extensie](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Uitbreiding van de toegang tot Azure VM |Gebruikers en referenties beheren |[Uitbreiding van de VM-toegang voor Linux](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de VM-extensies [virtuele machine van Azure-extensies en functies overzicht](overview.md).