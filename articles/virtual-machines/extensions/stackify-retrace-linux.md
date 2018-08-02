---
title: Azure Linux Agent-extensie keren stackify | Microsoft Docs
description: De Stackify keren Linux-agent op een Linux-machine implementeren.
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
ms.author: roiyz
ms.openlocfilehash: b286ebc2e50166e8491b45346a81b161227f8d21
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39415118"
---
# <a name="stackify-retrace-linux-agent-extension"></a>Stackify keren Linux-Agent-extensie

## <a name="overview"></a>Overzicht
Stackify biedt producten die meer informatie over uw toepassing om te zoeken en oplossen van problemen snel bijhouden. Voor teams van ontwikkelaars, keren dat is een volledig geïntegreerde, meerdere omgevingen app prestaties Supergebruikers. Het combineert verschillende programma's die elke ontwikkelteam nodig heeft.

Keren is de alleen-hulpprogramma dat voorziet in alle van de volgende mogelijkheden in alle omgevingen in één platform.

* Beheer van prestaties van toepassingen (APM)
* Toepassings- en server-logboekregistratie
* Fout volgen en controleren
* Server-, toepassings- en aangepaste metrische gegevens

**Stackify over Linux-Agent-extensie**

Deze uitbreiding biedt een installatiepad voor de Linux-Agent voor keren. 

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem 
De agent keren kan worden uitgevoerd voor deze Linux-distributies

| Distributie | Versie |
|---|---|
| Ubuntu | 16.04 LTS, 14.04 LTS, 16,10 en 17.04 |
| Debian | 7,9 + en 8.2 +, 9 |
| Red Hat | 6.7 +, 7.1 + |
| CentOS | 6.3 +, 7.0 + |

### <a name="internet-connectivity"></a>Internetconnectiviteit
De Stackify Agent-extensie voor Linux is vereist dat de virtuele doelmachine is verbonden met internet. 

Mogelijk moet u uw netwerkconfiguratie Stackify verbindingen worden toegestaan, raadpleeg dan aanpassen https://support.stackify.com/hc/en-us/articles/207891903-Adding-Exceptions-to-a-Firewall. 


## <a name="extension-schema"></a>Extensieschema
---

De volgende JSON ziet u het schema voor de Stackify keren Agent-extensie. De extensie is vereist de `environment` en `activationKey`.

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

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie worden beschreven, kan de Stackify keren Linux Agent-extensie uitgevoerd tijdens de sjabloonimplementatie van een Azure Resource Manager-in een Azure Resource Manager-sjabloon worden gebruikt.  

De JSON voor de extensie van een virtuele machine kan worden genest in de bron van de virtuele machine of geplaatst op de hoofdmap of het hoogste niveau van een Resource Manager JSON-sjabloon. De plaatsing van de JSON is van invloed op de waarde van de resourcenaam en het type. Zie voor meer informatie, de naam en het type voor de onderliggende resources.

Het volgende voorbeeld wordt ervan uitgegaan dat de Stackify keren Linux-extensie is genest in de bron van de virtuele machine. Wanneer het nesten van de extensie-resource, de JSON wordt geplaatst in de 'resources': []-object van de virtuele machine.

De extensie is vereist de `environment` en `activationKey`.

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

Bij het plaatsen van de JSON-extensie in de hoofdmap van de sjabloon, naam van de resource bevat een verwijzing naar de bovenliggende virtuele machine en het type weerspiegelt de geneste configuratie.

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

De `Set-AzureRmVMExtension` opdracht kan worden gebruikt om de extensie van de virtuele machine Stackify keren Linux Agent implementeren op een bestaande virtuele machine. Voordat u de opdracht uitvoert, moeten de openbare en privé-configuraties worden opgeslagen in een PowerShell-hash-tabel.

De extensie is vereist de `environment` en `activationKey`.

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

De Azure CLI-hulpprogramma kan worden gebruikt voor de extensie van de virtuele machine Stackify keren Linux Agent implementeren op een bestaande virtuele machine.  

De extensie is vereist de `environment` en `activationKey`.

```azurecli
az vm extension set --publisher 'Stackify.LinuxAgent.Extension' --version 1.0 --name 'StackifyLinuxAgentExtension' --protected-settings '{"activationKey":"myActivationKey"}' --settings '{"environment":"myEnvironment"}'  --resource-group 'myResourceGroup' --vm-name 'myVmName'
```

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="error-codes"></a>Foutcodes

| Foutcode | Betekenis | Mogelijke actie |
| :---: | --- | --- |
| 10 | Fout bij installeren | wget is vereist |
| 20 | Fout bij installeren | Python is vereist |
| 30 | Fout bij installeren | sudo is vereist |
| 40 | Fout bij installeren | Kluisbestand is vereist |
| 51 | Fout bij installeren | Distributie van besturingssysteem niet ondersteund |
| 60 | Fout bij installeren | omgeving is vereist |
| 70 | Fout bij installeren | Onbekend |
| 80 | Fout bij inschakelen | Service-instellingen is mislukt |
| 90 | Fout bij inschakelen | Starten van de service is mislukt |
| 100 | Fout bij uitschakelen | Service stoppen is mislukt |
| 110 | Fout bij uitschakelen | Service verwijderen is mislukt |
| 120 | Fout bij verwijderen | Service stoppen is mislukt |

Als u meer hulp nodig kunt u contact met ondersteuning van Stackify op https://support.stackify.com.