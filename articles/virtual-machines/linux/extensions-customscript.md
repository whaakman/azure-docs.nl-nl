---
title: Aangepaste scripts uitvoeren op virtuele Linux-machines in Azure | Microsoft Docs
description: Configuratietaken voor Linux-VM automatiseren met behulp van de aangepaste Scriptextensie
services: virtual-machines-linux
documentationcenter: 
author: danielsollondon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cf17ab2b-8d7e-4078-b6df-955c6d5071c2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: danis
ms.openlocfilehash: 53adef0f512c54e036a981dbaa0d08453db6b194
ms.sourcegitcommit: a0d2423f1f277516ab2a15fe26afbc3db2f66e33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="use-the-azure-custom-script-extension-with-linux-virtual-machines"></a>De Azure-extensie voor aangepaste scripts gebruiken met Linux virtuele machines
De aangepaste Scriptextensie downloads en scripts uitvoeren op virtuele machines in Azure. Deze uitbreiding is nuttig voor na de implementatie-configuratie, software-installatie of een andere Configuratiebeheer /-taak. U kunt scripts downloaden van Azure Storage of een andere internetlocatie voor toegankelijk of u kunt ze met de extensie runtime opgeven. 

De aangepaste Scriptextensie integreert met Azure Resource Manager-sjablonen. U kunt deze ook uitvoeren met behulp van Azure CLI, PowerShell, de Azure-portal of de REST-API van Azure virtuele Machines.

Dit artikel wordt uitgelegd hoe u de aangepaste Scriptextensie van Azure CLI, en het uitvoeren van de uitbreiding met behulp van een Azure Resource Manager-sjabloon. In dit artikel bevat ook stappen voor probleemoplossing voor Linux-systemen.

## <a name="extension-configuration"></a>Configuratie voor uitbreiding
De configuratie van de aangepaste Scriptextensie geeft bijvoorbeeld de locatie van script en de opdracht om te worden uitgevoerd. U kunt deze configuratie opslaan in configuratiebestanden, geef hem op in de opdrachtregel of opgeven in een Azure Resource Manager-sjabloon. 

U kunt de gevoelige gegevens opslaan in een beveiligde configuratie, waardoor wordt gecodeerd en worden alleen ontsleuteld in de virtuele machine. De configuratie van de beveiligde is handig wanneer de opdracht uitvoering geheimen zoals een wachtwoord bevat.

### <a name="public-configuration"></a>De openbare configuratie
Het schema voor de openbare configuratie is als volgt.

>[!NOTE]
>De namen van deze eigenschappen zijn hoofdlettergevoelig. Implementatie om problemen te voorkomen, door de namen te gebruiken, zoals hier wordt weergegeven.

* **commandToExecute** (vereist, string): de vermelding point-script uit te voeren.
* **fileUris** (optioneel, string array): de URL's voor bestanden worden gedownload.
* **tijdstempel** (optioneel, geheel getal): de tijdstempel van het script. De waarde van dit veld alleen wijzigen als u wilt activeren, een opnieuw uitvoeren van het script.

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Beveiligde configuratie
Het schema voor de configuratie van de beveiligde is als volgt.

>[!NOTE]
>De namen van deze eigenschappen zijn hoofdlettergevoelig. Implementatie om problemen te voorkomen, door de namen te gebruiken, zoals hier wordt weergegeven.

* **commandToExecute** (optioneel, string): de vermelding point-script uit te voeren. Dit veld gebruiken als uw opdracht geheimen zoals wachtwoorden bevat.
* **storageAccountName** (optioneel, string): de naam van het opslagaccount. Als u de storage-referenties opgeeft, moeten alle bestand URI's URL's voor Azure blobs.
* **storageAccountKey** (optioneel, string): de toegangssleutel van het opslagaccount.

```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure-CLI
Wanneer u Azure CLI om uit te voeren van de aangepaste Scriptextensie, maakt u een configuratiebestand of bestanden. Ten minste bevatten configuratiebestanden de URI van het bestand en de scriptopdracht kan worden uitgevoerd.

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

Desgewenst kunt u de instellingen in de opdracht als een tekenreeks JSON-indeling. Hiermee wordt de configuratie wordt opgegeven tijdens de uitvoering en zonder een afzonderlijk configuratiebestand.

```azurecli
az vm extension set '
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
```

### <a name="azure-cli-examples"></a>Azure CLI-voorbeelden

#### <a name="public-configuration-with-script-file"></a>De openbare configuratie met een scriptbestand

```json
{
  "fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],
  "commandToExecute": "./config-music.sh"
}
```

Azure CLI-opdracht:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

#### <a name="public-configuration-with-no-script-file"></a>De openbare configuratie zonder script een bestand

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI-opdracht:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

#### <a name="public-and-protected-configuration-files"></a>Configuratie van openbare en beveiligde bestanden

Een bestand met de openbare configuratie kunt u de URI van het scriptbestand opgeven. U een beveiligde configuratiebestand gebruiken om op te geven van de opdracht om te worden uitgevoerd.

De openbare configuratie voor bestand:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"]
}
```

Beveiligde configuratiebestand:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Azure CLI-opdracht:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json --protected-settings ./protected-config.json
```

## <a name="resource-manager-template"></a>Resource Manager-sjabloon
U kunt de Azure-extensie voor aangepaste scripts tijdens de implementatie virtuele machine uitvoeren met behulp van een Resource Manager-sjabloon. Om dit te doen toevoegen juist opgemaakte JSON naar de implementatiesjabloon.

### <a name="resource-manager-examples"></a>Voorbeelden van Resource Manager

#### <a name="public-configuration"></a>De openbare configuratie

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

#### <a name="execution-command-in-protected-configuration"></a>De opdracht kan worden uitgevoerd in beveiligde configuratie

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
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
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

Zie voor een compleet voorbeeld de [.NET muziek Store demo](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="troubleshooting"></a>Problemen oplossen
Wanneer de aangepaste Scriptextensie wordt uitgevoerd, kan het script wordt gemaakt of gedownload naar een map die vergelijkbaar is met het volgende voorbeeld. Uitvoer van de opdracht wordt ook opgeslagen in deze map in `stdout` en `stderr` bestanden.

```bash
/var/lib/waagent/custom-script/download/0/
```

De extensie van het Azure-Script wordt een logboek, kunt u hier vinden:

```bash
/var/log/azure/custom-script/handler.log
```

U kunt ook de uitvoeringsstatus van de aangepaste Scriptextensie ophalen met behulp van Azure CLI:

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

De uitvoer ziet er de volgende tekst:

```azurecli
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over andere VM-extensies voor script [extensieoverzicht Azure script voor Linux](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

