---
title: Azure Linux Agent extensie keren stackify | Microsoft Docs
description: Implementeer de Stackify keren Linux-agent op een virtuele Linux-machine.
services: virtual-machines-linux
documentationcenter: ''
author: darinhoward
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/12/2018
ms.author: danis
ms.openlocfilehash: 376c5a087f74fbe087db9fa2df38b2ba4e6cf1ff
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33943080"
---
# <a name="stackify-retrace-linux-agent-extension"></a>Linux-Agent extensie keren stackify

## <a name="overview"></a>Overzicht
Stackify biedt producten die gegevens over uw toepassing bij het oplossen van problemen snel bijhouden. Voor ontwikkelaars teams is keren een volledig geïntegreerde, meerdere omgeving app prestaties absoluut macht. Verschillende hulpprogramma's voor die elke ontwikkelteam moet worden gecombineerd.

Keren is het alleen-hulpprogramma dat alle van de volgende mogelijkheden in alle omgevingen in één enkel platform biedt.

* Beheer van de prestaties van toepassingen (APM)
* Toepassings- en server-logboekregistratie
* Fout bijhouden en bewaking
* Server-, toepassings- en aangepaste metrische gegevens

**Punt Stackify uitbreiding van de Linux-Agent**

Deze uitbreiding biedt een installatiepad voor de Linux-Agent voor keren. 

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem 
De agent keren kan worden uitgevoerd op basis van deze Linux-distributies

| Distributie | Versie |
|---|---|
| Ubuntu | 16.04 TNS, 14.04 TNS, 16,10 en 17.04 |
| Debian | 7,9 + en 8.2 +, 9 |
| RedHat | 6.7 +, 7.1 + |
| CentOS | 6.3 +, 7.0 + |

### <a name="internet-connectivity"></a>Internetconnectiviteit
De extensie Stackify Agent voor Linux vereist dat de virtuele doelmachine is verbonden met internet. 

Mogelijk moet u uw netwerkconfiguratie als u wilt toestaan verbindingen met Stackify, Zie aanpassen https://support.stackify.com/hc/en-us/articles/207891903-Adding-Exceptions-to-a-Firewall. 


## <a name="extension-schema"></a>Extensieschema
---

De volgende JSON ziet u het schema voor de uitbreiding Stackify keren Agent. De uitbreiding vereist de `environment` en `activationKey`.

```json
    {
      "type": "extensions",
      "name": "StackifyExtension",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Compute/virtualMachines',variables('vmName'))]"
      ],
      "properties": {
        "publisher": "Stackify.LinuxAgent.Extension",
        "type": "StackifyLinuxAgentExtension",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "environment": "myEnvironment"
        },
        "protectedSettings": {
          "activationKey": "myActivationKey"
        }
      }
    }      
```

## <a name="template-deployment"></a>Sjabloonimplementatie 

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie wordt beschreven, kan de extensie keren Linux-Agent Stackify uitvoeren tijdens de sjabloonimplementatie van een Azure Resource Manager-in een Azure Resource Manager-sjabloon worden gebruikt.  

De JSON voor de extensie van een virtuele machine worden genest in de bron van de virtuele machine, of aan de basis- of bovenste niveau van een Resource Manager JSON-sjabloon geplaatst. De plaatsing van de JSON van invloed op de waarde van de resourcenaam en het type. Zie de naam en type op voor de onderliggende resources voor meer informatie.

Het volgende voorbeeld wordt ervan uitgegaan dat de extensie keren Linux Stackify is genest binnen de bron van de virtuele machine. Wanneer het nesten van de extensie-resource, de JSON wordt geplaatst in de 'resources': []-object van de virtuele machine.

De uitbreiding vereist de `environment` en `activationKey`.

```json
    {
      "type": "extensions",
      "name": "StackifyExtension",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Compute/virtualMachines',variables('vmName'))]"
      ],
      "properties": {
        "publisher": "Stackify.LinuxAgent.Extension",
        "type": "StackifyLinuxAgentExtension",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "environment": "myEnvironment"
        },
        "protectedSettings": {
          "activationKey": "myActivationKey"
        }
      }
    }      
```

Bij het plaatsen van de JSON-extensie in de hoofdmap van de sjabloon, de naam van de bron bevat een verwijzing naar de bovenliggende virtuele machine en het type reflecteert de geneste configuratie.

```json
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "<parentVmResource>/StackifyExtension",
        "apiVersion": "[variables('apiVersion')]",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
        ],
        "properties": {
            "publisher": "Stackify.LinuxAgent.Extension",
            "type": "StackifyLinuxAgentExtension",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "environment": "myEnvironment"
            },
            "protectedSettings": {
              "activationKey": "myActivationKey"
            }
        }
    }
```


## <a name="powershell-deployment"></a>PowerShell-implementatie

De `Set-AzureRmVMExtension` opdracht kan worden gebruikt voor het implementeren van de extensie van de virtuele machine keren Linux-Agent Stackify op een bestaande virtuele machine. Voordat u de opdracht uitvoert, moeten de openbare en persoonlijke configuraties worden opgeslagen in een PowerShell-hash-tabel.

De uitbreiding vereist de `environment` en `activationKey`.

```
$PublicSettings = @{"environment" = "myEnvironment"}
$ProtectedSettings = @{"activationKey" = "myActivationKey"}

Set-AzureRmVMExtension -ExtensionName "Stackify.LinuxAgent.Extension" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Stackify.LinuxAgent.Extension" `
    -ExtensionType "StackifyLinuxAgentExtension" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS `
```

## <a name="azure-cli-deployment"></a>Azure CLI-implementatie 

De Azure CLI-hulpprogramma kan worden gebruikt voor het implementeren van de extensie van de virtuele machine keren Linux-Agent Stackify op een bestaande virtuele machine.  

De uitbreiding vereist de `environment` en `activationKey`.

```azurecli
az vm extension set --publisher 'Stackify.LinuxAgent.Extension' --version 1.0 --name 'StackifyLinuxAgentExtension' --protected-settings '{"activationKey":"myActivationKey"}' --settings '{"environment":"myEnvironment"}'  --resource-group 'myResourceGroup' --vm-name 'myVmName'
```

## <a name="troubleshoot-and-support"></a>Oplossen van problemen en ondersteunen

### <a name="error-codes"></a>Foutcodes

| Foutcode | Betekenis | Mogelijke actie |
| :---: | --- | --- |
| 10 | Fout bij installeren | wget is vereist |
| 20 | Fout bij installeren | Python is vereist |
| 30 | Fout bij installeren | sudo is vereist |
| 40 | Fout bij installeren | activationKey is vereist |
| 51 | Fout bij installeren | OS-distro niet ondersteund |
| 60 | Fout bij installeren | omgeving is vereist |
| 70 | Fout bij installeren | Onbekend |
| 80 | Fout bij inschakelen | Service-instellingen is mislukt |
| 90 | Fout bij inschakelen | Starten van de service is mislukt |
| 100 | Fout bij uitschakelen | Service stoppen is mislukt |
| 110 | Fout bij uitschakelen | Service verwijderen is mislukt |
| 120 | Fout bij verwijderen | Service stoppen is mislukt |

Als u meer hulp nodig kunt u wel een contact opnemen met ondersteuning Stackify https://support.stackify.com.