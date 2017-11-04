---
title: Aangepaste scripts uitvoeren op virtuele Linux-machines in Azure | Microsoft Docs
description: Configuratietaken voor Linux-VM automatiseren met behulp van de aangepaste Scriptextensie
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
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
ms.author: nepeters
ms.openlocfilehash: 1dde64aac72c11ccfccf4fdb676279692befaadd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="using-the-azure-custom-script-extension-with-linux-virtual-machines"></a>De extensie Azure aangepast Script gebruiken met Linux virtuele Machines
De aangepaste Scriptextensie downloads en scripts op virtuele machines in Azure wordt uitgevoerd. Deze uitbreiding is nuttig voor post-implementatieconfiguratie, software-installatie of een andere configuratie / beheertaak. Scripts kunnen worden gedownload van Azure storage of andere toegankelijke internetlocatie, of naar de runtime-extensie. De aangepaste scriptextensie kan worden geïntegreerd met Azure Resource Manager-sjablonen en kan ook worden uitgevoerd met de Azure CLI, PowerShell, Azure-portal of de REST-API van Azure virtuele Machine.

Dit document beschrijft het gebruik van de aangepaste Scriptextensie van de Azure CLI en een Azure Resource Manager-sjabloon en ook gegevens stappen voor probleemoplossing op Linux-systemen.

## <a name="extension-configuration"></a>Configuratie voor uitbreiding
De configuratie van de aangepaste Scriptextensie geeft bijvoorbeeld de locatie van script en de opdracht om te worden uitgevoerd. Deze configuratie kan worden opgeslagen in de configuratiebestanden, op de opdrachtregel of in een Azure Resource Manager-sjabloon opgegeven. Gevoelige gegevens kunnen worden opgeslagen in een beveiligde configuratie die is versleuteld en worden alleen ontsleuteld in de virtuele machine. De configuratie van de beveiligde is handig wanneer de opdracht uitvoering geheimen zoals een wachtwoord bevat.

### <a name="public-configuration"></a>De openbare configuratie
Schema:

**Opmerking** -de namen van deze eigenschappen zijn hoofdlettergevoelig. Gebruik de namen zoals hieronder wordt weergegeven om te voorkomen dat problemen bij de implementatie.

* **commandToExecute**: (vereist, string) de vermelding punt script uitvoeren
* **fileUris**: (optioneel, string array) de URL's voor bestanden worden gedownload.
* **tijdstempel** (optioneel, geheel getal) dit veld gebruiken alleen voor het activeren van een opnieuw uitvoeren van het script door de waarde van dit veld te wijzigen.

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Beveiligde configuratie
Schema:

**Opmerking** -de namen van deze eigenschappen zijn hoofdlettergevoelig. Gebruik de namen zoals hieronder wordt weergegeven om te voorkomen dat problemen bij de implementatie.

* **commandToExecute**: (optioneel, string) de vermelding punt script uit te voeren. Dit veld in plaats daarvan gebruiken als uw opdracht geheimen zoals wachtwoorden bevat.
* **storageAccountName**: (optioneel, string) de naam van de storage-account. Als u de storage-referenties opgeeft, moet alle fileUris URL's voor Azure Blobs.
* **storageAccountKey**: (optioneel, string) de toegangssleutel van storage-account.

```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure CLI
Als u de Azure CLI voor het uitvoeren van de aangepaste Scriptextensie, maken een configuratiebestand of bestanden met ten minste de uri van het bestand en de scriptopdracht kan worden uitgevoerd.

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

Desgewenst kunnen de instellingen in de opdracht worden opgegeven als een JSON-indeling in tekenreeks. Hiermee wordt de configuratie wordt opgegeven tijdens de uitvoering en zonder een afzonderlijk configuratiebestand.

```azurecli
az vm extension set '
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],"commandToExecute": "./test.sh"}'
```

### <a name="azure-cli-examples"></a>Voorbeelden van Azure CLI

**Voorbeeld 1** -openbare configuratie met een scriptbestand.

```json
{
  "fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],
  "commandToExecute": "./test.sh"
}
```

Azure CLI-opdracht:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

**Voorbeeld 2** -openbare configuratie zonder script een bestand.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI-opdracht:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

**Voorbeeld 3** - bestand met een openbare configuratie wordt gebruikt om het scriptbestand URI geven en een beveiligde configuratiebestand wordt gebruikt om op te geven van de opdracht om te worden uitgevoerd.

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
De aangepaste Scriptextensie van Azure kan worden uitgevoerd tijdens de virtuele Machine implementatie met een Resource Manager-sjabloon. Om dit te doen toevoegen juist opgemaakte JSON naar de implementatiesjabloon.

### <a name="resource-manager-examples"></a>Voorbeelden van Resource Manager
**Voorbeeld 1** -openbare configuratie.

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

**Voorbeeld 2** -opdracht kan worden uitgevoerd in beveiligde configuratie.

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

Zie de .net Core muziek Store Demo voor een compleet voorbeeld - [muziek Store Demo](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).

## <a name="troubleshooting"></a>Problemen oplossen
Wanneer de aangepaste Scriptextensie wordt uitgevoerd, wordt het script wordt gemaakt, of in een map die vergelijkbaar is met het volgende voorbeeld wordt gedownload. Uitvoer van de opdracht wordt ook opgeslagen in deze map in `stdout` en `stderr` bestand.

```bash
/var/lib/waagent/custom-script/download/0/
```

De extensie van het Azure-Script produceert een logboek u hier vindt.

```bash
/var/log/azure/custom-script/handler.log
```

De uitvoeringsstatus van de aangepaste Scriptextensie kan ook worden opgehaald met de Azure CLI.

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
Zie voor meer informatie over andere VM-extensies voor Script [overzicht van de uitbreiding van de Azure-Script voor Linux](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

