---
title: Uitbreidingen van de virtuele machine en functies voor Linux | Microsoft Docs
description: Meer informatie over welke extensies beschikbaar zijn voor virtuele machines in Azure, gegroepeerd op wat ze bieden of verbeteren.
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 52f5d0ec-8f75-49e7-9e15-88d46b420e63
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: nepeters
ms.openlocfilehash: 8a5b39351f665c51ae7d83f755329e54ff3cf786
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="virtual-machine-extensions-and-features-for-linux"></a>Uitbreidingen van de virtuele machine en functies voor Linux

Virtuele machine van Azure-extensies zijn kleine toepassingen waarmee de configuratie en automatisering taken na de implementatie op virtuele machines in Azure. Als een virtuele machine vereist een software-installatie, beveiliging tegen virussen of Docker-configuratie, kan er bijvoorbeeld een VM-extensie worden gebruikt om deze taken te voltooien. Azure VM-extensies kunnen worden uitgevoerd met behulp van de Azure CLI, PowerShell, Azure resourcemanager-sjablonen en de Azure-portal. Extensies worden gebundeld met een nieuwe implementatie van de virtuele machine of eventuele bestaande systeem uitgevoerd.

Dit document bevat een overzicht van de VM-extensies voor vereisten voor het gebruik van Azure VM-extensies en instructies voor het detecteren, beheren en verwijderen van de VM-extensies. Dit document vindt u algemene informatie omdat veel VM-extensies beschikbaar zijn, elk met een potentieel unieke configuratie. Extensie-specifieke informatie vindt u in elk document specifiek is voor de afzonderlijke extensie.

## <a name="use-cases-and-samples"></a>Gebruiksvoorbeelden en voorbeelden

Enkele andere Azure VM-extensies beschikbaar zijn, elk met een specifieke gebruiksvoorbeeld. Een aantal voorbeelden:

- Status van PowerShell gewenste configuraties toepassen op een virtuele machine met behulp van de DSC-uitbreiding voor Linux. Zie voor meer informatie [Azure gewenst State configuration-uitbreiding](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).
- Bewaking van een virtuele machine met de Microsoft Monitoring Agent VM-extensie te configureren. Zie voor meer informatie [het bewaken van Linux-VM](tutorial-monitoring.md).
- Bewaking van uw Azure-infrastructuur met de extensie Datadog configureren. Zie voor meer informatie de [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).
- Configureer een Docker-host op een virtuele machine van Azure met behulp van de Docker-VM-extensie. Zie voor meer informatie [Docker VM-extensie](dockerextension.md).

Een aangepast Script-uitbreiding is naast processpecifieke uitbreidingen beschikbaar voor Windows- en Linux virtuele machines. De aangepaste scriptextensie voor Linux kunt u een Bash-script worden uitgevoerd op een virtuele machine. Aangepaste scripts zijn handig voor het ontwerpen van Azure-implementaties die moeten worden geconfigureerd afgezien van wat systeemeigen Azure tooling kan bieden. Zie voor meer informatie [Linux VM aangepaste scriptextensie](extensions-customscript.md).


## <a name="prerequisites"></a>Vereisten

De extensie van elke virtuele machine mogelijk een eigen set vereisten. De Docker-VM-extensie heeft bijvoorbeeld een vereiste van een ondersteunde Linux-distributie. Vereisten van afzonderlijke uitbreidingen worden beschreven in de documentatie van extensie-specifieke.

### <a name="azure-vm-agent"></a>Azure VM-agent

De Azure VM-agent beheert interactie tussen Azure een virtuele machine en de domeincontroller van de Azure-infrastructuur. De VM-agent is verantwoordelijk voor veel functionele aspecten van het implementeren en beheren van virtuele machines in Azure, waaronder het uitvoeren van de VM-extensies. De Azure VM-agent is geïnstalleerd op Azure Marketplace-installatiekopieën en kan handmatig worden geïnstalleerd op ondersteunde besturingssystemen.

Zie voor informatie over ondersteunde besturingssystemen en installatie-instructies, [virtuele machine van Azure-agent](../windows/classic/agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>VM-extensies detecteren

Veel andere VM-extensies zijn beschikbaar voor gebruik met virtuele machines in Azure. Voer de volgende opdracht met de Azure CLI, de Voorbeeldlocatie vervangen door de locatie van uw keuze voor een volledig overzicht.

```azurecli
az vm extension image list --location westus -o table
```

## <a name="run-vm-extensions"></a>VM-extensies worden uitgevoerd

Uitbreidingen van de virtuele machine van Azure kunnen op bestaande virtuele machines, die vooral nuttig zijn wanneer u configuratiewijzigingen aanbrengen of herstel de verbinding op een reeds geïmplementeerde virtuele machine moet worden uitgevoerd. VM-extensies kunnen ook worden gebundeld met implementaties van Azure Resource Manager-sjabloon. Via uitbreidingen met Resource Manager-sjablonen kunnen virtuele machines in Azure worden geïmplementeerd en geconfigureerd zonder tussenkomst van de na de implementatie.

De volgende manieren kunnen worden gebruikt voor het uitvoeren van een uitbreiding op een bestaande virtuele machine.

### <a name="azure-cli"></a>Azure CLI

Uitbreidingen van de virtuele machine van Azure kunnen worden uitgevoerd op basis van een bestaande virtuele machine met behulp van de `az vm extension set` opdracht. In dit voorbeeld wordt de extensie voor aangepaste scripts uitgevoerd op basis van een virtuele machine.

```azurecli
az vm extension set `
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

Het script wordt de uitvoer is vergelijkbaar met de volgende tekst:

```azurecli
info:    Executing command vm extension set
+ Looking up the VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a>Azure Portal

VM-extensies kunnen worden toegepast op een bestaande virtuele machine via de Azure portal. Selecteer de virtuele machine om dit te doen, kiest u **extensies**, en klik op **toevoegen**. Selecteer de uitbreiding die u wilt dat in de lijst met beschikbare uitbreidingen en volg de instructies in de wizard.

De volgende afbeelding ziet u de installatie van de extensie Linux aangepast Script van de Azure-portal.

![Extensie voor aangepaste scripts installeren](./media/extensions-features/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen

VM-extensies worden toegevoegd aan een Azure Resource Manager-sjabloon en uitgevoerd met de implementatie van de sjabloon. Wanneer u een uitbreiding met een sjabloon implementeert, kunt u volledig geconfigureerde Azure-implementaties. Bijvoorbeeld is de volgende JSON afkomstig van een Resource Manager-sjabloon. De sjabloon implementeert een set van netwerktaakverdeling virtuele machines en een Azure SQL database, en installeert een toepassing .NET Core op elke virtuele machine. De VM-extensie zorgt voor de software-installatie.

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

Zie voor meer informatie [Azure Resource Manager-sjablonen samenstellen](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).

## <a name="secure-vm-extension-data"></a>Beveiligde gegevens van de VM-extensie

Wanneer u een VM-extensie uitvoert, is het mogelijk met gevoelige informatie zoals referenties, namen van opslagaccounts en toegangssleutels voor opslag-account nodig. Veel VM-extensies bevatten een beveiligde configuratie die gegevens versleutelt en ontsleutelt deze alleen in de doel-virtuele machine. Elke uitbreiding heeft een specifieke beveiligde configuratieschema en elk wordt beschreven in de uitbreiding specifieke documentatie bij.

Het volgende voorbeeld ziet een exemplaar van de aangepaste scriptextensie voor Linux. U ziet dat de opdracht voor het uitvoeren van een set referenties bevat. In dit voorbeeld de opdracht wordt uitgevoerd niet versleuteld.


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

Het verplaatsen van de **opdracht wordt uitgevoerd** eigenschap in op de **beveiligd** configuratie beveiligt de tekenreeks kan worden uitgevoerd.

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

## <a name="troubleshoot-vm-extensions"></a>VM-extensies oplossen

Elk VM-extensie mogelijk naar de extensie specifieke stappen voor probleemoplossing. Bijvoorbeeld, wanneer u de extensie voor aangepaste scripts, script uitvoering details zijn te vinden lokaal op de virtuele machine waarop de uitbreiding is uitgevoerd. Probleemoplossing-extensie-specifieke worden beschreven in de documentatie van de extensie-specifieke.

De volgende stappen van toepassing op alle uitbreidingen van de virtuele machine.

### <a name="view-extension-status"></a>Status van de extensie weergeven

Extensie nadat een virtuele machine is uitgevoerd voor een virtuele machine, gebruikt u de volgende opdracht in de Azure CLI om te retourneren van de status van extensie. Parameternamen voorbeeld vervangen door uw eigen waarden.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

De uitvoer ziet er de volgende tekst:

```azurecli
AutoUpgradeMinorVersion    Location    Name          ProvisioningState    Publisher                   ResourceGroup      TypeHandlerVersion  VirtualMachineExtensionType
-------------------------  ----------  ------------  -------------------  --------------------------  ---------------  --------------------  -----------------------------
True                       westus      customScript  Succeeded            Microsoft.Azure.Extensions  exttest                             2  customScript
```

Status van extensie-uitvoering kan worden gevonden in de Azure portal. Selecteer de virtuele machine om de status van een uitbreiding, weergeven, kiest u **extensies**, en selecteer de gewenste extensie.

### <a name="rerun-a-vm-extension"></a>Voer een VM-extensie

Mogelijk zijn er gevallen waarin de extensie van een virtuele machine moet opnieuw worden gestart. U kunt een uitbreiding opnieuw uitvoeren te verwijderen en vervolgens opnieuw uit te voeren de uitbreiding met een methode van de uitvoering van uw keuze. Voer de volgende opdracht met de Azure CLI voor het verwijderen van extensie. Parameternamen voorbeeld vervangen door uw eigen waarden.

```azurecli
az vm extension delete --name customScript --resource-group myResourceGroup --vm-name myVM
```

U kunt een uitbreiding verwijderen met behulp van de volgende stappen uit in de Azure-portal:

1. Selecteer een virtuele machine.
2. Kies **extensies**.
3. Selecteer de gewenste extensie.
4. Kies **verwijderen**.

## <a name="common-vm-extension-reference"></a>Algemene referentie voor VM-extensie
| Naam van de uitbreiding | Beschrijving | Meer informatie |
| --- | --- | --- |
| Extensie voor aangepaste scripts voor Linux |Scripts uitvoeren op een virtuele machine van Azure |[Extensie voor aangepaste scripts voor Linux](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Docker-uitbreiding |Installeer de Docker-daemon ter ondersteuning van externe Docker-opdrachten. |[Docker-VM-extensie](dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| VM-extensie voor toegang |Weer toegang tot een virtuele machine van Azure |[VM-extensie voor toegang](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| Azure extensie voor diagnostische gegevens |Azure Diagnostics beheren |[Azure extensie voor diagnostische gegevens](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| De extensie Azure VM-toegang |Gebruikers en referenties beheren |[Uitbreiding van de VM-toegang voor Linux](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
