---
title: Sleutel kluis VM-extensie voor Windows | Microsoft Docs
description: Een agent automatische vernieuwing van Sleutelkluis geheimen uitvoeren op virtuele machines met de extensie van een virtuele machine implementeert.
services: virtual-machines-windows
documentationcenter: ''
author: dragav
manager: jeconnoc
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/14/2018
ms.author: dragosav
ms.openlocfilehash: 19e177f086ea9fa817a36e67ace77aed39ee89b5
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="key-vault-virtual-machine-extension-for-windows"></a>Sleutelkluis de extensie van de virtuele machine voor Windows

## <a name="overview"></a>Overzicht

De Sleutelkluis VM-extensie biedt automatische vernieuwing van geheimen die zijn opgeslagen in een Azure sleutelkluis. In het bijzonder de extensie-monitors die een lijst met certificaten die zijn opgeslagen in sleutelkluizen, en, bij het detecteren van een wijziging waargenomen opgehaald en de bijbehorende certificaten installeert. De Sleutelkluis VM-extensie is gepubliceerd en ondersteund door Microsoft, die momenteel op VM's van Windows. In dit document worden de ondersteunde platforms, configuraties en implementatie-opties voor de Sleutelkluis VM-extensie voor Windows. 

## <a name="prerequisites"></a>Vereisten

De Sleutelkluis VM-extensie is afhankelijk van de beheerde Service identiteit van VM-extensie, om te authenticeren bij de Sleutelkluis-service. Raadpleeg de documentatie voor de VM-extensie van de MSI voor meer informatie.

### <a name="operating-system"></a>Besturingssysteem

De Sleutelkluis VM-extensie ondersteunt momenteel alle Windows-versies.

| Distributie | Versie |
|---|---|
| Windows | Kern |

### <a name="internet-connectivity"></a>Internetconnectiviteit

De Sleutelkluis VM-extensie voor Windows is vereist dat de virtuele doelmachine is verbonden met internet. 

## <a name="extension-schema"></a>Extensieschema

De volgende JSON ziet u het schema voor de Sleutelkluis VM-extensie. De extensie hoeven geen beveiligde instellingen: de instellingen ervan worden beschouwd als informatie zonder gevolgen voor beveiliging. De extensie is vereist voor een lijst met bewaakte geheimen pollingfrequentie en het certificaatarchief van de bestemming. Specifiek:  

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KVVMExtensionForWindows",
      "apiVersion": "2016-10-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
            "publisher": "Microsoft.Azure.KeyVault.Edp",
            "type": "KeyVaultForWindows",
            "typeHandlerVersion": "0.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "secretsManagementSettings": {
                    "pollingIntervalInS": <polling interval in seconds>,
                    "certificateStoreName": <certificate store name, e.g.: "MY">,
                    "certificateStoreLocation": <certificate store location, e.g.: "LOCAL_MACHINE">,
                    "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net:443/secrets/mycertificate"
                }         
            }
      }
    }
```

### <a name="property-values"></a>Eigenschapswaarden

| Naam | Waarde / voorbeeld | Gegevenstype |
| ---- | ---- | ---- |
| apiVersion | 2016-10-01 | datum |
| Uitgever | Microsoft.Azure.KeyVault.Edp | tekenreeks |
| type | KeyVaultForWindows | tekenreeks |
| typeHandlerVersion | 0.0 | int |
| pollingIntervalInS | 3600 | int |
| NaamCertificaatarchief | MIJN | tekenreeks |
| certificateStoreLocation  | LOCAL_MACHINE | tekenreeks |
| observedCertificates  | ["https://myvault.vault.azure.net:443/secrets/mycertificate"] | Tekenreeksmatrix


## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. Sjablonen zijn ideaal bij het implementeren van een of meer virtuele machines waarvoor de post-implementatie vernieuwen van certificaten. De extensie kan worden toegepast op afzonderlijke virtuele machines of virtuele machine sets schalen. De schema- en gemeenschappelijk zijn voor beide sjabloontypen. 

De JSON-configuratie voor de extensie van een virtuele machine moet worden genest in de resource-fragment van de virtuele machine van de sjabloon specifiek `"resources": []` object van de virtuele machine.

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KVVMExtensionForWindows",
      "apiVersion": "2016-10-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
            "publisher": "Microsoft.Azure.KeyVault.Edp",
            "type": "KeyVaultForWindows",
            "typeHandlerVersion": "0.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "secretsManagementSettings": {
                    "pollingIntervalInS": <polling interval in seconds>,
                    "certificateStoreName": <certificate store name, e.g.: "MY">,
                    "certificateStoreLocation": <certificate store location, e.g.: "LOCAL_MACHINE">,
                    "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net:443/secrets/mycertificate"
                }         
            }
      }
    }
```

## <a name="azure-powershell-deployment"></a>Azure PowerShell-implementatie

Azure PowerShell kan worden gebruikt voor het implementeren van de Sleutelkluis VM-extensie aan een bestaande virtuele machine of virtuele-machineschaalset. 

* De extensie op een virtuele machine implementeren:
    
    ```powershell
        # Build settings
        $settings = '{"secretsManagementSettings": 
            { "pollingIntervalInS": "' + <pollingInterval> + 
            '", "certificateStoreName": "' + <certStoreName> + 
            '", "certificateStoreLocation": "' + <certStoreLoc> + 
            '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName = <VMName> + '/KeyVaultForWindows' 
        $extPublisher = "Microsoft.Azure.KeyVault.Edp"
        $extType = "KeyVaultForWindows"
    
        # Start the deployment
        Set-AzureRmVmExtension -VMName <VMName> -Name $extName -Publisher $extPublisher -Type $extType -SettingString $settings
    
    ```

* De extensie op een VM-schaalset bevatten, bijvoorbeeld als onderdeel van een Service Fabric-cluster implementeren:

    ```powershell
    
        # Build settings
        $settings = '{"secretsManagementSettings": 
            { "pollingIntervalInS": "' + <pollingInterval> + 
            '", "certificateStoreName": "' + <certStoreName> + 
            '", "certificateStoreLocation": "' + <certStoreLoc> + 
            '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName = <VMSSName> + '_KeyVaultForWindows' 
        $extPublisher = "Microsoft.Azure.KeyVault.Edp"
        $extType = "KeyVaultForWindows"
    
        # Start the deployment
        Add-AzureRmVmssExtension -VirtualMachineScaleSet <VMSS> -Name $extName -Publisher $extPublisher -Type $extType -Setting $settings
    
    ```

Let op de hoogte van de volgende beperkingen/vereisten:
- De implementatie moet worden uitgevoerd in de regio Zuid-centraal VS
- Sleutelkluis beperkingen:
    - Deze moet bestaat op het moment van de implementatie 
    - Moet zich bevinden in dezelfde regio en resourcegroep als de implementatie
    - Moet zijn ingeschakeld voor de implementatie en sjabloonimplementatie

## <a name="azure-cli-deployment"></a>Azure CLI-implementatie

Voorbeelden voor het implementeren van de Sleutelkluis VM-extensie in de Azure CLI zijn binnenkort beschikbaar. 

## <a name="troubleshoot-and-support"></a>Oplossen van problemen en ondersteunen

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure portal en met behulp van Azure PowerShell. Overzicht van de implementatiestatus van uitbreidingen voor een bepaalde virtuele machine, voer de volgende opdracht met de Azure PowerShell.

```powershell
Get-AzureRMVMExtension -VMName <vmName> -ResourceGroupname <resource group name>
```

De uitvoer van de extensie-uitvoering wordt vastgelegd in het volgende bestand:

```
%windrive%\WindowsAzure\Logs\Plugins\Microsoft.Azure.KeyVault.Edp.KeyVaultForWindows\<version>\akvvm_service_<date>.log
```


### <a name="support"></a>Ondersteuning

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op de [MSDN Azure en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer de Get-ondersteuning. Voor meer informatie over het gebruik van Azure ondersteuning voor de [ondersteuning van Microsoft Azure Veelgestelde vragen over](https://azure.microsoft.com/support/faq/).
