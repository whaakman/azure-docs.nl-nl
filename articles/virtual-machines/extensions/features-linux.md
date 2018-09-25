---
title: Azure VM-extensies en functies voor Linux | Microsoft Docs
description: Meer informatie over welke extensies beschikbaar zijn voor virtuele machines van Azure, gegroepeerd op wat ze bieden of verbeteren.
services: virtual-machines-linux
documentationcenter: ''
author: zroiy
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
ms.author: roiyz
ms.openlocfilehash: 66c3da4f14141bdb5c357dbabfaec5dabf0ab521
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971376"
---
# <a name="virtual-machine-extensions-and-features-for-linux"></a>Extensies voor virtuele machines en functies voor Linux

Extensies voor Azure-machines (VM) zijn kleine toepassingen die taken van configuratie- en automatiseringstaken na de implementatie op Azure Virtual machines bieden. Bijvoorbeeld, als een virtuele machine is vereist voor software-installatie, beveiliging tegen virussen, of een script daarbinnen uit te voeren, kan een VM-extensie kan worden gebruikt. Azure VM-extensies kunnen worden uitgevoerd met de Azure CLI, PowerShell, Azure resourcemanager-sjablonen en Azure portal. Extensies worden gebundeld met een nieuwe VM-implementatie of op basis van een bestaand systeem worden uitgevoerd.

In dit artikel biedt een overzicht van VM-extensies, vereisten voor het gebruik van Azure VM-extensies, en instructies over het detecteren, beheren en verwijderen van de VM-extensies. Dit artikel bevat algemene informatie, omdat veel VM-extensies beschikbaar zijn, elk met een potentieel unieke configuratie. Extensie-specifieke details vindt u in elk document specifiek is voor de afzonderlijke extensie.

## <a name="use-cases-and-samples"></a>Use cases en voorbeelden

Meerdere verschillende Azure-VM-extensies beschikbaar zijn, elk met een specifieke use-case. Voorbeelden zijn:

- PowerShell Desired State configuraties toepassen op een virtuele machine met de DSC-extensie voor Linux. Zie voor meer informatie, [Azure Desired State configuration-extensie](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).
- Bewaking van een virtuele machine met de Microsoft Monitoring Agent-VM-extensie configureren. Zie voor meer informatie, [over het bewaken van een Linux-VM](../linux/tutorial-monitoring.md).
- Bewaking van uw Azure-infrastructuur met de extensie Chef of Datadog configureren. Zie voor meer informatie de [Chef docs](https://docs.chef.io/azure_portal.html) of [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).

Een aangepast Script-extensie is naast processpecifieke extensies beschikbaar voor zowel Windows als Linux virtuele machines. De aangepaste scriptextensie voor Linux kunt Bash-script om te worden uitgevoerd op een virtuele machine. Aangepaste scripts zijn handig voor het ontwerpen van Azure-implementaties die moeten worden geconfigureerd dan welke systeemeigen Azure-hulpprogramma kan bieden. Zie voor meer informatie, [Linux VM Custom Script extension](custom-script-linux.md).

## <a name="prerequisites"></a>Vereisten

Voor het afhandelen van de extensie op de virtuele machine, moet u de Azure Linux Agent is geïnstalleerd. Aantal afzonderlijke extensies hebben vereisten, zoals toegang tot bronnen of afhankelijkheden.

### <a name="azure-vm-agent"></a>Azure VM-agent

Interactie tussen een Azure-VM en de Azure-infrastructuurcontroller wordt beheerd door de Azure VM-agent. De VM-agent is verantwoordelijk voor veel functionele aspecten van het implementeren en beheren van virtuele machines van Azure, inclusief het uitvoeren van VM-extensies. De Azure VM-agent is vooraf geïnstalleerd in Azure Marketplace-installatiekopieën en handmatig kan worden geïnstalleerd op ondersteunde besturingssystemen. De Azure VM-Agent voor Linux staat bekend als de Linux-agent.

Zie voor informatie over ondersteunde besturingssystemen en installatie-instructies, [virtuele Azure-machineagent](agent-linux.md).

#### <a name="supported-agent-versions"></a>Ondersteunde agent-versies

Als u wilt de best mogelijke ervaring bieden, zijn er minimaal vereiste versies van de agent. Raadpleeg [dit artikel](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) voor meer informatie.

#### <a name="supported-oses"></a>Ondersteunde besturingssystemen

De Linux-agent wordt uitgevoerd op meerdere besturingssystemen, maar het framework extensions een limiet voor de besturingssystemen die uitbreidingen heeft. Zie voor meer informatie [in dit artikel] (https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems ).

Bepaalde extensies worden niet ondersteund in alle besturingssystemen en verzendt *fout Code 51, 'Niet-ondersteund besturingssysteem'*. Raadpleeg de documentatie van de afzonderlijke-extensie voor ondersteuning.

#### <a name="network-access"></a>Netwerktoegang

-Extensiepakketten zijn gedownload vanuit de opslagplaats van de extensie Azure Storage en extensie status uploaden naar Azure Storage worden geboekt. Als u [ondersteund](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) versie van de agents, hoeft u niet voor toegang tot Azure Storage in de regio van de virtuele machine, zoals de agent kan worden gebruikt zodat de communicatie worden omgeleid naar de Azure-infrastructuurcontroller voor communicatie van agent. Als u van een niet-ondersteunde versie van de agent gebruikmaakt, moet u de uitgaande toegang tot Azure storage in die regio van de virtuele machine toestaan.

> [!IMPORTANT]
> Als u toegang tot hebben geblokkeerd *168.63.129.1* met behulp van de Gast-firewall, extensies mislukt, ongeacht de bovenstaande.

Agents kunnen alleen worden gebruikt voor het downloaden van extensiepakketten en rapporteringsstatus. Bijvoorbeeld, als een extensie installeren moet een script downloaden vanuit GitHub (aangepast Script) of moet toegang tot Azure Storage (Azure Backup), klikt u vervolgens aanvullende firewall/Network Security Group-poorten moeten worden geopend. Verschillende extensies hebben verschillende vereisten, omdat ze toepassingen in hun eigen rechts. Voor uitbreidingen waarvoor toegang tot Azure Storage, kunt u toegang met behulp van Azure NSG servicetags voor [opslag](https://docs.microsoft.com/en-us/azure/virtual-network/security-overview#service-tags).

De Linux-Agent heeft om te leiden verkeer agentaanvragen, ondersteuning voor proxyserver. Ondersteuning voor deze proxyserver geldt echter niet extensies. U moet elke afzonderlijke extensie om te werken met een proxy configureren.

## <a name="discover-vm-extensions"></a>Detecteren van VM-extensies

Er zijn veel verschillende VM-extensies beschikbaar voor gebruik met Azure-VM's. Als een volledige lijst wilt weergeven, gebruikt [az vm extension image lijst](/cli/azure/vm/extension/image#az-vm-extension-image-list). Het volgende voorbeeld worden alle beschikbare uitbreidingen in de *westus* locatie:

```azurecli
az vm extension image list --location westus --output table
```

## <a name="run-vm-extensions"></a>VM-extensies worden uitgevoerd

Azure VM-extensies worden uitgevoerd op bestaande VM's, die is handig als u wilt configuratiewijzigingen aanbrengen of beschikbaar is op een reeds geïmplementeerde virtuele machine herstellen. VM-extensies kunnen ook worden gebundeld met sjabloonimplementaties van Azure Resource Manager. Met extensies met Resource Manager-sjablonen, kan virtuele machines van Azure worden geïmplementeerd en geconfigureerd zonder tussenkomst van de na de implementatie.

De volgende methoden kunnen worden gebruikt om uit te voeren van een uitbreiding op basis van een bestaande virtuele machine.

### <a name="azure-cli"></a>Azure-CLI

Azure VM-extensies kunnen worden uitgevoerd op basis van een bestaande virtuele machine met de [az vm extension set](/cli/azure/vm/extension#az-vm-extension-set) opdracht. Het volgende voorbeeld wordt de extensie aangepast Script uitgevoerd op basis van een virtuele machine met de naam *myVM* in een resourcegroep met de naam *myResourceGroup*:

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/me/project/hello.sh"],"commandToExecute": "./hello.sh"}'
```

Wanneer de uitbreiding correct wordt uitgevoerd, wordt de uitvoer is vergelijkbaar met het volgende voorbeeld:

```bash
info:    Executing command vm extension set
+ Looking up the VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a>Azure Portal

VM-extensies kunnen worden toegepast op een bestaande virtuele machine via de Azure-portal. Selecteer de virtuele machine in de portal, kiest u **extensies**en selecteer vervolgens **toevoegen**. Kies de extensie die u wilt in de lijst van beschikbare uitbreidingen en volg de instructies in de wizard.

De volgende afbeelding ziet u de installatie van de Linux-aangepaste scriptextensie van Azure portal:

![Aangepaste scriptextensie te installeren](./media/features-linux/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen

VM-extensies kunnen worden toegevoegd aan een Azure Resource Manager-sjabloon en uitgevoerd met de implementatie van de sjabloon. Wanneer u een uitbreiding met een sjabloon implementeert, kunt u volledig geconfigureerde Azure-implementaties maken. Bijvoorbeeld, is de volgende JSON afkomstig uit een Resource Manager-sjabloon die een set virtuele machines met load balancing en een Azure SQL-database wordt geïmplementeerd, en vervolgens een .NET Core-toepassing wordt geïnstalleerd op elke virtuele machine. De VM-extensie zorgt dat de software-installatie.

Zie voor meer informatie, de volledige [Resource Manager-sjabloon](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

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

Zie voor meer informatie over het maken van Resource Manager-sjablonen [Authoring Azure Resource Manager-sjablonen](../windows/template-description.md#extensions).

## <a name="secure-vm-extension-data"></a>VM-extensie-gegevens beveiligen

Wanneer u een VM-extensie uitvoert, is het mogelijk dat het nodig zijn voor gevoelige informatie zoals referenties, namen van opslagaccounts en opslagaccountsleutels. Veel VM-extensies bevatten een beveiligde configuratie die gegevens versleutelt en ontsleutelt deze alleen in de doel-VM. Elke uitbreiding heeft een specifieke beveiligde configuratieschema en elk wordt beschreven in de extensie-specifieke documentatie bij.

Het volgende voorbeeld ziet een exemplaar van de aangepaste scriptextensie voor Linux. De opdracht om uit te voeren bevat een set referenties. In dit voorbeeld wordt de opdracht om uit te voeren niet versleuteld:

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

Het verplaatsen van de **opdracht om uit te voeren** eigenschap in op de **beveiligd** configuratie beveiligt de tekenreeks kan worden uitgevoerd, zoals wordt weergegeven in het volgende voorbeeld:

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

#### <a name="agent-updates"></a>Agentupdates

De VM-Agent voor Linux bevat *Agentcode inrichting* en *code-extensie verwerken* in één pakket, dat niet kan worden gescheiden. U kunt uitschakelen de *Agent ingericht* wanneer u wilt inrichten op Azure met behulp van cloud-init. Om dit te doen, Zie [cloud-init gebruiken](../linux/using-cloud-init.md).

Ondersteunde versies van de Agents kunnen automatische updates gebruiken. De enige code die kan worden bijgewerkt is de *code-extensie verwerken*, niet de inrichting code. De *inrichting agentcode* code is eenmalig worden uitgevoerd.

De *code-extensie verwerken* verantwoordelijk is voor communicatie met de Azure-infrastructuur en de afhandeling van de VM-extensies-bewerkingen, zoals is geïnstalleerd, die de status, de afzonderlijke uitbreidingen bijwerken en verwijderen. Updates bevatten oplossingen, oplossingen voor problemen en verbeteringen in de *code-extensie verwerken*.

Wanneer de agent is geïnstalleerd, wordt een bovenliggende-daemon wordt gemaakt. Een onderliggend proces dat wordt gebruikt voor het afhandelen van extensies wordt vervolgens gestart door dit bovenliggende item. Als een update beschikbaar is voor de agent, het is gedownload, de bovenliggende stopt met het onderliggende proces, deze upgrades en start deze opnieuw op. Moet er een probleem met de update, teruggedraaid het bovenliggende proces naar de vorige versie van de onderliggende.

Het bovenliggende proces mag niet automatisch bijgewerkt. De bovenliggende kan alleen worden bijgewerkt door een pakketupdate distributie.

Als u wilt controleren welke versie u gebruikt, controleert u de `waagent` als volgt:

```bash
waagent --version
```

De uitvoer lijkt op die in het volgende voorbeeld:

```bash
WALinuxAgent-2.2.17 running on ubuntu 16.04
Python: 3.5.2
Goal state agent: 2.2.18
```

In het voorgaande van de voorbeelduitvoer is de bovenliggende of 'pakket geïmplementeerd versie' *WALinuxAgent 2.2.17*

De 'doel status agent' is de versie van de update automatisch.

Het is raadzaam dat u altijd automatisch bijwerken voor de agent hebt [AutoUpdate.Enabled=y](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent). Niet dat u hoeft dit ingeschakelde betekent dat die u wilt houden handmatig bijwerken van de agent, en oplossingen voor bugs en beveiliging niet ophalen.

#### <a name="extension-updates"></a>Extensie-updates

Wanneer een update van de extensie beschikbaar is, wordt de Linux-Agent downloadt en de extensie wordt bijgewerkt. Automatische verlenging updates zijn *kleine* of *Hotfix*. U kunt aanmelden of afmelden voor extensies *kleine* bijgewerkt wanneer u de extensie inrichten. Het volgende voorbeeld ziet u het automatisch upgraden van secundaire versies in Resource Manager-sjabloon met *autoUpgradeMinorVersion ': ' True ','*:

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

Als u de nieuwste secundaire versie oplossingen voor problemen, is het raadzaam dat u altijd automatisch bijwerken in uw extensie-implementaties selecteren. Hotfix-updates die beveiligings- of sleutel bugfixes uitvoeren kunnen niet worden afgemeld.

### <a name="how-to-identify-extension-updates"></a>Extensie updates identificeren

#### <a name="identifying-if-the-extension-is-set-with-autoupgrademinorversion-on-a-vm"></a>Als de extensie met autoUpgradeMinorVersion is ingesteld op een virtuele machine identificeren

U kunt zien uit het model van de virtuele machine als de extensie is ingericht met 'autoUpgradeMinorVersion'. Als u wilt controleren, gebruikt u [az vm show](/cli/azure/vm#az-vm-show) en geef de resourcegroep en VM naam als volgt:

```azurecli
az vm show --resource-group myResourceGroup --name myVM
```

De volgende voorbeelduitvoer ziet u dat *autoUpgradeMinorVersion* is ingesteld op *waar*:

```json
  "resources": [
    {
      "autoUpgradeMinorVersion": true,
      "forceUpdateTag": null,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM/extensions/CustomScriptExtension",
```

#### <a name="identifying-when-an-autoupgrademinorversion-occurred"></a>Wanneer een autoUpgradeMinorVersion opgetreden identificeren

Om te zien wanneer een update voor de extensie is opgetreden, controleert u de agent-logboeken op de virtuele machine op */var/log/waagent.log*.

In het volgende voorbeeld wordt de virtuele machine was *Microsoft.OSTCExtensions.LinuxDiagnostic 2.3.9025* geïnstalleerd. Er is een hotfix beschikbaar voor *Microsoft.OSTCExtensions.LinuxDiagnostic 2.3.9027*:

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

## <a name="agent-permissions"></a>Agent-machtigingen

Als u wilt de taken uitvoert, wordt de agent moet uit te voeren als *hoofdmap*.

## <a name="troubleshoot-vm-extensions"></a>Problemen met VM-extensies oplossen

Elke VM-extensie kan hebben op de extensie voor specifieke stappen voor probleemoplossing. Bijvoorbeeld, wanneer u de aangepaste scriptextensie gebruikt, uitvoeringsdetails voor script vindt u lokaal op de virtuele machine waar de extensie is uitgevoerd. Probleemoplossing-extensie-specifieke worden beschreven in de extensie-specifieke documentatie bij.

De volgende stappen voor probleemoplossing van toepassing op alle VM-extensies.

1. Om te controleren of het logboek van de Linux-Agent, de activiteit kijken wanneer de extensie is ingericht in */var/log/waagent.log*

2. Raadpleeg de werkelijke extensie-logboeken voor meer informatie in   */var/aanmelden/azure /<extensionName>*

3. Raadpleeg de documentatie van extensie-specifieke secties voor foutcodes, bekende problemen enzovoort oplossen van problemen.

3. Bekijk de systeemlogboeken. Controleer voor andere bewerkingen die met de extensie in, zoals een langdurige installatie van een andere toepassing die exclusieve package manager toegang vereiste mag hebben verstoord.

### <a name="common-reasons-for-extension-failures"></a>Veelvoorkomende redenen voor extensies oplossen

1. Extensies zijn 20 minuten om uit te voeren (uitzonderingen zijn de CustomScript-extensies, Chef en DSC waarvoor 90 minuten). Als uw implementatie van dit moment overschrijdt, wordt deze gemarkeerd als een time-out. De oorzaak hiervan kan worden veroorzaakt door onvoldoende bronnen virtuele machines, andere VM-configuraties/starten van de taken die grote hoeveelheden resource verbruikt, terwijl de extensie wordt geprobeerd om in te richten.

2. Minimale vereisten niet voldaan aan. Aantal extensies zijn afhankelijk van VM-SKU's, zoals HPC-afbeeldingen. Extensies mogelijk bepaalde netwerken vereisten voor gegevenstoegang, zoals communicatie met Azure Storage of openbare services. Andere voorbeelden mogelijk toegang tot de pakket-opslagplaatsen, onvoldoende schijfruimte of beveiligingsbeperkingen.

3. Exclusieve package manager-toegang. In sommige gevallen kunnen optreden een langlopende VM-configuratie en installatie van de extensie conflicterende, waar ze beide exclusieve toegang tot de package manager nodig.

### <a name="view-extension-status"></a>Status van de extensie weergeven

Nadat een VM-extensie is uitgevoerd op basis van een virtuele machine, gebruikt u [az vm get-instance-view](/cli/azure/vm#az-vm-get-instance-view) om terug te keren status van de extensie als volgt:

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

Uitvoeringsstatus van de extensie kan ook worden gevonden in Azure portal. Als u wilt weergeven van de status van een uitbreiding, selecteer de virtuele machine, kiest u **extensies**, selecteert u de gewenste extensie.

### <a name="rerun-a-vm-extension"></a>Opnieuw uitvoeren van een VM-extensie

Mogelijk zijn er gevallen waarin een VM-extensie moet opnieuw worden gestart. U kunt een uitbreiding opnieuw uitvoeren door deze te verwijderen en vervolgens opnieuw uit te voeren de extensie met een methode van de uitvoering van uw keuze. U kunt een extensie verwijderen [az vm extension delete](/cli/azure/vm/extension#az-vm-extension-delete) als volgt:

```azurecli
az vm extension delete \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --name customScript
```

U kunt ook verwijderen een extensie in de Azure-portal als volgt:

1. Selecteer een virtuele machine.
2. Kies **extensies**.
3. Selecteer de gewenste extensie.
4. Kies **verwijderen**.

## <a name="common-vm-extension-reference"></a>Algemene VM-extensie-referentie

| Extensienaam | Beschrijving | Meer informatie |
| --- | --- | --- |
| Aangepaste scriptextensie voor Linux |Scripts uitvoeren op een virtuele machine van Azure |[Aangepaste scriptextensie voor Linux](custom-script-linux.md) |
| VM-extensie voor toegang |Opnieuw toegang krijgen tot een virtuele machine van Azure |[VM-extensie voor toegang](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| Azure Diagnostics-extensie |Beheren van Azure Diagnostics |[Azure Diagnostics-extensie](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Azure-extensie voor VM-toegang |Beheren van gebruikers en referenties |[Extensie voor Linux-VM-toegang](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over VM-extensies, [overzicht van de extensies en functies van de virtuele machine van Azure](overview.md).
