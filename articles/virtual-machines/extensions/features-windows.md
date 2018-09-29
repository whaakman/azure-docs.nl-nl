---
title: Azure VM-extensies en functies voor Windows | Microsoft Docs
description: Meer informatie over welke extensies beschikbaar zijn voor virtuele machines van Azure, gegroepeerd op wat ze bieden of verbeteren.
services: virtual-machines-windows
documentationcenter: ''
author: roiyz-msft
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
ms.author: roiyz
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d7b183a5b87d5777609deac02e4424d9451e9643
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452493"
---
# <a name="virtual-machine-extensions-and-features-for-windows"></a>Extensies voor virtuele machines en functies voor Windows

Extensies voor Azure-machines (VM) zijn kleine toepassingen die taken van configuratie- en automatiseringstaken na de implementatie op Azure Virtual machines bieden. Bijvoorbeeld, als een virtuele machine is vereist voor software-installatie, beveiliging tegen virussen, of een script daarbinnen uit te voeren, kan een VM-extensie kan worden gebruikt. Azure VM-extensies kunnen worden uitgevoerd met de Azure CLI, PowerShell, Azure resourcemanager-sjablonen en Azure portal. Extensies worden gebundeld met een nieuwe VM-implementatie of op basis van een bestaand systeem worden uitgevoerd.

In dit artikel biedt een overzicht van VM-extensies, vereisten voor het gebruik van Azure VM-extensies, en instructies over het detecteren, beheren en verwijderen van de VM-extensies. Dit artikel bevat algemene informatie, omdat veel VM-extensies beschikbaar zijn, elk met een potentieel unieke configuratie. Extensie-specifieke details vindt u in elk document specifiek is voor de afzonderlijke extensie.

## <a name="use-cases-and-samples"></a>Use cases en voorbeelden

Meerdere verschillende Azure-VM-extensies beschikbaar zijn, elk met een specifieke use-case. Voorbeelden zijn:

- PowerShell Desired State configuraties toepassen op een virtuele machine met de DSC-extensie voor Windows. Zie voor meer informatie, [Azure Desired State configuration-extensie](dsc-overview.md).
- Bewaking van een virtuele machine met de Microsoft Monitoring Agent-VM-extensie configureren. Zie voor meer informatie, [Azure VM's verbinding maken met Log Analytics](../../log-analytics/log-analytics-azure-vm-extension.md).
- Een Azure-VM configureren met behulp van Chef. Zie voor meer informatie, [implementatie van de Azure-VM automatiseren met Chef](../windows/chef-automation.md).
- Bewaking van uw Azure-infrastructuur met de extensie Datadog configureren. Zie voor meer informatie de [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).


Een aangepast Script-extensie is naast processpecifieke extensies beschikbaar voor zowel Windows als Linux virtuele machines. De aangepaste scriptextensie voor Windows kunt een PowerShell-script om te worden uitgevoerd op een virtuele machine. Aangepaste scripts zijn handig voor het ontwerpen van Azure-implementaties die moeten worden geconfigureerd dan welke systeemeigen Azure-hulpprogramma kan bieden. Zie voor meer informatie, [Windows VM Custom Script extension](custom-script-windows.md).

## <a name="prerequisites"></a>Vereisten

Voor het afhandelen van de extensie op de virtuele machine, moet u de Azure Windows-Agent is geïnstalleerd. Aantal afzonderlijke extensies hebben vereisten, zoals toegang tot bronnen of afhankelijkheden.

### <a name="azure-vm-agent"></a>Azure VM-agent

Interactie tussen een Azure-VM en de Azure-infrastructuurcontroller wordt beheerd door de Azure VM-agent. De VM-agent is verantwoordelijk voor veel functionele aspecten van het implementeren en beheren van virtuele machines van Azure, inclusief het uitvoeren van VM-extensies. De Azure VM-agent is vooraf geïnstalleerd in Azure Marketplace-installatiekopieën en handmatig kan worden geïnstalleerd op ondersteunde besturingssystemen. De Azure VM-Agent voor Windows staat bekend als de Windows Guest-agent.

Zie voor informatie over ondersteunde besturingssystemen en installatie-instructies, [virtuele Azure-machineagent](agent-windows.md).

#### <a name="supported-agent-versions"></a>Ondersteunde agent-versies

Als u wilt de best mogelijke ervaring bieden, zijn er minimaal vereiste versies van de agent. Raadpleeg [dit artikel](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) voor meer informatie.

#### <a name="supported-oses"></a>Ondersteunde besturingssystemen

De Windows Guest-agent wordt uitgevoerd op meerdere besturingssystemen, maar het framework extensions een limiet voor de besturingssystemen die uitbreidingen heeft. Zie voor meer informatie [in dit artikel] (https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems ).

Bepaalde extensies worden niet ondersteund in alle besturingssystemen en verzendt *fout Code 51, 'Niet-ondersteund besturingssysteem'*. Raadpleeg de documentatie van de afzonderlijke-extensie voor ondersteuning.

#### <a name="network-access"></a>Netwerktoegang

-Extensiepakketten zijn gedownload vanuit de opslagplaats van de extensie Azure Storage en extensie status uploaden naar Azure Storage worden geboekt. Als u [ondersteund](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) versie van de agents, hoeft u niet voor toegang tot Azure Storage in de regio van de virtuele machine, zoals de agent kan worden gebruikt zodat de communicatie worden omgeleid naar de Azure-infrastructuurcontroller voor communicatie van agent. Als u van een niet-ondersteunde versie van de agent gebruikmaakt, moet u de uitgaande toegang tot Azure storage in die regio van de virtuele machine toestaan.

> [!IMPORTANT]
> Als u toegang tot hebben geblokkeerd *168.63.129.16* met behulp van de Gast-firewall, extensies mislukt, ongeacht de bovenstaande.

Agents kunnen alleen worden gebruikt voor het downloaden van extensiepakketten en rapporteringsstatus. Bijvoorbeeld, als een extensie installeren moet een script downloaden vanuit GitHub (aangepast Script) of moet toegang tot Azure Storage (Azure Backup), klikt u vervolgens aanvullende firewall/Network Security Group-poorten moeten worden geopend. Verschillende extensies hebben verschillende vereisten, omdat ze toepassingen in hun eigen rechts. Voor uitbreidingen waarvoor toegang tot Azure Storage, kunt u toegang met behulp van Azure NSG servicetags voor [opslag](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

De Windows Guest-Agent heeft geen ondersteuning voor u omleiden van verkeer agentaanvragen via proxy-server.

## <a name="discover-vm-extensions"></a>Detecteren van VM-extensies

Er zijn veel verschillende VM-extensies beschikbaar voor gebruik met Azure-VM's. Als een volledige lijst wilt weergeven, gebruikt [Get-AzureRmVMExtensionImage](/powershell/module/azurerm.compute/get-azurermvmextensionimage). Het volgende voorbeeld worden alle beschikbare uitbreidingen in de *WestUS* locatie:

```powershell
Get-AzureRmVmImagePublisher -Location "WestUS" | `
Get-AzureRmVMExtensionImageType | `
Get-AzureRmVMExtensionImage | Select Type, Version
```

## <a name="run-vm-extensions"></a>VM-extensies worden uitgevoerd

Azure VM-extensies worden uitgevoerd op bestaande VM's, die is handig als u wilt configuratiewijzigingen aanbrengen of beschikbaar is op een reeds geïmplementeerde virtuele machine herstellen. VM-extensies kunnen ook worden gebundeld met sjabloonimplementaties van Azure Resource Manager. Met extensies met Resource Manager-sjablonen, kan virtuele machines van Azure worden geïmplementeerd en geconfigureerd zonder tussenkomst van de na de implementatie.

De volgende methoden kunnen worden gebruikt om uit te voeren van een uitbreiding op basis van een bestaande virtuele machine.

### <a name="powershell"></a>PowerShell

Er bestaan verschillende PowerShell-opdrachten voor het uitvoeren van afzonderlijke uitbreidingen. Een lijst wilt bekijken, gebruikt u [Get-Command](/powershell/module/microsoft.powershell.core/get-command) en filtert u op *extensie*:

```powershell
Get-Command Set-AzureRM*Extension* -Module AzureRM.Compute
```

Deze biedt uitvoer is vergelijkbaar met het volgende:

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

Het volgende voorbeeld wordt de aangepaste scriptextensie voor het downloaden van een script vanuit een GitHub-opslagplaats naar de doel-VM en voer het script. Zie voor meer informatie over de aangepaste scriptextensie [Custom Script extensieoverzicht](custom-script-windows.md).

```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" -Name "myCustomScript" `
    -FileUri "https://raw.githubusercontent.com/neilpeterson/nepeters-azure-templates/master/windows-custom-script-simple/support-scripts/Create-File.ps1" `
    -Run "Create-File.ps1" -Location "West US"
```

In het volgende voorbeeld wordt de extensie voor VM-toegang gebruikt om het beheerderswachtwoord van een Windows-VM naar een tijdelijk wachtwoord opnieuw in te. Zie voor meer informatie over de extensie voor VM-toegang, [opnieuw instellen van extern bureaublad-service in een Windows-VM](../windows/reset-rdp.md). Nadat u dit hebt uitgevoerd, moet u bij de eerste aanmelding het wachtwoord opnieuw instellen:

```powershell
$cred=Get-Credential

Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myVMAccess" `
    -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

De `Set-AzureRmVMExtension` opdracht kan worden gebruikt om te beginnen een VM-extensie. Zie voor meer informatie de [Set-AzureRmVMExtension verwijzing](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension).


### <a name="azure-portal"></a>Azure Portal

VM-extensies kunnen worden toegepast op een bestaande virtuele machine via de Azure-portal. Selecteer de virtuele machine in de portal, kiest u **extensies**en selecteer vervolgens **toevoegen**. Kies de extensie die u wilt in de lijst van beschikbare uitbreidingen en volg de instructies in de wizard.

Het volgende voorbeeld ziet u de installatie van de Microsoft Antimalware-extensie van de Azure-portal:

![Anti-malware-extensie installeren](./media/features-windows/installantimalwareextension.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen

VM-extensies kunnen worden toegevoegd aan een Azure Resource Manager-sjabloon en uitgevoerd met de implementatie van de sjabloon. Wanneer u een uitbreiding met een sjabloon implementeert, kunt u volledig geconfigureerde Azure-implementaties maken. Bijvoorbeeld, de volgende JSON wordt gemaakt van een Resource Manager sjabloon implementeert u een set virtuele machines met load balancing en een Azure SQL database en vervolgens een .NET Core-toepassing wordt geïnstalleerd op elke virtuele machine. De VM-extensie zorgt dat de software-installatie.

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

Zie voor meer informatie over het maken van Resource Manager-sjablonen [Azure Resource Manager-sjablonen samenstellen met Windows-VM-extensies](../windows/template-description.md#extensions).

## <a name="secure-vm-extension-data"></a>VM-extensie-gegevens beveiligen

Wanneer u een VM-extensie uitvoert, is het mogelijk dat het nodig zijn voor gevoelige informatie zoals referenties, namen van opslagaccounts en opslagaccountsleutels. Veel VM-extensies bevatten een beveiligde configuratie die gegevens versleutelt en ontsleutelt deze alleen in de doel-VM. Elke uitbreiding heeft een specifieke beveiligde configuratieschema en elk wordt beschreven in de extensie-specifieke documentatie bij.

Het volgende voorbeeld ziet een exemplaar van de aangepaste scriptextensie voor Windows. De opdracht om uit te voeren bevat een set referenties. In dit voorbeeld wordt de opdracht om uit te voeren niet versleuteld:

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

Het verplaatsen van de **opdracht om uit te voeren** eigenschap in op de **beveiligd** configuratie beveiligt de tekenreeks kan worden uitgevoerd, zoals wordt weergegeven in het volgende voorbeeld:

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

### <a name="how-do-agents-and-extensions-get-updated"></a>Hoe agents en -extensies bijgewerkt?

De Agents en -extensies delen de dezelfde updatemechanisme. Sommige updates vereisen geen aanvullende firewallregels.

Wanneer een update beschikbaar is, is deze alleen geïnstalleerd op de virtuele machine wanneer er een wijziging naar uitbreidingen en andere wijzigingen in het gegevensmodel van de virtuele machine, zoals:

- Gegevensschijven
- Extensies
- Boot diagnostics container
- Gast-OS-geheimen
- VM-grootte
- Netwerkprofiel

Uitgevers stellen updates beschikbaar aan regio's op verschillende tijdstippen, zodat het mogelijk dat u kunt virtuele machines in verschillende regio's op verschillende versies hebben.

#### <a name="listing-extensions-deployed-to-a-vm"></a>Aanbieding-uitbreidingen die zijn geïmplementeerd op een virtuele machine

```powershell
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -VMName "myVM"
$vm.Extensions | select Publisher, VirtualMachineExtensionType, TypeHandlerVersion
```

```powershell
Publisher             VirtualMachineExtensionType          TypeHandlerVersion
---------             ---------------------------          ------------------
Microsoft.Compute     CustomScriptExtension                1.9
```

#### <a name="agent-updates"></a>Agentupdates

De Windows Guest-Agent bevat alleen *code-extensie verwerken*, wordt de *inrichten van Windows-code* is gescheiden. U kunt de Windows Guest-Agent verwijderen. U kunt de automatische update van de Gastagent venster niet uitschakelen.

De *code-extensie verwerken* verantwoordelijk is voor communicatie met de Azure-infrastructuur en de afhandeling van de VM-extensies-bewerkingen, zoals is geïnstalleerd, die de status, de afzonderlijke uitbreidingen bijwerken en verwijderen. Updates bevatten oplossingen, oplossingen voor problemen en verbeteringen in de *code-extensie verwerken*.

Als u wilt controleren welke versie u gebruikt, Zie [opsporen Windows-Gastagent heeft geïnstalleerd](agent-windows.md#detect-the-vm-agent).

#### <a name="extension-updates"></a>Extensie-updates

Wanneer een extensie-update beschikbaar is, wordt de Windows Guest Agent downloadt en de extensie wordt bijgewerkt. Automatische verlenging updates zijn *kleine* of *Hotfix*. U kunt aanmelden of afmelden voor extensies *kleine* bijgewerkt wanneer u de extensie inrichten. Het volgende voorbeeld ziet u het automatisch upgraden van secundaire versies in Resource Manager-sjabloon met *autoUpgradeMinorVersion ': ' True ','*:

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

Als u de nieuwste secundaire versie oplossingen voor problemen, is het raadzaam dat u altijd automatisch bijwerken in uw extensie-implementaties selecteren. Hotfix-updates die beveiligings- of sleutel bugfixes uitvoeren kunnen niet worden afgemeld.

### <a name="how-to-identify-extension-updates"></a>Extensie updates identificeren

#### <a name="identifying-if-the-extension-is-set-with-autoupgrademinorversion-on-a-vm"></a>Als de extensie met autoUpgradeMinorVersion is ingesteld op een virtuele machine identificeren

U kunt zien uit het model van de virtuele machine als de extensie is ingericht met 'autoUpgradeMinorVersion'. Als u wilt controleren, gebruikt u [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm) en geef de resourcegroep en VM naam als volgt:

```powerShell
 $vm = Get-AzureRmVm -ResourceGroupName "myResourceGroup" -VMName "myVM"
 $vm.Extensions
```

De volgende voorbeelduitvoer ziet u dat *autoUpgradeMinorVersion* is ingesteld op *waar*:

```powershell
ForceUpdateTag              :
Publisher                   : Microsoft.Compute
VirtualMachineExtensionType : CustomScriptExtension
TypeHandlerVersion          : 1.9
AutoUpgradeMinorVersion     : True
```

#### <a name="identifying-when-an-autoupgrademinorversion-occurred"></a>Wanneer een autoUpgradeMinorVersion opgetreden identificeren

Om te zien wanneer een update voor de extensie is opgetreden, controleert u de agent-logboeken op de virtuele machine op *C:\WindowsAzure\Logs\WaAppAgent.log*

In het volgende voorbeeld wordt de virtuele machine was *Microsoft.Compute.CustomScriptExtension 1.8* geïnstalleerd. Er is een hotfix beschikbaar naar versie *1.9*:

```powershell
[INFO]  Getting plugin locations for plugin 'Microsoft.Compute.CustomScriptExtension'. Current Version: '1.8', Requested Version: '1.9'
[INFO]  Auto-Upgrade mode. Highest public version for plugin 'Microsoft.Compute.CustomScriptExtension' with requested version: '1.9', is: '1.9'
```

## <a name="agent-permissions"></a>Agent-machtigingen

Als u wilt de taken uitvoert, wordt de agent moet uit te voeren als *lokaal systeem*.

## <a name="troubleshoot-vm-extensions"></a>Problemen met VM-extensies oplossen

Elke VM-extensie kan hebben op de extensie voor specifieke stappen voor probleemoplossing. Bijvoorbeeld, wanneer u de aangepaste scriptextensie gebruikt, uitvoeringsdetails voor script vindt u lokaal op de virtuele machine waar de extensie is uitgevoerd. Probleemoplossing-extensie-specifieke worden beschreven in de extensie-specifieke documentatie bij.

De volgende stappen voor probleemoplossing van toepassing op alle VM-extensies.

1. Om te controleren of het logboek van de Windows Guest Agent, de activiteit kijken wanneer de extensie is ingericht in *C:\WindowsAzure\Logs\WaAppAgent.txt*

2. Raadpleeg de werkelijke extensie-logboeken voor meer informatie in *C:\WindowsAzure\Logs\Plugins\<Extensienaam >*

3. Controleer de extensie specifieke documentatie over het oplossen van problemen secties voor foutcodes, bekende problemen enzovoort.

4. Bekijk de systeemlogboeken. Controleer voor andere bewerkingen die met de extensie in, zoals een langdurige installatie van een andere toepassing die exclusieve package manager toegang vereiste mag hebben verstoord.

### <a name="common-reasons-for-extension-failures"></a>Veelvoorkomende redenen voor extensies oplossen

1. Extensies zijn 20 minuten om uit te voeren (uitzonderingen zijn de CustomScript-extensies, Chef en DSC waarvoor 90 minuten). Als uw implementatie van dit moment overschrijdt, wordt deze gemarkeerd als een time-out. De oorzaak hiervan kan worden veroorzaakt door onvoldoende bronnen virtuele machines, andere VM-configuraties/starten van de taken die grote hoeveelheden resource verbruikt, terwijl de extensie wordt geprobeerd om in te richten.

2. Minimale vereisten niet voldaan aan. Aantal extensies zijn afhankelijk van VM-SKU's, zoals HPC-afbeeldingen. Extensies mogelijk bepaalde netwerken vereisten voor gegevenstoegang, zoals communicatie met Azure Storage of openbare services. Andere voorbeelden mogelijk toegang tot de pakket-opslagplaatsen, onvoldoende schijfruimte of beveiligingsbeperkingen.

3. Exclusieve package manager-toegang. In sommige gevallen kunnen optreden een langlopende VM-configuratie en installatie van de extensie conflicterende, waar ze beide exclusieve toegang tot de package manager nodig.

### <a name="view-extension-status"></a>Status van de extensie weergeven

Nadat een VM-extensie is uitgevoerd op basis van een virtuele machine, gebruikt u [Get-AzureRmVM ](/powershell/module/azurerm.compute/get-azurermvm) Extensiestatus moet worden geretourneerd. *Substatus [0]* laat zien dat de extensie wordt ingericht is voltooid, wat betekent dat deze succesvolle geïmplementeerd op de VM, maar de uitvoering van de extensie in de virtuele machine is mislukt, *substatus [1]*.

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

Uitvoeringsstatus van de extensie kan ook worden gevonden in Azure portal. Als u wilt weergeven van de status van een uitbreiding, selecteer de virtuele machine, kiest u **extensies**, selecteert u de gewenste extensie.

### <a name="rerun-vm-extensions"></a>Opnieuw uitvoeren van VM-extensies

Mogelijk zijn er gevallen waarin een VM-extensie moet opnieuw worden gestart. U kunt een uitbreiding opnieuw uitvoeren door deze te verwijderen en vervolgens opnieuw uit te voeren de extensie met een methode van de uitvoering van uw keuze. U kunt een extensie verwijderen [Remove-AzureRmVMExtension](/powershell/module/AzureRM.Compute/Remove-AzureRmVMExtension) als volgt:

```powershell
Remove-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myExtensionName"
```

U kunt ook verwijderen een extensie in de Azure-portal als volgt:

1. Selecteer een virtuele machine.
2. Kies **extensies**.
3. Selecteer de gewenste extensie.
4. Kies **verwijderen**.

## <a name="common-vm-extensions-reference"></a>Algemene referentie van de VM-extensies
| Extensienaam | Beschrijving | Meer informatie |
| --- | --- | --- |
| Aangepaste Scriptextensie voor Windows |Scripts uitvoeren op een virtuele machine van Azure |[Aangepaste Scriptextensie voor Windows](custom-script-windows.md) |
| DSC-extensie voor Windows |PowerShell DSC (Desired State Configuration)-extensie |[DSC-extensie voor Windows](dsc-overview.md) |
| Azure Diagnostics-extensie |Beheren van Azure Diagnostics |[Azure Diagnostics-extensie](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Azure VM-extensie voor toegang |Beheren van gebruikers en referenties |[VM-extensie voor toegang voor Linux](https://azure.microsoft.com/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over VM-extensies, [overzicht van de extensies en functies van de virtuele machine van Azure](overview.md).
