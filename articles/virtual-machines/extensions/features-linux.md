---
title: Azure VM-extensies en functies voor Linux | Microsoft Docs
description: Meer informatie over welke extensies beschikbaar zijn voor virtuele machines in Azure, gegroepeerd op wat ze bieden of verbeteren.
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 52f5d0ec-8f75-49e7-9e15-88d46b420e63
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/30/2018
ms.author: danis
ms.openlocfilehash: 760f832bc12bccbf1cce77db25bf60413ad9a36b
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="virtual-machine-extensions-and-features-for-linux"></a>Uitbreidingen van de virtuele machine en functies voor Linux

Azure virtuele machine (VM) extensies zijn kleine toepassingen waarmee de configuratie en automatisering taken na de implementatie op Azure Virtual machines. Bijvoorbeeld, als een virtuele machine is vereist voor software-installatie, beveiliging tegen virussen, of een script daarbinnen uitvoeren, kan een VM-extensie kan worden gebruikt. Azure VM-extensies kunnen worden uitgevoerd met de Azure CLI, PowerShell, Azure resourcemanager-sjablonen en de Azure-portal. Uitbreidingen worden gebundeld met een nieuwe VM-implementatie of uitvoeren op een bestaande systeem.

In dit artikel biedt een overzicht van de VM-extensies voor vereisten voor het gebruik van Azure VM-extensies en instructies voor het detecteren, beheren en verwijderen van de VM-extensies. Dit artikel vindt u algemene informatie omdat veel VM-extensies beschikbaar zijn, elk met een potentieel unieke configuratie. Extensie-specifieke informatie vindt u in elk document specifiek is voor de afzonderlijke extensie.

## <a name="use-cases-and-samples"></a>Gebruiksvoorbeelden en voorbeelden

Enkele andere Azure VM-extensies beschikbaar zijn, elk met een specifieke gebruiksvoorbeeld. Voorbeelden zijn:

- Status van PowerShell gewenste configuraties toepassen op een virtuele machine met de DSC-extensie voor Linux. Zie voor meer informatie [Azure gewenst State configuration-uitbreiding](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).
- Bewaking van een virtuele machine met de Microsoft Monitoring Agent VM-extensie te configureren. Zie voor meer informatie [het bewaken van Linux-VM](../linux/tutorial-monitoring.md).
- Bewaking van uw Azure-infrastructuur met de extensie Chef of Datadog configureren. Zie voor meer informatie de [Chef docs](https://docs.chef.io/azure_portal.html) of [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).

Een aangepast Script-uitbreiding is naast processpecifieke uitbreidingen beschikbaar voor Windows- en Linux virtuele machines. De aangepaste scriptextensie voor Linux kunt u een Bash-script worden uitgevoerd op een virtuele machine. Aangepaste scripts zijn handig voor het ontwerpen van Azure-implementaties die moeten worden geconfigureerd afgezien van wat systeemeigen Azure tooling kan bieden. Zie voor meer informatie [Linux VM aangepaste scriptextensie](custom-script-linux.md).

## <a name="prerequisites"></a>Vereisten

Voor het afhandelen van de uitbreiding op de virtuele machine, moet u de Azure Linux Agent is geïnstalleerd. Een aantal afzonderlijke uitbreidingen hebben vereisten, zoals toegang tot bronnen of afhankelijkheden.

### <a name="azure-vm-agent"></a>Azure VM-agent

De Azure VM-agent beheert interactie tussen een Azure-virtuele machine en de domeincontroller van de Azure-infrastructuur. De VM-agent is verantwoordelijk voor veel functionele aspecten van het implementeren en beheren van Azure Virtual machines, waaronder het uitvoeren van de VM-extensies. De Azure VM-agent is geïnstalleerd op Azure Marketplace-installatiekopieën en kan handmatig worden geïnstalleerd op ondersteunde besturingssystemen. De Azure VM-Agent voor Linux wordt de Linux-agent genoemd.

Zie voor informatie over ondersteunde besturingssystemen en installatie-instructies, [virtuele machine van Azure-agent](agent-linux.md).

#### <a name="supported-agent-versions"></a>Ondersteunde agent-versies

Om de best mogelijke ervaring bieden, zijn er minimaal vereiste versies van de agent. Raadpleeg [dit artikel](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) voor meer informatie.

#### <a name="supported-oses"></a>Ondersteunde besturingssystemen

De Linux-agent wordt uitgevoerd op meerdere besturingssystemen, maar het framework extensies een limiet voor de besturingssystemen die uitbreidingen kent. Zie voor meer informatie [in dit artikel] (https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems ).

Bepaalde extensies worden niet ondersteund in alle besturingssystemen en verzendt *fout Code 51, 'Niet-ondersteunde OS'*. Raadpleeg de documentatie van de afzonderlijke extensie voor ondersteuning.

#### <a name="network-access"></a>Netwerktoegang

-Extensiepakketten worden gedownload vanuit de opslagplaats van de extensie Azure Storage en extensie status uploads worden gepubliceerd naar Azure Storage. Als u [ondersteund](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) versie van de agents, hoeft u niet dat toegang tot Azure Storage in het gebied van de virtuele machine als de agent kan worden gebruikt om te leiden van de communicatie naar de controller Azure-infrastructuur voor communicatie van agent. Als u van een niet-ondersteunde versie van de agent gebruikmaakt, moet u uitgaand verkeer toestaan naar Azure-opslag in deze regio van de virtuele machine.

> [!IMPORTANT]
> Als u toegang tot hebben geblokkeerd *168.63.129.1* met de Gast-firewall, klikt u vervolgens extensies mislukken ongeacht de bovenstaande.

Agents kunnen alleen worden gebruikt voor het downloaden van extensiepakketten en rapporteringsstatus. Bijvoorbeeld als een installatie van de extensie moet een script downloaden vanuit GitHub (aangepast Script) of moet toegang tot Azure Storage (Azure Backup), klikt u vervolgens extra firewall/netwerk beveiliging groep poorten moeten worden geopend. Verschillende extensies hebben verschillende vereisten, omdat ze toepassingen in hun eigen rechts. Voor uitbreidingen waarvoor toegang tot Azure Storage, kunt u toegang met behulp van Azure NSG Service-Tags voor [opslag](https://docs.microsoft.com/en-us/azure/virtual-network/security-overview#service-tags).

De Linux-Agent heeft agent verkeer aanvragen wilt omleiden, ondersteuning voor proxyserver. Ondersteuning voor deze proxyserver geldt echter niet uitbreidingen. U moet elke afzonderlijke extensie te kunnen gebruiken met een proxy configureren.

## <a name="discover-vm-extensions"></a>VM-extensies detecteren

Veel andere VM-extensies zijn beschikbaar voor gebruik met Azure Virtual machines. Als een volledige lijst wilt weergeven, gebruikt [az vm-extensie afbeeldingenlijst](/cli/azure/vm/extension/image#az-vm-extension-image-list). Het volgende voorbeeld worden alle beschikbare uitbreidingen in de *westus* locatie:

```azurecli
az vm extension image list --location westus --output table
```

## <a name="run-vm-extensions"></a>VM-extensies worden uitgevoerd

Azure VM-extensies worden uitgevoerd op de bestaande virtuele machines, die is handig als u wilt configuratiewijzigingen aanbrengen of connectiviteit op een reeds geïmplementeerde virtuele machine herstellen. VM-extensies kunnen ook worden gebundeld met implementaties van Azure Resource Manager-sjabloon. Via uitbreidingen met Resource Manager-sjablonen kunt virtuele Azure-machines worden geïmplementeerd en geconfigureerd zonder tussenkomst van de na de implementatie.

De volgende manieren kunnen worden gebruikt voor het uitvoeren van een uitbreiding op een bestaande virtuele machine.

### <a name="azure-cli-20"></a>Azure CLI 2.0

Azure VM-extensies kunnen worden uitgevoerd op basis van een bestaande virtuele machine met de [az vm-extensie set](/cli/azure/vm/extension#az-vm-extension-set) opdracht. Het volgende voorbeeld wordt de extensie voor aangepaste scripts uitgevoerd tegen een virtuele machine met de naam *myVM* in een resourcegroep met de naam *myResourceGroup*:

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/me/project/hello.sh"],"commandToExecute": "./hello.sh"}'
```

Wanneer de uitbreiding juist wordt uitgevoerd, wordt de uitvoer is vergelijkbaar met het volgende voorbeeld:

```bash
info:    Executing command vm extension set
+ Looking up the VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a>Azure Portal

VM-extensies kunnen worden toegepast op een bestaande virtuele machine via de Azure portal. Selecteer de virtuele machine in de portal, kies **extensies**, selecteer daarna **toevoegen**. Kies de extensie die u wilt dat in de lijst met beschikbare uitbreidingen en volg de instructies in de wizard.

De volgende afbeelding ziet u de installatie van de extensie Linux aangepast Script van de Azure-portal:

![Extensie voor aangepaste scripts installeren](./media/features-linux/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen

VM-extensies worden toegevoegd aan een Azure Resource Manager-sjabloon en uitgevoerd met de implementatie van de sjabloon. Wanneer u een uitbreiding met een sjabloon implementeert, kunt u volledig geconfigureerde Azure-implementaties. Bijvoorbeeld is de volgende JSON afkomstig van een Resource Manager-sjabloon die u implementeert een set van netwerktaakverdeling VM's en een Azure SQL database en vervolgens installeert een .NET Core-toepassing op elke virtuele machine. De VM-extensie zorgt voor de software-installatie.

Voor meer informatie raadpleegt u de volledige [Resource Manager-sjabloon](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
    }
}
```

Zie voor meer informatie over het maken van Resource Manager-sjablonen [Azure Resource Manager-sjablonen samenstellen](../windows/template-description.md#extensions).

## <a name="secure-vm-extension-data"></a>Beveiligde gegevens van de VM-extensie

Wanneer u een VM-extensie uitvoert, is het mogelijk nodig is met gevoelige informatie zoals referenties, namen van opslagaccounts en toegangssleutels voor opslag-account. Veel VM-extensies bevatten een beveiligde configuratie die gegevens versleutelt en ontsleutelt deze alleen binnen het doel-virtuele machine. Elke uitbreiding heeft een specifieke beveiligde configuratieschema en elk wordt beschreven in de uitbreiding specifieke documentatie bij.

Het volgende voorbeeld ziet een exemplaar van de aangepaste scriptextensie voor Linux. De opdracht uit te voeren bevat een set referenties. In dit voorbeeld wordt de opdracht wordt uitgevoerd niet versleuteld:

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ],
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
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
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
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

#### <a name="agent-updates"></a>Agent-updates

De Linux VM-Agent bevat *inrichting Agent Code* en *code afhandeling van extensie* in één pakket, dat niet kan worden gescheiden. U kunt uitschakelen de *inrichting Agent* wanneer u wilt inrichten op Azure met behulp van cloud-init. Om dit te doen, Zie [met behulp van cloud-init](../linux/using-cloud-init.md).

Ondersteunde versies van de Agents kunnen automatische updates gebruiken. De alleen-code die kan worden bijgewerkt is de *code afhandeling van extensie*, niet de inrichting code. De *inrichting Agent code* code eenmaal wordt uitgevoerd.

De *code afhandeling van extensie* verantwoordelijk is voor communicatie met de Azure-infrastructuur en verwerken van de VM-extensies bewerkingen, zoals is geïnstalleerd, rapportage over de status, de afzonderlijke uitbreidingen bijgewerkt en deze te verwijderen. Updates bevatten beveiligingsverbeteringen, oplossingen voor problemen en verbeteringen van de *code afhandeling van extensie*.

Wanneer de agent is geïnstalleerd, wordt een bovenliggende-daemon wordt gemaakt. Deze bovenliggende gestart vervolgens een onderliggend proces dat wordt gebruikt voor het afhandelen van extensies. Als een update beschikbaar is voor de agent, deze is gedownload, de bovenliggende stopt het onderliggende proces, upgrade van deze en start deze opnieuw op. Moet er een probleem met de update, het bovenliggende proces is teruggedraaid naar de vorige versie van de onderliggende.

Het bovenliggende proces kan niet automatisch bijgewerkt. Het bovenliggende object kan alleen worden bijgewerkt door een pakketupdate distro.

Als u wilt controleren welke versie u uitvoert, Controleer de `waagent` als volgt:

```bash
waagent --version
```

De uitvoer lijkt op die in het volgende voorbeeld:

```bash
WALinuxAgent-2.2.17 running on ubuntu 16.04
Python: 3.5.2
Goal state agent: 2.2.18
```

In het voorgaande voorbeeld van uitvoer is de bovenliggende of 'pakket geïmplementeerd versie' *WALinuxAgent 2.2.17*

De 'doel status agent' is de versie van de update automatisch.

Het is raadzaam dat u altijd automatisch bijwerken voor de agent hebben [AutoUpdate.Enabled=y](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent). Niet dat deze ingeschakeld betekent dat u wilt bewaren handmatig bijwerken van de agent en oplossingen voor oplossingen en -beveiliging niet ophalen.

#### <a name="extension-updates"></a>Extensie-updates

Wanneer een uitbreidingsupdate beschikbaar is, wordt de Linux-Agent downloadt en upgrades van de extensie. Automatische verlenging updates zijn *secundaire* of *Hotfix*. U kunt aanmelden of afmelden extensies *secundaire* bijgewerkt wanneer u de extensie inrichten. Het volgende voorbeeld ziet u het automatisch bijwerken van secundaire versies in Resource Manager-sjabloon met *autoUpgradeMinorVersion ': true,'*:

```json
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
        ]
    },
```

Als u de nieuwste secundaire versie oplossingen voor problemen, is het raadzaam dat u altijd automatisch bijwerken in uw implementaties extensie selecteren. Hotfix-updates die beveiligings- of sleutel bugfixes uitvoeren kunnen niet worden heeft zich afgemeld.

### <a name="how-to-identify-extension-updates"></a>Extensie updates identificeren

#### <a name="identifying-if-the-extension-is-set-with-autoupgrademinorversion-on-a-vm"></a>Bepalen of de uitbreiding met autoUpgradeMinorVersion is ingesteld op een virtuele machine

U kunt zien uit het model van de virtuele machine als de extensie is ingericht met 'autoUpgradeMinorVersion'. Gebruiken om te controleren, [az vm weergeven](/cli/azure/vm#az-vm-show) en geef de resourcegroep en de VM naam als volgt:

```azurecli
az vm show --resource-group myResourceGroup --name myVM
```

De volgende voorbeelduitvoer wordt weergegeven die *autoUpgradeMinorVersion* is ingesteld op *true*:

```json
  "resources": [
    {
      "autoUpgradeMinorVersion": true,
      "forceUpdateTag": null,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM/extensions/CustomScriptExtension",
```

#### <a name="identifying-when-an-autoupgrademinorversion-occurred"></a>Om te identificeren wanneer een autoUpgradeMinorVersion opgetreden

Logboeken wilt zien wanneer een update voor de uitbreiding is opgetreden, Controleer de agent op de virtuele machine op */var/log/waagent.log*.

In het volgende voorbeeld wordt de virtuele machine had *Microsoft.OSTCExtensions.LinuxDiagnostic 2.3.9025* geïnstalleerd. Er is een hotfix beschikbaar om te *Microsoft.OSTCExtensions.LinuxDiagnostic 2.3.9027*:

```bash
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Expected handler state: enabled
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Decide which version to use
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Use version: 2.3.9027
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Current handler state is: NotInstalled
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Download extension package
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Unpack extension package
INFO Event: name=Microsoft.OSTCExtensions.LinuxDiagnostic, op=Download, message=Download succeeded
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Initialize extension directory
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Update settings file: 0.settings
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9025] Disable extension.
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9025] Launch command:diagnostic.py -disable
...
INFO Event: name=Microsoft.OSTCExtensions.LinuxDiagnostic, op=Disable, message=Launch command succeeded: diagnostic.py -disable
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Update extension.
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Launch command:diagnostic.py -update
2017/08/14 20:21:57 LinuxAzureDiagnostic started to handle.
```

## <a name="agent-permissions"></a>Agent machtigingen

Voor het uitvoeren van de taken, de agent moet worden uitgevoerd als *hoofdmap*.

## <a name="troubleshoot-vm-extensions"></a>VM-extensies oplossen

Elk VM-extensie mogelijk naar de extensie specifieke stappen voor probleemoplossing. Bijvoorbeeld, wanneer u de extensie voor aangepaste scripts, script uitvoering details zijn te vinden lokaal op de virtuele machine waar de extensie is uitgevoerd. Probleemoplossing-extensie-specifieke worden beschreven in de documentatie van de extensie-specifieke.

De volgende stappen van toepassing op alle VM-extensies.

1. Als u wilt controleren in het logboek van de Linux-Agent, de activiteit kijken wanneer de uitbreiding is ingericht in */var/log/waagent.log*

2. Raadpleeg de logboeken van de werkelijke extensie voor meer informatie in   */var/aanmelden/azure /<extensionName>*

3. Raadpleeg de documentatie van de extensie-specifieke probleemoplossing secties voor foutcodes, bekende problemen, enzovoort.

3. Bekijk de systeemlogboeken. Controleer of er andere bewerkingen die mogelijk met de extensie, zoals een langdurige installatie van een andere toepassing die exclusieve package manager toegang nodig hebben verstoord.

### <a name="common-reasons-for-extension-failures"></a>Veelvoorkomende redenen voor uitbreiding fouten

1. Uitbreidingen moeten 20 minuten om uit te voeren (uitzonderingen zijn de extensies CustomScript, Chef en DSC waarvoor 90 minuten). Als uw implementatie ditmaal overschrijdt, wordt dit is gemarkeerd als een time-out. De oorzaak hiervan kan zijn veroorzaakt door onvoldoende bronnen virtuele machines, andere VM configuraties/opstarten taken hoge hoeveelheden resource gebruiken terwijl de uitbreiding wordt geprobeerd om in te richten.

2. Minimale vereisten niet voldaan aan. Bepaalde extensies zijn afhankelijk van VM-SKU's, zoals HPC-installatiekopieën. Uitbreidingen mogelijk bepaalde netwerken toegangsvereisten, zoals communicatie met Azure Storage of openbare services. Andere voorbeelden mogelijk toegang tot het pakket, onvoldoende schijfruimte of beveiligingsbeperkingen opslagplaatsen.

3. Exclusieve package manager toegang. In sommige gevallen kunnen optreden een langdurige VM-configuratie en installatie van de extensie conflicterende, waar ze beide exclusieve toegang tot de package manager nodig.

### <a name="view-extension-status"></a>Status van de extensie weergeven

Nadat u een VM-extensie is uitgevoerd voor een virtuele machine, gebruikt u [az vm get--instantieweergave](/cli/azure/vm#az-vm-get-instance-view) om terug te keren status van extensie als volgt:

```azurecli
az vm get-instance-view \
    --resource-group rgName \
    --name myVM \
    --query "instanceView.extensions"
```

De uitvoer is vergelijkbaar met de volgende voorbeelduitvoer:

```bash
  {
    "name": "customScript",
    "statuses": [
      {
        "code": "ProvisioningState/failed/0",
        "displayStatus": "Provisioning failed",
        "level": "Error",
        "message": "Enable failed: failed to execute command: command terminated with exit status=127\n[stdout]\n\n[stderr]\n/bin/sh: 1: ech: not found\n",
        "time": null
      }
    ],
    "substatuses": null,
    "type": "Microsoft.Azure.Extensions.customScript",
    "typeHandlerVersion": "2.0.6"
  }
```

Status van extensie-uitvoering kan worden gevonden in de Azure portal. Om weer te geven de status van een uitbreiding, selecteert u de virtuele machine, kiest u **extensies**, selecteert u de gewenste extensie.

### <a name="rerun-a-vm-extension"></a>Voer een VM-extensie

Mogelijk zijn er gevallen waarin een VM-extensie moet opnieuw worden gestart. U kunt een uitbreiding opnieuw uitvoeren te verwijderen en vervolgens opnieuw uit te voeren de uitbreiding met een methode van de uitvoering van uw keuze. U kunt een uitbreiding met [az vm-extensie verwijderen](/cli/azure/vm/extension#az-vm-extension-delete) als volgt:

```azurecli
az vm extension delete \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --name customScript
```

U kunt ook verwijderen extensie in de Azure portal als volgt:

1. Selecteer een virtuele machine.
2. Kies **extensies**.
3. Selecteer de gewenste extensie.
4. Kies **verwijderen**.

## <a name="common-vm-extension-reference"></a>Algemene referentie voor VM-extensie

| Naam van de uitbreiding | Beschrijving | Meer informatie |
| --- | --- | --- |
| Extensie voor aangepaste scripts voor Linux |Scripts uitvoeren op een virtuele machine van Azure |[Extensie voor aangepaste scripts voor Linux](custom-script-linux.md) |
| VM-extensie voor toegang |Weer toegang tot een virtuele machine van Azure |[VM-extensie voor toegang](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| Azure extensie voor diagnostische gegevens |Azure Diagnostics beheren |[Azure extensie voor diagnostische gegevens](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| De extensie Azure VM-toegang |Gebruikers en referenties beheren |[Uitbreiding van de VM-toegang voor Linux](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de VM-extensies [virtuele machine van Azure-extensies en functies overzicht](overview.md).
