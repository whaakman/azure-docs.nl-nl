---
title: Chef-extensie voor virtuele Azure-machines | Microsoft Docs
description: De Chef-Client implementeren op een virtuele machine met behulp van de Chef-VM-extensie.
services: virtual-machines-linux
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/21/2018
ms.author: roiyz
ms.openlocfilehash: 159ce1b565068e2cfdb3cb1cb2e5b5f72ff6848f
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451354"
---
# <a name="chef-vm-extension-for-linux-and-windows"></a>Chef VM-extensie voor Linux en Windows

Chef Software levert een DevOps-automatiseringsplatform voor Linux en Windows. Hiermee kunnen zowel fysieke als virtuele serverconfiguraties worden beheerd. De VM-extensie van Chef is een uitbreiding waarmee Chef op virtuele machines.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De Chef-VM-extensie wordt ondersteund op alle de [extensie ondersteund besturingssysteem](https://support.microsoft.com/help/4078134/azure-extension-supported-operating-systems) in Azure.

### <a name="internet-connectivity"></a>Internetconnectiviteit

De Chef-VM-extensie is vereist dat de virtuele doelmachine is verbonden met internet om op te halen van de nettolading van de Chef-Client van het content delivery network (CDN).  

## <a name="extension-schema"></a>Extensieschema

De volgende JSON bevat het schema voor de Chef-VM-extensie. De extensie vereist ten minste de URL van de Chef-Server, de naam van de validatie-Client en de validatiesleutel voor de Chef-Server. Deze waarden kunnen u vinden in de `knife.rb` bestand in de starter-kit.zip dat wordt gedownload tijdens de installatie van [Chef Automate](https://azuremarketplace.microsoft.com/marketplace/apps/chef-software.chef-automate) of een zelfstandige [Chef Server](https://downloads.chef.io/chef-server). Omdat de validatiesleutel moet worden behandeld als gevoelige gegevens, moet deze worden geconfigureerd onder de **protectedSettings** -element, wat betekent dat deze alleen op de virtuele doelmachine worden ontsleuteld.

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'),'/', parameters('chef_vm_extension_type'))]",
  "apiVersion": "2017-12-01",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Chef.Bootstrap.WindowsAzure",
    "type": "[parameters('chef_vm_extension_type')]",
    "typeHandlerVersion": "1210.12",
    "settings": {
      "bootstrap_options": {
        "chef_server_url": "[parameters('chef_server_url')]",
        "validation_client_name": "[parameters('chef_validation_client_name')]"
      },
      "runlist": "[parameters('chef_runlist')]"
    },
    "protectedSettings": {
      "validation_key": "[parameters('chef_validation_key')]"
    }
  }
}  
```

### <a name="core-property-values"></a>Core-eigenschapswaarden

| Naam | Waarde / voorbeeld | Gegevenstype
| ---- | ---- | ---- | ----
| apiVersion | `2017-12-01` | tekenreeks (datum) |
| Uitgever | `Chef.Bootstrap.WindowsAzure` | tekenreeks |
| type | `LinuxChefClient` (Linux), `ChefClient` (Windows) | tekenreeks |
| typeHandlerVersion | `1210.12` | tekenreeks (double) |

### <a name="settings"></a>Instellingen

| Naam | Waarde / voorbeeld | Gegevenstype | Vereist?
| ---- | ---- | ---- | ----
| instellingen/bootstrap_options/chef_server_url | `https://api.chef.io/organizations/myorg` | tekenreeks (url) | J |
| instellingen/bootstrap_options/validation_client_name | `myorg-validator` | tekenreeks | J |
| instellingen/runlist | `recipe[mycookbook::default]` | tekenreeks | J |

### <a name="protected-settings"></a>Beveiligde instellingen

| Naam | Voorbeeld | Gegevenstype | Vereist?
| ---- | ---- | ---- | ---- |
| protectedSettings/validation_key | `-----BEGIN RSA PRIVATE KEY-----\nKEYDATA\n-----END RSA PRIVATE KEY-----` | tekenreeks | J |

<!--
### Linux-specific settings

| Name | Value / Example | Data Type |
| ---- | ---- | ---- |

### Windows-specific settings

| Name | Value / Example | Data Type |
| ---- | ---- | ---- |
-->

## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. Sjablonen kunnen worden gebruikt om een of meer virtuele machines implementeren, de Chef-Client installeren, verbinding maken met de Chef-Server en het uitvoeren de eerste configuratie op de server zoals gedefinieerd door de [uitvoeren-lijst](https://docs.chef.io/run_lists.html)

Een voorbeeld van Resource Manager-sjabloon met de Chef-VM-extensie kunt u vinden op de [Azure Quick Start-galerie](https://github.com/Azure/azure-quickstart-templates/tree/master/chef-json-parameters-linux-vm).

De JSON-configuratie voor een VM-extensie worden genest in de bron van de virtuele machine of geplaatst op de hoofdmap of het hoogste niveau van een Resource Manager JSON-sjabloon. De plaatsing van de JSON-configuratie is van invloed op de waarde van de resourcenaam en het type. Zie voor meer informatie, [naam en type voor de onderliggende resources instellen](../../azure-resource-manager/resource-manager-template-child-resource.md).

## <a name="azure-cli-deployment"></a>Azure CLI-implementatie

De Azure CLI kan worden gebruikt om de VM-extensie van Chef implementeren op een bestaande virtuele machine. Vervang de **validation_key** met de inhoud van uw validatiesleutel (dit bestand op als een `.pem` extensie).  Vervang **validation_client_name**, **chef_server_url** en **run_list** met deze waarden uit de `knife.rb` bestand in uw Starter Kit.

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myExistingVM \
  --name LinuxChefClient \
  --publisher Chef.Bootstrap.WindowsAzure \
  --version 1210.12 --protected-settings '{"validation_key": "<validation_key>"}' \
  --settings '{ "bootstrap_options": { "chef_server_url": "<chef_server_url>", "validation_client_name": "<validation_client_name>" }, "runlist": "<run_list>" }'
```

## <a name="troubleshooting-and-support"></a>Problemen oplossen en ondersteuning

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure-portal en met behulp van de Azure CLI. Als wilt zien de implementatiestatus van extensies voor een bepaalde virtuele machine, voert u de volgende opdracht uit met de Azure CLI.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myExistingVM -o table
```

Extensie uitvoering uitvoer wordt vastgelegd in het volgende bestand:

### <a name="linux"></a>Linux

```bash
/var/lib/waagent/Chef.Bootstrap.WindowsAzure.LinuxChefClient
```

### <a name="windows"></a>Windows

```powershell
C:\Packages\Plugins\Chef.Bootstrap.WindowsAzure.ChefClient\
```

### <a name="error-codes-and-their-meanings"></a>Foutcodes en hun betekenis

| Foutcode | Betekenis | Mogelijke actie |
| :---: | --- | --- |
| 51 | Deze extensie wordt niet ondersteund voor besturingssysteem van de virtuele machine | |

Als u meer informatie over probleemoplossing vindt u de [Chef VM-extensie Leesmij](https://github.com/chef-partners/azure-chef-extension).

## <a name="next-steps"></a>Volgende stappen

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).
